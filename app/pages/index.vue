<template>
  <main class="min-h-screen bg-cream text-charcoal">
    <!-- HERO -->
    <section
      class="relative overflow-hidden bg-gradient-to-br from-purple-100 via-cream to-white"
    >
      <!-- Soft glow blobs -->
      <div class="pointer-events-none absolute inset-0">
        <div class="blob blob-1"></div>
        <div class="blob blob-2"></div>
      </div>

      <!-- Paw “particles” (CSS-only) -->
      <div class="pointer-events-none absolute inset-0">
        <span class="paw paw-1" aria-hidden="true">🐾</span>
        <span class="paw paw-2" aria-hidden="true">🐾</span>
        <span class="paw paw-3" aria-hidden="true">🐾</span>
        <span class="paw paw-4" aria-hidden="true">🐾</span>
      </div>

      <div class="relative z-10 max-w-6xl mx-auto px-6 py-20 md:py-24">
        <div class="grid md:grid-cols-2 gap-12 items-center">
          <!-- Copy -->
          <div>
            <div
              class="inline-flex items-center gap-2 rounded-full border border-purple-200 bg-white/60 backdrop-blur px-4 py-2 text-xs tracking-[0.25em] uppercase text-purple-800"
            >
              Lexi The Dog
              <span class="h-1.5 w-1.5 rounded-full bg-purple-600"></span>
              Cane Corso
            </div>

            <h1
              class="mt-6 font-serif text-5xl md:text-6xl leading-[1.05] text-purple-900"
            >
              Backyard Defender.<br />
              Cheeseburger Enthusiast.
            </h1>

            <p class="mt-6 text-lg text-gray-700 max-w-prose">
              150lb fawn Cane Corso with a black mask and a purple collar.
              Serious about perimeter security. Emotionally available for
              snacks.
            </p>

            <div class="mt-8 flex flex-wrap gap-3">
              <NuxtLink
                to="/about"
                class="px-6 py-3 rounded-full bg-purple-700 text-white font-semibold shadow-sm hover:bg-purple-800 transition"
              >
                Meet Lexi
              </NuxtLink>

              <NuxtLink
                to="/protect-the-yard"
                class="px-6 py-3 rounded-full bg-amber-500 text-white font-semibold shadow-sm hover:bg-amber-600 transition"
              >
                Play Protect the Yard
              </NuxtLink>

              <!--NuxtLink
                to="/gallery"
                class="px-6 py-3 rounded-full border border-purple-700 text-purple-800 font-semibold bg-white/60 backdrop-blur hover:bg-white transition"
              >
                The Gallery
              </NuxtLink-->
            </div>

            <p class="mt-6 text-sm text-gray-500 italic">
              Warning: may judge you quietly.
            </p>
          </div>

          <!-- Hero “device” photo frame -->
          <div class="flex justify-center md:justify-end">
            <div class="relative w-[320px] md:w-[380px]">
              <div class="frame">
                <img
                  src="/images/lexi/lexi.png"
                  alt="Lexi the Cane Corso wearing a purple collar"
                  class="h-[460px] md:h-[520px] w-full object-cover"
                  loading="lazy"
                />
              </div>

              <!-- little label chip -->
              <div
                class="absolute -bottom-5 left-1/2 -translate-x-1/2 rounded-full border border-purple-200 bg-white/80 backdrop-blur px-4 py-2 text-xs text-gray-700 shadow-sm"
              >
                Purple collar • Big vibe
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- angled divider -->
      <div class="relative h-14 bg-cream overflow-hidden">
        <div class="absolute inset-0 bg-cream -skew-y-2 origin-top-left"></div>
      </div>
    </section>

    <!-- FEATURES -->
    <section class="bg-cream py-16 px-6">
      <div class="max-w-6xl mx-auto">
        <div class="text-center mb-12">
          <div class="kicker">Vibe Check</div>
          <h2 class="mt-3 font-serif text-4xl text-purple-900">
            Three modes. One queen.
          </h2>
          <p class="mt-4 text-gray-700 text-lg max-w-3xl mx-auto">
            Lexi is equal parts security system, cuddle tank, and snack
            attorney.
          </p>
        </div>

        <div class="grid md:grid-cols-3 gap-8">
          <div class="card">
            <div class="badge">🛡️</div>
            <h3 class="mt-5 text-lg font-semibold text-gray-900">Guard Mode</h3>
            <p class="mt-2 text-sm text-gray-700">
              Notices everything. Approves nothing. Will escort suspicious
              leaves off the premises.
            </p>
          </div>

          <div class="card">
            <div class="badge">💅</div>
            <h3 class="mt-5 text-lg font-semibold text-gray-900">
              Soft Girl Mode
            </h3>
            <p class="mt-2 text-sm text-gray-700">
              Pretends she’s small. Claims the couch. Requires forehead kisses
              and emotional support.
            </p>
          </div>

          <div class="card">
            <div class="badge">🍔</div>
            <h3 class="mt-5 text-lg font-semibold text-gray-900">Snack Mode</h3>
            <p class="mt-2 text-sm text-gray-700">
              Negotiates intensely. Believes all burgers are community property.
              Especially yours.
            </p>
          </div>
        </div>
      </div>
    </section>

    <!--
    <!-- GALLERY PREVIEW -->
    <!--section class="bg-purple-50 py-18 px-6">
      <div class="max-w-6xl mx-auto">
        <div class="text-center mb-10">
          <div class="kicker">Gallery</div>
          <h2 class="mt-3 font-serif text-4xl text-purple-900">
            Evidence of excellence
          </h2>
          <p class="mt-4 text-gray-700 text-lg max-w-3xl mx-auto">
            A curated selection of Lexi being majestic, dramatic, and
            occasionally offended.
          </p>
        </div>

        <div class="grid sm:grid-cols-2 lg:grid-cols-3 gap-6">
          <a class="tile" href="/gallery">
            <img
              src="/images/lexi/lexi.png"
              alt="Lexi portrait"
              loading="lazy"
            />
            <span class="chip">Guard Mode</span>
          </a>

          <a class="tile" href="/gallery">
            <img
              src="/images/lexi/lexi.png"
              alt="Lexi outdoors"
              loading="lazy"
            />
            <span class="chip">Outside Boss</span>
          </a>

          <a class="tile" href="/gallery">
            <img
              src="/images/lexi/lexi.png"
              alt="Lexi being cute"
              loading="lazy"
            />
            <span class="chip">Soft Girl</span>
          </a>
        </div>

        <div class="mt-10 text-center">
          <NuxtLink
            to="/gallery"
            class="px-7 py-3 rounded-full border border-purple-700 text-purple-800 font-semibold bg-white/70 backdrop-blur hover:bg-white transition"
          >
            Open full gallery
          </NuxtLink>
        </div>
      </div>
    </section>
    -->

    <!-- FOOTER -->
    <footer class="bg-cream py-12 px-6">
      <div
        class="max-w-6xl mx-auto flex flex-col md:flex-row gap-6 md:items-center md:justify-between"
      >
        <div>
          <div class="font-serif text-xl text-purple-900">lexithe.dog</div>
          <div class="text-sm text-gray-600 mt-1">
            Big dog. Purple collar. Strong opinions.
          </div>
        </div>

        <div class="text-sm text-gray-600">
          © {{ year }} Lexi The Dog. All burgers reserved.
        </div>
      </div>
    </footer>
  </main>
</template>

<script setup lang="ts">
const year = new Date().getFullYear();
</script>

<style scoped>
/* Simple “brand” colors without needing Tailwind config changes */
.bg-cream {
  background-color: #fbf7f1;
}
.text-charcoal {
  color: #1f2937;
}

/* Kicker label */
.kicker {
  @apply text-xs uppercase tracking-[0.22em] text-purple-700/80;
}

/* Card + badge (Pinterest landing vibe) */
.card {
  @apply bg-white/80 backdrop-blur p-8 rounded-3xl border border-purple-100 shadow-sm;
}
.badge {
  @apply w-12 h-12 rounded-full bg-purple-100 flex items-center justify-center text-purple-800;
}

/* Photo frame */
.frame {
  @apply rounded-[34px] border border-purple-100 bg-white/70 backdrop-blur shadow-xl overflow-hidden;
}

/* Gallery tiles */
.tile {
  @apply relative rounded-3xl overflow-hidden border border-purple-100 bg-white shadow-sm block;
}
.tile img {
  @apply w-full h-[260px] object-cover transition-transform duration-500;
}
.tile:hover img {
  transform: scale(1.03);
}
.chip {
  @apply absolute bottom-4 left-4 px-3 py-1 rounded-full text-xs bg-white/80 backdrop-blur border border-purple-100 text-gray-700;
}

/* Soft glow blobs */
.blob {
  position: absolute;
  border-radius: 9999px;
  filter: blur(64px);
}
.blob-1 {
  width: 560px;
  height: 560px;
  top: -160px;
  left: -160px;
  background: rgba(168, 85, 247, 0.2);
}
.blob-2 {
  width: 480px;
  height: 480px;
  top: 120px;
  right: -160px;
  background: rgba(196, 181, 253, 0.35);
}

/* Paw particles */
.paw {
  position: absolute;
  opacity: 0.12;
  filter: blur(0.2px);
  animation: floatPaw 10s ease-in-out infinite;
  transform: translateY(0);
  font-size: 26px;
}
.paw-1 {
  top: 18%;
  left: 8%;
  animation-duration: 11s;
}
.paw-2 {
  top: 30%;
  right: 14%;
  animation-duration: 13s;
  font-size: 22px;
}
.paw-3 {
  bottom: 22%;
  left: 18%;
  animation-duration: 12s;
  font-size: 24px;
}
.paw-4 {
  bottom: 18%;
  right: 10%;
  animation-duration: 14s;
  font-size: 20px;
}

@keyframes floatPaw {
  0%,
  100% {
    transform: translateY(0px) rotate(-3deg);
  }
  50% {
    transform: translateY(-18px) rotate(3deg);
  }
}
</style>
