
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
         #output2 {
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
        #output3 {
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
         #output4 {
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
        #output5 {
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
            overflow-y: auto;
           
        }
        #checkboxList2 {
            width: 100%;
            background: #fff;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
            overflow-y: auto;
           
        }
        #checkboxList3 {
            width: 100%;
            background: #fff;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
            overflow-y: auto;
           
        }
        #checkboxList h3 {
            margin-top: 0;
            margin-bottom: 15px;
            font-size: 1.2rem;
            color: #333;
            border-bottom: 1px solid #eee;
            padding-bottom: 10px;
        }
                  .search-input {
              width: 100%;
              padding: 8px;
              margin-bottom: 10px;
              border: 1px solid #ddd;
              border-radius: 4px;
          }

          .keywords-container {
              max-height: 300px;
              overflow-y: auto;
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

<div class="tabs">
        <div class="tab active" data-tab="tab1">Author Keywords</div>
        <div class="tab" data-tab="tab2">Index Keywords</div>
        <div class="tab" data-tab="tab3">Author + Index</div>
        <div class="tab" data-tab="tab4">Title</div>
        <div class="tab" data-tab="tab5">Abstract</div>
</div>
    
<div id="tab1" class="tab-content active">
        <!-- Tab 1 Content -->

        <h1>Excel Data Transformation</h1>

         <div class="upload-container">
            <div class="file-input">
                <label for="fileInput">Choose File</label>
                <input type="file" id="fileInput" accept=".xlsx, .csv" />
            </div>
            <div class="input-field">
                <label for="topN">Show Top:</label>
                <input type="number" id="topN" min="1" value="10" />
            </div>
            <button class="download-button" onclick="downloadCSV()" disabled id="downloadButton">Download  CSV</button>
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
            <div id="checkboxList"></div>
            <h3>Select Keywords (Frequency)</h3>
            <!-- Checkboxes will be dynamically added here -->
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
                        // Find the exact match for this keyword and year
                        const match = transformedData.find(r => 
                            r[0] === keyword && 
                            r[1] == year &&  // Note: == instead of === to handle string vs number
                            year >= startYear && 
                            year <= endYear
                        );
                        row.push(match ? match[2] : 0);
                    }
                    row.push(filteredKeywordFrequencies[keyword] || 0);
                    row.push(totalFrequency);
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
    <!-- Tab 2 Content -->
    <h1>Excel Data Transformation</h1>
   
     <div class="upload-container">
        <div class="file-input">
            <label for="fileInput2">Choose File</label>
            <input type="file" id="fileInput2" accept=".xlsx, .csv" />
        </div>
        <div class="input-field">
            <label for="topN2">Show Top:</label>
            <input type="number" id="topN2" min="1" value="10" />
        </div>
        <button class="download-button" onclick="downloadCSV2()" disabled id="downloadButton2">Download CSV</button>
    </div>
   
   
    <div id="total-frequency-summary2"></div>
    <pre id="output2"></pre>
    <div class="main-container">
        <!-- Year range slider above the graph -->
        <div class="range-slider">
            <label for="yearRange2">Year Range:</label>
            <div class="slider-container">
                <input type="range" id="startYear2" min="2000" max="2099" value="2000" />
                <input type="range" id="endYear2" min="2000" max="2099" value="2099" />
            </div>
            <div class="slider-values">
                <span id="startYearValue2">2000</span>
                <span id="endYearValue2">2099</span>
            </div>
        </div>
        <!-- Graph with resize handle -->
        <div class="graph-container">
            <canvas id="keywordChart2"></canvas>
            <div class="resize-handle"></div>
        </div>
        <!-- Checkbox list below the graph -->
        <div id="checkboxList2" style="max-height: 200px; overflow-y: auto;">
            <h3>Select Keywords (Frequency)</h3>
            <!-- Checkboxes will be dynamically added here -->
        </div>
    </div>

<script>
    // Specific code for Tab 2 (Index Keywords)
    let transformedData2 = [];
    let keywordChart2 = null;
    let minYear2 = 2000;
    let maxYear2 = 2099;
    let currentTopKeywords2 = [];
    // DOM Elements for Tab 2
    const startYearSlider2 = document.getElementById('startYear2');
    const endYearSlider2 = document.getElementById('endYear2');
    const startYearValue2 = document.getElementById('startYearValue2');
    const endYearValue2 = document.getElementById('endYearValue2');
    const topNInput2 = document.getElementById('topN2');
    const checkboxList2 = document.getElementById('checkboxList2');
    const fileInput2 = document.getElementById('fileInput2');

    // Event Listeners for file upload in Tab 2
     // Initialize slider values
        startYearSlider2.value = minYear2;
        endYearSlider2.value = maxYear2;
        startYearValue2.textContent = minYear2;
        endYearValue2.textContent = maxYear2;
        // Event Listeners
        startYearSlider2.addEventListener('input', updateYearSlider2);
        endYearSlider2.addEventListener('input', updateYearSlider2);
        topNInput2.addEventListener('input', updateDataAndChart2);
        fileInput2.addEventListener('change', processFile2);
        function updateYearSlider2() {
            const startYear2 = parseInt(startYearSlider2.value);
            const endYear2 = parseInt(endYearSlider2.value);
            if (startYear2 > endYear2) {
                startYearSlider2.value = endYear2;
                startYearValue2.textContent = endYear2;
            } else {
                startYearValue2.textContent = startYear2;
            }
            if (endYear2 < startYear2) {
                endYearSlider2.value = startYear2;
                endYearValue2.textContent = startYear2;
            } else {
                endYearValue2.textContent = endYear2;
            }
            updateDataAndChart2();
        }
    // Function to process the uploaded file for Tab 2
    function processFile2() {
        const file2 = fileInput2.files[0];
        if (!file2) {
            alert('Please select a file.');
            return;
        }
        const reader2 = new FileReader();
        reader2.onload = function(e) {
            const data2 = e.target.result;
            const workbook2 = XLSX.read(data2, { type: 'array' });
            const sheetName2 = workbook2.SheetNames[0];
            const sheet2 = workbook2.Sheets[sheetName2];
            const jsonData2 = XLSX.utils.sheet_to_json(sheet2, { header: 1 });

            // Identify column indices for "Year" and "Index Keywords"
            const yearIndex2 = jsonData2[0].indexOf('Year');
            const keywordIndex2 = jsonData2[0].indexOf('Index Keywords');

            // Extract relevant data based on column indices
            const relevantData2 = jsonData2.slice(1).map(row => [row[yearIndex2], row[keywordIndex2]]);

            // Handle empty data for keywords
            relevantData2.forEach(row2 => {
                if (!row2[1]) {
                    row2[1] = ''; // Replace empty data with an empty string
                }
            });

            const years2 = relevantData2.slice(1).map(row2 => parseInt(row2[0]));
            minYear2 = Math.min(...years2);
            maxYear2 = Math.max(...years2);
            startYearSlider2.min = minYear2;
            endYearSlider2.min = minYear2;
            startYearSlider2.max = maxYear2;
            endYearSlider2.max = maxYear2;
            startYearSlider2.value = minYear2;
            endYearSlider2.value = maxYear2;
            startYearValue2.textContent = minYear2;
            endYearValue2.textContent = maxYear2;

            transformedData2 = transformData2(relevantData2);
            document.getElementById('downloadButton2').disabled = false;
            updateDataAndChart2();
        };
        reader2.onerror = function(e) {
            console.error('Error reading file:', e);
            alert('Error reading file. Please try again.');
        };
        reader2.readAsArrayBuffer(file2);
    }

    // Function to transform data for Tab 2
    function transformData2(data) {
        const keywordFrequencyByYear2 = {};
        const totalKeywordFrequency2 = {};
        // Process each row in the Excel or CSV data
        for (let i = 1; i < data.length; i++) {
            const year2 = data[i][0];
            const keywords2 = data[i][1];
            // Handle empty data for keywords
            if (!keywords2) {
                continue; // Skip rows with empty keywords
            }
            const keywordArray2 = keywords2.split(';').map(keyword2 => keyword2.trim().toLowerCase());
            if (!keywordFrequencyByYear2[year2]) {
                keywordFrequencyByYear2[year2] = {};
            }
            keywordArray2.forEach(keyword2 => {
                keywordFrequencyByYear2[year2][keyword2] = (keywordFrequencyByYear2[year2][keyword2] || 0) + 1;
                totalKeywordFrequency2[keyword2] = (totalKeywordFrequency2[keyword2] || 0) + 1;
            });
        }
        // Transform data into an array of rows
        const transformedData2 = [];
        for (const year2 in keywordFrequencyByYear2) {
            const sortedKeywords2 = Object.entries(keywordFrequencyByYear2[year2])
                .sort((a, b) => b[1] - a[1]);
            sortedKeywords2.forEach(([keyword2, frequency2]) => {
                transformedData2.push([keyword2, year2, frequency2, totalKeywordFrequency2[keyword2]]);
            });
        }
        // Add header row
        transformedData2.unshift(['Keyword', 'Year', 'Frequency', 'Total Frequency']);
        return transformedData2;
    }

    // Function to update data and charts for Tab 2
    function updateDataAndChart2() {
        const topN2 = parseInt(topNInput2.value);
        const startYear2 = parseInt(startYearSlider2.value);
        const endYear2 = parseInt(endYearSlider2.value);
        const filteredData2 = transformedData2.filter(row2 => {
            const year2 = row2[1];
            return year2 >= startYear2 && year2 <= endYear2;
        });
        document.getElementById('output2').textContent = filteredData2.map(row2 => row2.join(' | ')).join('\n');
        // Calculate and display the total frequency and unique keywords
        const totalFrequency2= calculateTotalFrequency2(transformedData2);
        const uniqueKeywordsCount2 = calculateUniqueKeywordsCount2(transformedData2);
        // Display the total frequency and unique keywords above the graph
        const totalFrequencyDiv2 = document.getElementById('total-frequency-summary2');
        totalFrequencyDiv2.innerHTML = `
            <strong>Total Frequency (All Keywords):</strong> ${totalFrequency2}<br>
            <strong>Total Unique Keywords:</strong> ${uniqueKeywordsCount2}
        `;
        updateCheckboxList2(filteredData2, topN2);
        updateChartWithSelectedKeywords2(filteredData2, topN2);
    }

    // Function to calculate total frequency
    function calculateTotalFrequency2(data2) {
        let totalFrequency2 = 0;
        for (let i = 1; i < data2.length; i++) {
            const frequency2 = data2[i][2];
            totalFrequency2 += frequency2;
        }
        return totalFrequency2;
    }

    // Function to calculate unique keywords count
    function calculateUniqueKeywordsCount2(data2) {
        const uniqueKeywords2 = new Set();
        for (let i = 1; i < data2.length; i++) {
            const keyword2 = data2[i][0];
            uniqueKeywords2.add(keyword2);
        }
        return uniqueKeywords2.size;
    }

    // Function to update checkbox list for Tab 2
    function updateCheckboxList2(data2, topN2) {
        const keywordTotalFrequencies2 = {};
        const filteredKeywordFrequencies2 = {};
        // Calculate total frequencies across all years
        for (let i = 1; i < data2.length; i++) {
            const keyword2 = data2[i][0];
            const totalFrequency2 = data2[i][3];
            keywordTotalFrequencies2[keyword2] = totalFrequency2;
        }
        // Calculate filtered frequencies for the selected year range
        for (let i = 1; i < data2.length; i++) {
            const keyword2 = data2[i][0];
            const year2 = data2[i][1];
            const frequency2 = data2[i][2];
            if (year2 >= startYearSlider2.value && year2 <= endYearSlider2.value) {
                filteredKeywordFrequencies2[keyword2] = (filteredKeywordFrequencies2[keyword2] || 0) + frequency2;
            }
        }
        // Get top N keywords based on total frequency
        currentTopKeywords2 = Object.entries(keywordTotalFrequencies2)
            .sort((a, b) => b[1] - a[1])
            .slice(0, topN2);
        // Clear and populate the checkbox list
        checkboxList2.innerHTML = '<h3>Select Keywords (Frequency)</h3>';
        currentTopKeywords2.forEach(([keyword2, totalFrequency2]) => {
            const filteredFrequency2 = filteredKeywordFrequencies2[keyword2] || 0;
            const itemDiv2 = document.createElement('div');
            itemDiv2.className = 'keyword-item';
            const checkbox2 = document.createElement('input');
            checkbox2.type = 'checkbox';
            checkbox2.id = `keyword-${keyword2}`;
            checkbox2.checked = true;
            checkbox2.addEventListener('change', () => updateChartWithSelectedKeywords2(data2, topN2));
            const label = document.createElement('label');
            label.htmlFor = `keyword-${keyword2}`;
            label.textContent = `${keyword2} (${totalFrequency2}) [Filtered: ${filteredFrequency2}]`;
            itemDiv2.appendChild(checkbox2);
            itemDiv2.appendChild(label);
            checkboxList2.appendChild(itemDiv2);
        });
    }

    // Function to update chart for Tab 2
    function updateChartWithSelectedKeywords2(data2, topN2) {
        const selectedKeywords2 = [];
        const checkboxes2 = checkboxList2.querySelectorAll('input[type="checkbox"]');
        checkboxes2.forEach(checkbox2 => {
            if (checkbox2.checked) {
                const keyword2 = checkbox2.id.replace('keyword-', '');
                selectedKeywords2.push(keyword2);
            }
        });
        const years2 = [...new Set(data2.slice(1).map(row2 => row2[1]))].sort();
        const datasets2 = selectedKeywords2.map(keyword2 => {
            const frequencies2 = years2.map(year2 => {
                const row2 = data2.find(row2 => row2[0] === keyword2 && row2[1] === year2);
                return row2 ? row2[2] : 0;
            });
            return {
                label: `${keyword2}`,
                data: frequencies2,
                borderWidth: 2,
                fill: false
            };
        });
        if (keywordChart2) {
            keywordChart2.destroy();
        }
        const ctx = document.getElementById('keywordChart2').getContext('2d');
        keywordChart2 = new Chart(ctx, {
            type: 'line',
            data: {
                labels: years2,
                datasets: datasets2
            },
            options: {
                responsive: true,
                plugins: {
                    title: {
                        display: true,
                        text: `Keyword Trends (${minYear2}-${maxYear2})`
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

    // Function to download CSV for Tab 2
    function downloadCSV2() {
        if (transformedData2.length === 0) {
            alert('No data to download. Please transform the data first.');
            return;
        }
        const topN2 = parseInt(topNInput2.value);
        const startYear2 = parseInt(startYearSlider2.value);
        const endYear2 = parseInt(endYearSlider2.value);
        // Filter data for the selected year range
        const filteredData2 = transformedData2.filter(row2 => {
            const year2 = row2[1];
            return year2 >= startYear2 && year2 <= endYear2;
        });
        // Prepare the CSV data
        const csvData2 = [];
        const headers2 = ['Keyword'];
        for (let year2 = startYear2; year2 <= endYear2; year2++) {
            headers2.push(year2.toString());
        }
        headers2.push('Total Within Range', 'Total Across All Years');
        csvData2.push(headers2);
        const keywordTotalFrequencies2 = {};
        const filteredKeywordFrequencies2 = {};
        // Calculate total frequencies across all years
        for (let i = 1; i < transformedData2.length; i++) {
            const keyword2 = transformedData2[i][0];
            const totalFrequency2 = transformedData2[i][3]; // Total frequency across all years
            keywordTotalFrequencies2[keyword2] = totalFrequency2;
        }
        // Calculate filtered frequencies for the selected year range
        for (let i = 1; i < filteredData2.length; i++) {
            const keyword2 = filteredData2[i][0];
            const year2 = filteredData2[i][1];
            const frequency2 = filteredData2[i][2]; // Frequency within the selected year range
            if (year2 >= startYear2 && year2 <= endYear2) {
                filteredKeywordFrequencies2[keyword2] = (filteredKeywordFrequencies2[keyword2] || 0) + frequency2;
            }
        }
        // Get top N keywords based on total frequency
        const topKeywords2 = Object.entries(keywordTotalFrequencies2)
            .sort((a, b) => b[1] - a[1]) // Sort by total frequency
            .slice(0, topN2);
        // Add rows for top keywords
      
    topKeywords2.forEach(([keyword2, totalFrequency2]) => {
        const row2 = [keyword2];
        for (let year2 = startYear2; year2 <= endYear2; year2++) {
            const match = transformedData2.find(r => 
                r[0] === keyword2 && 
                r[1] == year2 &&  // Note: == instead of ===
                year2 >= startYear2 && 
                year2 <= endYear2
            );
            row2.push(match ? match[2] : 0);
        }
        row2.push(filteredKeywordFrequencies2[keyword2] || 0);
        row2.push(totalFrequency2);
        csvData2.push(row2);
    });

        // Convert to CSV format
        const csvContent2 = csvData2.map(row2 => row2.join(',')).join('\n');
        // Create a downloadable link
        const blob = new Blob([csvContent2], { type: 'text/csv;charset=utf-8;' });
        const link = document.createElement('a');
        link.href = URL.createObjectURL(blob);
        link.download = 'filtered_data_index_keywords.csv';
        link.style.display = 'none';
        document.body.appendChild(link);
        link.click();
        document.body.removeChild(link);
    }
</script>
</div>

<!-- Tab 3 Content -->
<div id="tab3" class="tab-content">
    <h1>Combined Keywords Analysis</h1>
    <div class="upload-container">
        <div class="file-input">
            <label for="fileInput3">Choose File</label>
            <input type="file" id="fileInput3" accept=".xlsx, .csv" />
        </div>
        <div class="input-field">
            <label for="topN3">Show Top:</label>
            <input type="number" id="topN3" min="1" value="10" />
        </div>
        <button class="download-button" onclick="downloadCSV3()" disabled id="downloadButton3">Download CSV</button>
    </div>
    <div id="total-frequency-summary3"></div>
    <pre id="output3"></pre>
    <div class="main-container">
        <!-- Year range slider -->
        <div class="range-slider">
            <label for="yearRange3">Year Range:</label>
            <div class="slider-container">
                <input type="range" id="startYear3" min="2000" max="2099" value="2000" />
                <input type="range" id="endYear3" min="2000" max="2099" value="2099" />
            </div>
            <div class="slider-values">
                <span id="startYearValue3">2000</span>
                <span id="endYearValue3">2099</span>
            </div>
        </div>
        <!-- Graph -->
        <div class="graph-container">
            <canvas id="keywordChart3"></canvas>
            <div class="resize-handle"></div>
        </div>
        <!-- Checkbox list -->
        <div id="checkboxList3" style="max-height: 200px; overflow-y: auto;">
            <h3>Select Keywords (Frequency)</h3>
        </div>
    </div>
</div>

<script>

// Tab 3 Variables
let transformedData3 = [];
let keywordChart3 = null;
let minYear3 = 2000;
let maxYear3 = 2099;
let currentTopKeywords3 = [];

// DOM Elements for Tab 3
const fileInput3 = document.getElementById('fileInput3');
const startYearSlider3 = document.getElementById('startYear3');
const endYearSlider3 = document.getElementById('endYear3');
const startYearValue3 = document.getElementById('startYearValue3');
const endYearValue3 = document.getElementById('endYearValue3');
const topNInput3 = document.getElementById('topN3');
const checkboxList3 = document.getElementById('checkboxList3');

// Initialize sliders
startYearSlider3.value = minYear3;
endYearSlider3.value = maxYear3;
startYearValue3.textContent = minYear3;
endYearValue3.textContent = maxYear3;

// Event Listeners
fileInput3.addEventListener('change', processFile3);
startYearSlider3.addEventListener('input', updateYearSlider3);
endYearSlider3.addEventListener('input', updateYearSlider3);
topNInput3.addEventListener('input', updateDataAndChart3);

function updateYearSlider3() {
    const startYear = parseInt(startYearSlider3.value);
    const endYear = parseInt(endYearSlider3.value);
    if (startYear > endYear) {
        startYearSlider3.value = endYear;
        startYearValue3.textContent = endYear;
    } else {
        startYearValue3.textContent = startYear;
    }
    if (endYear < startYear) {
        endYearSlider3.value = startYear;
        endYearValue3.textContent = startYear;
    } else {
        endYearValue3.textContent = endYear;
    }
    updateDataAndChart3();
}

function processFile3() {
    const file = fileInput3.files[0];
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

        // Debug: Log headers to verify column names
        console.log("Headers:", jsonData[0]);

        const yearIndex = jsonData[0].indexOf('Year');
        const authorKeywordsIndex = jsonData[0].indexOf('Author Keywords');
        const indexKeywordsIndex = jsonData[0].indexOf('Index Keywords');

        if (authorKeywordsIndex === -1 || indexKeywordsIndex === -1) {
            alert('Error: File must contain "Author Keywords" and "Index Keywords" columns.');
            return;
        }

        // Merge keywords
        const relevantData = jsonData.slice(1).map(row => {
            const year = row[yearIndex];
            const authorKeywords = row[authorKeywordsIndex] || '';
            const indexKeywords = row[indexKeywordsIndex] || '';
            const combinedKeywords = `${authorKeywords};${indexKeywords}`
                .split(';')
                .map(k => k.trim().toLowerCase())
                .filter(k => k !== '');
            return [year, combinedKeywords.join(';')];
        });

        const years = relevantData.map(row => parseInt(row[0])).filter(year => !isNaN(year));
        minYear3 = Math.min(...years);
        maxYear3 = Math.max(...years);
        startYearSlider3.min = minYear3;
        endYearSlider3.min = minYear3;
        startYearSlider3.max = maxYear3;
        endYearSlider3.max = maxYear3;
        startYearSlider3.value = minYear3;
        endYearSlider3.value = maxYear3;
        startYearValue3.textContent = minYear3;
        endYearValue3.textContent = maxYear3;

        transformedData3 = transformData3(relevantData);
        console.log("Transformed data (first 5 rows):", transformedData3.slice(0, 5));
        document.getElementById('downloadButton3').disabled = false;
        updateDataAndChart3();
    };
    reader.readAsArrayBuffer(file);
}

function transformData3(data) {
    const keywordFrequencyByYear = {};
    const totalKeywordFrequency = {};
    for (let i = 0; i < data.length; i++) {
        const year = data[i][0];
        const keywords = data[i][1];
        if (!keywords) continue;
        const keywordArray = keywords.split(';').map(k => k.trim().toLowerCase());
        if (!keywordFrequencyByYear[year]) {
            keywordFrequencyByYear[year] = {};
        }
        keywordArray.forEach(keyword => {
            keywordFrequencyByYear[year][keyword] = (keywordFrequencyByYear[year][keyword] || 0) + 1;
            totalKeywordFrequency[keyword] = (totalKeywordFrequency[keyword] || 0) + 1;
        });
    }
    const transformedData = [];
    for (const year in keywordFrequencyByYear) {
        Object.entries(keywordFrequencyByYear[year])
            .sort((a, b) => b[1] - a[1])
            .forEach(([keyword, freq]) => {
                transformedData.push([keyword, year, freq, totalKeywordFrequency[keyword]]);
            });
    }
    transformedData.unshift(['Keyword', 'Year', 'Frequency', 'Total Frequency']);
    return transformedData;
}

function updateDataAndChart3() {
    const topN = parseInt(topNInput3.value);
    const startYear = parseInt(startYearSlider3.value);
    const endYear = parseInt(endYearSlider3.value);
    const filteredData = transformedData3.filter(row => {
        const year = row[1];
        return year >= startYear && year <= endYear;
    });
    document.getElementById('output3').textContent = filteredData.map(row => row.join(' | ')).join('\n');

    // Update summary
    const totalFrequency = calculateTotalFrequency3(transformedData3);
    const uniqueKeywordsCount = calculateUniqueKeywordsCount3(transformedData3);
    document.getElementById('total-frequency-summary3').innerHTML = `
        <strong>Total Frequency (All Keywords):</strong> ${totalFrequency}<br>
        <strong>Total Unique Keywords:</strong> ${uniqueKeywordsCount}
    `;

    updateCheckboxList3(filteredData, topN);
    updateChartWithSelectedKeywords3(filteredData, topN);
}

function calculateTotalFrequency3(data) {
    let total = 0;
    for (let i = 1; i < data.length; i++) total += data[i][2];
    return total;
}

function calculateUniqueKeywordsCount3(data) {
    const unique = new Set();
    for (let i = 1; i < data.length; i++) unique.add(data[i][0]);
    return unique.size;
}

function updateCheckboxList3(data, topN) {
    const keywordTotals = {};
    const filteredFrequencies = {};
    for (let i = 1; i < data.length; i++) {
        const keyword = data[i][0];
        keywordTotals[keyword] = data[i][3];
        if (data[i][1] >= startYearSlider3.value && data[i][1] <= endYearSlider3.value) {
            filteredFrequencies[keyword] = (filteredFrequencies[keyword] || 0) + data[i][2];
        }
    }
    currentTopKeywords3 = Object.entries(keywordTotals)
        .sort((a, b) => b[1] - a[1])
        .slice(0, topN);

    checkboxList3.innerHTML = '<h3>Select Keywords (Frequency)</h3>';
    currentTopKeywords3.forEach(([keyword, total]) => {
        const filtered = filteredFrequencies[keyword] || 0;
        const div = document.createElement('div');
        div.className = 'keyword-item';
        div.innerHTML = `
            <input type="checkbox" id="keyword-${keyword}" checked>
            <label for="keyword-${keyword}">${keyword} (${total}) [Filtered: ${filtered}]</label>
        `;
        div.querySelector('input').addEventListener('change', () => updateChartWithSelectedKeywords3(data, topN));
        checkboxList3.appendChild(div);
    });
}

function updateChartWithSelectedKeywords3(data, topN) {
    const selected = [];
    checkboxList3.querySelectorAll('input[type="checkbox"]').forEach(checkbox => {
        if (checkbox.checked) selected.push(checkbox.id.replace('keyword-', ''));
    });

    const years = [...new Set(data.slice(1).map(row => row[1]))].sort();
    const datasets = selected.map(keyword => {
        const freqs = years.map(year => {
            const row = data.find(r => r[0] === keyword && r[1] === year);
            return row ? row[2] : 0;
        });
        return { label: keyword, data: freqs, borderWidth: 2, fill: false };
    });

    if (keywordChart3) keywordChart3.destroy();
    const ctx = document.getElementById('keywordChart3').getContext('2d');
    keywordChart3 = new Chart(ctx, {
        type: 'line',
        data: { labels: years, datasets },
        options: {
            responsive: true,
            plugins: {
                title: { display: true, text: `Keyword Trends (${minYear3}-${maxYear3})` },
                tooltip: { callbacks: { label: ctx => `${ctx.dataset.label}: ${ctx.raw}` } }
            },
            scales: {
                x: { title: { display: true, text: 'Year' } },
                y: { title: { display: true, text: 'Frequency' }, beginAtZero: true }
            }
        }
    });
}

function downloadCSV3() {
    if (transformedData3.length === 0) {
        alert('No data to download. Please transform the data first.');
        return;
    }
    const topN = parseInt(topNInput3.value);
    const startYear = parseInt(startYearSlider3.value);
    const endYear = parseInt(endYearSlider3.value);
    const filteredData = transformedData3.filter(row => {
        const year = row[1];
        return year >= startYear && year <= endYear;
    });

    const csvData = [['Keyword', ...Array.from({ length: endYear - startYear + 1 }, (_, i) => startYear + i), 'Total Within Range', 'Total Across All Years']];
    const keywordTotals = {};
    const filteredFrequencies = {};

    for (let i = 1; i < transformedData3.length; i++) {
        keywordTotals[transformedData3[i][0]] = transformedData3[i][3];
    }
    for (let i = 1; i < filteredData.length; i++) {
        const keyword = filteredData[i][0];
        const year = filteredData[i][1];
        const freq = filteredData[i][2];
        if (year >= startYear && year <= endYear) {
            filteredFrequencies[keyword] = (filteredFrequencies[keyword] || 0) + freq;
        }
    }

  Object.entries(keywordTotals)
        .sort((a, b) => b[1] - a[1])
        .slice(0, topN)
        .forEach(([keyword, total]) => {
            const row = [keyword];
            for (let year = startYear; year <= endYear; year++) {
                const match = transformedData3.find(r => 
                    r[0] === keyword && 
                    r[1] == year &&  // Note: == instead of ===
                    year >= startYear && 
                    year <= endYear
                );
                row.push(match ? match[2] : 0);
            }
            row.push(filteredFrequencies[keyword] || 0, total);
            csvData.push(row);
        });

    const blob = new Blob([csvData.map(row => row.join(',')).join('\n')], { type: 'text/csv' });
    const link = document.createElement('a');
    link.href = URL.createObjectURL(blob);
    link.download = 'combined_keywords_data.csv';
    link.click();
}
</script>

<!-- Add this with your other tabs -->
<div id="tab4" class="tab-content">
    <h1>Title Word Analysis</h1>
   
    <div class="upload-container">
        <div class="file-input">
            <label for="fileInput4">Choose File</label>
            <input type="file" id="fileInput4" accept=".xlsx, .csv" />
        </div>
        <div class="input-field">
            <label for="topN4">Show Top:</label>
            <input type="number" id="topN4" min="1" value="10" />
        </div>
        <button class="download-button" onclick="downloadCSV4()" disabled id="downloadButton4">Download CSV</button>
    </div>
   
    <div id="total-frequency-summary4"></div>
    <pre id="output4"></pre>
    <div class="main-container">
        <!-- Year range slider -->
        <div class="range-slider">
            <label for="yearRange4">Year Range:</label>
            <div class="slider-container">
                <input type="range" id="startYear4" min="2000" max="2099" value="2000" />
                <input type="range" id="endYear4" min="2000" max="2099" value="2099" />
            </div>
            <div class="slider-values">
                <span id="startYearValue4">2000</span>
                <span id="endYearValue4">2099</span>
            </div>
        </div>
       
        <!-- Graph -->
        <div class="graph-container">
            <canvas id="wordChart4"></canvas>
            <div class="resize-handle"></div>
        </div>
       
        <!-- Checkbox list with search -->
        <div id="checkboxList4" style="max-height: 200px; overflow-y: auto;">
            <h3>Select Words (Frequency)</h3>
            <input type="text" id="searchWords4" placeholder="Search words..." class="search-input">
            <div id="wordsContainer4"></div>
        </div>
    </div>
</div>

<script>
// Tab 4 Variables
let transformedData4 = [];
let wordChart4 = null;
let minYear4 = 2000;
let maxYear4 = 2099;
let currentTopWords4 = [];

// DOM Elements for Tab 4
const fileInput4 = document.getElementById('fileInput4');
const startYearSlider4 = document.getElementById('startYear4');
const endYearSlider4 = document.getElementById('endYear4');
const startYearValue4 = document.getElementById('startYearValue4');
const endYearValue4 = document.getElementById('endYearValue4');
const topNInput4 = document.getElementById('topN4');
const checkboxList4 = document.getElementById('checkboxList4');

// Initialize sliders
startYearSlider4.value = minYear4;
endYearSlider4.value = maxYear4;
startYearValue4.textContent = minYear4;
endYearValue4.textContent = maxYear4;

// Event Listeners
fileInput4.addEventListener('change', processFile4);
startYearSlider4.addEventListener('input', updateYearSlider4);
endYearSlider4.addEventListener('input', updateYearSlider4);
topNInput4.addEventListener('input', updateDataAndChart4);

function updateYearSlider4() {
    const startYear = parseInt(startYearSlider4.value);
    const endYear = parseInt(endYearSlider4.value);
   
    if (startYear > endYear) {
        startYearSlider4.value = endYear;
        startYearValue4.textContent = endYear;
    } else {
        startYearValue4.textContent = startYear;
    }
   
    if (endYear < startYear) {
        endYearSlider4.value = startYear;
        endYearValue4.textContent = startYear;
    } else {
        endYearValue4.textContent = endYear;
    }
   
    updateDataAndChart4();
}

function processFile4() {
    const file = fileInput4.files[0];
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

        // Identify column indices
        const yearIndex = jsonData[0].indexOf('Year');
        const titleIndex = jsonData[0].indexOf('Title');
       
        if (yearIndex === -1 || titleIndex === -1) {
            alert('Error: File must contain "Year" and "Title" columns.');
            return;
        }

        // Extract and process titles
        const relevantData = jsonData.slice(1).map(row => {
            const year = row[yearIndex];
            const title = row[titleIndex] || '';
           
            // Split title into words and clean them
            const words = title
                .split(/\s+/) // Split by one or more spaces
                .map(word => word
                    .replace(/[^\w\s]/g, '') // Remove punctuation
                    .toLowerCase()
                    .trim()
                )
                .filter(word => word.length > 0); // Remove empty strings
               
            return [year, words];
        });

        // Process years
        const years = relevantData.map(row => parseInt(row[0])).filter(year => !isNaN(year));
        minYear4 = Math.min(...years);
        maxYear4 = Math.max(...years);
       
        // Update sliders
        startYearSlider4.min = minYear4;
        endYearSlider4.min = minYear4;
        startYearSlider4.max = maxYear4;
        endYearSlider4.max = maxYear4;
        startYearSlider4.value = minYear4;
        endYearSlider4.value = maxYear4;
        startYearValue4.textContent = minYear4;
        endYearValue4.textContent = maxYear4;

        transformedData4 = transformData4(relevantData);
        document.getElementById('downloadButton4').disabled = false;
        updateDataAndChart4();
    };
    reader.readAsArrayBuffer(file);
}

function transformData4(data) {
    const wordFrequencyByYear = {};
    const totalWordFrequency = {};
   
    // Process each row
    for (let i = 0; i < data.length; i++) {
        const year = data[i][0];
        const words = data[i][1];
       
        if (!wordFrequencyByYear[year]) {
            wordFrequencyByYear[year] = {};
        }
       
        // Count each word
        words.forEach(word => {
            wordFrequencyByYear[year][word] = (wordFrequencyByYear[year][word] || 0) + 1;
            totalWordFrequency[word] = (totalWordFrequency[word] || 0) + 1;
        });
    }
   
    // Convert to array format
    const transformedData = [];
    for (const year in wordFrequencyByYear) {
        Object.entries(wordFrequencyByYear[year])
            .sort((a, b) => b[1] - a[1]) // Sort by frequency
            .forEach(([word, freq]) => {
                transformedData.push([word, year, freq, totalWordFrequency[word]]);
            });
    }
   
    // Add header row
    transformedData.unshift(['Word', 'Year', 'Frequency', 'Total Frequency']);
    return transformedData;
}

function updateDataAndChart4() {
    const topN = parseInt(topNInput4.value);
    const startYear = parseInt(startYearSlider4.value);
    const endYear = parseInt(endYearSlider4.value);
   
    // Filter data by year range
    const filteredData = transformedData4.filter(row => {
        const year = row[1];
        return year >= startYear && year <= endYear;
    });
   
    // Update output display
    document.getElementById('output4').textContent = filteredData.map(row => row.join(' | ')).join('\n');
   
    // Update summary
    const totalFrequency = calculateTotalFrequency4(transformedData4);
    const uniqueWordsCount = calculateUniqueWordsCount4(transformedData4);
    document.getElementById('total-frequency-summary4').innerHTML = `
        <strong>Total Frequency (All Words):</strong> ${totalFrequency}<br>
        <strong>Total Unique Words:</strong> ${uniqueWordsCount}
    `;
   
    updateCheckboxList4(filteredData, topN);
    updateChartWithSelectedWords4(filteredData, topN);
}

function calculateTotalFrequency4(data) {
    let total = 0;
    for (let i = 1; i < data.length; i++) total += data[i][2];
    return total;
}

function calculateUniqueWordsCount4(data) {
    const unique = new Set();
    for (let i = 1; i < data.length; i++) unique.add(data[i][0]);
    return unique.size;
}

function updateCheckboxList4(data, topN) {
    const wordTotals = {};
    const filteredFrequencies = {};
   
    // Calculate frequencies
    for (let i = 1; i < data.length; i++) {
        const word = data[i][0];
        wordTotals[word] = data[i][3]; // Total frequency
       
        const year = data[i][1];
        const freq = data[i][2];
        if (year >= startYearSlider4.value && year <= endYearSlider4.value) {
            filteredFrequencies[word] = (filteredFrequencies[word] || 0) + freq;
        }
    }
   
    // Get top words
    currentTopWords4 = Object.entries(wordTotals)
        .sort((a, b) => b[1] - a[1]) // Sort by frequency
        .slice(0, topN);
   
    // Update HTML
    checkboxList4.innerHTML = `
        <h3>Select Words (Frequency)</h3>
        <input type="text" id="searchWords4" placeholder="Search words..." class="search-input">
        <div class="keywords-container" id="wordsContainer4"></div>
    `;
   
    const wordsContainer = document.getElementById('wordsContainer4');
   
    // Add words to container
    currentTopWords4.forEach(([word, total]) => {
        const filtered = filteredFrequencies[word] || 0;
        const itemDiv = document.createElement('div');
        itemDiv.className = 'keyword-item';
        itemDiv.innerHTML = `
            <input type="checkbox" id="word-4-${word}" checked>
            <label for="word-4-${word}">
                ${word} (${total}) [Filtered: ${filtered}]
            </label>
        `;
        itemDiv.querySelector('input').addEventListener('change', () =>
            updateChartWithSelectedWords4(data, topN));
        wordsContainer.appendChild(itemDiv);
    });
   
    // Add search functionality
    document.getElementById('searchWords4').addEventListener('input', (e) => {
        const searchTerm = e.target.value.toLowerCase();
        const items = wordsContainer.querySelectorAll('.keyword-item');
       
        items.forEach(item => {
            const label = item.querySelector('label').textContent.toLowerCase();
            item.style.display = label.includes(searchTerm) ? 'flex' : 'none';
        });
    });
}

function updateChartWithSelectedWords4(data, topN) {
    const selected = [];
    const checkboxes = document.querySelectorAll('#wordsContainer4 input[type="checkbox"]');
   
    checkboxes.forEach(checkbox => {
        if (checkbox.checked) {
            const word = checkbox.id.replace('word-4-', '');
            selected.push(word);
        }
    });
   
    const years = [...new Set(data.slice(1).map(row => row[1]))].sort();
    const datasets = selected.map(word => {
        const freqs = years.map(year => {
            const row = data.find(r => r[0] === word && r[1] === year);
            return row ? row[2] : 0;
        });
        return {
            label: word,
            data: freqs,
            borderWidth: 2,
            fill: false
        };
    });
   
    // Update chart
    if (wordChart4) wordChart4.destroy();
   
    const ctx = document.getElementById('wordChart4').getContext('2d');
    wordChart4 = new Chart(ctx, {
        type: 'line',
        data: { labels: years, datasets },
        options: {
            responsive: true,
            plugins: {
                title: { display: true, text: `Word Trends (${minYear4}-${maxYear4})` },
                tooltip: {
                    callbacks: {
                        label: function(context) {
                            return `${context.dataset.label}: ${context.raw}`;
                        }
                    }
                }
            },
            scales: {
                x: { title: { display: true, text: 'Year' } },
                y: { title: { display: true, text: 'Frequency' }, beginAtZero: true }
            }
        }
    });
}

function downloadCSV4() {
    if (transformedData4.length === 0) {
        alert('No data to download. Please transform the data first.');
        return;
    }
   
    const topN = parseInt(topNInput4.value);
    const startYear = parseInt(startYearSlider4.value);
    const endYear = parseInt(endYearSlider4.value);
   
    // Filter data by year range
    const filteredData = transformedData4.filter(row => {
        const year = row[1];
        return year >= startYear && year <= endYear;
    });
   
    // Prepare CSV data
    const csvData = [['Word', ...Array.from({ length: endYear - startYear + 1 }, (_, i) => startYear + i), 'Total Within Range', 'Total Across All Years']];
   
    const wordTotals = {};
    const filteredFrequencies = {};
   
    // Calculate frequencies
    for (let i = 1; i < transformedData4.length; i++) {
        wordTotals[transformedData4[i][0]] = transformedData4[i][3];
    }
   
    for (let i = 1; i < filteredData.length; i++) {
        const word = filteredData[i][0];
        const year = filteredData[i][1];
        const freq = filteredData[i][2];
       
        if (year >= startYear && year <= endYear) {
            filteredFrequencies[word] = (filteredFrequencies[word] || 0) + freq;
        }
    }
   
    // Add top words to CSV
    Object.entries(wordTotals)
        .sort((a, b) => b[1] - a[1])
        .slice(0, topN)
        .forEach(([word, total]) => {
            const row = [word];
            for (let year = startYear; year <= endYear; year++) {
                const match = transformedData4.find(r => 
                    r[0] === word && 
                    r[1] == year &&  // Note: == instead of ===
                    year >= startYear && 
                    year <= endYear
                );
                row.push(match ? match[2] : 0);
            }
            row.push(filteredFrequencies[word] || 0, total);
            csvData.push(row);
        });
        
    // Create download link
    const blob = new Blob([csvData.map(row => row.join(',')).join('\n')], { type: 'text/csv' });
    const link = document.createElement('a');
    link.href = URL.createObjectURL(blob);
    link.download = 'title_word_analysis.csv';
    link.click();
}
</script>


<div id="tab5" class="tab-content">
    <h1>Abstract Word Analysis</h1>
    
    <div class="upload-container">
        <div class="file-input">
            <label for="fileInput5">Choose File</label>
            <input type="file" id="fileInput5" accept=".xlsx, .csv" />
        </div>
        <div class="input-field">
            <label for="topN5">Show Top:</label>
            <input type="number" id="topN5" min="1" value="10" />
        </div>
        <button class="download-button" onclick="downloadCSV5()" disabled id="downloadButton5">Download CSV</button>
    </div>
    
    <div id="total-frequency-summary5"></div>
    <pre id="output5"></pre>
    <div class="main-container">
        <!-- Year range slider -->
        <div class="range-slider">
            <label for="yearRange5">Year Range:</label>
            <div class="slider-container">
                <input type="range" id="startYear5" min="2000" max="2099" value="2000" />
                <input type="range" id="endYear5" min="2000" max="2099" value="2099" />
            </div>
            <div class="slider-values">
                <span id="startYearValue5">2000</span>
                <span id="endYearValue5">2099</span>
            </div>
        </div>
        
        <!-- Graph -->
        <div class="graph-container">
            <canvas id="wordChart5"></canvas>
            <div class="resize-handle"></div>
        </div>
        
        <!-- Checkbox list with search -->
        <div id="checkboxList5" style="max-height: 200px; overflow-y: auto;">
            <h3>Select Words (Frequency)</h3>
            <input type="text" id="searchWords5" placeholder="Search words..." class="search-input">
            <div id="wordsContainer5"></div>
        </div>
    </div>
</div>

<script>
// Tab 5 Variables
let transformedData5 = [];
let wordChart5 = null;
let minYear5 = 2000;
let maxYear5 = 2099;
let currentTopWords5 = [];

// DOM Elements for Tab 5
const fileInput5 = document.getElementById('fileInput5');
const startYearSlider5 = document.getElementById('startYear5');
const endYearSlider5 = document.getElementById('endYear5');
const startYearValue5 = document.getElementById('startYearValue5');
const endYearValue5 = document.getElementById('endYearValue5');
const topNInput5 = document.getElementById('topN5');
const checkboxList5 = document.getElementById('checkboxList5');

// Initialize sliders
startYearSlider5.value = minYear5;
endYearSlider5.value = maxYear5;
startYearValue5.textContent = minYear5;
endYearValue5.textContent = maxYear5;

// Event Listeners
fileInput5.addEventListener('change', processFile5);
startYearSlider5.addEventListener('input', updateYearSlider5);
endYearSlider5.addEventListener('input', updateYearSlider5);
topNInput5.addEventListener('input', updateDataAndChart5);

function updateYearSlider5() {
    const startYear = parseInt(startYearSlider5.value);
    const endYear = parseInt(endYearSlider5.value);
    
    if (startYear > endYear) {
        startYearSlider5.value = endYear;
        startYearValue5.textContent = endYear;
    } else {
        startYearValue5.textContent = startYear;
    }
    
    if (endYear < startYear) {
        endYearSlider5.value = startYear;
        endYearValue5.textContent = startYear;
    } else {
        endYearValue5.textContent = endYear;
    }
    
    updateDataAndChart5();
}

function processFile5() {
    const file = fileInput5.files[0];
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

        // Identify column indices
        const yearIndex = jsonData[0].indexOf('Year');
        const abstractIndex = jsonData[0].indexOf('Abstract');
        
        if (yearIndex === -1 || abstractIndex === -1) {
            alert('Error: File must contain "Year" and "Abstract" columns.');
            return;
        }

        // Extract and process abstracts
        const relevantData = jsonData.slice(1).map(row => {
            const year = row[yearIndex];
            const abstract = row[abstractIndex] || '';
            
            // Split abstract into words and clean them
            const words = abstract
                .split(/\s+/) // Split by one or more spaces
                .map(word => word
                    .replace(/[^\w\s]/g, '') // Remove punctuation
                    .toLowerCase()
                    .trim()
                )
                .filter(word => word.length > 0); // Remove empty strings
                
            return [year, words];
        });

        // Process years
        const years = relevantData.map(row => parseInt(row[0])).filter(year => !isNaN(year));
        minYear5 = Math.min(...years);
        maxYear5 = Math.max(...years);
        
        // Update sliders
        startYearSlider5.min = minYear5;
        endYearSlider5.min = minYear5;
        startYearSlider5.max = maxYear5;
        endYearSlider5.max = maxYear5;
        startYearSlider5.value = minYear5;
        endYearSlider5.value = maxYear5;
        startYearValue5.textContent = minYear5;
        endYearValue5.textContent = maxYear5;

        transformedData5 = transformData5(relevantData);
        document.getElementById('downloadButton5').disabled = false;
        updateDataAndChart5();
    };
    reader.readAsArrayBuffer(file);
}

function transformData5(data) {
    const wordFrequencyByYear = {};
    const totalWordFrequency = {};
    
    // Process each row
    for (let i = 0; i < data.length; i++) {
        const year = data[i][0];
        const words = data[i][1];
        
        if (!wordFrequencyByYear[year]) {
            wordFrequencyByYear[year] = {};
        }
        
        // Count each word
        words.forEach(word => {
            wordFrequencyByYear[year][word] = (wordFrequencyByYear[year][word] || 0) + 1;
            totalWordFrequency[word] = (totalWordFrequency[word] || 0) + 1;
        });
    }
    
    // Convert to array format
    const transformedData = [];
    for (const year in wordFrequencyByYear) {
        Object.entries(wordFrequencyByYear[year])
            .sort((a, b) => b[1] - a[1]) // Sort by frequency
            .forEach(([word, freq]) => {
                transformedData.push([word, year, freq, totalWordFrequency[word]]);
            });
    }
    
    // Add header row
    transformedData.unshift(['Word', 'Year', 'Frequency', 'Total Frequency']);
    return transformedData;
}

function updateDataAndChart5() {
    const topN = parseInt(topNInput5.value);
    const startYear = parseInt(startYearSlider5.value);
    const endYear = parseInt(endYearSlider5.value);
    
    // Filter data by year range
    const filteredData = transformedData5.filter(row => {
        const year = row[1];
        return year >= startYear && year <= endYear;
    });
    
    // Update output display
    document.getElementById('output5').textContent = filteredData.map(row => row.join(' | ')).join('\n');
    
    // Update summary
    const totalFrequency = calculateTotalFrequency5(transformedData5);
    const uniqueWordsCount = calculateUniqueWordsCount5(transformedData5);
    document.getElementById('total-frequency-summary5').innerHTML = `
        <strong>Total Frequency (All Words):</strong> ${totalFrequency}<br>
        <strong>Total Unique Words:</strong> ${uniqueWordsCount}
    `;
    
    updateCheckboxList5(filteredData, topN);
    updateChartWithSelectedWords5(filteredData, topN);
}

function calculateTotalFrequency5(data) {
    let total = 0;
    for (let i = 1; i < data.length; i++) total += data[i][2];
    return total;
}

function calculateUniqueWordsCount5(data) {
    const unique = new Set();
    for (let i = 1; i < data.length; i++) unique.add(data[i][0]);
    return unique.size;
}

function updateCheckboxList5(data, topN) {
    const wordTotals = {};
    const filteredFrequencies = {};
    
    // Calculate frequencies
    for (let i = 1; i < data.length; i++) {
        const word = data[i][0];
        wordTotals[word] = data[i][3]; // Total frequency
        
        const year = data[i][1];
        const freq = data[i][2];
        if (year >= startYearSlider5.value && year <= endYearSlider5.value) {
            filteredFrequencies[word] = (filteredFrequencies[word] || 0) + freq;
        }
    }
    
    // Get top words
    currentTopWords5 = Object.entries(wordTotals)
        .sort((a, b) => b[1] - a[1]) // Sort by frequency
        .slice(0, topN);
    
    // Update HTML
    checkboxList5.innerHTML = `
        <h3>Select Words (Frequency)</h3>
        <input type="text" id="searchWords5" placeholder="Search words..." class="search-input">
        <div class="keywords-container" id="wordsContainer5"></div>
    `;
    
    const wordsContainer = document.getElementById('wordsContainer5');
    
    // Add words to container
    currentTopWords5.forEach(([word, total]) => {
        const filtered = filteredFrequencies[word] || 0;
        const itemDiv = document.createElement('div');
        itemDiv.className = 'keyword-item';
        itemDiv.innerHTML = `
            <input type="checkbox" id="word-5-${word}" checked>
            <label for="word-5-${word}">
                ${word} (${total}) [Filtered: ${filtered}]
            </label>
        `;
        itemDiv.querySelector('input').addEventListener('change', () => 
            updateChartWithSelectedWords5(data, topN));
        wordsContainer.appendChild(itemDiv);
    });
    
    // Add search functionality
    document.getElementById('searchWords5').addEventListener('input', (e) => {
        const searchTerm = e.target.value.toLowerCase();
        const items = wordsContainer.querySelectorAll('.keyword-item');
        
        items.forEach(item => {
            const label = item.querySelector('label').textContent.toLowerCase();
            item.style.display = label.includes(searchTerm) ? 'flex' : 'none';
        });
    });
}

function updateChartWithSelectedWords5(data, topN) {
    const selected = [];
    const checkboxes = document.querySelectorAll('#wordsContainer5 input[type="checkbox"]');
    
    checkboxes.forEach(checkbox => {
        if (checkbox.checked) {
            const word = checkbox.id.replace('word-5-', '');
            selected.push(word);
        }
    });
    
    const years = [...new Set(data.slice(1).map(row => row[1]))].sort();
    const datasets = selected.map(word => {
        const freqs = years.map(year => {
            const row = data.find(r => r[0] === word && r[1] === year);
            return row ? row[2] : 0;
        });
        return { 
            label: word,
            data: freqs,
            borderWidth: 2,
            fill: false
        };
    });
    
    // Update chart
    if (wordChart5) wordChart5.destroy();
    
    const ctx = document.getElementById('wordChart5').getContext('2d');
    wordChart5 = new Chart(ctx, {
        type: 'line',
        data: { labels: years, datasets },
        options: {
            responsive: true,
            plugins: {
                title: { display: true, text: `Word Trends (${minYear5}-${maxYear5})` },
                tooltip: {
                    callbacks: {
                        label: function(context) {
                            return `${context.dataset.label}: ${context.raw}`;
                        }
                    }
                }
            },
            scales: {
                x: { title: { display: true, text: 'Year' } },
                y: { title: { display: true, text: 'Frequency' }, beginAtZero: true }
            }
        }
    });
}

function downloadCSV5() {
    if (transformedData5.length === 0) {
        alert('No data to download. Please transform the data first.');
        return;
    }
    
    const topN = parseInt(topNInput5.value);
    const startYear = parseInt(startYearSlider5.value);
    const endYear = parseInt(endYearSlider5.value);
    
    // Filter data by year range
    const filteredData = transformedData5.filter(row => {
        const year = row[1];
        return year >= startYear && year <= endYear;
    });
    
    // Prepare CSV data
    const csvData = [['Word', ...Array.from({ length: endYear - startYear + 1 }, (_, i) => startYear + i), 'Total Within Range', 'Total Across All Years']];
    
    const wordTotals = {};
    const filteredFrequencies = {};
    
    // Calculate frequencies
    for (let i = 1; i < transformedData5.length; i++) {
        wordTotals[transformedData5[i][0]] = transformedData5[i][3];
    }
    
    for (let i = 1; i < filteredData.length; i++) {
        const word = filteredData[i][0];
        const year = filteredData[i][1];
        const freq = filteredData[i][2];
        
        if (year >= startYear && year <= endYear) {
            filteredFrequencies[word] = (filteredFrequencies[word] || 0) + freq;
        }
    }
    
    // Add top words to CSV
    Object.entries(wordTotals)
        .sort((a, b) => b[1] - a[1])
        .slice(0, topN)
        .forEach(([word, total]) => {
            const row = [word];
            for (let year = startYear; year <= endYear; year++) {
                const match = transformedData5.find(r => 
                    r[0] === word && 
                    r[1] == year &&  // Note: == instead of ===
                    year >= startYear && 
                    year <= endYear
                );
                row.push(match ? match[2] : 0);
            }
            row.push(filteredFrequencies[word] || 0, total);
            csvData.push(row);
        });
        
    // Create download link
    const blob = new Blob([csvData.map(row => row.join(',')).join('\n')], { type: 'text/csv' });
    const link = document.createElement('a');
    link.href = URL.createObjectURL(blob);
    link.download = 'abstract_word_analysis.csv';
    link.click();
}
</script>

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
