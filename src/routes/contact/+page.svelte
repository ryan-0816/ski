<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>RIT Ski Team - Contact Us</title>
    <script src="https://cdn.jsdelivr.net/npm/@emailjs/browser@3/dist/email.min.js"></script>
    <style>
        body {
            margin: 0;
            padding: 0;
            width: 100%;
            overflow-x: hidden;
            -webkit-text-size-adjust: 100%;
            font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
        }

        .page-container {
            width: 100%;
            max-width: 1000px;
            margin: 0 auto;
            padding: 1rem;
            box-sizing: border-box;
        }

        h1 {
            text-align: center;
            font-size: 2rem;
            margin: 1rem 0 1.5rem;
            color: #264d40;
        }

        .contact-container {
            max-width: 450px;
            margin: 0 auto;
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
    </style>
</head>
<body>
    <div class="page-container">
        <h1>Connect with us!</h1>
        
        <div class="contact-container">
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
            
            <input type="text" placeholder="Name (optional)" id="name" />
            <input type="email" placeholder="Contact (optional)" id="email" />
            <textarea placeholder="Your message..." id="message"></textarea>
            
            <button id="send-button">Send! ➤📤📨⌯⌲</button>
            
            <p class="status" id="status"></p>
        </div>
    </div>

    <script>
        // Get DOM elements
        const nameInput = document.getElementById('name');
        const emailInput = document.getElementById('email');
        const messageInput = document.getElementById('message');
        const sendButton = document.getElementById('send-button');
        const statusEl = document.getElementById('status');

        // Email configuration - VERIFY THESE IN YOUR EMAILJS DASHBOARD
        const PUBLIC_KEY = 'Rck1sjqBH0dNeF-aW'; // Replace with your actual public key
        const SERVICE_ID = 'service_jkq5b3u';    // Replace with your actual service ID
        const TEMPLATE_ID = 'template_i9ke89m';  // Replace with your actual template ID
        const COOLDOWN_MS = 60000;

        let lastSendTime = 0;
        let isSending = false;

        // Initialize EmailJS with error handling
        function initializeEmailJS() {
            try {
                if (typeof emailjs === 'undefined') {
                    statusEl.textContent = "❌ EmailJS library failed to load";
                    return false;
                }
                
                emailjs.init(PUBLIC_KEY);
                console.log("EmailJS initialized");
                return true;
            } catch (error) {
                console.error("EmailJS init error:", error);
                statusEl.textContent = "❌ EmailJS initialization failed";
                return false;
            }
        }

        // Initialize when page loads
        document.addEventListener('DOMContentLoaded', function() {
            if (!initializeEmailJS()) {
                sendButton.disabled = true;
                sendButton.textContent = "Service Unavailable";
            }
        });

        // Send email function
        const sendEmail = async () => {
            // Check if EmailJS is available
            if (typeof emailjs === 'undefined') {
                statusEl.textContent = "❌ Email service not loaded";
                return;
            }

            const now = Date.now();
            if (now - lastSendTime < COOLDOWN_MS) {
                statusEl.textContent = `⏳ Please wait before sending again`;
                return;
            }

            const message = messageInput.value.trim();
            if (!message) {
                statusEl.textContent = "Please write your message";
                return;
            }

            isSending = true;
            sendButton.disabled = true;
            sendButton.textContent = "Sending...";
            statusEl.textContent = "📨 Sending...";

            try {
                const templateParams = {
                    from_name: nameInput.value.trim() || "Anonymous",
                    from_email: emailInput.value.trim() || "no-email-provided",
                    message: message,
                    reply_to: emailInput.value.trim() || "no-reply@example.com"
                };

                console.log("Sending email with params:", templateParams);
                
                const response = await emailjs.send(SERVICE_ID, TEMPLATE_ID, templateParams);
                console.log("Email sent successfully:", response);
                
                statusEl.textContent = "✅ Message sent successfully!";
                nameInput.value = '';
                emailInput.value = '';
                messageInput.value = '';
                lastSendTime = now;

            } catch (error) {
                console.error("Email send error details:", error);
                
                // More specific error messages
                if (error.status === 400) {
                    statusEl.textContent = "❌ Invalid email configuration";
                } else if (error.status === 401) {
                    statusEl.textContent = "❌ Email service unauthorized";
                } else if (error.status === 0) {
                    statusEl.textContent = "❌ Network error - check connection";
                } else {
                    statusEl.textContent = "❌ Failed to send message";
                }
            } finally {
                isSending = false;
                sendButton.disabled = false;
                sendButton.textContent = "Send! ➤📤📨⌯⌲";
            }
        };

        // Add event listener
        sendButton.addEventListener('click', sendEmail);

        // Allow Enter key to send message
        messageInput.addEventListener('keypress', function(e) {
            if (e.key === 'Enter' && !e.shiftKey) {
                e.preventDefault();
                sendEmail();
            }
        });
    </script>
</body>
</html>