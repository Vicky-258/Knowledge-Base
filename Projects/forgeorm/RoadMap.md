


### 🌉 **Milestone 5: Postgres Support**

**Goal: Prove adapter system works**

-  Create `PostgresAdapter`
    
-  Override `get_sql_type` where needed
    
-  Switchable adapter in config
    
-  Test table creation with PostgreSQL
    

---

### 🔮 Future Phases (Phase 2+ Ideas)

- 🧠 QueryBuilder class for more complex conditions
    
- 🔒 Field validations (like `max_length`)
    
- 🧩 Relationship support (`ForeignKeyField`, `OneToMany`, etc.)
    
- ⏳ Migrations system
    
- 🗃️ Connection pooling / thread safety
    
- 🧼 CLI commands: `forge init`, `forge makemigrations`, `forge migrate`