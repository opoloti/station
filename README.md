
> Petrol station system is a PHP based Information Pool System (or simply a Petrol station Website), consisting of a complete Login/Registration system, User Profile system, System user questions,users list excel report .


# Table of Contents

* [Installation](#installation)
  * [Requirements](#requirements)
  * [Installation Steps](#installation-steps)
  * [Getting Started](#getting-started)
* [Features](#Features)
* [Components](#Components)
  * [Languages](#Languages)
  * [Development Environment](#Development-Environment)
  * [Database](#database)
  * [DBMS](#DBMS)
  * [API](#api)
  * [Frameworks and Libraries](#Frameworks-and-Libraries)
  * [External PLugins](#external-plugins)
* [Details](#details)
* [Application Files](#application-files)
* [Future Improvements](#future-improvements)



## Installation

#### Requirements
* PHP
* Apache server
* MySQL Database
* SQL
* phpMyAdmin

> All of these requirements can be completed at once by simply installing a server stack like `Wamp` or `Xampp` etc.

#### Installation Steps
1. Import the `klik_database.sql` file in the `includes` folder into phpMyAdmin. There is no need for any change in the .sql file. This will create the database required for the application to function.

2. Edit the `dbh.inc.php` file in the `includes` folder to create the database connection. Change the password and username to the ones being used within current installation of `phpMyAdmin`. There is no need to change anything else.

```php
$serverName = "localhost";
$dBUsername = "root";
$dBPassword = "examplePassword";
$dBName = "klik_database";

$conn = mysqli_connect($serverName, $dBUsername, $dBPassword, $dBName, 3307);

if (!$conn)
{
    die("Connection failed: ". mysqli_connect_error());
}
```
> The port number does not need to be changed under normal circumstances, but if you are running into a problem or the server stack is installed on another port, feel free to change it, but do so carefully.

3. Edit the `email-server.php` file in the `includes` folder and change the variables accordingly:

  * `$SMTPuser` : email address on `gmail`
  * `$SMTPpwd` : email address password
  * `SMTPtitle` : hypothetical company's name
  * `Domain` : Domain of the website, like localhost on local server or if on live domain, something like www.hypotheticalwebsite.com

```php
$SMTPuser = 'fuel@gmail.com';   
$SMTPpwd = 'some-example-password';
$SMTPtitle = "petrolstation inc.";
$Domain = 'localhost';
```
> This step is mainly for setting up an email account to enable the `Qn3.php` and `Qn4.php`, all of which require mailing.

> In the current stage of the application, only `Gmail` accounts are supported.


#### Getting started
The database file already contains a lot of sample data and users. Most users in the database have the same password as their usernames except for a few. It is not possible to signup as an administrator through the application, since I decided that it was an exploitable weakness. Therefore, you will have to create an account and manually go to the `users` table in the database to change the userLevel of that account to `1` from `0`.
> 0 Level means a normal user and Level 1 means admin


## Features

* [Application Dashboard](#application-Dashboard)
* [Login/Registration and User Authentication](#Login-Registration-and-User-Authentication)
* [User Profile System](#user-profile-system)
* [Security](#security)


## Components

#### Languages
```
PHP 5.5.12
JavaScript ES 6
HTML5
CSS3
```

#### Development Environment
```
WampServer Stack  5.6.17
Windows 10
```

#### Database
```
MySQL Database 8.0.13
```

#### DBMS
```
phpMyAdmin 4.1.14
```

#### API
```
MySQLi APIs
```

#### Frameworks and Libraries
```
JQuery v3.3.1
BootStrap v4.2.1
```

#### External Plugins
```
[PHPMailer 6.0.6](https://github.com/PHPMailer/PHPMailer)
```
> This was used for creating a `mail server` on `Windows localhost`, since unlike in Linux, there isnt one already installed in windows. This plugin was used for the sending and receiving of emails on localhost.

## Details

> Details of important Features of the Application

### Application Dashboard

The Dashboard provides a welcome interface that verifies that its really `logged-in user` that we are interacting with, as well as a link to the profile and the profile-editing page. The checkboxes helps user to acertify their identity and the two blue buttons on the lower right corner provides a prominent links one  to the next page which showcases the `first qustion to user` and  the other link to` goodbye page`

The upper navigation bar  provides access to 3 features: the `refresher` which refreshes the page,`the settings menu` on the with dropdown that contains `view users report` , `My profile`and  `Edit profile`, each of these lead to interfaces where user can view different users list, view profile and edit profile respectively and the `the logout` feature.


### Login/Registration and User Authentication

Petrol station service system supports a complete login/registration and User Profile system. On startup, the application shows options for logging in, signing up. Each user can make a unique username which cannot be changed later. The user `passwords` are `hashed` before storing in database so even admins do not have access to the original passwords as well. Additional User information include `Full Name`, `email`, `Profile Image`, `Profile Headline`, `Gender` and `Bio`.

The app uses several authentication methods for signing up and logging in. It checks for `empty fields`, `wrong username`, `wrong password`, `SQL errors`, `server errors` and in case of signing up, `corrupted image` or `wrong image type` errors

### User Profile System

Petrol station service system has a complete `User profile system`. Each user is assigned a profile on signing up, with which the user can interact with the app's features. The user's full name, headline and bio, as well as profile image are also included, meaning that anyone can signup with setting those. There is also a case, the user will be assigned a default user image if he/she doesn't upload one.

The `user profile` can be accessed through the option in the settings menu on the navigation bar. The profile page shows the basic User information like username, full name, gender, headline and bio. 

There is also a `Profile Editing System` which allows the User to edit his profile information. It can be accessed through the respective option in the settings menu in the navigation bar or by simply clicking the pencil icon next to the user profile image on the profile card. The system allows the user to change most of his information except for the username, which cannot be changed. All fields already have the current information, so the user does not have to type everything all over again if he only wishes to slightly edit the current information. 


### Security

* `Password hashing` before storing in database.
* Filtering of information obtained from `$_GET` and `$_POST` methods to prevent `header injection`.
* Implementation of `MySQLi Prepared Statements` for **advanced** database security.

  **Example:**
```php
$sql = "select uidUsers from users where uidUsers=?;";
        $stmt = mysqli_stmt_init($conn);
        if (!mysqli_stmt_prepare($stmt, $sql))
        {
            header("Location: ../signup.php?error=sqlerror");
            exit();
        }
        else
        {
            mysqli_stmt_bind_param($stmt, "s", $userName);
            mysqli_stmt_execute($stmt);
            mysqli_stmt_store_result($stmt);
       }
```

## Application Files

> A list of all main Application Features and their respective front-end and back-end files.

|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; Feature &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;|Front-end Files| Back-end Files |
| ----------- | -- | -- |
|Dashboard| `index.php (Main Dashboard)` | 
|Signup/ Login| `signup.php`, `login.php`| `signup.inc.php`, `login.inc.php`, `logout.inc.php`|
|Profile System| `profile.php`, `edit-profile.php` | `profileUpdate.inc.php`|
|Image Upload| N/A| `upload.inc.php`|
| Users report list| `users-view.php`|N/A|

> **Note:** The GUI files are in the `root directory`, and the `backend files` are present in the `includes` folder. Similarly, all CSS and JS files are present in their prespective `css` & `js` directories. The main HTML structuring file the `HTML-head.php` which also resides in the includes folder

## Future Improvements
* Continuous Bug fixes and improvements

