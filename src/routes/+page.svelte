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

<section class="bg-[url(/hero-1.png)] bg-cover backdrop-brightness-50 min-h-[100svh] text-white grid place-content-center text-center relative">
	<div class="drop-shadow-2xl">
		<h1 class="text-5xl lg:text-6xl font-bold mb-4 mt-24">Convertite en un Krak.</h1>
		<p class="mb-8">Unite a nosotros y transformá tu carrera profesional.</p>
		<a href="#contacto" class="block bg-red-500 hover:bg-black text-white uppercase rounded-full w-fit mx-auto py-6 px-14 font-bold text-sm"> Quiero empezar </a>
	</div>
	<a href="#banner-1" class="absolute bottom-8 right-8 flex flex-col items-center gap-4">
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
	<div class="max-w-4xl w-full mx-auto rounded-3xl aspect-video bg-black drop-shadow-2xl shadow-2xl mb-12"></div>
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