This procedure describes how to transform a project schedule in Excel into an RDF Turtle file (`.ttl`) containing tasks as nodes, hierarchy edges, and attributes including start/finish dates.

---

## **1. Input Assumptions**

- Input file: Excel workbook (`.xlsx`).
    
- The workbook contains at least one sheet with project tasks.
    
- Columns present:
    
    - **Name**: Task name (header = `name`, case insensitive).
        
    - **Outline/Level**: Outline level or indent (header contains one of: `outline`, `outline level`, `level`, `indent`, `indent level`, `lvl`).
        
    - **Start**: Task start date (header contains one of: `start`, `start date`, `begin`).
        
    - **Finish**: Task finish date (header contains one of: `finish`, `finish date`, `end`, `end date`).
        

---

## **2. Load the Data**

1. Load the Excel workbook with pandas.
    
2. Select the first sheet (or a named sheet if specified).
    
3. Detect the correct columns:
    
    - `name_col` = column exactly named `"name"` (lowercased). If not found, try `"task name"`, `"description"`, `"titel"`, `"title"`.
        
    - `outline_col` = column containing outline levels (see assumptions above).
        
    - `start_col` = column with start dates (see assumptions above).
        
    - `finish_col` = column with finish dates (see assumptions above).
        
4. If any of these required columns cannot be found, stop with an error.
    

---

## **3. Normalize Columns**

- Create a dataframe `df` with columns:
    
    - `name` = from `name_col` (string).
        
    - `level` = from `outline_col` (integer).
        
    - `start` = from `start_col` if present.
        
    - `finish` = from `finish_col` if present.
        

### Normalize levels

`def to_int(v):     try: return int(float(v))     except: return 0 df["level"] = df["level"].apply(to_int)`

### Normalize dates

Convert `start` and `finish` to ISO-8601 strings:

`def to_iso(val):     if pd.isna(val): return None     dt = pd.to_datetime(val, errors="coerce")     if pd.isna(dt): return None     if dt.hour==0 and dt.minute==0 and dt.second==0:         return dt.date().isoformat()     return dt.isoformat()`

Apply to both columns → `df["start_iso"]`, `df["finish_iso"]`.

---

## **4. Build Hierarchy**

We must compute parent/child relationships from the outline levels.

Algorithm:

1. Maintain a stack of `(row_index, level)`.
    
2. For each row:
    
    - Let `lvl = row.level`.
        
    - While stack is not empty and stack[-1].level ≥ `lvl`, pop the stack.
        
    - If stack not empty after popping, the parent of this row = stack[-1].row_index.
        
    - Otherwise, parent = None.
        
    - Push `(row_index, lvl)` onto the stack.
        
3. Store parent indices in `df["parent_idx"]`.
    

---

## **5. Create RDF URIs**

For each row, create a unique URI:

1. Slugify the task name (ASCII-only, replace spaces/punctuation with `-`).
    
2. Append the row index to guarantee uniqueness.
    
3. Example:
    
    `name = "Transport & Connecting" slug = "transport-connecting" row index = 42 URI = task:transport-connecting-42`
    

---

## **6. Generate Turtle File**

### Prefixes

`@prefix sabre: <https://sabre.example/vocab#> . @prefix task:  <https://sabre.example/task/> . @prefix xsd:   <http://www.w3.org/2001/XMLSchema#> .`

### Triples for each task

- `a sabre:Task`
    
- `sabre:label` → task name (escaped string).
    
- `sabre:outlineLevel` → integer.
    
- `sabre:start` → xsd:date or xsd:dateTime (if present).
    
- `sabre:finish` → xsd:date or xsd:dateTime (if present).
    
- `sabre:isSummary` → boolean (`true` if this node has children, else `false`).
    

Example:

`task:transport-connecting-42 a sabre:Task ;   sabre:label "Transport & Connecting" ;   sabre:outlineLevel "2"^^xsd:integer ;   sabre:start "2025-03-12"^^xsd:date ;   sabre:finish "2025-03-15"^^xsd:date ;   sabre:isSummary "true"^^xsd:boolean .`

### Hierarchy edges

For each parent–child relationship:

- `parent sabre:hasChild child .`
    
- `child sabre:childOf parent .`
    
- `child sabre:orderIndex "N"^^xsd:integer` (sibling order under parent).
    

Example:

`task:transport-connecting-42 sabre:hasChild task:hull-superstructure-yard-43 . task:hull-superstructure-yard-43 sabre:childOf task:transport-connecting-42 . task:hull-superstructure-yard-43 sabre:orderIndex "1"^^xsd:integer .`

---

## **7. Output**

- Write all triples to a `.ttl` file (`schedule_full_with_dates.ttl`).
    
- Ensure UTF-8 encoding.
    
- Validate by searching for `sabre:hasChild` and `sabre:childOf` in the file to confirm hierarchy edges are present.
    

---

## **8. Validation Checklist**

1. Total number of nodes = number of rows in Excel.
    
2. Each node has:
    
    - `sabre:label`
        
    - `sabre:outlineLevel`
        
    - Optional `sabre:start` and `sabre:finish` if dates exist
        
    - `sabre:isSummary`
        
3. Each non-root node has:
    
    - `sabre:childOf` triple
        
    - `sabre:orderIndex`
        
4. Each parent has:
    
    - One or more `sabre:hasChild` triples