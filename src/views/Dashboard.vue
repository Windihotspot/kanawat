<script setup>
import { ref, reactive } from 'vue'

/* ---------------------------------------------
   Dummy data — swap for real API/store data later
--------------------------------------------- */
const producers = reactive([
  { id: 1, name: 'Kelvin Wave', initials: 'KW', color: '#0F9D58' },
  { id: 2, name: 'DJ Bootsy', initials: 'DB', color: '#1B7F4C' },
  { id: 3, name: 'Nova Keys', initials: 'NK', color: '#2F9E68' },
  { id: 4, name: 'Ray Static', initials: 'RS', color: '#0C6B3B' },
  { id: 5, name: 'Mella B', initials: 'MB', color: '#3CB878' },
])

const hero = reactive({
  title: 'Midnight Drive',
  producer: 'Kelvin Wave',
  plays: '482K plays',
  price: '$29.99',
  isFavorite: false,
})

const topBeats = reactive([
  { id: 1, title: 'Neon Nights', producer: 'Kelvin Wave', duration: '3:12', price: '$24.99', favorite: true },
  { id: 2, title: 'Concrete Bloom', producer: 'Nova Keys', duration: '2:54', price: '$19.99', favorite: false },
  { id: 3, title: 'Static Motion', producer: 'Ray Static', duration: '3:40', price: '$34.99', favorite: false },
  { id: 4, title: 'Low Tide', producer: 'Mella B', duration: '2:38', price: '$22.00', favorite: false },
  { id: 5, title: 'Backroom Talk', producer: 'DJ Bootsy', duration: '3:05', price: '$27.50', favorite: true },
])

const library = reactive([
  { id: 1, title: 'Neon Nights', kind: 'Purchased', producer: 'Kelvin Wave' },
  { id: 2, title: 'Chill Pack Vol. 2', kind: 'Playlist', producer: 'by you' },
  { id: 3, title: 'Static Motion', kind: 'Purchased', producer: 'Ray Static' },
  { id: 4, title: 'Trap Essentials', kind: 'Playlist', producer: 'by you' },
])

const activeMenu = ref('Discover')
const activeLibraryTab = ref('Purchased')
const isPlaying = ref(true)
const progress = ref(38) // percent

function togglePlay() {
  isPlaying.value = !isPlaying.value
}

function toggleHeroFavorite() {
  hero.isFavorite = !hero.isFavorite
}

function toggleBeatFavorite(beat) {
  beat.favorite = !beat.favorite
}
</script>

<template>
  <div class="gb-shell min-h-screen w-full bg-white text-[#12201A] flex">
    <!-- ============ LEFT SIDEBAR ============ -->
    <aside class="hidden lg:flex flex-col w-60 shrink-0 border-r border-[#EDF2EF] px-5 py-6">
      <div class="flex items-center gap-2 mb-9">
        <div class="h-8 w-8 rounded-full bg-[#0F9D58] flex items-center justify-center">
          <i class="fa-solid fa-seedling text-white text-sm"></i>
        </div>
        <span class="gb-heading text-[17px] font-bold tracking-tight">Kanawat</span>
      </div>

      <p class="text-[11px] font-semibold text-[#9AA79E] mb-3 px-1">Menu</p>
      <nav class="flex flex-col gap-1 mb-8">
        <button
          v-for="item in ['Discover', 'Genres', 'Producers', 'Charts']"
          :key="item"
          @click="activeMenu = item"
          class="flex items-center gap-3 px-3 py-2 rounded-xl text-[14px] transition-colors"
          :class="activeMenu === item
            ? 'bg-[#EAF7EF] text-[#0F9D58] font-semibold'
            : 'text-[#5B6862] hover:bg-[#F5F9F6]'"
        >
          <i
            class="mdi text-[18px]"
            :class="{
              'mdi-compass-outline': item === 'Discover',
              'mdi-shape-outline': item === 'Genres',
              'mdi-account-music-outline': item === 'Producers',
              'mdi-chart-line': item === 'Charts',
            }"
          ></i>
          {{ item }}
        </button>
      </nav>

      <p class="text-[11px] font-semibold text-[#9AA79E] mb-3 px-1">Library</p>
      <nav class="flex flex-col gap-1 mb-8">
        <button
          v-for="item in ['Purchases', 'Playlists', 'Favorites', 'Downloads']"
          :key="item"
          class="flex items-center gap-3 px-3 py-2 rounded-xl text-[14px] text-[#5B6862] hover:bg-[#F5F9F6] transition-colors"
        >
          <i
            class="mdi text-[18px]"
            :class="{
              'mdi-bag-checked': item === 'Purchases',
              'mdi-playlist-music-outline': item === 'Playlists',
              'mdi-heart-outline': item === 'Favorites',
              'mdi-tray-arrow-down': item === 'Downloads',
            }"
          ></i>
          {{ item }}
        </button>
      </nav>

      <p class="text-[11px] font-semibold text-[#9AA79E] mb-3 px-1">Settings</p>
      <nav class="flex flex-col gap-1">
        <button class="flex items-center gap-3 px-3 py-2 rounded-xl text-[14px] text-[#5B6862] hover:bg-[#F5F9F6] transition-colors">
          <i class="mdi mdi-account-outline text-[18px]"></i>
          Account
        </button>
        <button class="flex items-center gap-3 px-3 py-2 rounded-xl text-[14px] text-[#5B6862] hover:bg-[#F5F9F6] transition-colors">
          <i class="mdi mdi-logout text-[18px]"></i>
          Log out
        </button>
      </nav>

      <div class="mt-auto pt-6">
        <div class="rounded-2xl bg-[#0F1B14] p-4 text-white">
          <div class="flex items-center gap-2 mb-3">
            <i class="fa-solid fa-microphone-lines text-[#4ADE80]"></i>
            <span class="text-[13px] font-semibold">Producer Hub</span>
          </div>
          <p class="text-[12px] text-[#B7C4BC] leading-relaxed mb-3">
            Upload your beats and start collecting sales today.
          </p>
          <button class="w-full bg-[#0F9D58] hover:bg-[#0D8A4C] transition-colors text-white text-[13px] font-semibold py-2 rounded-lg">
            Become a producer
          </button>
        </div>
      </div>
    </aside>

    <!-- ============ MAIN CONTENT ============ -->
    <main class="flex-1 px-6 lg:px-9 py-6 lg:py-8 min-w-0">
      <!-- top bar -->
      <div class="flex items-center justify-between mb-8">
        <div class="flex items-center gap-2 lg:hidden">
          <div class="h-7 w-7 rounded-full bg-[#0F9D58] flex items-center justify-center">
            <i class="fa-solid fa-seedling text-white text-xs"></i>
          </div>
          <span class="gb-heading text-[15px] font-bold">Kanawat</span>
        </div>
        <nav class="hidden md:flex items-center gap-7 text-[14px] text-[#5B6862]">
          <a href="#" class="text-[#0F9D58] font-semibold">Beats</a>
          <a href="#" class="hover:text-[#12201A]">Genres</a>
          <a href="#" class="hover:text-[#12201A]">Charts</a>
          <a href="#" class="hover:text-[#12201A]">Producers</a>
        </nav>
        <div class="flex items-center gap-4">
          <button class="h-9 w-9 rounded-full hover:bg-[#F5F9F6] flex items-center justify-center text-[#5B6862]">
            <i class="mdi mdi-cart-outline text-[20px]"></i>
          </button>
          <button class="h-9 w-9 rounded-full hover:bg-[#F5F9F6] flex items-center justify-center text-[#5B6862]">
            <i class="mdi mdi-bell-outline text-[20px]"></i>
          </button>
          <div class="h-9 w-9 rounded-full bg-[#0F9D58] flex items-center justify-center text-white text-[13px] font-semibold">
            BX
          </div>
        </div>
      </div>

      <h1 class="gb-heading text-[26px] font-bold mb-5">Discover</h1>

      <!-- hero -->
      <section class="relative rounded-3xl overflow-hidden bg-[#0F1B14] text-white p-7 lg:p-9 mb-9 min-h-[230px] flex flex-col justify-between">
        <div
          class="absolute inset-0 opacity-90"
          style="background: radial-gradient(circle at 78% 30%, #1F5E3C 0%, #0F1B14 62%);"
        ></div>
        <div class="relative z-10">
          <p class="text-[13px] text-[#9BE8B8] mb-2">Trending this week</p>
          <h2 class="gb-heading text-[34px] lg:text-[40px] font-bold leading-tight mb-1">{{ hero.title }}</h2>
          <p class="text-[14px] text-[#C9D6CE]">{{ hero.producer }} &middot; {{ hero.plays }}</p>
        </div>
        <div class="relative z-10 flex items-center gap-4 mt-6">
          <button
            @click="togglePlay"
            class="flex items-center gap-2 bg-[#0F9D58] hover:bg-[#0D8A4C] transition-colors text-white text-[14px] font-semibold px-5 py-2.5 rounded-full"
          >
            <i class="mdi" :class="isPlaying ? 'mdi-pause' : 'mdi-play'"></i>
            {{ isPlaying ? 'Playing' : 'Play now' }}
          </button>
          <button
            @click="toggleHeroFavorite"
            class="h-10 w-10 rounded-full bg-white/10 hover:bg-white/20 flex items-center justify-center transition-colors"
          >
            <i class="mdi" :class="hero.isFavorite ? 'mdi-heart text-[#4ADE80]' : 'mdi-heart-outline text-white'"></i>
          </button>
          <span class="ml-auto text-[15px] font-semibold text-[#9BE8B8]">{{ hero.price }}</span>
        </div>
      </section>

      <!-- top producers -->
      <section class="mb-9">
        <div class="flex items-center justify-between mb-4">
          <h3 class="gb-heading text-[16px] font-bold">Top producers</h3>
          <a href="#" class="text-[13px] text-[#0F9D58] font-semibold hover:underline">See all</a>
        </div>
        <div class="flex gap-6 overflow-x-auto pb-1">
          <div v-for="p in producers" :key="p.id" class="flex flex-col items-center gap-2 shrink-0">
            <div
              class="h-16 w-16 rounded-full flex items-center justify-center text-white text-[15px] font-semibold"
              :style="{ backgroundColor: p.color }"
            >
              {{ p.initials }}
            </div>
            <span class="text-[12px] text-[#5B6862]">{{ p.name }}</span>
          </div>
        </div>
      </section>

      <!-- top beats -->
      <section>
        <div class="flex items-center justify-between mb-4">
          <h3 class="gb-heading text-[16px] font-bold">Top beats</h3>
          <a href="#" class="text-[13px] text-[#0F9D58] font-semibold hover:underline">See all</a>
        </div>

        <div class="flex flex-col">
          <div
            v-for="(beat, idx) in topBeats"
            :key="beat.id"
            class="flex items-center gap-4 py-3 border-b border-[#EDF2EF] last:border-0"
          >
            <span class="w-5 text-[13px] text-[#9AA79E]">{{ idx + 1 }}</span>
            <div class="h-10 w-10 rounded-lg bg-[#EAF7EF] flex items-center justify-center shrink-0">
              <i class="mdi mdi-waveform text-[#0F9D58] text-[18px]"></i>
            </div>
            <div class="min-w-0 flex-1">
              <p class="text-[14px] font-semibold truncate">{{ beat.title }}</p>
              <p class="text-[12px] text-[#9AA79E]">{{ beat.producer }}</p>
            </div>
            <span class="hidden sm:block text-[13px] text-[#9AA79E] w-14">{{ beat.duration }}</span>
            <span class="text-[13px] font-semibold text-[#0F9D58] w-16 text-right">{{ beat.price }}</span>
            <button @click="toggleBeatFavorite(beat)" class="h-8 w-8 flex items-center justify-center text-[#5B6862] hover:text-[#0F9D58]">
              <i class="mdi" :class="beat.favorite ? 'mdi-heart text-[#0F9D58]' : 'mdi-heart-outline'"></i>
            </button>
            <button class="h-8 w-8 flex items-center justify-center text-[#5B6862] hover:text-[#0F9D58]">
              <i class="mdi mdi-dots-horizontal"></i>
            </button>
          </div>
        </div>

        <!-- mini player -->
        <div class="mt-5 flex items-center gap-4 rounded-2xl bg-[#F5F9F6] px-5 py-3">
          <button class="h-8 w-8 flex items-center justify-center text-[#5B6862]">
            <i class="mdi mdi-shuffle-variant"></i>
          </button>
          <button class="h-8 w-8 flex items-center justify-center text-[#5B6862]">
            <i class="mdi mdi-skip-previous"></i>
          </button>
          <button
            @click="togglePlay"
            class="h-9 w-9 rounded-full bg-[#0F9D58] flex items-center justify-center text-white"
          >
            <i class="mdi" :class="isPlaying ? 'mdi-pause' : 'mdi-play'"></i>
          </button>
          <button class="h-8 w-8 flex items-center justify-center text-[#5B6862]">
            <i class="mdi mdi-skip-next"></i>
          </button>
          <div class="flex-1 h-1.5 rounded-full bg-[#DCEAE1] overflow-hidden">
            <div class="h-full bg-[#0F9D58]" :style="{ width: progress + '%' }"></div>
          </div>
          <button class="h-8 w-8 flex items-center justify-center text-[#5B6862]">
            <i class="mdi mdi-repeat"></i>
          </button>
        </div>
      </section>
    </main>

    <!-- ============ RIGHT SIDEBAR ============ -->
    <aside class="hidden xl:flex flex-col w-72 shrink-0 border-l border-[#EDF2EF] px-6 py-6">
      <div class="relative mb-6">
        <i class="mdi mdi-magnify absolute left-3 top-1/2 -translate-y-1/2 text-[#9AA79E] text-[18px]"></i>
        <input
          type="text"
          placeholder="Search beats, producers..."
          class="w-full bg-[#F5F9F6] rounded-full pl-9 pr-3 py-2.5 text-[13px] outline-none focus:ring-2 focus:ring-[#0F9D58]/30"
        />
      </div>

      <div class="flex items-center justify-between mb-4">
        <h3 class="gb-heading text-[15px] font-bold">Library</h3>
        <div class="flex bg-[#F5F9F6] rounded-full p-1">
          <button
            v-for="tab in ['Purchased', 'Playlists']"
            :key="tab"
            @click="activeLibraryTab = tab"
            class="px-3 py-1 rounded-full text-[12px] font-medium transition-colors"
            :class="activeLibraryTab === tab ? 'bg-white text-[#0F9D58] shadow-sm' : 'text-[#9AA79E]'"
          >
            {{ tab }}
          </button>
        </div>
      </div>

      <div class="flex flex-col gap-1 mb-8">
        <button
          v-for="item in library"
          :key="item.id"
          class="flex items-center justify-between px-2 py-2 rounded-xl hover:bg-[#F5F9F6] transition-colors text-left"
        >
          <div class="flex items-center gap-3 min-w-0">
            <div class="h-9 w-9 rounded-lg bg-[#EAF7EF] flex items-center justify-center shrink-0">
              <i class="mdi mdi-music-note text-[#0F9D58] text-[16px]"></i>
            </div>
            <div class="min-w-0">
              <p class="text-[13px] font-semibold truncate">{{ item.title }}</p>
              <p class="text-[11px] text-[#9AA79E]">{{ item.kind }} &middot; {{ item.producer }}</p>
            </div>
          </div>
          <i class="mdi mdi-chevron-right text-[#C7D2CB]"></i>
        </button>
      </div>

      <div class="mt-auto rounded-2xl overflow-hidden bg-[#0F1B14] text-white">
        <div class="h-32 bg-gradient-to-br from-[#1F5E3C] to-[#0F1B14] flex items-center justify-center">
          <i class="fa-solid fa-compact-disc text-[#4ADE80] text-4xl"></i>
        </div>
        <div class="p-4 flex items-center justify-between">
          <div class="min-w-0">
            <p class="text-[13px] font-semibold truncate">Summer Trap Pack</p>
            <p class="text-[11px] text-[#9AA79E]">Kelvin Wave</p>
          </div>
          <i class="mdi mdi-heart text-[#4ADE80] text-[18px] shrink-0"></i>
        </div>
      </div>
    </aside>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Manrope:wght@600;700;800&family=Inter:wght@400;500;600&display=swap');

.gb-shell {
  font-family: 'Inter', ui-sans-serif, system-ui, sans-serif;
}

.gb-heading {
  font-family: 'Manrope', ui-sans-serif, system-ui, sans-serif;
  letter-spacing: -0.01em;
}
</style>