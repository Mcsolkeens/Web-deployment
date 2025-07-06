# EC2 Web Deployment Workflow

This repository includes a GitHub Actions workflow for automatic deployment of website files to an AWS EC2 instance when code is pushed to the webdeploy branch.
Developer code can be clone to your repo: git clone https://github.com/whxitte/Project-Taaza.git

 # Workflow Overview

Trigger: On push to webdeploy branch
Action: 
Connects to a remote EC2 instance, installs Apache & PHP if needed, and deploys website files using rsync.

# Workflow Steps

    Checkout Code
    Uses the latest version of the repo on push.

    Setup SSH Key
    The private SSH key is securely retrieved from GitHub Secrets and saved as key.pem for use in the SSH session.

    Install Apache & PHP
    Connects to the EC2 instance via SSH and installs required software.

    Deploy Website Files
    Uses rsync over SSH to transfer files to /var/www/html/ on the EC2 instance.

# Security Considerations
   Best Practices Implemented

    Secrets Management:
    Sensitive information such as the SSH private key (EC2_SSH_KEY) and EC2 host address (EC2_HOST) are stored securely using GitHub Secrets.

    File Permissions:
    The SSH key file (key.pem) is given 600 permissions to restrict access.

    Strict Host Key Checking Disabled (Temporary):
    The line StrictHostKeyChecking=no is used to avoid manual host verification. For production, you should consider managing known hosts securely.

    Minimal Exposure:
    The SSH key is never printed or logged. It is excluded from deployment using --exclude=key.pem.

 Testing

To test deployment, push any changes to the webdeploy branch. GitHub Actions will execute the deployment pipeline automatically.

# Secrets Required

To enable this workflow, you must define the following repository-level secrets:

Secret Name	Description
EC2_SSH_KEY	Private SSH key for EC2 access
EC2_HOST	Public IP or DNS of the EC2 instance

# Important Note

Never commit your private SSH key or sensitive configuration files directly to the repository. Always use GitHub Secrets for secret management.


# Deployment Challenge: Apache Displaying Default "It Works!" Page Instead of PHP Site

Problem Encountered

After deploying the code to your EC2 instance. Launch the web site using the public EC2 IP:
http://publicIP
Note: ensure your EC2 has internet access and in public subnet. Equally enable http traffic from anywhere on the security group



Apache displayed the default "It works!" page, instead of loading your website or PHP application.

Initial Observations

This issue occurred because:

    Apache was serving its default content.

    The original index.php in your project wasn’t being rendered.

    PHP was not yet installed or enabled on the server.

    Apache was prioritizing its default index.html or not executing PHP properly.

# Solution 
- Before Testing PHP

To avoid a conflict with your original application file, you renamed the existing index.php in your deployment code to:

sudo mv /var/www/html/index.php /var/www/html/index2.php

This ensured that the Apache default page or your own file wouldn’t interfere with the PHP test file.
🔧 1. Install PHP and Required Modules

On Debian run:

sudo apt update
sudo apt install -y php libapache2-mod-php
sudo systemctl restart apache2

This enabled Apache to process .php files.
🧪 2. Test PHP Setup

You then created a test file to verify PHP was working:

echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/index.php

Visiting:

http://<your-ec2-public-ip>/index.php

✅ Displayed the PHP info page successfully.

# Cleanup and Restore

After confirming PHP was functional:

sudo rm /var/www/html/index.php
sudo mv /var/www/html/index2.php /var/www/html/index.php

This restored your original site file and removed the temporary test page.












# Project-Taaza

<a href="https://www.instagram.com/whxitte"><img src="https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white" /></a> <a href="https://www.linkedin.com/in/sethusatheesh/"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white" /></a> <a href="https://medium.com/@S3THU"><img src="https://img.shields.io/badge/Medium-12100E?style=for-the-badge&logo=medium&logoColor=white" /></a>

Taaza is a full website project. It is a responsive restaurant website which provides online services related to a restaurant

# Key Features

    🍪 Good Template for a restaurant website
    🍪 Main pages: Home, About, Services, Our Menu, Contact, Dashboard, Register/Login
    🍪 Register/Login: Forgot password, Mail sending etc..
    🍪 Main services website offer(modules): Food ordering (Add to cart --> checkout || Increase/decrease quantity), Table booking (Normal & vip || Book & View Bookings), Vip membership (For discount and more)
    🍪 Bill printing functionality (PDF generation) [Discounted for vip & non discounted]
    🍪 Email sending functionality (During registration, forgot password, delete order by admin like events)
    🍪 Admin functionality (To manage users, orders feedbacks, enabling/disabling pages, giving message etc)

[![Typing SVG](https://readme-typing-svg.demolab.com?font=Fira+Code&pause=1000&width=435&lines=RESPONSIVE+WEBSITE+TEMPLATE;)](https://git.io/typing-svg)

# Screenshot

![Taaza](https://github.com/WH1T3-E4GL3/Project-Taaza/assets/118425907/7d54c3f1-656d-471b-860d-4880cd93e415)



This is a web template that I have created and made available for private use. Please note the licensing terms before using this template.

## License

This work is licensed under the [Creative Commons Attribution-NonCommercial 4.0 International License](http://creativecommons.org/licenses/by-nc/4.0/).

![Creative Commons License](https://i.creativecommons.org/l/by-nc/4.0/88x31.png)

### Usage

You are free to download and use this template for personal projects, educational purposes, or for non-commercial use. However, any public or commercial use, including publishing, marketing, or using it in a commercial project, is not permitted without explicit permission.

### Adaptations

If you choose to adapt this template for your own needs, you must share your adaptations under the same Creative Commons Attribution-NonCommercial 4.0 International License. This ensures that others can benefit from your modifications while respecting the original creator's intentions.

Please read the full license text at [http://creativecommons.org/licenses/by-nc/4.0/](http://creativecommons.org/licenses/by-nc/4.0/) for more details.


# How to run
Create a database, tables, and columns with following details using phpmyadmin (Detailed structure and query given below this box):

    Database name: taaza_db
    Table names: ⦿ admin
                     ↳colum names:  id, email, name, password, resettoken, resettokenexpire, enable_table_booking, enable_menu_page

                 ⦿ admin_message
                     ↳colum names:  id, message, enable_meessage

                 ⦿ contact
                    ↳colum names:  id, email, timestamp 	

                 ⦿ feedback
                     ↳colum names:  feedback_id, user_email, feedback_text, timestamp

                 ⦿ lend_hand
                     ↳colum names:   id, name, email, amount, timestamp, show_detail 

                 ⦿ menu_items
                     ↳colum names:    id, name, description, category, price, quantity, available, image_path 	
                
                 ⦿ orders
                    ↳colum names:  order_id ,name, email, address, item, quantity, total_price, timestamp 	

                 ⦿ registered_users
                     ↳colum names: name, email, password, gender, state, district, verification_code, is_verified, resettoken, resettokenexpire, is_vip

                 ⦿ table_booking_ground
                     ↳colum names: id, name, email, section, seat, date, time, payment 

                 ⦿ table_booking_vip
                     ↳colum names:  id, name, email, section, seat, decor, date, time, payment 

# Database Structure & creating queries [Table By Table]
        
        Database name: taaza_db

        -------------------------

        CREATE DATABASE taaza_db;
        
---------------------------------------------------------------     
        == Table structure for table admin
        
        |------
        |Column|Type|Null|Default
        |------
        |id|int|No|
        |email|varchar(50)|No|
        |name|varchar(100)|No|
        |password|varchar(190)|No|
        |resettoken|varchar(190)|No|
        |resettokenexpire|date|Yes|NULL
        |enable_table_booking|tinyint|No|
        |enable_menu_page|tinyint|No|

    -----------------------------------------------------

    Create 'admin' table Query :
    
        CREATE TABLE admin (
        id INT NOT NULL,
        email VARCHAR(50) NOT NULL,
        name VARCHAR(100) NOT NULL,
        password VARCHAR(190) NOT NULL,
        resettoken VARCHAR(190) NOT NULL,
        resettokenexpire DATE DEFAULT NULL,
        enable_table_booking TINYINT NOT NULL,
        enable_menu_page TINYINT NOT NULL
    );

---------------------------------------------------------------
        == Table structure for table admin_message
        
        |------
        |Column|Type|Null|Default
        |------
        |id|int|No|
        |message|varchar(5000)|No|
        |enable_meessage|tinyint|No|

    -----------------------------------------------------

    Create 'admin_message' table Query :

        CREATE TABLE admin_message (
        id INT NOT NULL,
        message VARCHAR(5000) NOT NULL,
        enable_message TINYINT NOT NULL
    );

---------------------------------------------------------------
        
        == Table structure for table contact
        
        |------
        |Column|Type|Null|Default
        |------
        |id|int|No|
        |email|varchar(90)|No|
        |timestamp|timestamp|No|CURRENT_TIMESTAMP
        
    -----------------------------------------------------

    Create 'contact' table Query :

        CREATE TABLE contact (
        id INT NOT NULL,
        email VARCHAR(90) NOT NULL,
        timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP
    );

---------------------------------------------------------------
        
        
        == Table structure for table feedback
        
        |------
        |Column|Type|Null|Default
        |------
        |feedback_id|int|No|
        |user_email|varchar(255)|No|
        |feedback_text|text|No|
        |timestamp|timestamp|Yes|CURRENT_TIMESTAMP

    -----------------------------------------------------

    Create 'feedback' table Query :

        CREATE TABLE feedback (
        feedback_id INT NOT NULL,
        user_email VARCHAR(255) NOT NULL,
        feedback_text TEXT NOT NULL,
        timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP
    );

---------------------------------------------------------------
        
        == Table structure for table lend_hand
        
        |------
        |Column|Type|Null|Default
        |------
        |id|int|No|
        |name|varchar(50)|No|
        |email|varchar(90)|No|
        |amount|int|No|
        |timestamp|timestamp|No|
        |show_detail|tinyint|No|

    -----------------------------------------------------

    Create 'lend_hand' table Query :

        CREATE TABLE lend_hand (
        id INT NOT NULL,
        name VARCHAR(50) NOT NULL,
        email VARCHAR(90) NOT NULL,
        amount INT NOT NULL,
        timestamp TIMESTAMP NOT NULL,
        show_detail TINYINT NOT NULL
    );
    
--------------------------------------------------------------- 
    
    == Table structure for table menu_items
    
    |------
    |Column|Type|Null|Default
    |------
    |//**id**//|int|No|
    |name|varchar(255)|No|
    |description|text|Yes|NULL
    |category|varchar(50)|Yes|NULL
    |price|decimal(10,2)|No|
    |quantity|int|No|0
    |available|tinyint(1)|No|1
    |image_path|varchar(255)|Yes|NULL
    
    ---------------------------------------------------
    
    CREATE TABLE menu_items (
        id int NOT NULL AUTO_INCREMENT,
        name varchar(255) NOT NULL,
        description text,
        category varchar(50),
        price decimal(10,2) NOT NULL,
        quantity int NOT NULL DEFAULT 0,
        available tinyint(1) NOT NULL DEFAULT 1,
        image_path varchar(255),
        PRIMARY KEY (id)
    );
    
------------------------------------------------
        
        == Table structure for table orders
        
        |------
        |Column|Type|Null|Default
        |------
        |order_id|int|No|
        |name|varchar(50)|No|
        |email|varchar(100)|No|
        |address|varchar(200)|No|
        |item|varchar(30)|No|
        |quantity|varchar(30)|No|
        |total_price|varchar(30)|No|
        |timestamp|timestamp|Yes|CURRENT_TIMESTAMP

    -----------------------------------------------------

    Create 'orders' table Query :

        CREATE TABLE orders (
        order_id INT NOT NULL,
        name VARCHAR(50) NOT NULL,
        email VARCHAR(100) NOT NULL,
        address VARCHAR(200) NOT NULL,
        item VARCHAR(30) NOT NULL,
        quantity VARCHAR(30) NOT NULL,
        total_price VARCHAR(30) NOT NULL,
        timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP
    );

---------------------------------------------------------------
        
        == Table structure for table registered_users
        
        |------
        |Column|Type|Null|Default
        |------
        |name|varchar(30)|No|
        |email|varchar(30)|No|
        |password|varchar(100)|No|
        |gender|varchar(18)|No|
        |state|varchar(30)|No|
        |district|varchar(30)|No|
        |verification_code|varchar(225)|No|
        |is_verified|int|No|0
        |resettoken|varchar(255)|Yes|NULL
        |resettokenexpire|date|Yes|NULL
        |is_vip|tinyint|No|

    -----------------------------------------------------

    Create 'registered_users' table Query :

        CREATE TABLE registered_users (
        name VARCHAR(30) NOT NULL,
        email VARCHAR(30) NOT NULL,
        password VARCHAR(100) NOT NULL,
        gender VARCHAR(18) NOT NULL,
        state VARCHAR(30) NOT NULL,
        district VARCHAR(30) NOT NULL,
        verification_code VARCHAR(225) NOT NULL,
        is_verified INT NOT NULL DEFAULT 0,
        resettoken VARCHAR(255) DEFAULT NULL,
        resettokenexpire DATE DEFAULT NULL,
        is_vip TINYINT NOT NULL
    );

---------------------------------------------------------------
        
        == Table structure for table table_booking_ground
        
        |------
        |Column|Type|Null|Default
        |------
        |id|int|No|
        |name|varchar(30)|No|
        |email|varchar(50)|No|
        |section|varchar(30)|No|
        |seat|varchar(30)|Yes|NULL
        |date|date|No|
        |time|varchar(50)|No|
        |payment|tinyint(1)|No|
        
    -----------------------------------------------------

    Create 'table_booking_ground' table Query :

        CREATE TABLE table_booking_ground (
        id INT NOT NULL,
        name VARCHAR(30) NOT NULL,
        email VARCHAR(50) NOT NULL,
        section VARCHAR(30) NOT NULL,
        seat VARCHAR(30) DEFAULT NULL,
        date DATE NOT NULL,
        time VARCHAR(50) NOT NULL,
        payment TINYINT NOT NULL
    );

---------------------------------------------------------------
        
        == Table structure for table table_booking_vip
        
        |------
        |Column|Type|Null|Default
        |------
        |id|int|No|
        |name|varchar(30)|No|
        |email|varchar(30)|No|
        |section|varchar(30)|No|
        |seat|varchar(30)|No|
        |decor|varchar(50)|No|
        |date|date|No|
        |time|varchar(20)|No|
        |payment|tinyint(1)|No|
        
    -----------------------------------------------------

    Create 'table_booking_vip' table Query :

        CREATE TABLE table_booking_vip (
        id INT NOT NULL,
        name VARCHAR(30) NOT NULL,
        email VARCHAR(30) NOT NULL,
        section VARCHAR(30) NOT NULL,
        seat VARCHAR(30) NOT NULL,
        decor VARCHAR(50) NOT NULL,
        date DATE NOT NULL,
        time VARCHAR(20) NOT NULL,
        payment TINYINT NOT NULL
    );





# Technologies Used

<img src="https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white" /> <img src="https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white" /> <img src="https://img.shields.io/badge/JavaScript-323330?style=for-the-badge&logo=javascript&logoColor=F7DF1E" /> <img src="https://img.shields.io/badge/PHP-777BB4?style=for-the-badge&logo=php&logoColor=white" /> <img src="https://img.shields.io/badge/jQuery-0769AD?style=for-the-badge&logo=jquery&logoColor=white" /> <img src="https://img.shields.io/badge/Bootstrap-563D7C?style=for-the-badge&logo=bootstrap&logoColor=white" /> <img alt="mysql" src="https://img.shields.io/badge/MySQL-005C84?style=for-the-badge&logo=mysql&logoColor=white"> <img src="https://img.shields.io/badge/Apache-D22128?style=for-the-badge&logo=Apache&logoColor=white" /> <img src= "https://img.shields.io/badge/Font_Awesome-339AF0?style=for-the-badge&logo=fontawesome&logoColor=white"> 

<img src="https://img.shields.io/badge/VSCode-0078D4?style=for-the-badge&logo=visual%20studio%20code&logoColor=white" /> <img src="https://img.shields.io/badge/ChatGPT-74aa9c?style=for-the-badge&logo=openai&logoColor=white"> <img src="https://img.shields.io/badge/Canva-%2300C4CC.svg?&style=for-the-badge&logo=Canva&logoColor=white" />

# Don't just copy, hit the star also😊



# Taaza - The website for restaurant.
This include front end design and backend code for a restaurant based website

This pack comes under Creative Commons Attribution-NonCommercial license (CC BY-NC). 

~Work by Sethu Satheesh
  
  ©Sethu    Reach me at [Instagram](https://www.instagram.com/whxite.exe/)

