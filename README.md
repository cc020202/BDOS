<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>交易計算器</title>
    <style>
        body { 
            font-family: Arial, sans-serif; 
            margin: 12px; 
            padding: 0; 
            background-color: #f8f9fa;
        }

        .container { 
            max-width: 90%; 
            width: 500px; 
            margin: auto; 
            background: white; 
            padding: 20px; 
            border-radius: 8px; 
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1); 
        }

        /* 縮小前三行介紹文字 */
        .container p:first-of-type,
        .container p:nth-of-type(2),
        .container p:nth-of-type(3) {
            font-size: 14px;  /* 可改成 12px 如果想更小 */
            color: #666;  /* 讓顏色稍微變淡 */
        }

        /* 調整 info 文字大小與顏色 */
        .info { 
            font-size: 12px;  /* 縮小 info 文字 */
            color: #999;  /* 讓顏色更淺 */
            margin-top: 3px; 
        }

        label { 
            display: block; 
            margin-top: 12px; 
            font-weight: bold; 
        }

        input { 
            width: 100%; 
            padding: 10px; 
            margin-top: 5px; 
            border: 1px solid #ccc; 
            border-radius: 5px; 
            font-size: 16px; 
        }

        button { 
            margin-top: 15px; 
            padding: 12px; 
            width: 100%; 
            background: #007bff; 
            color: white; 
            border: none; 
            font-size: 18px; 
            cursor: pointer; 
            border-radius: 5px; 
            transition: background 0.3s;
        }

        button:hover { 
            background: #0056b3; 
        }

        @media (max-width: 768px) {
            .container { 
                max-width: 95%; 
                padding: 15px; 
            }

            input, button { 
                font-size: 14px; 
                padding: 8px; 
            }
        }
        #result { 
            font-size: 24px;  /* 你可以改成 22px、24px 來更大 */
            font-weight: bold; /* 讓文字加粗 */
            color: #333; /* 讓顏色更明顯 */
            margin-top: 10px;
        }

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
        
        <label>總本金（USDT）：</label>
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
            document.getElementById("result").innerText = "名義價值: " + nominalValue.toFixed(2) + "USDT";
        }
    </script>
</body>
</html>
