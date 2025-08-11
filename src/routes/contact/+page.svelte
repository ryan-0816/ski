<script>
    import emailjs from 'emailjs-com';
  
    let name = '';
    let email = '';
    let message = '';
    let status = '';
  
    const SERVICE_ID = 'service_jkq5b3u';
    const TEMPLATE_ID = 'template_pkf9tia';
    const PUBLIC_KEY = 'Rck1sjqBH0dNeF-aW';
  
    const sendEmail = async () => {
      if (!name || !email || !message) {
        status = "Please fill in all fields.";
        return;
      }
  
      try {
        await emailjs.send(
          SERVICE_ID,
          TEMPLATE_ID,
          { from_name: name, from_email: email, message: message },
          PUBLIC_KEY
        );
        status = "✅ Message sent successfully!";
        name = email = message = '';
      } catch (error) {
        console.error("Email send error:", error);
        status = "❌ Failed to send message. Try again.";
      }
    };
  </script>
  
  <div class="contact-container">
    <h1>Connect with us!</h1>
    <h2>Instagram: link</h2>
    <h2><a href="https://campusgroups.rit.edu/cct" target="_blank" rel="noopener noreferrer">CampusGroups</a></h2>
    <h2>📧 Feel free to reach out here!</h2>
    <input type="text" placeholder="Name (optional)" bind:value={name} />
    <input type="email" placeholder="Contact (optional)" bind:value={email} />
    <textarea placeholder="Your message..." bind:value={message}></textarea>
    <button on:click={sendEmail}>Send! ➤📤📨⌯⌲</button>
    {#if status}
      <p class="status">{status}</p>
    {/if}
  </div>
  
  <style>
    .contact-container {
      max-width: 400px;
      margin: auto;
      padding: 2rem;
      background: #fff;
      border-radius: 12px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.1);
      display: flex;
      flex-direction: column;
      align-items: center; /* Center fields inside the box */
      gap: 1rem;
      font-family: system-ui, sans-serif;
    }
  
    h2 {
      text-align: center;
      margin-bottom: 1rem;
    }
  
    input, textarea {
      padding: 0.8rem;
      border-radius: 8px;
      border: 1px solid #ccc;
      font-size: 1rem;
      width: 90%; /* Centered but with margin on sides */
      display: block;
    }
  
    textarea {
      resize: none;
      height: 100px;
    }
  
    button {
      background: #1a73e8;
      color: white;
      padding: 0.8rem;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      font-size: 1rem;
      transition: background 0.2s;
      width: 90%; /* Matches input width */
    }
  
    button:hover {
      background: #155ab6;
    }
  
    .status {
      text-align: center;
      font-weight: bold;
    }
  </style>
  