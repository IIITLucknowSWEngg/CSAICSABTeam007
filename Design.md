# Software Design Description (SDD)
## 1. Introduction

### 1.1 Purpose
The purpose of this Software Design Description (SDD) is to provide a detailed design for the YouTube clone video streaming platform. This document describes the software's architecture, components, interfaces, and their interactions to ensure that the implementation meets the system's functional and non-functional requirements.

### 1.2 Scope
The YouTube clone system enables users to upload, view, and interact with video content via a web or mobile application. The platform supports video streaming, user interaction (likes, comments), and content management. The system includes the frontend application, backend API services, real-time communication, and integration with external services like content delivery networks (CDNs) and analytics tools.

## 4. Module Design

### 4.1 Frontend Application (Web/Mobile)
#### 4.1.1 User Interface (UI)
The UI is designed to be user-friendly and adaptive to various devices. The main components include:

- **Home Screen**: Displays video recommendations and trending content.
- **Video Player Screen**: Provides a video playback interface with interactive features.
- **Upload Screen**: Allows users to upload videos with metadata.
- **User Profile**: Displays user details, uploaded videos, and watch history.
- **Search and Filter**: Enables users to search for videos based on keywords.

#### 4.1.2 Controller Layer
Manages user interactions and forwards requests to the service layer. Key controllers include:

- **Video Controller**
- **Authentication Controller**
- **Comment Controller**

#### 4.1.3 Service Layer
Implements business logic for:

- Video uploading and processing
- Video playback and recommendation
- User interactions (likes, comments, subscriptions)

### 4.2 Backend System (Server-side)
#### 4.2.1 API Gateway
The API Gateway serves as the entry point for all application requests, routing them to appropriate backend services.

#### 4.2.2 Authentication Service
Handles:
- User registration and login
- Token-based authentication
- Password management

#### 4.2.3 Video Management Service
Handles:
- Video Upload: Processes and stores videos.
- Video Streaming: Optimizes video playback using CDNs.
- Metadata Management: Manages video titles, descriptions, and tags.

#### 4.2.4 Interaction Service
Handles:
- User interactions like likes, comments, and subscriptions.
- Notifications for user actions (e.g., new video uploads).

#### 4.2.5 Recommendation Service
Provides personalized video recommendations using machine learning algorithms.

## 5. Database Design
The YouTube clone uses a combination of SQL and NoSQL databases. Below is the schema for major entities:

- **Users**: Stores user information like username, email, and preferences.
- **Videos**: Stores video metadata, upload details, and URLs.
- **Comments**: Stores user comments and associated metadata.
- **Interactions**: Tracks likes, views, and subscriptions.

<image src="./database-design.png"/>

#### Plantuml code
```plantuml
@startuml
left to right direction
actor User
actor "Unregistered Visitor" as Visitor
actor "Content Creator" as Creator
actor "Administrator" as Admin

rectangle YouTube {
    // Authentication Use Cases
    usecase "Register Account" as Register
    usecase "Login" as Login
    usecase "Manage Profile" as ManageProfile

    // Video Interaction Use Cases
    usecase "Watch Video" as WatchVideo
    usecase "Upload Video" as UploadVideo
    usecase "Edit Video Details" as EditVideo
    usecase "Delete Video" as DeleteVideo
    usecase "Like/Dislike Video" as RateVideo
    usecase "Comment on Video" as Comment
    usecase "Share Video" as ShareVideo

    // Channel Interaction Use Cases
    usecase "Create Channel" as CreateChannel
    usecase "Customize Channel" as CustomizeChannel
    usecase "Subscribe to Channel" as Subscribe
    usecase "View Channel Analytics" as ChannelAnalytics

    // Playlist Use Cases
    usecase "Create Playlist" as CreatePlaylist
    usecase "Add Video to Playlist" as AddToPlaylist
    usecase "Remove Video from Playlist" as RemoveFromPlaylist

    // Search and Discovery
    usecase "Search Videos" as Search
    usecase "Recommended Videos" as Recommendations
    usecase "Browse Categories" as BrowseCategories

    // Monetization
    usecase "Enable Monetization" as Monetize
    usecase "View Earnings" as ViewEarnings

    // Admin Use Cases
    usecase "Content Moderation" as Moderation
    usecase "User Management" as UserManagement
    usecase "Platform Settings" as PlatformSettings

    // Relationships
    Visitor --> WatchVideo
    Visitor --> Search
    Visitor --> Login
    Visitor --> Register

    User --> Login
    User --> WatchVideo
    User --> RateVideo
    User --> Comment
    User --> ShareVideo
    User --> Search
    User --> CreatePlaylist
    User --> AddToPlaylist
    User --> RemoveFromPlaylist
    User --> Subscribe
    User --> Recommendations
    User --> ManageProfile

    Creator --> UploadVideo
    Creator --> EditVideo
    Creator --> DeleteVideo
    Creator --> CreateChannel
    Creator --> CustomizeChannel
    Creator --> ChannelAnalytics
    Creator --> Monetize
    Creator --> ViewEarnings

    Admin --> Moderation
    Admin --> UserManagement
    Admin --> PlatformSettings
}
@enduml
```

## 6. Interface Design

### 6.1 API Design
Below are some of the REST API endpoints for interaction:

- **POST /upload**: Upload a video.
- **GET /videos**: Fetch video recommendations.
- **POST /comments**: Add a comment to a video.

### 6.2 External System Interfaces
- **CDN**: Delivers video content to users efficiently.
- **Analytics Tools**: Tracks user interactions and performance metrics.
- **Search Engine**: Provides fast and accurate search results.

### 6.3 Notification Flow Diagram
This diagram represents the flow of notifications for events like new comments or video uploads.

## 7. Non-Functional Requirements

### 7.1 Performance
The system should support at least 100,000 simultaneous users.

### 7.2 Scalability
The backend and CDN must be horizontally scalable.

### 7.3 Availability
Ensure 99.9% uptime with redundant components.

### 7.4 Security
Implement encryption for sensitive data and robust access control mechanisms.

## 8. Conclusion
This Software Design Description outlines the architecture, modules, database, and interfaces for the YouTube clone system, ensuring the platform is robust, scalable, and user-friendly. Modular design and integration with external services ensure the system can handle high traffic and provide an excellent user experience.