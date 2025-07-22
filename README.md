# Splunk Reports and Dashboards: A Practical Guide

A comprehensive, step-by-step guide to creating reports and dashboards in Splunk for security monitoring. This project walks through the process of analyzing failed login data, from initial search to building a multi-panel, interactive dashboard.

[View PDF](Splunk reports and dashboards report.pdf)

---

## Why Reports and Dashboards Matter in Splunk

Reports and dashboards are vital tools for transforming complex machine-generated data into clear, actionable insights. They provide a structured and visual way to monitor system activities, track security events, and make data-driven decisions.

-   **Accessibility:** They allow non-technical stakeholders (managers, auditors) to easily interpret data without writing queries.
-   **Clarity:** Users can see trends, patterns, and anomalies through intuitive charts, tables, and visual panels instead of raw logs.
-   **Automation:** Reports can be scheduled and emailed, ensuring the right information is delivered regularly without manual effort.
-   **Real-Time Monitoring:** Dashboards enable teams to react quickly to issues like failed login attempts, network spikes, or application errors.

## Key Concepts

### Reports vs. Dashboards

| Aspect | Reports | Dashboards |
| :--- | :--- | :--- |
| **Definition** | A saved search that can be rerun or scheduled. | A collection of one or more visual panels (charts, tables, etc.). |
| **Purpose** | To run and share specific, often heavy, searches at set intervals. | To display multiple visualizations together for at-a-glance monitoring. |
| **Output** | Static results (tables/charts) that can be exported or emailed. | Interactive panels that update dynamically when the dashboard is loaded. |
| **Creation** | Run a search, then select **Save As > Report**. | Use the Dashboard Editor to add new panels or embed existing reports. |

### Data Collection & Indexing

Splunk ingests data from virtually any source (servers, devices, apps). The data is then parsed, timestamped, and stored in an **index**. Fields are automatically extracted for fast searching. This project queries a sample dataset (`tutorialdata.zip`) containing security events.

### Visualization Options

Splunk offers a wide range of visualization types:
-   **Time Series Charts:** Line, Area, Column charts for showing trends over time.
-   **Comparison Charts:** Bar and Pie charts for comparing categories.
-   **Tables & Statistics:** For displaying raw event data or aggregated stats.
-   **Gauges:** For visualizing a metric against a range.

### User Access and Permissions (RBAC)

Splunk uses **Role-Based Access Control (RBAC)** to manage what users can see and do. When creating reports and dashboards, permissions can be set to be:
-   **Private:** Visible only to the creator.
-   **App-Level:** Visible to all users with access to the specific Splunk app.
-   **Global:** Visible to users across all apps.
-   **Role-Specific:** You can assign **Read** (view) and **Write** (edit) permissions to specific roles.

---

## Practical Walkthrough: Failed Login Analysis

This section follows the step-by-step demonstration in the PDF to build a dashboard that tracks the top 10 IP addresses with the most failed login attempts.

### Step 1: Searching for Failed Logins

The process begins by using the Splunk Processing Language (SPL) to query the data. Since the internal `_audit` index may have limited data, the guide uses a richer sample dataset.

-   **Initial Search:** Find all events containing "Failed password".
    ```spl
    source="tutorialdata.zip:*" "Failed password"
    ```
-   **Refined SPL Query:** This command is extended to extract IP addresses, count the attempts, and display the top 10 results.
    ```spl
    source="tutorialdata.zip:*" "Failed password" | rex "from (?<ip>\\d{1,3}(?:\\.\\d{1,3}){3})" | stats count as attempts by ip | sort -attempts | head 10
    ```
    -   `rex`: Extracts the IP address using a regular expression.
    -   `stats`: Counts the number of failed attempts for each IP.
    -   `sort -attempts`: Sorts the results in descending order.
    -   `head 10`: Limits the output to the top 10 results.

### Step 2: Creating and Saving Reports

Once the query is finalized, it is saved as both a data report (table) and a visualization report (bar chart).

1.  **Create Data Report:** Run the query and click **Save As > Report**. Give it a title (e.g., `failed_login_report`) and save it as a "Statistics Table".
2.  **Create Visualization Report:** Run the query again, go to the **Visualization** tab, select a **Bar Chart**, and save it as a new report.
3.  **Set Permissions:** For the saved reports, navigate to **Edit > Edit Permissions** to control access. You can make it visible to "All apps" and assign Read/Write permissions to different roles (e.g., `Everyone` gets Read, `power` gets Write).

### Step 3: Building the Dashboard

A dashboard is created to display multiple visualizations in a single view.

1.  **Create New Dashboard:** From the home page, go to **Dashboards > Create New Dashboard**.
2.  **Configuration:**
    -   Provide a **Dashboard Title** (e.g., `Failed_login_dash`).
    -   Choose **Classic Dashboards**.
    -   Click **Create**.
3.  **Add Panels from Scratch:**
    -   In the empty dashboard, click **Add Panel > New**.
    -   Select a visualization (e.g., **Line Chart**).
    -   Paste the SPL query from Step 1 into the **Search String** box.
    -   Set the **Time Range** to "All time".
    -   Click **Add to Dashboard**.
    -   Repeat this process to add a **Bar Chart** using the same query.
4.  **Add Panel from Report:**
    -   Navigate to the previously saved `failed_login_report`.
    -   Click **Add to Dashboard**.
    -   Select the existing `Failed_login_dash` to add the report's data table as a new panel.

### Step 4: Arranging the Final Dashboard

The final step is to organize the panels for better readability. You can click and drag the panels to arrange them side-by-side or in any preferred layout. The final dashboard provides a comprehensive view with:
-   A line chart showing trends.
-   A bar chart for easy comparison of the top attackers.
-   A raw data table for detailed analysis.

This layout allows for quick identification of suspicious IPs and provides both high-level visual insights and detailed data for further investigation.

---

## Author

**Balaji Varaprasad**

## Connect with Me

-   [LinkedIn](https://www.linkedin.com/in/balaji-varaprasad?utm_source=share&utm_campaign=share_via&utm_content=profile&utm_medium=android_app)
