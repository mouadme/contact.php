<?php
use PHPMailer\PHPMailer\PHPMailer;
use PHPMailer\PHPMailer\Exception;

require 'PHPMailer/src/Exception.php';
require 'PHPMailer/src/PHPMailer.php';
require 'PHPMailer/src/SMTP.php';

if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $name = htmlspecialchars(trim($_POST['name']));
    $email = htmlspecialchars(trim($_POST['email']));
    $subject = htmlspecialchars(trim($_POST['subject']));
    $message = htmlspecialchars(trim($_POST['message']));

    $mail = new PHPMailer(true);

    try {
        // إعدادات SMTP
        $mail->isSMTP();
        $mail->Host = 'smtp.gmail.com';  // خادم SMTP الخاص بـ Gmail
        $mail->SMTPAuth = true;
        $mail->Username = 'your-email@gmail.com';  // بريدك الإلكتروني
        $mail->Password = 'your-password';  // كلمة مرور بريدك
        $mail->SMTPSecure = 'tls';
        $mail->Port = 587;

        // إعدادات البريد الإلكتروني
        $mail->setFrom($email, $name);
        $mail->addAddress('craftedcharm7@gmail.com');
        $mail->isHTML(true);
        $mail->Subject = $subject;
        $mail->Body = "
        <html>
        <head>
            <title>$subject</title>
        </head>
        <body>
            <h2>رسالة جديدة من $name</h2>
            <p><strong>البريد الإلكتروني:</strong> $email</p>
            <p><strong>الموضوع:</strong> $subject</p>
            <p><strong>الرسالة:</strong></p>
            <p>$message</p>
        </body>
        </html>
        ";

        $mail->send();
        echo 'تم إرسال الرسالة بنجاح';
    } catch (Exception $e) {
        echo "فشل في إرسال الرسالة. الخطأ: {$mail->ErrorInfo}";
    }
}
?>
