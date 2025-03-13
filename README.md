
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>交易計算器</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 6px; }
        .container { max-width: 500px; margin: auto; }
        label { display: block; margin-top: 12px; }
        input { width: 100%; padding: 8px; margin-top: 5px; }
        button { margin-top: 15px; padding: 10px; width: 100%; background: blue; color: white; border: none; }
        p { line-height: 1.4; margin: 5px 0; } /* 調整行高與間距 */
        .info { font-size: 14px; color: gray; margin-top: 3px; }
    </style>
</head>
<body>
    <div class="container">
        <h2>風險控制倉位計算器</h2>
        <p>🔹請選填「預期損失金額」或「總本金×總本金虧損百分比」（二擇一）</p>
        <p>🔹這是一個倉位計算器可幫助使用者根據可承受的風險，計算適當的倉位大小</p>
        <p>🔹適用於新手與有經驗的交易者，確保每筆交易風險可控</p>
       
        <label>預期損失金額（USDT）：</label>
        <input type="number" id="risk_amount">
        <p class="info">這筆交易你最多願意承受的虧損金額。</p>
        
        <label>總資金（USDT）：</label>
        <input type="number" id="capital">
        <p class="info">你在市場中的『總投資金』（非單筆交易的本金），此欄需搭配「總本金虧損百分比」。</p>
        
        <label>總本金虧損百分比（%）：</label>
        <input type="number" id="risk_percent" placeholder="1" value="1">
        <p class="info">單筆交易最多虧損總資金的百分比，未填寫則預設 1%。</p>
        
        <label>開倉價格：</label>
        <input type="number" id="entry_price">
	<p class="info">你計畫買入（做多）或賣出（做空）的價格。</p>        

        <label>止損價格：</label>
        <input type="number" id="stop_loss_price">
        <p class="info">交易價格不利時，此單要出場的價格。</p>
        
        <button onclick="calculate()">計算</button>
        <h3>計算結果：</h3>
        <p id="result"></p>
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
