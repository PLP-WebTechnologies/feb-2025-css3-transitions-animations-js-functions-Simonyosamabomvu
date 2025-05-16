<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Web Page with Animations</title>
    <style>
        /* Base Styles */
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            line-height: 1.6;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            background-color: #f5f5f5;
        }

        h1 {
            color: #2c3e50;
            text-align: center;
        }

        section {
            background: white;
            border-radius: 8px;
            padding: 20px;
            margin-bottom: 20px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }

        /* Animation Styles */
        .animated-box {
            width: 100px;
            height: 100px;
            background-color: #3498db;
            margin: 20px auto;
            border-radius: 8px;
            transition: all 0.5s ease;
        }

        .animated-box:hover {
            transform: scale(1.2) rotate(15deg);
            background-color: #e74c3c;
        }

        @keyframes slideIn {
            from {
                transform: translateX(-100%);
                opacity: 0;
            }
            to {
                transform: translateX(0);
                opacity: 1;
            }
        }

        .slide-animation {
            animation: slideIn 1s ease-out forwards;
        }

        .pulse {
            animation: pulse 1.5s infinite;
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.1); }
            100% { transform: scale(1); }
        }

        /* Button Styles */
        button {
            background-color: #2ecc71;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
            transition: all 0.3s ease;
            margin: 5px;
        }

        button:hover {
            background-color: #27ae60;
            transform: translateY(-3px);
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        }

        /* Form Styles */
        .preference-form {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        label {
            font-weight: bold;
        }

        select, input {
            padding: 8px;
            border-radius: 4px;
            border: 1px solid #ddd;
        }
    </style>
</head>
<body>
    <h1>Interactive Web Page</h1>

    <section id="animation-section">
        <h2>CSS Animations</h2>
        <div class="animated-box" id="colorBox"></div>
        <button id="animateBtn">Trigger Animation</button>
        <button id="pulseBtn">Toggle Pulse Animation</button>
    </section>

    <section id="preferences-section">
        <h2>User Preferences</h2>
        <form class="preference-form" id="preferenceForm">
            <label for="theme">Select Theme:</label>
            <select id="theme" name="theme">
                <option value="light">Light</option>
                <option value="dark">Dark</option>
                <option value="blue">Blue</option>
            </select>

            <label for="username">Your Name:</label>
            <input type="text" id="username" name="username" placeholder="Enter your name">

            <button type="submit">Save Preferences</button>
        </form>
        <div id="preferenceDisplay"></div>
    </section>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // DOM Elements
            const colorBox = document.getElementById('colorBox');
            const animateBtn = document.getElementById('animateBtn');
            const pulseBtn = document.getElementById('pulseBtn');
            const preferenceForm = document.getElementById('preferenceForm');
            const preferenceDisplay = document.getElementById('preferenceDisplay');

            // Load saved preferences
            loadPreferences();

            // Animation Button Click
            animateBtn.addEventListener('click', function() {
                colorBox.classList.add('slide-animation');
                
                // Remove animation class after it completes to allow re-triggering
                setTimeout(() => {
                    colorBox.classList.remove('slide-animation');
                }, 1000);
            });

            // Pulse Animation Toggle
            pulseBtn.addEventListener('click', function() {
                colorBox.classList.toggle('pulse');
            });

            // Save Preferences
            preferenceForm.addEventListener('submit', function(e) {
                e.preventDefault();
                
                const theme = document.getElementById('theme').value;
                const username = document.getElementById('username').value.trim();
                
                // Save to localStorage
                localStorage.setItem('userTheme', theme);
                localStorage.setItem('username', username);
                
                // Update UI
                updateTheme(theme);
                displayPreferences();
                
                // Show confirmation animation
                const confirmation = document.createElement('div');
                confirmation.textContent = 'Preferences saved!';
                confirmation.style.animation = 'slideIn 0.5s ease-out';
                preferenceDisplay.appendChild(confirmation);
                
                setTimeout(() => {
                    confirmation.style.animation = 'fadeOut 0.5s ease-out forwards';
                    setTimeout(() => confirmation.remove(), 500);
                }, 2000);
            });

            // Function to load saved preferences
            function loadPreferences() {
                const savedTheme = localStorage.getItem('userTheme');
                const savedName = localStorage.getItem('username');
                
                if (savedTheme) {
                    document.getElementById('theme').value = savedTheme;
                    updateTheme(savedTheme);
                }
                
                if (savedName) {
                    document.getElementById('username').value = savedName;
                }
                
                displayPreferences();
            }

            // Function to update theme
            function updateTheme(theme) {
                document.body.className = ''; // Clear previous theme classes
                
                switch(theme) {
                    case 'dark':
                        document.body.classList.add('dark-theme');
                        break;
                    case 'blue':
                        document.body.classList.add('blue-theme');
                        break;
                    default:
                        // Light theme is default
                        break;
                }
            }

            // Function to display saved preferences
            function displayPreferences() {
                const savedTheme = localStorage.getItem('userTheme') || 'not set';
                const savedName = localStorage.getItem('username') || 'not set';
                
                preferenceDisplay.innerHTML = `
                    <h3>Current Preferences:</h3>
                    <p><strong>Theme:</strong> ${savedTheme}</p>
                    <p><strong>Name:</strong> ${savedName}</p>
                `;
            }

            // Initialize
            displayPreferences();
        });

        // Additional theme styles
        const style = document.createElement('style');
        style.textContent = `
            .dark-theme {
                background-color: #333;
                color: #fff;
            }
            .dark-theme section {
                background-color: #444;
                color: #fff;
            }
            .blue-theme {
                background-color: #e6f2ff;
            }
            .blue-theme section {
                background-color: #cce0ff;
            }
            @keyframes fadeOut {
                from { opacity: 1; }
                to { opacity: 0; }
            }
        `;
        document.head.appendChild(style);
    </script>
</body>
</html>
