<svelte:head>
    <title>Contact Us - RIT Ski Team</title>
    <meta name="description" content="Get in touch with the RIT Ski Team" />
</svelte:head>

<svelte:options accessors={true} />

<script lang="ts">
    import { onMount } from 'svelte';
    
    let name: string = '';
    let email: string = '';
    let message: string = '';
    let status: string = '';
    let isSending: boolean = false;
    let emailJSReady: boolean = false;
    let lastSubmissionTime: number = 0;
    
    // Image error states
    let imageErrors = {
        insta: false,
        campusgroups: false,
        discord: false
    };
    
    // Email configuration - using environment variables for security
    const PUBLIC_KEY: string = import.meta.env?.VITE_EMAILJS_PUBLIC_KEY || 'Rck1sjqBH0dNeF-aW';
    const SERVICE_ID: string = import.meta.env?.VITE_EMAILJS_SERVICE_ID || 'service_jkq5b3u';
    const TEMPLATE_ID: string = import.meta.env?.VITE_EMAILJS_TEMPLATE_ID || 'template_i9ke89m';
    
    // TypeScript fix: declare emailjs on window
    declare global {
        interface Window {
            emailjs: any;
        }
    }
    
    onMount(() => {
        // Load EmailJS dynamically and wait for it
        const script = document.createElement('script');
        script.src = 'https://cdn.jsdelivr.net/npm/@emailjs/browser@3/dist/email.min.js';
        script.async = true;
        
        script.onload = () => {
            if (window.emailjs) {
                window.emailjs.init(PUBLIC_KEY);
                emailJSReady = true;
                console.log('EmailJS initialized successfully');
            }
        };
        
        script.onerror = () => {
            console.error('Failed to load EmailJS');
            status = 'Email service unavailable';
        };
        
        document.head.appendChild(script);
        
        return () => {
            // Cleanup: remove script on component destroy
            if (script.parentNode) {
                script.parentNode.removeChild(script);
            }
        };
    });
    
    function validateForm(): boolean {
        if (!message.trim()) {
            status = 'Please write your message';
            return false;
        }
        
        if (email && !/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)) {
            status = 'Please enter a valid email address';
            return false;
        }
        
        return true;
    }
    
    async function sendEmail(event?: Event) {
        if (event) {
            event.preventDefault();
        }
        
        if (isSending) return;
        
        // Rate limiting - 5 second cooldown
        const now = Date.now();
        if (now - lastSubmissionTime < 5000) {
            status = 'Please wait a moment before sending another message';
            return;
        }
        
        if (!validateForm()) return;
        
        if (!emailJSReady || !window.emailjs) {
            status = 'Email service not ready. Please wait a moment and try again.';
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
            
            const response = await window.emailjs.send(SERVICE_ID, TEMPLATE_ID, templateParams);
            console.log('Email sent successfully:', response);
            
            status = '✅ Message sent successfully!';
            name = '';
            email = '';
            message = '';
            lastSubmissionTime = Date.now();
            
            // Clear status after 5 seconds
            setTimeout(() => {
                status = '';
            }, 5000);
            
        } catch (error: any) {
            console.error('Email send error:', error);
            
            if (error?.text) {
                console.error('Error details:', error.text);
            }
            
            if (error?.text?.includes('public key') || error?.text?.includes('Public Key')) {
                status = '❌ Invalid email configuration. Check public key.';
            } else if (error?.status === 400) {
                status = '❌ Invalid email configuration';
            } else if (error?.status === 401 || error?.status === 403) {
                status = '❌ Email service unauthorized';
            } else {
                status = '❌ Failed to send message. Please try again.';
            }
        } finally {
            isSending = false;
        }
    }
    
    function handleEnterKey(event: KeyboardEvent): void {
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
        
        <form class="contact-form" on:submit|preventDefault={sendEmail}>
            <input 
                id="name"
                type="text" 
                placeholder="Name (optional)" 
                bind:value={name} 
            />
            <input 
                id="email"
                type="email" 
                placeholder="Contact (optional)" 
                bind:value={email} 
            />
            <textarea 
                id="message"
                placeholder="Your message..." 
                bind:value={message} 
                on:keydown={handleEnterKey}
            ></textarea>
            
            <button type="submit" disabled={isSending || !emailJSReady}>
                {#if isSending}
                    Sending...
                {:else if !emailJSReady}
                    Loading Email Service...
                {:else}
                    Send! ➤📤📨⌯⌲
                {/if}
            </button>
        </form>
        
        <p class="status">{status}</p>
    </div>
</div>