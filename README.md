[gemini-code-1781973664591.html](https://github.com/user-attachments/files/29164465/gemini-code-1781973664591.html)
<!-- html-preview -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lumina AI - Next Gen Assistant</title>
    <style>
        :root {
            --bg-color: #0b0f19;
            --panel-bg: #131a2c;
            --accent-color: #6366f1;
            --accent-gradient: linear-gradient(135deg, #6366f1 0%, #a855f7 100%);
            --text-main: #f3f4f6;
            --text-muted: #9ca3af;
            --border-color: #1e293b;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-main);
            display: flex;
            height: 450px; /* Adjusted height tailored for an in-app browser preview wrapper */
            overflow: hidden;
            border-radius: 12px;
        }

        /* Sidebar Styling */
        .sidebar {
            width: 200px;
            background-color: var(--panel-bg);
            border-right: 1px solid var(--border-color);
            display: flex;
            flex-direction: column;
            padding: 15px;
        }

        .logo {
            font-size: 18px;
            font-weight: 700;
            background: var(--accent-gradient);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            margin-bottom: 20px;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .new-chat-btn {
            background: var(--accent-gradient);
            color: white;
            border: none;
            padding: 10px;
            border-radius: 8px;
            cursor: pointer;
            font-weight: 600;
            font-size: 13px;
            transition: opacity 0.2s;
        }

        .new-chat-btn:hover {
            opacity: 0.9;
        }

        /* Main Chat Area */
        .chat-container {
            flex: 1;
            display: flex;
            flex-direction: column;
            background-color: var(--bg-color);
        }

        .chat-header {
            padding: 15px 25px;
            border-bottom: 1px solid var(--border-color);
            background-color: rgba(19, 26, 44, 0.5);
            backdrop-filter: blur(10px);
        }

        .chat-header h3 {
            font-size: 16px;
        }

        .messages-box {
            flex: 1;
            overflow-y: auto;
            padding: 25px;
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        /* Message Bubbles */
        .message {
            max-width: 85%;
            padding: 12px 16px;
            border-radius: 12px;
            line-height: 1.4;
            font-size: 14px;
            animation: fadeIn 0.25s ease-in-out;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(5px); }
            to { opacity: 0; transform: translateY(0); }
        }

        .user-message {
            background-color: var(--accent-color);
            color: white;
            align-self: flex-end;
            border-bottom-right-radius: 2px;
        }

        .ai-message {
            background-color: var(--panel-bg);
            color: var(--text-main);
            align-self: flex-start;
            border-bottom-left-radius: 2px;
            border: 1px solid var(--border-color);
        }

        /* Input Area */
        .input-area {
            padding: 20px 25px;
        }

        .input-wrapper {
            display: flex;
            background-color: var(--panel-bg);
            border: 1px solid var(--border-color);
            border-radius: 10px;
            padding: 6px;
            align-items: center;
        }

        .input-wrapper:focus-within {
            border-color: var(--accent-color);
        }

        textarea {
            flex: 1;
            background: transparent;
            border: none;
            color: var(--text-main);
            padding: 8px;
            resize: none;
            outline: none;
            font-size: 14px;
            height: 20px;
        }

        .send-btn {
            background: var(--accent-gradient);
            border: none;
            color: white;
            padding: 8px 16px;
            border-radius: 6px;
            cursor: pointer;
            font-weight: 600;
            font-size: 13px;
        }

        .send-btn:disabled {
            background: #334155;
            color: var(--text-muted);
            cursor: not-allowed;
        }
    </style>
</head>
<body>

    <!-- Sidebar Navigation -->
    <div class="sidebar">
        <div class="logo">✦ Lumina AI</div>
        <button class="new-chat-btn" onclick="clearChat()">+ New Chat</button>
    </div>

    <!-- Chat Interface -->
    <div class="chat-container">
        <div class="chat-header">
            <h3>Lumina-v1.0</h3>
        </div>

        <div class="messages-box" id="chatBox">
            <div class="message ai-message">
                Hello! I am Lumina, your AI assistant. How can I help you create or learn today?
            </div>
        </div>

        <div class="input-area">
            <div class="input-wrapper">
                <textarea id="userInput" placeholder="Ask Lumina anything..." rows="1" onkeydown="handleKeyPress(event)"></textarea>
                <button class="send-btn" id="sendBtn" onclick="sendMessage()">Send</button>
            </div>
        </div>
    </div>

    <script>
        const chatBox = document.getElementById('chatBox');
        const userInput = document.getElementById('userInput');
        const sendBtn = document.getElementById('sendBtn');

        userInput.addEventListener('input', function() {
            this.style.height = 'auto';
            this.style.height = (this.scrollHeight - 16) + 'px';
        });

        function handleKeyPress(e) {
            if (e.key === 'Enter' && !e.shiftKey) {
                e.preventDefault();
                sendMessage();
            }
        }

        function appendMessage(text, isUser) {
            const msgDiv = document.createElement('div');
            msgDiv.classList.add('message', isUser ? 'user-message' : 'ai-message');
            msgDiv.innerText = text;
            chatBox.appendChild(msgDiv);
            chatBox.scrollTop = chatBox.scrollHeight;
        }

        function clearChat() {
            chatBox.innerHTML = `
                <div class="message ai-message">
                    Hello! I am Lumina, your AI assistant. How can I help you create or learn today?
                </div>
            `;
        }

        async function sendMessage() {
            const prompt = userInput.value.trim();
            if (!prompt) return;

            appendMessage(prompt, true);
            userInput.value = '';
            userInput.style.height = '20px';
            
            sendBtn.disabled = true;
            const loadingDiv = document.createElement('div');
            loadingDiv.classList.add('message', 'ai-message');
            loadingDiv.innerText = "Lumina is thinking...";
            chatBox.appendChild(loadingDiv);
            chatBox.scrollTop = chatBox.scrollHeight;

            try {
                // Free public LLM endpoint using Llama 3 
                const response = await fetch("https://api-inference.huggingface.co/models/meta-llama/Meta-Llama-3-8B-Instruct", {
                    method: "POST",
                    headers: { "Content-Type": "application/json" },
                    body: JSON.stringify({ 
                        inputs: `<|begin_of_text|><|start_header_id|>user<|end_header_id|>${prompt}<|eot_id|><|start_header_id|>assistant<|end_header_id|>` 
                    }),
                });

                const data = await response.json();
                chatBox.removeChild(loadingDiv);

                let aiReply = "";
                if (data && data[0] && data[0].generated_text) {
                    const rawText = data[0].generated_text;
                    aiReply = rawText.split("<|start_header_id|>assistant<|end_header_id|>").pop().replace("<|eot_id|>", "").trim();
                } else {
                    aiReply = "I encountered a tiny hiccup processing that. Could you please try again?";
                }

                appendMessage(aiReply, false);

            } catch (error) {
                console.error(error);
                chatBox.removeChild(loadingDiv);
                appendMessage("Connection error. Please check your internet connection or try again later.", false);
            } finally {
                sendBtn.disabled = false;
            }
        }
    </script>
</body>
</html>
