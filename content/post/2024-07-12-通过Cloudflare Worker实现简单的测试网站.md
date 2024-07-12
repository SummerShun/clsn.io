---
title: "通过Cloudflare Worker实现简单的测试网站"
date: 2024-07-12T11:27:07+08:00
draft: true
author: 惨绿少年
type: post
Baidusubmit:
  - 1
categories:
  - 监控方案 
  - Cloudflare

---
## 创建worker
关于创建worker的方法可以参考 [借助Cloudflare Worker获取公网IP](https://clsn.io/post/2024-07-11-%E5%80%9F%E5%8A%A9cloudflare%E8%8E%B7%E5%8F%96%E5%85%AC%E7%BD%91ip)

## worker.js 代码
```js
const html = `
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Speed Test</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #111;
            color: #f0f0f0;
            text-align: center;
            padding: 20px;
        }
        h1 {
            color: #4CAF50;
        }
        .container {
            max-width: 600px;
            margin: 0 auto;
            padding: 20px;
            background-color: #222;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
        }
        button {
            background-color: #4CAF50;
            color: #fff;
            border: none;
            padding: 15px 25px;
            font-size: 16px;
            border-radius: 5px;
            cursor: pointer;
            margin-bottom: 20px;
        }
        button:disabled {
            background-color: #888;
            cursor: not-allowed;
        }
        .status {
            font-size: 18px;
            margin: 10px 0;
        }
        .status span {
            font-weight: bold;
        }
        .progress {
            width: 100%;
            background-color: #333;
            height: 20px;
            border-radius: 5px;
            margin: 10px 0;
        }
        .progress-bar {
            height: 100%;
            background-color: #4CAF50;
            border-radius: 5px;
            transition: width 0.2s ease;
        }
        .spinner {
            border: 4px solid rgba(255, 255, 255, 0.1);
            border-left: 4px solid #4CAF50;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            margin: 0 auto;
            animation: spin 1s linear infinite;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        .unit-selector {
            margin-bottom: 20px;
        }
        .unit-selector label {
            font-size: 16px;
            margin-right: 10px;
        }
        .unit-selector select {
            background-color: #4CAF50;
            color: #fff;
            border: none;
            padding: 10px 15px;
            font-size: 16px;
            border-radius: 5px;
            cursor: pointer;
        }
        .unit-selector select:focus {
            outline: none;
            box-shadow: 0 0 5px rgba(76, 175, 80, 0.5);
        }
        .history-container {
            margin-top: 30px;
            text-align: left;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 10px;
        }
        table, th, td {
            border: 1px solid #333;
        }
        th, td {
            padding: 8px;
            text-align: left;
        }
        th {
            background-color: #333;
        }
        .history-item {
            background-color: #333;
            padding: 10px;
            border-radius: 5px;
            margin-bottom: 10px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Speed Test</h1>
        <div class="unit-selector">
            <label for="unit">Choose Unit:</label>
            <select id="unit">
                <option value="Mbps">Mbps</option>
                <option value="MB/s">MB/s</option>
            </select>
        </div>
        <button id="startTest">Start Test</button>
        <div id="progressContainer">
            <div id="downloadProgress" class="progress">
                <div id="downloadBar" class="progress-bar">Download</div>
            </div>
            <div id="uploadProgress" class="progress">
                <div id="uploadBar" class="progress-bar">Upload</div>
            </div>
        </div>
        <div id="results">
            <p class="status">Ping: <span id="ping">0</span> ms</p>
            <p class="status">Download Speed: <span id="download">0</span> <span id="downloadUnit">Mbps</span></p>
            <p class="status">Upload Speed: <span id="upload">0</span> <span id="uploadUnit">Mbps</span></p>
        </div>
        <div id="spinner" class="spinner" style="display: none;"></div>
        <div class="history-container">
            <h2>History</h2>
            <table id="historyTable">
                <thead>
                    <tr>
                        <th>Time</th>
                        <th>Ping (ms)</th>
                        <th>Download Speed</th>
                        <th>Upload Speed</th>
                    </tr>
                </thead>
                <tbody id="history">
                </tbody>
            </table>
        </div>
    </div>

    <script>
        document.getElementById('startTest').addEventListener('click', async () => {
            const startButton = document.getElementById('startTest');
            startButton.disabled = true;

            const pingElement = document.getElementById('ping');
            const downloadElement = document.getElementById('download');
            const uploadElement = document.getElementById('upload');
            const downloadBar = document.getElementById('downloadBar');
            const uploadBar = document.getElementById('uploadBar');
            const spinner = document.getElementById('spinner');
            const unit = document.getElementById('unit').value;
            const downloadUnitElement = document.getElementById('downloadUnit');
            const uploadUnitElement = document.getElementById('uploadUnit');
            const historyContainer = document.getElementById('history');

            downloadUnitElement.textContent = unit;
            uploadUnitElement.textContent = unit;

            // Show spinner
            spinner.style.display = 'block';

            // Reset progress bars
            downloadBar.style.width = '0%';
            uploadBar.style.width = '0%';

            // Measure ping
            const pingStart = performance.now();
            await fetch('/ping');
            const pingEnd = performance.now();
            const ping = (pingEnd - pingStart).toFixed(2);
            pingElement.textContent = ping;

            // Measure download speed
            const downloadStart = performance.now();
            const downloadResponse = await fetch('/download');
            const downloadBlob = await downloadResponse.blob();
            const downloadEnd = performance.now();
            const downloadDuration = (downloadEnd - downloadStart) / 1000;
            const downloadSize = downloadBlob.size * 8 / (1024 * 1024); // Convert bytes to megabits
            let downloadSpeed;
            if (unit === 'Mbps') {
                downloadSpeed = (downloadSize / downloadDuration).toFixed(2);
            } else { // MB/s
                downloadSpeed = (downloadSize / 8 / downloadDuration).toFixed(2);
            }
            downloadElement.textContent = downloadSpeed;
            downloadBar.style.width = '100%';

            // Measure upload speed
            const uploadSize = 10 * 1024 * 1024; // 10MB
            const uploadData = new Uint8Array(uploadSize);
            const uploadStart = performance.now();
            await fetch('/upload', { method: 'POST', body: uploadData });
            const uploadEnd = performance.now();
            const uploadDuration = (uploadEnd - uploadStart) / 1000;
            let uploadSpeed;
            if (unit === 'Mbps') {
                uploadSpeed = (uploadSize * 8 / (1024 * 1024) / uploadDuration).toFixed(2);
            } else { // MB/s
                uploadSpeed = (uploadSize / (1024 * 1024) / uploadDuration).toFixed(2);
            }
            uploadElement.textContent = uploadSpeed;
            uploadBar.style.width = '100%';

            // Hide spinner
            spinner.style.display = 'none';

            // Store the result in localStorage
            let history = JSON.parse(localStorage.getItem('speedTestHistory')) || [];
            const timestamp = new Date().toLocaleString();
            const result = {
                timestamp: timestamp,
                ping: ping,
                download: downloadSpeed,
                upload: uploadSpeed,
                unit: unit
            };
            history.unshift(result);
            history = history.slice(0, 5); // Keep only the latest 5 entries
            localStorage.setItem('speedTestHistory', JSON.stringify(history));

            // Update the history display
            displayHistory();

            // Re-enable the start button
            startButton.disabled = false;
        });

        function displayHistory() {
            const historyTable = document.getElementById('history');
            historyTable.innerHTML = '';
            const history = JSON.parse(localStorage.getItem('speedTestHistory')) || [];
            history.forEach(item => {
                const row = document.createElement('tr');
                row.innerHTML = \`
                    <td>\${item.timestamp}</td>
                    <td>\${item.ping}</td>
                    <td>\${item.download} \${item.unit}</td>
                    <td>\${item.upload} \${item.unit}</td>
                \`;
                historyTable.appendChild(row);
            });
        }

        // Display history on page load
        displayHistory();
    </script>
</body>
</html>
`;

addEventListener('fetch', event => {
    event.respondWith(handleRequest(event.request));
});

async function handleRequest(request) {
    const url = new URL(request.url);

    if (url.pathname === '/') {
        return new Response(html, {
            headers: { 'Content-Type': 'text/html' }
        });
    }

    if (url.pathname === '/ping') {
        return new Response('pong', { status: 200 });
    }

    if (url.pathname === '/download') {
        let size = 10 * 1024 * 1024; // Default size: 10MB in bytes

        // Check if size parameter is provided in MB
        const sizeParam = url.searchParams.get('size');
        if (sizeParam) {
            const sizeInMB = parseFloat(sizeParam);
            if (isNaN(sizeInMB) || sizeInMB <= 0) {
                return new Response('Invalid size parameter', { status: 400 });
            }
            // Convert MB to bytes
            size = sizeInMB * 1024 * 1024;
            if (size > 128 * 1024 * 1024) { // Limit to 128M maximum
                return new Response('Size exceeds maximum allowed (128M)', { status: 400 });
            }
        }

        const data = new Uint8Array(size);
        return new Response(data, { status: 200 });
    }

    if (url.pathname === '/upload') {
        const body = await request.arrayBuffer();
        return new Response('upload complete', { status: 200 });
    }

    return new Response('Not found', { status: 404 });
}
```

## 效果
![图 0](/png/2024-07-12-%E9%80%9A%E8%BF%87Cloudflare%20Worker%E5%AE%9E%E7%8E%B0%E7%AE%80%E5%8D%95%E7%9A%84%E6%B5%8B%E8%AF%95%E7%BD%91%E7%AB%99/IMG_20240712-113310060.gif)  
