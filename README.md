<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Excel Data Transformation</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.17.4/xlsx.full.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        /* General Styles */
        body {
            font-family: 'Arial', sans-serif;
            background-color: #f4f4f9;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            flex-direction: column;
            padding: 20px 0;
        }
        h1 {
            color: #333;
            margin-bottom: 20px;
            font-size: 2rem;
        }
        /* Tabs Styling */
        .tabs {
            display: flex;
            justify-content: center;
            margin-bottom: 20px;
        }
        .tab {
            padding: 10px 20px;
            cursor: pointer;
            background-color: #ddd;
            border-radius: 5px 5px 0 0;
            margin-right: 5px;
            transition: background-color 0.3s ease;
        }
        .tab.active {
            background-color: #007bff;
            color: #fff;
        }
        /* Tab Content Styling */
        .tab-content {
            display: none;
            padding: 20px;
            background-color: #fff;
            border-radius: 0 0 5px 5px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
            min-height: 400px;
        }
        .tab-content.active {
            display: block;
        }
        /* File Upload Container */
        .upload-container {
            background: #fff;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
            text-align: center;
            width: 90%;
            max-width: 500px;
            margin-bottom: 20px;
        }
        /* File Input Styling */
        .file-input {
            margin: 20px 0;
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
        }
        .file-input label {
            background: #007bff;
            color: #fff;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
            transition: background 0.3s ease;
        }
        .file-input label:hover {
            background: #0056b3;
        }
        .file-input input[type="file"] {
            display: none;
        }
        /* Input Field Styling */
        .input-field {
            margin: 10px 0;
        }
        .input-field label {
            margin-right: 10px;
        }
        .input-field input {
            padding: 5px;
            border-radius: 5px;
            border: 1px solid #ccc;
            width: 100px;
        }
        /* Double Range Slider Styling */
        .range-slider {
            margin: 20px 0;
            width: 100%;
            max-width: 800px;
        }
        .slider-container {
            position: relative;
            height: 20px;
            margin: 20px 0;
            width: 100%;
        }
        .slider-container input[type="range"] {
            position: absolute;
            width: 100%;
            pointer-events: none;
            -webkit-appearance: none;
            background: transparent;
            margin: 0;
        }
        .slider-container input[type="range"]::-webkit-slider-runnable-track {
            height: 4px;
            background: #ddd;
            border-radius: 2px;
        }
        .slider-container input[type="range"]::-webkit-slider-thumb {
            -webkit-appearance: none;
            height: 16px;
            width: 16px;
            background: #007bff;
            border-radius: 50%;
            pointer-events: auto;
            cursor: pointer;
            margin-top: -6px;
            position: relative;
            z-index: 2;
        }
        .slider-container input[type="range"]::-moz-range-thumb {
            height: 16px;
            width: 16px;
            background: #007bff;
            border-radius: 50%;
            pointer-events: auto;
            cursor: pointer;
            position: relative;
            z-index: 2;
        }
        .slider-container input[type="range"]::-moz-range-track {
            height: 4px;
            background: #ddd;
            border-radius: 2px;
        }
        /* Ensure the second slider's thumb is above the first */
        .slider-container input[type="range"]:last-of-type::-webkit-slider-thumb {
            z-index: 3;
        }
        .slider-container input[type="range"]:last-of-type::-moz-range-thumb {
            z-index: 3;
        }
        .slider-values {
            display: flex;
            justify-content: space-between;
            margin-top: 5px;
        }
        /* Button Styling */
        .download-button {
            background: #007bff;
            color: #fff;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1rem;
            transition: background 0.3s ease;
            margin: 5px;
        }
        .download-button:hover {
            background: #0056b3;
        }
        /* Output Styling */
        #output {
            margin-top: 20px;
            background: #fff;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
            width: 90%;
            max-width: 800px;
            white-space: pre-wrap;
            font-family: 'Courier New', Courier, monospace;
            color: #333;
            max-height: 300px;
            overflow-y: auto;
        }
        /* Main Container */
        .main-container {
            width: 90%;
            max-width: 1200px;
            margin-top: 20px;
        }
        /* Total Frequency Display */
        #total-frequency-summary {
            margin-bottom: 10px;
            font-weight: bold;
            text-align: center;
            padding: 10px;
            background: #f0f0f0;
            border-radius: 5px;
        }
        /* Graph Container with Resize */
        .graph-container {
            position: relative;
            width: 100%;
            background: #fff;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
            margin-bottom: 20px;
            resize: both;
            overflow: hidden;
            min-height: 400px;
            min-width: 600px;
        }
        .resize-handle {
            position: absolute;
            bottom: 5px;
            right: 5px;
            width: 20px;
            height: 20px;
            background: #007bff;
            cursor: nwse-resize;
            z-index: 10;
            border-radius: 2px;
        }
        /* Checkbox List */
        #checkboxList {
            width: 100%;
            background: #fff;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
        }
        #checkboxList h3 {
            margin-top: 0;
            margin-bottom: 15px;
            font-size: 1.2rem;
            color: #333;
            border-bottom: 1px solid #eee;
            padding-bottom: 10px;
        }
        .keyword-item {
            display: flex;
            align-items: center;
            margin-bottom: 8px;
            padding: 5px;
            border-radius: 4px;
            transition: background-color 0.2s;
        }
        .keyword-item:hover {
            background-color: #f5f5f5;
        }
        .keyword-item input[type="checkbox"] {
            margin-right: 10px;
            cursor: pointer;
        }
        .keyword-item label {
            cursor: pointer;
            font-size: 0.95rem;
            color: #555;
            flex-grow: 1;
        }
        /* Responsive Design */
        @media (max-width: 768px) {
            .graph-container {
                resize: none;
                min-width: 100%;
            }
            .resize-handle {
                display: none;
            }
        }
        @media (max-width: 480px) {
            h1 {
                font-size: 1.5rem;
            }
            .upload-container {
                padding: 20px;
            }
            .download-button {
                width: 100%;
            }
        }
    </style>
</head>
<body>
    <h1>Excel Data Transformation</h1>
    <div class="tabs">
        <div class="tab active" data-tab="tab1">Tab 1</div>
        <div class="tab" data-tab="tab2">Tab 2</div>
        <div class="tab" data-tab="tab3">Tab 3</div>
    </div>
    <div id="tab1" class="tab-content active">
        <!-- Tab 1 Content -->
    <div class="upload-container">
        <div class="file-input">
            <label for="fileInput">Choose File</label>
            <input type="file" id="fileInput" accept=".xlsx, .csv" />
        </div>
        <div class="input-field">
            <label for="topN">Show Top:</label>
            <input type="number" id="topN" min="1" value="10" />
        </div>
        <button class="download-button" onclick="downloadCSV()" disabled id="downloadButton">Download CSV</button>
    </div>
    <div id="total-frequency-summary"></div>
    <pre id="output"></pre>
    <div class="main-container">
        <!-- Year range slider above the graph -->
        <div class="range-slider">
            <label for="yearRange">Year Range:</label>
            <div class="slider-container">
                <input type="range" id="startYear" min="2000" max="2099" value="2000" />
                <input type="range" id="endYear" min="2000" max="2099" value="2099" />
            </div>
            <div class="slider-values">
                <span id="startYearValue">2000</span>
                <span id="endYearValue">2099</span>
            </div>
        </div>
       <!-- Graph with resize handle -->
    <div class="graph-container">
        <canvas id="keywordChart"></canvas>
        <div class="resize-handle"></div>
    </div>
    
    <!-- Checkbox list below the graph -->
    <div id="checkboxListContainer" style="max-height: 200px; overflow-y: auto;">
    <div id="checkboxList">
        <h3>Select Keywords (Frequency)</h3>
        <!-- Checkboxes will be dynamically added here -->
    </div>
    </div>
    </div>
    <script>
        let transformedData = [];
        let keywordChart = null;
        let minYear = 2000;
        let maxYear = 2099;
        let currentTopKeywords = [];
        // DOM Elements
        const startYearSlider = document.getElementById('startYear');
        const endYearSlider = document.getElementById('endYear');
        const startYearValue = document.getElementById('startYearValue');
        const endYearValue = document.getElementById('endYearValue');
        const topNInput = document.getElementById('topN');
        const checkboxList = document.getElementById('checkboxList');
        const fileInput = document.getElementById('fileInput');
        // Initialize slider values
        startYearSlider.value = minYear;
        endYearSlider.value = maxYear;
        startYearValue.textContent = minYear;
        endYearValue.textContent = maxYear;
        // Event Listeners
        startYearSlider.addEventListener('input', updateYearSlider);
        endYearSlider.addEventListener('input', updateYearSlider);
        topNInput.addEventListener('input', updateDataAndChart);
        fileInput.addEventListener('change', processFile);
        function updateYearSlider() {
            const startYear = parseInt(startYearSlider.value);
            const endYear = parseInt(endYearSlider.value);
            if (startYear > endYear) {
                startYearSlider.value = endYear;
                startYearValue.textContent = endYear;
            } else {
                startYearValue.textContent = startYear;
            }
            if (endYear < startYear) {
                endYearSlider.value = startYear;
                endYearValue.textContent = startYear;
            } else {
                endYearValue.textContent = endYear;
            }
            updateDataAndChart();
        }
        function processFile() {
            const file = fileInput.files[0];
            if (!file) {
                alert('Please select a file.');
                return;
            }
            const reader = new FileReader();
            reader.onload = function(e) {
                const data = e.target.result;
                const workbook = XLSX.read(data, { type: 'array' });
                const sheetName = workbook.SheetNames[0];
                const sheet = workbook.Sheets[sheetName];
                const jsonData = XLSX.utils.sheet_to_json(sheet, { header: 1 });
                // Identify column indices for "Year" and "Author Keywords"
                const yearIndex = jsonData[0].indexOf('Year');
                const keywordIndex = jsonData[0].indexOf('Author Keywords');
                // Extract relevant data based on column indices
                const relevantData = jsonData.slice(1).map(row => [row[yearIndex], row[keywordIndex]]);
                // Handle empty data for keywords
                relevantData.forEach(row => {
                    if (!row[1]) {
                        row[1] = ''; // Replace empty data with an empty string
                    }
                });
                const years = relevantData.slice(1).map(row => parseInt(row[0]));
                minYear = Math.min(...years);
                maxYear = Math.max(...years);
                startYearSlider.min = minYear;
                startYearSlider.max = maxYear;
                startYearSlider.value = minYear;
                endYearSlider.min = minYear;
                endYearSlider.max = maxYear;
                endYearSlider.value = maxYear;
                startYearValue.textContent = minYear;
                endYearValue.textContent = maxYear;
                transformedData = transformData(relevantData);
                document.getElementById('downloadButton').disabled = false;
                updateDataAndChart();
            };
            reader.onerror = function(e) {
                console.error('Error reading file:', e);
                alert('Error reading file. Please try again.');
            };
            reader.readAsArrayBuffer(file);
        }
        function transformData(data) {
            const keywordFrequencyByYear = {};
            const totalKeywordFrequency = {};
            // Process each row in the Excel or CSV data
            for (let i = 1; i < data.length; i++) {
                const year = data[i][0];
                const keywords = data[i][1];
                // Handle empty data for keywords
                if (!keywords) {
                    continue; // Skip rows with empty keywords
                }
                const keywordArray = keywords.split(';').map(keyword => keyword.trim().toLowerCase());
                if (!keywordFrequencyByYear[year]) {
                    keywordFrequencyByYear[year] = {};
                }
                keywordArray.forEach(keyword => {
                    keywordFrequencyByYear[year][keyword] = (keywordFrequencyByYear[year][keyword] || 0) + 1;
                    totalKeywordFrequency[keyword] = (totalKeywordFrequency[keyword] || 0) + 1;
                });
            }
            // Transform data into an array of rows
            const transformedData = [];
            for (const year in keywordFrequencyByYear) {
                const sortedKeywords = Object.entries(keywordFrequencyByYear[year])
                    .sort((a, b) => b[1] - a[1]);
                sortedKeywords.forEach(([keyword, frequency]) => {
                    transformedData.push([keyword, year, frequency, totalKeywordFrequency[keyword]]);
                });
            }
            // Add header row
            transformedData.unshift(['Keyword', 'Year', 'Frequency', 'Total Frequency']);
            return transformedData;
        }
        function updateDataAndChart() {
            const topN = parseInt(topNInput.value);
            const startYear = parseInt(startYearSlider.value);
            const endYear = parseInt(endYearSlider.value);
            const filteredData = transformedData.filter(row => {
                const year = row[1];
                return year >= startYear && year <= endYear;
            });
            document.getElementById('output').textContent = filteredData.map(row => row.join(' | ')).join('\n');
            // Calculate and display the total frequency and unique keywords
            const totalFrequency = calculateTotalFrequency(transformedData);
            const uniqueKeywordsCount = calculateUniqueKeywordsCount(transformedData);
            // Display the total frequency and unique keywords above the graph
            const totalFrequencyDiv = document.getElementById('total-frequency-summary');
            totalFrequencyDiv.innerHTML = `
                <strong>Total Frequency (All Keywords):</strong> ${totalFrequency}<br>
                <strong>Total Unique Keywords:</strong> ${uniqueKeywordsCount}
            `;
            updateCheckboxList(filteredData, topN);
            updateChartWithSelectedKeywords(filteredData, topN);
        }
        function calculateTotalFrequency(data) {
            let totalFrequency = 0;
            for (let i = 1; i < data.length; i++) {
                const frequency = data[i][2];
                totalFrequency += frequency;
            }
            return totalFrequency;
        }
        function calculateUniqueKeywordsCount(data) {
            const uniqueKeywords = new Set();
            for (let i = 1; i < data.length; i++) {
                const keyword = data[i][0];
                uniqueKeywords.add(keyword);
            }
            return uniqueKeywords.size;
        }
        function updateCheckboxList(data, topN) {
            const keywordTotalFrequencies = {};
            const filteredKeywordFrequencies = {};
            // Calculate total frequencies across all years
            for (let i = 1; i < data.length; i++) {
                const keyword = data[i][0];
                const totalFrequency = data[i][3];
                keywordTotalFrequencies[keyword] = totalFrequency;
            }
            // Calculate filtered frequencies for the selected year range
            for (let i = 1; i < data.length; i++) {
                const keyword = data[i][0];
                const year = data[i][1];
                const frequency = data[i][2];
                if (year >= startYearSlider.value && year <= endYearSlider.value) {
                    filteredKeywordFrequencies[keyword] = (filteredKeywordFrequencies[keyword] || 0) + frequency;
                }
            }
            // Get top N keywords based on total frequency
            currentTopKeywords = Object.entries(keywordTotalFrequencies)
                .sort((a, b) => b[1] - a[1])
                .slice(0, topN);
            // Clear and populate the checkbox list
            checkboxList.innerHTML = '<h3>Select Keywords (Frequency)</h3>';
            currentTopKeywords.forEach(([keyword, totalFrequency]) => {
                const filteredFrequency = filteredKeywordFrequencies[keyword] || 0;
                const itemDiv = document.createElement('div');
                itemDiv.className = 'keyword-item';
                const checkbox = document.createElement('input');
                checkbox.type = 'checkbox';
                checkbox.id = `keyword-${keyword}`;
                checkbox.checked = true;
                checkbox.addEventListener('change', () => updateChartWithSelectedKeywords(data, topN));
                const label = document.createElement('label');
                label.htmlFor = `keyword-${keyword}`;
                label.textContent = `${keyword} (${totalFrequency}) [Filtered: ${filteredFrequency}]`;
                itemDiv.appendChild(checkbox);
                itemDiv.appendChild(label);
                checkboxList.appendChild(itemDiv);
            });
        }
        function updateChartWithSelectedKeywords(data, topN) {
            const selectedKeywords = [];
            const checkboxes = checkboxList.querySelectorAll('input[type="checkbox"]');
            checkboxes.forEach(checkbox => {
                if (checkbox.checked) {
                    const keyword = checkbox.id.replace('keyword-', '');
                    selectedKeywords.push(keyword);
                }
            });
            const years = [...new Set(data.slice(1).map(row => row[1]))].sort();
            const datasets = selectedKeywords.map(keyword => {
                const frequencies = years.map(year => {
                    const row = data.find(row => row[0] === keyword && row[1] === year);
                    return row ? row[2] : 0;
                });
                return {
                    label: `${keyword}`,
                    data: frequencies,
                    borderWidth: 2,
                    fill: false
                };
            });
            if (keywordChart) {
                keywordChart.destroy();
            }
            const ctx = document.getElementById('keywordChart').getContext('2d');
            keywordChart = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: years,
                    datasets: datasets
                },
                options: {
                    responsive: true,
                    plugins: {
                        title: {
                            display: true,
                            text: `Keyword Trends (${minYear}-${maxYear})`
                        },
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    return `${context.dataset.label}: ${context.raw}`;
                                }
                            }
                        }
                    },
                    scales: {
                        x: {
                            title: {
                                display: true,
                                text: 'Year'
                            }
                        },
                        y: {
                            title: {
                                display: true,
                                text: 'Frequency'
                            },
                            beginAtZero: true
                        }
                    }
                }
            });
        }
        function downloadCSV() {
            if (transformedData.length === 0) {
                alert('No data to download. Please transform the data first.');
                return;
            }
            const topN = parseInt(topNInput.value);
            const startYear = parseInt(startYearSlider.value);
            const endYear = parseInt(endYearSlider.value);
            // Filter data for the selected year range
            const filteredData = transformedData.filter(row => {
                const year = row[1];
                return year >= startYear && year <= endYear;
            });
            // Prepare the CSV data
            const csvData = [];
            const headers = ['Keyword'];
            for (let year = startYear; year <= endYear; year++) {
                headers.push(year.toString());
            }
            headers.push('Total Within Range', 'Total Across All Years');
            csvData.push(headers);
            const keywordTotalFrequencies = {};
            const filteredKeywordFrequencies = {};
            // Calculate total frequencies across all years
            for (let i = 1; i < transformedData.length; i++) {
                const keyword = transformedData[i][0];
                const totalFrequency = transformedData[i][3]; // Total frequency across all years
                keywordTotalFrequencies[keyword] = totalFrequency;
            }
            // Calculate filtered frequencies for the selected year range
            for (let i = 1; i < filteredData.length; i++) {
                const keyword = filteredData[i][0];
                const year = filteredData[i][1];
                const frequency = filteredData[i][2]; // Frequency within the selected year range
                if (year >= startYear && year <= endYear) {
                    filteredKeywordFrequencies[keyword] = (filteredKeywordFrequencies[keyword] || 0) + frequency;
                }
            }
            // Get top N keywords based on total frequency
            const topKeywords = Object.entries(keywordTotalFrequencies)
                .sort((a, b) => b[1] - a[1]) // Sort by total frequency
                .slice(0, topN);
            // Add rows for top keywords
            topKeywords.forEach(([keyword, totalFrequency]) => {
                const row = [keyword];
                for (let year = startYear; year <= endYear; year++) {
                    const yearData = filteredData.find(row => row[0] === keyword && row[1] === year);
                    row.push(yearData ? yearData[2] : 0); // Use frequency if found, otherwise 0
                }
                row.push(filteredKeywordFrequencies[keyword] || 0); // Total within range
                row.push(totalFrequency); // Total across all years
                csvData.push(row);
            });
            // Convert to CSV format
            const csvContent = csvData.map(row => row.join(',')).join('\n');
            // Create a downloadable link
            const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8;' });
            const link = document.createElement('a');
            link.href = URL.createObjectURL(blob);
            link.download = 'filtered_data.csv';
            link.style.display = 'none';
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
        }
        // Graph resizing functionality
        const graphContainer = document.querySelector('.graph-container');
        const resizeHandle = document.querySelector('.resize-handle');
        let isResizing = false;
        resizeHandle.addEventListener('mousedown', (e) => {
            isResizing = true;
            document.body.style.cursor = 'nwse-resize';
            e.preventDefault();
        });
        document.addEventListener('mousemove', (e) => {
            if (!isResizing) return;
            const containerRect = graphContainer.getBoundingClientRect();
            const newWidth = e.clientX - containerRect.left + 5;
            const newHeight = e.clientY - containerRect.top + 5;
            graphContainer.style.width = `${Math.max(600, newWidth)}px`;
            graphContainer.style.height = `${Math.max(400, newHeight)}px`;
            if (keywordChart) {
                keywordChart.resize();
            }
        });
        document.addEventListener('mouseup', () => {
            isResizing = false;
            document.body.style.cursor = '';
        });
    </script>
    </div>
    <div id="tab2" class="tab-content">
        <!---- Tab 2 Placeholder Content ---->
        <h2>Tab 2</h2>
        <p>This is where you can add the functionality for Tab 2.</p>
    </div>
    <div id="tab3" class="tab-content">
        <!---- Tab 3 Placeholder Content ---->
        <h2>Tab 3</h2>
        <p>This is where you can add the functionality for Tab 3.</p>
    </div>
    <script>
        // Tab Switching Logic
        const tabs = document.querySelectorAll('.tab');
        const tabContents = document.querySelectorAll('.tab-content');
        tabs.forEach(tab => {
            tab.addEventListener('click', () => {
                tabs.forEach(t => t.classList.remove('active'));
                tabContents.forEach(c => c.classList.remove('active'));
                tab.classList.add('active');
                document.getElementById(tab.dataset.tab).classList.add('active');
            });
        });
    </script>
</body>
</html>
