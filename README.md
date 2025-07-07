# Kaya Hotel Revenue Analysis (Power BI)

A comprehensive **Power BI** project that analyzes the revenue performance of Kaya Hotel. The report provides actionable insights across three key dashboards: Executive Overview, Diagnostics, and Operations, helping management make data-driven decisions.

## 📊 Dashboard Overview

### 1. Executive Dashboard
![Executive Dashboard](kaya_exec_page.PNG)

The Executive Dashboard provides a high-level overview of key performance indicators including total revenue, occupancy rates, ADR, and RevPAR. It's designed for quick decision-making with month-over-month comparisons and trend analysis.

### 2. Diagnostics Dashboard
![Diagnostics Dashboard](kaya_diagnostics_page.PNG)

The Diagnostics Dashboard dives deeper into performance metrics, highlighting booking patterns, cancellation rates, and market segment performance to identify areas for improvement.

### 3. Operations Dashboard
![Operations Dashboard](kaya_operations_page.PNG)

The Operations Dashboard focuses on day-to-day metrics, room inventory management, and staff performance indicators to ensure smooth hotel operations.

---

## 📂 Project Structure

| Path | Description |
|------|-------------|
| `kaya_hotel_Page1.pbix` | Main Power BI file containing all dashboards & measures |
| `kaya_exec_page.PNG` | Executive dashboard overview |
| `kaya_diagnostics_page.PNG` | Diagnostics dashboard |
| `kaya_operations_page.PNG` | Operations dashboard |
| `kaya_model.PNG` | Data model schema |
| `kaya hotel revenue analysis data.rar` | Compressed raw dataset (CSV / Excel inside) |

## ✨ Key Insights & KPIs

| KPI | Description |
|-----|-------------|
| **Total Revenue** | Overall hotel revenue during selected period |
| **Average Daily Rate (ADR)** | Avg price paid per occupied room |
| **RevPAR** | Revenue per available room |
| **Occupancy %** | Rooms sold vs rooms available |
| **Cancellation Rate** | Percentage of canceled bookings |
| **Booking Lead Time** | Average days between booking and check-in |

Additional insights include:

* Market segment performance (Corporate, OTA, Direct, etc.)  
* Room type performance and availability  
* Seasonal trends and booking patterns  
* Revenue forecasting and goal tracking

![Data Model](kaya_model.PNG)

---

## 🛠️ How to Use

1. **Prerequisite:** Install [Power BI Desktop](https://powerbi.microsoft.com/desktop/).
2. Clone the repository and open `kaya_hotel_Page1.pbix` in Power BI Desktop.
3. If prompted for credentials, choose *"Load without applying query changes"* (data is embedded).
4. Interact with slicers (Date, Market Segment, Room Type) to explore the data.
5. Modify or extend the model / visuals as needed.

---

## ⚙️ Data Model

The model consists of **fact** tables for reservations & revenue linked to **dimension** tables such as Date, Market Segment, and Room Type.  All relationships are *single-directional* to maintain star-schema integrity.

---

## 🚀 Getting Started Locally

```bash
# clone
> git clone https://github.com/<your-username>/kaya-hotel-revenue-analysis-PowerBI.git
> cd kaya-hotel-revenue-analysis-PowerBI

# open the PBIX file
> start kaya_hotel_Page1.pbix  # windows
```

---

## 🤝 Contributing

Pull requests are welcome!  For major changes, please open an issue first to discuss what you would like to change.

---

## 📄 License

This project is licensed under the MIT License – see the [LICENSE](LICENSE) file for details.

---

> © 2025 Kaya Hotel Analytics Team – Crafted with 💛 & Power BI
