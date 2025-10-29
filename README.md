# 🧪 Postman CI/CD Test Automation with GitHub Actions

This repository runs **Postman API test collections** automatically through **GitHub Actions** using **Newman** (the Postman CLI).  
Each test execution generates a **Newman HTML report**, which can be uploaded as an artifact and/or published to the `gh-pages` branch for use in external dashboards — such as the **Fidelanders Postman Dashboard**.

---

## ⚙️ CI/CD Workflow Overview

The GitHub Actions workflow is triggered on every:

- Push to `main` or `develop`
- Pull request targeting `main` or `develop`

### 🧩 Workflow Steps

1. **Checkout Repository**
   - Pulls your code from the current branch.

2. **Setup Node.js**
   - Uses Node.js v18 (recommended for Newman).

3. **Install Newman**
   - Installs Newman globally using NPM.

4. **Run Postman Collection**
   - Executes your Postman collection (`MyCollection.postman_collection.json`)  
     with the defined environment (`MyEnvironment.postman_environment.json`).
   - Generates both **CLI output** and an **HTML report** (`newman-report.html`).

5. **Upload HTML Report**
   - Uploads the report as a downloadable artifact in the GitHub Actions tab.

6. **Commit JSON Report to gh-pages**
   - Switches to the `gh-pages` branch.
   - Commits the latest Newman JSON report (`latest-report.json`).
   - Pushes it to GitHub Pages — making it accessible to your **Postman Dashboard App**.

---

## 📁 Project Structure

```

tests/
└── postman/
├── MyCollection.postman_collection.json
└── MyEnvironment.postman_environment.json

.github/
└── workflows/
└── postman-ci.yml  # The workflow file

newman-report.html      # Generated HTML report

````

---

## 🌐 Integrating with a Static Dashboard

If your dashboard (e.g., **Postman Report Dashboard App**) is a **static web app** (no backend),  
you can fetch the latest report directly from the `gh-pages` branch using a public JSON URL.

### Example Fetch Script
```js
// dashboard.js
async function fetchReport() {
  const response = await fetch(
    'https://<your-username>.github.io/<your-repo-name>/latest-report.json'
  );
  const data = await response.json();
  renderReport(data);
}

function renderReport(data) {
  document.getElementById('summary').innerText =
    `Total Tests: ${data.run.stats.tests.total}, Passed: ${data.run.stats.tests.total - data.run.stats.tests.failed}`;
}

fetchReport();
````

---

## 🚀 How to Run Locally

1. Install dependencies:

   ```bash
   npm install -g newman
   ```
2. Run tests:

   ```bash
   newman run tests/postman/MyCollection.postman_collection.json \
     -e tests/postman/MyEnvironment.postman_environment.json \
     --reporters cli,html,json \
     --reporter-html-export newman-report.html \
     --reporter-json-export newman-report.json
   ```
3. Open `newman-report.html` in your browser.

---

## 🧰 Tech Stack

* **Postman** – API testing platform
* **Newman** – CLI runner for Postman collections
* **GitHub Actions** – CI/CD automation
* **GitHub Pages** – Host and serve static reports
* **JavaScript (Frontend)** – Static dashboard data visualization

---

## 🧑‍💻 Author

**Fidelis Ogbeni**
Quality Assurance Engineer | Automation | Performance | DevOps
🌐 [GitHub](https://github.com/fidelanders)
📧 [fidelanders@gmail.com](mailto:fidelanders@gmail.com)

---

## 🛡️ License

This project is open source and available under the [MIT License](LICENSE).

```