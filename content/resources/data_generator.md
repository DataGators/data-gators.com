+++
title = 'Synthetic Data Generator'
date = 2026-04-03T10:00:00-05:00
draft = false
+++

<!-- add a line drop -->
<center>
&#x200B;
</center>

![Data Generator](/images/syn_data.png)

## Synthetic Data Generator

Generate realistic synthetic data for testing and development purposes. Configure multiple columns with different data types, set ranges and parameters, then download your dataset as CSV or text file.

---

<div id="dataGeneratorApp">
<style>
.generator-container {
max-width: 1200px;
margin: 20px auto;
padding: 20px;
/* background: #f9f9f9; */
background: #213e5f; /*frame of widget*/
border-radius: 8px;
box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}
.control-panel {
/* background: white; */
/* background: #b4a5a5; */
background: #a9b4a5;
padding: 20px;
border-radius: 6px;
margin-bottom: 20px;
}
.columns-list {
/* background: white; */
background: #0b7dda; /*Configured Columns field*/
padding: 20px;
border-radius: 6px;
margin-bottom: 20px;
max-height: 500px;
overflow-y: auto;
}
.column-item {
background: #f5f5f5;
padding: 15px;
margin-bottom: 15px;
border-radius: 4px;
border-left: 4px solid #4CAF50;
}
.column-header {
display: flex;
justify-content: space-between;
align-items: center;
margin-bottom: 10px;
}
.column-title {
font-weight: bold;
font-size: 16px;
color: #333;
}
.column-config {
display: grid;
grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
gap: 10px;
margin-top: 10px;
}
.form-group {
margin-bottom: 15px;
}
.form-group label {
display: block;
margin-bottom: 5px;
font-weight: 500;
/* color: #222; */
color: #222; /* menu titled ("Number of Rows", "Column Name", and "Data Type") */
}
.form-group input,
.form-group select {
width: 100%;
padding: 8px 12px;
border: 1px solid #ddd;
border-radius: 4px;
font-size: 20px;
/* color: #222; */
color: #aeede0; /*Text in the "Number of Rows" "Column Name" and "Data Type" fields */
}
.form-group input::placeholder {
color: #666;
opacity: 1;
}
.btn {
padding: 10px 20px;
border: none;
border-radius: 4px;
cursor: pointer;
font-size: 14px;
font-weight: 500;
transition: all 0.3s;
}
.btn-primary {
background: #4CAF50;
color: white;
}
.btn-primary:hover {
background: #45a049;
}
.btn-secondary {
background: #2196F3;
color: white;
}
.btn-secondary:hover {
background: #0b7dda;
}
.btn-danger {
background: #f44336;
color: white;
}
.btn-danger:hover {
background: #da190b;
}
.btn-warning {
background: #ff9800;
color: white;
}
.btn-warning:hover {
background: #e68900;
}
.btn-group {
display: flex;
gap: 10px;
flex-wrap: wrap;
margin-top: 20px;
}
.preview-section {
background: white;
padding: 20px;
border-radius: 6px;
margin-bottom: 20px;
}
.preview-table {
width: 100%;
overflow-x: auto;
margin-top: 15px;
}
.preview-table table {
width: 100%;
border-collapse: collapse;
font-size: 13px;
}
.preview-table th,
.preview-table td {
padding: 8px;
text-align: left;
border: 1px solid #ddd;
}
.preview-table th {
background: #4CAF50;
color: white;
font-weight: 600;
}
.preview-table tr:nth-child(even) {
background: #f9f9f9;
}
.stats-bar {
display: flex;
justify-content: space-around;
background: #e3f2fd;
padding: 15px;
border-radius: 4px;
margin-bottom: 20px;
}
.stat-item {
text-align: center;
}
.stat-value {
font-size: 24px;
font-weight: bold;
color: #2196F3;
}
.stat-label {
font-size: 12px;
color: #666;
margin-top: 5px;
}
.input-row {
display: flex;
gap: 10px;
align-items: flex-end;
}
.input-row .form-group {
flex: 1;
margin-bottom: 0;
}
</style>

<div class="generator-container">
<!-- Global Settings -->
<div class="control-panel">
<h3>⚙️ Global Settings</h3>
<div class="form-group">
<label for="rowCount">Number of Rows:</label>
<input type="number" id="rowCount" value="500" min="1" max="100000">
</div>
</div>

<!-- Add Column Section -->
<div class="control-panel">
<h3>➕ Add New Column</h3>
<div class="input-row">
<div class="form-group">
<label for="columnName">Column Name:</label>
<input type="text" id="columnName" placeholder="e.g., customer_name">
</div>
<div class="form-group">
<label for="columnType">Data Type:</label>
<select id="columnType" onchange="updateColumnConfig()">
<option value="name">Name (First/Last/Full)</option>
<option value="integer">Integer</option>
<option value="float">Float</option>
<option value="date">Date</option>
<option value="time">Time</option>
<option value="datetime">DateTime</option>
<option value="string">Random String</option>
<option value="char">Single Character</option>
<option value="paragraph">Paragraph</option>
<option value="email">Email Address</option>
<option value="phone">Phone Number</option>
<option value="boolean">Boolean</option>
<option value="uuid">UUID</option>
</select>
</div>
<button class="btn btn-primary" onclick="addColumn()" style="height: 42px;">Add Column</button>
</div>
<div id="columnConfigPreview" style="margin-top: 15px;"></div>
</div>

<!-- Columns List -->
<div class="columns-list">
<h3>📋 Configured Columns (<span id="columnCount">0</span>/20)</h3>
<div id="columnsList"></div>
</div>

<!-- Action Buttons -->
<div class="btn-group">
<button class="btn btn-primary" onclick="generateData()">🔄 Generate Data</button>
<button class="btn btn-secondary" onclick="downloadCSV()">💾 Download CSV</button>
<button class="btn btn-secondary" onclick="downloadTXT()">📄 Download TXT</button>
<button class="btn btn-warning" onclick="clearColumns()">🗑️ Clear All Columns</button>
</div>

<!-- Stats -->
<div class="stats-bar" id="statsBar" style="display: none;">
<div class="stat-item">
<div class="stat-value" id="statRows">0</div>
<div class="stat-label">Rows Generated</div>
</div>
<div class="stat-item">
<div class="stat-value" id="statCols">0</div>
<div class="stat-label">Columns</div>
</div>
<div class="stat-item">
<div class="stat-value" id="statSize">0 KB</div>
<div class="stat-label">Estimated Size</div>
</div>
</div>

<!-- Preview -->
<div class="preview-section" id="previewSection" style="display: none;">
<h3>👁️ Data Preview (First 10 Rows)</h3>
<div class="preview-table" id="previewTable"></div>
</div>
</div>
</div>

<script>
// Global state
let columns = [];
let generatedData = [];

// Sample data for generation
const firstNames = ['James', 'Mary', 'John', 'Patricia', 'Robert', 'Jennifer', 'Michael', 'Linda', 'William', 'Elizabeth', 'David', 'Barbara', 'Richard', 'Susan', 'Joseph', 'Jessica', 'Thomas', 'Sarah', 'Charles', 'Karen', 'Christopher', 'Nancy', 'Daniel', 'Lisa', 'Matthew', 'Betty', 'Anthony', 'Margaret', 'Mark', 'Sandra', 'Donald', 'Ashley', 'Steven', 'Kimberly', 'Paul', 'Emily', 'Andrew', 'Donna', 'Joshua', 'Michelle', 'Kenneth', 'Dorothy', 'Kevin', 'Carol', 'Brian', 'Amanda', 'George', 'Melissa', 'Edward', 'Deborah','Oliver'];

const lastNames = ['Smith', 'Johnson', 'Williams', 'Brown', 'Jones', 'Garcia', 'Miller', 'Davis', 'Rodriguez', 'Martinez', 'Hernandez', 'Lopez', 'Gonzalez', 'Wilson', 'Anderson', 'Thomas', 'Taylor', 'Moore', 'Jackson', 'Martin', 'Lee', 'Perez', 'Thompson', 'White', 'Harris', 'Sanchez', 'Clark', 'Ramirez', 'Lewis', 'Robinson', 'Walker', 'Young', 'Allen', 'King', 'Wright', 'Scott', 'Torres', 'Nguyen', 'Hill', 'Flores', 'Green', 'Adams', 'Nelson', 'Baker', 'Hall', 'Rivera', 'Campbell', 'Mitchell', 'Carter', 'Roberts','Bonham-Carter'];

const loremWords = ['lorem', 'ipsum', 'dolor', 'sit', 'amet', 'consectetur', 'adipiscing', 'elit', 'sed', 'do', 'eiusmod', 'tempor', 'incididunt', 'ut', 'labore', 'et', 'dolore', 'magna', 'aliqua', 'enim', 'ad', 'minim', 'veniam', 'quis', 'nostrud', 'exercitation', 'ullamco', 'laboris', 'nisi', 'aliquip', 'ex', 'ea', 'commodo', 'consequat', 'duis', 'aute', 'irure', 'in', 'reprehenderit', 'voluptate', 'velit', 'esse', 'cillum', 'fugiat', 'nulla', 'pariatur', 'excepteur', 'sint', 'occaecat', 'cupidatat', 'non', 'proident', 'sunt', 'culpa', 'qui', 'officia', 'deserunt', 'mollit', 'anim', 'id', 'est', 'laborum'];

const domains = ['gmail.com', 'yahoo.com', 'outlook.com', 'hotmail.com', 'example.com', 'company.com', 'allegheny.edu'];

// Update column config preview based on type
function updateColumnConfig () {
const type = document.getElementById('columnType').value;
const preview = document.getElementById('columnConfigPreview');

//let html = '<div style="background: #fff3cd; padding: 10px; border-radius: 4px; border-left: 4px solid #ffc107;"><strong>Configuration for ' + type + ':</strong> ';

let html = '<div style="background: #0b7dda; padding: 10px; border-radius: 4px; border-left: 4px solid #ffc107;"><strong>Configuration for ' + type + ':</strong> ';

switch(type) {
case 'name':
html += 'Generates random first names, last names, or full names';
break;
case 'integer':
html += 'Range: -1000 to 1000 (customizable after adding)';
break;
case 'float':
html += 'Range: 0.0 to 100.0 with 2 decimal places (customizable after adding)';
break;
case 'date':
html += 'Dates from 2020-01-01 to 2026-12-31 (customizable after adding)';
break;
case 'time':
html += 'Random times in 24-hour format (HH:MM:SS)';
break;
case 'datetime':
html += 'Combined date and time (customizable after adding)';
break;
case 'string':
html += 'Random alphanumeric strings, length 8 characters (customizable after adding)';
break;
case 'char':
html += 'Single random character (A-Z, a-z)';
break;
case 'paragraph':
html += 'Random paragraphs with 3-10 sentences (customizable after adding)';
break;
case 'email':
html += 'Random email addresses (based on names)';
break;
case 'phone':
html += 'Random phone numbers in format (XXX) XXX-XXXX';
break;
case 'boolean':
html += 'Random true/false values';
break;
case 'uuid':
html += 'Random UUID v4 identifiers';
break;
}

html += '</div>';
preview.innerHTML = html;
}

// Add a column
function addColumn() {
if (columns.length >= 20) {
alert('Maximum 20 columns allowed');
return;
}

const name = document.getElementById('columnName').value.trim();
const type = document.getElementById('columnType').value;

if (!name) {
alert('Please enter a column name');
return;
}

if (columns.some(col => col.name === name)) {
alert('Column name already exists');
return;
}

const column = {
name: name,
type: type,
config: getDefaultConfig(type)
};

columns.push(column);
document.getElementById('columnName').value = '';
updateColumnsList();
}

// Get default configuration for column type
function getDefaultConfig(type) {
switch(type) {
case 'name':
return { subtype: 'full' }; // first, last, full
case 'integer':
return { min: -1000, max: 1000 };
case 'float':
return { min: 0, max: 100, decimals: 2 };
case 'date':
return { start: '2020-01-01', end: '2026-12-31' };
case 'datetime':
return { start: '2020-01-01', end: '2026-12-31' };
case 'string':
return { length: 8, charset: 'alphanumeric' }; // alphanumeric, alpha, numeric
case 'paragraph':
return { minSentences: 3, maxSentences: 10, minWords: 5, maxWords: 15 };
default:
return {};
}
}

// Update columns list display
function updateColumnsList() {
const list = document.getElementById('columnsList');
document.getElementById('columnCount').textContent = columns.length;

if (columns.length === 0) {
list.innerHTML = '<p style="text-align: center; color: #999;">No columns configured yet. Add columns above to get started.</p>';
return;
}

list.innerHTML = columns.map((col, index) => `
<div class="column-item">
<div class="column-header">
<span class="column-title">${col.name} - ${col.type}</span>
<button class="btn btn-danger" onclick="removeColumn(${index})" style="padding: 5px 10px;">Remove</button>
</div>
${getConfigHTML(col, index)}
</div>
`).join('');
}

// Get configuration HTML for a column
function getConfigHTML(col, index) {
let html = '<div class="column-config">';

switch(col.type) {
case 'name':
html += `
<div class="form-group">
<label>Name Type:</label>
<select onchange="updateColumnConfigValue(${index}, 'subtype', this.value)" value="${col.config.subtype}">
<option value="first" ${col.config.subtype === 'first' ? 'selected' : ''}>First Name</option>
<option value="last" ${col.config.subtype === 'last' ? 'selected' : ''}>Last Name</option>
<option value="full" ${col.config.subtype === 'full' ? 'selected' : ''}>Full Name</option>
</select>
</div>
`;
break;
case 'integer':
html += `
<div class="form-group">
<label>Min Value:</label>
<input type="number" value="${col.config.min}" onchange="updateColumnConfigValue(${index}, 'min', parseInt(this.value))">
</div>
<div class="form-group">
<label>Max Value:</label>
<input type="number" value="${col.config.max}" onchange="updateColumnConfigValue(${index}, 'max', parseInt(this.value))">
</div>
`;
break;
case 'float':
html += `
<div class="form-group">
<label>Min Value:</label>
<input type="number" step="0.01" value="${col.config.min}" onchange="updateColumnConfigValue(${index}, 'min', parseFloat(this.value))">
</div>
<div class="form-group">
<label>Max Value:</label>
<input type="number" step="0.01" value="${col.config.max}" onchange="updateColumnConfigValue(${index}, 'max', parseFloat(this.value))">
</div>
<div class="form-group">
<label>Decimal Places:</label>
<input type="number" min="0" max="10" value="${col.config.decimals}" onchange="updateColumnConfigValue(${index}, 'decimals', parseInt(this.value))">
</div>
`;
break;
case 'date':
case 'datetime':
html += `
<div class="form-group">
<label>Start Date:</label>
<input type="date" value="${col.config.start}" onchange="updateColumnConfigValue(${index}, 'start', this.value)">
</div>
<div class="form-group">
<label>End Date:</label>
<input type="date" value="${col.config.end}" onchange="updateColumnConfigValue(${index}, 'end', this.value)">
</div>
`;
break;
case 'string':
html += `
<div class="form-group">
<label>String Length:</label>
<input type="number" min="1" max="100" value="${col.config.length}" onchange="updateColumnConfigValue(${index}, 'length', parseInt(this.value))">
</div>
<div class="form-group">
<label>Character Set:</label>
<select onchange="updateColumnConfigValue(${index}, 'charset', this.value)">
<option value="alphanumeric" ${col.config.charset === 'alphanumeric' ? 'selected' : ''}>Alphanumeric</option>
<option value="alpha" ${col.config.charset === 'alpha' ? 'selected' : ''}>Letters Only</option>
<option value="numeric" ${col.config.charset === 'numeric' ? 'selected' : ''}>Numbers Only</option>
</select>
</div>
`;
break;
case 'paragraph':
html += `
<div class="form-group">
<label>Min Sentences:</label>
<input type="number" min="1" value="${col.config.minSentences}" onchange="updateColumnConfigValue(${index}, 'minSentences', parseInt(this.value))">
</div>
<div class="form-group">
<label>Max Sentences:</label>
<input type="number" min="1" value="${col.config.maxSentences}" onchange="updateColumnConfigValue(${index}, 'maxSentences', parseInt(this.value))">
</div>
<div class="form-group">
<label>Min Words/Sentence:</label>
<input type="number" min="1" value="${col.config.minWords}" onchange="updateColumnConfigValue(${index}, 'minWords', parseInt(this.value))">
</div>
<div class="form-group">
<label>Max Words/Sentence:</label>
<input type="number" min="1" value="${col.config.maxWords}" onchange="updateColumnConfigValue(${index}, 'maxWords', parseInt(this.value))">
</div>
`;
break;
}

html += '</div>';
return html;
}

// Update column configuration
function updateColumnConfigValue(index, key, value) {
columns[index].config[key] = value;
}

// Remove a column
function removeColumn(index) {
columns.splice(index, 1);
updateColumnsList();
}

// Clear all columns
function clearColumns() {
if (confirm('Are you sure you want to remove all columns?')) {
columns = [];
generatedData = [];
updateColumnsList();
document.getElementById('previewSection').style.display = 'none';
document.getElementById('statsBar').style.display = 'none';
}
}

// Random number helpers
function randomInt(min, max) {
return Math.floor(Math.random() * (max - min + 1)) + min;
}

function randomFloat(min, max, decimals) {
const value = Math.random() * (max - min) + min;
return parseFloat(value.toFixed(decimals));
}

function randomDate(start, end) {
const startDate = new Date(start);
const endDate = new Date(end);
const randomTime = startDate.getTime() + Math.random() * (endDate.getTime() - startDate.getTime());
return new Date(randomTime);
}

function formatDate(date) {
return date.toISOString().split('T')[0];
}

function formatTime() {
const hours = randomInt(0, 23).toString().padStart(2, '0');
const minutes = randomInt(0, 59).toString().padStart(2, '0');
const seconds = randomInt(0, 59).toString().padStart(2, '0');
return `${hours}:${minutes}:${seconds}`;
}

function formatDateTime(date) {
const dateStr = formatDate(date);
const timeStr = formatTime();
return `${dateStr} ${timeStr}`;
}

function randomString(length, charset) {
let chars = '';
switch(charset) {
case 'alphanumeric':
chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
break;
case 'alpha':
chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz';
break;
case 'numeric':
chars = '0123456789';
break;
}
let result = '';
for (let i = 0; i < length; i++) {
result += chars.charAt(Math.floor(Math.random() * chars.length));
}
return result;
}

function randomChar() {
const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz';
return chars.charAt(Math.floor(Math.random() * chars.length));
}

function randomParagraph(minSentences, maxSentences, minWords, maxWords) {
const numSentences = randomInt(minSentences, maxSentences);
const sentences = [];

for (let i = 0; i < numSentences; i++) {
const numWords = randomInt(minWords, maxWords);
const words = [];
for (let j = 0; j < numWords; j++) {
words.push(loremWords[randomInt(0, loremWords.length - 1)]);
}
let sentence = words.join(' ');
sentence = sentence.charAt(0).toUpperCase() + sentence.slice(1) + '.';
sentences.push(sentence);
}

return sentences.join(' ');
}

function randomEmail(firstName, lastName) {
const domain = domains[randomInt(0, domains.length - 1)];
const num = Math.random() > 0.7 ? randomInt(1, 999) : '';
return `${firstName.toLowerCase()}${lastName ? '.' + lastName.toLowerCase() : ''}${num}@${domain}`;
}

function randomPhone() {
const area = randomInt(200, 999);
const prefix = randomInt(200, 999);
const line = randomInt(1000, 9999);
return `(${area}) ${prefix}-${line}`;
}

function randomBoolean() {
return Math.random() > 0.5 ? 'true' : 'false';
}

function randomUUID() {
return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function(c) {
const r = Math.random() * 16 | 0;
const v = c === 'x' ? r : (r & 0x3 | 0x8);
return v.toString(16);
});
}

// Generate value based on column type
function generateValue(column) {
switch(column.type) {
case 'name':
const firstName = firstNames[randomInt(0, firstNames.length - 1)];
const lastName = lastNames[randomInt(0, lastNames.length - 1)];
if (column.config.subtype === 'first') return firstName;
if (column.config.subtype === 'last') return lastName;
return `${firstName} ${lastName}`;

case 'integer':
return randomInt(column.config.min, column.config.max);

case 'float':
return randomFloat(column.config.min, column.config.max, column.config.decimals);

case 'date':
return formatDate(randomDate(column.config.start, column.config.end));

case 'time':
return formatTime();

case 'datetime':
return formatDateTime(randomDate(column.config.start, column.config.end));

case 'string':
return randomString(column.config.length, column.config.charset);

case 'char':
return randomChar();

case 'paragraph':
return randomParagraph(
column.config.minSentences,
column.config.maxSentences,
column.config.minWords,
column.config.maxWords
);

case 'email':
const fName = firstNames[randomInt(0, firstNames.length - 1)];
const lName = lastNames[randomInt(0, lastNames.length - 1)];
return randomEmail(fName, lName);

case 'phone':
return randomPhone();

case 'boolean':
return randomBoolean();

case 'uuid':
return randomUUID();

default:
return '';
}
}

// Generate data
function generateData() {
if (columns.length === 0) {
alert('Please add at least one column');
return;
}

const rowCount = parseInt(document.getElementById('rowCount').value);

if (rowCount < 1 || rowCount > 100000) {
alert('Row count must be between 1 and 100,000');
return;
}

generatedData = [];

for (let i = 0; i < rowCount; i++) {
const row = {};
columns.forEach(col => {
row[col.name] = generateValue(col);
});
generatedData.push(row);
}

updateStats();
updatePreview();
}

// Update statistics
function updateStats() {
document.getElementById('statRows').textContent = generatedData.length.toLocaleString();
document.getElementById('statCols').textContent = columns.length;

// Estimate size
const sampleSize = JSON.stringify(generatedData).length;
const sizeKB = (sampleSize / 1024).toFixed(2);
document.getElementById('statSize').textContent = sizeKB + ' KB';

document.getElementById('statsBar').style.display = 'flex';
}

// Update preview table
function updatePreview() {
const previewCount = Math.min(10, generatedData.length);
const previewData = generatedData.slice(0, previewCount);

let html = '<table><thead><tr>';
columns.forEach(col => {
html += `<th>${col.name}</th>`;
});
html += '</tr></thead><tbody>';

previewData.forEach(row => {
html += '<tr>';
columns.forEach(col => {
const value = row[col.name];
const displayValue = col.type === 'paragraph' && value.length > 50 
? value.substring(0, 50) + '...' 
: value;
html += `<td>${displayValue}</td>`;
});
html += '</tr>';
});

html += '</tbody></table>';

document.getElementById('previewTable').innerHTML = html;
document.getElementById('previewSection').style.display = 'block';
}

// Download as CSV
function downloadCSV() {
if (generatedData.length === 0) {
alert('Please generate data first');
return;
}

let csv = columns.map(col => `"${col.name}"`).join(',') + '\n';

generatedData.forEach(row => {
const values = columns.map(col => {
const value = row[col.name];
// Escape quotes and wrap in quotes if contains comma, newline, or quote
const stringValue = String(value).replace(/"/g, '""');
return /[",\n\r]/.test(stringValue) ? `"${stringValue}"` : stringValue;
});
csv += values.join(',') + '\n';
});

downloadFile(csv, 'synthetic_data.csv', 'text/csv');
}

// Download as TXT
function downloadTXT() {
if (generatedData.length === 0) {
alert('Please generate data first');
return;
}

let txt = columns.map(col => col.name).join('\t') + '\n';

generatedData.forEach(row => {
const values = columns.map(col => row[col.name]);
txt += values.join('\t') + '\n';
});

downloadFile(txt, 'synthetic_data.txt', 'text/plain');
}

// Download file helper
function downloadFile(content, filename, type) {
const blob = new Blob([content], { type: type });
const url = URL.createObjectURL(blob);
const link = document.createElement('a');
link.href = url;
link.download = filename;
document.body.appendChild(link);
link.click();
document.body.removeChild(link);
URL.revokeObjectURL(url);
}

// Initialize
updateColumnConfig();

</script>

---

## How to Use

1. **Set Global Settings**: Choose how many rows of data you want to generate (1 to 100,000)

2. **Add Columns**: 
   - Enter a column name
   - Select the data type
   - Click "Add Column"
   - Configure column-specific settings (ranges, formats, etc.)

3. **Configure Each Column**: After adding a column, you can customize its parameters:
   - **Name**: Choose first name, last name, or full name
   - **Integer**: Set minimum and maximum values
   - **Float**: Set range and decimal precision
   - **Date/DateTime**: Define date ranges
   - **String**: Set length and character type
   - **Paragraph**: Control sentence and word counts

4. **Generate Data**: Click "Generate Data" to create your synthetic dataset

5. **Preview**: Review the first 10 rows to ensure the data meets your needs

6. **Download**: Save as CSV for spreadsheet applications or TXT for text processing

## Use Cases

- Testing data analysis scripts and pipelines
- Developing data visualization prototypes
- Teaching data science concepts with realistic datasets
- Benchmarking algorithm performance
- Creating sample data for demonstrations
- Text analysis and natural language processing experiments

## Tips

- Start with a small number of rows to test your configuration
- Use meaningful column names that reflect your analysis needs
- For text analysis, use the paragraph type with varied sentence lengths
- Combine different data types to create realistic relational datasets
- Check the preview before downloading large datasets

---

**Note**: All data generated is completely synthetic and random. No real personal information is used or stored.
