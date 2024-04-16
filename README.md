# INSTALLATION INSTRUCTION

# DEPLOYEMENT
OPEN Portainer Go to Stacks > add stack
IN 'Name' Fild set wsdn 
SELECT Repository 
IN 'Repository URL *' Fild copy https://github.com/Taldorus/Docker-WebServer
IN PAGE BOTTOM CLick 'Deploy the stack'

# CONFIGURATION
Copy volumes folder (https://github.com/Taldorus/Docker-WebServer/tree/main/volumes) to your server 
GO TO Created container 'wdsn-apache2-php' in volumes section and get all Host/volume paths and replace the folowing command line
sudo cp -r /volumes/apache2/sites-available/. /Host/volume/apache2/sites-available
sudo cp -r /volumes/apache2/sites-enabled/. /Host/volume/apache2/sites-enabled
sudo cp -r /volumes/www/. /Host/volume/www

# CREATE DATA BASE AND ADD MYSQL USER WITH PHPMYADMIN
Go to localhost:8000 
IP : (Get it from portainer the wdsn-mysql container ip)
USER : root
PASS : rootpassword
Create new database (dbu)
Create new user 
USERNAME : dbworker
PASSWORD : dbworkerpassword

# UPDATE SCRIPT
sudo nano /Host/volume/index.php
UPDATE @IP = (Get it from portainer the wdsn-mysql container ip)

$servername = '@IP';
$dbname = 'dbu';
$username = 'dbworker';
$password = 'dbworkerpassword';
try{
    $conn = new PDO("mysql:host=$servername;dbname=$dbname;port=3306", $username, $password);
    $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    echo 'Connexion réussie';
}
catch(PDOException $e){
    echo "Erreur : " . $e->getMessage();
}

# RESTART CONTAINER
Restart wdsn-apache2-php container

DONE !!!
