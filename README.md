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

**********************************************************************************************************************************
login.php:

<?php
// Démarrer la session
session_start();

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
    $email = mysqli_real_escape_string($con, $_POST['mail']);
    $password = $_POST['pass'];

    // Vérifie si l'utilisateur existe
    $query = "SELECT * FROM site_ecommerce WHERE email = '$email'";
    $result = mysqli_query($con, $query);

    if (mysqli_num_rows($result) == 1) {
        $user = mysqli_fetch_assoc($result);
        $hashed_password = $user['Password'];

        if (password_verify($password, $hashed_password)) {
            // Connexion réussie
            $_SESSION['user_email'] = $user['email'];
            $_SESSION['user_name'] = $user['Firstname'];
            echo "<script>alert('Connexion réussie !'); window.location.href='index.php';</script>";
            exit;
        } else {
            echo "<script>alert('Mot de passe incorrect.');</script>";
        }
    } else {
        echo "<script>alert('Aucun utilisateur trouvé avec cet email.');</script>";
    }
}
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login form registration</title>
    <link rel="stylesheet" href="login1.css">
    <link href='https://unpkg.com/boxicons@2.1.4/css/boxicons.min.css' rel='stylesheet'>
</head> 
<body style="background-color: black;">

    <div class="wrapper">
        <form method="POST">
            <h1>Login</h1>
            <div class="input-box">
                <input type="email" name="mail" placeholder="Email" required>  
                <i class='bx bxs-user'></i>
            </div>
            <div class="input-box">
                <input type="password" name="pass" placeholder="Password" required>
                <i class='bx bxs-lock-alt'></i>
            </div>
            <div class="remember-forgot">
                <label><input type="checkbox">Remember me</label>
                <a href="#">Forgot password?</a>
            </div>
            <button type="submit" class="btn">Login</button>
            <div class="register-link">
                <p>Don't have an account? <a href="contact.php">Register now!</a></p>
            </div>
        </form>
    </div>
</body>
</html>
*******************************************************************************************************************************************************
style.css:
@import url('https://fonts.googleapis.com/css2?family=Raleway:ital,wght@0,100..900;1,100..900&display=swap');

/* RESET */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
a {
    text-decoration: none;
    color: #fff;
}
html {
    font-size: 62.5%;
    scroll-behavior: smooth;
    scroll-padding-top: 9px;
}
::-webkit-scrollbar {
    width: 8px;
}
::-webkit-scrollbar-thumb {
    background-color: #ccc;
}
body {
    background-color: black;
    font-family: 'Raleway', sans-serif;
}

/* ------------------- HEADER ------------------- */
header {
    background-color: black;
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 1000;
    display: flex;
    align-items: center;
    justify-content: space-between;
    height: 50px;
    padding: 0 5%;
}
header .logo a {
    font-size: 25px;
    color: #29D9D5;
}
header .logo a span {
    color: #fff;
}
.menu {
    display: flex;
    align-items: center;
}
.menu li {
    color: #fff;
    margin: 0 15px;
    list-style-type: none;
}
.btn-aboutUs {
    color: aqua;
    font-size: 14px;
    border: 2px solid #29d9d5;
    padding: 5px 20px;
    transition: 0.5s;
    font-weight: bolder;
}
.btn-aboutUs:hover {
    background-color: #29d9d5;
    color: #fff;
}

/* ------------------- HOME PAGE ------------------- */
#home {
    background: linear-gradient(rgba(0,0,0,0.1), #333), url(image1.webp);
    background-position: center;
    background-size: cover;
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    position: relative;
}
#home h2 {
    font-size: 45px;
    margin-bottom: 10px;
    -webkit-text-stroke: #fff 1.5px;
    color: transparent;
}
#home p {
    color: #fff;
    margin-bottom: 10px;
    font-size: 12px;
}
.home-btn {
    margin-bottom: 20px;
}
.find_product {
    background-color: black;
    padding: 20px;
    width: 50%;
    position: absolute;
    bottom: -50px;
}
.find_product form {
    display: flex;
    justify-content: space-between;
    align-items: flex-end;
    width: 100%;
}
.find_product form div,
.find_product form input[type="submit"] {
    display: flex;
    flex-direction: column;
    width: 20%;
}
.find_product form label {
    color: #999;
    font-size: 15px;
    margin-bottom: 10px;
}
.find_product form input {
    padding: 5px 20px;
    background-color: transparent;
    border: 1px solid #29d9d5;
    outline: 0;
    width: 100%;
    font-size: 13px;
    color: #fff;
}
.find_product form input[type="submit"] {
    text-transform: uppercase;
    color: #29D9D5;
    cursor: pointer;
    transition: 0.5s;
}
.find_product form input[type="submit"]:hover {
    box-shadow: 0 0 10px #29D9D5;
}

/* ------------------- SECTION ------------------- */
section {
    margin-top: 50px;
}

/* ------------------- À PROPOS ------------------- */
#a-propos {
    margin-top: 100px;
    margin-bottom: 55px;
    padding: 0 10%;
    width: 100%;
}
.title {
    text-transform: capitalize;
    margin: 70px 0;
    font-weight: bold;
    color: #29D9D5;
    position: relative;
    margin-left: 15px;
    font-size: 18px;
}
.title::after {
    position: absolute;
    left: -15px;
    content: "";
    height: 100%;
    background-color: #fff;
    width: 8px;
}
.img-desc {
    display: flex;
    align-items: center;
    justify-content: space-between;
    width: 100%;
}
.img-desc .left {
    position: relative;
    margin-left: 10px;
    height: 250px;
    width: 40%;
}
.img-desc .right {
    width: 75%;
}
.img-desc .left video {
    width: 100%;
    height: 100%;
    position: relative;
    box-shadow: 0 0 10px #29D9D5;
}
.img-desc .left::after {
    position: absolute;
    bottom: -10px;
    right: 9px;
    content: "";
    height: 100%;
    background-color: #29D9D5;
}
.img-desc .right h3 {
    color: #fff;
    font-size: 25px;
    margin-bottom: 30px;
    margin: 20px;
}
.img-desc .right p {
    color: #999;
    font-size: 20px;
    margin-bottom: 36px;
    margin: 20px;
}
.img-desc .right a {
    border: 1px solid #29D9D5;
    color: #29D9D5;
    font-size: 14px;
    padding: 8px 25px;
    font-weight: bold;
    margin: 20px;
}

/* ------------------- SIGNUP / CONTACT ------------------- */
.signup-page {
    min-height: 100vh;
    background: url('img.jpg') no-repeat center center/cover;
    display: flex;
    justify-content: center;
    align-items: center;
}

.wrapper {
    width: 420px;
    background: transparent;
    border: 2px solid rgba(255, 255, 255, 0.2);
    backdrop-filter: blur(20px);
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
    color: #fff;
    border-radius: 10px;
    padding: 30px 40px;
}

.wrapper h1 {
    font-size: 36px;
    text-align: center;
}

.input-box {
    position: relative;
    width: 100%;
    height: 50px;
    margin: 36px 0;
}

.input-box input {
    width: 100%;
    height: 100%;
    background-color: transparent;
    border: none;
    outline: none;
    border: 2px solid rgba(255, 255, 255, 0.2);
    border-radius: 40px;
    font-size: 16px;
    color: #fff;
    padding: 20px 45px 20px 20px;
}

.input-box input::placeholder {
    color: #fff;
}

.input-box i {
    position: absolute;
    right: 20px;
    top: 50%;
    transform: translateY(-50%);
    font-size: 20px;
}

.remember-forgot {
    display: flex;
    justify-content: space-between;
    font-size: 14.5px;
    margin: -15px 0 15px;
}

.remember-forgot label input {
    color: #fff;
    margin-right: 3px;
}

.remember-forgot a {
    color: #fff;
    text-decoration: none;
}

.remember-forgot a:hover {
    text-decoration: underline;
}

.wrapper .btn {
    width: 100%;
    height: 45px;
    background: #fff;
    border: none;
    outline: none;
    border-radius: 40px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    font-size: 16px;
    color: #333;
    font-weight: 600;
}

.register-link {
    font-size: 14.5px;
    text-align: center;
    margin: 20px 0 15px;
}

.register-link p a {
    color: #fff;
    text-decoration: none;
    font-weight: 600;
}

.register-link p a:hover {
    text-decoration: underline;
}
******************************************************************************************************
login1.css:
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: "poppins", sans-serif;
}

body {
    background-color: black;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    background: url('') no-repeat;
    background-size: cover;
    background-position: center;
}

.wrapper {
    width: 420px;
    background: transparent;
    border: 2px solid rgba(255, 255, 255, 0.2);
    backdrop-filter: blur(20px);
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
    color: #fff;
    border-radius: 10px;
    padding: 30px 40px;
}

.wrapper h1 {
    font-size: 36px;
    text-align: center;
}

.wrapper .input-box {
    position: relative;
    width: 100%;
    height: 50px;
    margin: 36px 0;
}

.input-box input {
    width: 100%;
    height: 100%;
    background-color: transparent;
    border: none;
    outline: none;
    border: 2px solid rgba(255, 255, 255, 0.2);
    border-radius: 40px;
    font-size: 16px;
    color: #fff;
    padding: 20px 45px 20px 20px;
}

.input-box input::placeholder {
    color: #fff;
}

.input-box i {
    position: absolute;
    right: 20px;
    top: 50%;
    transform: translateY(-50%);
    font-size: 20px;
}

.wrapper .remember-forgot {
    display: flex;
    justify-content: space-between;
    font-size: 14.5px; /* Added "px" unit */
    margin: -15px 0 15px;
}

.remember-forgot label input {
    color: #fff; /* Changed to "color" */
    margin-right: 3px;
}

.remember-forgot a {
    color: #fff;
    text-decoration: none;
}

.remember-forgot a:hover {
    text-decoration: underline;
}

.wrapper .btn {
    width: 100%;
    height: 45px;
    background: #fff;
    border: none;
    outline: none;
    border-radius: 40px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    font-size: 16px;
    color: #333;
    font-weight: 600;
}

.wrapper .register-link {
    font-size: 14.5px;
    text-align: center;
    margin: 20px 0 15px;
}

.register-link p a {
    color: #fff;
    text-decoration: none;
    font-weight: 600;
}

.register-link p a:hover {
    text-decoration: underline;
}
**************************************************************************************************************************************************
product.html:
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link rel="stylesheet" href="product.css">
</head>
<body>
    <section id="product1" class="section-p1">
        <h2> New Product</h2>
        <p>Commander maintenant. </p>
        <div class="pro-conatiner">
            <div class="pro" >
                <img src="images1.jpg" alt="">
                <div class="des">
                    <span>Adidas</span>
                    <h5>T-shirt for Summer !</h5>
                    <div class="star">
                        <i class="fas fa-star"></i>
                        <i class="fas fa-star"></i>
                        <i class="fas fa-star"></i>
                        <i class="fas fa-star"></i>
                        <i class="fas fa-star"></i>
                    </div>
                    <h4>$70</h4>
                </div>
                <a href="#"><i class="fal fa-shopping-cart cart"></i></a>
            </div>
            <div class="pro">
                <img src="images2.jpg" alt="">
                <div class="des">
                    <span>Adidas</span>
                    <h5>T-shirt for Summer !</h5>
                    <div class="star">
                        <i class="fas fa-star"></i>
                        <i class="fas fa-star"></i>
                        <i class="fas fa-star"></i>
                        <i class="fas fa-star"></i>
                        <i class="fas fa-star"></i>
                    </div>
                    <h4>$70</h4>
                </div>
                <a href="#"><i class="fal fa-shopping-cart cart"></i></a>
            </div>
            <div class="pro">
                <img src="images 3.jpg" alt="">
                <div class="des">
                    <span>Adidas</span>
                    <h5>T-shirt for Summer !</h5>
                    <div class="star">
                        <i class="fas fa-star"></i>
                        <i class="fas fa-star"></i>
                        <i class="fas fa-star"></i>
                        <i class="fas fa-star"></i>
                        <i class="fas fa-star"></i>
                    </div>
                    <h4>$70</h4>
                </div>
                <a href="#"><i class="fal fa-shopping-cart cart"></i></a>
            </div>
            <div class="pro">
                <img src="Iimages 4.jpg" alt="">
                <div class="des">
                    <span>Adidas</span>
                    <h5>T-shirt for Summer !</h5>
                    <div class="star">
                        <i class="fas fa-star"></i>
                        <i class="fas fa-star"></i>
                        <i class="fas fa-star"></i>
                        <i class="fas fa-star"></i>
                        <i class="fas fa-star"></i>
                    </div>
                    <h4>$70</h4>
                </div>
                <a href="#"><i class="fal fa-shopping-cart cart"></i></a>
            </div>
            <div class="pro">
                <img src="images 5.jpg" alt="">
                <div class="des">
                    <span>Adidas</span>
                    <h5>T-shirt for Summer !</h5>
                    <div class="star">
                        <i class="fas fa-star"></i>
                        <i class="fas fa-star"></i>
                        <i class="fas fa-star"></i>
                        <i class="fas fa-star"></i>
                        <i class="fas fa-star"></i>
                    </div>
                    <h4>$70</h4>
                </div>
                <a href="#"><i class="fal fa-shopping-cart cart"></i></a>
            </div>
            <div class="pro">
                <img src="images 6.jpg" alt="">
                <div class="des">
                    <span>Adidas</span>
                    <h5>T-shirt for Summer !</h5>
                    <div class="star">
                        <i class="fas fa-star"></i>
                        <i class="fas fa-star"></i>
                        <i class="fas fa-star"></i>
                        <i class="fas fa-star"></i>
                        <i class="fas fa-star"></i>
                    </div>
                    <h4>$70</h4>

                </div>
                <a href="#"><i class="fal fa-shopping-cart cart"></i></a>
            </div>
           
                <a href="#"><i class="fal fa-shopping-cart cart"></i></a>
            </div>
        </div>
    </section>
    
</body>
</html>
****************************************************************************************************************************************
product.css:
@import url("https://fonts.googleapis.com/css2?family=Spartan:wght@100;200;300;400;500;600;700;800;900&display=swap");
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Spartan", sans-serif;
}

html {
  scroll-behavior: smooth;
}

/* Global Styles */

h1 {
  font-size: 50px;
  line-height: 64px;
  color: #222;
}

h2 {
  font-size: 46px;
  line-height: 54px;
  color: #222;
}

h4 {
  font-size: 20px;
  color: #222;
}

h6 {
  font-weight: 700;
  font-size: 12px;
}

p {
  font-size: 16px;
  color: #465b52;
  margin: 15px 0 20px 0;
}

.section-p1 {
  padding: 40px 80px;
}

.section-m1 {
  margin: 40px 0;
}
button.normal{
  font-size: 14px;
  font-weight: 600;
  padding: 15px 30px;
  color: #000;
  background-color: #fff;
  border-radius: 4px;
  cursor: pointer;
  border: none;
  outline: none;
  transition: 0.2s;
}
button.white {
  font-size: 13px;
  font-weight: 600;
  padding: 11px 18px;
  color: #fff;
  background-color:transparent;
  cursor: pointer;
  border: 1px solid #fff;
  outline: none;
  transition: 0.2s;
}

body {
  width: 100%;
}
#header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 20px 88px;
  background: #e3e6f3;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.06);
  z-index: 999;
  position: sticky;
  top: 0;
  left: 0;
}
#navbar {
  display: flex;
  align-items: center;
  justify-content: center;

}
#navbar li{
  list-style: none;
  padding: 0 20px ;
  position: relative;
}
#navbar li a{
  text-decoration: none;
  font-size: 16px;
  font-weight: 600;
  color: #1a1a1a;
  transition: 0,03 ease;
}
#navbar li a:hover,
#navbar li a.active{

  color: #088178;
}
#navbar li a.active::after,
#navbar li a:hover::after
{
  content: "";
  width: 38%;
  height: 2px;
  background: #088178;
  position: absolute;
  bottom: -6px;
  left: 20px;
}
#mobile{
  display: none;
  justify-content: center;
}
#clos{
  display: none;
}
#hero{
  background-image: url("images/hero4.png");
  height: 90vh;
  width: 100%;
  background-size: cover;
  background-position: top 25% right 0;
  padding: 0 80px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: center;
}
#hero h4 {
  padding-bottom: 15px;

}
#hero h1 {
  color: #088178;
  
}
#hero button {
  background-image: url("images/button.png");
  background-color:transparent;
  color: #088178;
  border: 0;
  padding: 14px 80px 14px 65px;
  background-repeat: no-repeat;
  cursor: pointer;
  font-weight: 700;
  font-size: 15px;
  
}
/* Container */
#feature {
  display: flex;
  flex-wrap: wrap;
  align-items: flex-start; /* Aligne les items au début verticalement */
  justify-content: center; /* Centre les éléments horizontalement */
  gap: 15px; /* Espace entre les éléments */
  padding: 30px 0; /* Ajout de marge au-dessus et en dessous */
}

/* Card Styling */
#feature .fe-box {
  width: 180px;
  text-align: center;
  padding: 20px; /* Espacement uniforme à l'intérieur */
  box-shadow: 10px 10px 20px rgba(0, 0, 0, 0.05); /* Ombre légère */
  border: 1px solid #cce7d0;
  border-radius: 4px;
  margin: 0; /* Enlève la marge pour uniformiser les tailles */
}

/* Hover effect */
#feature .fe-box:hover {
  box-shadow: 0px 5px 15px rgba(0, 0, 0, 0.1); /* Ombre sur hover */
  transform: scale(1.05); /* Agrandissement léger au survol */
  transition: transform 0.3s ease; /* Animation pour effet fluide */
}

/* Image Styling */
#feature .fe-box img {
  width: 100%; /* Image prend toute la largeur */
  height: auto;
  margin-bottom: 10px;
}

/* Button Styling */
#feature .fe-box h6 {
  display: inline-block;
  padding: 10px 15px;
  line-height: 1;
  border-radius: 4px;
  color: #088178;
  background-color: #fddde4;
  font-weight: bold;
  font-size: 14px;
}
#feature .fe-box:nth-child(2) h6{
  background-color: rgb(201, 239, 188)
}
#feature .fe-box:nth-child(3) h6{
  background-color: rgb(159, 237, 237)
}
#feature .fe-box:nth-child(4) h6{
  background-color: rgb(129, 169, 250)
}
#feature .fe-box:nth-child(5) h6{
  background-color: rgb(14, 14, 13)
}
#feature .fe-box:nth-child(6) h6{
  background-color: rgb(215, 245, 174)
}
#product1 {
  text-align: center;
}
#product1 .pro-conatiner {
  display: flex;
  justify-content: space-between; /* Répartit les produits sur toute la ligne */
  padding-top: 25px;
  flex-wrap: wrap;
}


#product1 .pro {
  width: 23%; /* Ajuste la largeur pour éviter qu'ils ne passent en colonne */
  min-width: 250px;
  padding: 10px 12px;
  border: 1px solid #cce7d0;
  border-radius: 23px;
  cursor: pointer;
  box-shadow: 20px 20px 30px rgba(0, 0, 0, 0.02);
  margin: 15px 0;
  transition: 0.2s ease;
  position: relative;

}


#product1 .pro img {
  width: 100%; /* Force l’image à remplir le conteneur */
  border-radius: 20px;
}


#product1 .pro .des{
  text-align: start;
  padding: 10px 0;
}
#product1 .pro .des span{
  color: #606053;
  font-size: 12px;

}
#product1 .pro .des h5 {
  padding-top: 7px;
  color: #1a1a1a;
  font-size: 14px ;
}
#product1 .pro .des i{
  font-size: 12px;
  color: rgb(243,181,25);
}
#product1 .pro .des h4{
  padding-top: 7px;
  font-size: 15px;
  font-weight: 700;
  color: #088178;
}
#product1 .pro .cart{
  width: 40px;
  height: 40px;
  line-height: 40px;
  border-radius: 50px;
  background-color: #e8f6ea;
  font-weight: 500;
  color: #088178;
  border: 1px solid #cce7d0;
  position: absolute;
  bottom: 20px;
  right: 10px;

}
#banner{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  background-image: url("images/b2.jpg");
  width: 100%;
  height: 40vh;
  background-size: cover;
  background-position: center;
}
#banner h4{
  color: #fff;
  font-size: 16px;
}
#banner h2{
  color: #fff;
  font-size: 30px;
  padding-left: 10px 0;
}
#banner h2 span{
  color: #fc0c0c;
  
}
#banner button:hover {
  background-color: #088178;
  color: #fff;
}
#sm-banner{
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
}
#sm-banner .banner-box{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-start;
  background-image: url("images/b17\ \(1\).jpg");
  min-width: 580px;
  height: 50vh;
  background-size: cover;
  background-position: center;
  padding: 30px;


}
#sm-banner .banner-box2{
  
  background-image: url("images/b10.jpg");
}
 

#sm-banner h4 {
  color: #fff;
  font-size: 20px;
  font-weight: 300;

}
#sm-banner h2{
  color: #fff;
  font-size: 28px;
  font-weight: 800;
  
}
#sm-banner span {
  color: #fff;
  font-size: 14px;
  font-weight: 500;
  padding-bottom: 15px; 
}
#sm-banner .banner-box:hover button{
  background-color: #088178;
  border: 1px solid #088178;
}
#baner3{
  display: flex;
  justify-content: space-between;
  gap: 20px;
  padding: 0 80px;
}
#baner3 .banner-box{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-start;
  background-image: url("images/b7\ \(1\).jpg");
  min-width: 30%;
  height:   30vh;
  background-size: cover;
  background-position: center;
  padding: 20px;
  margin-bottom: 20px;
}
#baner3 .banner-box2{

  background-image: url("images/b4.jpg");
  
}
#baner3 .banner-box3{
  background-image: url("images/b18\ \(1\).jpg");
}

#baner3 h2{
  color: #fff;
  font-weight: 900;
  font-size: 22px;

}
#baner3 h3{
  color: #fd0e0e;
  font-weight: 800;
  font-size: 15px;
  
}
#newletter{
  display: flex;
  justify-content: space-between;
  gap: 20px;
  align-items: center;
  background-image: url(images/b14\ \(1\).png);
  background-repeat: no-repeat;
  background-position: 20% 30%;
  background-color: #041e42;
}
#newletter h4{
  font-size: 22px;
  font-weight: 700;
  color: #f7f8f9;
}
#newletter p{
  font-size: 14px;
  font-weight: 600;
  color: #818ea0;
}
#newletter p span{
  color: #f9f906;
}
#newletter .form{
  display: flex;
  width: 40%;
}
#newletter input{
  height: 3.125rem;
  padding: 0 1.2em;
  font-size: 14px;
  width: 100%;
  border: 1px solid transparent;
  border-radius: 4px;
  outline: none;
  border-top-right-radius: 0;
  border-bottom-right-radius: 0;
}
#newletter button{
  background-color: #088178;
  color: #fff;
  white-space: nowrap;
  border-top-left-radius: 0;
  border-bottom-left-radius: 0;
} 

footer{
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
}
footer .col {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  margin-bottom: 20px;

  
}
footer .logo{
  margin-bottom: 30px;
}
footer h4{
  font-size: 14px;
  padding-bottom: 20px;
}
footer p{
  font-size: 13px;
  margin: 0 0 8px 0;
}
footer a{
  font-size: 13px;
  text-decoration: none;
  color: #222;
  margin-bottom: 10px;
}
footer .follow{
  margin-top: 20px;


}
footer .follow i{
  color: #1a1a1a;
  padding-right: 4px;
  cursor: pointer;


}
footer .install .row img{
  border: 1px solid #088178;
  border-radius: 6px;

}
footer .install img{
  margin: 10px 0 15px;
}
footer .follow i:hover,
footer a:hover{
  color: #088178;

}
footer .copyright{
  width: 100%;
  text-align: center;

}
#page-header{
  background-image: url("images/b1\ \(1\).jpg");
  width: 100%;
  height: 40vh;
  background-size: cover;
  display: flex;
  justify-content: center;
  text-align: center;
  flex-direction: column;
  padding: 14px;

}
#page-header h2,
#page-header p {
color: #fff;
}
#pagination{
  text-align: center;
}
#pagination a{
  text-decoration: none;
  background-color: #088178;
  padding: 15px 20px;
  border-radius: 4px;
  color: #fff;
  font-weight: 600;
}
#pagination a i{
  font-size: 16px;
  font-weight: 900;

}
#prodetails{
  display: flex;
  margin-top: 20px;
}
#prodetails .single-pro-image {
  width: 40%;
  margin-right: 50px;
}
.smal-img-group{
  display: flex;
  justify-content: space-between;
}
.small-img-col {
  flex-basis: 24%;
  cursor: pointer;
}

#prodetails .single-pro-details{
  width: 50%;
  padding-top: 30px
}
#prodetails .single-pro-details h4{
  padding: 40px 0 20px 0;
 }
 #prodetails .single-pro-details h2{
  font-size: 26px;
 }
 #prodetails .single-pro-details select{
  display: block;
  padding: 5px 10px;
  margin-bottom: 10px;

 }
 #prodetails .single-pro-details input{
  width: 50px;
  height: 47px;
  padding-left: 10px;
  font-size: 16px;
  margin-right: 10px;
 }
 #prodetails .single-pro-details input:focus{
  outline: none;
 }
 #prodetails .single-pro-details button{
  background-color: #088178;
  color: #fff;
 }
 #prodetails .single-pro-details span{
    line-height: 25px;
 }
@media(max-width:799px){
  .section-p1 {
    padding: 40px 40px;
  }
  #navbar {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: flex-start;
    position: fixed;
    top: 0;
    right: -300px;
    height: 100vh;
    width: 300px;
    background-color: #e3e6f3;
    box-shadow: 0 40px 60px rgba(0, 0, 0, 0.1) ;
    padding: 80px 0 0 10px;
    transition: 0.3s;
  }
  #navbar.active {
    right: 0px;
  }

  #navbar li{
    margin-bottom: 25px;
  }
  #mobile{
    display: flex;
    justify-content: center;
  }
  #mobile i{
    color: #1a1a1a;
    font-size: 24px;
    padding-left: 20px;
  }
}
#clos{
  display: initial;
  position: absolute;
  top: 30px;
  left: 30px;
  color: #222;
  font-size: 24px;

}
#lg-bag{
  display:"";
  
}
#hero {

	height: 70vh;
	padding: 0 80px;
  background-position: top 30% right 30%;
}
#feature {
	justify-content: center;
	
}
#feature .fe-box {
	margin: 15px 15px;
}
#product1 .pro-conatiner {
	justify-content: center;

}

#product1 .pro {
 
  margin: 15px; /* Ajout d'une marge latérale pour équilibrer */
  
}

/* Supprimez les flottants après chaque ligne */



#banner {
	height: 20vh;
	
	
  
}
#sm-banner .banner-box {

	min-width: 100px;
	height: 30vh;
	
}
#sm-banner .banner-box2 {

  min-width: 100px;
	height: 30vh;

}
#baner3 {
	
	padding: 0 40px;
}

#baner3 .banner-box {

	width: 28%;

}

#newletter .form {
	width: 70%;
} 
@media (max-width: 447px) {
  .section-p1 {
    padding: 20px;
  }
  #header {
  
    padding: 10px 30px;
    
  }
  h1 {
    font-size: 38px;
  }
  h2 {
    font-size: 32px;
   ;
  }
  #hero {
    padding: 0 20px;
    background-position: 55%;
  }
  #feature {
    justify-content: space-between;
  }
  #feature .fe-box {
    width: 155px;
   
    margin: 0 0 15px 0 ;
  }
}


****************************************************************************************************************************************************


