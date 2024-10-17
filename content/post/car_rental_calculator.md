+++
author = "thounny"
title = "Car Rental Cost Estimator"
date = "2024-10-17"
description = "Building a Simple Car Rental Cost Estimator with JavaScript"
tags = [
    "code",
    "javascript",
]
categories = [
    "Code",
]
+++

# Building a Simple Car Rental Cost Estimator with JavaScript

## In this blog post, we’ll go over how to build a simple car rental cost estimator using HTML and JavaScript.

The goal is to calculate the total cost of renting a car based on the number of rental days, selected options (like toll tags, GPS, or roadside assistance), and whether the renter is under the age of 25 (which adds a surcharge). We will break down the code step-by-step so even beginners can follow along.

### The HTML Part – Setting up the User Interface

Let's first understand the structure of our web page, which is written in **HTML**. This part creates the form that the user interacts with to enter the number of rental days and choose additional options.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Car Rental</title>
  </head>
  <body>
    <h1>ACME Car Rental</h1>
    <label for="pickupDate"
      >Pickup Date:
      <input type="date" id="pickupDate" />
    </label>
    <label for="numberDays"
      >Number of days:
      <input type="number" id="numberDays" />
    </label>

    <h2>Options</h2>
    <label>
      <input type="checkbox" id="toll" />Electronic Toll Tag ($3.95/day)
    </label>
    <br />
    <label> <input type="checkbox" id="gps" />GPS ($2.95/day) </label>
    <br />
    <label>
      <input type="checkbox" id="roadside" />Roadside Assistance ($2.95/day)
    </label>

    <h2>Under 25</h2>
    <label> <input type="radio" name="under" id="notUnder" />No</label>
    <label> <input type="radio" name="under" id="isUnder" />Yes</label>
    <br /><br />
    <button id="estimateButton">Estimate Total Cost</button>

    <pre id="receiptOutput"></pre>

    <script src="script.js"></script>
  </body>
</html>
```

### Breaking Down the HTML:

1. **User Inputs:**
   - The user selects a pickup date using the `<input type="date">` element.
   - The number of rental days is entered using `<input type="number" id="numberDays">`.
2. **Options:**
   - Users can select extra services like Electronic Toll Tag, GPS, and Roadside Assistance using checkboxes (`<input type="checkbox">`).
3. **Under 25 Surcharge:**

   - A radio button allows users to indicate if they are under the age of 25, which affects the cost by applying an extra surcharge.

4. **Button:**

   - The button labeled “Estimate Total Cost” triggers the cost calculation when clicked.

5. **Receipt Display:**
   - The `<pre>` tag (with `id="receiptOutput"`) is used to show the cost breakdown after calculation.

### The JavaScript Part – Calculating the Cost

Now, let's dive into the **JavaScript** that powers the logic behind the form. This is contained in `script.js`, and it handles the rental cost calculations based on the user's input.

```javascript
function getReceipt(days, wantsToll, wantsGPS, wantsRoadSide, isUnderAge) {
  const rentalDay = 29.99; // Daily car rental cost
  const surchargePercent = 0.3; // 30% surcharge for underage
  let carRental = rentalDay * days; // Total rental cost without options
  let surchargeAmount = 0; // Initialize surchargeAmount

  // Apply surcharge if under age
  if (isUnderAge) {
    surchargeAmount = surchargePercent * carRental;
    carRental += surchargeAmount; // Add surcharge to car rental
  }

  let optionsSubTotal = 0; // Initialize options cost subtotal

  // Calculate options subtotal based on selected options
  if (wantsToll) {
    optionsSubTotal += 3.95 * days; // Toll charge per day
  }
  if (wantsGPS) {
    optionsSubTotal += 2.95 * days; // GPS charge per day
  }
  if (wantsRoadSide) {
    optionsSubTotal += 2.95 * days; // Roadside assistance charge per day
  }

  const grandTotal = (carRental + optionsSubTotal + surchargeAmount).toFixed(2); // Final total, formatted to 2 decimals

  return `
    Car Rental: ${carRental.toFixed(2)} 
    Options: ${optionsSubTotal.toFixed(2)} 
    Under 25 surcharge: ${surchargeAmount.toFixed(2)} 
    Total Due: ${grandTotal} 
  `;
}
```

### Explaining the JavaScript:

1. **Input Parameters:**
   The `getReceipt` function accepts several parameters:

   - `days`: The number of days the car is being rented.
   - `wantsToll`, `wantsGPS`, and `wantsRoadSide`: Boolean values that indicate whether the user selected these options.
   - `isUnderAge`: A boolean indicating whether the renter is under 25.

2. **Car Rental Calculation:**

   - The daily rental rate is set at `$29.99`.
   - If the renter is under 25, a 30% surcharge (`surchargePercent`) is added to the total rental cost.
   - The surcharge amount is calculated and added to the `carRental` cost if the renter is under 25.

3. **Options Cost Calculation:**

   - The cost of additional services is calculated based on the number of rental days. For example, the electronic toll tag costs `$3.95/day`, while both GPS and roadside assistance cost `$2.95/day`.

4. **Final Calculation:**
   - The grand total is the sum of the rental cost, any surcharge, and the cost of selected options.
   - The `.toFixed(2)` method ensures that the total is rounded to two decimal places for currency formatting.

### Linking the Form to the Function

Finally, we need to ensure that when the user clicks the “Estimate Total Cost” button, the `getReceipt` function is triggered, and the receipt is displayed. This is done in the event listener:

```javascript
document.addEventListener("DOMContentLoaded", () => {
  const numberDays = document.getElementById("numberDays");
  const toll = document.getElementById("toll");
  const gps = document.getElementById("gps");
  const roadside = document.getElementById("roadside");
  const isUnder = document.getElementById("isUnder");
  const estimateButton = document.getElementById("estimateButton");
  const receiptOutput = document.getElementById("receiptOutput");

  // Event listener for the button click
  estimateButton.addEventListener("click", () => {
    receiptOutput.innerText = getReceipt(
      parseInt(numberDays.value), // Convert to integer
      toll.checked,
      gps.checked,
      roadside.checked,
      isUnder.checked
    );
  });
});
```

### How It Works:

- The **DOMContentLoaded** event ensures that the JavaScript only runs after the page has loaded.
- The event listener on the “Estimate Total Cost” button grabs the values from the form, calls `getReceipt`, and displays the result in the `receiptOutput` area.
- Each option is checked whether it's selected or not, and the appropriate cost is calculated based on the user's choices.

### Conclusion

In this project, we built a simple car rental estimator using HTML and JavaScript. The key parts of the project include:

1. **HTML Form** to collect input from users.
2. **JavaScript Function** to calculate the total rental cost based on user input.
3. **Event Listener** to trigger the calculation and display the result.
