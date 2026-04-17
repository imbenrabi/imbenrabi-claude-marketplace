# Python Code Standards - Examples

## Class-Based Implementation

### Before: Poor Implementation

This violates standards: multiple responsibilities, modifies state in a loop, uses untyped dicts, no documentation.

```python
class DataProcessor:
    def __init__(self):
        self.processed_data = []

    def process(self, raw_data):
        # Filter out invalid items where value is 10 or less
        valid_items = [item for item in raw_data if item.get('id') and item.get('value', 0) > 10]

        # Now format the remaining items into strings
        for item in valid_items:
            formatted = f"{item['id'].upper()}: {item['value']:.2f}"
            self.processed_data.append(formatted)
```

### After: Correct Implementation

Pure helper methods, proper types with dataclass, full docstrings.

```python
from dataclasses import dataclass


@dataclass
class DataItem:
    id: str
    value: float


class DataProcessor:
    def __init__(self):
        self.processed_data: list[str] = []

    def process(self, raw_data: list[DataItem]) -> None:
        """
        Processes raw data by filtering and formatting, then updates instance state.

        Args:
            raw_data: The list of raw data objects to process.
        """
        valid_items = self._filter_data(raw_data)
        self.processed_data = [self._format_item(item) for item in valid_items]

    def _filter_data(self, data: list[DataItem]) -> list[DataItem]:
        """
        Filters a list to include only valid items with id and value > 10.

        Args:
            data: The list of data to filter.

        Returns:
            A new list containing only valid items.
        """
        return [item for item in data if item.id and item.value > 10]

    def _format_item(self, item: DataItem) -> str:
        """
        Formats a single data item into a standardized string.

        Args:
            item: The item to format.

        Returns:
            The formatted string representation.
        """
        return f"{item.id.upper()}: {item.value:.2f}"
```

---

## Functional Implementation

### Before: Poor Implementation

Relies on external constant, mixes filtering and formatting, untyped, no documentation.

```python
MIN_VALUE = 10

def process_data(data):
    results = []
    for item in data:
        if item.get('id') and item.get('value', 0) > MIN_VALUE:
            results.append(f"{item['id'].upper()}: {item['value']:.2f}")
    return results
```

### After: Correct Implementation

Composed pure functions, proper types, single responsibility per function.

```python
from dataclasses import dataclass


@dataclass
class DataItem:
    id: str
    value: float


def is_valid_item(item: DataItem) -> bool:
    """
    Validates that an item meets processing criteria.

    Args:
        item: The item to validate.

    Returns:
        True if the item has an id and value greater than 10.
    """
    return bool(item.id) and item.value > 10


def format_item(item: DataItem) -> str:
    """
    Formats a data item into a standardized string representation.

    Args:
        item: The item to format.

    Returns:
        The formatted string.
    """
    return f"{item.id.upper()}: {item.value:.2f}"


def process_data(raw_data: list[DataItem]) -> list[str]:
    """
    Processes raw data by filtering valid items and formatting them.

    Args:
        raw_data: The list of raw data objects to process.

    Returns:
        A new list of formatted strings.
    """
    return [format_item(item) for item in raw_data if is_valid_item(item)]
```
