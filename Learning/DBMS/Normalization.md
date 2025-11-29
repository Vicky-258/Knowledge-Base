#DBMS 

# **Normalization in DBMS – The Complete Guide**

## **🔥 What is Normalization?**

Normalization is the process of organizing data to **reduce redundancy** and **eliminate anomalies** while maintaining data integrity.

## **📜 Normal Forms (NF) Explained**

### **✅ 1NF (First Normal Form) – Atomicity**

**Rule:** Ensure **atomic** (indivisible) values in each column.

- **❌ Violation:** Multiple values in one cell (e.g., storing multiple phone numbers in one field).
- **✅ Fix:** Create separate rows or a new table.

### **✅ 2NF (Second Normal Form) – No Partial Dependencies**

**Rule:** Remove **partial dependencies** (non-prime attributes should fully depend on the **entire** primary key).

- **❌ Violation:** If part of a composite key determines a column independently.
- **✅ Fix:** Split into separate tables.

### **✅ 3NF (Third Normal Form) – No Transitive Dependencies**

**Rule:** Remove **transitive dependencies** (where a non-key attribute depends on another non-key attribute).

- **❌ Violation:** If column B depends on column A, and column C depends on column B.
- **✅ Fix:** Move the dependent attribute to another table.

### **✅ BCNF (Boyce-Codd Normal Form) – Stronger 3NF**

**Rule:** Every determinant must be a **candidate key**.

- **❌ Violation:** If a non-trivial functional dependency exists where the determinant is **not** a candidate key.
- **✅ Fix:** Decompose the table further.

### **✅ 4NF (Fourth Normal Form) – No Multi-Valued Dependencies**

**Rule:** A table must not contain **independent** multi-valued facts.

- **❌ Violation:** If two or more independent multi-valued attributes exist.
- **✅ Fix:** Split into separate tables.

### **✅ 5NF (Fifth Normal Form) – No Join Dependencies**

**Rule:** The table should not have any join dependencies that could cause data duplication.

- **❌ Violation:** If breaking the table into smaller tables causes redundancy when joined back.
- **✅ Fix:** Further decomposition into tables.

### **✅ 6NF (Sixth Normal Form) – Temporal Dependencies (Time-Based Data)**

**Rule:** Handle **time-dependent** data effectively.

- **❌ Violation:** If a table has values that change over time, leading to redundancy.
- **✅ Fix:** Introduce a new table to track temporal changes.

## **📊 Example of Normalization Process**

### **Unnormalized Table (0NF)**

|Student_ID|Student_Name|Course|Instructor|Time Slot|
|---|---|---|---|---|
|301|Vicky|AI & ML|Prof. Stark|10 AM|
|301|Vicky|AI & ML|Prof. Banner|2 PM|
|301|Vicky|DSA|Prof. Turing|3 PM|
|302|Natasha|AI & ML|Prof. Stark|10 AM|
|302|Natasha|AI & ML|Prof. Banner|2 PM|

### **Final 6NF Structure**

#### **1️⃣ Student-Course Table**

|Student_ID|Course|
|---|---|
|301|AI & ML|
|301|DSA|
|302|AI & ML|

#### **2️⃣ Course-Instructor Table**

|Course|Instructor|
|---|---|
|AI & ML|Prof. Stark|
|AI & ML|Prof. Banner|
|DSA|Prof. Turing|

#### **3️⃣ Instructor-Time Table**

|Instructor|Course|Time Slot|
|---|---|---|
|Prof. Stark|AI & ML|10 AM|
|Prof. Banner|AI & ML|2 PM|
|Prof. Turing|DSA|3 PM|

✅ **Now, the data is fully normalized and free from anomalies!** 🚀

---

## **🎯 Summary of Normalization Rules**

|Normal Form|Rule|
|---|---|
|**1NF**|Atomic values only|
|**2NF**|No partial dependencies|
|**3NF**|No transitive dependencies|
|**BCNF**|Every determinant is a candidate key|
|**4NF**|No independent multi-valued dependencies|
|**5NF**|No join dependencies|
|**6NF**|Handles time-dependent data|

---

🎉 **Congratulations! You're now a Normalization Master!** 🏆💡