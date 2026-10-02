<script setup>
import { onMounted, onUnmounted, ref } from 'vue'
import logoUrl from './assets/logos/TheVue-Compact-Gold.png'
import footerLogoUrl from './assets/logos/TheVue-Primary-Gold.png'
import heroImage from './assets/images/Hero_Placeholder.jpeg'
import boatImageOne from './assets/images/Boat_1.jpg'
import boatImageTwo from './assets/images/Boat_2.jpg'
import boatImageThree from './assets/images/Boat_3.jpg'

const boatImages = [
  { src: boatImageOne, alt: 'Aerial view of a boat on turquoise Whitsundays water' },
  { src: boatImageTwo, alt: 'Boat travelling along the Whitsundays coastline' },
  { src: boatImageThree, alt: 'Boat beside the Whitsundays shore' },
]

const activeBoatSlide = ref(0)
const isBoatSlideshowPlaying = ref(true)
let boatSlideshowTimer

function showBoatSlide(index) {
  activeBoatSlide.value = (index + boatImages.length) % boatImages.length
}

function startBoatSlideshow() {
  window.clearInterval(boatSlideshowTimer)
  isBoatSlideshowPlaying.value = true
  boatSlideshowTimer = window.setInterval(() => {
    showBoatSlide(activeBoatSlide.value + 1)
  }, 6500)
}

function stopBoatSlideshow() {
  window.clearInterval(boatSlideshowTimer)
  isBoatSlideshowPlaying.value = false
}

function toggleBoatSlideshow() {
  if (isBoatSlideshowPlaying.value) {
    stopBoatSlideshow()
  } else {
    startBoatSlideshow()
  }
}

onMounted(async () => {
  if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
    isBoatSlideshowPlaying.value = false
    return
  }

  await Promise.all(boatImages.map(({ src }) => {
    const image = new Image()
    image.src = src
    return image.decode().catch(() => undefined)
  }))

  startBoatSlideshow()
})

onUnmounted(stopBoatSlideshow)
</script>

<template>
  <div class="page-shell">
    <header class="site-header">
      <div class="nav-bar">
        <a href="#home" class="brand" aria-label="The Vue home">
          <img :src="logoUrl" alt="The Vue logo" class="brand-logo" />
        </a>

        <nav class="nav" aria-label="Main navigation">
          <a href="#home">Home</a>
          <a href="#retreat">Retreat</a>
          <a href="#experiences">Experiences</a>
          <a href="#the-boat">The Boat</a>
          <a href="#gallery">Gallery</a>
          <a href="#about">About</a>
        </nav>

        <button class="nav-cta">Enquire</button>
      </div>
    </header>

    <main>
      <section id="home" class="hero">
        <div class="hero-visual" aria-label="Hero image for the main banner">
          <img :src="heroImage" alt="The Vue hero placeholder" class="hero-image" />

          <div class="hero-copy">
              <h1>A private retreat beyond expectation.</h1>

            <div class="hero-actions">
              <a class="primary hero-primary" href="#contact">REQUEST AVAILABILITY</a>
            </div>
          </div>
        </div>
      </section>

      <section id="retreat" class="feature-panel">
        <div class="feature-copy">
          <p class="eyebrow eyebrow-dark">Retreat</p>
          <h2>A slower rhythm, shaped by the sea.</h2>
          <p>
            Set across a private coastline and sheltered gardens, The Vue is designed for ease,
            privacy, and unforgettable rituals from sunrise to starlight.
          </p>
          <a class="primary retreat-cta" href="#contact">Discover more</a>
        </div>

        <div class="feature-media">
          <img :src="heroImage" alt="The Vue retreat view" class="feature-photo feature-photo-large" />
          <div class="feature-note">
            <span>Private suites</span>
            <strong>10 curated residences</strong>
          </div>
        </div>
      </section>

      <section id="experiences" class="section experience-section">
        <div class="section-heading experiences-heading">
          <div class="section-heading-copy narrow">
            <p class="eyebrow eyebrow-dark">Experiences</p>
            <h2>Designed for the way you like to wander.</h2>
          </div>
          <a class="primary retreat-cta experiences-cta" href="#contact">Discover more</a>
        </div>

        <div class="experience-grid">
          <article class="experience-card">
            <img :src="heroImage" alt="Waterfront dining" />
            <div class="experience-copy">
              <span>01</span>
              <h3>Private dining</h3>
              <p>Chef-led evenings, candlelit tables, and seafood menus shaped by the season.</p>
            </div>
          </article>

          <article class="experience-card">
            <img :src="heroImage" alt="Wellbeing rituals" />
            <div class="experience-copy">
              <span>02</span>
              <h3>Wellbeing rituals</h3>
              <p>Movement, massage, and moments of stillness designed to restore the body and mind.</p>
            </div>
          </article>

          <article class="experience-card">
            <img :src="heroImage" alt="Coastal adventures" />
            <div class="experience-copy">
              <span>03</span>
              <h3>Coastal adventures</h3>
              <p>From hushed morning swims to guided island excursions, every day unfolds at your pace.</p>
            </div>
          </article>
        </div>
      </section>

      <section id="the-boat" class="boat-experience">
        <div class="showcase-panel">
          <div class="showcase-visual" role="region" aria-label="The Vue boat photos" aria-roledescription="carousel">
            <img
              v-for="(image, index) in boatImages"
              :key="image.src"
              :src="image.src"
              :alt="image.alt"
              :class="['boat-slide', { 'is-active': index === activeBoatSlide }]"
              :aria-hidden="index !== activeBoatSlide"
            />
          </div>
          <div class="showcase-copy">
            <p class="eyebrow">The boat</p>
            <h2>A private passage through the Whitsundays.</h2>
            <p>
              Spend long afternoons drifting between hidden coves, quiet anchorages, and untouched shores,
              accompanied by a tailored route and attentive crew.
            </p>
            <a href="#contact" class="boat-cta">Discover private charters</a>
          </div>
          <div class="boat-carousel-controls" role="group" aria-label="Boat slideshow controls">
            <button class="boat-carousel-arrow" type="button" aria-label="Previous photo" @click="showBoatSlide(activeBoatSlide - 1)">‹</button>
            <div class="boat-carousel-dots" aria-label="Choose a boat photo">
              <button
                v-for="(image, index) in boatImages"
                :key="image.src"
                class="boat-carousel-dot"
                :class="{ 'is-active': index === activeBoatSlide }"
                type="button"
                :aria-label="`Show photo ${index + 1}`"
                :aria-pressed="index === activeBoatSlide"
                @click="showBoatSlide(index)"
              ></button>
            </div>
            <button class="boat-carousel-arrow" type="button" aria-label="Next photo" @click="showBoatSlide(activeBoatSlide + 1)">›</button>
            <button
              class="boat-carousel-toggle"
              type="button"
              :aria-label="isBoatSlideshowPlaying ? 'Pause slideshow' : 'Play slideshow'"
              :aria-pressed="isBoatSlideshowPlaying"
              @click="toggleBoatSlideshow"
            >{{ isBoatSlideshowPlaying ? 'Pause' : 'Play' }}</button>
          </div>
        </div>
        <div class="boat-details" aria-label="Boat experience highlights">
          <p class="boat-detail">Tailored routes</p>
          <p class="boat-detail">Quiet anchorages</p>
          <p class="boat-detail">Secluded shores</p>
        </div>
      </section>

      <section id="gallery" class="section gallery-section">
        <div class="gallery-header">
          <div>
            <p class="eyebrow eyebrow-dark">Gallery</p>
            <h2>A place defined by light, stillness, and texture.</h2>
          </div>
          <a href="#contact" class="inline-link">View more</a>
        </div>

        <div class="gallery-grid">
          <img :src="heroImage" alt="Retreat terrace" class="gallery-item tall" />
          <img :src="heroImage" alt="Dining experience" class="gallery-item" />
          <img :src="heroImage" alt="Private pool" class="gallery-item" />
          <img :src="heroImage" alt="Coastal view" class="gallery-item wide" />
        </div>
      </section>

      <section id="about" class="story-section">
        <div class="story-copy">
          <p class="eyebrow eyebrow-dark">About</p>
          <h2>Thoughtful luxury, shaped by place.</h2>
          <p>
            The Vue is an intimate retreat inspired by the pace of the coastline — calm, considered,
            and deeply personal. Every detail is curated to create space for belonging, reflection,
            and memorable hospitality.
          </p>
        </div>

        <div class="story-quote">
          <p>
            “A sanctuary for long conversations, slow mornings, and a way of travelling that feels entirely your own.”
          </p>
        </div>
      </section>
    </main>

    <footer id="contact" class="footer">
      <div class="footer-inner">
        <a href="#home" class="footer-brand" aria-label="The Vue home">
          <img :src="footerLogoUrl" alt="The Vue" class="footer-logo" />
        </a>
        <nav class="footer-nav" aria-label="Footer navigation">
          <a href="#home">Home</a>
          <a href="#retreat">Retreat</a>
          <a href="#experiences">Experiences</a>
          <a href="#the-boat">The Boat</a>
          <a href="#gallery">Gallery</a>
          <a href="#about">About</a>
        </nav>
        <a href="mailto:hello@thevue.com" class="footer-cta">Enquire</a>
        <p class="footer-copyright">© The Vue Hamilton Island Private Retreat 2026. All Rights Reserved.</p>
      </div>
    </footer>
  </div>
</template>
