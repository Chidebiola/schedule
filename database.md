# Schedule Tracker - Database Model

## 1. User Authentication & Profile

### User (Collection)
- id (string) - Firebase Auth UID
- email (string)
- displayName (string)
- photoURL (string)
- createdAt (timestamp)
- updatedAt (timestamp)
- lastLoginAt (timestamp)
- isDeleted (boolean)
- deletedAt (timestamp)

### Profile (Collection)
- userId (reference)
- timeZone (string)
- workingHours (array)
  - start (string)
  - end (string)
  - dayOfWeek (string)
- preferredLanguage (string)
- notificationPreferences (object)
  - reminders (boolean)
  - progressUpdates (boolean)
  - teamNotifications (boolean)
- themePreference (string) - "light" | "dark" | "system"

## 2. Task Management

### Task (Collection)
- id (string)
- userId (reference)
- title (string)
- description (string)
- category (string) - enum: "Work", "Personal", "Fitness", etc.
- priority (string) - enum: "High", "Medium", "Low"
- status (string) - enum: "Pending", "In Progress", "Completed", "Deleted"
- dueDate (timestamp)
- startDate (timestamp)
- endDate (timestamp)
- estimatedTime (number) - in minutes
- actualTime (number) - in minutes
- tags (array)
- createdAt (timestamp)
- updatedAt (timestamp)
- deletedAt (timestamp)

### TaskHistory (Collection)
- taskId (reference)
- userId (reference)
- action (string) - enum: "Created", "Updated", "Deleted", "Completed"
- previousState (object)
- newState (object)
- timestamp (timestamp)

## 3. Time Tracking

### TimeEntry (Collection)
- id (string)
- userId (reference)
- taskId (reference)
- startTime (timestamp)
- endTime (timestamp)
- duration (number) - in minutes
- notes (string)
- category (string)
- date (date)
- createdAt (timestamp)
- updatedAt (timestamp)
- deletedAt (timestamp)

### DailySummary (Collection)
- userId (reference)
- date (date)
- totalTime (number) - in minutes
  - workTime (number)
  - personalTime (number)
  - fitnessTime (number)
  - otherTime (number)
- productivityScore (number) - 0-100
- mood (string)
- energyLevel (number) - 1-5
- createdAt (timestamp)
- updatedAt (timestamp)

## 4. Goals & Milestones

### Goal (Collection)
- id (string)
- userId (reference)
- title (string)
- description (string)
- category (string)
- startDate (timestamp)
- targetDate (timestamp)
- progress (number) - 0-100
  - completedTasks (number)
  - totalTasks (number)
- status (string) - enum: "Active", "Completed", "Deleted"
- streak (number)
- badges (array)
- createdAt (timestamp)
- updatedAt (timestamp)
- deletedAt (timestamp)

### Milestone (Collection)
- id (string)
- goalId (reference)
- title (string)
- description (string)
- dueDate (timestamp)
- status (string) - enum: "Pending", "Completed", "Deleted"
- createdAt (timestamp)
- updatedAt (timestamp)
- deletedAt (timestamp)

## 5. Team Collaboration

### Team (Collection)
- id (string)
- name (string)
- description (string)
- ownerId (reference)
  - members (array)
    - userId (reference)
    - role (string) - enum: "Admin", "Member"
    - joinedAt (timestamp)
  - createdAt (timestamp)
  - updatedAt (timestamp)

### TeamTask (Collection)
- id (string)
- teamId (reference)
- taskId (reference)
  - assignedTo (reference)
  - status (string) - enum: "Pending", "In Progress", "Completed", "Deleted"
  - comments (array)
    - userId (reference)
    - content (string)
    - timestamp (timestamp)
  - createdAt (timestamp)
  - updatedAt (timestamp)
  - deletedAt (timestamp)

## 6. Journal & Reflection

### JournalEntry (Collection)
- id (string)
- userId (reference)
- date (date)
  - content (string)
  - mood (string)
  - energyLevel (number) - 1-5
  - productivityRating (number) - 1-5
  - reflection (string)
  - createdAt (timestamp)
  - updatedAt (timestamp)
  - deletedAt (timestamp)

### Reflection (Collection)
- id (string)
- journalEntryId (reference)
  - insights (array)
    - content (string)
    - category (string)
    - createdAt (timestamp)
- createdAt (timestamp)
- updatedAt (timestamp)

## 7. Analytics & Reports

### Analytics (Collection)
- id (string)
- userId (reference)
- dateRange (object)
  - start (timestamp)
  - end (timestamp)
  - timeSpent (object)
    - byCategory (object)
    - byPriority (object)
  - productivityMetrics (object)
    - completionRate (number)
    - averageTime (number)
    - streak (number)
  - createdAt (timestamp)
  - updatedAt (timestamp)

## 8. Security & Access Control

### Role (Collection)
- id (string)
- name (string)
- permissions (array)
  - canCreateTasks (boolean)
  - canEditTasks (boolean)
  - canDeleteTasks (boolean)
  - canManageTeam (boolean)
  - canViewAnalytics (boolean)

### UserRoles (Collection)
- userId (reference)
- roleId (reference)
- teamId (reference)
- createdAt (timestamp)

## 9. Data Relationships

### One-to-Many Relationships
- User -> Profile (1:1)
- User -> Task (1:N)
- User -> TimeEntry (1:N)
- User -> DailySummary (1:N)
- User -> Goal (1:N)
- User -> JournalEntry (1:N)
- User -> Analytics (1:N)

### Many-to-Many Relationships
- Team <-> User (via TeamTask)
- Task <-> Category (via tags)

## 10. Indexes & Performance

### Indexes
- User: email (unique