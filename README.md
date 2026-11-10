<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GARDEN // ACCESS CONTROL</title>
    <style>
        :root {
            --bg-color: #dce7f0;
            --panel-bg: rgba(255, 255, 255, 0.85);
            --slate-blue: #5b7c99;
            --slate-light: #7b9ebc;
            --slate-soft: #a8c4db;
            --slate-dark: #2b3d4f;
            --accent-cyan: #0284c7;
            --error-pink: #f43f5e;
            --text-main: #334155;
            --font-mono: 'Courier New', Courier, monospace;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-main);
            font-family: var(--font-mono);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            overflow-x: hidden;
            background-image: 
                radial-gradient(circle at 50% 40%, rgba(168, 196, 219, 0.5) 0%, transparent 70%),
                linear-gradient(rgba(123, 158, 188, 0.15) 1px, transparent 1px),
                linear-gradient(90deg, rgba(123, 158, 188, 0.15) 1px, transparent 1px);
            background-size: 100% 100%, 25px 25px, 25px 25px;
        }

        .container {
            width: 100%;
            max-width: 800px;
            padding: 20px;
        }

        /* Card Container */
        .card {
            background: var(--panel-bg);
            border: 2px solid var(--slate-soft);
            box-shadow: 0 15px 35px rgba(91, 124, 153, 0.18);
            padding: 40px 30px;
            text-align: center;
            backdrop-filter: blur(12px);
            border-radius: 20px;
            transition: all 0.5s ease;
        }

        h1 {
            font-size: 1.1rem;
            letter-spacing: 3px;
            color: var(--slate-dark);
            margin-bottom: 8px;
            font-weight: 700;
        }

        p.subtitle {
            font-size: 0.8rem;
            color: var(--slate-blue);
            margin-bottom: 25px;
            letter-spacing: 1px;
        }

        .input-group {
            position: relative;
            margin-bottom: 20px;
        }

        input[type="password"] {
            width: 100%;
            max-width: 250px;
            background: #ffffff;
            border: 2px solid var(--slate-light);
            color: var(--slate-dark);
            font-family: var(--font-mono);
            font-size: 1.5rem;
            padding: 10px 15px;
            text-align: center;
            letter-spacing: 10px;
            outline: none;
            transition: all 0.3s;
            border-radius: 12px;
        }

        input[type="password"]:focus {
            border-color: var(--accent-cyan);
            box-shadow: 0 0 12px rgba(2, 132, 199, 0.25);
        }

        button {
            background: var(--slate-blue);
            color: #ffffff;
            border: none;
            padding: 12px 35px;
            font-family: var(--font-mono);
            font-size: 0.85rem;
            font-weight: bold;
            letter-spacing: 2px;
            cursor: pointer;
            border-radius: 12px;
            transition: all 0.3s;
            box-shadow: 0 4px 12px rgba(91, 124, 153, 0.3);
        }

        button:hover {
            background: var(--slate-dark);
            transform: translateY(-2px);
        }

        .error-message {
            color: var(--error-pink);
            font-size: 0.85rem;
            margin-top: 15px;
            height: 20px;
            letter-spacing: 1px;
        }

        /* Flower Display Area */
        .art-container {
            display: none;
            text-align: center;
            opacity: 0;
            transition: opacity 0.8s ease-in;
            background: var(--panel-bg);
            border: 2px solid var(--slate-soft);
            box-shadow: 0 15px 35px rgba(91, 124, 153, 0.18);
            padding: 30px 20px;
            backdrop-filter: blur(12px);
            border-radius: 20px;
        }

        .art-header {
            color: var(--slate-dark);
            font-size: 0.85rem;
            letter-spacing: 2px;
            margin-bottom: 15px;
            font-weight: bold;
            border-bottom: 1px dashed var(--slate-soft);
            padding-bottom: 10px;
        }

        pre#flower-art {
            font-family: var(--font-mono);
            font-size: 10px;
            line-height: 1.1;
            color: var(--slate-dark);
            text-shadow: 0 0 3px rgba(91, 124, 153, 0.25);
            display: inline-block;
            text-align: left;
            white-space: pre;
            user-select: none;
        }

        /* Cute Shake Animation */
        .shake {
            animation: softShake 0.4s ease-in-out;
        }

        @keyframes softShake {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(-5px) rotate(-0.5deg); }
            75% { transform: translateX(5px) rotate(0.5deg); }
        }
    </style>
</head>
<body>

    <div class="container">
        <!-- Input Section -->
        <div class="card" id="login-card">
            <h1>✦ ENTER PASSCODE ✦</h1>
            <p class="subtitle">Please enter 6-digit access code</p>
            <div class="input-group">
                <input type="password" id="passcode" maxlength="6" placeholder="••••••" autocomplete="off">
            </div>
            <button onclick="checkPasscode()">UNLOCK ♡</button>
            <div class="error-message" id="error-msg"></div>
        </div>

        <!-- ASCII Flower Output Section -->
        <div class="art-container" id="art-card">
            <div class="art-header">✿ ACCESS GRANTED // BLOOM SEQUENCE ✿</div>
            <pre id="flower-art"></pre>
        </div>
    </div>

    <script>
        const CORRECT_CODE = "140224";

        // Your Custom ASCII Art
        const FLOWER_ART = `
██████████                                
                                    ██▓▓░░▒▒▒▒▓▓▓▓▓▓▓▓▓▓▒▒                        
                            ████████▓▓▒▒▓▓▓▓▓▓████▓▓▒▒░░██▓▓                      
                        ████▒▒▒▒██▒▒████▓▓██████▓▓██▓▓▒▒▒▒▓▓                      
                        ██░░▒▒██▓▓▓▓██▒▒██▒▒▒▒██▓▓▒▒██▓▓▒▒▓▓                      
                        ██▒▒▒▒██▒▒▓▓██▓▓████▓▓▓▓██▒▒▒▒██▓▓▓▓                      
                        ██▒▒▒▒▒▒██▓▓██▓▓▒▒██▓▓▓▓██▒▒▓▓▓▓▓▓▓▓                      
                        ████▒▒▓▓▓▓████▓▓▓▓▓▓██████▓▓▓▓██▓▓████▓▓                  
                          ████▒▒▒▒▓▓██▓▓▓▓▓▓██▓▓▓▓████▓▓▒▒▒▒▒▒▓▓                  
                              ██▒▒▒▒▓▓████▓▓▓▓████▒▒▒▒▒▒▒▒▒▒▒▒▓▓                  
                              ████▒▒▒▒▓▓██▓▓██▒▒▒▒▒▒░░██████▓▓                    
                              ██▓▓▓▓██▓▓▒▒██▒▒▒▒██████▓▓                          
                            ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▒▒▓▓▓▓▓▓▓▓▓▓▒▒                        
                            ██▓▓▒▒▓▓▓▓▓▓▒▒████▓▓▓▓▒▒▓▓▓▓▓▓                        
                            ████▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓                        
                            ░░██▓▓▓▓▓▓██▓▓▓▓▓▓▓▓▓▓▒▒▓▓▓▓░░▒▒▒▒                    
                              ░░▓▓▓▓▓▓██▓▓▓▓▓▓▓▓▓▓▓▓▓▓░░▒▒▒▒                      
                                ░░██▓▓██▓▓▓▓▓▓▓▓▓▓▓▓░░  ▒▒░░                      
                                  ░░░░██▓▓▓▓▓▓▓▓██▓▓▒▒▒▒                          
                                    ▒▒▓▓▓▓▓▓▓▓▒▒▒▒                                
                                ▒▒▒▒▓▓  ▒▒▒▒▒▒▒▒▒▒▒▒                              
                        ▒▒    ▒▒      ▒▒▒▒▒▒▓▓    ▒▒                              
                        ▒▒▓▓        ▒▒▒▒  ▒▒▒▒    ▒▒▒▒                            
                        ▒▒░░        ▒▒    ▒▒▒▒      ▒▒                            
                        ▒▒░░▒▒      ▒▒    ▒▒▒▒      ▒▒                            
                        ▒▒░░▒▒▒▒    ▒▒    ▒▒▒▒                                    
                        ▒▒▒▒░░▒▒▒▒        ▒▒▒▒                                    
  ▒▒▓▓▒▒▒▒▒▒▒▒▓▓        ▒▒▒▒▒▒░░▒▒        ▒▒▒▒  ░░                                
    ▒▒▒▒░░░░░░▒▒▒▒▓▓      ▒▒▒▒░░▒▒        ▒▒▒▒░░                                  
    ▒▒▒▒▒▒▒▒░░░░░░░░▒▒▓▓  ▒▒▒▒░░▒▒▒▒      ▒▒▒▒░░                                  
      ▒▒▒▒▒▒▒▒▒▒▒▒░░░░░░▒▒▒▒▒▒░░▒▒▒▒      ▒▒▒▒                                    
          ▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒    ▒▒▒▒      ▒▒▒▒                                    
              ▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒  ▒▒▒▒      ▒▒▒▒                      ░░▓▓          
                            ▒▒▒▒  ▒▒  ░░  ▒▒▒▒                      ▒▒░░          
                                ░░      ░░▒▒▒▒          ░░        ▒▒░░▒▒          
                                  ▒▒    ░░░░▒▒        ░░░░      ▒▒▒▒░░▒▒          
                ▓▓▓▓▓▓▓▓▓▓▒▒▒▒▒▒░░▒▒▒▒    ▒▒░░      ▒▒░░░░    ▓▓▒▒░░▒▒▒▒          
              ▓▓▓▓▒▒░░░░░░▒▒        ░░░░  ░░▒▒    ▒▒░░░░░░    ▒▒▒▒░░▒▒▒▒          
          ▓▓▓▓▓▓▒▒░░░░▒▒▒▒▒▒          ░░▒▒▒▒▒▒    ▒▒░░▒▒▒▒  ▒▒▒▒░░▒▒▒▒▒▒          
          ▓▓▒▒░░░░▒▒▒▒▒▒▒▒▒▒            ░░▒▒▒▒    ░░░░▒▒▒▒  ▓▓▒▒░░▒▒▒▒            
        ▓▓░░░░░░▒▒▒▒▒▒▒▒▒▒                ▒▒▒▒  ▒▒▒▒░░▒▒  ▓▓▒▒░░░░▒▒▒▒            
        ▒▒▒▒▒▒▒▒▒▒▒▒▒▒                    ▒▒▒▒  ▒▒░░░░▒▒  ▓▓▒▒░░▒▒▒▒▒▒            
                                          ▒▒▒▒  ▒▒░░▒▒▒▒  ▓▓░░▒▒▒▒▒▒              
                                          ▒▒▒▒  ▒▒░░▒▒    ▒▒▒▒▒▒▒▒                
                                          ▒▒▒▒  ▒▒░░▒▒  ▒▒▒▒▒▒                    
                                          ▒▒▒▒    ▒▒▒▒  ▒▒      ░░▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓
                                          ▒▒▒▒      ▒▒▒▒▒▒    ▓▓▒▒▒▒▒▒░░░░░░░░▒▒  
                                      ░░  ▒▒▒▒      ▒▒      ▒▒░░░░░░░░░░▒▒▒▒▒▒▒▒  
                                        ░░▒▒▒▒      ▒▒  ▒▒▒▒░░▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒    
                                        ░░▒▒▒▒    ▒▒▒▒▒▒      ▒▒▒▒▒▒▒▒▒▒▒▒▒▒      
                                        ░░▒▒▒▒    ▒▒░░░░      ░░░░░░░░░░▒▒░░      
                                          ▒▒▒▒  ░░▒▒                              
                                          ▒▒▒▒  ▒▒░░                              
                                          ▒▒▒▒░░▒▒                                
                                          ▒▒▒▒▒▒░░                                
                                          ▒▒▒▒░░                                  
                                          ▒▒▒▒                                    
                                          ▒▒▒▒                                    
                                          ▒▒▒▒  ░░                                
                                          ▒▒▒▒░░                                  
                                          ▒▒▒▒░░                                  
                                          ▒▒▒▒░░                                  
                                          ▒▒▒▒                                    
                                      ░░  ▒▒▒▒                                    
                                        ░░▒▒▒▒                                    
                                        ░░▒▒▒▒                                    
                                        ░░▒▒▒▒                                    
                                          ▒▒▒▒                                    
                                          ▒▒▒▒                                    
                                          ▒▒▒▒                                    
                                          ▒▒▒▒                                    
                                          ▒▒▒▒                                    
                                          ▒▒▒▒                                    
                                          ░░▒▒                                    
                                          ▒▒░░                                    
                                          ░░▒▒                                    
                                          ░░▒▒                                    
        `;

        // Check passcode on Enter key
        document.getElementById('passcode').addEventListener('keypress', function (e) {
            if (e.key === 'Enter') {
                checkPasscode();
            }
        });

        function checkPasscode() {
            const input = document.getElementById('passcode');
            const errorMsg = document.getElementById('error-msg');
            const loginCard = document.getElementById('login-card');
            const artCard = document.getElementById('art-card');

            if (input.value === CORRECT_CODE) {
                // Fade out login card
                loginCard.style.opacity = '0';
                loginCard.style.transform = 'scale(0.96)';
                setTimeout(() => {
                    loginCard.style.display = 'none';
                    artCard.style.display = 'block';
                    setTimeout(() => {
                        artCard.style.opacity = '1';
                        renderArtSlowly();
                    }, 50);
                }, 400);
            } else {
                // Display error and shake effect
                errorMsg.innerText = "Incorrect passcode ☁︎ Try again";
                loginCard.classList.add('shake');
                input.value = '';
                setTimeout(() => {
                    loginCard.classList.remove('shake');
                }, 400);
            }
        }

        // Fast Typewriter Effect to render huge ASCII art smoothly
        function renderArtSlowly() {
            const artTarget = document.getElementById('flower-art');
            let index = 0;
            artTarget.innerText = '';

            const timer = setInterval(() => {
                if (index < FLOWER_ART.length) {
                    artTarget.innerText += FLOWER_ART.substring(index, index + 35); // Render in chunks for better performance
                    index += 35;
                } else {
                    clearInterval(timer);
                }
            }, 5);
        }
    </script>
</body>
</html>