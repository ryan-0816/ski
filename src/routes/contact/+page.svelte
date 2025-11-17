<script lang="ts">
    import { onMount } from 'svelte';
    import { browser } from '$app/environment';
    
    let name: string = '';
    let email: string = '';
    let message: string = '';
    let status: string = '';
    let isSending: boolean = false;
    let lastSendTime: number = 0;
    
    // Image error states
    let imageErrors = {
        insta: false,
        campusgroups: false,
        discord: false
    };
    
    // Email configuration - hardcoded keys
    const PUBLIC_KEY: string = 'Rck1sjqBH0dNeF-aW';
    const SERVICE_ID: string = 'service_jkq5b3u';
    const TEMPLATE_ID: string = 'template_i9ke89m';
    const COOLDOWN_MS: number = 60000; // 60 seconds between sends
    
    onMount(() => {
        if (!browser) return;
        
        // Load EmailJS dynamically
        const script = document.createElement('script');
        script.src = 'https://cdn.jsdelivr.net/npm/@emailjs/browser@3/dist/email.min.js';
        script.async = true;
        
        script.onload = () => {
            if (window.emailjs) {
                window.emailjs.init(PUBLIC_KEY);
                console.log('EmailJS initialized successfully');
            }
        };
        
        script.onerror = () => {
            console.error('Failed to load EmailJS');
            status = 'Email service unavailable';
        };
        
        document.head.appendChild(script);
        
        return () => {
            if (script.parentNode) {
                script.parentNode.removeChild(script);
            }
        };
    });
    
    const sendEmail = async () => {
        const now = Date.now();
        
        // Cooldown check
        if (now - lastSendTime < COOLDOWN_MS) {
            status = '⏳ Please wait a few seconds before sending again.';
            return;
        }
        
        // Only message is required
        if (!message.trim()) {
            status = 'Please write your message.';
            return;
        }
        
        if (!window.emailjs) {
            status = 'Email service not ready. Please try again.';
            return;
        }
        
        isSending = true;
        status = '📨 Sending...';
        
        try {
            await window.emailjs.send(
                SERVICE_ID,
                TEMPLATE_ID,
                {
                    from_name: name.trim() || 'Anonymous',
                    from_email: email.trim() || 'no-email-provided',
                    message: message.trim()
                }
            );
            
            status = '✅ Message sent successfully!';
            name = '';
            email = '';
            message = '';
            lastSendTime = now;
            
        } catch (error: any) {
            console.error('Email send error:', error);
            status = '❌ Failed to send message. Try again.';
        } finally {
            isSending = false;
        }
    };
</script>

<svelte:head>
    <title>Contact Us - RIT Ski Team</title>
    <meta name="description" content="Get in touch with the RIT Ski Team" />
</svelte:head>

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

    .contact-form {
        width: 100%;
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 1rem;
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

    /* Screen reader only text for accessibility */
    .sr-only {
        position: absolute;
        width: 1px;
        height: 1px;
        padding: 0;
        margin: -1px;
        overflow: hidden;
        clip: rect(0, 0, 0, 0);
        white-space: nowrap;
        border: 0;
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
                <img 
                    src={imageErrors.insta ? '/fallback-social.jpg' : '/insta.jpg'} 
                    alt="Instagram logo" 
                    class="social-logo instagram-logo" 
                    on:error={() => imageErrors.insta = true}
                />
            </a>
            
            <!-- CampusGroups -->
            <a href="https://campusgroups.rit.edu/AlpineSki"
               target="_blank" rel="noopener noreferrer" aria-label="CampusGroups">
                <img 
                    src={imageErrors.campusgroups ? '/fallback-social.jpg' : '/campusgroups.png'} 
                    alt="CampusGroups logo" 
                    class="social-logo campusgroups-logo" 
                    on:error={() => imageErrors.campusgroups = true}
                />
            </a>
            
            <!-- Discord -->
            <a href="https://discord.gg/HHuBQTKDwQ"
               target="_blank" rel="noopener noreferrer" aria-label="Discord">
                <img 
                    src={imageErrors.discord ? '/fallback-social.jpg' : '/discordlogo.jpg'} 
                    alt="Discord logo" 
                    class="social-logo discord-logo" 
                    on:error={() => imageErrors.discord = true}
                />
            </a>
        </div>

        <p class="subheading">📧 Feel free to reach out here!</p>
        
        <!-- Hidden labels for accessibility -->
        <div class="sr-only">
            <label for="name">Name</label>
            <label for="email">Email</label>
            <label for="message">Message</label>
        </div>
        
        <div class="contact-form">
            <input 
                id="name"
                type="text" 
                placeholder="Name (optional)" 
                bind:value={name} 
            />
            <input 
                id="email"
                type="text" 
                placeholder="Contact (optional)" 
                bind:value={email} 
            />
            <textarea 
                id="message"
                placeholder="Your message..." 
                bind:value={message}
            ></textarea>
            
            <button on:click={sendEmail} disabled={isSending}>
                {isSending ? 'Sending...' : 'Send! ➤📤📨⌯⌲'}
            </button>
        </div>
        
        {#if status}
            <p class="status">{status}</p>
        {/if}
    </div>
</div>