🛠️ Lathe Machine Monitoring using Node-RED, MongoDB, Email PDF & Grafana

This project monitors Lathe Machine sensor parameters (RPM, Feed, Temperature etc.) using Node-RED, stores data into MongoDB, automatically deletes data older than 5 days, sends Auto PDF Report every 24 Hours via Email, and visualizes live data in Grafana Dashboard.

✨ Features

✔ Collect Machine Data using Node-RED
✔ Store data into MongoDB Database
✔ Automatically delete records older than 5 days
✔ Automatically generate PDF Report every 24 Hours
✔ Send PDF to Gmail automatically
✔ Provide REST API /latheData to fetch last 24 hrs data
✔ Connect MongoDB to Grafana
✔ Display Data in Grafana Graph Dashboard

🧰 Tech Stack

Node-RED

MongoDB

Grafana

PDFMake Node

Gmail SMTP

JSON API

JavaScript

✅ 1️⃣ MongoDB Setup
🔹 Install MongoDB

Download & Install
https://www.mongodb.com/try/download/community

🔹 Start MongoDB Service

Ensure MongoDB running:

mongodb://127.0.0.1:27017

🔹 Create Database & Collection

Database: SensorData
Collection: machineData

✅ 2️⃣ Node-RED Setup
🔹 Install Node-RED
npm install -g node-red

🔹 Start Node-RED
node-red


Open in browser
👉 http://localhost:1880

🔗 Required Node-RED Nodes

Install these:

node-red-node-email
node-red-node-mongodb
node-red-contrib-pdfmake2

📥 Import Project Flow

1️⃣ Open Node-RED
2️⃣ Click Menu → Import
3️⃣ Paste your exported JSON Flow
4️⃣ Deploy ✔

🧾 Data Insert to MongoDB (Manual / Live)

Function Node code:

msg.payload = {
    rpm: 1000,
    feed: 20,
    temperature: 20,
    time: new Date()
};
return msg;


This data goes into MongoDB:

SensorData → machineData

🗑️ Auto Delete Old Data (5 Days)

MongoDB TTL Index Feature use kiya.

Open MongoDB Shell / Compass Run:
db.machineData.createIndex(
  { "time": 1 },
  { expireAfterSeconds: 432000 }
)


432000 seconds = 5 days

🔹 Now MongoDB will automatically delete old data
No cron job needed
No Node-RED logic needed ✔

📧 Auto Email PDF Every 24 Hours
🔹 Daily Trigger Inject Node

Set:

Repeat → at specific time
24 hours interval

🔹 Query Last 24 Hours Data

Function Node

let now = new Date();
let last24 = new Date(now.getTime() - 24*60*60*1000);

msg.payload = {
    time: { $gte: last24 }
};
return msg;

🔹 MongoDB Node

Operation: find
Collection: machineData

🧾 Generate PDF Report

Function Node:

Makes table

Arranges data

Prepare pdf structure

PDFMake node converts into PDF.

📤 Send Email with PDF

Email Node:

Server: smtp.gmail.com
Port: 465
Use Secure: True
Username: your gmail
Password: App Password


⚠️ Use Gmail App Password (not real password)

🌐 REST API for Last 24 Hours Data

GET:

http://localhost:1880/latheData


Returns Mongo Data JSON
Useful for Apps / Web UI

📊 Grafana Setup
1️⃣ Install Grafana

Download:
https://grafana.com/grafana/download

Run Grafana & Open:

http://localhost:3000

🔗 Connect MongoDB to Grafana

Grafana does not support MongoDB directly,
So we used:

Infinity datasource plugin

Steps:

1️⃣ Go to Grafana
2️⃣ Connections → Add Data Source
3️⃣ Select → Infinity Plugin
4️⃣ Mode: URL
5️⃣ URL:

http://localhost:1880/latheData


6️⃣ Format → JSON
7️⃣ Parser → JSONata
8️⃣ Save ✔

📉 Create Dashboard Panel

1️⃣ Create New Dashboard
2️⃣ Add Panel
3️⃣ Select Infinity datasource
4️⃣ Select Time field
5️⃣ Convert fields:

time → Time

rpm → Number

temperature → Number

feed → Number

🎯 Final Output You Get

✔ Data storing in Database
✔ Old data auto deleted (5 Days)
✔ Auto Email PDF Every Day
✔ Live Graph Dashboard
✔ REST API Ready
✔ Easily Scalable Project

👨‍💻 Developer

Krishna Shrangare
Full Stack / IoT Developer
