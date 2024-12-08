---
layout: post
title: Flocker Art
search_exclude: true
description: Guess the Drawing
hide: true
---

<div id="app"></div>

<script>
    document.addEventListener('DOMContentLoaded', () => {
        const app = document.getElementById('app');

        app.style.cssText = `
            background: linear-gradient(135deg, #ff5f6d, #ffc371, #47cf73, #2d9bf0, #6a11cb, #ff6a00);
            background-size: 400% 400%;
            animation: vibrantBG 10s ease infinite;
            font-family: Arial, sans-serif;
            color: white;
            margin: 0;
            padding: 0;
            height: 100vh;
            width: 100%;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: flex-start;
        `;

        const style = document.createElement('style');
        style.textContent = `
            @keyframes vibrantBG {
                0% { background-position: 0% 50%; }
                50% { background-position: 100% 50%; }
                100% { background-position: 0% 50%; }
            }

            .submenu {
                display: flex;
                justify-content: center;
                padding: 10px;
                background: rgba(0, 0, 0, 0.6);
                width: 100%;
                position: sticky;
                top: 0;
                z-index: 1000;
            }

            .submenu a {
                color: white;
                text-decoration: none;
                margin: 0 15px;
                padding: 10px 20px;
                border-radius: 5px;
                transition: background 0.3s ease;
            }

            .submenu a:hover {
                background: rgba(255, 255, 255, 0.3);
            }

            #content {
                padding: 20px;
                text-align: center;
                width: 100%;
                flex: 1;
                display: flex;
                justify-content: center;
                align-items: center;
                flex-direction: column;
            }

            #leaderboard-content,
            #chatroom-content {
                display: none;
                width: 80%;
                background: rgba(0, 0, 0, 0.6);
                padding: 20px;
                border-radius: 10px;
            }

            #chatbox {
                width: 100%;
                height: 300px;
                overflow-y: auto;
                border: 1px solid #ccc;
                padding: 10px;
                background-color: black;
                color: white;
                border-radius: 5px;
                font-family: monospace;
            }

            .chat-message {
                margin-bottom: 10px;
                color: white;
            }

            input, button {
                margin-top: 10px;
                padding: 10px;
                border: none;
                border-radius: 5px;
            }

            input {
                width: 80%;
            }

            button {
                background-color: #ff6a00;
                color: white;
                cursor: pointer;
            }

            button:hover {
                background-color: #e65a00;
            }
        `;
        document.head.appendChild(style);

        const submenu = document.createElement('div');
        submenu.className = 'submenu';
        submenu.innerHTML = `
            <a href="#" onclick="showFeature('leaderboard-content')">Leaderboard</a>
            <a href="#" onclick="showFeature('chatroom-content')">Chatroom</a>
        `;

        const content = document.createElement('div');
        content.id = 'content';

        const leaderboardContent = document.createElement('div');
        leaderboardContent.id = 'leaderboard-content';
        leaderboardContent.innerHTML = `
            <h1>Leaderboard</h1>
            <p>Coming Soon: Feature under development!</p>
        `;

        const chatroomContent = document.createElement('div');
        chatroomContent.id = 'chatroom-content';
        chatroomContent.innerHTML = `
            <h1>Chatroom</h1>
            <div id="chatbox"></div>
            <div id="controls">
                <input type="text" id="message" placeholder="Enter your message" />
                <button id="sendButton">Send</button>
            </div>
        `;

        content.appendChild(leaderboardContent);
        content.appendChild(chatroomContent);
        app.appendChild(submenu);
        app.appendChild(content);

        window.showFeature = function (featureId) {
            const allFeatures = ['leaderboard-content', 'chatroom-content'];
            allFeatures.forEach(id => {
                document.getElementById(id).style.display = id === featureId ? 'block' : 'none';
            });

            if (featureId === 'chatroom-content') {
                fetchMessages(); // Load chat messages when chatroom is active
            }
        };

        const chatbox = document.getElementById('chatbox');

        const displayMessage = (msg) => {
            const div = document.createElement('div');
            div.className = 'chat-message';
            div.textContent = `${msg.timestamp || new Date().toLocaleTimeString()}: ${msg.message}`;
            chatbox.appendChild(div);
            chatbox.scrollTop = chatbox.scrollHeight;
        };

        async function fetchMessages() {
            const messages = [
                { message: "Welcome to the chatroom!", timestamp: "10:00 AM" },
                { message: "Feel free to chat here.", timestamp: "10:01 AM" }
            ];
            chatbox.innerHTML = '';
            messages.forEach(displayMessage);
        }

        async function sendMessage() {
            const message = document.getElementById('message').value;
            if (!message) return;

            const newMessage = {
                message,
                timestamp: new Date().toLocaleTimeString()
            };
            displayMessage(newMessage);  // Display immediately in chatbox
            document.getElementById('message').value = ''; // Clear input
        }

        document.getElementById('sendButton').addEventListener('click', sendMessage);

        // Load initial messages when Chatroom is selected
        showFeature('chatroom-content');
    });
</script>
