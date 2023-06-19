<!DOCTYPE html>
<html>
<head>
    <title>ChatGPT Bot</title>
</head>
<body>
    <div id="chat-container">
        <div id="chat-log"></div>
        <input type="text" id="user-input" placeholder="Digite sua mensagem..." autocomplete="off">
        <button id="send-btn">Enviar</button>
    </div>

    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script>
        $(document).ready(function () {
            var chatLog = document.getElementById("chat-log");
            var userInput = document.getElementById("user-input");
            var sendBtn = document.getElementById("send-btn");

            // Função para exibir mensagens do usuário e respostas do bot
            function appendMessage(message, sender) {
                var messageElement = document.createElement("div");
                messageElement.classList.add(sender);
                messageElement.innerHTML = message;
                chatLog.appendChild(messageElement);
                chatLog.scrollTop = chatLog.scrollHeight;
            }

            // Função para enviar a mensagem do usuário para o modelo ChatGPT
            function sendMessage() {
                var message = userInput.value;
                appendMessage("<strong>Você:</strong> " + message, "user");
                userInput.value = "";

                // Solicitação à API do OpenAI para receber a resposta gerada pelo modelo
                $.ajax({
                    type: "POST",
                    url: "https://api.openai.com/v1/engines/davinci-codex/completions",
                    headers: {
                        "Authorization": "Bearer YOUR_API_KEY",
                        "Content-Type": "application/json"
                    },
                    data: JSON.stringify({
                        "prompt": message,
                        "max_tokens": 50
                    }),
                    success: function (response) {
                        var answer = response.choices[0].text.trim();
                        appendMessage("<strong>ChatGPT:</strong> " + answer, "bot");
                    }
                });
            }

            // Envia a mensagem quando o botão de envio é clicado
            sendBtn.addEventListener("click", function () {
                sendMessage();
            });

            // Envia a mensagem quando a tecla Enter é pressionada
            userInput.addEventListener("keyup", function (event) {
                if (event.keyCode === 13) {
                    sendMessage();
                }
            });
        });
    </script>
</body>
</html>
