<script>
  import { onMount } from 'svelte';
  import { writable } from 'svelte/store';

  let images = [
    "/image.jpg",
    "/image1.jpg",
    "/image2.jpg",
    "/image3.jpg"
  ];

  let current = 0;
  let total = images.length;

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

  onMount(() => {
    const interval = setInterval(() => {
      current = (current + 1) % total;
    }, 4000);

    window.addEventListener('scroll', handleScroll);

    return () => {
      clearInterval(interval);
      window.removeEventListener('scroll', handleScroll);
    };
  });

  let qrCodeSrc = "/qr-code.png";
</script>

<style>
  body {
    margin: 0;
    min-height: 100vh;
    background: linear-gradient(135deg, #d9f0ff 0%, #a3c4f3 70%);
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    color: #2a423d;
  }

  main {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 3rem 1rem 6rem;
  }

  .welcome-box {
    background-color: #e6f0e8;
    border: 4px solid #5a6f54;
    padding: 4rem 3rem;
    border-radius: 25px;
    box-shadow: 0 0 30px 8px rgba(60, 130, 180, 0.5);
    text-align: center;
    max-width: 95vw;
    width: 1000px;
    margin-bottom: 3rem;
  }
  .welcome-box h1 {
    margin: 0;
    font-size: 4rem;
    font-weight: 900;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: #264d40;
    text-shadow:
      0 0 8px #a3c4f3bb,
      0 0 16px #a3c4f355;
  }

  .carousel-about-wrapper {
    display: flex;
    gap: 3.5rem;
    max-width: 1000px;
    width: 95vw;
    margin-bottom: 4rem;
    align-items: stretch;
  }

  .carousel {
    position: relative;
    flex: 1 1 480px;
    height: auto;
    min-height: 380px;
    border-radius: 25px;
    overflow: hidden;
    border: 4px solid #5a6f54;
    box-shadow: 0 0 30px 6px rgba(60, 130, 180, 0.3);
    background: #f9fbf9;
  }
  .carousel img {
    position: absolute;
    width: 100%;
    height: 100%;
    object-fit: cover;
    opacity: 0;
    transition: opacity 1.2s ease-in-out;
  }
  .carousel img.active {
    opacity: 1;
    z-index: 1;
  }

  /* Updated About Us box colors */
  .about-us {
    flex: 1 1 520px;
    background: linear-gradient(135deg, #cde3f7, #a0b8d9);
    border: 4px solid #5a6f54;
    border-radius: 25px;
    padding: 3rem 2.5rem;
    box-shadow: 0 0 30px 6px rgba(90, 111, 84, 0.3);
    color: #213544;
    font-size: 1.3rem;
    line-height: 1.6;
    user-select: text;
    overflow-y: auto;
    min-height: 380px;
  }

  .signup-box {
    background: linear-gradient(135deg, #3b6e47, #264d40);
    border-radius: 25px;
    box-shadow: 0 0 40px 10px rgba(38, 77, 64, 0.9);
    padding: 3.5rem 2rem 5rem;
    text-align: center;
    max-width: 95vw;
    width: 1000px;
    color: #d9f0ff;
    position: relative;
    overflow: visible;
  }
  .signup-box h1 {
    font-size: 3.5rem;
    margin-bottom: 2rem;
    font-weight: 900;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    text-shadow: 0 0 10px #a3c4f3dd;
  }
  .signup-link {
    display: inline-block;
    font-weight: 700;
    font-size: 1.5rem;
    color: #d9f0ff;
    text-decoration: none;
    border: 3px solid #d9f0ff;
    padding: 0.9rem 2rem;
    border-radius: 18px;
    transition: background-color 0.35s ease, color 0.35s ease, box-shadow 0.35s ease;
    box-shadow: 0 0 12px 3px #a3c4f3aa;
  }
  .signup-link:hover,
  .signup-link:focus {
    background-color: #d9f0ff;
    color: #264d40;
    box-shadow: 0 0 20px 6px #ffffffcc;
    outline: none;
  }

  .qr-container {
    max-width: 220px;
    margin: 3rem auto 0;
    border-radius: 25px;
    box-shadow: 0 0 20px 5px #a3c4f3cc;
    overflow: hidden;
  }
  .qr-container img {
    width: 100%;
    height: auto;
    display: block;
  }

  /* Scroll hint black style */
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
  <div class="welcome-box" role="banner">
    <h1>Welcome to RIT Alpine Ski Club!</h1>
  </div>

  <div class="carousel-about-wrapper">
    <div class="carousel" aria-label="Ski Club photo carousel">
      {#each images as src, i}
        <img src={src} alt={`Ski Club image ${i + 1}`} class={i === current ? 'active' : ''} />
      {/each}
    </div>

    <section class="about-us" aria-labelledby="about-heading" tabindex="0">
      <h2 id="about-heading" style="margin-top:0;">About Us</h2>
      <p>
        <!-- Your engaging story, club mission, and vibe go here. -->
      </p>
    </section>
  </div>

  <div class="signup-box" role="region" aria-labelledby="signup-heading">
    <h1 id="signup-heading">Ready to ski?</h1>
    <a
      class="signup-link"
      href="https://campusgroups.rit.edu/AlpineSki/club_signup"
      target="_blank"
      rel="noopener noreferrer"
    >
      Sign Up Here
    </a>
    <div class="qr-container" aria-label="QR code to join RIT Alpine Ski Club">
      <img src={"/qrcode.jpg"} alt="QR code for RIT Alpine Ski Club signup" />
    </div>
  </div>

  {#if $showScrollHint}
    <div class="scroll-hint" aria-hidden="true">
      <span>Scroll down</span>
      <svg viewBox="0 0 24 24" >
        <polyline points="6 9 12 15 18 9" />
      </svg>
    </div>
  {/if}
</main>
