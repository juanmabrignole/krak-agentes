<script>
  import { onMount } from 'svelte';

  // =========================
  // CONFIG
  // =========================
  const YOUTUBE_ID = 'GD6frexLvxs';
  const VIDEO_TITLE = 'Masterclass de Krak Real Estate con Marcelo Napolitano';

  // Webhooks (separados para evitar duplicados)
  // Gate (email) → idealmente apunta a tu n8n real (fly). Si todavía no lo activaste, igual hay fallback.
  const WEBHOOK_GATE_URL = 'https://krak-n8n.fly.dev/webhook/video-gate';

  // Form original (backup) — si lo seguís usando
  const WEBHOOK_NEWSLETTER_URL = 'https://n8n-krak.com/webhook/workshop-newsletter';

  // Form chico (postulación + CV) — cuando lo tengas listo en n8n
  const WEBHOOK_AGENT_URL = 'https://krak-n8n.fly.dev/webhook/postulacion-agente';

  // =========================
  // VIDEO GATE
  // =========================
  const LS_KEY = 'krak_masterclass_video_access_v1';

  let showGate = false;
  let gateEmail = '';
  let gateError = '';
  let videoUnlocked = false; // habilita reproducción (email OK)
  let videoLoaded = false; // performance: iframe solo al click

  const isValidEmail = (v) => /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(String(v || '').trim());
  const poster = `https://i.ytimg.com/vi/${YOUTUBE_ID}/hqdefault.jpg`;

  onMount(() => {
    const cached = localStorage.getItem(LS_KEY);
    if (cached && isValidEmail(cached)) {
      videoUnlocked = true;
    }
  });

  function openVideo() {
    if (videoUnlocked) {
      videoLoaded = true;
      return;
    }
    showGate = true;
    gateEmail = '';
    gateError = '';
  }

  async function sendJson(url, payload) {
    const res = await fetch(url, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(payload),
    });
    if (!res.ok) throw new Error('Webhook error');
    return res.json().catch(() => null);
  }

  async function handleGateEmail() {
    if (!isValidEmail(gateEmail)) {
      gateError = 'Ingresá un email válido.';
      return;
    }

    try {
      await sendJson(WEBHOOK_GATE_URL, {
        source: 'agentes.krak.com.ar',
        funnel: 'video_gate',
        email: gateEmail.trim(),
        createdAt: new Date().toISOString(),
      });

      // OK
      localStorage.setItem(LS_KEY, gateEmail.trim());
      videoUnlocked = true;
      showGate = false;
      videoLoaded = true;
    } catch (e) {
      // 🔥 Modo "video always works" para hoy:
      // si el webhook falla, igual no bloqueamos el contenido.
      localStorage.setItem(LS_KEY, gateEmail.trim());
      videoUnlocked = true;
      showGate = false;
      videoLoaded = true;
    }
  }

  // =========================
  // FORM ORIGINAL (backup)
  // =========================
  const MAX_WORDS = 80;
  const MAX_CHARS = 600; // backup por seguridad

  let comentario = '';
  let comentarioWords = 0;

  function countWords(text) {
    return text.trim().split(/\s+/).filter(Boolean).length;
  }

  function clampWords(text, maxWords) {
    const words = text.trim().split(/\s+/).filter(Boolean);
    if (words.length <= maxWords) return text;
    return words.slice(0, maxWords).join(' ');
  }

  function handleComentarioInput(e) {
    let value = e.currentTarget.value;

    // backup por caracteres
    if (value.length > MAX_CHARS) value = value.slice(0, MAX_CHARS);

    // límite real por palabras
    value = clampWords(value, MAX_WORDS);

    comentario = value;
    comentarioWords = countWords(comentario);
  }

  let isSubmitting = false;
  let submitError = '';
  let submitSuccess = false;

  const FETCH_TIMEOUT_MS = 15000;

  async function handleForm(e) {
    e.preventDefault();
    if (isSubmitting) return;

    isSubmitting = true;
    submitError = '';
    submitSuccess = false;

    const formEl = e.currentTarget;
    const form = new FormData(formEl);

    form.append('source', 'agentes.krak.com.ar');
    form.append('ts', new Date().toISOString());
    form.set('comentario', comentario);

    const controller = new AbortController();
    const timeout = setTimeout(() => controller.abort(), FETCH_TIMEOUT_MS);

    try {
      const res = await fetch(WEBHOOK_NEWSLETTER_URL, {
        method: 'POST',
        body: form,
        signal: controller.signal,
      });

      clearTimeout(timeout);

      if (!res.ok) {
        const msg = await res.text().catch(() => '');
        console.error('Webhook error:', res.status, msg);
        submitError = 'Hubo un error. Por favor, intentá nuevamente.';
        alert(submitError);
        return;
      }

      const data = await res.clone().json().catch(() => null);
      if (data && data.ok === false) {
        console.error('Webhook response not ok:', data);
        submitError = 'Hubo un error. Por favor, intentá nuevamente.';
        alert(submitError);
        return;
      }

      if (typeof window !== 'undefined' && window.fbq) {
        window.fbq('track', 'Lead');
      }

      submitSuccess = true;
      alert('Gracias por inscribirte!');
      formEl.reset();
      comentario = '';
      comentarioWords = 0;
    } catch (error) {
      clearTimeout(timeout);
      console.error(error);

      submitError =
        error?.name === 'AbortError'
          ? 'La respuesta tardó demasiado. Probá nuevamente en unos segundos.'
          : 'Hubo un error. Por favor, intentá nuevamente.';

      alert(submitError);
    } finally {
      isSubmitting = false;
    }
  }

  // =========================
  // FORM CHICO (AGENTES + CV)
  // =========================
  let a_nombre = '';
  let a_apellido = '';
  let a_celular = '';
  let a_email = '';
  let a_cv = null; // File

  async function handleAgentForm(e) {
    e.preventDefault();

    if (!a_nombre.trim() || !a_apellido.trim()) return alert('Completá nombre y apellido.');
    if (!a_celular.trim()) return alert('Completá tu celular.');
    if (!isValidEmail(a_email)) return alert('Ingresá un email válido.');
    if (!a_cv) return alert('Adjuntá tu CV (PDF/DOC/DOCX).');

    const fd = new FormData();
    fd.append('source', 'agentes.krak.com.ar');
    fd.append('funnel', 'post_video_form');
    fd.append('nombre', a_nombre.trim());
    fd.append('apellido', a_apellido.trim());
    fd.append('celular', a_celular.trim());
    fd.append('email', a_email.trim());
    fd.append('createdAt', new Date().toISOString());
    fd.append('cv', a_cv);

    try {
      const res = await fetch(WEBHOOK_AGENT_URL, { method: 'POST', body: fd });
      if (res.ok) {
        alert('¡Gracias! Recibimos tu info 🙌');
        a_nombre = '';
        a_apellido = '';
        a_celular = '';
        a_email = '';
        a_cv = null;
      } else {
        alert('Hubo un error. Por favor, intentá nuevamente.');
      }
    } catch (error) {
      console.error(error);
      alert('Hubo un error. Por favor, intentá nuevamente.');
    }
  }
</script>

<svelte:head>
  <title>Masterclass de Krak Real Estate con Marcelo Napolitano</title>
  <meta
    name="description"
    content="Masterclass para agentes inmobiliarios: estrategias para generar confianza, manejar objeciones y acelerar cierres con seguridad y autoridad."
  />
</svelte:head>

<!-- HERO (dark + glow Krak) -->
<section class="relative overflow-hidden bg-[#050A14] py-20 sm:py-24 px-4 sm:px-6 lg:px-8">
  <!-- glows -->
  <div
    class="pointer-events-none absolute -top-44 left-1/2 h-[520px] w-[520px] -translate-x-1/2 rounded-full bg-[#08407C]/30 blur-[120px]"
  ></div>
  <div
    class="pointer-events-none absolute -bottom-60 right-[-120px] h-[520px] w-[520px] rounded-full bg-[#08407C]/25 blur-[140px]"
  ></div>

  <div class="relative mx-auto max-w-5xl text-center">
    <p class="inline-flex items-center gap-2 rounded-full border border-white/10 bg-white/5 px-4 py-2 text-sm text-white/80">
      Masterclass para agentes inmobiliarios
      <span class="h-1 w-1 rounded-full bg-white/40"></span>
      Mentor: Marcelo Napolitano
    </p>

    <h1 class="mt-7 text-4xl sm:text-5xl md:text-6xl font-bold text-white font-inter leading-tight">
      Convertite en el agente inmobiliario que cierra con seguridad y autoridad
    </h1>

    <p class="mt-6 text-lg sm:text-xl md:text-2xl text-white/75 font-inter max-w-3xl mx-auto">
      Hoy tenés clientes, propiedades y oportunidades
      <br class="hidden md:block" />
      pero cerrar operaciones sigue siendo más difícil de lo que debería.
    </p>

    <div class="mt-10 flex flex-col sm:flex-row items-center justify-center gap-4">
      <a
        href="#video"
        class="inline-flex items-center justify-center rounded-xl bg-[#08407C] hover:bg-[#0A4D95] text-white font-bold text-lg py-4 px-8 shadow-lg shadow-[#08407C]/30 transition duration-300 transform hover:scale-[1.02] border border-white/10 w-full sm:w-auto"
      >
        Ver el video (con acceso)
      </a>

      <a
        href="#postulacion"
        class="inline-flex items-center justify-center rounded-xl bg-white/5 hover:bg-white/10 text-white font-semibold text-lg py-4 px-8 transition duration-300 border border-white/10 w-full sm:w-auto"
      >
        Postularme como agente
      </a>
    </div>

    <p class="mt-6 text-sm text-white/50">Mirá la masterclass y dejá tus datos para sumarte al equipo.</p>
  </div>
</section>

<!-- VALOR / MENTOR -->
<section class="py-14 sm:py-16 px-4 sm:px-6 lg:px-8 bg-[#070F1F] text-white">
  <div class="max-w-5xl mx-auto grid grid-cols-1 lg:grid-cols-2 gap-10 items-center">
    <div>
      <h2 class="text-3xl md:text-4xl font-bold font-inter leading-tight">
        Aprendé a cerrar sin dudar, sin improvisar y sin desgastarte
      </h2>

      <p class="mt-5 text-lg text-white/75 font-inter leading-relaxed">
        En esta masterclass vas a aprender las estrategias reales que usan los agentes que inspiran confianza,
        negocian sin miedo y aceleran cierres sin fricción.
      </p>

      <div class="mt-7 rounded-2xl border border-white/10 bg-white/5 p-6">
        <p class="text-sm uppercase tracking-wider text-white/60 font-semibold">Mentor</p>
        <p class="mt-2 text-lg text-white/85 font-inter">
          Marcelo Napolitano te va a mostrar cómo pensar, cómo hablar y cómo actuar como un profesional que domina su mercado.
        </p>
      </div>
    </div>

    <div class="relative">
      <div class="absolute -inset-6 rounded-3xl bg-[#08407C]/20 blur-2xl"></div>

      <div class="relative rounded-3xl border border-white/10 bg-white/5 p-4">
        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
          <div class="aspect-[4/5] rounded-2xl bg-white/10 border border-white/10 overflow-hidden">
            <img src="/marcelo-hero.jpg" alt="Marcelo Napolitano" class="h-full w-full object-cover" loading="lazy" />
          </div>

          <div class="aspect-[4/5] rounded-2xl bg-white/10 border border-white/10 overflow-hidden">
            <img src="/marcelo-mentor.jpg" alt="Marcelo Napolitano" class="h-full w-full object-cover" loading="lazy" />
          </div>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- VIDEO (gateado por email) -->
<section id="video" class="py-14 sm:py-16 px-4 sm:px-6 lg:px-8 bg-[#070F1F]">
  <div class="max-w-4xl mx-auto">
    <div class="relative rounded-2xl overflow-hidden border border-white/10 bg-black">
      <div class="pointer-events-none absolute -inset-8 bg-[#08407C]/15 blur-2xl"></div>

      <div class="relative aspect-video">
        {#if videoLoaded}
          <iframe
            class="absolute inset-0 w-full h-full"
            src={"https://www.youtube-nocookie.com/embed/" + YOUTUBE_ID + "?autoplay=1&rel=0&modestbranding=1&playsinline=1"}
            title={VIDEO_TITLE}
            frameborder="0"
            allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
            allowfullscreen
          />
        {:else}
          <button
            type="button"
            class="absolute inset-0 w-full h-full grid place-items-center bg-black"
            on:click={openVideo}
            aria-label="Reproducir masterclass (requiere email)"
          >
            <img src={poster} alt="" class="absolute inset-0 w-full h-full object-cover opacity-80" loading="lazy" />
            <span class="relative z-10 flex items-center gap-3 rounded-full bg-black/60 px-6 py-3 border border-white/15">
              <span class="text-2xl">▶</span>
              <span class="text-white font-semibold">
                {videoUnlocked ? 'Reproducir masterclass' : 'Ver el video (dejanos tu email)'}
              </span>
            </span>
          </button>
        {/if}
      </div>
    </div>
  </div>

  <!-- MODAL EMAIL GATE -->
  {#if showGate}
    <div class="fixed inset-0 z-50">
      <button class="absolute inset-0 bg-black/70" on:click={() => (showGate = false)} aria-label="Cerrar"></button>

      <div
        class="absolute left-1/2 top-1/2 w-[min(32rem,calc(100%-2rem))] -translate-x-1/2 -translate-y-1/2 rounded-2xl border border-white/10 bg-[#0b1220] p-6 text-white shadow-2xl"
        role="dialog"
        aria-modal="true"
      >
        <h4 class="text-xl font-bold">Acceso al video</h4>
        <p class="mt-2 text-white/70">Dejanos tu email y habilitamos la reproducción.</p>

        <label class="mt-5 grid gap-2">
          <span class="text-sm text-white/70">Email</span>
          <input class="inputDark" type="email" bind:value={gateEmail} placeholder="tu@email.com" />
        </label>

        {#if gateError}
          <p class="mt-3 text-sm text-red-200">{gateError}</p>
        {/if}

        <div class="mt-6 flex justify-end gap-3">
          <button
            class="rounded-xl bg-white/10 hover:bg-white/15 border border-white/10 px-4 py-2"
            type="button"
            on:click={() => (showGate = false)}
          >
            Cancelar
          </button>

          <button class="rounded-xl bg-white text-[#08407C] font-bold px-4 py-2" type="button" on:click={handleGateEmail}>
            Ver video
          </button>
        </div>
      </div>
    </div>
  {/if}
</section>

<!-- FORM CHICO (Postulación + CV) -->
<section id="postulacion" class="py-14 sm:py-16 px-4 sm:px-6 lg:px-8 bg-[#070F1F]">
  <div class="max-w-4xl mx-auto">
    <div class="rounded-2xl border border-white/10 bg-white/5 p-6 sm:p-8 text-white">
      <h3 class="text-2xl sm:text-3xl font-bold font-inter">Postulate para ser agente</h3>
      <p class="mt-2 text-white/70">Dejanos tus datos y adjuntá tu CV. Te contactamos para el próximo paso.</p>

      <form on:submit={handleAgentForm} class="mt-6 space-y-5">
        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
          <label class="grid gap-2">
            <span class="text-sm text-white/70">Nombre</span>
            <input bind:value={a_nombre} required class="inputDark" />
          </label>

          <label class="grid gap-2">
            <span class="text-sm text-white/70">Apellido</span>
            <input bind:value={a_apellido} required class="inputDark" />
          </label>

          <label class="grid gap-2">
            <span class="text-sm text-white/70">Celular</span>
            <input bind:value={a_celular} required class="inputDark" placeholder="11 69726950" />
          </label>

          <label class="grid gap-2">
            <span class="text-sm text-white/70">Email</span>
            <input type="email" bind:value={a_email} required class="inputDark" />
          </label>
        </div>

        <label class="grid gap-2">
          <span class="text-sm text-white/70">CV (PDF/DOC/DOCX)</span>
          <input
            type="file"
            required
            class="inputFile"
            accept=".pdf,.doc,.docx,application/pdf,application/msword,application/vnd.openxmlformats-officedocument.wordprocessingml.document"
            on:change={(e) => (a_cv = e.currentTarget.files?.[0] ?? null)}
          />
        </label>

        <button
          type="submit"
          class="w-full sm:w-auto inline-flex items-center justify-center rounded-xl bg-white text-[#08407C] font-bold text-lg py-3 px-6 shadow-lg transition duration-300 transform hover:scale-[1.02] border border-white/10"
        >
          Enviar postulación
        </button>

        <p class="text-xs text-white/50">Al enviar, aceptás ser contactado por el equipo de Krak Real Estate.</p>
      </form>
    </div>
  </div>
</section>

<!-- BENEFICIOS -->
<section class="py-14 sm:py-16 px-4 sm:px-6 lg:px-8 bg-[#050A14]">
  <div class="max-w-5xl mx-auto">
    <h2 class="text-3xl md:text-4xl font-bold text-white text-center font-inter">Lo que cambia después de esta masterclass</h2>

    <div class="mt-10 grid grid-cols-1 md:grid-cols-2 gap-6">
      <div class="rounded-2xl border border-white/10 bg-white/5 p-6">
        <p class="text-white font-semibold text-lg">Generar confianza desde el primer contacto</p>
        <p class="mt-2 text-white/70">Entrar a cada charla con una forma clara de presentarte, encuadrar la conversación y marcar el ritmo.</p>
      </div>

      <div class="rounded-2xl border border-white/10 bg-white/5 p-6">
        <p class="text-white font-semibold text-lg">Manejar objeciones sin improvisar</p>
        <p class="mt-2 text-white/70">Responder con argumentos sólidos, sin ponerte a la defensiva ni regalar autoridad.</p>
      </div>

      <div class="rounded-2xl border border-white/10 bg-white/5 p-6">
        <p class="text-white font-semibold text-lg">Cerrar propiedades más rápido y con menos fricción</p>
        <p class="mt-2 text-white/70">Reducir vueltas, evitar desgaste y avanzar hacia el cierre con decisiones más claras.</p>
      </div>

      <div class="rounded-2xl border border-white/10 bg-white/5 p-6">
        <p class="text-white font-semibold text-lg">Posicionarte como referente, no como “uno más”</p>
        <p class="mt-2 text-white/70">Elevar tu estándar: cómo hablás, cómo proponés y cómo te plantás en el mercado.</p>
      </div>
    </div>

    <div class="mt-10 text-center">
      <a
        href="#postulacion"
        class="inline-flex items-center justify-center rounded-xl bg-[#08407C] hover:bg-[#0A4D95] text-white font-bold text-lg py-4 px-8 shadow-lg shadow-[#08407C]/30 transition duration-300 transform hover:scale-[1.02] border border-white/10"
      >
        Postularme como agente
      </a>
    </div>
  </div>
</section>

<!-- FORMULARIO ORIGINAL (backup) -->
<section id="formulario" class="py-16 sm:py-20 px-4 sm:px-6 lg:px-8 bg-[#f0f4f8]">
  <div class="max-w-3xl mx-auto">
    <h2 class="text-3xl sm:text-4xl font-bold mb-10 text-center font-inter text-gray-900">Completá el formulario para reservar tu lugar</h2>

    <form on:submit={handleForm} class="space-y-8 bg-white p-6 sm:p-8 rounded-xl shadow-md">
      {#if submitError}
        <div class="rounded-lg border border-red-200 bg-red-50 text-red-700 px-4 py-3 text-sm">{submitError}</div>
      {/if}

      {#if submitSuccess}
        <div class="rounded-lg border border-green-200 bg-green-50 text-green-800 px-4 py-3 text-sm">
          ¡Listo! Te registramos correctamente.
        </div>
      {/if}

      <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
        <div>
          <label for="nombre" class="block text-lg font-medium text-gray-700">Nombre</label>
          <input
            type="text"
            id="nombre"
            name="nombre"
            required
            class="mt-2 block w-full border-2 border-gray-300 rounded-lg shadow-sm py-3 px-4 focus:outline-none focus:ring-2 focus:ring-[#08407C] focus:border-[#08407C] text-lg"
          />
        </div>

        <div>
          <label for="apellido" class="block text-lg font-medium text-gray-700">Apellido</label>
          <input
            type="text"
            id="apellido"
            name="apellido"
            required
            class="mt-2 block w-full border-2 border-gray-300 rounded-lg shadow-sm py-3 px-4 focus:outline-none focus:ring-2 focus:ring-[#08407C] focus:border-[#08407C] text-lg"
          />
        </div>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
        <div>
          <label for="email" class="block text-lg font-medium text-gray-700">Email</label>
          <input
            type="email"
            id="email"
            name="email"
            required
            class="mt-2 block w-full border-2 border-gray-300 rounded-lg shadow-sm py-3 px-4 focus:outline-none focus:ring-2 focus:ring-[#08407C] focus:border-[#08407C] text-lg"
          />
        </div>

        <div>
          <label for="telefono" class="block text-lg font-medium text-gray-700">Teléfono</label>
          <input
            type="tel"
            id="telefono"
            name="telefono"
            required
            class="mt-2 block w-full border-2 border-gray-300 rounded-lg shadow-sm py-3 px-4 focus:outline-none focus:ring-2 focus:ring-[#08407C] focus:border-[#08407C] text-lg"
          />
        </div>
      </div>

      <div>
        <label for="comentario" class="block text-lg font-medium text-gray-700">Comentario</label>
        <textarea
          id="comentario"
          name="comentario"
          rows="4"
          bind:value={comentario}
          on:input={handleComentarioInput}
          maxlength={MAX_CHARS}
          class="mt-2 block w-full border-2 border-gray-300 rounded-lg shadow-sm py-3 px-4 focus:outline-none focus:ring-2 focus:ring-[#08407C] focus:border-[#08407C] text-lg"
        ></textarea>

        <p class="mt-2 text-sm text-gray-500">{comentarioWords}/{MAX_WORDS} palabras</p>
      </div>

      <div>
        <button
          type="submit"
          disabled={isSubmitting}
          class="w-full bg-white hover:bg-gray-100 text-[#08407C] font-bold text-xl py-4 px-6 rounded-lg shadow-lg transition duration-300 transform hover:scale-[1.02] border-2 border-[#08407C] disabled:opacity-60 disabled:cursor-not-allowed"
        >
          {isSubmitting ? 'Enviando...' : 'Inscribite gratis y empezá a jugar en serio'}
        </button>
      </div>
    </form>
  </div>
</section>

<style>
  .inputDark {
    padding: 1rem;
    border-radius: 0.9rem;
    border: 1px solid rgba(255, 255, 255, 0.12);
    background: rgba(255, 255, 255, 0.06);
    color: white;
    outline: none;
  }
  .inputDark:focus {
    border-color: rgba(255, 255, 255, 0.22);
    box-shadow: 0 0 0 3px rgba(8, 64, 124, 0.35);
  }
  .inputFile {
    padding: 0.9rem;
    border-radius: 0.9rem;
    border: 1px solid rgba(255, 255, 255, 0.12);
    background: rgba(255, 255, 255, 0.06);
    color: white;
  }
</style>
