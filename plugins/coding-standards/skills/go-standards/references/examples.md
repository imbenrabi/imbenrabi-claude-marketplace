# Go Code Standards - Examples

## Struct-Based Implementation

### Before: Poor Implementation

This violates standards: uses `interface{}`, no documentation, mutates state in a loop, single method doing everything.

```go
package processor

type DataProcessor struct {
    ProcessedData []string
}

func (dp *DataProcessor) Process(rawData []interface{}) {
    for _, item := range rawData {
        m := item.(map[string]interface{})
        id, _ := m["id"].(string)
        value, _ := m["value"].(float64)
        if id != "" && value > 10 {
            formatted := fmt.Sprintf("%s: %.2f", strings.ToUpper(id), value)
            dp.ProcessedData = append(dp.ProcessedData, formatted)
        }
    }
}
```

### After: Correct Implementation

Proper struct types, GoDoc comments, unexported helper methods, value semantics where possible.

```go
package processor

import (
    "fmt"
    "strings"
)

// DataItem represents a single data record with an identifier and numeric value.
type DataItem struct {
    ID    string
    Value float64
}

// DataProcessor filters and formats data items.
type DataProcessor struct {
    ProcessedData []string
}

// Process filters raw data for valid items and formats them, replacing
// the current processed data.
func (dp *DataProcessor) Process(rawData []DataItem) {
    validItems := filterData(rawData)
    formatted := make([]string, 0, len(validItems))
    for _, item := range validItems {
        formatted = append(formatted, formatItem(item))
    }
    dp.ProcessedData = formatted
}

// filterData returns a new slice containing only items with a non-empty
// ID and a Value greater than 10.
func filterData(data []DataItem) []DataItem {
    result := make([]DataItem, 0, len(data))
    for _, item := range data {
        if item.ID != "" && item.Value > 10 {
            result = append(result, item)
        }
    }
    return result
}

// formatItem converts a data item into its standardized string representation.
func formatItem(item DataItem) string {
    return fmt.Sprintf("%s: %.2f", strings.ToUpper(item.ID), item.Value)
}
```

---

## Functional Implementation

### Before: Poor Implementation

Uses raw maps, external constant, mixes filtering and formatting, no documentation.

```go
package processor

var minValue = 10.0

func ProcessData(data []map[string]interface{}) []string {
    var results []string
    for _, item := range data {
        id, _ := item["id"].(string)
        value, _ := item["value"].(float64)
        if id != "" && value > minValue {
            results = append(results, fmt.Sprintf("%s: %.2f", strings.ToUpper(id), value))
        }
    }
    return results
}
```

### After: Correct Implementation

Composed pure functions, proper types, single responsibility per function.

```go
package processor

import (
    "fmt"
    "strings"
)

// DataItem represents a single data record with an identifier and numeric value.
type DataItem struct {
    ID    string
    Value float64
}

// IsValid reports whether the item meets processing criteria:
// a non-empty ID and a Value greater than 10.
func (d DataItem) IsValid() bool {
    return d.ID != "" && d.Value > 10
}

// Format returns the standardized string representation of the item.
func (d DataItem) Format() string {
    return fmt.Sprintf("%s: %.2f", strings.ToUpper(d.ID), d.Value)
}

// ProcessData filters valid items and returns their formatted representations.
func ProcessData(rawData []DataItem) []string {
    result := make([]string, 0, len(rawData))
    for _, item := range rawData {
        if item.IsValid() {
            result = append(result, item.Format())
        }
    }
    return result
}
```
