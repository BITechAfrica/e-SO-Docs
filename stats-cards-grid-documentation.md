# Strategic Performance Stats Cards Grid

## Description and Overview

The **Stats Cards Grid** is a critical component of the Strategic Analytics Dashboard that provides an at-a-glance overview of key performance indicators across the organization's strategic initiatives. This component displays four essential metrics in an easily digestible card-based layout, offering immediate insights into the health and progress of strategic projects, objectives, and overall performance.

The component consists of four primary cards:
1. **Strategic Projects** - Total and active project counts
2. **Strategic Objectives** - Total objectives and achievement status
3. **Sub-Objectives** - Comprehensive task tracking and completion rates
4. **Overall Performance** - Aggregated performance percentage with status indicators

## Why Is It Important?

### Executive Decision Making
The stats cards grid serves as the **primary dashboard entry point** for executives and stakeholders, providing immediate visibility into organizational performance without requiring deep analysis of complex charts or detailed reports.

### Performance Monitoring
- **Real-time Insights**: Instantly identify areas requiring attention
- **Progress Tracking**: Monitor advancement across all strategic dimensions
- **Resource Allocation**: Make informed decisions about where to focus efforts and resources
- **Accountability**: Maintain transparency in strategic execution

### Strategic Alignment
- **Holistic View**: Ensures all strategic components are progressing in harmony
- **Performance Correlation**: Understand how projects, objectives, and sub-objectives interconnect
- **Goal Achievement**: Track progress toward organizational targets and milestones

## Calculation Formulas

### 1. Strategic Projects Card

```javascript
// Total Projects
totalProjects = projects.length

// Active Projects (Strategic Classification)
activeProjects = projects.filter(p => 
    p.schedule?.projectClassification === 'Strategic'
).length
```

**Formula Explanation:**
- **Total Projects**: Simple count of all projects in the system
- **Active Projects**: Filtered count where `projectClassification` equals `'Strategic'`

### 2. Strategic Objectives Card

```javascript
// Total Objectives
totalObjectives = strategicObjectives.length

// Achieved Objectives
achievedObjectives = strategicObjectives.filter(obj =>
    parseFloat(obj.actualMeasure || 0) >= parseFloat(obj.targetMeasure || 0)
).length
```

**Formula Explanation:**
- **Total Objectives**: Count of all strategic objectives
- **Achieved Objectives**: Count where `actualMeasure ≥ targetMeasure`
- **Achievement Rate**: `(achievedObjectives / totalObjectives) × 100`

### 3. Sub-Objectives Card

```javascript
// Total Sub-Objectives Available
totalSubObjectives = strategicObjectives.reduce((total, obj) =>
    total + (obj.subObjectives ? obj.subObjectives.length : 0), 0
)

// Completed Sub-Objectives
completedSubObjectives = strategicObjectives.reduce((total, obj) => {
    if (!obj.subObjectives) return total;
    
    const categorySubObjectives = getAvailableSubObjectivesForCategory(obj.objectiveName);
    const completedCount = categorySubObjectives.filter(subObjDef =>
        obj.subObjectives.some(sub => sub.name === subObjDef.name)
    ).length;
    
    return total + completedCount;
}, 0)
```

**Formula Explanation:**
- **Total Sub-Objectives**: Sum of all sub-objectives across all strategic objectives
- **Completed Sub-Objectives**: Count of sub-objectives that match predefined category requirements
- **Completion Rate**: `(completedSubObjectives / totalSubObjectives) × 100`

### 4. Overall Performance Card

```javascript
// Overall Performance Calculation
overallPerformance = () => {
    if (strategicObjectives.length === 0) return 0;
    
    const totalPerformance = strategicObjectives.reduce((sum, obj) => {
        const actual = parseFloat(obj.actualMeasure || 0);
        const target = parseFloat(obj.targetMeasure || 1);
        const performance = Math.min((actual / target) * 100, 100);
        return sum + performance;
    }, 0);
    
    return Math.round(totalPerformance / strategicObjectives.length);
}

// Performance Status Classification
performanceStatus = () => {
    const perf = overallPerformance;
    if (perf >= 90) return 'Excellent';
    if (perf >= 75) return 'Good';
    if (perf >= 60) return 'Fair';
    return 'Needs Improvement';
}
```

**Formula Explanation:**
- **Individual Objective Performance**: `(actualMeasure / targetMeasure) × 100` (capped at 100%)
- **Overall Performance**: Average of all individual objective performances
- **Status Classification**: Based on performance thresholds (90%, 75%, 60%)

## Performance Classification System

| Performance Range | Status | Color Coding | Action Required |
|------------------|--------|--------------|----------------|
| 90% - 100% | Excellent | Green | Maintain momentum |
| 75% - 89% | Good | Blue | Continue current approach |
| 60% - 74% | Fair | Orange | Monitor closely |
| < 60% | Needs Improvement | Red | Immediate action required |

## Component Implementation

### Template Structure
```vue
<div class="stats-cards-grid">
  <div class="stat-card projects">
    <div class="stat-icon">
      <i class="fa fa-folder-open"></i>
    </div>
    <div class="stat-content">
      <h3>{{ totalProjects }}</h3>
      <p>Strategic Projects</p>
      <span class="stat-detail">{{ activeProjects }} Active</span>
    </div>
  </div>
  
  <div class="stat-card objectives">
    <div class="stat-icon">
      <i class="fa fa-bullseye"></i>
    </div>
    <div class="stat-content">
      <h3>{{ totalObjectives }}</h3>
      <p>Strategic Objectives</p>
      <span class="stat-detail">{{ achievedObjectives }} Achieved</span>
    </div>
  </div>
  
  <div class="stat-card sub-objectives">
    <div class="stat-icon">
      <i class="fa fa-tasks"></i>
    </div>
    <div class="stat-content">
      <h3>{{ totalSubObjectives }}</h3>
      <p>Sub-Objectives</p>
      <span class="stat-detail">{{ completedSubObjectives }} Completed</span>
    </div>
  </div>
  
  <div class="stat-card performance">
    <div class="stat-icon">
      <i class="fa fa-line-chart"></i>
    </div>
    <div class="stat-content">
      <h3>{{ overallPerformance }}%</h3>
      <p>Overall Performance</p>
      <span class="stat-detail" :class="performanceClass">{{ performanceStatus }}</span>
    </div>
  </div>
</div>
```

### CSS Styling Features
- **Responsive Grid Layout**: Adapts to different screen sizes using CSS Grid
- **Hover Effects**: Interactive animations for better user experience
- **Color Coding**: Visual indicators for performance levels
- **Icon Integration**: FontAwesome icons for visual clarity
- **Gradient Backgrounds**: Modern visual design with subtle gradients

## Screenshots Section

### Main Dashboard View
![Stats Cards Grid Overview](../images/OverallAnalytics/stats-cards-overview.png)
*Caption: Complete stats cards grid showing all four key performance indicators*

### Individual Card Details
![Strategic Projects Card](../images/OverallAnalytics/projects-card-detail.png)
*Caption: Strategic Projects card highlighting total and active project counts*

![Strategic Objectives Card](../images/OverallAnalytics/objectives-card-detail.png)
*Caption: Strategic Objectives card showing achievement progress*

![Sub-Objectives Card](../images/OverallAnalytics/sub-objectives-card-detail.png)
*Caption: Sub-Objectives card displaying completion status*

![Performance Card](../images/OverallAnalytics/performance-card-detail.png)
*Caption: Overall Performance card with status indicator*

### Responsive Layout
![Mobile View](../images/OverallAnalytics/stats-cards-mobile.png)
*Caption: Stats cards grid optimized for mobile devices*

![Tablet View](../images/OverallAnalytics/stats-cards-tablet.png)
*Caption: Stats cards grid layout on tablet screens*

### Performance Status Indicators
![Excellent Performance](../images/OverallAnalytics/performance-excellent.png)
*Caption: Performance card showing "Excellent" status (90%+ performance)*

![Good Performance](../images/OverallAnalytics/performance-good.png)
*Caption: Performance card showing "Good" status (75-89% performance)*

![Fair Performance](../images/OverallAnalytics/performance-fair.png)
*Caption: Performance card showing "Fair" status (60-74% performance)*

![Needs Improvement](../images/OverallAnalytics/performance-poor.png)
*Caption: Performance card showing "Needs Improvement" status (<60% performance)*

## Screenshot Guidelines

### For Documentation
1. **Full Dashboard Context**: Always show the stats cards within the complete dashboard layout
2. **High Resolution**: Use minimum 1920x1080 resolution for clarity
3. **Consistent Lighting**: Maintain consistent theme (light/dark mode) across screenshots
4. **Annotations**: Add callouts and labels to highlight specific features
5. **Data Variety**: Include screenshots with different data scenarios (high/low performance)

### For User Guides
1. **Step-by-Step Views**: Show progression through different states
2. **Interactive Elements**: Capture hover states and animations
3. **Error States**: Document how cards appear with missing or invalid data
4. **Loading States**: Show cards during data loading phases

## Integration Notes

### Data Dependencies
- **strategicObjectives**: Array of strategic objective objects
- **projects**: Array of project objects with schedule classification
- **subObjectives**: Nested within strategic objectives

### Component Props
```javascript
props: {
  strategicObjectives: {
    type: Array,
    default: () => []
  },
  projects: {
    type: Array,
    default: () => []
  }
}
```

### Key Computed Properties
- `totalProjects`: Computed total project count
- `activeProjects`: Filtered strategic project count
- `totalObjectives`: Strategic objectives count
- `achievedObjectives`: Objectives meeting targets
- `totalSubObjectives`: Aggregated sub-objectives count
- `completedSubObjectives`: Completed sub-objectives count
- `overallPerformance`: Weighted average performance
- `performanceStatus`: Status classification string
- `performanceClass`: CSS class for status styling

## Usage Recommendations

### Best Practices
1. **Data Freshness**: Ensure data is updated in real-time or near real-time
2. **Performance Optimization**: Use computed properties for calculations
3. **Error Handling**: Implement fallbacks for missing data
4. **Accessibility**: Include appropriate ARIA labels and keyboard navigation
5. **Testing**: Validate calculations with various data scenarios

### Customization Options
- **Threshold Adjustment**: Modify performance classification thresholds
- **Color Schemes**: Adapt colors to match organizational branding
- **Icon Replacement**: Use custom icons instead of FontAwesome
- **Animation Timing**: Adjust hover and transition effects
- **Grid Layout**: Modify responsive breakpoints and card sizing

---

*This documentation is part of the Strategic Analytics Dashboard system. For additional information about related components, refer to the complete dashboard documentation.*
