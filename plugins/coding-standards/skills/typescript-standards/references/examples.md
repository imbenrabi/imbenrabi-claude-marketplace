# TypeScript Code Standards - Examples

## Class-Based Implementation

### Before: Poor Implementation

This violates standards: multiple responsibilities, modifies state in a loop, uses `any`, no documentation.

```ts
class DataProcessor {
  processedData: string[] = [];

  process(rawData: any[]): void {
    const validItems = rawData.filter(item => item.id && item.value > 10);

    for (const item of validItems) {
      const formatted = `${item.id.toUpperCase()}: ${item.value.toFixed(2)}`;
      this.processedData.push(formatted);
    }
  }
}
```

### After: Correct Implementation

Pure helper methods, proper types, full TSDoc documentation.

```ts
interface DataItem {
  id: string;
  value: number;
}

class DataProcessor {
  processedData: string[] = [];

  /**
   * Processes raw data by filtering and formatting, then updates instance state.
   * @param rawData - The array of raw data objects to process.
   */
  process(rawData: DataItem[]): void {
    const validItems = this.#filterData(rawData);
    const formattedItems = validItems.map(item => this.#formatItem(item));
    this.processedData = formattedItems;
  }

  /**
   * Filters an array to include only valid items with id and value > 10.
   * @param data - The array of data to filter.
   * @returns A new array containing only valid items.
   */
  #filterData(data: DataItem[]): DataItem[] {
    return data.filter(item => item.id && item.value > 10);
  }

  /**
   * Formats a single data item into a standardized string.
   * @param item - The item to format.
   * @returns The formatted string representation.
   */
  #formatItem(item: DataItem): string {
    return `${item.id.toUpperCase()}: ${item.value.toFixed(2)}`;
  }
}
```

---

## Functional Implementation

### Before: Poor Implementation

Relies on external constant, mixes filtering and formatting, uses `any`, no documentation.

```ts
const MIN_VALUE = 10;

function processData(data: any[]): string[] {
  const results: string[] = [];
  for (const item of data) {
    if (item.id && item.value > MIN_VALUE) {
      results.push(`${item.id.toUpperCase()}: ${item.value.toFixed(2)}`);
    }
  }
  return results;
}
```

### After: Correct Implementation

Composed pure functions, proper types, single responsibility per function.

```ts
interface DataItem {
  id: string;
  value: number;
}

/**
 * Validates that an item meets processing criteria.
 * @param item - The item to validate.
 * @returns True if the item has an id and value greater than 10.
 */
const isValidItem = (item: DataItem): boolean => {
  return Boolean(item.id) && item.value > 10;
};

/**
 * Formats a data item into a standardized string representation.
 * @param item - The item to format.
 * @returns The formatted string.
 */
const formatItem = (item: DataItem): string => {
  return `${item.id.toUpperCase()}: ${item.value.toFixed(2)}`;
};

/**
 * Processes raw data by filtering valid items and formatting them.
 * @param rawData - The array of raw data objects to process.
 * @returns A new array of formatted strings.
 */
const processData = (rawData: DataItem[]): string[] => {
  return rawData.filter(isValidItem).map(formatItem);
};
```
