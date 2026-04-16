---
description: Create a single dummy user in the database
allowed-tools: Read, Bash(python3:*)
---

Read database/db.py to understand the users table schema and the get_db() helper.

then write and run a Python script using that bash that:

1. Generate the random Pakistani user using your own knowledge of Pakistan names across regions:

 - Name: a realist Pakistani First + Last name
 - Email: derived from the name with random 2-3 digit number suffix (e.g rahul.sharma91@gmai.com)
 - Password: "password123" hashed with werkzueg's generate_password_hash 
 - created_at: current datetime

2. Checks if the generate email already exists in the user table.if it does,regenerate until unique

3. Inserts the user into the database using the same get_db() pattern found in db.py.

4. print confirmation:
 - id
 - name
 - email
 