# Detailed Analytics Summary Table

![image](https://github.com/user-attachments/assets/64339a7b-9db7-45c4-b7ae-3e30f6bb5de1)

## Description and Overview

The **Detailed Analytics Summary Table** provides a comprehensive tabular view of strategic performance across all organizational categories. This table serves as the detailed data foundation underlying the dashboard visualizations, offering executives and analysts complete visibility into category-specific performance metrics, completion rates, and status assessments.

The table displays:
- **Strategic Categories** - All defined strategic focus areas with visual icons
- **Total Objectives** - Count of objectives per category for scope understanding
- **Average Performance** - Category-level performance percentages with color-coded badges
- **Sub-Objectives Count** - Number of completed sub-objectives per category
- **Completion Rate** - Visual progress bars showing task completion percentages
- **Status Assessment** - Overall category health with status badges and icons

This table transforms high-level dashboard metrics into actionable detailed data for strategic analysis and decision-making.

## Why is it Important

### Comprehensive Analysis Value
The Detailed Analytics Table is essential for thorough strategic management because it:

1. **Data Transparency**: Provides complete visibility into the calculations behind dashboard summaries
2. **Category Comparison**: Enables direct comparison of performance across strategic areas
3. **Drill-Down Analysis**: Offers detailed data for investigating performance patterns
4. **Audit Trail**: Creates verifiable record of performance assessments and calculations
5. **Planning Foundation**: Supplies detailed data needed for strategic planning and resource allocation

### Management Benefits
- **Performance Accountability**: Clear category-by-category performance tracking
- **Resource Allocation**: Data-driven insights for investment and staffing decisions
- **Trend Analysis**: Historical performance tracking capability for each strategic area
- **Status Monitoring**: Real-time health assessment across all strategic categories

## Calculation Methods and Formulas

### Category Performance Calculation

**Average Performance Formula:**
```
Category Average Performance = Σ(Individual Objective Performance) ÷ Objectives in Category

Where:
Individual Objective Performance = (Actual Measure ÷ Target Measure) × 100
(Capped at 100% maximum)
```

**Step-by-Step Process:**
1. **Group Objectives by Category**:
   ```
   For each strategic category (Growth, Operations, HR, Customer, Innovation, ESG):
   Collect all objectives belonging to that category
   ```

2. **Calculate Individual Performances**:
   ```
   For each objective in the category:
   Performance = (Actual Achievement ÷ Target Value) × 100
   If Performance > 100%, cap at 100%
   ```

3. **Calculate Category Average**:
   ```
   Category Performance = Sum of all objective performances ÷ Number of objectives
   Round to nearest whole number
   ```

**Example Calculation:**
```
Growth Category Analysis:
Objective 1: Revenue Growth
- Target: $50M, Actual: $58M
- Performance = (58 ÷ 50) × 100 = 116% → Capped at 100%

Objective 2: Market Share  
- Target: 25%, Actual: 23%
- Performance = (23 ÷ 25) × 100 = 92%

Objective 3: Head Count
- Target: 100 employees, Actual: 105
- Performance = (105 ÷ 100) × 100 = 105% → Capped at 100%

Growth Category Performance = (100% + 92% + 100%) ÷ 3 = 97%
```

### Sub-Objectives Completion Calculation

**Sub-Objectives Count Formula:**
```
Category Sub-Objectives Count = Number of completed sub-objectives matching predefined standards

Where:
Predefined Standards = 3 standard sub-objectives per strategic category
Completed = Sub-objectives implemented and matching standard definitions
```

**Calculation Process:**
1. **Define Standard Sub-Objectives per Category**:
   ```
   Growth: [Revenue growth, Market share, Head Count]
   Operations: [Productivity improvement, Efficiency, Availability]
   HR: [Employee engagement score, Retention, Job satisfaction]
   Customer: [Customer satisfaction, Net Promoter Score, Customer Retention Rate]
   Innovation: [Revenue from New Products, R&D Investment %, Innovation Pipeline]
   ESG: [Revenue from Sustainable Products, ESG Investment %, ESG index score]
   ```

2. **Count Completed Sub-Objectives**:
   ```
   For each category:
   Available Sub-Objectives = getAvailableSubObjectivesForCategory(categoryName)
   Completed Count = Available.filter(subObj => 
       category.subObjectives.some(actual => actual.name === subObj.name)
   ).length
   ```

**Example:**
```
Growth Category Sub-Objectives:
Available Standards: [Revenue growth, Market share, Head Count]
Actual Sub-Objectives: [Revenue growth, Head Count, Customer acquisition]

Matching Count:
- Revenue growth: ✓ (matches standard)
- Market share: ✗ (not implemented)  
- Head Count: ✓ (matches standard)
- Customer acquisition: ✗ (not in standard list)

Completed Sub-Objectives Count = 2
```

### Completion Rate Calculation

**Completion Rate Formula:**
```
Completion Rate = (Completed Sub-Objectives ÷ Total Possible Sub-Objectives) × 100

Where:
Total Possible = Number of objectives in category × 3 standard sub-objectives
Completed = Count of implemented standard sub-objectives
```

**Detailed Calculation:**
1. **Calculate Total Possible**:
   ```
   Total Possible = Category Objectives Count × 3
   ```

2. **Calculate Completion Percentage**:
   ```
   Completion Rate = (Completed Count ÷ Total Possible) × 100
   Round to nearest whole number
   ```

**Example:**
```
Operations Category Completion Rate:
- Operations Objectives: 4
- Total Possible Sub-Objectives: 4 × 3 = 12
- Completed Sub-Objectives: 9
- Completion Rate = (9 ÷ 12) × 100 = 75%
```

### Status Determination Logic

**Status Classification Algorithm:**
```
Category Status = f(Average Performance, Completion Rate)

Status Rules:
if (Performance ≥ 90% AND Completion ≥ 80%) → "Excellent"
else if (Performance ≥ 75% AND Completion ≥ 60%) → "Good"  
else if (Performance ≥ 60% AND Completion ≥ 40%) → "Fair"
else → "Poor"
```

**Status Icon Assignment:**
```
Excellent: fa fa-check-circle (green)
Good: fa fa-thumbs-up (blue)
Fair: fa fa-exclamation-triangle (orange)
Poor: fa fa-times-circle (red)
```

### Comprehensive Table Example

**TechCorp Q3 2024 Detailed Analytics:**

**Growth Category:**
```
Objectives: 4
Individual Performances: [100%, 92%, 100%, 85%]
Average Performance: (100+92+100+85) ÷ 4 = 94%
Total Possible Sub-Objectives: 4 × 3 = 12
Completed Sub-Objectives: 10
Completion Rate: (10 ÷ 12) × 100 = 83%
Status: Excellent (94% ≥ 90% AND 83% ≥ 80%)
```

**Operations Category:**
```
Objectives: 3  
Individual Performances: [88%, 95%, 78%]
Average Performance: (88+95+78) ÷ 3 = 87%
Total Possible Sub-Objectives: 3 × 3 = 9
Completed Sub-Objectives: 7
Completion Rate: (7 ÷ 9) × 100 = 78%
Status: Good (87% ≥ 75% AND 78% ≥ 60%)
```

**Human Capital Category:**
```
Objectives: 2
Individual Performances: [72%, 68%]  
Average Performance: (72+68) ÷ 2 = 70%
Total Possible Sub-Objectives: 2 × 3 = 6
Completed Sub-Objectives: 3
Completion Rate: (3 ÷ 6) × 100 = 50%
Status: Fair (70% ≥ 60% AND 50% ≥ 40%)
```

**Customer Category:**
```
Objectives: 3
Individual Performances: [45%, 52%, 58%]
Average Performance: (45+52+58) ÷ 3 = 52%
Total Possible Sub-Objectives: 3 × 3 = 9
Completed Sub-Objectives: 3
Completion Rate: (3 ÷ 9) × 100 = 33%
Status: Poor (52% < 60% AND 33% < 40%)
```

**Final Table Display:**
```
┌─────────────────┬───────────┬────────────────┬──────────────┬─────────────────┬─────────────┐
│ Category        │ Total Obj │ Avg Performance│ Sub-Obj      │ Completion Rate │ Status      │
├─────────────────┼───────────┼────────────────┼──────────────┼─────────────────┼─────────────┤
│ Growth          │     4     │     94%        │      10      │      83%        │ Excellent   │
│ Operations      │     3     │     87%        │       7      │      78%        │ Good        │
│ Human Capital   │     2     │     70%        │       3      │      50%        │ Fair        │
│ Customer        │     3     │     52%        │       3      │      33%        │ Poor        │
└─────────────────┴───────────┴────────────────┴──────────────┴─────────────────┴─────────────┘
```

## Screenshot Section

### Table Overview Screenshot
**Filename**: `analytics-table-overview.png`
**Content**: Complete table showing all strategic categories with full data
**Annotations**:
- Highlight column headers and data organization
- Show color-coded performance badges
- Point out completion rate progress bars
- Emphasize status badges with icons

### Performance Badge Variations
**Excellent Performance** (`analytics-table-excellent.png`):
- Green performance badges for high-achieving categories
- 90%+ performance indicators
- Check-circle status icons

**Poor Performance** (`analytics-table-poor.png`):
- Red performance badges for underperforming categories
- <60% performance indicators  
- Warning status icons

### Completion Rate Visualizations
**High Completion** (`analytics-table-high-completion.png`):
- Progress bars showing 80%+ completion
- Green completion indicators
- Full progress bar visualization

**Low Completion** (`analytics-table-low-completion.png`):
- Progress bars showing <40% completion
- Red completion indicators
- Partial progress bar visualization

### Category-Specific Views
**Growth Category Focus** (`analytics-table-growth-detail.png`):
- Detailed view of Growth category performance
- Specific objective breakdown
- Sub-objective completion analysis

**Operations Excellence Detail** (`analytics-table-operations-detail.png`):
- Operations category performance metrics
- Efficiency and productivity indicators
- Operational excellence tracking

### Status Classification Views
**Mixed Status Display** (`analytics-table-mixed-status.png`):
- Multiple status levels shown together
- Color-coded status differentiation
- Comprehensive status overview

### Interactive Elements
**Sortable Columns** (`analytics-table-sortable.png`):
- Column sorting functionality
- Performance ranking capabilities
- Data organization options

**Row Hover Effects** (`analytics-table-hover.png`):
- Row highlighting on mouse-over
- Enhanced readability features
- Interactive table navigation

### Responsive Design
**Desktop Table** (`analytics-table-desktop.png`):
- Full table layout with all columns visible
- Optimal spacing and readability
- Complete data presentation

**Mobile Table** (`analytics-table-mobile.png`):
- Responsive table design for mobile devices
- Horizontal scrolling capability
- Essential information prioritized

---

# What Does "Avg. Performance" Mean?

**Avg. Performance** (Average Performance) is a metric that shows, in percentage terms, how much progress has been made toward achieving the strategic objectives in each category.

## How Is It Calculated?
- For each strategic objective, we look at its sub-objectives and their weights (importance) and completion levels.
- We calculate a weighted score for each objective, reflecting both how much of each sub-objective is done and how important it is.
- We cap each objective's performance at 100% (so you can't get more than fully complete).
- We then average these percentages for all objectives in the category.

## How Do We Measure Completion?
- **Sub-objective completion** is measured by its `amount` value (as a percentage, e.g., 80 means 80% complete).
- Each sub-objective also has a `measure` (weight), showing how important it is compared to others under the same objective.
- For an objective, we multiply each sub-objective's completion (`amount/100`) by its weight (`measure/100`), sum these up, and cap the result at 100%.
- An objective is considered fully complete if all its sub-objectives are 100% complete and their weights sum to 100%.
- **Category completion rate** is calculated by counting all sub-objectives that are 100% complete, divided by the total possible sub-objectives in that category.

## What Does It Tell You?
- **100% Avg. Performance**: All objectives in the category are fully achieved.
- **50% Avg. Performance**: On average, objectives in the category are halfway to completion.
- **0% Avg. Performance**: No progress has been made on any objectives in the category.

## Why Is This Useful?
- It gives a quick, fair, and comparable view of progress across different categories, regardless of how many objectives or sub-objectives each has.
- It helps you spot which strategic areas are on track and which need attention.

## Example
If a category has two objectives:
- Objective 1: 100% complete
- Objective 2: 50% complete

**Avg. Performance = (100% + 50%) / 2 = 75%**

This means, on average, the objectives in this category are 75% complete.

## Detailed Example Analysis (with 2 Projects)

Let's say we have a strategic category called **"Growth"**. There are 2 projects, each with a strategic objective under this category. Each objective has sub-objectives with different weights and completion levels.

### Project 1: "Plant Upgrade"
- **Strategic Objective:** Growth
  - **Sub-Objectives:**
    - Revenue growth: measure = 60, amount = 100 (fully complete)
    - Market share: measure = 10, amount = 100 (fully complete)
    - Head Count: measure = 30, amount = 100 (fully complete)
  - **Calculation:**
    - Total weight = 60 + 10 + 30 = 100
    - Weighted completion = (60/100 * 100/100) + (10/100 * 100/100) + (30/100 * 100/100) = 0.6 + 0.1 + 0.3 = 1.0 (100%)
    - Capped at 100%.

### Project 2: "Mobile Marketing"
- **Strategic Objective:** Growth
  - **Sub-Objectives:**
    - Revenue growth: measure = 100, amount = 50 (half complete)
  - **Calculation:**
    - Total weight = 100
    - Weighted completion = (100/100 * 50/100) = 0.5 (50%)
    - Capped at 50%.

### Category Calculation
- **Objective 1 (Plant Upgrade):** 100%
- **Objective 2 (Mobile Marketing):** 50%
- **Avg. Performance for "Growth" = (100% + 50%) / 2 = 75%**

**Interpretation:**
- The "Growth" category is, on average, 75% complete across its objectives.
- This reflects both the number of objectives and their actual progress, weighted by the importance of each sub-objective.

---

**In summary:**
Avg. Performance is a clear, percentage-based indicator of how much progress has been made toward the strategic goals in each category, making it easy for you to monitor and compare performance at a glance.


*This table provides the detailed analytical foundation for strategic performance management and decision-making across all organizational categories.*
