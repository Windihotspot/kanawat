<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import AOS from 'aos'
import 'aos/dist/aos.css'

/* ------------------------------------------------------------------
   ASSETS: change these two filenames to match what is in your assets folder
------------------------------------------------------------------- */
import producerImg from '@/assets/olomson.jpeg'
import logoImg from '@/assets/kanawat-logo.jpeg'

/* ------------------------------------------------------------------
   CONTENT
------------------------------------------------------------------- */
const whatsappLink =
  'https://wa.me/2347035931443?text=Hi%20Olomson%2C%20I%27d%20like%20to%20work%20with%20you.%20My%20budget%20is%20'
const email = 'kanawatmedia@gmail.com'

const navLinks = [
  { label: 'About', href: '#about' },
  { label: 'Credits', href: '#credits' },
  { label: 'Services', href: '#services' },
  { label: 'Listen', href: '#listen' },
  { label: 'Contact', href: '#contact' },
]

const credits = [
  { artist: 'Victony', title: 'Outlaw EP', role: 'One of the producers on the EP project', tag: 'EP' },
  { artist: 'Victony', title: 'Jolene', role: 'Producer', tag: 'Single' },
  { artist: 'Brown Joel', title: 'Pose For My Camera', role: 'Producer', tag: 'Single' },
  { artist: 'BabyLov AD', title: 'B&B (Booze & Bumbum)', role: 'Producer', tag: 'Single' },
]

const services = [
  { icon: 'fa-solid fa-drum', title: 'Original Beat Production', text: 'Fresh, custom beats built around your sound, from Afrobeats to world fusion.' },
  { icon: 'fa-solid fa-music', title: 'Full Song Production', text: 'From the first idea to the finished record, produced from scratch with you.' },
  { icon: 'fa-solid fa-sliders', title: 'Mixing & Mastering', text: 'Clean, loud and balanced, finished to Spotify standard and radio ready.' },
  { icon: 'fa-solid fa-microphone-lines', title: 'Vocal Tuning & Arrangement', text: 'Polished vocals and tight arrangements that let the song breathe.' },
]

const stats = [
  { value: 2017, suffix: '', label: 'Producing since' },
  { value: 4, suffix: '+', label: 'Major credits' },
  { value: 2, suffix: 'hrs', label: 'WhatsApp reply time' },
  { value: 80, suffix: 'k', prefix: '₦', label: 'Packages from' },
]

/* Replace the titles with the real track names (IDs are from your Spotify links) */
const tracks = [
  { id: '6OeHAhsVn22nStOm1XRHLQ' }, // Jolene (Victony)
  { id: '3eVFi4PkdBI2BGvSH1ASb8' },
  { id: '3OsXnXDO9gIb0L8t4nHhGy' },
  { id: '6lalrsvL8JRBW5BnC47oiW' },
  { id: '37K3L7YRbLSGiuyH9BcudB' },
]
const spotifyHero = 'https://open.spotify.com/track/6OeHAhsVn22nStOm1XRHLQ'

const marquee = ['Victony', 'Brown Joel', 'BabyLov AD', 'Afrobeats', 'World Fusion', 'Radio Ready', 'Spotify Ready', 'Lagos to the World']

/* ------------------------------------------------------------------
   BEHAVIOUR
------------------------------------------------------------------- */
const scrolled = ref(false)
const menuOpen = ref(false)
const bars = Array.from({ length: 28 }, (_, i) => i)

const onScroll = () => {
  scrolled.value = window.scrollY > 40
}

/* Count-up for stats */
const statRefs = ref([])
const counts = ref(stats.map(() => 0))
let counted = false
const runCounters = () => {
  if (counted) return
  counted = true
  stats.forEach((s, i) => {
    const duration = 1600
    const start = performance.now()
    const from = s.value > 1000 ? s.value - 9 : 0
    const tick = (now) => {
      const p = Math.min((now - start) / duration, 1)
      const eased = 1 - Math.pow(1 - p, 3)
      counts.value[i] = Math.round(from + (s.value - from) * eased)
      if (p < 1) requestAnimationFrame(tick)
    }
    requestAnimationFrame(tick)
  })
}

let observer
onMounted(() => {
  AOS.init({ duration: 900, easing: 'ease-out-cubic', once: true, offset: 80 })
  window.addEventListener('scroll', onScroll, { passive: true })
  onScroll()

  observer = new IntersectionObserver(
    (entries) => entries.forEach((e) => e.isIntersecting && runCounters()),
    { threshold: 0.4 }
  )
  const el = document.getElementById('stats')
  if (el) observer.observe(el)
})

onBeforeUnmount(() => {
  window.removeEventListener('scroll', onScroll)
  observer && observer.disconnect()
})
</script>

<template>
  <div class="relative min-h-screen overflow-x-hidden bg-[#050505] text-[#f3ead2] font-sans selection:bg-[#d4af37] selection:text-black">
    <!-- Ambient glow -->
    <div class="pointer-events-none fixed inset-0 -z-0">
      <div class="glow absolute -top-40 -left-40 h-[32rem] w-[32rem] rounded-full bg-[#d4af37]/10 blur-[120px]"></div>
      <div class="glow-delay absolute top-1/3 -right-40 h-[30rem] w-[30rem] rounded-full bg-[#b8860b]/10 blur-[120px]"></div>
    </div>

    <!-- NAVBAR -->
    <header
      class="fixed inset-x-0 top-0 z-50 transition-all duration-500"
      :class="scrolled ? 'bg-black/70 backdrop-blur-xl border-b border-[#d4af37]/15 py-2' : 'py-4'"
    >
      <nav class="mx-auto flex max-w-7xl items-center justify-between px-5 md:px-8">
        <a href="#top" class="flex items-center gap-3">
          <img :src="logoImg" alt="Kanawat logo" class="h-11 w-11 rounded-md object-cover ring-1 ring-[#d4af37]/40" />
          <span class="gold-text text-lg font-semibold tracking-[0.35em]">KANAWAT</span>
        </a>

        <ul class="hidden items-center gap-9 md:flex">
          <li v-for="l in navLinks" :key="l.href">
            <a :href="l.href" class="nav-link text-sm uppercase tracking-[0.2em] text-[#e9dcb5]/80 hover:text-[#f5d77a] transition-colors">
              {{ l.label }}
            </a>
          </li>
        </ul>

        <a :href="whatsappLink" target="_blank" class="btn-gold hidden md:inline-flex">
          <i class="fa-brands fa-whatsapp"></i> Book Now
        </a>

        <button class="md:hidden text-2xl text-[#f5d77a]" @click="menuOpen = !menuOpen" aria-label="Menu">
          <i :class="menuOpen ? 'fa-solid fa-xmark' : 'fa-solid fa-bars'"></i>
        </button>
      </nav>

      <transition name="drop">
        <div v-if="menuOpen" class="md:hidden mx-4 mt-3 rounded-2xl border border-[#d4af37]/20 bg-black/90 p-6 backdrop-blur-xl">
          <ul class="flex flex-col gap-5">
            <li v-for="l in navLinks" :key="l.href">
              <a :href="l.href" @click="menuOpen = false" class="text-sm uppercase tracking-[0.25em] text-[#e9dcb5]">{{ l.label }}</a>
            </li>
            <li>
              <a :href="whatsappLink" target="_blank" class="btn-gold w-full justify-center">
                <i class="fa-brands fa-whatsapp"></i> Book on WhatsApp
              </a>
            </li>
          </ul>
        </div>
      </transition>
    </header>

    <main id="top" class="relative z-10">
      <!-- HERO -->
      <section class="relative flex min-h-screen items-center pt-28 pb-20">
        <div class="mx-auto grid max-w-7xl items-center gap-14 px-5 md:px-8 lg:grid-cols-2">
          <div>
            <p data-aos="fade-down" class="mb-5 inline-flex items-center gap-2 rounded-full border border-[#d4af37]/30 bg-[#d4af37]/5 px-4 py-1.5 text-xs uppercase tracking-[0.3em] text-[#f5d77a]">
              <span class="h-1.5 w-1.5 animate-ping rounded-full bg-[#f5d77a]"></span>
              Producer · Sound Engineer · Founder
            </p>

            <h1 data-aos="fade-up" data-aos-delay="100" class="text-6xl font-bold leading-[0.95] sm:text-7xl lg:text-8xl">
              <span class="gold-text shimmer">OLOMSON</span>
            </h1>

            <p data-aos="fade-up" data-aos-delay="250" class="mt-7 max-w-xl text-xl leading-relaxed text-[#f3ead2]/85 md:text-2xl">
              I make hits, not just beats. <span class="text-[#f5d77a]">Afrobeats to World Fusion</span>, gold standard sound from Lagos to the world.
            </p>

            <div data-aos="fade-up" data-aos-delay="400" class="mt-9 flex flex-wrap gap-4">
              <a :href="spotifyHero" target="_blank" class="btn-gold">
                <i class="fa-brands fa-spotify"></i> Listen on Spotify
              </a>
              <a :href="whatsappLink" target="_blank" class="btn-outline">
                <i class="fa-brands fa-whatsapp"></i> Book on WhatsApp
              </a>
            </div>

            <div data-aos="fade-up" data-aos-delay="550" class="mt-12 flex items-center gap-4">
              <div class="flex h-10 items-end gap-[3px]">
                <span v-for="b in bars" :key="b" class="eq-bar" :style="{ animationDelay: b * 0.07 + 's' }"></span>
              </div>
              <p class="text-xs uppercase tracking-[0.25em] text-[#e9dcb5]/60">Credits: Victony · Brown Joel · BabyLov AD</p>
            </div>
          </div>

          <!-- Photo -->
          <div data-aos="zoom-in" data-aos-delay="200" class="relative mx-auto w-full max-w-md lg:max-w-lg">
            <div class="ring-spin absolute -inset-4 rounded-[2rem] opacity-70"></div>
            <div class="relative overflow-hidden rounded-[1.75rem] border border-[#d4af37]/40 bg-black shadow-[0_0_80px_-15px_rgba(212,175,55,0.45)]">
              <img :src="producerImg" alt="Olomson, producer and founder of Kanawat" class="float-img aspect-[4/5] w-full object-cover" />
              <div class="absolute inset-0 bg-gradient-to-t from-black/60 via-transparent to-transparent"></div>
              <div class="absolute bottom-5 left-5 right-5 flex items-center justify-between rounded-xl border border-[#d4af37]/30 bg-black/55 px-4 py-3 backdrop-blur-md">
                <div>
                  <p class="text-sm font-semibold text-[#f5d77a]">Olomson</p>
                  <p class="text-xs text-[#e9dcb5]/70">Founder, Kanawat</p>
                </div>
                <i class="fa-solid fa-circle-play text-2xl text-[#f5d77a]"></i>
              </div>
            </div>
          </div>
        </div>

        <a href="#about" class="absolute bottom-6 left-1/2 hidden -translate-x-1/2 text-[#f5d77a]/70 md:block">
          <i class="fa-solid fa-chevron-down animate-bounce"></i>
        </a>
      </section>

      <!-- MARQUEE -->
      <section class="border-y border-[#d4af37]/15 bg-black/60 py-5">
        <div class="marquee">
          <div class="marquee-track">
            <template v-for="n in 2" :key="n">
              <span v-for="m in marquee" :key="m + n" class="mx-8 inline-flex items-center gap-8 text-sm uppercase tracking-[0.35em] text-[#e9dcb5]/70">
                {{ m }} <i class="fa-solid fa-diamond text-[8px] text-[#d4af37]"></i>
              </span>
            </template>
          </div>
        </div>
      </section>

      <!-- ABOUT -->
      <section id="about" class="mx-auto max-w-7xl px-5 py-28 md:px-8">
        <div class="grid items-center gap-14 lg:grid-cols-5">
          <div class="lg:col-span-2" data-aos="fade-right">
            <p class="eyebrow">About</p>
            <h2 class="section-title">Sound with a <span class="gold-text">gold standard</span>.</h2>
            <div class="mt-6 h-px w-24 bg-gradient-to-r from-[#d4af37] to-transparent"></div>
          </div>

          <div class="space-y-6 text-lg leading-relaxed text-[#f3ead2]/80 lg:col-span-3" data-aos="fade-left" data-aos-delay="150">
            <p>
              I am <span class="text-[#f5d77a]">Olomson</span>, Saidi Olomo Olawumi, from Ikare Akoko, Ondo State, and a graduate of English from Adekunle Ajasin University.
            </p>
            <p>
              I started professional music production in 2017. My sound is a confluence of all genres, and every original entertainer inspires me.
            </p>
            <p>
              I am the founder of <span class="text-[#f5d77a]">Kanawat</span>, an independent production house based in Lagos, working with artists worldwide, in person and remotely. My work is built on one promise: radio-ready, Spotify-ready sound at an honest price.
            </p>
          </div>
        </div>

        <!-- Stats -->
        <div id="stats" class="mt-20 grid grid-cols-2 gap-5 md:grid-cols-4">
          <div
            v-for="(s, i) in stats"
            :key="s.label"
            data-aos="fade-up"
            :data-aos-delay="i * 120"
            class="card-gold text-center"
          >
            <p class="gold-text text-4xl font-bold md:text-5xl">{{ s.prefix || '' }}{{ counts[i] }}{{ s.suffix }}</p>
            <p class="mt-2 text-xs uppercase tracking-[0.25em] text-[#e9dcb5]/60">{{ s.label }}</p>
          </div>
        </div>
      </section>

      <!-- CREDITS -->
      <section id="credits" class="relative border-y border-[#d4af37]/10 bg-gradient-to-b from-black via-[#0b0905] to-black py-28">
        <div class="mx-auto max-w-7xl px-5 md:px-8">
          <div class="mb-14 text-center" data-aos="fade-up">
            <p class="eyebrow">Selected Credits</p>
            <h2 class="section-title">Records that <span class="gold-text">moved the culture</span>.</h2>
            <p class="mx-auto mt-4 max-w-xl text-[#e9dcb5]/60">All releases are available on Spotify, Apple Music, Boomplay and all streaming platforms.</p>
          </div>

          <div class="grid gap-6 sm:grid-cols-2">
            <article
              v-for="(c, i) in credits"
              :key="c.title"
              :data-aos="i % 2 === 0 ? 'fade-right' : 'fade-left'"
              :data-aos-delay="i * 100"
              class="credit group relative overflow-hidden rounded-2xl border border-[#d4af37]/20 bg-[#0c0a06] p-7 transition-all duration-500 hover:-translate-y-1.5 hover:border-[#d4af37]/60"
            >
              <span class="absolute -right-4 -top-6 select-none text-[9rem] font-black leading-none text-[#d4af37]/[0.06] transition-all duration-500 group-hover:text-[#d4af37]/[0.12]">
                0{{ i + 1 }}
              </span>
              <div class="relative">
                <span class="rounded-full border border-[#d4af37]/40 px-3 py-1 text-[10px] uppercase tracking-[0.3em] text-[#f5d77a]">{{ c.tag }}</span>
                <h3 class="mt-5 text-2xl font-semibold text-[#f3ead2] md:text-3xl">{{ c.title }}</h3>
                <p class="gold-text mt-1 text-lg font-medium">{{ c.artist }}</p>
                <p class="mt-4 flex items-center gap-2 text-sm text-[#e9dcb5]/60">
                  <i class="fa-solid fa-sliders text-[#d4af37]"></i> {{ c.role }}
                </p>
              </div>
            </article>
          </div>
        </div>
      </section>

      <!-- SERVICES -->
      <section id="services" class="mx-auto max-w-7xl px-5 py-28 md:px-8">
        <div class="mb-14" data-aos="fade-up">
          <p class="eyebrow">What I Do</p>
          <h2 class="section-title">I produce, mix <span class="gold-text">&amp; master</span>.</h2>
        </div>

        <div class="grid gap-6 sm:grid-cols-2 lg:grid-cols-4">
          <div
            v-for="(s, i) in services"
            :key="s.title"
            data-aos="flip-up"
            :data-aos-delay="i * 120"
            class="card-gold group"
          >
            <div class="mb-6 flex h-14 w-14 items-center justify-center rounded-xl bg-gradient-to-br from-[#f5d77a] to-[#b8860b] text-xl text-black transition-transform duration-500 group-hover:rotate-[10deg] group-hover:scale-110">
              <i :class="s.icon"></i>
            </div>
            <h3 class="text-xl font-semibold text-[#f3ead2]">{{ s.title }}</h3>
            <p class="mt-3 text-sm leading-relaxed text-[#e9dcb5]/65">{{ s.text }}</p>
          </div>
        </div>

        <div data-aos="zoom-in" class="mt-12 flex flex-col items-center justify-between gap-6 rounded-2xl border border-[#d4af37]/30 bg-gradient-to-r from-[#d4af37]/10 via-transparent to-[#d4af37]/10 p-8 text-center md:flex-row md:text-left">
          <div>
            <p class="text-sm uppercase tracking-[0.3em] text-[#f5d77a]">Packages from</p>
            <p class="gold-text text-4xl font-bold md:text-5xl">₦80,000</p>
          </div>
          <p class="max-w-md text-[#e9dcb5]/75">WhatsApp me with your budget, let's cook.</p>
          <a :href="whatsappLink" target="_blank" class="btn-gold"><i class="fa-brands fa-whatsapp"></i> Start a Project</a>
        </div>
      </section>

      <!-- LISTEN -->
      <section id="listen" class="border-t border-[#d4af37]/10 bg-gradient-to-b from-black to-[#0b0905] py-28">
        <div class="mx-auto max-w-7xl px-5 md:px-8">
          <div class="mb-14 text-center" data-aos="fade-up">
            <p class="eyebrow">Listen</p>
            <h2 class="section-title">Hear the <span class="gold-text">work</span>.</h2>
          </div>

          <div class="grid gap-6 md:grid-cols-2 lg:grid-cols-3">
            <div
              v-for="(t, i) in tracks"
              :key="t.id"
              data-aos="fade-up"
              :data-aos-delay="(i % 3) * 120"
              class="overflow-hidden rounded-2xl border border-[#d4af37]/20 bg-black p-2 transition-all duration-500 hover:border-[#d4af37]/60 hover:shadow-[0_0_40px_-10px_rgba(212,175,55,0.5)]"
            >
              <iframe
                :src="`https://open.spotify.com/embed/track/${t.id}?theme=0`"
                width="100%"
                height="152"
                frameborder="0"
                allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture"
                loading="lazy"
                class="rounded-xl"
              ></iframe>
            </div>
          </div>
        </div>
      </section>

      <!-- CONTACT -->
      <section id="contact" class="relative mx-auto max-w-5xl px-5 py-28 text-center md:px-8">
        <div class="pointer-events-none absolute left-1/2 top-1/2 h-72 w-72 -translate-x-1/2 -translate-y-1/2 rounded-full bg-[#d4af37]/15 blur-[110px]"></div>

        <div class="relative">
          <p data-aos="fade-up" class="eyebrow">Contact</p>
          <h2 data-aos="fade-up" data-aos-delay="100" class="section-title">Ready to make <span class="gold-text">your hit</span>?</h2>
          <p data-aos="fade-up" data-aos-delay="200" class="mx-auto mt-5 max-w-xl text-[#e9dcb5]/70">
            Lagos, Nigeria, with remote sessions worldwide. I reply within 2 hours on WhatsApp.
          </p>

          <div class="mt-12 grid gap-5 sm:grid-cols-3">
            <a :href="whatsappLink" target="_blank" data-aos="zoom-in" class="card-gold group block">
              <i class="fa-brands fa-whatsapp mb-4 text-3xl text-[#f5d77a] transition-transform group-hover:scale-125"></i>
              <p class="text-xs uppercase tracking-[0.25em] text-[#e9dcb5]/60">WhatsApp</p>
              <p class="mt-1 font-medium">07035931443</p>
            </a>
            <a :href="`mailto:${email}`" data-aos="zoom-in" data-aos-delay="120" class="card-gold group block">
              <i class="fa-solid fa-envelope mb-4 text-3xl text-[#f5d77a] transition-transform group-hover:scale-125"></i>
              <p class="text-xs uppercase tracking-[0.25em] text-[#e9dcb5]/60">Email</p>
              <p class="mt-1 break-all text-sm font-medium">{{ email }}</p>
            </a>
            <div data-aos="zoom-in" data-aos-delay="240" class="card-gold group">
              <i class="fa-solid fa-location-dot mb-4 text-3xl text-[#f5d77a] transition-transform group-hover:scale-125"></i>
              <p class="text-xs uppercase tracking-[0.25em] text-[#e9dcb5]/60">Location</p>
              <p class="mt-1 font-medium">Lagos, Nigeria</p>
            </div>
          </div>

          <a :href="whatsappLink" target="_blank" data-aos="fade-up" class="btn-gold pulse mt-12 !px-10 !py-4 text-base">
            <i class="fa-brands fa-whatsapp"></i> Let's Cook
          </a>
        </div>
      </section>
    </main>

    <!-- FOOTER -->
    <footer class="relative z-10 border-t border-[#d4af37]/15 bg-black py-10">
      <div class="mx-auto flex max-w-7xl flex-col items-center justify-between gap-5 px-5 md:flex-row md:px-8">
        <div class="flex items-center gap-3">
          <img :src="logoImg" alt="Kanawat" class="h-9 w-9 rounded object-cover" />
          <span class="gold-text tracking-[0.35em]">KANAWAT</span>
        </div>
        <p class="text-center text-xs text-[#e9dcb5]/50">
          © 2026 Olomson — Kanawat | Production Credits: Victony, Brown Joel, BabyLov AD, Spotify
        </p>
        <div class="flex gap-5 text-xl text-[#f5d77a]">
          <a :href="spotifyHero" target="_blank" class="transition-transform hover:-translate-y-1"><i class="fa-brands fa-spotify"></i></a>
          <a :href="whatsappLink" target="_blank" class="transition-transform hover:-translate-y-1"><i class="fa-brands fa-whatsapp"></i></a>
          <a :href="`mailto:${email}`" class="transition-transform hover:-translate-y-1"><i class="fa-solid fa-envelope"></i></a>
        </div>
      </div>
    </footer>
  </div>
</template>

<style scoped>
/* Gold palette taken from the Kanawat logo */
.gold-text {
  background: linear-gradient(100deg, #9c7a1c 0%, #f5d77a 35%, #d4af37 55%, #f9e8a6 75%, #b8860b 100%);
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
  color: transparent;
}
.shimmer {
  background-size: 250% auto;
  animation: shimmer 5s linear infinite;
}
@keyframes shimmer {
  to { background-position: 250% center; }
}

.eyebrow {
  margin-bottom: 0.9rem;
  font-size: 0.75rem;
  letter-spacing: 0.4em;
  text-transform: uppercase;
  color: #d4af37;
}
.section-title {
  font-size: clamp(2rem, 5vw, 3.5rem);
  font-weight: 700;
  line-height: 1.1;
}

/* Buttons */
.btn-gold {
  display: inline-flex;
  align-items: center;
  gap: 0.6rem;
  padding: 0.75rem 1.6rem;
  border-radius: 9999px;
  font-size: 0.85rem;
  font-weight: 600;
  letter-spacing: 0.08em;
  color: #0a0a0a;
  background: linear-gradient(100deg, #b8860b, #f5d77a 50%, #d4af37);
  background-size: 200% auto;
  transition: background-position 0.5s, transform 0.3s, box-shadow 0.3s;
  box-shadow: 0 8px 30px -8px rgba(212, 175, 55, 0.6);
}
.btn-gold:hover {
  background-position: right center;
  transform: translateY(-3px);
  box-shadow: 0 14px 40px -8px rgba(245, 215, 122, 0.7);
}
.btn-outline {
  display: inline-flex;
  align-items: center;
  gap: 0.6rem;
  padding: 0.75rem 1.6rem;
  border-radius: 9999px;
  font-size: 0.85rem;
  font-weight: 600;
  letter-spacing: 0.08em;
  color: #f5d77a;
  border: 1px solid rgba(212, 175, 55, 0.5);
  transition: all 0.3s;
}
.btn-outline:hover {
  background: rgba(212, 175, 55, 0.12);
  transform: translateY(-3px);
}
.pulse {
  animation: pulse 2.4s infinite;
}
@keyframes pulse {
  0% { box-shadow: 0 0 0 0 rgba(212, 175, 55, 0.55); }
  70% { box-shadow: 0 0 0 22px rgba(212, 175, 55, 0); }
  100% { box-shadow: 0 0 0 0 rgba(212, 175, 55, 0); }
}

/* Cards */
.card-gold {
  position: relative;
  border-radius: 1rem;
  border: 1px solid rgba(212, 175, 55, 0.2);
  background: linear-gradient(160deg, #0f0c06, #060606);
  padding: 1.75rem;
  transition: transform 0.5s, border-color 0.5s, box-shadow 0.5s;
}
.card-gold:hover {
  transform: translateY(-8px);
  border-color: rgba(245, 215, 122, 0.6);
  box-shadow: 0 20px 50px -20px rgba(212, 175, 55, 0.5);
}

/* Nav underline */
.nav-link {
  position: relative;
}
.nav-link::after {
  content: '';
  position: absolute;
  left: 0;
  bottom: -6px;
  height: 1px;
  width: 0;
  background: #d4af37;
  transition: width 0.35s;
}
.nav-link:hover::after {
  width: 100%;
}

/* Equalizer bars */
.eq-bar {
  display: block;
  width: 3px;
  height: 100%;
  border-radius: 2px;
  background: linear-gradient(to top, #b8860b, #f5d77a);
  transform-origin: bottom;
  animation: eq 1.1s ease-in-out infinite;
}
@keyframes eq {
  0%, 100% { transform: scaleY(0.15); }
  50% { transform: scaleY(1); }
}

/* Hero photo */
.float-img {
  animation: floaty 7s ease-in-out infinite;
}
@keyframes floaty {
  0%, 100% { transform: scale(1.04) translateY(0); }
  50% { transform: scale(1.04) translateY(-8px); }
}
.ring-spin {
  background: conic-gradient(from 0deg, transparent, #d4af37, transparent 40%, #f5d77a, transparent 80%);
  filter: blur(18px);
  animation: spin 9s linear infinite;
}
@keyframes spin {
  to { transform: rotate(360deg); }
}

/* Ambient glows */
.glow { animation: drift 14s ease-in-out infinite; }
.glow-delay { animation: drift 18s ease-in-out infinite reverse; }
@keyframes drift {
  0%, 100% { transform: translate(0, 0); }
  50% { transform: translate(40px, 30px); }
}

/* Marquee */
.marquee {
  overflow: hidden;
  white-space: nowrap;
  mask-image: linear-gradient(to right, transparent, #000 12%, #000 88%, transparent);
}
.marquee-track {
  display: inline-block;
  animation: marquee 32s linear infinite;
}
@keyframes marquee {
  to { transform: translateX(-50%); }
}

/* Mobile menu transition */
.drop-enter-active,
.drop-leave-active {
  transition: all 0.35s ease;
}
.drop-enter-from,
.drop-leave-to {
  opacity: 0;
  transform: translateY(-12px);
}

@media (prefers-reduced-motion: reduce) {
  .eq-bar, .float-img, .ring-spin, .glow, .glow-delay, .marquee-track, .shimmer, .pulse {
    animation: none;
  }
}
</style>