# Auto-Check-In-FUNCTOR-Node
This script is usable on Tampermonkey or Violetmonkey extension. This is for educational purpose only.

## Overview  
This script automates the login and daily check-in process on the FUNCTOR Network platform, ensuring consistent activity without manual intervention.

## Features  
- Automates login using your email and password.  
- Performs daily check-in and mining tasks.  
- Refreshes the page periodically to maintain session activity.  

## Requirements  

### Extensions  
1. **[Tampermonkey](https://chromewebstore.google.com/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo)** (or **Violentmonkey**)  
   - Required to run the script.  

2. **[Ignore X-Frame Options](https://chromewebstore.google.com/detail/ignore-x-frame-options/ammjifkhlacaphegobaekhnapdjmeclo)**  
   - Enhances performance by bypassing browser restrictions.  

### Browser Compatibility  
- **Google Chrome** (recommended)  
- Microsoft Edge  
- Mozilla Firefox  

---

## Installation  

### Step 1: Install Required Extensions  
1. Install **Tampermonkey** or **Violentmonkey** using the links above.  
2. Install **Ignore X-Frame Options** to improve script execution.  

### Step 2: Add the Script  
1. Open the **Tampermonkey Dashboard** by clicking the extension icon in your browser.  
2. Click **Create a New Script**.  
3. Copy and paste the following script into the editor:  

    ```javascript
    // ==UserScript==
    // @name         FUNCTOR Network Auto Check-In
    // @namespace    Nodebot by Juliwicks
    // @version      v.1.0.0
    // @description  Automates login and check-in for the FUNCTOR Network platform.
    // @author       Nodebot by Juliwicks
    // @match        https://node.securitylabs.xyz/*
    // @icon         https://www.google.com/s2/favicons?sz=64&domain=securitylabs.xyz
    // @grant        none
    // ==/UserScript==

    (function() {
        'use strict';

        // User credentials
        const userEmail = 'YOUREMAIL'; // Replace with your email
        const userPassword = 'YOURPASSWORD'; // Replace with your password

        // Schedule periodic tasks
        setTimeout(autoCheckIn, 10000);
        setInterval(autoCheckIn, 60000);
        setTimeout(() => location.reload(), 3 * 60 * 60 * 1000);

        function autoCheckIn() {
            if (isLoggedIn()) {
                const checkInButton = document.querySelector('.lab-amount button');
                if (checkInButton && (checkInButton.innerText === 'CHECK IN' || checkInButton.innerText.includes('MINE'))) {
                    checkInButton.click();
                    console.log('Check-in performed.');
                }
            } else {
                initiateLogin();
                setTimeout(enterCredentials, 2000);
            }
        }

        function isLoggedIn() {
            return !document.querySelectorAll('.modal-body button').some(btn => btn.innerText.includes('EMAIL'));
        }

        function initiateLogin() {
            document.querySelectorAll('.modal-body button').forEach(button => {
                if (button.innerText.includes('EMAIL')) button.click();
            });
        }

        function enterCredentials() {
            const emailInput = document.querySelector('.terminal-input-container input');
            if (emailInput) {
                emailInput.focus();
                setNativeValue(emailInput, userEmail);
                emailInput.blur();
                emailInput.dispatchEvent(new KeyboardEvent('keydown', { key: 'Enter', bubbles: true }));
                setTimeout(() => {
                    const passwordInput = document.querySelector('.terminal-input-container input');
                    if (passwordInput) {
                        passwordInput.focus();
                        setNativeValue(passwordInput, userPassword);
                        passwordInput.blur();
                        passwordInput.dispatchEvent(new KeyboardEvent('keydown', { key: 'Enter', bubbles: true }));
                    }
                }, 2000);
            }
        }

        function setNativeValue(element, value) {
            const valueSetter = Object.getOwnPropertyDescriptor(element, 'value').set;
            valueSetter.call(element, value);
            element.dispatchEvent(new Event('input', { bubbles: true }));
        }
    })();
    ```

4. Replace `YOUREMAIL` and `YOURPASSWORD` with your FUNCTOR Network credentials or just leave it if you don't want to auto-login
5. Save the script.  

### Step 3: Enable the Script  
1. Visit the FUNCTOR Network website: `https://node.securitylabs.xyz/`.  
2. Ensure the script is active in Tampermonkey or Violentmonkey.  

---

## Tips for Best Performance  
- Use **Ignore X-Frame Options** to bypass restrictions and ensure smoother execution.  
- Keep your browser and extensions up to date.  

For questions or support, feel free to reach out to **Nodebot by Juliwicks**!  
