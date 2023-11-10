<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Menu Calculator</title>
    <style>
        body {
            text-align: center;
	    padding: 20px;
	    background-color: #963264;
	    font-size: 16px; /* Default font size */
        }

        .category {
            float: left;
            margin-right: 20px;
			margin-left: 60px;
        }

        .item {
            margin-bottom: 10px;
			font-size: 24px;
        }

        h2, #total, #subtotal, #commission {
            text-align: center;
        }

        .bottom-row {
            text-align: center;
            margin-top: 20px;
        }
		
		
    </style>
</head>
<a href="Old_Menu.html">OLD MENU</a>

<body>
	
    <form id="menuForm">
        <div class="category">
            <h2>Specials</h2>
            <div class="item">
                <label><input type="checkbox" class="appetizer" value="1500"> First Special</label>
                <input type="number" class="quantity" value="1" min="1">
            </div>
            <div class="item">
                <label><input type="checkbox" class="appetizer" value="1500"> Second Special</label>
                <input type="number" class="quantity" value="1" min="1">
            </div>
            <div class="item">
                <label><input type="checkbox" class="appetizer" value="250"> Third Special</label>
                <input type="number" class="quantity" value="1" min="1">
            </div>
        </div>

        <div class="category">
            <h2>Combo's</h2>
            <div class="item">
                <label><input type="checkbox" class="main-dish" value="300"> 1st Choice</label>
                <input type="number" class="quantity" value="1" min="1">
            </div>
            <div class="item">
                <label><input type="checkbox" class="main-dish" value="300"> 2nd Choice</label>
                <input type="number" class="quantity" value="1" min="1">
            </div>
			<div class="item">
                <label><input type="checkbox" class="main-dish" value="300"> 3rd Choice</label>
                <input type="number" class="quantity" value="1" min="1">
            </div>
            <div class="item">
                <label><input type="checkbox" class="main-dish" value="300"> 4th Choice</label>
                <input type="number" class="quantity" value="1" min="1">
            </div>
        </div>

        

        <div class="category">
            <h2>Entree</h2>
            <div class="item">
                <label><input type="checkbox" class="drink" value="150"> Salad</label>
                <input type="number" class="quantity" value="1" min="1">
            </div>
            <div class="item">
                <label><input type="checkbox" class="drink" value="200"> Fruit Explosion</label>
                <input type="number" class="quantity" value="1" min="1">
            </div>
			<div class="item">
                <label><input type="checkbox" class="drink" value="150"> Sandwhiches</label>
                <input type="number" class="quantity" value="1" min="1">
            </div>
            <div class="item">
                <label><input type="checkbox" class="drink" value="175"> Choccy Pancakes</label>
                <input type="number" class="quantity" value="1" min="1">
            </div>
        </div>  



           
			
        </div>
		
		
        
		
        </div>
		
		

        <div style="clear:both;"></div> <!-- Clear float -->
		
		  <div style="margin-bottom: 30px;"></div>
		
		<!-- Employee Name Input Field -->
            <div class="employee-name-input">
				<label for="employeeName">Employee Name:</label>
				<input type="text" id="employeeName" name="employeeName" required>
			</div>
			
			  <div style="margin-bottom: 10px;"></div>

        <div class="bottom-row">
            
            <h2>Total: $<span id="subtotal">0.00</span></h2>
            <h2>Commission (10%): $<span id="commission">0.00</span></h2>
            <h2>Discount Amount: $<span id="discountAmount">0.00</span></h2>
			
			<!-- Discount Options -->
            <input type="radio" name="discount" value="25"> 25% Off (EMS,PD,LEO MECH
            <input type="radio" name="discount" value="30"> 30% Off (Freshness under 100%)
            <input type="radio" name="discount" value="50"> 50% Off (Employee Discount)

            
			
			  <div style="margin-bottom: 20px;"></div>
			
			  

            <button type="button" id="calculateBtn">Calculate Total</button>
			<input type="submit" value="Submit Order">
			
        </div>
    </form>


<script>
           // JavaScript code
    const checkboxes = document.querySelectorAll('input[type="checkbox"]');
    const quantities = document.querySelectorAll('.quantity');
    const calculateBtn = document.getElementById('calculateBtn');
    const subtotalDisplay = document.getElementById('subtotal');
    const commissionDisplay = document.getElementById('commission');
    const discountAmountDisplay = document.getElementById('discountAmount');
    const totalDisplay = document.getElementById('subtotal'); // Changed to subtotal, assuming it's the total before applying discount and commission
    const menuForm = document.getElementById('menuForm');
    const employeeNameInput = document.getElementById('employeeName');
    const discountOptions = document.getElementsByName('discount');

    calculateBtn.addEventListener('click', calculateTotal);

    function calculateTotal() {
    let subtotal = 0;
    let itemsOrdered = [];

    checkboxes.forEach((checkbox, index) => {
        if (checkbox.checked) {
            const itemName = checkbox.parentElement.innerText.trim();
            const itemPrice = parseFloat(checkbox.value);
            const quantity = parseInt(quantities[index].value);
            subtotal += itemPrice * quantity;
            
            // Add item details to the itemsOrdered array
            itemsOrdered.push({ name: itemName, quantity: quantity, price: itemPrice });
        }
    });

        const commission = subtotal * 0.1;
        let selectedDiscount = 0;
        for (let i = 0; i < discountOptions.length; i++) {
            if (discountOptions[i].checked) {
                selectedDiscount = parseFloat(discountOptions[i].value);
                break;
            }
        }

        const discountPercentage = selectedDiscount / 100;
        const discountAmount = subtotal * discountPercentage;

        const total = subtotal - discountAmount;

        subtotalDisplay.textContent = subtotal.toFixed(2);
        commissionDisplay.textContent = commission.toFixed(2);
        discountAmountDisplay.textContent = discountAmount.toFixed(2);
        totalDisplay.textContent = total.toFixed(2);

        return { itemsOrdered, subtotal, selectedDiscount, commission, discountAmount, total };
}

    function resetMenu() {
    checkboxes.forEach(checkbox => {
        checkbox.checked = false;
    });

    quantities.forEach(quantityInput => {
        quantityInput.value = 1;
    });

    clearDiscountOptions(); // Call the function to clear discount options
	
	function clearDiscountOptions() {
    discountOptions.forEach(option => {
        option.checked = false;
    });
}
}

    menuForm.addEventListener('submit', function(event) {
    event.preventDefault();

    const employeeName = employeeNameInput.value;
    const { itemsOrdered, subtotal, selectedDiscount, commission, discountAmount, total } = calculateTotal();

    if (total <= 0) {
        alert('Error: Total amount must be greater than 0. Please select items and calculate the total again.');
        return;
    }

        const data = {
                content: 'New Order!',
                embeds: [{
                    title: 'Order Details',
                    color: 0xFF5733,
                    fields: [
                        {
                            name: 'Name',
                            value: employeeName,
                            inline: true
                        },
                        {
                            name: 'Total',
                            value: '$' + total.toFixed(2),
                            inline: true
                        },
                        {
                            name: 'Discount Total',
                            value: '$' + discountAmount.toFixed(2),
                            inline: false
                        },
                        {
                            name: 'Discount Applied',
                            value: selectedDiscount + '%',
                            inline: true
                        },
                        {
                            name: 'Commission (10%)',
                            value: '$' + commission.toFixed(2),
                            inline: true
                        },
                        {
                            name: 'Ordered Items',
                            value: itemsOrdered.map(item => `${item.quantity}x ${item.name} - $${item.price.toFixed(2)}`).join('\n'),
                            inline: false
                        }
                    ]
                }]
            };

        // Discord Webhook URL (replace with your actual webhook URL)
        const webhookUrl = 'https://discord.com/api/webhooks/1157451563212750959/FqNuldbbd1b4cZOxmo3xsbngVnMEWZfOSyXxwtwMuv7iTmeLhgDbL6maZiZJnfgYgVwy';

        fetch(webhookUrl, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify(data),
        })
            .then(response => {
                if (!response.ok) {
                    throw new Error('Network response was not ok');
                }
                // Show an alert when the order is successfully sent
                alert('Order submitted successfully!');
            })
            .catch(error => {
                console.error('There has been a problem with your fetch operation:', error);
            });

        // Reset menu items and quantities after submitting the order
        resetMenu();
    });
    </script>
</body>

</html>
