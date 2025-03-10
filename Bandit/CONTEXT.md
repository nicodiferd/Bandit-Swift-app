# Bandit App: Flow and Features Documentation

This document provides a comprehensive overview of the "Bandit" app's flow and key features. It is intended to guide developers through the design and implementation of the app, ensuring clarity on user interactions and feature behavior.

---

## Table of Contents
1. [Overview](#overview)
2. [User Onboarding & Authentication](#user-onboarding--authentication)
3. [Main Dashboard & Navigation](#main-dashboard--navigation)
4. [Home Tab](#home-tab)
5. [Private Event Tab](#private-event-tab)
6. [Profile Tab](#profile-tab)
7. [Settings Tab](#settings-tab)
8. [Authentication Instructions](#authentication-instructions)
9. [Tech Stack](#tech-stack)
10. [Database Schema](#database-schema)
11. [Folder Structure](#folder-structure)
12. [Additional Considerations](#additional-considerations)

---

## Overview

**Bandit** is an event ride-share app designed to offer a safe and cost-effective alternative to services like Uber and Lyft in college towns. The primary goal is to reduce drinking and driving among college students by providing reliable transportation during events.

**Key Features:**
- **User Authentication:** Sign up or log in using Google, Apple ID, or email.
- **Dashboard Navigation:** A bottom tab bar with four main sections: Home, Private Event, Profile, and Settings.
- **Local Event Map:** A custom UI map displays local events to help users find rides.
- **Private Event Functionality:** Users can enter a unique event code to connect with designated drivers for private events.
- **Profile Customization:** Users can update and customize their profile details.
- **Settings Management:** Users can manage app settings and preferences.

---

## User Onboarding & Authentication

1. **Welcome Screen:**
   - **Visuals:** A clean screen featuring the Bandit company logo.
   - **Purpose:** Introduces users to the brand with minimal distraction.

2. **Sign-Up/Login:**
   - **Methods:** Users can sign up or log in using:
     - **Google Sign-In**
     - **Apple ID**
     - **Email authentication**
   - **Backend:** Authentication is handled via Firebase, ensuring secure and reliable account management.
   - **Flow:** Upon successful authentication, users are directed to the Home screen.

---

## Main Dashboard & Navigation

After authentication, users land on the main dashboard. This dashboard is composed of two key components:

1. **Dashboard Area:**
   - **Content:** Displays a custom UI map and/or event information relevant to the user's current location.

2. **Bottom Navigation Tab (Tab Bar):**
   - **Layout:** Equally spaced options, ensuring an intuitive user experience.
   - **Tabs:**
     - **Home (Default)**
     - **Private Event**
     - **Profile**
     - **Settings**

---

## Home Tab

- **Default Screen:** This is the landing screen after login.
- **Features:**
  - **Local Events Map:** A custom UI map that displays events happening near the user's current location.
  - **Event Details:** Tapping on events can provide more detailed information or options to request rides.
- **Purpose:** Provide a quick view of nearby events and facilitate ride requests to those events.

---

## Private Event Tab

- **Functionality:**
  - **Event Code Entry:** Users can enter a unique event code provided by event organizers.
  - **Driver Matching:** Once the event code is entered, the system searches for available drivers who are registered to service that specific private event.
- **User Flow:**
  1. User navigates to the Private Event tab.
  2. The app prompts for an event code.
  3. Upon code submission, the backend verifies the code and displays available drivers or event-specific ride options.
- **UI Considerations:** 
  - A simple input field for the event code.
  - Clear error messages for invalid or expired codes.
  - A loading indicator during the driver matching process.

---

## Profile Tab

- **Customization:**
  - **User Details:** Users can update personal information (e.g., name, profile picture, contact info).
  - **Preferences:** Options for ride preferences, payment methods, etc.
- **Flow:**
  1. User selects the Profile tab.
  2. The screen displays current profile details with editable fields.
  3. Upon making changes, users can save updates, which sync with the backend.
- **UI Considerations:** 
  - A clean and user-friendly layout.
  - Instant feedback on successful updates.
  - Integration with the Firebase backend for profile data management.

---

## Settings Tab

- **Functionality:**
  - **App Settings:** Options include notification preferences, privacy settings, and account management.
  - **Customization:** Users can tailor the app's behavior (e.g., map settings, ride preferences, etc.).
- **Flow:**
  1. User navigates to the Settings tab.
  2. Settings are categorized for easy navigation.
  3. Changes are saved and applied immediately or after confirmation.
- **UI Considerations:**
  - Use of toggles, dropdowns, and sliders for intuitive control.
  - Clear labeling and help icons for additional information on each setting.

---

## Authentication Instructions

This section provides guidelines for setting up authentication using Firebase and Google Sign-In.

### Firebase Setup for iOS
- **Project Setup:**
  - Create a new Firebase project in the [Firebase Console](https://console.firebase.google.com/) and add your iOS app.
  - Download the `GoogleService-Info.plist` file and add it to your Xcode project.
- **Dependency Management:**
  - Use CocoaPods to integrate Firebase SDK into your project. Include pods such as `Firebase/Auth` for authentication functionalities.
  - Run `pod install` to set up the dependencies.
- **Initialization:**
  - In your app's entry point (e.g., AppDelegate), import Firebase and call `FirebaseApp.configure()` during application launch.
- **Documentation Reference:** Detailed steps can be found in the [Firebase iOS Setup guide](https://firebase.google.com/docs/ios/setup) 

### Google Sign-In Integration (Web)
- **Configuration:**
  - Register your application on the [Google API Console](https://console.developers.google.com/) and obtain a client ID.
  - Include the Google Platform Library (`gapi`) in your project.
- **Implementation:**
  - Configure the Google Sign-In button in your app, ensuring proper scopes and client configurations.
  - Handle the authentication flow by retrieving the user's ID token and exchanging it with Firebase if needed.
- **Documentation Reference:** More details are available in the [Google Sign-In for Web documentation](https://developers.google.com/identity/sign-in/web/sign-in) 

---

## Tech Stack

- **Frontend:** Swift on Xcode
- **Backend/Database:** Firebase
- **UI Framework:** SwiftUI
- **AI Processing:** DeepSeek
---

## Database Schema

### Users Collection
```json
{
  "uid": "string (primary key)",
  "email": "string",
  "displayName": "string",
  "phoneNumber": "string?",
  "profileImage": "string (URL)?",
  "createdAt": "timestamp",
  "lastLogin": "timestamp",
  "isDriver": "boolean",
  "rating": "number",
  "totalRides": "number",
  "preferences": {
    "notifications": "boolean",
    "darkMode": "boolean",
    "language": "string"
  },
  "paymentMethods": [{
    "id": "string",
    "type": "string",
    "last4": "string",
    "isDefault": "boolean"
  }]
}
```

### Events Collection
```json
{
  "eventId": "string (primary key)",
  "name": "string",
  "description": "string",
  "location": {
    "latitude": "number",
    "longitude": "number",
    "address": "string"
  },
  "startTime": "timestamp",
  "endTime": "timestamp",
  "createdBy": "string (user uid)",
  "isPrivate": "boolean",
  "eventCode": "string?",
  "maxCapacity": "number?",
  "currentAttendees": "number",
  "status": "string (active/cancelled/completed)",
  "pricing": {
    "basePrice": "number",
    "surgeMultiplier": "number"
  }
}
```

### Rides Collection
```json
{
  "rideId": "string (primary key)",
  "eventId": "string (ref: events)",
  "driverId": "string (ref: users)",
  "passengerId": "string (ref: users)",
  "status": "string (pending/accepted/completed/cancelled)",
  "pickupLocation": {
    "latitude": "number",
    "longitude": "number",
    "address": "string"
  },
  "dropoffLocation": {
    "latitude": "number",
    "longitude": "number",
    "address": "string"
  },
  "requestTime": "timestamp",
  "pickupTime": "timestamp?",
  "completionTime": "timestamp?",
  "fare": "number",
  "rating": "number?",
  "review": "string?"
}
```

### Drivers Collection
```json
{
  "driverId": "string (ref: users)",
  "licenseNumber": "string",
  "vehicleInfo": {
    "make": "string",
    "model": "string",
    "year": "number",
    "color": "string",
    "licensePlate": "string"
  },
  "documents": {
    "license": "string (URL)",
    "insurance": "string (URL)",
    "backgroundCheck": "string (URL)"
  },
  "availability": {
    "isAvailable": "boolean",
    "lastLocationUpdate": "timestamp",
    "currentLocation": {
      "latitude": "number",
      "longitude": "number"
    }
  }
  "earnings": {
    "total": "number",
    "currentBalance": "number",
    "paymentHistory": [{
      "date": "timestamp",
      "amount": "number",
      "status": "string"
    }]
  }
}
```

### Transactions Collection
```json
{
  "transactionId": "string (primary key)",
  "rideId": "string (ref: rides)",
  "amount": "number",
  "status": "string (pending/completed/failed/refunded)",
  "paymentMethod": "string",
  "timestamp": "timestamp",
  "type": "string (ride/tip/refund)",
  "metadata": {
    "receiptUrl": "string?",
    "refundReason": "string?"
  }
}
```

## Folder Structure

```
BanditApp/
├── App.swift
├── Info.plist
├── Features/
│   ├── Authentication/
│   │   ├── Views/
│   │   │   ├── LoginView.swift
│   │   │   ├── SignUpView.swift
│   │   │   └── ForgotPasswordView.swift
│   │   ├── ViewModels/
│   │   │   └── AuthViewModel.swift
│   │   └── Models/
│   │       └── (Optional: e.g., UserCredentials.swift)
│   ├── Home/
│   │   ├── Views/
│   │   │   ├── HomeView.swift
│   │   │   ├── MapView.swift
│   │   │   └── EventListView.swift
│   │   ├── ViewModels/
│   │   │   └── HomeViewModel.swift
│   │   └── Models/
│   │       └── (Optional: e.g., EventViewData.swift)
│   ├── PrivateEvent/
│   │   ├── Views/
│   │   │   ├── PrivateEventView.swift
│   │   │   └── EventCodeEntry.swift
│   │   ├── ViewModels/
│   │   │   └── PrivateEventViewModel.swift
│   │   └── Models/
│   │       └── (Optional: e.g., PrivateEventData.swift)
│   ├── Profile/
│   │   ├── Views/
│   │   │   ├── ProfileView.swift
│   │   │   └── EditProfileView.swift
│   │   ├── ViewModels/
│   │   │   └── ProfileViewModel.swift
│   │   └── Models/
│   │       └── (Optional: e.g., ProfileData.swift)
│   └── Settings/
│       └── Views/
│           └── SettingsView.swift
├── Models/
│   ├── User.swift
│   ├── Event.swift
│   ├── Ride.swift
│   ├── Driver.swift
│   └── Transaction.swift
├── Services/
│   ├── AuthService.swift
│   ├── NetworkService.swift
│   ├── LocationService.swift
│   ├── PaymentService.swift
│   └── NotificationService.swift
├── Resources/
│   ├── Assets.xcassets
│   ├── Fonts/
│   └── Localizations/
└── Config/
    ├── GoogleService-Info.plist
    └── AppConfig.swift
```
## Additional Considerations

1. **Security & Data Handling:**
   - Ensure all authentication flows are secure.
   - Use best practices for data storage and user privacy on both Firebase and Supabase.

2. **User Experience:**
   - Maintain a consistent and minimalistic design across all screens.
   - Ensure the navigation is intuitive, especially for first-time users.
   - Optimize the custom UI map for performance and responsiveness.

3. **Error Handling & Feedback:**
   - Provide clear error messages and status updates (e.g., invalid event codes, network issues).
   - Include loading indicators where applicable to improve user experience during data fetching.

4. **Scalability:**
   - Design backend services to handle increasing user loads, especially during peak event times.
   - Use scalable solutions provided by Supabase and Firebase.

5. **Testing:**
   - Perform thorough testing for user flows, especially the event code validation and driver matching processes.
   - Ensure that changes in one tab (e.g., profile updates) reflect seamlessly across the app.

---

## Development Roadmap

### Phase 1: Project Setup and Authentication (1-2 weeks)
1. **Initial Project Setup**
   ```swift
   - Create new Xcode project with SwiftUI
   - Set up Git repository
   - Configure .gitignore for Xcode and CocoaPods
   ```

2. **Firebase Integration**
   ```swift
   - Install CocoaPods
   - Add Firebase pods
   - Configure Firebase in project
   - Set up Firebase Authentication
   ```

3. **Authentication Views**
   ```swift
   - Implement LoginView
   - Implement SignUpView
   - Implement ForgotPasswordView
   - Create AuthViewModel with Firebase Auth
   ```

### Phase 2: Core Data Models and Services (1 week)
1. **Data Models**
   ```swift
   - Create User model
   - Create Event model
   - Create Ride model
   - Create Driver model
   - Create Transaction model
   ```

2. **Base Services**
   ```swift
   - Implement NetworkService
   - Implement LocationService
   - Set up Firebase Firestore service
   ```

### Phase 3: Home Tab and Map Integration (2 weeks)
1. **Map Setup**
   ```swift
   - Integrate MapKit
   - Implement basic MapView
   - Add user location tracking
   ```

2. **Event Display**
   ```swift
   - Create EventCard component
   - Implement EventListView
   - Add map annotations for events
   ```

3. **Home Tab Integration**
   ```swift
   - Implement HomeView with map and event list
   - Add event filtering and search
   - Implement real-time event updates
   ```

### Phase 4: Private Event Feature (1 week)
1. **Event Code System**
   ```swift
   - Implement event code generation
   - Create EventCodeEntry view
   - Add validation and error handling
   ```

2. **Private Event Flow**
   ```swift
   - Implement PrivateEventView
   - Add driver matching logic
   - Create ride request flow
   ```

### Phase 5: Profile and Settings (1 week)
1. **Profile Management**
   ```swift
   - Implement ProfileView
   - Add profile editing
   - Implement image upload
   ```

2. **Settings Implementation**
   ```swift
   - Create SettingsView
   - Add notification preferences
   - Implement theme switching
   ```

### Phase 6: Payment Integration (1-2 weeks)
1. **Payment Setup**
   ```swift
   - Integrate Stripe SDK
   - Implement PaymentService
   - Create payment method management
   ```

2. **Transaction Flow**
   ```swift
   - Implement ride payment flow
   - Add tipping functionality
   - Create transaction history
   ```

### Phase 7: Driver Features (2 weeks)
1. **Driver Onboarding**
   ```swift
   - Create driver registration flow
   - Implement document upload
   - Add verification system
   ```

2. **Driver Dashboard**
   ```swift
   - Create driver-specific views
   - Implement earnings tracking
   - Add ride acceptance flow
   ```

### Phase 8: Testing and Polish (2 weeks)
1. **Testing**
   ```swift
   - Write unit tests
   - Implement UI tests
   - Perform integration testing
   ```

2. **Performance**
   ```swift
   - Optimize map performance
   - Improve load times
   - Reduce memory usage
   ```

3. **Final Polish**
   ```swift
   - Add loading animations
   - Implement error handling
   - Polish UI transitions
   ```

### Phase 9: Deployment (1 week)
1. **Preparation**
   ```swift
   - Finalize app icon and assets
   - Complete App Store listing
   - Prepare privacy policy
   ```

2. **Submission**
   ```swift
   - Run final tests
   - Create App Store build
   - Submit for review
   ```

Each phase should be completed and tested before moving to the next. This ensures a stable foundation for subsequent features. The timeline is approximate and may need adjustment based on team size and complexity encountered during development.

---

## Summary

The Bandit app is designed to provide a streamlined and safe ride-sharing experience for college students during events. With a focus on ease of use, security, and scalability, the app combines essential features such as a local events map, private event code functionality, customizable profiles, and comprehensive settings management. This documentation serves as a detailed blueprint for developers to build and enhance the app effectively.
