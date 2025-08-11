<script>
  import { onMount } from 'svelte';

  let images = [
    "/image.jpg",
    "/image1.jpg",
    "/image2.jpg",
    "/image3.jpg"
  ];

  let current = 0;
  let total = images.length;

  onMount(() => {
    setInterval(() => {
      current = (current + 1) % total;
    }, 4000);
  });

  // You can replace this with the actual QR code image path
  let qrCodeSrc = "/qr-code.png";
</script>

<style>
  body {
    margin: 0;
    height: 100vh;
    background: linear-gradient(135deg, #d9f0ff 0%, #a3c4f3 70%);
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    color: #2a423d;
  }
  .welcome-box, .signup-box {
    background-color: #e6f0e8;
    border: 3px solid #5a6f54;
    padding: 3rem 4rem;
    border-radius: 20px;
    box-shadow: 0 0 20px 5px rgba(60, 130, 180, 0.5);
    text-align: center;
    max-width: 420px;
    margin-bottom: 2rem;
  }
  h1 {
    margin: 0;
    font-size: 3rem;
    font-weight: 900;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: #264d40;
    text-shadow:
      0 0 6px #a3c4f3aa,
      0 0 12px #a3c4f344;
  }
  a.signup-link {
    display: inline-block;
    margin-bottom: 1.5rem;
    font-weight: 700;
    font-size: 1.2rem;
    color: #3b6e47;
    text-decoration: none;
    border: 2px solid #3b6e47;
    padding: 0.6rem 1.2rem;
    border-radius: 12px;
    transition: background-color 0.3s ease, color 0.3s ease;
  }
  a.signup-link:hover,
  a.signup-link:focus {
    background-color: #3b6e47;
    color: #e6f0e8;
    outline: none;
  }
  .carousel {
    position: relative;
    width: 420px;
    height: 270px;
    border-radius: 20px;
    overflow: hidden;
    border: 3px solid #5a6f54;
    box-shadow: 0 0 20px 5px rgba(60, 130, 180, 0.3);
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
  .qr-container {
    max-width: 180px;
    margin: 0 auto;
  }
  .qr-container img {
    width: 100%;
    height: auto;
    border-radius: 15px;
    box-shadow: 0 0 15px 3px rgba(60, 130, 180, 0.4);
  }
</style>

<div class="welcome-box" role="banner">
  <h1>Welcome to RIT Alpine Ski Club!</h1>
</div>

<div class="carousel" aria-label="Ski Club photo carousel">
  {#each images as src, i}
    <img src={src} alt={`Ski Club image ${i + 1}`} class={i === current ? 'active' : ''} />
  {/each}
</div>

<!-- New signup box with QR code -->
<div class="signup-box" role="region" aria-labelledby="signup-heading">
  <h1 id="signup-heading">Join here!</h1>
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
