<script>
  import { onMount } from 'svelte';
  import { writable } from 'svelte/store';

  let tripImages = [
    "/jay1.jpg",
    "/jay2.jpg",
    "/jay3.jpg",
    "/jay4.jpg",
    "/jay5.jpg",
    "/jay6.jpg",
    "/jay7.jpg",
    "/jay8.jpg",
    "/jay9.jpg",
    "/jay10.jpg",
    "/jay11.jpg",
    "/jay12.jpg",
    "/jay13.jpg",
    "/jay14.jpg",
    "/jay15.jpg",
    "/jay16.jpg",
    "/jay17.jpg",
    "/jay18.jpg",
    "/jay19.jpg",
    "/jay21.jpg"
  ]
  let tripImages2 = [
    "/photo3.jpg",
    "/photo6.JPEG",
    "/photo1.JPG",
    "/photo2.jpg",
    "/photo4.jpg",
    "/photo5.jpg"
  ]

  let current = 0;
  let carouselInterval = 0;

  const showScrollHint = writable(true);

  function handleScroll() {
    const scrollPosition = window.scrollY + window.innerHeight;
    const fullHeight = document.documentElement.scrollHeight;

    if (scrollPosition >= fullHeight - 100) {
      showScrollHint.set(false);
    } else {
      showScrollHint.set(true);
    }
  }

  function nextImage() {
    current = (current + 1) % tripImages2.length;
  }

  onMount(() => {
    carouselInterval = setInterval(nextImage, 4000);
    window.addEventListener('scroll', handleScroll);
    handleScroll();

    return () => {
      if (carouselInterval) clearInterval(carouselInterval);
      window.removeEventListener('scroll', handleScroll);
    };
  });
</script>

<svelte:head>
  <title>Annual Jay Peak Trip - RIT Alpine Ski Club</title>
</svelte:head>

<style>
  :global(body), :global(html) {
    margin: 0;
    padding: 0;
    width: 100%;
    overflow-x: hidden;
    -webkit-text-size-adjust: 100%;
  }

  main {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 2rem 0 6rem;
    width: 100%;
    max-width: 100vw;
    box-sizing: border-box;
  }

  .hero-box {
    background: linear-gradient(135deg, #3b6e47, #264d40);
    border: 4px solid #5a6f54;
    padding: 3rem 1.5rem;
    border-radius: 25px;
    box-shadow: 0 0 40px 10px rgba(60, 130, 180, 0.6);
    text-align: center;
    width: 1000px;
    max-width: 95vw;
    margin-bottom: 2rem;
    box-sizing: border-box;
  }

  .hero-box h1 {
    margin: 0 0 0.5rem;
    font-size: 3.5rem;
    font-weight: 900;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: #d9f0ff;
    text-shadow:
      0 0 10px #a3c4f3dd,
      0 0 20px #a3c4f377;
  }

  .hero-box .subtitle {
    font-size: 1.5rem;
    color: #a3c4f3;
    margin: 0;
    font-weight: 600;
  }

  @media (max-width: 768px) {
    .hero-box h1 {
      font-size: 2rem;
      line-height: 1.2;
    }
    .hero-box .subtitle {
      font-size: 1.1rem;
    }
  }

  .carousel-section {
    width: 1000px;
    max-width: 95vw;
    margin-bottom: 2rem;
  }

  .carousel {
    height: 500px;
    border-radius: 25px;
    overflow: hidden;
    border: 4px solid #5a6f54;
    box-shadow: 0 0 30px 6px rgba(60, 130, 180, 0.3);
    background: #f9fbf9;
    position: relative;
  }

  @media (max-width: 768px) {
    .carousel {
      height: 300px;
    }
  }

  .carousel img {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    object-fit: cover;
    opacity: 0;
    transition: opacity 1.2s ease-in-out;
    pointer-events: none;
  }

  .carousel img.active {
    opacity: 1;
    z-index: 1;
  }

  .info-box {
    width: 1000px;
    max-width: 95vw;
    background: linear-gradient(135deg, #cde3f7, #a0b8d9);
    border: 4px solid #5a6f54;
    border-radius: 25px;
    padding: 2rem 1.5rem;
    box-shadow: 0 0 30px 6px rgba(90, 111, 84, 0.3);
    color: #213544;
    font-size: 1.1rem;
    line-height: 1.8;
    margin-bottom: 2rem;
    box-sizing: border-box;
  }

  @media (min-width: 768px) {
    .info-box {
      font-size: 1.3rem;
      padding: 2.5rem 2rem;
    }
  }

  .info-box h2 {
    font-size: 2rem;
    margin-top: 0;
    margin-bottom: 1rem;
    color: #264d40;
    text-shadow: 0 0 5px #a3c4f3bb;
  }

  @media (min-width: 768px) {
    .info-box h2 {
      font-size: 2.5rem;
    }
  }

  .info-box p {
    margin-bottom: 1.2rem;
  }

  .info-box strong {
    color: #264d40;
  }

  /* ── Marquee Gallery ── */
  .marquee-wrapper {
    width: 1000px;
    max-width: 95vw;
    background: linear-gradient(135deg, #cde3f7, #a0b8d9);
    border: 4px solid #5a6f54;
    border-radius: 25px;
    padding: 1.5rem 0;
    box-shadow: 0 0 30px 6px rgba(90, 111, 84, 0.3);
    margin-bottom: 2rem;
    box-sizing: border-box;
    overflow: hidden;
    position: relative;
  }

  .marquee-wrapper::before,
  .marquee-wrapper::after {
    content: '';
    position: absolute;
    top: 0;
    bottom: 0;
    width: 80px;
    z-index: 2;
    pointer-events: none;
  }

  .marquee-wrapper::before {
    left: 0;
    background: linear-gradient(to right, #cde3f7, transparent);
  }

  .marquee-wrapper::after {
    right: 0;
    background: linear-gradient(to left, #a0b8d9, transparent);
  }

  .marquee-track {
    display: flex;
    gap: 1.2rem;
    width: max-content;
    animation: marquee-scroll 40s linear infinite;
    padding: 0.5rem 1.2rem;
  }

  .marquee-track:hover {
    animation-play-state: paused;
  }

  @keyframes marquee-scroll {
    0%   { transform: translateX(0); }
    100% { transform: translateX(-50%); }
  }

  .marquee-item {
    flex-shrink: 0;
    width: 320px;
    height: 220px;
    border-radius: 20px;
    overflow: hidden;
    border: 3px solid #5a6f54;
    box-shadow: 0 0 20px 4px rgba(60, 130, 180, 0.25);
    transition: transform 0.3s ease, box-shadow 0.3s ease;
    cursor: pointer;
  }

  .marquee-item:hover {
    transform: scale(1.04) translateY(-4px);
    box-shadow: 0 0 32px 8px rgba(60, 130, 180, 0.45);
  }

  .marquee-item img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
    transition: transform 0.5s ease;
  }

  .marquee-item:hover img {
    transform: scale(1.07);
  }

  @media (max-width: 768px) {
    .marquee-item {
      width: 220px;
      height: 155px;
    }

    .marquee-wrapper::before,
    .marquee-wrapper::after {
      width: 40px;
    }
  }

  /* ── Scroll hint ── */
  .scroll-hint {
    position: fixed;
    bottom: 20px;
    left: 50%;
    transform: translateX(-50%);
    font-size: 1.2rem;
    font-weight: 700;
    color: #000000ee;
    text-shadow: none;
    animation: bounce 2.5s infinite ease-in-out;
    user-select: none;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 6px;
    z-index: 9999;
    pointer-events: none;
    transition: opacity 0.5s ease;
  }

  .scroll-hint.hidden {
    opacity: 0;
    pointer-events: none;
  }

  .scroll-hint svg {
    width: 24px;
    height: 24px;
    stroke: #000000ee;
    stroke-width: 3;
    fill: none;
    animation: arrowDown 2.5s infinite ease-in-out;
  }

  @keyframes bounce {
    0%, 100% { transform: translate(-50%, 0); }
    50% { transform: translate(-50%, 15px); }
  }

  @keyframes arrowDown {
    0%, 100% { opacity: 1; transform: translateY(0); }
    50% { opacity: 0.4; transform: translateY(8px); }
  }
</style>

<main>
  <div class="hero-box" role="banner">
    <h1>Annual Jay Peak Trip</h1>
    <p class="subtitle">The Ultimate Winter Break Adventure</p>
  </div>

  <div class="carousel-section">
    <div class="carousel" aria-label="Trip photo carousel">
      {#each tripImages2 as src, i (src)}
        <img 
          src={src} 
          alt="Trip memory {i + 1}" 
          class:active={i === current}
          loading={i === 0 ? 'eager' : 'lazy'}
        />
      {/each}
    </div>
  </div>

  <section class="info-box" aria-labelledby="trip-info-heading">
    <h2 id="trip-info-heading">Experience the Best of Vermont Skiing</h2>
    <p>
      Every year during the last week of winter break, RIT Alpine Ski Club brinds ~30 people to Jay Peak Resort in Vermont 
      for our signature annual trip. This is the highlight of our season and an incredible opportunity to ski 
      some of the best terrain on the East Coast while bonding with fellow club members. We will introduce the trip at the start of the fall 2026 semester, with several
      interest meetings and logistics planning to follow. Tickets are ~$500, and include 5 full day passes and 5 
      nights at mountainside cabins. All photos below were 
      taken from the 2026 trip. Please reach out if you're interested in joining us next year!
    </p>
  </section>

  <!-- Marquee photo gallery -->
  <div class="marquee-wrapper" aria-label="Scrolling trip photo gallery">
    <div class="marquee-track">
      {#each tripImages as src, i}
        <div class="marquee-item">
          <img src={src} alt="Jay Peak trip photo {i + 1}" loading="lazy" />
        </div>
      {/each}
      <!-- Duplicate set for seamless looping -->
      {#each tripImages as src, i}
        <div class="marquee-item" aria-hidden="true">
          <img src={src} alt="" loading="lazy" />
        </div>
      {/each}
    </div>
  </div>

  {#if $showScrollHint}
    <div class="scroll-hint" aria-hidden="true">
      <span>Scroll down</span>
      <svg viewBox="0 0 24 24">
        <polyline points="6 9 12 15 18 9" />
      </svg>
    </div>
  {/if}
</main>