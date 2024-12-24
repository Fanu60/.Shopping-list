
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Shopping List App</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h1>Shopping List</h1>
        <div class="input-container">
            <input type="text" id="item-input" placeholder="Enter item">
            <input type="number" id="price-input" placeholder="Enter price (optional)" min="0" step="0.01">
            <button onclick="addItem()">Add</button>
        </div>
        <ul id="shopping-list"></ul>
        <button class="clear-btn" onclick="clearList()">Clear List</button>
        <button class="finish-btn" onclick="finishShopping()">Finish</button>
        <div id="total-price"></div>
    </div>

    <script>
        let totalPrice = 0;

        function addItem() {
            const itemInput = document.getElementById('item-input');
            const priceInput = document.getElementById('price-input');
            const itemText = itemInput.value.trim();
            const priceText = priceInput.value.trim() !== '' ? parseFloat(priceInput.value).toFixed(2) : null;

            if (itemText === '') {
                alert('Please enter a valid item.');
                return;
            }

            const li = document.createElement('li');

            const itemSpan = document.createElement('span');
            itemSpan.textContent = priceText ? `${itemText} - $${priceText}` : itemText;

            itemSpan.onclick = function () {
                itemSpan.classList.toggle('crossed');
            };

            li.appendChild(itemSpan);

            const deleteButton = document.createElement('button');
            deleteButton.textContent = 'Delete';
            deleteButton.onclick = function () {
                if (priceText) {
                    totalPrice -= parseFloat(priceText);
                    updateTotalPriceDisplay();
                }
                li.remove();
            };

            li.appendChild(deleteButton);

            document.getElementById('shopping-list').appendChild(li);

            if (priceText) {
                totalPrice += parseFloat(priceText);
                updateTotalPriceDisplay();
            }

            itemInput.value = '';
            priceInput.value = '';
        }

        function clearList() {
            const list = document.getElementById('shopping-list');
            list.innerHTML = '';
            totalPrice = 0;
            updateTotalPriceDisplay();
        }

        function finishShopping() {
            const listItems = document.querySelectorAll('#shopping-list li span');
            const summary = Array.from(listItems).map(item => item.textContent).join('<br>');

            localStorage.setItem('shoppingSummary', summary);
            localStorage.setItem('totalPrice', totalPrice.toFixed(2));

            window.location.href = 'summary.html';
        }

        function updateTotalPriceDisplay() {
            const totalPriceElement = document.getElementById('total-price');
            totalPriceElement.textContent = `Total Price: $${totalPrice.toFixed(2)}`;
        }
    </script>
</body>
</html>


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Shopping Summary</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="summary">
        <h1>Shopping Summary</h1>
        <p id="summary-content"></p>
        <p><strong>Total Price:</strong> $<span id="total-price"></span></p>
    </div>
    <script>
        document.getElementById('summary-content').innerHTML = localStorage.getItem('shoppingSummary');
        document.getElementById('total-price').textContent = localStorage.getItem('totalPrice');
    </script>
</body>
</html>

/* Shared CSS file: styles.css */
body {
    font-family: Arial, sans-serif;
    background-color: #f4f4f4;
    margin: 0;
    padding: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}
.container, .summary {
    background: #fff;
    padding: 20px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    border-radius: 8px;
    width: 90%;
    max-width: 400px;
}
h1 {
    text-align: center;
    color: #333;
}
.input-container {
    display: flex;
    gap: 10px;
    margin-bottom: 20px;
    flex-wrap: wrap;
}
.input-container input {
    flex: 1 1 calc(100% - 80px);
    padding: 8px;
    font-size: 16px;
    border: 1px solid #ccc;
    border-radius: 4px;
}
.input-container button {
    flex: 1 1 60px;
    padding: 8px;
    font-size: 16px;
    color: #fff;
    background-color: #28a745;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}
.input-container button:hover {
    background-color: #218838;
}
ul {
    list-style-type: none;
    padding: 0;
}
li {
    display: flex;
    justify-content: space-between;
    background: #f9f9f9;
    padding: 8px;
    margin-bottom: 10px;
    border-radius: 4px;
    border: 1px solid #ddd;
    cursor: pointer;
}
li span {
    flex: 1;
}
li span.crossed {
    text-decoration: line-through;
    color: #888;
}
li button {
    background: #dc3545;
    color: #fff;
    border: none;
    border-radius: 4px;
    padding: 4px 8px;
    cursor: pointer;
}
li button:hover {
    background: #c82333;
}
.clear-btn, .finish-btn {
    width: 100%;
    padding: 10px;
    font-size: 16px;
    margin-top: 10px;
    color: #fff;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}
.clear-btn {
    background-color: #007bff;
}
.clear-btn:hover {
    background-color: #0056b3;
}
.finish-btn {
    background-color: #28a745;
}
.finish-btn:hover {
    background-color: #218838;
}
#total-price {
    margin-top: 20px;
    font-size: 18px;
    text-align: center;
    color: #333;
}
@media (max-width: 600px) {
    .container, .summary {
        padding: 15px;
    }
    .input-container {
        flex-wrap: wrap;
    }
    .input-container input {
        flex: 1 1 100%;
    }
    .input-container button {
        flex: 1 1 100%;
        margin-top: 10px;
    }
    li {
        flex-direction: column;
        align-items: start;
    }
    li button {
        margin-top: 5px;
    }
}
