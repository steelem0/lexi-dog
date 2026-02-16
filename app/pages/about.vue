<script setup lang="ts">
const year = new Date().getFullYear();
</script>

<template>
  <main class="min-h-screen bg-[#fbf7f1] text-gray-800">
    <!-- HEADER -->
    <!-- HEADER -->
    <header
      class="sticky top-0 z-20 bg-transparent backdrop-blur border-b border-purple-100"
    >
      <div
        class="max-w-6xl mx-auto px-6 py-4 flex items-center justify-between"
      >
        <NuxtLink to="/" class="font-serif text-xl text-purple-900">
          lexithe.dog
        </NuxtLink>

        <nav class="text-sm text-gray-700 flex gap-6">
          <NuxtLink to="/" class="hover:text-purple-800 transition"
            >Home</NuxtLink
          >
          <NuxtLink to="/about" class="hover:text-purple-800 transition"
            >About</NuxtLink
          >
        </nav>
      </div>
    </header>
    <!-- HERO -->
    <section
      class="relative overflow-hidden bg-gradient-to-br from-purple-100 via-[#fbf7f1] to-white"
    >
      <div class="max-w-5xl mx-auto px-6 py-16 text-center">
        <div class="text-xs uppercase tracking-[0.25em] text-purple-700/80">
          About Lexi
        </div>
        <h1 class="mt-4 font-serif text-5xl text-purple-900">
          Professional Good Girl
        </h1>
        <p class="mt-6 text-lg text-gray-700 max-w-2xl mx-auto">
          Lexi is a very large Cane Corso with very important responsibilities —
          including yard security, emotional support, and monitoring all food
          activities.
        </p>
      </div>

      <div class="relative h-12 bg-[#fbf7f1] overflow-hidden">
        <div
          class="absolute inset-0 bg-[#fbf7f1] -skew-y-2 origin-top-left"
        ></div>
      </div>
    </section>

    <!-- PROFILE -->
    <section class="py-16 px-6">
      <div class="max-w-6xl mx-auto grid md:grid-cols-2 gap-12 items-center">
        <div
          class="rounded-3xl overflow-hidden border border-purple-100 shadow-lg"
        >
          <img
            src="/images/lexi/lexi.png"
            alt="Lexi portrait"
            class="w-full h-[420px] object-cover"
          />
        </div>

        <div>
          <h2 class="font-serif text-4xl text-purple-900">
            Official Biography
          </h2>
          <p class="mt-4 text-gray-700">
            Born with natural authority and zero understanding of personal
            space, Lexi takes her role seriously. She patrols the perimeter
            hourly, supervises all outdoor activities, and ensures visitors
            behave appropriately.
          </p>
          <p class="mt-4 text-gray-700">
            Despite her intimidating presence, she is extremely affectionate and
            firmly believes she is a medium-sized lap dog. She also believes all
            snacks are shared property.
          </p>
        </div>
      </div>
    </section>

    <!-- STATS -->
    <section class="bg-white py-16 px-6">
      <div class="max-w-4xl mx-auto text-center">
        <h2 class="font-serif text-4xl text-purple-900">Specifications</h2>

        <div class="mt-10 grid sm:grid-cols-2 gap-6 text-left">
          <div class="p-6 rounded-2xl border border-purple-100 bg-purple-50">
            <div class="text-sm uppercase tracking-wide text-purple-700">
              Breed
            </div>
            <div class="mt-2 text-lg font-semibold">Cane Corso</div>
          </div>

          <div class="p-6 rounded-2xl border border-purple-100 bg-purple-50">
            <div class="text-sm uppercase tracking-wide text-purple-700">
              Weight Class
            </div>
            <div class="mt-2 text-lg font-semibold">Large And In Charge</div>
          </div>

          <div class="p-6 rounded-2xl border border-purple-100 bg-purple-50">
            <div class="text-sm uppercase tracking-wide text-purple-700">
              Primary Skill
            </div>
            <div class="mt-2 text-lg font-semibold">Perimeter Security</div>
          </div>

          <div class="p-6 rounded-2xl border border-purple-100 bg-purple-50">
            <div class="text-sm uppercase tracking-wide text-purple-700">
              Weakness
            </div>
            <div class="mt-2 text-lg font-semibold">Cheeseburgers</div>
          </div>
        </div>
      </div>
    </section>

    <!-- FOOTER -->
    <footer class="py-10 text-center text-sm text-gray-500">
      © {{ year }} Lexi The Dog — All snacks monitored
    </footer>
  </main>
</template>

<style scoped>
.font-serif {
  font-family: "Playfair Display", serif;
}
</style>
