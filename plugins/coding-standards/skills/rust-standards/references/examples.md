# Rust Code Standards - Examples

## Struct-Based Implementation

### Before: Poor Implementation

This violates standards: uses `Box<dyn Any>`, no documentation, mutates state in a loop, no derive macros, single method doing everything.

```rust
use std::any::Any;

struct DataProcessor {
    processed_data: Vec<String>,
}

impl DataProcessor {
    fn process(&mut self, raw_data: Vec<Box<dyn Any>>) {
        for item in raw_data {
            if let Some(map) = item.downcast_ref::<std::collections::HashMap<String, String>>() {
                let id = map.get("id").cloned().unwrap_or_default();
                let value: f64 = map.get("value").and_then(|v| v.parse().ok()).unwrap_or(0.0);
                if !id.is_empty() && value > 10.0 {
                    let formatted = format!("{}: {:.2}", id.to_uppercase(), value);
                    self.processed_data.push(formatted);
                }
            }
        }
    }
}
```

### After: Correct Implementation

Proper struct types, Rustdoc comments, derive macros, private helper methods, iterator combinators.

```rust
/// A single data record with an identifier and numeric value.
#[derive(Debug, Clone, PartialEq)]
struct DataItem {
    id: String,
    value: f64,
}

/// Filters and formats data items into standardized string representations.
#[derive(Debug, Default)]
struct DataProcessor {
    processed_data: Vec<String>,
}

impl DataProcessor {
    /// Processes raw data by filtering for valid items and formatting them,
    /// replacing the current processed data.
    ///
    /// # Arguments
    ///
    /// * `raw_data` - The slice of data items to process.
    fn process(&mut self, raw_data: &[DataItem]) {
        self.processed_data = filter_data(raw_data)
            .iter()
            .map(|item| format_item(item))
            .collect();
    }
}

/// Returns a new vector containing only items with a non-empty ID
/// and a value greater than 10.
///
/// # Arguments
///
/// * `data` - The slice of data items to filter.
///
/// # Returns
///
/// A new vector containing only the valid items.
fn filter_data(data: &[DataItem]) -> Vec<&DataItem> {
    data.iter()
        .filter(|item| !item.id.is_empty() && item.value > 10.0)
        .collect()
}

/// Converts a data item into its standardized string representation.
///
/// # Arguments
///
/// * `item` - The data item to format.
///
/// # Returns
///
/// The formatted string.
fn format_item(item: &DataItem) -> String {
    format!("{}: {:.2}", item.id.to_uppercase(), item.value)
}
```

---

## Functional Implementation

### Before: Poor Implementation

Uses untyped HashMap, external constant, mixes filtering and formatting, no documentation.

```rust
use std::collections::HashMap;

const MIN_VALUE: f64 = 10.0;

fn process_data(data: Vec<HashMap<String, String>>) -> Vec<String> {
    let mut results = Vec::new();
    for item in data {
        let id = item.get("id").cloned().unwrap_or_default();
        let value: f64 = item.get("value").and_then(|v| v.parse().ok()).unwrap_or(0.0);
        if !id.is_empty() && value > MIN_VALUE {
            results.push(format!("{}: {:.2}", id.to_uppercase(), value));
        }
    }
    results
}
```

### After: Correct Implementation

Composed pure functions, proper types, iterator chains, single responsibility per function.

```rust
/// A single data record with an identifier and numeric value.
#[derive(Debug, Clone, PartialEq)]
struct DataItem {
    id: String,
    value: f64,
}

impl DataItem {
    /// Reports whether the item meets processing criteria:
    /// a non-empty ID and a value greater than 10.
    fn is_valid(&self) -> bool {
        !self.id.is_empty() && self.value > 10.0
    }

    /// Returns the standardized string representation of the item.
    fn format(&self) -> String {
        format!("{}: {:.2}", self.id.to_uppercase(), self.value)
    }
}

/// Processes raw data by filtering valid items and formatting them.
///
/// # Arguments
///
/// * `raw_data` - The slice of data items to process.
///
/// # Returns
///
/// A new vector of formatted strings.
fn process_data(raw_data: &[DataItem]) -> Vec<String> {
    raw_data
        .iter()
        .filter(|item| item.is_valid())
        .map(|item| item.format())
        .collect()
}
```
