<script lang="ts">
	import { reCaptcha } from "$lib";

	let prevButton: HTMLButtonElement;
	let nextButton: HTMLButtonElement;
	let aboutUsCarousel: HTMLDivElement;
    let curriculumFileName: string = ""
	let featuresCarousel: HTMLDivElement

    async function handleForm(e: SubmitEvent & {
        currentTarget: EventTarget & HTMLFormElement;
    }) {
		let target = e.target
        if (!(target instanceof HTMLFormElement)) return;
		
        target.classList.add('await')
        try {
			let form = new FormData(target)
			let token = await reCaptcha()

			form.append('g-recaptcha-response', token)
            let res = await fetch('https://n8n-krak.com/webhook/contacto-agentes', { method: 'POST', body: form })
            if (res.ok) {
                target.classList.add('then')
            } else throw new Error()
        } catch(_) {
            console.log(_)
            target.classList.add('catch')
        }
		setTimeout(()=>{
			target.classList.remove('await')
		}, 1500)
    }
</script>

<section class="bg-cover min-h-[50svh] sm:min-h-[100svh] text-white grid place-content-center text-center relative">
	<img class="absolute top-0 left-0 w-full h-full object-cover object-right blur -z-10" alt="" src="data:image/png;base64,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"/>
	<img class="absolute top-0 left-0 w-full h-full object-cover object-right" alt="" src="/hero-1.png"/>

	<div class="drop-shadow-2xl">
		<h1 class="text-5xl lg:text-6xl font-bold mb-4 mt-24">Convertite en un Krak.</h1>
		<p class="mb-8">Unite a nosotros y transformá tu carrera profesional.</p>
		<a href="#contacto" class="block bg-red-500 hover:bg-black text-white uppercase rounded-full w-fit mx-auto py-6 px-14 font-bold text-sm"> Quiero empezar </a>
	</div>
	<a href="#banner-1" class="absolute bottom-8 right-8 flex-col hidden sm:flex items-center gap-4">
		<span class="text-sm">SCROLL</span>
		<img class="h-12" src="/svg/arrow-white.svg" alt="" />
	</a>
</section>

<section class="bg-red-500 text-white" id="banner-1">
	<div class="mx-auto w-full max-w-6xl grid lg:grid-cols-2 px-4">
		<h2 class="text-3xl lg:text-5xl font-medium py-20 text-balance">Forma parte de un<br> equipo de bienes<br> raíces al servicio de<br> tu desarrollo personal<br> y profesional</h2>
		<div class="relative h-80 lg:h-full">
			<img class="absolute w-fit h-96 bottom-0 inset-x-0 mx-auto lg:left-0" src="/svg/circle.svg" alt="" />
			<img class="absolute w-fit h-72 lg:h-80 bottom-0 inset-x-0 mx-auto" src="/woman.png" alt="woman" />
		</div>
	</div>
</section>

<section class="py-20">
	<div class="mx-auto w-full max-w-6xl">
		<h2 class="text-4xl lg:text-5xl font-semibold text-red-500 text-center mb-12">Ventajas de ser un Krak</h2>
		<div bind:this={featuresCarousel} class="overflow-x-scroll flex px-[calc((100vw-20rem)/2)] lg:px-0 no-scrollbar snap-x snap-mandatory lg:grid lg:grid-cols-3 lg:place-content-center mb-8">
			<div class="group overflow-hidden relative hover:bg-red-500 rounded-3xl min-w-64 px-4 pb-16 lg:pb-10 lg:px-10 p-10 hover:text-white transition-all group snap-always snap-center">
				<div class="h-16 w-fit mx-auto mb-8 text-7xl group-hover:text-white text-red-500 transition-all"><span class="[mask-image:url(/svg/feature-1.svg)] [mask-repeat:no-repeat] [mask-size:100%_100%] inline-block bg-current h-[1em] w-[1em]"></span></div>
				<p class="group-hover:translate-y-64 transition-all text-center mx-auto text-sm max-w-64">Visibilidad destacada en los portales inmobiliarios más importantes del país</p>
				<p class="absolute group-hover:translate-y-0 translate-y-64 top-32 mt-2 text-center inset-x-0 text-sm transition-all max-w-52 lg:max-w-64 mx-auto text-balance !text-white">Obtene visibilidad destacada en los portales inmobiliarios más importantes del país.</p>
				<button on:click={()=>{featuresCarousel.scrollBy({behavior: 'smooth', left: 1})}} class="absolute lg:hidden bottom-4 right-4 flex items-center gap-2 font-bold">Ver más <span class="text-3xl icon-[tabler--hand-move]"></span></button>
			</div>
			<div class="group overflow-hidden relative hover:bg-red-500 rounded-3xl min-w-64 px-4 pb-16 lg:pb-10 lg:px-10 p-10 hover:text-white transition-all group snap-always snap-center">
				<div class="h-16 w-fit mx-auto mb-8 text-7xl group-hover:text-white text-red-500 transition-all"><span class="[mask-image:url(/svg/feature-2.svg)] [mask-repeat:no-repeat] [mask-size:100%_100%] inline-block bg-current h-[1em] w-[1em]"></span></div>
				<p class="group-hover:translate-y-64 transition-all text-center mx-auto text-lg">Exposición de tu cartera<br /> en redes sociales</p>
				<p class="absolute group-hover:translate-y-0 translate-y-64 top-32 mt-2 text-center inset-x-0 text-sm transition-all max-w-52 lg:max-w-64 mx-auto text-balance !text-white">Adaptamos tu cartera a piezas de redes sociales para mayor alcance.</p>
			</div>
			<div class="group overflow-hidden relative hover:bg-red-500 rounded-3xl min-w-64 px-4 pb-16 lg:pb-10 lg:px-10 p-10 hover:text-white transition-all group snap-always snap-center">
				<div class="h-16 w-fit mx-auto mb-8 text-7xl group-hover:text-white text-red-500 transition-all"><span class="[mask-image:url(/svg/feature-3.svg)] [mask-repeat:no-repeat] [mask-size:100%_100%] inline-block bg-current h-[1em] w-[1em]"></span></div>
				<p class="group-hover:translate-y-64 transition-all text-center mx-auto text-lg">Comisiones<br /> más justas</p>
				<p class="absolute group-hover:translate-y-0 translate-y-64 top-32 mt-2 text-center inset-x-0 text-sm transition-all max-w-52 lg:max-w-64 mx-auto text-balance !text-white">Valoramos tu esfuerzo y reconocemos tus habilidades. Sabemos que tu dedicación es fundamental para alcanzar el éxito juntos.</p>
			</div>
			<div class="group overflow-hidden relative hover:bg-red-500 rounded-3xl min-w-64 px-4 pb-16 lg:pb-10 lg:px-10 p-10 hover:text-white transition-all group snap-always snap-center">
				<div class="h-16 w-fit mx-auto mb-8 text-7xl group-hover:text-white text-red-500 transition-all"><span class="[mask-image:url(/svg/feature-4.svg)] [mask-repeat:no-repeat] [mask-size:100%_100%] inline-block bg-current h-[1em] w-[1em]"></span></div>
				<p class="group-hover:translate-y-64 transition-all text-center mx-auto text-lg">Asistencia en el desarrollo<br /> de tu marca personal</p>
				<p class="absolute group-hover:translate-y-0 translate-y-64 top-32 mt-2 text-center inset-x-0 text-sm transition-all max-w-52 lg:max-w-64 mx-auto text-balance !text-white">Capacitación especializada, mentoría personalizada y recursos de marketing avanzados para fortalecer tu marca personal.</p>
			</div>
			<div class="group overflow-hidden relative hover:bg-red-500 rounded-3xl min-w-64 px-4 pb-16 lg:pb-10 lg:px-10 p-10 hover:text-white transition-all group snap-always snap-center">
				<div class="h-16 w-fit mx-auto mb-8 text-6xl group-hover:text-white text-red-500 transition-all"><span class="[mask-image:url(/svg/feature-5.svg)] [mask-repeat:no-repeat] [mask-size:100%_100%] inline-block bg-current h-[1em] w-[1em]"></span></div>
				<p class="group-hover:translate-y-64 transition-all text-center mx-auto text-lg">Capacitaciones<br /> constantes</p>
				<p class="absolute group-hover:translate-y-0 translate-y-64 top-32 mt-2 text-center inset-x-0 text-sm transition-all max-w-52 lg:max-w-64 mx-auto text-balance !text-white">Programas continuos para mantenerte actualizado en el rubro. No creemos en aprender por aprender, queremos que tengas resultados.</p>
			</div>
			<div class="group overflow-hidden relative hover:bg-red-500 rounded-3xl min-w-64 px-4 pb-16 lg:pb-10 lg:px-10 p-10 hover:text-white transition-all group snap-always snap-center">
				<div class="h-16 w-fit mx-auto mb-8 text-7xl group-hover:text-white text-red-500 transition-all"><span class="[mask-image:url(/svg/feature-6.svg)] [mask-repeat:no-repeat] [mask-size:100%_100%] inline-block bg-current h-[1em] w-[1em]"></span></div>
				<p class="group-hover:translate-y-64 transition-all text-center mx-auto text-lg">Bonificaciones<br /> por objetivos</p>
				<p class="absolute group-hover:translate-y-0 translate-y-64 top-32 mt-2 text-center inset-x-0 text-sm transition-all max-w-52 lg:max-w-64 mx-auto text-balance !text-white">Obtén bonificaciones al alcanzar tus metas. Gana más con cada objetivo cumplido.</p>
			</div>
			<div class="group overflow-hidden relative hover:bg-red-500 rounded-3xl min-w-64 px-4 pb-16 lg:pb-10 lg:px-10 p-10 hover:text-white transition-all group snap-always snap-center">
				<div class="h-16 w-fit mx-auto mb-8 text-7xl group-hover:text-white text-red-500 transition-all"><span class="[mask-image:url(/svg/feature-7.svg)] [mask-repeat:no-repeat] [mask-size:100%_100%] inline-block bg-current h-[1em] w-[1em]"></span></div>
				<p class="group-hover:translate-y-64 transition-all text-center mx-auto text-lg">Ambiente profesional<br /> y desafiante</p>
				<p class="absolute group-hover:translate-y-0 translate-y-64 top-32 mt-2 text-center inset-x-0 text-sm transition-all max-w-52 lg:max-w-64 mx-auto text-balance !text-white">Entorno de compañerismo y colaboración con profesionales de experiencia.</p>
			</div>
			<div class="group overflow-hidden relative hover:bg-red-500 rounded-3xl min-w-64 px-4 pb-16 lg:pb-10 lg:px-10 p-10 hover:text-white transition-all group snap-always snap-center">
				<div class="h-16 w-fit mx-auto mb-8 text-6xl group-hover:text-white text-red-500 transition-all pt-4"><span class="[mask-image:url(/svg/feature-8.svg)] [mask-repeat:no-repeat] [mask-size:100%_100%] inline-block bg-current h-[1em] w-[1em]"></span></div>
				<p class="group-hover:translate-y-64 transition-all text-center mx-auto text-lg">Infraestructura</p>
				<p class="absolute group-hover:translate-y-0 translate-y-64 top-32 mt-2 text-center inset-x-0 text-sm transition-all max-w-52 lg:max-w-64 mx-auto text-balance !text-white">Oficina totalmente equipada con soporte adminsitrativo, salas de reuniones, terraza y comedor.</p>
			</div>
			<div class="group overflow-hidden relative hover:bg-red-500 rounded-3xl min-w-64 px-4 pb-16 lg:pb-10 lg:px-10 p-10 hover:text-white transition-all group snap-always snap-center">
				<div class="h-16 w-fit mx-auto mb-8 text-5xl group-hover:text-white text-red-500 transition-all pt-4"><span class="[mask-image:url(/svg/feature-9.svg)] [mask-repeat:no-repeat] [mask-size:100%_100%] inline-block bg-current h-[1em] w-[1em]"></span></div>
				<p class="group-hover:translate-y-64 transition-all text-center mx-auto text-lg">Cultura<br /> emprendedora</p>
				<p class="absolute group-hover:translate-y-0 translate-y-64 top-32 mt-2 text-center inset-x-0 text-sm transition-all max-w-52 lg:max-w-64 mx-auto text-balance !text-white">Desarrolla tu iniciativa y habilidades de liderazgo. Son emprendedores quienes lideran el mercado.</p>
			</div>
		</div>
	</div>
	<a href="#contacto" class="block bg-red-500 hover:bg-black text-white uppercase rounded-full w-fit mx-auto py-6 px-14 font-bold text-sm"> Quiero ser un Krak </a>
</section>

<section class="pt-20 pb-24 bg-[#313B51]">
	<div class="flex flex-col-reverse items-center text-white mb-12">
		<h2 class="text-4xl lg:text-5xl font-bold text-balance text-center">¿Qué significa ser un Krak?</h2>
		<p class="font-medium mb-4 text-gray-100">Te contamos en pocos minutos</p>
	</div>
	<div class="max-w-4xl w-full mx-auto rounded-3xl aspect-video bg-black drop-shadow-2xl shadow-2xl mb-12 overflow-hidden">
		<iframe height="720" width="1280" class="w-full h-full" src="https://www.youtube.com/embed/e9mFXA4KLGo?si=BzCVF7Ea35kDMF-U&amp;controls=0" title="¿Qué significa ser un Krak?" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
	</div>
	<a href="#contacto" class="block bg-white hover:bg-black hover:text-white text-red-500 uppercase rounded-full w-fit mx-auto py-6 px-14 font-bold text-sm"> Quiero ser un Krak </a>
</section>

<section class="bg-white py-20 relative">
	<div class="flex flex-col-reverse items-center mb-12">
		<h2 class="text-4xl lg:text-5xl font-bold text-red-500">Nuestra cultura</h2>
		<p class="font-medium mb-4 text-gray-600">Para el trabajo y para la vida</p>
	</div>
	<p class="max-w-xl mx-auto w-fit lg:text-center text-balance px-6 lg:text-pretty mb-16 text-lg text-gray-600">Estos son nuestros valores para el trabajo y para la vida, con estos valores nos guiamos y le damos la bienvenida a cada uno que se incorpore a la experiencia de ser parte de Krak.</p>
	<div class="hidden lg:block">
		<button
			on:click={() => {
				aboutUsCarousel.scrollBy({ behavior: 'smooth', left: -1 });
			}}
			bind:this={prevButton}
			class="absolute size-16 z-10 hidden bg-white hover:bg-black hover:text-white rounded-full grid place-content-center drop-shadow-2xl top-[24rem] left-12"><span class="icon-[tabler--chevron-left] text-3xl"></span></button>
		<button
			on:click={() => {
				aboutUsCarousel.scrollBy({ behavior: 'smooth', left: 1 });
			}}
			bind:this={nextButton}
			class="absolute size-16 z-10 bg-white hover:bg-black hover:text-white rounded-full grid place-content-center drop-shadow-2xl top-[24rem] right-12"><span class="icon-[tabler--chevron-right] text-3xl"></span></button>
	</div>

	<div
		bind:this={aboutUsCarousel}
		on:scroll={(e) => {
			let target = e.currentTarget;

			if (target.scrollLeft == 0) {
				prevButton.classList.add('hidden');
				nextButton.classList.remove('hidden');
			}
			if (target.scrollLeft == target.scrollWidth - target.clientWidth) {
				prevButton.classList.remove('hidden');
				nextButton.classList.add('hidden');
			}
		}}
		class="overflow-x-scroll flex gap-4 px-[calc((100vw-20rem)/2)] md:px-[calc((100vw-64rem)/2)] no-scrollbar snap-x snap-mandatory">
		<div class="bg-red-500 text-white rounded-3xl w-80 min-w-80 px-8 pt-7 pb-16 snap-always snap-center relative">
			<h3 class="font-thin text-5xl font-inter text-red-200 mb-4">01</h3>
			<h4 class="font-bold text-2xl mb-4">Lealtad</h4>
			<p class="text-sm text-red-100">Sobre todo lealtad! Quiere decir que estamos para nuestros compañeros, estamos en primera linea con ellos, y valoramos los esfuerzos de cada uno porque ellos harían lo mismo por nosotros. Somos leales con nuestro compañeros y con nosotros mismos buscando alcanzar nuestros objetivos con caballerosidad y códigos.</p>
			<button on:click={()=>{aboutUsCarousel.scrollBy({behavior: 'smooth', left: 1})}} class="absolute lg:hidden bottom-4 right-4 text-red-300 flex items-center gap-2 font-bold">Ver más <span class="text-3xl icon-[tabler--hand-move]"></span></button>
		</div>
		<div class="bg-red-500 text-white rounded-3xl w-80 min-w-80 px-8 pt-7 pb-16 snap-always snap-center">
			<h3 class="font-thin text-5xl font-inter text-red-200 mb-4">02</h3>
			<h4 class="font-bold text-2xl mb-4">Todos somos líderes</h4>
			<p class="text-sm text-red-100">Nuestro trabajo es el reflejo de nuestra personalidad. Es nuestra carta de presentación y fija los estándares de calidad de quienes nos rodean. No hay mejor forma para liderar que haber hecho el trabajo antes. Lideramos desde el frente de batalla, no desde la comodidad del fondo de la linea.</p>
		</div>
		<div class="bg-red-500 text-white rounded-3xl w-80 min-w-80 px-8 pt-7 pb-16 snap-always snap-center">
			<h3 class="font-thin text-5xl font-inter text-red-200 mb-4">03</h3>
			<h4 class="font-bold text-2xl mb-4">Valor agregado</h4>
			<p class="text-sm text-red-100">Entendemos por valor agregado superar las expectativas de nuestra contraparte, sea un cliente o un compañero. Dedicarle un tiempo extra a escuchar a un compañero también es valor agregado. Siempre causar el impacto positivo en la otra persona a través de un extra no esperado.</p>
		</div>
		<div class="bg-red-500 text-white rounded-3xl w-80 min-w-80 px-8 pt-7 pb-16 snap-always snap-center">
			<h3 class="font-thin text-5xl font-inter text-red-200 mb-4">04</h3>
			<h4 class="font-bold text-2xl mb-4">Humildad</h4>
			<p class="text-sm text-red-100">
				No nos conformamos nunca, siempre vamos por más. Somos seres de objetivos, si algún día consideramos que llegamos a lo que queremos ser, será el día que dejaremos de existir. El camino es claro, siempre para arriba y adelante, nunca para atrás. En los momentos de que logramos nuestros objetivos, son los momentos de mayor humildad, siempre hay alguien mejor que nosotros, pero te vamos a
				alcanzar.
			</p>
		</div>


		<div class="bg-red-500 text-white rounded-3xl w-80 min-w-80 px-8 pt-7 pb-16 snap-always snap-center">
			<h3 class="font-thin text-5xl font-inter text-red-200 mb-4">05</h3>
			<h4 class="font-bold text-2xl mb-4">Responsabilidad</h4>
			<p class="text-sm text-red-100">
				Definimos responsabilidad, como la habilidad de responder por nuestros actos. No apuntamos con el dedo, sino que aceptamos sin ego y sin emoción la habilidad de respuesta de nuestros actos. No existen los reproches, sino nuevos problemas a resolver. Somos un equipo, si uno falla, no fue culpa de una persona, es culpa de todos. Estamos jugando y lo hacemos en serio.
			</p>
		</div>
		<div class="bg-red-500 text-white rounded-3xl w-80 min-w-80 px-8 pt-7 pb-16 snap-always snap-center">
			<h3 class="font-thin text-5xl font-inter text-red-200 mb-4">06</h3>
			<h4 class="font-bold text-2xl mb-4">Disciplina</h4>
			<p class="text-sm text-red-100">
				Cuando las cosas no salen, no entrometerse a la emoción en el medio. Corremos a un lado y atacamos el problema, corregimos y ejecutamos. La disciplina nos permite cumplir con lo que dijimos que íbamos a hacer, le da validez a nuestra palabra para que sea escuchada. La palabra es el capital más importante que tenemos para con nosotros mismos y el cliente.
			</p>
		</div>
		<div class="bg-red-500 text-white rounded-3xl w-80 min-w-80 px-8 pt-7 pb-16 snap-always snap-center">
			<h3 class="font-thin text-5xl font-inter text-red-200 mb-4">07</h3>
			<h4 class="font-bold text-2xl mb-4">Iniciativa</h4>
			<p class="text-sm text-red-100">
				No esperamos que otra persona nos diga qué hacer. cuando necesita hacerse, se hace y punto. Ejecutamos en unidad las tareas de cada uno. Nuestro equipo es nuestra comunidad, si queremos que las cosas cambien tenemos que tomar la iniciativa, desde cumplir con las tareas, ayudar al compañero, ordenar el puesto de trabajo, colaborar con la limpieza y otros. Como hacer una cosa, haces todo.
			</p>
		</div>
		<div class="bg-red-500 text-white rounded-3xl w-80 min-w-80 px-8 pt-7 pb-16 snap-always snap-center">
			<h3 class="font-thin text-5xl font-inter text-red-200 mb-4">08</h3>
			<h4 class="font-bold text-2xl mb-4">Buen clima de trabajo</h4>
			<p class="text-sm text-red-100">
				Parte de nuestro trabajo es generar un buen clima laboral, para ello tenemos que apoyar a nuestro compañero, desde lo personal y lo profesional. No dejemos a nadie atrás. Somos un equipo y pasamos mucho tiempo juntos, los lazos entre las personas crean equipos fuertes, es parte de nuestro trabajo hacerlo.
			</p>
		</div>
		<div class="bg-red-500 text-white rounded-3xl w-80 min-w-80 px-8 pt-7 pb-16 snap-always snap-center">
			<h3 class="font-thin text-5xl font-inter text-red-200 mb-4">09</h3>
			<h4 class="font-bold text-2xl mb-4">Aprendemos todo el tiempo</h4>
			<p class="text-sm text-red-100">
				Invertimos tiempo en nosotros para ser cada día mejores, pero no para nosotros sino para los demás. Una mejor performance eleva la vara. Nuestro desarrollo personal trae beneficios a quienes nos rodean, es por eso que tenemos que ser mejores por nuestra cuenta primero para que el grupo sea mejor. Cualquiera puede ser parte de un equipo de corderos, ser parte de un equipo de leones es el verdadero desafío.
			</p>
		</div>
		<div class="bg-red-500 text-white rounded-3xl w-80 min-w-80 px-8 pt-7 pb-16 snap-always snap-center">
			<h3 class="font-thin text-5xl font-inter text-red-200 mb-4">10</h3>
			<h4 class="font-bold text-2xl mb-4">Dar sin agenda</h4>
			<p class="text-sm text-red-100">
				Hacemos lo correcto y no lo que es más cómodo. Vamos por todo sin esperar nada a cambio. Agregamos valor sin el ego de buscar reconocimiento, los logros de un compañero son los logros del equipo. Cuando logramos un objetivo lo celebramos todo porque el éxito de un compañero eleva el estándar de calidad del resto.
			</p>
		</div>
		<div class="bg-red-500 text-white rounded-3xl w-80 min-w-80 px-8 pt-7 pb-16 snap-always snap-center">
			<h3 class="font-thin text-5xl font-inter text-red-200 mb-4">11</h3>
			<h4 class="font-bold text-2xl mb-4">Los ojos en el objetivo</h4>
			<p class="text-sm text-red-100">
				Siempre miramos el cuadro completo. No trabajamos solo para hacer sino también para parecer. Sabemos a dónde queremos ir y estamos dispuestos a hacer lo necesario para llegar al objetivo. La tenacidad y la disciplina siempre la pagan a largo plazo. No tenemos FE en que vamos a ser mejores, tenemos absoluta CERTEZA de que lo somos y vamos por mas.
			</p>
		</div>
	</div>
</section>

<section class="bg-gray-100 py-20 w-full">
	<h2 class="text-5xl text-red-500 mb-16 text-center font-semibold">Preguntas Frecuentes</h2>
	<ul class="flex flex-col gap-2 max-w-xs lg:max-w-4xl mx-auto">
		<li>
			<label class="flex flex-col bg-red-500 text-white py-4 px-12 rounded-xl has-[:checked]:bg-red-50 group has-[:checked]:text-black hover:cursor-pointer relative">
                <input class="sr-only peer" name="faq" type="checkbox" />
				<span class="absolute top-4 right-10 icon-[tabler--chevron-down] text-3xl peer-checked:rotate-180 hidden lg:block"></span>
				<h3 class="text-xl font-semibold">¿Por qué ser un agente Krak?</h3>
				<div class="max-h-0 overflow-hidden peer-checked:max-h-96 transition-all">
                    <div class="py-4">
                        <p class="text-gray-700">Porque nuestro plan es darte las herramientas para ser una agente altamente competitivo en el mercado y acompañarte en el proceso para lograrlo.</p>
                    </div>
				</div>
            </label>
		</li>
		<li>
			<label class="flex flex-col bg-red-500 text-white py-4 px-12 rounded-xl has-[:checked]:bg-red-50 group has-[:checked]:text-black hover:cursor-pointer relative">
                <input class="sr-only peer" name="faq" type="checkbox" />
				<span class="absolute top-4 right-10 icon-[tabler--chevron-down] text-3xl peer-checked:rotate-180 hidden lg:block"></span>
				<h3 class="text-xl font-semibold">¿Tengo que tener experiencia?</h3>
				<div class="max-h-0 overflow-hidden peer-checked:max-h-96 transition-all">
                    <div class="py-4">
                        <p class="text-gray-700">Es ideal que tengas formacion vinculada al rubro, pero lo mas importante es que tengas la misma cultura emprededora que tenemos nosotros.</p>
                    </div>
				</div>
            </label>
		</li>
		<li>
			<label class="flex flex-col bg-red-500 text-white py-4 px-12 rounded-xl has-[:checked]:bg-red-50 group has-[:checked]:text-black hover:cursor-pointer relative">
                <input class="sr-only peer" name="faq" type="checkbox" />
				<span class="absolute top-4 right-10 icon-[tabler--chevron-down] text-3xl peer-checked:rotate-180 hidden lg:block"></span>
				<h3 class="text-xl font-semibold">¿Tengo que pagar algun fee para trabajar con ustedes?</h3>
				<div class="max-h-0 overflow-hidden peer-checked:max-h-96 transition-all">
                    <div class="py-4">
                        <p class="text-gray-700">No cobramos por el valor de tu trabajo. Nuestro compromiso se traduce en invertir en tu desarrollo.</p>
                    </div>
				</div>
            </label>
		</li>
		<li>
			<label class="flex flex-col bg-red-500 text-white py-4 px-12 rounded-xl has-[:checked]:bg-red-50 group has-[:checked]:text-black hover:cursor-pointer relative">
                <input class="sr-only peer" name="faq" type="checkbox" />
				<span class="absolute top-4 right-10 icon-[tabler--chevron-down] text-3xl peer-checked:rotate-180 hidden lg:block"></span>
				<h3 class="text-xl font-semibold">¿Cuál es el diferencial de Krak Real Estate?</h3>
				<div class="max-h-0 overflow-hidden peer-checked:max-h-96 transition-all">
                    <div class="py-4">
                        <p class="text-gray-700">Contamos con una agencia de marketing inhouse al servicio de nuestros equipos de trabajo.</p>
                    </div>
				</div>
            </label>
		</li>
		<li>
			<label class="flex flex-col bg-red-500 text-white py-4 px-12 rounded-xl has-[:checked]:bg-red-50 group has-[:checked]:text-black hover:cursor-pointer relative">
                <input class="sr-only peer" name="faq" type="checkbox" />
				<span class="absolute top-4 right-10 icon-[tabler--chevron-down] text-3xl peer-checked:rotate-180 hidden lg:block"></span>
				<h3 class="text-xl font-semibold">¿Las capacitaciones tienen costo?</h3>
				<div class="max-h-0 overflow-hidden peer-checked:max-h-96 transition-all">
                    <div class="py-4">
                        <p class="text-gray-700">Ningun costo, tu desarrollo es la garantia de nuestro exito.</p>
                    </div>
				</div>
            </label>
		</li>
		<li>
			<label class="flex flex-col bg-red-500 text-white py-4 px-12 rounded-xl has-[:checked]:bg-red-50 group has-[:checked]:text-black hover:cursor-pointer relative">
                <input class="sr-only peer" name="faq" type="checkbox" />
				<span class="absolute top-4 right-10 icon-[tabler--chevron-down] text-3xl peer-checked:rotate-180 hidden lg:block"></span>
				<h3 class="text-xl font-semibold">¿Cómo puedo sumarme al equipo?</h3>
				<div class="max-h-0 overflow-hidden peer-checked:max-h-96 transition-all">
                    <div class="py-4">
                        <p class="text-gray-700">Dejanos tus datos en el formulario al pie y uno de nustros team leaders estara contactandose con vos.</p>
                    </div>
				</div>
            </label>
		</li>
	</ul>
</section>

<section class="pt-20 pb-24 bg-[#313B51]" id="contacto">
	<div class="flex flex-col-reverse items-center text-white mb-12 lg:mb-20">
		<h2 class="text-4xl lg:text-5xl font-bold text-center">Queremos <br class="lg:hidden"> conocerte</h2>
	</div>
	<form on:submit|preventDefault={handleForm} class="max-w-xs group lg:max-w-4xl w-full mx-auto rounded-3xl relative">
		<div class="group-then:-translate-x-full group-catch:translate-x-full group-then:opacity-0 group-catch:opacity-0 transition-all">
			<label class="text-gray-500 mb-4 relative flex group-await:opacity-50">
				<input class="rounded-xl peer pt-7 pb-2 pl-12 px-4 w-full font-bold" type="text" placeholder="" maxlength="255" name="name" required> 
				<span class="absolute peer-placeholder-shown:top-5 peer-placeholder-shown:text-gray-500 peer-placeholder-shown:text-base peer-focus:text-xs peer-focus:top-2 peer-focus:text-gray-500 text-gray-500 text-xs left-12 top-2 transition-all">Nombre</span>
			</label>
			<label class="text-gray-500 mb-4 relative flex group-await:opacity-50">
				<input class="rounded-xl peer pt-7 pb-2 pl-12 px-4 w-full font-bold" type="email" placeholder="" maxlength="255" name="email" required> 
				<span class="absolute peer-placeholder-shown:top-5 peer-placeholder-shown:text-gray-500 peer-placeholder-shown:text-base peer-focus:text-xs peer-focus:top-2 peer-focus:text-gray-500 text-gray-500 text-xs left-12 top-2 transition-all">Correo</span>
			</label>
			<label class="text-gray-500 mb-4 relative flex group-await:opacity-50">
				<input class="rounded-xl peer pt-7 pb-2 pl-12 px-4 w-full font-bold" type="tel" placeholder="" maxlength="255" name="phone" required> 
				<span class="absolute peer-placeholder-shown:top-5 peer-placeholder-shown:text-gray-500 peer-placeholder-shown:text-base peer-focus:text-xs peer-focus:top-2 peer-focus:text-gray-500 text-gray-500 text-xs left-12 top-2 transition-all">Teléfono</span>
			</label>
			<div class="grid lg:grid-cols-2 gap-4 lg:gap-2 mb-4">
				<div class="pl-12 pr-8 lg:pr-12  py-4 bg-white rounded-xl flex justify-between items-center gap-8 lg:gap-12">
					<span>Experiencia</span>
	
					<label class="flex items-center gap-2 lg:gap-4 ml-auto">
						<input type="radio" name="experience" value="yes" required on:input={(e)=>{
							let input = document.querySelector('input[name="years"]')
							if (input instanceof HTMLInputElement) {
								input.required = true
								input.disabled = false
							}
						}}/>
						<span>Si</span>
					</label>
					<label class="flex items-center gap-2 lg:gap-4 ">
						<input type="radio" name="experience" value="no" required on:input={(e)=>{
							let input = document.querySelector('input[name="years"]')
							if (input instanceof HTMLInputElement) {
								input.required = false
								input.disabled = true
							}
						}}/>
						<span>No</span>
					</label>
				</div>
				<label class="text-gray-500 relative flex group-await:opacity-50 has-[:disabled]:opacity-50">
					<input class="rounded-xl peer pt-7 pb-2 pl-12 px-4 w-full font-bold" type="number" placeholder="" maxlength="255" name="years"> 
					<span class="absolute peer-placeholder-shown:top-5 peer-placeholder-shown:text-gray-500 peer-placeholder-shown:text-base peer-focus:text-xs peer-focus:top-2 peer-focus:text-gray-500 text-gray-500 text-xs left-12 top-2 transition-all">Años de experiencia</span>
				</label>
			</div>
			<label class="text-gray-500 mb-8 relative flex group-await:opacity-50 hover:cursor-pointer group/file">
				<input on:input={(e)=>{
					let files = e.currentTarget.files
					if (files) {
						let file = files[0]
						curriculumFileName = file.name
					}
				}} class="absolute inset-0 sr-only" type="file" placeholder="" name="curriculum" required accept=".pdf,.doc,.docx,.odt,.jpg,.jpeg,.png,.gif"> 
				<span class="rounded-xl bg-white group-hover/file:bg-gray-100 peer pt-5 pl-44 h-16 px-4 relative w-full font-bold peer whitespace-nowrap overflow-hidden">
				</span>
				{#if curriculumFileName === ""}
					<span class="absolute top-5 text-gray-500 text-base font-normal left-12 transition-all flex items-center gap-2"><span class="icon-[tabler--circle-plus-filled] -my-2 text-3xl"></span> Adjuntar CV</span>
					<span class="absolute top-5 right-12 text-gray-200 hidden lg:block">Se aceptan archivos .pdf, .doc, imagenes, etc.</span>
				{:else}
					<span class="absolute top-5 text-gray-500 text-base font-normal left-12 transition-all flex items-center gap-2"><span class="icon-[tabler--paperclip] -my-2 text-xl"></span> {curriculumFileName}</span>
				{/if}
			</label>
			<button class="ml-auto text-white group/button px-20 py-5 bg-red-500 hover:bg-black rounded-full group-await:bg-black font-semibold *:overflow-hidden flex items-center justify-center mx-auto" type="submit">
				<span class="w-16 group-await:w-0">Enviar</span>
				<span class="group-await:hidden w-0 group-hover/button:w-6 transition-all text-2xl icon-[mdi--chevron-right]"></span>
				<span class="icon-[tabler--loader-2] text-2xl w-0 group-await:w-6 group-await:animate-spin"></span>
			</button>
		</div>        
		<div class="absolute inset-0 opacity-0 translate-x-full group-then:translate-x-0 group-then:opacity-100 rounded-3xl w-full bg-white px-8 py-7 transition-all">
			<h2 class="text-4xl font-bold mb-8 lg:mb-4">Muchas gracias!</h2>
			<p class="text-gray-700">Seras contactado a la brevedad.</p>
			<img class="w-fit h-16 absolute right-8 bottom-8" src="/svg/iso-gray.svg" alt=""/>
		</div>
		<div class="absolute inset-0 opacity-0 -translate-x-full group-catch:translate-x-0 bg-red-300 group-catch:opacity-100 rounded-3xl w-full px-8 py-7 transition-all">
			<h2 class="text-4xl font-bold mb-8 lg:mb-4">Ocurrio un error!</h2>
			<p class="text-gray-700">Temporalmente no funciona este formulario. Porfavor, contactanos por otros medios.</p>
			<img class="w-fit h-16 absolute right-8 bottom-8" src="/svg/iso-gray.svg" alt=""/>
		</div>
    </form>
</section>