<script>
  import { onDestroy } from 'svelte';

  // Countdown hacia el 15 de enero de 2026
  let days = '00';
  let hours = '00';
  let minutes = '00';
  let seconds = '00';

  const countDownDate = new Date('Jan 15, 2026 00:00:00').getTime();

  const countdownFunction = setInterval(() => {
    const now = new Date().getTime();
    const distance = countDownDate - now;

    if (distance < 0) {
      clearInterval(countdownFunction);
      return;
    }

    days = Math.floor(distance / (1000 * 60 * 60 * 24)).toString().padStart(2, '0');
    hours = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60)).toString().padStart(2, '0');
    minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60)).toString().padStart(2, '0');
    seconds = Math.floor((distance % (1000 * 60)) / 1000).toString().padStart(2, '0');
  }, 1000);

  // Limpieza al destruir el componente
  onDestroy(() => {
    clearInterval(countdownFunction);
  });

  // Función para manejar el formulario
  async function handleForm(e) {
    e.preventDefault();
    const form = new FormData(e.target);
    const body = JSON.stringify(Object.fromEntries(form));

    try {
      const res = await fetch('https://n8n-krak.com/webhook/newsletter', {
        method: 'POST',
        body,
      });
      if (res.ok) {
        alert('Gracias por inscribirte!');
        e.target.reset();
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
  <meta name="description" content="Inscríbete a la masterclass exclusiva para agentes de Krak Real Estate con Marcelo Napolitano." />
</svelte:head>

<!-- Hero con fondo azul -->
<section class="py-24 px-4 sm:px-6 lg:px-8 bg-[#08407C]">
  <div class="max-w-4xl mx-auto text-center">
    <h1 class="text-5xl md:text-6xl font-bold mb-6 text-white font-inter leading-tight">Masterclass de Krak Real Estate con Marcelo Napolitano</h1>
    <p class="text-2xl text-gray-200 mb-10 font-inter max-w-3xl mx-auto">Aprende las estrategias más efectivas para cerrar propiedades más rápido y generar confianza con tus clientes.</p>
    <a href="#formulario" class="inline-block bg-white hover:bg-gray-100 text-[#08407C] font-bold text-xl py-4 px-8 rounded-lg shadow-lg transition duration-300 transform hover:scale-105 border-2 border-white">Inscribite acá sin cargo</a>
  </div>
</section>

<!-- Video de Marcelo invitando a la Masterclass -->
<section class="py-16 px-4 sm:px-6 lg:px-8 bg-gray-50">
  <div class="max-w-4xl mx-auto">
    <h2 class="text-2xl font-bold text-gray-900 mb-6 text-center font-inter">🎥 Video de Marcelo invitando a la Masterclass</h2>
    <div class="aspect-video bg-black rounded-xl overflow-hidden">
      <video
  class="w-full h-full object-cover"
  controls
  preload="metadata"
  muted
  playsinline>
  <source src="Masterclasslanding.mp4" type="video/mp4">
  Tu navegador no soporta el elemento de video.
</video>
      </div>
      </div>
</section>

<!-- Countdown con fondo blanco y números grandes -->
<section class="py-20 px-4 sm:px-6 lg:px-8 bg-white">
  <div class="max-w-4xl mx-auto text-center">
    <h2 class="text-4xl font-bold text-gray-900 mb-10 font-inter">⏰ ¡Quedan solo:</h2>
    <div class="flex flex-wrap justify-center gap-10 text-6xl font-bold text-[#08407C] font-inter"> <!-- Agrandado a text-6xl -->
      <div class="flex flex-col items-center min-w-[120px]">
        <span class="text-7xl">{days}</span> <!-- Agrandado aún más -->
        <span class="text-lg font-normal mt-2 text-gray-600">días</span>
      </div>
      <div class="flex flex-col items-center min-w-[120px]">
        <span class="text-7xl">{hours}</span>
        <span class="text-lg font-normal mt-2 text-gray-600">horas</span>
      </div>
      <div class="flex flex-col items-center min-w-[120px]">
        <span class="text-7xl">{minutes}</span>
        <span class="text-lg font-normal mt-2 text-gray-600">minutos</span>
      </div>
      <div class="flex flex-col items-center min-w-[120px]">
        <span class="text-7xl">{seconds}</span>
        <span class="text-lg font-normal mt-2 text-gray-600">segundos</span>
      </div>

    </div>
  </div>
</section>

<!-- Countdown con fondo blanco y números grandes -->
<section class="py-20 px-4 sm:px-6 lg:px-8 bg-white">
  <div class="max-w-4xl mx-auto text-center">
    <h2 class="text-4xl font-bold text-gray-900 mb-10 font-inter">⏰ ¡Quedan solo:</h2>
    <div class="flex flex-wrap justify-center gap-10 text-6xl font-bold text-[#08407C] font-inter"> <!-- Agrandado a text-6xl -->
      <div class="flex flex-col items-center min-w-[120px]">
        <span class="text-7xl">{days}</span> <!-- Agrandado aún más -->
        <span class="text-lg font-normal mt-2 text-gray-600">días</span>
      </div>
      <div class="flex flex-col items-center min-w-[120px]">
        <span class="text-7xl">{hours}</span>
        <span class="text-lg font-normal mt-2 text-gray-600">horas</span>
      </div>
      <div class="flex flex-col items-center min-w-[120px]">
        <span class="text-7xl">{minutes}</span>
        <span class="text-lg font-normal mt-2 text-gray-600">minutos</span>
      </div>
      <div class="flex flex-col items-center min-w-[120px]">
        <span class="text-7xl">{seconds}</span>
        <span class="text-lg font-normal mt-2 text-gray-600">segundos</span>
      </div>
    </div>
  </div>
</section>

<!-- Formulario de inscripción con fondo azul claro -->
<section id="formulario" class="py-20 px-4 sm:px-6 lg:px-8 bg-[#f0f4f8]"> <!-- Fondo azul claro -->
  <div class="max-w-3xl mx-auto">
    <h2 class="text-4xl font-bold mb-10 text-center font-inter text-gray-900">Completá el formulario para reservar tu lugar</h2>
    <form on:submit|preventDefault={handleForm} class="space-y-8 bg-white p-8 rounded-xl shadow-md">
      <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
        <div>
          <label for="nombre" class="block text-lg font-medium text-gray-700">Nombre</label>
          <input type="text" id="nombre" name="nombre" required class="mt-2 block w-full border-2 border-gray-300 rounded-lg shadow-sm py-3 px-4 focus:outline-none focus:ring-2 focus:ring-[#08407C] focus:border-[#08407C] text-lg">
        </div>
        <div>
          <label for="apellido" class="block text-lg font-medium text-gray-700">Apellido</label>
          <input type="text" id="apellido" name="apellido" required class="mt-2 block w-full border-2 border-gray-300 rounded-lg shadow-sm py-3 px-4 focus:outline-none focus:ring-2 focus:ring-[#08407C] focus:border-[#08407C] text-lg">
        </div>
      </div>
      <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
        <div>
          <label for="email" class="block text-lg font-medium text-gray-700">Email</label>
          <input type="email" id="email" name="email" required class="mt-2 block w-full border-2 border-gray-300 rounded-lg shadow-sm py-3 px-4 focus:outline-none focus:ring-2 focus:ring-[#08407C] focus:border-[#08407C] text-lg">
        </div>
        <div>
          <label for="telefono" class="block text-lg font-medium text-gray-700">Teléfono</label>
          <input type="tel" id="telefono" name="telefono" required class="mt-2 block w-full border-2 border-gray-300 rounded-lg shadow-sm py-3 px-4 focus:outline-none focus:ring-2 focus:ring-[#08407C] focus:border-[#08407C] text-lg">
        </div>
      </div>
      <div>
        <label for="comentario" class="block text-lg font-medium text-gray-700">Comentario</label>
        <textarea id="comentario" name="comentario" rows="4" class="mt-2 block w-full border-2 border-gray-300 rounded-lg shadow-sm py-3 px-4 focus:outline-none focus:ring-2 focus:ring-[#08407C] focus:border-[#08407C] text-lg"></textarea>
      </div>
      <div>
        <button type="submit" class="w-full bg-white hover:bg-gray-100 text-[#08407C] font-bold text-xl py-4 px-6 rounded-lg shadow-lg transition duration-300 transform hover:scale-105 border-2 border-[#08407C]">Enviar inscripción</button> <!-- Botón con borde -->
      </div>
    </form>
  </div>
</section>