<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>交易計算器</title>
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
</head>
<body class="bg-gray-100 p-6">
    <div class="max-w-xl mx-auto bg-white p-6 rounded-lg shadow-lg">
        <h2 class="text-2xl font-bold mb-4">BDOS風險控制交易計算器</h2>
        <p class="text-sm text-gray-600 mb-4">請填入「預期損失金額」或「本金 × 預期虧損百分比」（二擇一）</p>

        <label class="block mb-3">預期損失金額（USDT，可留空）：
            <input type="number" id="risk_amount" class="w-full p-2 border rounded">
        </label>

        <div class="grid grid-cols-2 gap-4">
            <label class="block">本金（USDT，可留空）：
                <input type="number" id="capital" class="w-full p-2 border rounded">
            </label>
            <label class="block">預期虧損百分比（%，可留空）：
                <input type="number" id="risk_percent" class="w-full p-2 border rounded">
            </label>
        </div>

        <div class="grid grid-cols-2 gap-4 mt-3">
            <label class="block">開倉價格：
                <input type="number" id="entry_price" class="w-full p-2 border rounded">
            </label>
            <label class="block">止損價格：
                <input type="number" id="stop_loss_price" class="w-full p-2 border rounded">
            </label>
        </div>

        <button onclick="calculate()" class="mt-4 w-full bg-blue-500 text-white py-2 rounded hover:bg-blue-600">
            計算
        </button>

        <h3 class="text-lg font-semibold mt-4">計算結果：</h3>
        <p id="result" class="text-gray-700 mt-2"></p>
    </div>

    <script>
        function calculate() {
            let riskAmount = parseFloat(document.getElementById("risk_amount").value);
            let capital = parseFloat(document.getElementById("capital").value);
            let riskPercent = parseFloat(document.getElementById("risk_percent").value);
            let entryPrice = parseFloat(document.getElementById("entry_price").value);
            let stopLossPrice = parseFloat(document.getElementById("stop_loss_price").value);

            if (isNaN(entryPrice) || isNaN(stopLossPrice) || entryPrice <= 0) {
                document.getElementById("result").innerText = "請輸入有效的開倉價格與止損價格！";
                return;
            }

            let stopLossDiff = Math.abs(stopLossPrice - entryPrice);
            let stopLossRatio = stopLossDiff / entryPrice;

            if (stopLossRatio <= 0) {
                document.getElementById("result").innerText = "止損比例無效，請檢查輸入數據！";
                return;
            }

            if (isNaN(riskAmount) || riskAmount <= 0) {
                if (!isNaN(capital) && !isNaN(riskPercent) && riskPercent > 0) {
                    riskAmount = capital * (riskPercent / 100);
                } else {
                    document.getElementById("result").innerText = "請輸入預期損失金額 或 本金+預期虧損百分比！";
                    return;
                }
            }

            let nominalValue = riskAmount / stopLossRatio;
            document.getElementById("result").innerText = "名義價值: " + nominalValue.toFixed(2) + " USD";
        }
    </script>

</body>
</html>
