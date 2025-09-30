<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>App de Encriptación de Mensajes</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/crypto-js/4.1.1/crypto-js.min.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            margin: 0;
            padding: 20px;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }
        .container {
            background-color: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            max-width: 600px;
            width: 100%;
        }
        h1 {
            text-align: center;
            color: #333;
        }
        .section {
            margin-bottom: 30px;
        }
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }
        input, textarea {
            width: 100%;
            padding: 10px;
            margin-bottom: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
            box-sizing: border-box;
        }
        button {
            width: 100%;
            padding: 10px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
        }
        button:hover {
            background-color: #45a049;
        }
        textarea {
            height: 100px;
            resize: vertical;
        }
        .output {
            margin-top: 10px;
            padding: 10px;
            background-color: #e9e9e9;
            border: 1px solid #ddd;
            border-radius: 4px;
            white-space: pre-wrap;
            word-wrap: break-word;
        }
        .error {
            color: red;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>App de Encriptación de Mensajes</h1>
        
        <div class="section">
            <h2>Encriptar Mensaje</h2>
            <label for="message">Mensaje:</label>
            <textarea id="message" placeholder="Ingresa tu mensaje aquí"></textarea>
            
            <label for="password">Contraseña:</label>
            <input type="password" id="password" placeholder="Ingresa una contraseña segura">
            
            <button onclick="encryptMessage()">Encriptar</button>
            
            <div id="encryptedOutput" class="output"></div>
        </div>
        
        <div class="section">
            <h2>Desencriptar Mensaje</h2>
            <label for="encrypted">Mensaje Encriptado:</label>
            <textarea id="encrypted" placeholder="Pega el mensaje encriptado aquí"></textarea>
            
            <label for="decryptPassword">Contraseña:</label>
            <input type="password" id="decryptPassword" placeholder="Ingresa la contraseña">
            
            <button onclick="decryptMessage()">Desencriptar</button>
            
            <div id="decryptedOutput" class="output"></div>
        </div>
    </div>

    <script>
        function encryptMessage() {
            var message = document.getElementById('message').value;
            var password = document.getElementById('password').value;
            
            if (!message || !password) {
                document.getElementById('encryptedOutput').innerHTML = '<span class="error">Por favor, ingresa un mensaje y una contraseña.</span>';
                return;
            }
            
            // Crear datos con hash para verificación
            var data = {
                message: message,
                hash: CryptoJS.SHA256(message).toString()
            };
            var jsonData = JSON.stringify(data);
            
            // Encriptar
            var ciphertext = CryptoJS.AES.encrypt(jsonData, password).toString();
            
            document.getElementById('encryptedOutput').innerHTML = 'Mensaje encriptado:<br>' + ciphertext;
            document.getElementById('encrypted').value = ciphertext; // Copiar automáticamente para desencriptar
        }

        function decryptMessage() {
            var ciphertext = document.getElementById('encrypted').value;
            var password = document.getElementById('decryptPassword').value;
            
            if (!ciphertext || !password) {
                document.getElementById('decryptedOutput').innerHTML = '<span class="error">Por favor, ingresa el mensaje encriptado y la contraseña.</span>';
                return;
            }
            
            try {
                // Desencriptar
                var bytes = CryptoJS.AES.decrypt(ciphertext, password);
                var jsonData = bytes.toString(CryptoJS.enc.Utf8);
                
                if (!jsonData) {
                    throw new Error('Desencriptación fallida');
                }
                
                var data = JSON.parse(jsonData);
                
                // Verificar hash
                var computedHash = CryptoJS.SHA256(data.message).toString();
                if (data.hash !== computedHash) {
                    throw new Error('Hash no coincide');
                }
                
                document.getElementById('decryptedOutput').innerHTML = 'Mensaje desencriptado:<br>' + data.message;
            } catch (error) {
                document.getElementById('decryptedOutput').innerHTML = '<span class="error">Contraseña incorrecta o mensaje inválido.</span>';
            }
        }
    </script>
</body>
</html># web
