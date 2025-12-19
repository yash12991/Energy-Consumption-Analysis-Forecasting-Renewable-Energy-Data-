# Power BI Dashboard Creation Guide
## Renewable Energy Consumption Analysis

---

## 📋 Prerequisites
- **Power BI Desktop** (Download from: https://powerbi.microsoft.com/desktop/)
- **CSV Files** exported from Python analysis:
  - `cleaned_energy_data.csv`
  - `daily_energy_summary.csv`
  - `hourly_pattern.csv`
  - `monthly_energy_summary.csv`
  - `kpi_summary.csv`

---

## 🚀 Step-by-Step Dashboard Creation

### **STEP 1: Import Data into Power BI**

1. **Open Power BI Desktop**
2. Click **"Get Data"** → **"Text/CSV"**
3. **Import all 5 CSV files** one by one:
   - Navigate to your project folder
   - Select each CSV file
   - Click **"Load"** (not "Transform Data" yet)

4. **Verify Data Import:**
   - Check left panel under "Data" view
   - All 5 tables should appear
   - Click on each table to preview data

**Interview Tip:** "I imported multiple data sources into Power BI and established relationships between tables for comprehensive analysis."

---

### **STEP 2: Data Modeling & Relationships**

1. Click on **"Model"** view (icon on left sidebar)
2. Power BI may auto-detect relationships, but verify:
   - No explicit relationships needed for this project (each table is independent)
   - Tables are pre-aggregated for specific visualizations

3. **Create a Date Table** (Optional but Professional):
   ```DAX
   DateTable = 
   CALENDAR(
       MIN(cleaned_energy_data[time]),
       MAX(cleaned_energy_data[time])
   )
   ```
   - Go to "Table tools" → "New table" → Paste above DAX
   - Add columns: Year, Month, Day using DAX

**Interview Tip:** "I created a proper date dimension table for time-intelligence functions and better time-based filtering."

---

### **STEP 3: Create KPI Cards**

**Dashboard Layout:** Top row with 5 KPI cards

#### **3.1 Total Load KPI Card**
1. Click **"Card"** visualization from right panel
2. Drag `Total Load (MW)` from `kpi_summary` table
3. **Format the card:**
   - Click "Format" (paint roller icon)
   - **Category label:** OFF
   - **Title:** ON → Text: "Total Load"
   - **Background:** Light blue
   - **Font size:** 24 for value, 12 for title
   - **Data label → Display units:** Millions (M)

#### **3.2 Peak Load KPI Card**
- Repeat above steps with `Peak Load (MW)`
- Title: "Peak Load"
- Background: Orange

#### **3.3 Average Load KPI Card**
- Use `Average Load (MW)`
- Title: "Average Load"
- Background: Green

#### **3.4 Load Factor KPI Card**
- Use `Load Factor (%)`
- Title: "Load Factor"
- Background: Purple
- Add "%" suffix

#### **3.5 Renewable Energy % KPI Card**
- Use `Renewable Energy (%)`
- Title: "Renewable Energy"
- Background: Dark green
- Add "%" suffix

**Interview Tip:** "I created KPI cards displaying critical metrics that stakeholders can view at a glance for quick decision-making."

---

### **STEP 4: Create Time-Based Filters (Slicers)**

#### **4.1 Date Range Slicer**
1. Click **"Slicer"** visualization
2. Drag `Date` field from `daily_energy_summary`
3. **Format:**
   - Slicer settings → Style: **"Between"** (for range selection)
   - Position: Top right corner
   - Width: 250px

#### **4.2 Year Slicer**
1. Add another **Slicer**
2. Drag `Year` from `monthly_energy_summary`
3. **Format:**
   - Style: **"Dropdown"** or **"List"**
   - Enable multi-select
   - Position: Below date slicer

#### **4.3 Month Slicer**
1. Add another **Slicer**
2. Drag `Month` from `monthly_energy_summary`
3. Format as dropdown with multi-select

**Interview Tip:** "I implemented interactive slicers allowing users to filter data by specific time periods for focused analysis."

---

### **STEP 5: Create Visualizations**

#### **5.1 Energy Consumption Over Time (Line Chart)**
1. Click **"Line Chart"** visualization
2. **Fields:**
   - **X-axis:** `Date` (from `daily_energy_summary`)
   - **Y-axis:** `Avg_Load`
   - **Legend:** None
3. **Format:**
   - Title: "Daily Energy Consumption Trend"
   - Line color: Dark blue
   - Data labels: OFF
   - Gridlines: Horizontal only
   - Enable zoom slider
4. **Size:** Full width, below KPI cards

**What it shows:** Time-series trend of energy consumption, seasonal patterns

#### **5.2 Hourly Consumption Pattern (Column Chart)**
1. Click **"Clustered Column Chart"**
2. **Fields:**
   - **X-axis:** `Hour` (from `hourly_pattern`)
   - **Y-axis:** `Avg_Load`
   - **Legend:** None
3. **Format:**
   - Title: "Average Load by Hour of Day"
   - Color: Gradient (conditional formatting)
   - Data labels: ON
4. **Conditional Formatting:**
   - Right-click Y-axis field → Conditional formatting → Background color
   - Rule: Higher values = Darker color

**What it shows:** Peak consumption hours for grid management

#### **5.3 Day of Week Analysis (Bar Chart)**
1. Click **"Bar Chart"**
2. **Fields:**
   - **Y-axis:** `Day` (from `hourly_pattern`)
   - **X-axis:** `Avg_Load` (Average)
3. **Format:**
   - Title: "Consumption by Day of Week"
   - Sort: Monday to Sunday
   - Color weekends differently (use conditional formatting)

**What it shows:** Weekday vs weekend consumption patterns

#### **5.4 Monthly Trend (Area Chart)**
1. Click **"Area Chart"**
2. **Fields:**
   - **X-axis:** `Month` (from `monthly_energy_summary`)
   - **Y-axis:** `Avg_Load`
   - **Legend:** `Year`
3. **Format:**
   - Title: "Monthly Consumption Comparison"
   - Enable legend
   - Different colors for each year

**What it shows:** Seasonal trends and year-over-year comparison

#### **5.5 Renewable vs Non-Renewable (Donut Chart)**
1. Click **"Donut Chart"**
2. **Fields:**
   - **Legend:** Create a calculated column for categories
   - **Values:** Sum of renewable and non-renewable generation
3. **Format:**
   - Title: "Energy Mix: Renewable vs Non-Renewable"
   - Colors: Green for renewable, Red for non-renewable
   - Show percentages

**DAX Measure for Total Renewable:**
```DAX
Total_Renewable_All = SUM(daily_energy_summary[Renewable_Gen])
```

**DAX Measure for Total Non-Renewable:**
```DAX
Total_NonRenewable_All = SUM(daily_energy_summary[NonRenewable_Gen])
```

#### **5.6 Heatmap (Matrix Visual)**
1. Click **"Matrix"** visualization
2. **Fields:**
   - **Rows:** `Hour`
   - **Columns:** `Day`
   - **Values:** `Avg_Load`
3. **Format:**
   - Title: "Consumption Heatmap: Hour x Day"
   - Conditional formatting → Background color → Color scale
   - Values: Hide or reduce font size

**What it shows:** Detailed hourly patterns across the week

#### **5.7 Top Renewable Sources (Stacked Bar Chart)**
1. Use `cleaned_energy_data` table
2. Create measures for each renewable source:
```DAX
Total_Solar = SUM(cleaned_energy_data[generation solar])
Total_Wind_Onshore = SUM(cleaned_energy_data[generation wind onshore])
Total_Wind_Offshore = SUM(cleaned_energy_data[generation wind offshore])
Total_Hydro = SUM(cleaned_energy_data[generation hydro run-of-river and poundage])
```
3. Click **"Stacked Bar Chart"**
4. Add all measures as values
5. Format with distinct colors

**What it shows:** Contribution of different renewable sources

---

### **STEP 6: Dashboard Layout & Design**

#### **Recommended Layout:**

```
┌─────────────────────────────────────────────────────────────┐
│  Title: "Renewable Energy Consumption Dashboard"           │
├──────────┬──────────┬──────────┬──────────┬────────────────┤
│ Total    │ Peak     │ Average  │ Load     │ Renewable %   │
│ Load     │ Load     │ Load     │ Factor   │               │
├──────────┴──────────┴──────────┴──────────┴────────────────┤
│                                           ┌─────────────────┤
│   Daily Energy Consumption Trend          │  Date Filter    │
│   (Line Chart - Full Width)               │  Year Filter    │
│                                           │  Month Filter   │
├───────────────────┬───────────────────────┴─────────────────┤
│  Hourly Pattern   │      Monthly Trend (Area Chart)         │
│  (Column Chart)   │                                         │
├───────────────────┼─────────────────────────────────────────┤
│  Day of Week      │   Renewable vs Non-Renewable            │
│  (Bar Chart)      │   (Donut Chart)                         │
├───────────────────┴─────────────────────────────────────────┤
│         Heatmap: Hour x Day of Week (Matrix)                │
├─────────────────────────────────────────────────────────────┤
│    Top Renewable Energy Sources (Stacked Bar)               │
└─────────────────────────────────────────────────────────────┘
```

#### **Design Best Practices:**
1. **Consistent Colors:**
   - Use color theme: Blue (loads), Green (renewable), Red (non-renewable)
   
2. **White Space:**
   - Proper spacing between visuals
   - Not cluttered
   
3. **Font Consistency:**
   - Title: 16px, Bold
   - Axis labels: 11px
   - Values: 12px
   
4. **Background:**
   - Light gray background (#F5F5F5)
   - White visual backgrounds
   
5. **Borders:**
   - Subtle borders on visuals (1px, light gray)

---

### **STEP 7: Add Interactivity**

1. **Cross-Filtering:**
   - Click any visual → Format → Edit interactions
   - Enable cross-highlighting between related visuals
   - Disable where not relevant

2. **Drill-Through:**
   - Create a detail page
   - Add drill-through fields (Date, Hour)
   - Users can right-click a data point → Drill through

3. **Tooltips:**
   - Custom tooltips showing additional metrics
   - Format → Tooltip → Report page tooltip

**Interview Tip:** "I enabled cross-filtering and drill-through capabilities for interactive data exploration."

---

### **STEP 8: Create Additional Measures (DAX)**

Add these calculated measures for enhanced analysis:

```DAX
// Average Daily Load
Avg_Daily_Load = AVERAGE(daily_energy_summary[Avg_Load])

// Peak Hour Badge
Peak_Hour_Badge = 
VAR PeakHr = CALCULATE(MAX(hourly_pattern[Hour]), 
    TOPN(1, hourly_pattern, hourly_pattern[Avg_Load], DESC))
RETURN PeakHr & ":00"

// Year-over-Year Growth
YoY_Growth = 
VAR CurrentYear = SUM(monthly_energy_summary[Total_Load])
VAR PreviousYear = CALCULATE(
    SUM(monthly_energy_summary[Total_Load]),
    DATEADD(monthly_energy_summary[Year], -1, YEAR)
)
RETURN 
DIVIDE(CurrentYear - PreviousYear, PreviousYear, 0) * 100

// Renewable Percentage Dynamic
Renewable_Pct = 
DIVIDE(
    SUM(daily_energy_summary[Renewable_Gen]),
    SUM(daily_energy_summary[Renewable_Gen]) + SUM(daily_energy_summary[NonRenewable_Gen]),
    0
) * 100
```

---

### **STEP 9: Publish to Power BI Service (Optional)**

1. Click **"Publish"** in Home tab
2. Sign in to Power BI account
3. Select workspace
4. Share dashboard link with stakeholders

**Interview Tip:** "I published the dashboard to Power BI Service for organization-wide access and scheduled data refresh."

---

## 📊 What Each Visual Answers

| Visual | Business Question |
|--------|------------------|
| **KPI Cards** | What are our key performance metrics? |
| **Line Chart (Daily Trend)** | How is consumption changing over time? |
| **Column Chart (Hourly)** | When do we experience peak demand? |
| **Bar Chart (Day of Week)** | Are there weekly patterns? |
| **Area Chart (Monthly)** | What are seasonal trends? |
| **Donut Chart** | What's our renewable energy mix? |
| **Heatmap** | What are detailed hourly patterns? |
| **Stacked Bar** | Which renewable sources contribute most? |

---

## 🎯 Interview Talking Points

**When describing this project:**

1. **Data Integration:**
   - "I integrated multiple CSV data sources into Power BI, each serving a specific analytical purpose"

2. **Data Modeling:**
   - "I designed a star schema with fact and dimension tables for optimized query performance"

3. **DAX Knowledge:**
   - "I created custom DAX measures for dynamic KPI calculations and time-intelligence functions"

4. **Visualization Selection:**
   - "I chose specific visualizations based on data types and business questions being answered"

5. **User Experience:**
   - "I implemented interactive filters and cross-highlighting for intuitive data exploration"

6. **Business Value:**
   - "This dashboard helps stakeholders make data-driven decisions on grid management, capacity planning, and renewable energy investment"

---

## 📝 Common Dashboard Features Checklist

- ✅ KPI cards for key metrics
- ✅ Time-based slicers (Date, Year, Month)
- ✅ Trend analysis (Line/Area charts)
- ✅ Pattern identification (Bar/Column charts)
- ✅ Composition analysis (Pie/Donut charts)
- ✅ Detailed view (Heatmap/Matrix)
- ✅ Consistent color scheme
- ✅ Clear titles and labels
- ✅ Interactive filtering
- ✅ Mobile-responsive layout (optional)

---

## 🔧 Troubleshooting Tips

**Issue: Slicers not filtering all visuals**
- **Solution:** Check Edit Interactions settings

**Issue: Date not sorting correctly**
- **Solution:** Ensure date field is in Date/Time format

**Issue: Visuals load slowly**
- **Solution:** Use aggregated tables instead of raw data

**Issue: Colors not consistent**
- **Solution:** Use Format Painter or custom color theme

---

## 📚 Additional Resources

- **Power BI Documentation:** https://docs.microsoft.com/power-bi/
- **DAX Guide:** https://dax.guide/
- **Community Forums:** https://community.powerbi.com/

---

**Dashboard File Naming:** `Renewable_Energy_Dashboard.pbix`

**Save your work regularly and create a copy before major changes!**
