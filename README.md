# Nudge

## Table of Contents

1. [Overview](#Overview)
2. [Product Spec](#Product-Spec)
3. [Wireframes](#Wireframes)
4. [Schema](#Schema)

## Overview

### Description

A task-management app designed to help users (especially those with ADHD or executive dysfunction) break overwhelming tasks into tiny, actionable steps. Encourages progress through microtasks.

### App Evaluation

[Evaluation of your app across the following attributes]
- **Category:** Productivity
- **Mobile:** Uniquely mobile for quick, on-the-go task breakdowns and reminders.
- **Story:** Addresses the paralysis of starting tasks by making them feel approachable.
- **Market:** Students, neurodivergent individuals, and anyone overwhelmed by complex tasks.
- **Habit:** Daily use for task logging and progress tracking.
- **Scope:** Focused on core functionality (task breakdowns + calendar), with room for expansions like making it gamified.

## Product Spec

### 1. User Stories (Required and Optional)

**Required Must-have Stories**

* User can create a task and break it into subtasks.
* User can mark subtasks as complete (visual progress).
* User can view tasks/subtasks in a calendar/daily view.

**Optional Nice-to-have Stories**
* Streaks/rewards for completing subtasks.
* Pomodoro timer integration for focus sessions.
  
### 2. Screen Archetypes

- [ ] Task Creation Screen
* Input main task → add subtasks (e.g., "Write Essay" → "Outline → Draft Intro").
- [ ] Task Dashboard
*  Today’s tasks + progress bars.
- [ ] Calender view
* Completed tasks.

### 3. Navigation

**Tab Navigation** (Tab to Screen)

* Today’s Tasks
* Task Breakdown
* Calendar view

**Flow Navigation** (Screen to Screen)

- [ ] Task dashboard
      => Tap task
      => Subtask editor
      => Calender

## Wireframes

[Add picture of your hand sketched wireframes in this section]
<img src="https://i.ibb.co/6Gy6FBP/IMG-4312.jpg" width=600>

### [BONUS] Digital Wireframes & Mockups

### [BONUS] Interactive Prototype

## Schema

### Models

---

#### User
| Property   | Type     | Description                                  |
|------------|----------|----------------------------------------------|
| `username` | String   | User's chosen username                       |
| `email`    | String   | User's email address                         |
| `password` | String   | User's password (handled securely by Parse)  |
| `theme`    | String   | (Optional) Current theme selected by user    |

---

#### Entry
| Property    | Type          | Description                                           |
|-------------|---------------|-------------------------------------------------------|
| `user`      | Pointer<User> | Reference to the user who created the entry          |
| `photo`     | File          | Image file captured by the camera                    |
| `note`      | String        | User’s written text entry                            |
| `date`      | Date          | Date the entry is associated with                    |
| `location`  | GeoPoint      | (Optional) Location where the entry was created      |
| `createdAt` | Date          | Timestamp when the entry was created (auto by Parse) |

---

#### Theme (Optional – for customizable themes)
| Property  | Type           | Description                           |
|-----------|----------------|---------------------------------------|
| `user`    | Pointer<User>  | Owner of the custom theme             |
| `name`    | String         | Theme name (e.g., “Moonlight”)        |
| `colors`  | Dictionary     | Key-value pairs for theme colors      |

---

### Networking

---

#### Auth Screens
- **[POST] Sign Up**
    ```swift
    let user = PFUser()
    user.username = "yourUsername"
    user.email = "yourEmail"
    user.password = "yourPassword"
    user.signUpInBackground { (success, error) in
        // Handle success or error
    }
    ```

- **[POST] Log In**
    ```swift
    PFUser.logInWithUsername(inBackground: "yourUsername", password: "yourPassword") { user, error in
        // Handle login result
    }
    ```

---

#### Camera / Entry Creation
- **[POST] Create New Entry**
    ```swift
    let entry = PFObject(className: "Entry")
    entry["user"] = PFUser.current()
    entry["photo"] = photoFile
    entry["note"] = noteText
    entry["date"] = selectedDate
    entry["location"] = PFGeoPoint(latitude: lat, longitude: lon) // optional
    entry.saveInBackground { success, error in
        // Handle save result
    }
    ```

---

#### Notes Screen
- **[POST] Save Note to Existing Entry**
    (if separated from photo)
    ```swift
    entry["note"] = updatedNote
    entry.saveInBackground()
    ```

---

#### Calendar Screen
- **[GET] Fetch Entries by User**
    ```swift
    let query = PFQuery(className: "Entry")
    query.whereKey("user", equalTo: PFUser.current()!)
    query.order(byDescending: "date")
    query.findObjectsInBackground { objects, error in
        // Handle entries
    }
    ```

- **[GET] Fetch Entries by Specific Date**
    ```swift
    query.whereKey("date", equalTo: selectedDate)
    ```

---

#### Theme Selection
- **[POST] Save User Theme Selection**
    ```swift
    PFUser.current()?["theme"] = selectedTheme
    PFUser.current()?.saveInBackground()
    ```

- **[GET] Fetch User’s Theme**
    ```swift
    let theme = PFUser.current()?["theme"] as? String
    ```

---

#### [Optional] Year in Review
- **[GET] Fetch Entry Stats**
    (Manually calculated client-side with multiple queries)
    - Total entries
    - Most used theme
    - Most active month
    - Top location

---
- [OPTIONAL: List endpoints if using existing API such as Yelp]
