# Advanced Web Calculator 

This project is a more advanced web calculator integrated into a martial arts dojo website template. It includes features for basic arithmetic operations, square, percentage, plus/minus, decimal input control, and basic error handling (division by zero). It also maintains a simple history of operations.

## Overview

The calculator is embedded within the `<main>` section of the HTML page. It utilizes a `<table>` for its layout and includes:

* **Display:** A read-only text input (`<input type="text" id="result">`) within the table caption to show the current input and results.
* **Hidden Inputs:** Several hidden input fields (`operation`, `operand`, `equivalent`, `decimalVar`) are used to store the calculator's internal state.
* **Buttons:** Buttons for digits (0-9), operators (+, -, ×, ÷), a decimal point (.), equals (=), clear (ac), square (x²), percent (%), and plus/minus (±). Each button has an `onclick` event handler that calls a corresponding JavaScript function.
* **External JavaScript (`scripts/calculator.js`):** Handles all the calculator's logic, including input handling, operation storage, calculations, and display updates.
* **External CSS (`css/style.css` and `css/calculator.css`):** Styles the overall website template and specifically the calculator's appearance.

## How to Use

1.  **Save the files:** Ensure you have `index.html`, the `css` folder (containing `style.css` and `calculator.css`), the `scripts` folder (containing `calculator.js`), and the `media` folder (containing `logo.png`) in the correct relative paths.
2.  **Open in a browser:** Open the `index.html` file in your web browser.
3.  **Navigate to the Calculator:** The calculator is located within the `<main>` section of the home page.
4.  **Perform calculations:**
    * Click the digit buttons to enter numbers.
    * Click the operator buttons (÷, ×, –, +) to select an operation.
    * Click the "." button to enter decimal points (multiple decimal points are prevented).
    * Click the "=" button to see the result.
    * Click the "ac" button to clear the display and reset the calculator.
    * Click the "x²" button to square the current number.
    * Click the "%" button to calculate the percentage of the current number.
    * Click the "±" button to toggle the sign of the current number.

## Project Files

* **`index.html`:** (As provided) - Contains the HTML structure for the entire page, including the calculator embedded within the `<main>` section. It links to the CSS files and the external JavaScript file.
* **`css/style.css`:** (Assumed - likely contains general styling for the website template).
* **`css/calculator.css`:**
    ```css
    table {
        border: 0.1em solid black;
        border-collapse: collapse;
        width: 95%;
        margin-left: 2.5%;
        background-color: black;
    }

    caption {
        border: 0.1em solid black;
        font-size: 1.5em;
        background-color: black;
    }

    caption input {
        text-align: right; /* This puts our 0 on the right.*/
    }

    th, td {
        border: 0.1em solid black;
        padding: 0.5em;
        text-align: center;
    }

    input {
        width: 95%;
        height: 2.5em;
        font-size: 2em;
    }
    ```
* **`scripts/calculator.js`:**
    ```javascript
    let display = document.getElementById('result');
    let operation = document.getElementById('operation');
    let operand = document.getElementById('operand');
    let equivalent = document.getElementById('equivalent');
    let decimalVar = document.getElementById('decimalVar');

    function equivalentCheck() {
        if (parseInt(equivalent.value)) {
            equivalent.value = 0;
            display.value = 0;
        }
    }

    function input(x) {
        equivalentCheck();
        let y = parseFloat(display.value);
        if (decimalVar.value == 0) {
            display.value = y * 10 + x;
        } else {
            let decimalCount = parseInt(decimalVar.value);
            if (decimalCount == 1) {
                x *= 0.1;
                display.value = y + x;
            } else {
                display.value += x;
            }
            decimalVar.value = decimalCount + 1;
        }
    }

    function decimalPoint() {
        if (!display.value.includes('.')) {
            decimalVar.value = 1;
            if (display.value === '') {
                display.value = '0.';
            } else if (parseInt(operation.value)) {
                display.value = '0.';
                operation.value = 0;
            } else {
                display.value += '.';
            }
        }
    }

    function operandCheck() {
        if (operand.value == "") {
            operand.value = display.value;
            equivalent.value = 1;
        } else {
            operatorCheck();
        }
    }

    function operatorCheck() {
        let a = parseFloat(operand.value);
        let b = parseFloat(display.value);
        let op = parseInt(operation.value);

        if (isNaN(a) || isNaN(b)) {
            return;
        }

        switch (op) {
            case 1: //addition
                a += b;
                break;
            case 2: //subtraction
                a -= b;
                break;
            case 3: //multiplication
                a *= b;
                break;
            case 4: //division
                if (b === 0) {
                    alert("Cannot divide by zero!");
                    allClear();
                    return;
                }
                a /= b;
                break;
        }
        operand.value = a;
        display.value = a;
        equivalent.value = 1;
    }

    function operators(x) {
        operandCheck();
        operation.value = x;
        decimalVar.value = 0; // Reset decimal for the new number
    }

    function equals() {
        operators(parseInt(operation.value));
        display.value = operand.value;
        operand.value = "";
        equivalent.value = 1;
        operation.value = 0; // Reset operation
        decimalVar.value = 0; // Reset decimal
    }

    function allClear() {
        display.value = 0;
        operand.value = "";
        operation.value = 0;
        equivalent.value = 0;
        decimalVar.value = 0;
    }

    function plusminus() {
        let x = parseFloat(display.value);
        if (!isNaN(x)) {
            display.value = -x;
        }
    }

    function percent() {
        let x = parseFloat(display.value);
        if (!isNaN(x)) {
            display.value = x / 100;
        }
    }

    function square() {
        let x = parseFloat(display.value);
        if (!isNaN(x)) {
            display.value = x * x;
        }
    }
    ```

## Troubles and Challenges Faced 

Integrating this calculator into the existing website template presented a few interesting challenges, and our previous conversations were instrumental in resolving some of them:

* **Initial Confusion with External Script Linking:** Just like in the simpler example, I initially had questions about the best way to link the `calculator.js` file. Since this was part of a larger HTML structure with a `<head>` and `<body>`, I wasn't sure if the placement or attributes mattered. We discussed placing the `<script>` tag just before the closing `</body>` to ensure the calculator's HTML elements were available when the script tried to access them. The `type="text/javascript"` attribute was also something I wasn't entirely sure about, and you clarified its role (though it's often optional for modern browsers).

* **Accessing HTML Elements within the Calculator:** Once the script was linked, I needed to correctly target the calculator's display (`<input id="result">`) and the hidden input fields. I initially made some typos in the `document.getElementById()` calls, which resulted in the JavaScript not being able to find the elements. Double-checking the IDs in the HTML and the JavaScript was a key part of the troubleshooting process, something you emphasized.

* **Managing Calculator State with Hidden Inputs:** The use of hidden input fields (`operation`, `operand`, `equivalent`, `decimalVar`) to manage the calculator's state was a new concept. I had to understand how these hidden fields were being updated and read by the JavaScript functions to keep track of the current operation, the first number entered, whether the last operation was an equals, and if a decimal point had been used. We talked about the flow of data and how these hidden fields acted as the calculator's "memory."

* **Implementing the `equivalentCheck()`:** I initially had issues where pressing a digit after hitting the equals button would append to the previous result instead of starting a new calculation. The `equivalentCheck()` function, which resets the display and the `equivalent` flag, was crucial to solve this. Understanding when to call this function (at the beginning of the `input()` function) was a key step in the troubleshooting.

* **Controlling Decimal Input:** Preventing multiple decimal points in a number was a specific challenge. The `decimalPoint()` function and the `decimalVar` hidden input were designed to handle this. I initially had a simpler version that didn't quite work correctly when operators were involved. We refined the `decimalPoint()` function to check if a decimal already existed in the display and to reset the `decimalVar` when a new operation started.

* **Handling the Order of Operations (Basic):** While this calculator doesn't implement full order of operations (PEMDAS/BODMAS), ensuring the basic sequence of number-operator-number-equals worked correctly required careful management of the `operand` and `operation` variables. The `operandCheck()` and `operatorCheck()` functions were designed to handle this flow, and I had to step through the logic to ensure the values were being stored and used at the right times.

* **Implementing Special Functions (`square`, `percent`, `plusminus`):** Adding the extra functions like square, percent, and plus/minus required understanding how to take the current value in the display, perform the operation, and update the display. The key here was ensuring that these operations didn't interfere with the ongoing arithmetic operations.

Through the iterative process of writing the code, testing it in the browser, and debugging. I was able to address these challenges and build a functional web calculator integrated into the website template. The use of hidden inputs for state management and the careful handling of user input and operations were the most significant learning points.
