contact.php
<?php
// Connexion à la base de données
$host = "localhost";
$user = "root";
$pass = "";
$dbname = "eco";

$con = mysqli_connect($host, $user, $pass, $dbname);

if (!$con) {
    die("Erreur de connexion : " . mysqli_connect_error());
}

// Traitement du formulaire
if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $prenom = mysqli_real_escape_string($con, $_POST['fname']);
    $nom = mysqli_real_escape_string($con, $_POST['Iname']);
    $genre = mysqli_real_escape_string($con, $_POST['gender']);
    $contact = mysqli_real_escape_string($con, $_POST['number']);
    $adresse = mysqli_real_escape_string($con, $_POST['add']);
    $email = mysqli_real_escape_string($con, $_POST['mail']);
    $password = mysqli_real_escape_string($con, $_POST['pass']);

    if (!empty($email) && !empty($password) && filter_var($email, FILTER_VALIDATE_EMAIL)) {
        $hashed_password = password_hash($password, PASSWORD_DEFAULT);

        $query = "INSERT INTO site_ecommerce (Firstname, Lastename, numero, adresse, email, Password) 
                  VALUES ('$prenom', '$nom', '$contact', '$adresse', '$email', '$hashed_password')";

        if (mysqli_query($con, $query)) {
            echo "<script>alert('Inscription réussie !'); window.location.href='login1.php';</script>";
        } else {
            echo "<script>alert('Erreur lors de l\'inscription : " . mysqli_error($con) . "');</script>";
        }
    } else {
        echo "<script>alert('Email invalide ou mot de passe manquant');</script>";
    }
}
?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Signup</title>
    <link rel="stylesheet" href="style.css">


</head>
<body class="signup-page">
    <div class="wrapper">
        <h1>Signup</h1>
        <p style="text-align:center;">It's free and take it easily</p>

        <form method="POST">
            <div class="input-box">
                <input type="text" name="fname" placeholder="First name" required>
            </div>

            <div class="input-box">
                <input type="text" name="Iname" placeholder="Last name" required>
            </div>

            <div class="input-box">
                <input type="text" name="gender" placeholder="Gender" required>
            </div>

            <div class="input-box">
                <input type="tel" name="number" placeholder="Contact address" required>
            </div>

            <div class="input-box">
                <input type="text" name="add" placeholder="Adresse" required>
            </div>

            <div class="input-box">
                <input type="email" name="mail" placeholder="Email" required>
            </div>

            <div class="input-box">
                <input type="password" name="pass" placeholder="Password" required>
            </div>

            <button type="submit" class="btn">Submit</button>

            <div class="register-link">
                <p>Try your account here by clicking <a href="#">Terms and conditions</a></p>
                <p>Already have an account? <a href="login1.php">Login here</a></p>
            </div>
        </form>
    </div>
</body>

