<svelte:head>
    <title>Contact Us - RIT Ski Team</title>
    <meta name="description" content="Get in touch with the RIT Ski Team" />
</svelte:head>

<script>
    import { onMount } from 'svelte';
    
    let name = '';
    let email = '';
    let message = '';
    let status = '';
    let isSending = false;
    
    // Email configuration - UPDATE THESE!
    const PUBLIC_KEY = 'Rck1sjqBH0dNeF-aW'; // Get from EmailJS dashboard
    const SERVICE_ID = 'service_jkq5b3u';
    const TEMPLATE_ID = 'template_i9ke89m';
    
    onMount(async () => {
        // Load EmailJS
        try {
            const emailjs = await import('@emailjs/browser');
            window.emailjs = emailjs;
            emailjs.init(PUBLIC_KEY);
            console.log('EmailJS initialized');
        } catch (error) {
            console.error('Failed to load EmailJS:', error);
            status = 'Email service unavailable';
        }
    });
    
    async function sendEmail() {
        if (isSending) return;
        
        if (!message.trim()) {
            status = 'Please write your message';
            return;
        }
        
        if (!window.emailjs) {
            status = 'Email service not loaded yet';
            return;
        }
        
        isSending = true;
        status = '📨 Sending...';
        
        try {
            const templateParams = {
                from_name: name.trim() || 'Anonymous',
                from_email: email.trim() || 'no-email-provided',
                message: message.trim(),
                reply_to: email.trim() || ''
            };
            
            await window.emailjs.send(SERVICE_ID, TEMPLATE_ID, templateParams);
            
            status = '✅ Message sent successfully!';
            name = '';
            email = '';
            message = '';
            
        } catch (error) {
            console.error('Email send error:', error);
            
            if (error?.message?.includes('public key')) {
                status = '❌ Invalid email configuration. Check public key.';
            } else if (error?.status === 400) {
                status = '❌ Invalid email configuration';
            } else if (error?.status === 401) {
                status = '❌ Email service unauthorized';
            } else {
                status = '❌ Failed to send message';
            }
        } finally {
            isSending = false;
        }
    }
    
    function handleEnterKey(event) {
        if (event.key === 'Enter' && !event.shiftKey) {
            event.preventDefault();
            sendEmail();
        }
    }
</script>

<style>
    :global(body) {
        margin: 0;
        padding: 0;
        width: 100%;
        overflow-x: hidden;
        -webkit-text-size-adjust: 100%;
        font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
        background: #f8f9fa;
    }

    .page-container {
        width: 100%;
        max-width: 1000px;
        margin: 0 auto;
        padding: 1rem;
        box-sizing: border-box;
        display: flex;
        justify-content: center;
        align-items: center;
        min-height: 100vh;
    }

    .contact-container {
        width: 100%;
        max-width: 500px;
        margin: auto;
        padding: 2.5rem;
        background: #fff;
        border-radius: 16px;
        box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 1.2rem;
        text-align: center;
    }

    h1 {
        text-align: center;
        font-size: 2rem;
        margin: 1rem 0 1.5rem;
        color: #264d40;
    }

    .social-container {
        display: flex;
        justify-content: center;
        gap: 1.8rem;
        margin: 0.5rem 0 1rem 0;
    }

    .social-logo {
        height: 42px;
        width: 42px;
        transition: all 0.3s ease;
        border-radius: 50%;
        padding: 8px;
        background: #f8f9fa;
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
    }

    .social-logo:hover {
        transform: translateY(-3px);
        box-shadow: 0 6px 12px rgba(0, 0, 0, 0.1);
    }

    .instagram-logo:hover {
        background: #fdf4f7;
    }

    .campusgroups-logo:hover {
        background: #f0f8ff;
    }

    .discord-logo:hover {
        background: #f0f4ff;
    }

    input, textarea {
        padding: 0.9rem;
        border-radius: 10px;
        border: 1px solid #ddd;
        font-size: 1rem;
        width: 85%;
        display: block;
        transition: border 0.3s ease;
        box-sizing: border-box;
        font-family: inherit;
    }

    input:focus, textarea:focus {
        outline: none;
        border-color: #1a73e8;
        box-shadow: 0 0 0 2px rgba(26, 115, 232, 0.2);
    }

    textarea {
        resize: none;
        height: 110px;
    }

    button {
        background: #1a73e8;
        color: white;
        padding: 0.9rem;
        border: none;
        border-radius: 10px;
        cursor: pointer;
        font-size: 1rem;
        font-weight: 600;
        transition: all 0.2s;
        width: 85%;
        margin-top: 0.5rem;
        font-family: inherit;
    }

    button:disabled {
        background: #a0aec0;
        cursor: not-allowed;
        transform: none;
    }

    button:hover:not(:disabled) {
        background: #155ab6;
        transform: translateY(-2px);
        box-shadow: 0 4px 8px rgba(26, 115, 232, 0.3);
    }

    .status {
        text-align: center;
        font-weight: 500;
        min-height: 24px;
        margin: 0.5rem 0 0 0;
        color: #333;
    }

    .subheading {
        color: #4a5568;
        margin: 0.5rem 0 1rem 0;
        font-weight: 500;
    }

    @media (min-width: 768px) {
        h1 {
            font-size: 2.5rem;
            margin: 1.5rem 0 2rem;
        }

        .page-container {
            padding: 1.5rem;
        }
    }

    @media (max-width: 480px) {
        .contact-container {
            padding: 1.5rem;
            margin: 1rem;
        }

        input, textarea, button {
            width: 95%;
        }

        .social-container {
            gap: 1rem;
        }

        .social-logo {
            height: 36px;
            width: 36px;
            padding: 6px;
        }
    }
</style>

<div class="page-container">
    <div class="contact-container">
        <h1>Connect with us!</h1>
        
        <div class="social-container">
            <!-- Instagram -->
            <a href="https://www.instagram.com/officialritskiteam/"
               target="_blank" rel="noopener noreferrer" aria-label="Instagram">
                <img src="/insta.jpg" alt="Instagram logo" class="social-logo instagram-logo" />
            </a>
            
            <!-- CampusGroups -->
            <a href="https://campusgroups.rit.edu/AlpineSki"
               target="_blank" rel="noopener noreferrer" aria-label="CampusGroups">
                <img src="/campusgroups.png" alt="CampusGroups logo" class="social-logo campusgroups-logo" />
            </a>
            
            <!-- Discord -->
            <a href="https://discord.gg/HHuBQTKDwQ"
               target="_blank" rel="noopener noreferrer" aria-label="Discord">
                <img src="/discordlogo.jpg" alt="Discord logo" class="social-logo discord-logo" />
            </a>
        </div>

        <p class="subheading">📧 Feel free to reach out here!</p>
        
        <input type="text" placeholder="Name (optional)" bind:value={name} />
        <input type="email" placeholder="Contact (optional)" bind:value={email} />
        <textarea placeholder="Your message..." bind:value={message} on:keydown={handleEnterKey}></textarea>
        
        <button on:click={sendEmail} disabled={isSending}>
            {#if isSending}Sending...{:else}Send! ➤📤📨⌯⌲{/if}
        </button>
        
        <p class="status">{status}</p>
    </div>
</div>