---
title: Bar Chart Visualization
type: docs
weight: 8
---

Simple data visualization with bar charts.

## Code

```go
labels := ["Apple", "Banana", "Orange", "Kiwi"]
values := [3, 4, 7, 5]

barchart := chart.NewBarChart("Fruit Sales", "Count", labels, values)
window.SetContent(barchart)
```

## Parameters

- **Title**: "Fruit Sales" - Chart title
- **Y-axis label**: "Count" - Label for values
- **Labels**: Array of category names
- **Values**: Array of numeric values

## Key Concepts

- Simple chart creation with arrays
- Automatic scaling and layout
- Clean visualization for data

## Use Cases

- Sales reports
- Survey results
- Performance metrics
- Data comparison

## Tips

- Keep labels short for better readability
- Use meaningful titles and axis labels
- Values are automatically scaled to fit the chart
