<!DOCTYPE html>
<html>
<head>
  <title>NIST Dataset Viewer</title>
  <style>
    /* Your CSS styles here */
  </style>
  <script>
    // Fetch and display the current data
    function showCurrentData() {
      const url = "https://smstestbed.nist.gov/vds/current";
      fetchData(url);
    }
    
    // Fetch and display the sample data
    function showSampleData() {
      const url = "https://smstestbed.nist.gov/vds/sample";
      fetchData(url);
    }
    
    // Fetch data from the given URL and display it on the webpage
    function fetchData(url) {
      fetch(url)
        .then(response => response.text())
        .then(data => {
          // Parse the XML data and extract the relevant information
          const parser = new DOMParser();
          const xmlDoc = parser.parseFromString(data, "text/xml");

          // Clear the existing data in the container
          const dataContainer = document.getElementById("data-container");
          dataContainer.innerHTML = "";

          // Extract data items and display them on the webpage
          const devices = xmlDoc.getElementsByTagName("Device");
          for (let i = 0; i < devices.length; i++) {
            const device = devices[i];
            const deviceId = device.getAttribute("id");
            const deviceName = device.getAttribute("name");
            
            // Create a div for device information
            const deviceInfo = document.createElement("div");
            deviceInfo.classList.add("device");
            deviceInfo.innerHTML = `<h2>${deviceName}</h2>`;
            dataContainer.appendChild(deviceInfo);
            
            // Display data items for the device
            const dataItems = device.getElementsByTagName("DataItem");
            for (let j = 0; j < dataItems.length; j++) {
              const dataItem = dataItems[j];
              const dataItemId = dataItem.getAttribute("id");
              const dataItemName = dataItem.getAttribute("name");
              const dataItemValue = dataItem.textContent;
              
              // Create a div for data item information
              const dataItemInfo = document.createElement("div");
              dataItemInfo.classList.add("data-item");
              dataItemInfo.innerHTML = `<p>${dataItemName}: ${dataItemValue}</p>`;
              deviceInfo.appendChild(dataItemInfo);
            }
          }
        });
    }
  </script>
</head>
<body>
  <h1>NIST Dataset Viewer</h1>
  
  <button onclick="showCurrentData()">Show Current Data</button>
  <button onclick="showSampleData()">Show Sample Data</button>
  
  <div id="data-container"></div>
</body>
</html>
