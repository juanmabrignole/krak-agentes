<script lang="ts">
    import "../app.css"
    import SEO from 'svelte-seo'

    async function handleNewsLetter(e: SubmitEvent & {
        currentTarget: EventTarget & HTMLFormElement;
    }) {
        let target = e.target
        if (!(target instanceof HTMLFormElement)) return;
        let form = Object.fromEntries(new FormData(target))
        let body = JSON.stringify(form)
        target.classList.add('await')
        try {
            let res = await fetch('https://n8n-krak.com/webhook/newsletter', { method: 'POST', body })
            console.log(res)
            if (res.ok) {
                target.classList.add('then')
                target.classList.remove('await')
            } else throw new Error()
        } catch(_) {
            console.log(_)
            target.classList.add('catch')
            target.classList.remove('await')
        }
    }
</script>

<SEO
title="Agentes | Krak Real Estate"
description="Comercializamos bienes raíces de una manera diferente."
canonical="https://www.krak.com.ar/"
keywords="real estate, marketing"
openGraph={{
  title: "Agentes | Krak Real Estate",
  description: "Comercializamos bienes raíces de una manera diferente.",
  //@ts-ignore
  image: "https://www.krak.com.ar/og.jpg",
  url: "https://www.krak.com.ar/",
  type: "website",
  site_name: "Krak Real Estate",
}}

/>
<svelte:head>
    <script src="https://www.google.com/recaptcha/api.js?render=6LcE9jsqAAAAAOyzMhfoTOA-G0kI6kCxadXeFwME"></script>

    <link rel="preload" href="/fonts/Montserrat-Light.woff2" as="font" type="font/woff2" crossorigin>
    <link rel="preload" href="/fonts/Montserrat-Regular.woff2" as="font" type="font/woff2" crossorigin>
    <link rel="preload" href="/fonts/Montserrat-SemiBold.woff2" as="font" type="font/woff2" crossorigin>
    <link rel="preload" href="/fonts/Montserrat-Bold.woff2" as="font" type="font/woff2" crossorigin>

    <link rel="preload" href="/fonts/Inter-Black.woff2" as="font" type="font/woff2" crossorigin>
    <link rel="preload" href="/fonts/Inter-SemiBold.woff2" as="font" type="font/woff2" crossorigin>
    <link rel="preload" href="/fonts/Inter-Bold.woff2" as="font" type="font/woff2" crossorigin>
    <link rel="preload" href="/fonts/Inter-Medium.woff2" as="font" type="font/woff2" crossorigin>
    <link rel="preload" href="/fonts/Inter-Regular.woff2" as="font" type="font/woff2" crossorigin>
</svelte:head>

<header class="max-w-6xl mx-auto w-full px-4 h-24 sm:h-36 flex items-center justify-between *:z-10 -mb-24 sm:-mb-36">
    <img src="/svg/iso-white.svg" class="h-16 object-contain" alt="" />
    <div class="flex items-center text-2xl gap-3 text-white">
        <a href="https://www.instagram.com/krak.realestate/" class="hover:text-red-300 icon-[mdi--instagram]"> </a>
        <a href="https://www.tiktok.com/@krak.realestate" class="hover:text-red-300 icon-[ic--baseline-tiktok]"> </a>
        <a href="https://www.linkedin.com/company/krakre/" class="hover:text-red-300 icon-[mdi--linkedin]"> </a>
        <a href="https://www.youtube.com/channel/UCx5MEQ0uTFHNaaoGOVGFFSw" class="hover:text-red-300 icon-[mdi--youtube]"> </a>
    </div>
</header>

<main class="min-h-[calc(100svh-5rem)] flex flex-col font-inter overflow-x-hidden">
    <slot />
</main>

<footer class="bg-gray-500 text-white py-16">
    <div class="max-w-6xl mx-auto w-full px-4 grid sm:grid-cols-2 md:grid-cols-3 gap-16 sm:gap-8 md:gap-4">
        <div class="flex flex-col">
            <img src="/svg/imago-white.svg" class="h-16 w-fit mb-8" alt=""/>
            <h2 class="text-sm mb-2 font-semibold">Marcelo Napolitano</h2>
            <p class="text-gray-100 text-sm">
                <b class="text-xs">CMCPSI:</b> 6645. <br>
                <b class="text-xs">CUCICBA:</b> 5456
            </p>
        </div>
        <div>
            <h2 class="uppercase text-sm mb-8">Comunicate ahora mismo</h2>
            <a class="flex items-center text-lg gap-2 hover:text-red-300 mb-2 font-semibold tracking-wider" href="tel:+541138882095"><span class="icon-[mdi--phone] text-2xl"></span><span>11 3888-2095</span></a>
            <a class="flex items-center text-lg gap-2 hover:text-red-300 mb-3 font-semibold tracking-wider" href="mailto:info@krak.com.ar"><span class="icon-[mdi--email] text-2xl"></span><span>info@krak.com.ar</span></a>
            <div class="flex items-center text-2xl gap-3">
                <a href="https://www.instagram.com/krak.realestate/" class="hover:text-red-300 icon-[mdi--instagram]"> </a>
                <a href="https://www.tiktok.com/@krak.realestate" class="hover:text-red-300 icon-[ic--baseline-tiktok]"> </a>
                <a href="https://www.linkedin.com/company/krakre/" class="hover:text-red-300 icon-[mdi--linkedin]"> </a>
                <a href="https://www.youtube.com/channel/UCx5MEQ0uTFHNaaoGOVGFFSw" class="hover:text-red-300 icon-[mdi--youtube]"> </a>
            </div>
        </div>
        <form class="group flex flex-col sm:col-start-2 md:col-start-auto sm:mt-8 md:mt-0" on:submit|preventDefault={handleNewsLetter}> 
            <h2 class="uppercase text-sm mb-8">Suscribite al newsletter</h2>
            <div class="block group-then:hidden">
                <label class="text-gray-500 mb-2 relative flex group-await:opacity-50">
                    <input class="rounded-lg peer pt-5 pb-1 px-4 w-full font-semibold" type="email" placeholder=" " maxlength="255" name="email" required> 
                    <span class="absolute peer-placeholder-shown:top-3.5 peer-placeholder-shown:text-gray-500 peer-placeholder-shown:text-base peer-focus:text-xs peer-focus:top-1.5 peer-focus:text-gray-500 text-gray-500 text-xs left-4 top-1.5 transition-all">Email</span>
                </label>
                <button class="ml-auto text-black group/button px-4 py-3 bg-white hover:bg-red-300 rounded-lg group-await:bg-red-300 font-semibold *:overflow-hidden flex items-center justify-center" type="submit">
                    <span class="w-16 group-await:w-0">Enviar</span>
                    <span class="group-await:hidden w-0 group-hover/button:w-6 transition-all text-2xl icon-[mdi--chevron-right]"></span>
                    <span class="icon-[tabler--loader-2] text-2xl w-0 group-await:w-6 group-await:animate-spin"></span>
                </button>
            </div>
            <div class="hidden group-then:block">
                <h2 class="font-bold uppercase mb-2">Muchas gracias</h2>
                <p class="text-gray-200 leading-snug">Nos alegra que quieras saber mas de nosotros.</p>
            </div>
        </form>
    </div>
</footer>