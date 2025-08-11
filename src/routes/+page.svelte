<script>
  import { onMount } from 'svelte';
  import { writable } from 'svelte/store';

  let images = [
    "/image.jpg",
    "/image1.jpg",
    "/image2.jpg",
    "/image3.jpg"
  ];

  let logoImage = "/logo.jpg";

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
  const password = "alpine24";

  function copyPassword() {
    navigator.clipboard.writeText(password).catch(() => {});
  }
</script>

<style>
  body, html {
    margin: 0;
    padding: 0;
    width: 100%;
    overflow-x: hidden;
  }

  main {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 3rem 0 6rem;
    width: 100%;
    max-width: 100vw;
    box-sizing: border-box;
  }

  .welcome-box {
    background-color: #e6f0e8;
    border: 4px solid #5a6f54;
    padding: 3rem 2rem;
    border-radius: 25px;
    box-shadow: 0 0 30px 8px rgba(60, 130, 180, 0.5);
    text-align: center;
    width: 1000px;
    max-width: 95vw;
    margin-bottom: 3rem;
    box-sizing: border-box;
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

  /* Make content container a flex container with stretch alignment */
  .content-container {
    width: 1000px;
    max-width: 95vw;
    display: flex;
    gap: 2rem;
    margin-bottom: 3rem;

    /* add this so children stretch equally in height */
    align-items: stretch;
  }

  .left-column {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 2rem;

    /* Make left-column stretch children */
    justify-content: flex-start;
  }

  .carousel {
    height: 380px;
    border-radius: 25px;
    overflow: hidden;
    border: 4px solid #5a6f54;
    box-shadow: 0 0 30px 6px rgba(60, 130, 180, 0.3);
    background: #f9fbf9;
    position: relative;
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
  }

  .carousel img.active {
    opacity: 1;
    z-index: 1;
  }

  /* Stretch logo box to fill the height of about-us */
  .logo-box {
    border-radius: 25px;
    overflow: hidden;
    border: 4px solid #5a6f54;
    box-shadow: 0 0 30px 6px rgba(60, 130, 180, 0.3);
    background: #f9fbf9;

    /* Stretch to fill height */
    flex-grow: 1;
    height: auto;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .logo-box img {
    width: 100%;
    height: 100%;
    object-fit: contain;
  }

  .about-us {
    flex: 1;
    background: linear-gradient(135deg, #cde3f7, #a0b8d9);
    border: 4px solid #5a6f54;
    border-radius: 25px;
    padding: 2rem;
    box-shadow: 0 0 30px 6px rgba(90, 111, 84, 0.3);
    color: #213544;
    font-size: 1.3rem;
    line-height: 1.6;
    user-select: text;
    overflow-y: auto;
    min-height: 600px;
  }

  .signup-box {
    background: linear-gradient(135deg, #3b6e47, #264d40);
    border-radius: 25px;
    box-shadow: 0 0 40px 10px rgba(38, 77, 64, 0.9);
    padding: 3rem 2rem 5rem;
    text-align: center;
    width: 1000px;
    max-width: 95vw;
    color: #d9f0ff;
    position: relative;
    overflow: visible;
    box-sizing: border-box;
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

  <div class="content-container">
    <div class="left-column">
      <div class="carousel" aria-label="Ski Club photo carousel">
        {#each images as src, i}
          <img src={src} alt={`Ski Club image ${i + 1}`} class={i === current ? 'active' : ''} />
        {/each}
      </div>
      <div class="logo-box" aria-label="Club logo">
        <img src={logoImage} alt="RIT Alpine Ski Club logo" />
      </div>
    </div>

    <section class="about-us" aria-labelledby="about-heading" tabindex="0">
      <p>
        RIT Alpine Ski Club! Operating during spring semester, we have two halves: recreation and race. Recreation hosts 
        trips locally, such as to Swain Resort or Bristol Mountain, on a weekly basis. Joining the team gets you access
        to those group discounts, carpool/transportation, and other things. The race team competes against nearby colleges
        in both slalom and giant slalom, frequenting resorts such as Labrador Mountain, Greek Peak, Bristol Resort, and West 
        Mountain. They also train two days a week at Swain Resort. The ski team also hosts an annual trip to Jay Peak on the 
        last week of winter break. This is an insane package consisting of several days of amazing skiing and very fun nights for an 
        excellent deal. If you enjoy or would like to try skiing, please reach out and see what we 
        can set up! Any level of skiing is welcomed!
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

    <div style="margin-top: 1rem; font-size: 1.1rem; color: #d9f0ff; display: flex; align-items: center; justify-content: center; gap: 0.75rem; user-select: text;">
      <span><strong>Password:</strong> alpine24</span>
      <button
        on:click={copyPassword}
        style="
          background: #d9f0ff;
          border: none;
          border-radius: 12px;
          padding: 0.3rem 0.8rem;
          font-weight: 700;
          color: #264d40;
          cursor: pointer;
          box-shadow: 0 0 6px #a3c4f3cc;
          transition: background-color 0.3s ease;
          display: flex;
          align-items: center;
          justify-content: center;
        "
        on:mouseover={e => e.currentTarget.style.backgroundColor = '#bcd8ff'}
        on:mouseout={e => e.currentTarget.style.backgroundColor = '#d9f0ff'}
        aria-label="Copy password alpine 24 to clipboard"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="18"
          height="18"
          fill="none"
          stroke="#264d40"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
          viewBox="0 0 24 24"
          aria-hidden="true"
        >
          <rect x="9" y="9" width="13" height="13" rx="2" ry="2"></rect>
          <rect x="2" y="2" width="13" height="13" rx="2" ry="2"></rect>
        </svg>
      </button>
    </div>

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
