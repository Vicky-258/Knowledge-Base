### 🧱 **Milestone 1: Adapter-Level Execution**

**Goal: Actually create the table in SQLite DB**

-  Connect to SQLite DB from `SQLiteAdapter`
    
-  `create_table(model_cls)` — executes the SQL in SQLite
    
-  Check if table exists before creating (optional but neat)
    

---

### 🛠️ **Milestone 2: Insertion Logic**

**Goal: Insert records via `.save()`**

-  Add `save()` method in `BaseModel`
    
-  Adapter builds and executes `INSERT INTO` SQL
    
-  Auto-handle primary key (skip if `None`)
    
-  Type-safe default formatting
    

---

### 🔍 **Milestone 3: Querying Support**

**Goal: Add `.get()`, `.filter()`, and `.all()`**

-  `all()` → returns all rows
    
-  `filter(**kwargs)` → WHERE clause builder
    
-  `get(pk=...)` → shortcut to fetch by primary key
    
-  ORM returns model objects (not dicts)
    

---

### 🧪 **Milestone 4: Testing**

**Goal: Lock things in 🔐 with unit tests**

-  Use `pytest` to test:
    
    - Field initialization
        
    - Table creation
        
    - Data insertion
        
    - Querying
        
-  Use in-memory SQLite for speed: `sqlite:///:memory:`
    

---