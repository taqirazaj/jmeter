

# 📊 JMeter Load Testing 

## 🚀 Overview

This repository contains a **JMeter test plan (`.jmx`)** to load test the **Publish APIs**.
It is designed to simulate realistic user behavior with:

* ✅ A **Login Thread Group** that refreshes tokens every 15 minutes
* ✅ A **Business Thread Group** that executes API requests under load
* ✅ Randomized data (modifiers) for more realistic test coverage
* ✅ Configured headers & tokens passed automatically
* ✅ Ramp-up and timers for realistic load simulation

---

## 🛠 Prerequisites

Before running, ensure you have:

* [Apache JMeter 5.6.3+](https://jmeter.apache.org/download_jmeter.cgi) installed
* Java 11 or later installed (`java -version`)
* (Optional) [JMeter Plugins Manager](https://jmeter-plugins.org/wiki/PluginsManager/)

---

## 📂 Repository Structure

```
.
├── jmeter-tests/
│   └── Publish.jmx       # Main JMeter test plan
├── results/              # Results (.jtl, .csv) will be saved here
├── README.md             # Documentation
```

---

## ▶️ How to Run Tests

### Run Locally

1. Open JMeter GUI:

   ```bash
   jmeter
   ```

   * Load `jmeter-tests/Publish.jmx`
   * Run from GUI (green ▶️ button) to debug/test

2. Run in **non-GUI mode** (recommended for load):

   ```bash
   jmeter -n -t jmeter-tests/Publish.jmx -l results/results_publish.jtl -j results/jmeter.log
   ```

---

## 📌 Test Plan Details

### 1. **Login Thread Group**

* **1 virtual user**
* Loops **forever** but executes once every **15 minutes**
* Extracts `AccessToken` and `IdToken` from login API response
* Saves tokens into JMeter **properties** (`ACCESS_TOKEN`, `ID_TOKEN`)

### 2. **Business Thread Group**

* **200 users**
* **Ramp-up = 600 seconds** (users added gradually)
* APIs covered:

  * `PUT /api/modifier/update`
  * `PUT /api/restaurant-publish/update-status`
* Random `modifierName` used for each request
* Headers automatically populated with tokens

### 3. **Timers**

* **Uniform Random Timer (0–5000ms)** simulates user think time between requests

### 4. **Reports**

* `Summary Report` → performance overview
* `View Results Tree` → detailed request/response (debugging only)

---

## 🧰 JMeter Features Used

This project uses a combination of JMeter core and advanced features:

* **Thread Groups**

  * Login Thread Group (token management)
  * Business Thread Group (API load)

* **Timers**

  * Uniform Random Timer (to simulate think time)

* **Controllers**

  * Loop Controller (infinite login loop every 15 minutes)

* **Config Elements**

  * HTTP Header Manager (injecting `Authorization` header with token)
  * CSV Data Set Config (random modifier names for API requests)

* **Post-Processors**

  * JSON Extractor (extract `accessToken` and `idToken` from login response)

* **Scripting**

  * JSR223 PostProcessor (Groovy script to store tokens into JMeter properties)

* **Listeners**

  * Summary Report
  * View Results Tree

---

## 📈 Reporting

### HTML Report

Generate a human-readable HTML report:

```bash
jmeter -g results/results_publish.jtl -o results/html-report
```

Open `results/html-report/index.html` in your browser.

---

