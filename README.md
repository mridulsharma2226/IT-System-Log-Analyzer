<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Log Analyzer</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        table { width: 100%; border-collapse: collapse; margin-top: 20px; }
        th, td { border: 1px solid #ddd; padding: 8px; text-align: left; }
        th { background-color: #f4f4f4; }
    </style>
</head>
<body>
    <h2>IT System Log Analyzer</h2>
    
    <input type="file" id="logFile" accept=".txt">
    <button onclick="uploadLog()">Upload Log</button>

    <h3>Log Entries</h3>
    <input type="text" id="search" placeholder="Search logs..." onkeyup="filterLogs()">
    <table id="logTable">
        <thead>
            <tr><th>Timestamp</th><th>Level</th><th>Message</th></tr>
        </thead>
        <tbody></tbody>
    </table>

    <script>
        function uploadLog() {
            const fileInput = document.getElementById('logFile');
            if (!fileInput.files.length) return alert("Please select a log file!");
            
            const formData = new FormData();
            formData.append('logFile', fileInput.files[0]);

            fetch('/upload-log', { method: 'POST', body: formData })
                .then(res => res.json())
                .then(data => loadLogs(data.logs))
                .catch(err => console.error('Error:', err));
        }

        function loadLogs(logs) {
            const tableBody = document.querySelector('#logTable tbody');
            tableBody.innerHTML = '';
            logs.forEach(log => {
                const row = `<tr>
                    <td>${log.timestamp}</td>
                    <td>${log.level}</td>
                    <td>${log.message}</td>
                </tr>`;
                tableBody.innerHTML += row;
            });
        }

        function filterLogs() {
            let filter = document.getElementById('search').value.toLowerCase();
            let rows = document.querySelectorAll("#logTable tbody tr");

            rows.forEach(row => {
                let text = row.innerText.toLowerCase();
                row.style.display = text.includes(filter) ? "" : "none";
            });
        }
    </script>
</body>
</html>
