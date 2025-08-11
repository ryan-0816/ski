<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Ski Club</title>
<style>
  body {
    margin: 0;
    height: 100vh;
    background: linear-gradient(135deg, #d9f0ff 0%, #a3c4f3 70%); /* frosty pale blue sky */
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    color: #2a423d; /* deep pine green */
  }
  .welcome-box {
    background-color: #e6f0e8; /* soft snow white */
    border: 3px solid #5a6f54; /* pine green dark */
    padding: 3rem 4rem;
    border-radius: 20px;
    box-shadow: 0 0 20px 5px rgba(60, 130, 180, 0.5); /* pale blue glow */
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
    color: #264d40; /* deep pine */
    text-shadow:
      0 0 6px #a3c4f3aa,
      0 0 12px #a3c4f344; /* subtle pale blue glow */
  }
  .carousel {
    position: relative;
    width: 420px;
    height: 270px;
    border-radius: 20px;
    overflow: hidden;
    border: 3px solid #5a6f54;
    box-shadow: 0 0 20px 5px rgba(60, 130, 180, 0.3);
    background: #f9fbf9; /* fresh snow */
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
</style>
</head>
<body>
  <div class="welcome-box" role="banner">
    <h1>Welcome to Ski Club</h1>
  </div>

  <div class="carousel" aria-label="Ski Club photo carousel">
    <img src="https://images.unsplash.com/photo-1512820790803-83ca734da794?auto=format&fit=crop&w=800&q=80" alt="Snowy mountain peak" class="active" />
    <img src="https://images.unsplash.com/photo-1506744038136-46273834b3fb?auto=format&fit=crop&w=800&q=80" alt="Skiers on a slope" />
    <img src="https://images.unsplash.com/photo-1483683804023-6ccdb62f86ef?auto=format&fit=crop&w=800&q=80" alt="Snow-covered trees" />
  </div>

<script>
  const images = document.querySelectorAll('.carousel img');
  let current = 0;
  const total = images.length;

  function rotateImages() {
    images[current].classList.remove('active');
    current = (current + 1) % total;
    images[current].classList.add('active');
  }

  setInterval(rotateImages, 4000);
</script>
</body>
</html>
