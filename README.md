# system
webdev proj
<?php
   $Email = $password = "";
   $EmailErr = $passwordErr = "";
   if($_SERVER["REQUEST_METHOD"] == "POST"){
       if(empty($_POST["email"])){
           $EmailErr = "Email is Required";
       }else{
           $Email = $_POST["email"];
       }

       if(empty($_POST["password"])){
           $passwordErr = "Password is Required";
       }else{
           $password = $_POST["password"];
       }

       if($Email && $password){
           include("Connections.php");
           $query = "SELECT * FROM login_table WHERE Email = '$Email'";
           $check_email = mysqli_query($Connections, $query);
           $check_email_row = mysqli_num_rows($check_email);

           if($check_email_row > 0){
               while($row = mysqli_fetch_assoc($check_email)){
                   $db_password = $row["password"];
                   if($password == $db_password){
                       $db_account_type = $row["account_type"];
                       if($db_account_type == "1"){
                           echo"<script>window.location.href='admin.php';</script>";
                       }else{
                           echo"<script>window.location.href='user.php';</script>";
                       }
                   }else{
                       $passwordErr = "Invalid Password";
                   }
               }
           }else{
               $EmailErr = "Email is not Registered";
           }
       }
   }
?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login/Signup Page</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
  
    <div class="container">
        <form method="POST" action="<?PHP htmlspecialchars($_SERVER ["PHP_SELF"]); ?>">
        <div class="login form">
            <header>LOGIN</header>
            <span><?php echo $EmailErr?></span>
            <input type="email" name="email" placeholder="Enter your email" value="<?php echo $Email ?>">
            <span><?php echo $passwordErr?></span>

            <input type="password" name="password" placeholder="Enter your Password">
            <button type="submit" value="Login" class="button">Login</button>
            <div class="signup">
            <div class="">Don't have an account?
                <a href="signup.php">Signup</a>
            </div>
        </div>
    </div>
</form>
</body>
</html>
