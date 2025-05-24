# Data-Modelling-Challenge-1
Challenge in Data Modeling: Handling Multiple Date Fields
# Handling Multiple Date Columns in Power BI Data Modeling

In real-world data models, it is common to encounter fact tables with **multiple date columns**—for instance, `order_date` and `ship_date`. Customers often want to analyze KPIs from **multiple date perspectives**, such as year-over-year trends for orders vs. shipments. This introduces complexity because Power BI allows only **one active relationship** between a date column and a date dimension table at a time.

This project demonstrates how to **effectively manage multiple date fields** in your data model using two industry-standard techniques:

---

## 🧩 Use Case

Our sales fact table contains two key date fields:
- `order_date`
- `ship_date`

To support flexible time-based reporting, we created a **calendar date dimension** (`Date`) that includes all calendar days, enabling continuous time-series visuals and filling gaps in the data.



![Screen_short1](https://github.com/user-attachments/assets/018c33af-33eb-4f8f-b44a-0ded61ef87c9)


---

## ❓ The Challenge

Power BI supports only **one active relationship** between a date dimension and the fact table. This makes it difficult to create visuals or measures that switch between `order_date` and `ship_date`.

---

## ✅ Solution Approaches

### 1️⃣ Role-Playing Dimensions

We create **separate date dimension tables** for each role:

- `Order_Date_Dim`
- `Ship_Date_Dim`

Each is linked to its respective date field in the fact table:
```plaintext
Sales[order_date] —> Order_Date_Dim[Date]
Sales[ship_date]  —> Ship_Date_Dim[Date]

These separate dimensions can then be **linked to the corresponding fields in the fact table**.  
In your visuals, you simply use the **relevant date dimension** depending on the analytical context.

---

### 2️⃣ Inactive Relationships with DAX

Power BI allows us to define **multiple relationships** between two tables, but **only one can be active at a time**.

To leverage the inactive relationship, we use the `USERELATIONSHIP()` function in DAX.
This explicitly tells the engine to use the **inactive relationship** for a specific calculation, giving you **full control** over which **date context** to apply in your analysis.

![Screen_short_2](https://github.com/user-attachments/assets/3c030a2a-d84d-4570-8d5f-632ff551bc13)

And the DAX which we can use for this is:

![Use_inactive_relation](https://github.com/user-attachments/assets/6faf392b-77f4-419b-a672-075da568f252)


