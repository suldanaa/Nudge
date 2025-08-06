# MemoMe

## Table of Contents

1. [Overview](#Overview)
2. [Product Spec](#Product-Spec)
3. [Wireframes](#Wireframes)
4. [Schema](#Schema)

## Overview

### Description

A photo-journal app where you're encouraged to take images, add your entries, and look back on your entries throughout the years.

### App Evaluation

[Evaluation of your app across the following attributes]
- **Category:** Lifestyle
- **Mobile:** Real-time capturing of your life through your phone camera lenses gives this app a particular need for it to be a mobile app. It wouldn't be able to be a website and hold the same purpose.
- **Story:** We live in a time where many life-documentation apps are based around social engagement. Locket, instagram, snapchat. We have very few photo apps where the main goal is to document things for your own interest in looking back at where you were earlier in the year.
- **Market:** Those who love journaling and life-documentation, but want a more private and accessible way to do such a thing.
- **Habit:** People will use this whenever they get the urge to photograph something, it can be multiple times a day, everyday.
- **Scope:** We haven't discussed how to incorporate a camera view yet so I'm a bit intimidated by that aspect. However, every other part is something we've already done (persiting data, calender view, etc.) However, I have a very clear view of the app.

## Product Spec

### 1. User Stories (Required and Optional)

**Required Must-have Stories**

* User can capture images on a specific date
* User can save notes on a specific date
* User can sign up with email and password
* User can view photos and notes associated with certain date
* User can select different visual themes for the app

**Optional Nice-to-have Stories**
* User can view entry stats (e.g. number of photos taken, most written month, most visited themes)
* User can add location information to their entries

### 2. Screen Archetypes

- [ ] Login Screen
* User can log in with email and password
- [ ] Registration Screen
*  User can register with email and password
- [ ] Camera screen
* User can take photos
- [ ] Notes screen
* User can take notes
- [ ] Calender view
* User can view past photo+text entries

### 3. Navigation

**Tab Navigation** (Tab to Screen)

* Text notes
* Camera
* Calender view

**Flow Navigation** (Screen to Screen)

- [ ] Login Screen
        => Camera
- [ ] Registration Screen
        => Camera
- [ ] Camera
        => Calender
        => Text notes

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
