<script>
  import { onMount } from 'svelte';
  import { writable } from 'svelte/store';

  let tripImages = [
    "/photo3.jpg",
    "/photo6.JPEG",
    "/photo1.JPG",
    "/photo2.jpg",
    "/photo4.jpg",
    "/photo5.jpg"
  ];

  let current = 0;
  let total = tripImages.length;
  let carouselInterval;

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
    current = (current + 1) % total;
  }

  onMount(() => {
    // Start carousel
    carouselInterval = setInterval(nextImage, 4000);

    // Add scroll listener
    window.addEventListener('scroll', handleScroll);
    
    // Initial scroll check
    handleScroll();

    // Cleanup
    return () => {
      if (carouselInterval) {
        clearInterval(carouselInterval);
      }
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

  .links-container {
    width: 1000px;
    max-width: 95vw;
    display: grid;
    grid-template-columns: 1fr;
    gap: 1.5rem;
    margin-bottom: 2rem;
  }

  @media (min-width: 768px) {
    .links-container {
      grid-template-columns: repeat(2, 1fr);
    }
  }

  .link-box {
    background: linear-gradient(135deg, #e6f0e8, #cde3f7);
    border: 4px solid #5a6f54;
    border-radius: 25px;
    padding: 2rem 1.5rem;
    box-shadow: 0 0 30px 6px rgba(60, 130, 180, 0.3);
    text-align: center;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
    box-sizing: border-box;
  }

  .link-box:hover {
    transform: translateY(-5px);
    box-shadow: 0 0 40px 8px rgba(60, 130, 180, 0.5);
  }

  .link-box h3 {
    font-size: 1.8rem;
    margin: 0 0 1rem;
    color: #264d40;
    text-shadow: 0 0 5px #a3c4f3bb;
  }

  .link-box p {
    font-size: 1rem;
    color: #213544;
    margin-bottom: 1.5rem;
    line-height: 1.5;
  }

  @media (min-width: 768px) {
    .link-box p {
      font-size: 1.1rem;
    }
  }

  .link-button {
    display: inline-block;
    font-weight: 700;
    font-size: 1.1rem;
    color: #d9f0ff;
    background: linear-gradient(135deg, #3b6e47, #264d40);
    text-decoration: none;
    border: 3px solid #264d40;
    padding: 0.8rem 1.8rem;
    border-radius: 18px;
    transition: background 0.35s ease, box-shadow 0.35s ease, transform 0.2s ease;
    box-shadow: 0 0 12px 3px rgba(60, 130, 180, 0.4);
  }

  @media (min-width: 768px) {
    .link-button {
      font-size: 1.2rem;
    }
  }

  .link-button:hover,
  .link-button:focus {
    background: linear-gradient(135deg, #4a8456, #2e5d4f);
    box-shadow: 0 0 20px 6px rgba(163, 196, 243, 0.6);
    transform: scale(1.05);
    outline: none;
  }

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
      {#each tripImages as src, i (src)}
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
      Every year during the last week of winter break, RIT Alpine Ski Club heads to Jay Peak Resort in Vermont 
      for our signature annual trip. This is the highlight of our season and an incredible opportunity to ski 
      some of the best terrain on the East Coast while bonding with fellow club members.
    </p>
    <p>
      <strong>Cost:</strong> The pricing for this trip is split into three parts: 
      <br> Club dues - $25 for Recreational or $50 for Racing, one or the other.
      <br> Initial deposit - $100. This secures your spot to give you time to make the next payment.
      <br> Final deposit - $380, with a tentative deadline of November 30th.
      <br> Overall: $505, assuming rec dues.
    </p>
    <p>
      <strong>What you get:</strong> The trip package includes five full days of lift tickets, lodging, and access to Jay Peak's legendary glades and terrain. Whether you're racing down 
      steep chutes or exploring tree runs, Jay Peak offers something for every skill level.
    </p>
    <p>
      <strong>The Nightlife:</strong> After long days on the slopes, we gather for unforgettable evenings 
      filled with team bonding, games, and memories that last a lifetime. This is where friendships are 
      forged and stories are made.
    </p>
    <p>
      <strong>Logistics:</strong> 
      <br>Transportation: We do our best to coordinate carpooling to the mountain. If you are in the New England area, there are good chances someone will be able to pick you up. If you are from farther, many people will meet at RIT a week early and several cars will head to the mountain from there. Bus transportation from the cabins to the mountain is included. 
      <br>Lodging: We have 5 cabins with 6 people each. We will make rooming assignments.
      <br>Food: We expect everyone to contribute equally, whether that is in cooking, buying food, or cleaning up. 
      <br>Timeline: We want everyone to have made their payments by November 30th, as we will be hosting a details meeting on December 1st to make plans. We will be staying in the cabins from the nights of January 4th through 8th, and skiing from the 5th through 9th. Optional day passes can be purchased separately on the 4th if you arrive early.
    </p>
    <p>
      <strong>Why You Should Join:</strong> This trip offers unbeatable value, amazing skiing, and the 
      chance to be part of something special. From beginners to experts, everyone finds their place on 
      this adventure. Don't miss out on what many members call the best trip of the year!
    </p>
    <p>
        Well, what do you think? Ready to try it out? There are: <b>11</b> spots left as of November 14th. Claim yours now! If you have any questions or concerns, please reach out to us in the <a href="https://discord.gg/HHuBQTKDwQ">discord</a> or to alpine@rit.edu!
    </p>
  </section>

  <div class="links-container">
    <div class="link-box">
      <h3>Jay Peak Resort</h3>
      <p>Learn more about the mountain, trails, and conditions</p>
      <a
        class="link-button"
        href="https://jaypeakresort.com"
        target="_blank"
        rel="noopener noreferrer"
      >
        View Website
      </a>
    </div>

    <div class="link-box">
      <h3>Slideshow</h3>
      <p>Presentation with a few of the finer details</p>
      <a
        class="link-button"
        href="https://docs.google.com/presentation/d/19RK-Tfv78vYm9NCPSv2cIm8J_HPMiijRWXeWv21kW6g/edit?usp=sharing"
        target="_blank"
        rel="noopener noreferrer"
      >
        View Slideshow
      </a>
    </div>

    <div class="link-box">
      <h3>Store Link</h3>
      <p>CampusGroups Store for Jay Peak payments</p>
      <a
        class="link-button"
        href="https://campusgroups.rit.edu/store?store_id=4564"
        target="_blank"
        rel="noopener noreferrer"
      >
        Visit Website
      </a>
    </div>

    <div class="link-box">
      <h3>Dues Link</h3>
      <p>CampusGroups store for dues payments</p>
      <a
        class="link-button"
        href="https://campusgroups.rit.edu/store?store_id=4036"
        target="_blank"
        rel="noopener noreferrer"
      >
        Visit Website
      </a>
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