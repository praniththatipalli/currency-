<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Currency Exchange</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <style>
        body {
            font-family: Arial, sans-serif;
            background: linear-gradient(135deg, #1e3c72, #2a5298);
            color: #333;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }

        .container {
            background: #fff;
            padding: 25px;
            border-radius: 10px;
            width: 320px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.2);
        }

        h2 {
            text-align: center;
            margin-bottom: 20px;
            color: #1e3c72;
        }

        .input-group {
            margin-bottom: 15px;
        }

        label {
            display: block;
            font-size: 14px;
            margin-bottom: 5px;
        }

        input, select, button {
            width: 100%;
            padding: 10px;
            font-size: 14px;
            border-radius: 5px;
            border: 1px solid #ccc;
        }

        button {
            background: #1e3c72;
            color: white;
            border: none;
            cursor: pointer;
            margin-top: 10px;
        }

        button:hover {
            background: #16325c;
        }

        .result {
            margin-top: 15px;
            text-align: center;
            font-weight: bold;
            font-size: 16px;
        }

        footer {
            text-align: center;
            margin-top: 15px;
            font-size: 12px;
            color: #777;
        }
    </style>
</head>
<body>

<div class="container">
    <h2>Currency Exchange</h2>

    <div class="input-group">
        <label>Amount</label>
        <input type="number" id="amount" value="1" min="0">
    </div>

    <div class="input-group">
        <label>From</label>
        <select id="fromCurrency"></select>
    </div>

    <div class="input-group">
        <label>To</label>
        <select id="toCurrency"></select>
    </div>

    <button onclick="convertCurrency()">Convert</button>

    <div class="result" id="result"></div>

    <footer>
        Live exchange rates
    </footer>
</div>

<script>
    const fromCurrency = document.getElementById("fromCurrency");
    const toCurrency = document.getElementById("toCurrency");
    const result = document.getElementById("result");

    const API_URL = "https://api.exchangerate.host/latest";

    async function loadCurrencies() {
        const res = await fetch(API_URL);
        const data = await res.json();
        const currencies = Object.keys(data.rates);

        currencies.forEach(currency => {
            const option1 = document.createElement("option");
            const option2 = document.createElement("option");
            option1.value = option2.value = currency;
            option1.textContent = option2.textContent = currency;
            fromCurrency.appendChild(option1);
            toCurrency.appendChild(option2);
        });

        fromCurrency.value = "USD";
        toCurrency.value = "EUR";
    }

    async function convertCurrency() {
        const amount = document.getElementById("amount").value;
        const from = fromCurrency.value;
        const to = toCurrency.value;

        if (amount <= 0) {
            result.textContent = "Enter a valid amount.";
            return;
        }

        const res = await fetch(`${API_URL}?base=${from}&symbols=${to}`);
        const data = await res.json();
        const rate = data.rates[to];

        const converted = (amount * rate).toFixed(2);
        result.textContent = `${amount} ${from} = ${converted} ${to}`;
    }

    loadCurrencies();
</script>

</body>
</html>
