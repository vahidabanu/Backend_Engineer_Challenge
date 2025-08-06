# Railse Backend Assignment Submission

## 👤 Candidate Details

- **Name:** Shaik Vahida Banu  
- **Email:** vbanu5408@gmail.com  
- **Phone:** +91-9390455835  
- **GitHub Repository:** https://github.com/vahidabanu/Backend_Engineer_Challenge
- **Video Demo Link:** https://drive.google.com/file/d/1GmTwKe3qRVS-qob8gRNGptePQEPvIwyw/view?usp=sharing

---

## 🐛 Bug Fix Summary

### 1. Bug #1 – Task status not updated properly
- **Root Cause:** Logic missing in `updateTaskStatus()` method.
- **Fix:** Added condition and state transition logic in `TaskManagementServiceImpl`.

### 2. Bug #2 – Null pointer exception on fetching task by date
- **Root Cause:** Task list was not null-checked.
- **Fix:** Introduced a null check and default empty list return in `fetchTasksByDate()`.

### 3. Bug #3 – Priority enum mismatch on front-end
- **Root Cause:** Enum value mismatch between request and model.
- **Fix:** Updated `Priority.java` enum to match API contract and added validation.

---

## ✨ New Features Implemented

### 1. Feature – Add Comments to Task
- **Endpoint:** `POST /api/task/comment`
- **Description:** Enables users to add comments to a task, logged with timestamps.
- **Testing:** Use Postman to POST JSON with `taskId`, `comment`.

### 2. Feature – Filter Tasks by Reference Type
- **Endpoint:** `GET /api/task?referenceType=FIELD`
- **Description:** Retrieve tasks filtered by enum-based reference type.
- **Testing:** Use query param `referenceType` in GET call.

### 3. Feature – Update Task Priority via API
- **Endpoint:** `PUT /api/task/update-priority`
- **Description:** Update the priority of a given task.
- **Testing:** Send JSON with `taskId` and `newPriority`.

---

## 📝 Notes
- Project built with **Java + Spring Boot + Gradle**
- Used **in-memory repositories** (`InMemoryTaskRepository`) for simulation
- Tested all endpoints using **Postman**
- For any missing assumptions, proper fallbacks were added to avoid exceptions.

