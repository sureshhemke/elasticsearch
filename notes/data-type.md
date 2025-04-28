Data Type | What It Stores | Example|
|---------|----------------|--------|
Text | Large blocks of text that you want to search through. | "Hello world"
Keyword | Exact values (like IDs or tags) that are not changed or analyzed. | "user123"
Integer | Whole numbers (small numbers). | 42
Long | Larger whole numbers (big numbers). | 1000000
Float | Numbers with decimals (small numbers). | 3.14
Double | Numbers with decimals (bigger numbers). | 3.14159
Boolean | True or false values. | true
Date | Date and time information. | "2025-04-28T15:30:00"
Binary | Any binary data, like images or files, stored in a special format. | "aGVsbG8gd29ybGQ="
IP | Internet IP addresses (IPv4 or IPv6). | "192.168.1.1"
Geo-point | Geographic locations (latitude and longitude). | "40.7128, -74.0060"
Geo-shape | More complex geographic data (like areas or shapes). | A shape of a country, city, etc.
Object | Structured data (like a small chunk of JSON). | {"name": "John", "age": 30}
Nested | Arrays of structured data (like a list of things, each with details). | [{"color": "red", "size": "L"}, {"color": "blue", "size": "M"}]
Range | Ranges of values (like numbers, dates, or IP addresses). | {"gte": 10, "lte": 20}


# Simple Explanation:
- Text & Keyword: Use text for searchable sentences (like articles or product descriptions) and keyword for exact values (like tags or IDs).

- Numeric Types: Use integer or long for whole numbers and float or double for numbers with decimals.

- Boolean: Stores true/false values, like checking if something is available.

- Date: For storing dates and times, useful for tracking events or records.

- Binary: For storing file data (like images or files).

- IP: Stores IP addresses, useful for tracking user locations.

- Geo-point: Stores geographic coordinates for locations on the map.

- Geo-shape: Stores shapes or areas on a map, such as a polygon or a circle.

- Object & Nested: Useful for structured data, like details about a person or multiple related items.

- Range: For storing ranges (like between 10 and 20), useful for filtering or searching within a range of values.
