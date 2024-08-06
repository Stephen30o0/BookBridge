** THE DEMO FOR THIS WEB APPLICATION IS HOSTED LIVE HERE http://oolurinola.bubbleapps.io**


= SPEC-1: BookBridge Mobile Library Application
:sectnums:
:toc:


== Background

The BookBridge mobile library application is designed to address the educational challenges faced by students in rural areas, particularly in Africa. With limited access to physical libraries and educational resources, there is a significant gap in the availability of learning materials. BookBridge aims to bridge this gap by providing a digital library accessible via smartphones and tablets. The application offers offline access, multilingual content, and interactive learning features to enhance educational outcomes. By integrating with local educational systems and digital libraries, BookBridge supports personalized learning experiences and resource sharing among students and teachers.

== Requirements

*Functional Requirements*

=== Must Have
- FR 1.1: Allow users to browse and search for books.
- FR 1.2: Enable users to download books for offline access.
- FR 2.1: Provide quizzes related to book content.
- FR 2.2: Track user progress and performance on quizzes.
- FR 3.1: Support user profiles for personalized tracking and customization.
- FR 3.2: Offer multilingual content.

=== Should Have
- FR 4.1: Include teacher resources such as lesson plans and teaching guides.
- FR 4.2: Allow teachers to track student progress.

=== Could Have
- FR 5.1: Integrate social features for users to share and discuss books.
- FR 5.2: Provide recommendations based on user preferences and history.

=== Won't Have
- FR 6.1: Integration with third-party educational platforms in the initial version.

*Non-Functional Requirements*

=== Must Have
- NFR 1: The app must load within 5 seconds.
- NFR 2: Must support up to 10,000 concurrent users.
- NFR 3: Ensure that content is appropriate for the specified age group.
- NFR 4: User data must be encrypted in transit and at rest.
- NFR 5: Authentication required for accessing user profiles and progress.
- NFR 6: The app must be reliable, with 99.9% uptime.
- NFR 7: Must be maintainable and allow for easy updates and content additions.
- NFR 8: Only authorized personnel can upload or modify content.

=== Should Have
- NFR 9: The app should be intuitive and easy to use, even for those with basic smartphone literacy.
- NFR 10: Provide comprehensive user manuals and integrated help sections within the app.

=== Could Have
- NFR 11: The app could offer customizable themes and display settings for better accessibility.
- NFR 12: Implement advanced analytics for tracking and improving user engagement.

=== Won't Have
- NFR 13: Support for desktop or web application versions in the initial release.

== Method

=== Architecture Overview

The BookBridge application will follow a client-server architecture. The mobile application will act as the client, interacting with a backend server that handles data storage, synchronization, and user authentication. The server will also integrate with external content providers to fetch and update educational resources.

[plantuml, architecture, png]
----
@startuml
!define RECTANGLE(rect_name) rect rect_name

package "Client" {
  RECTANGLE(Android/iOS App)
}

package "Backend Server" {
  RECTANGLE(API Gateway)
  RECTANGLE(Authentication Service)
  RECTANGLE(Content Management Service)
  RECTANGLE(Database)
}

package "External Services" {
  RECTANGLE(Educational Content Providers)
}

Client --> Backend Server : API Requests
Backend Server --> Database : Read/Write
Backend Server --> External Services : Fetch/Update Content
Client --> Backend Server : User Authentication
@enduml
----

=== Component Design

==== Client (Mobile Application)

The mobile application will be developed using a cross-platform framework like React Native to ensure compatibility with both Android and iOS. The application will include the following components:

- **Home Screen**: Provides quick access to the library, downloads, and user profile.
- **Library Screen**: Allows users to browse and search for books and resources.
- **Book Viewer**: Enables users to read and interact with digital content.
- **Quiz Interface**: Allows users to engage with quizzes and track their scores.
- **User Profile**: Displays personalized information and tracks user progress.

==== Backend Server

The backend server will be developed using Node.js with Express.js for handling API requests. The server components include:

- **API Gateway**: Routes requests to appropriate services.
- **Authentication Service**: Manages user authentication and authorization.
- **Content Management Service**: Handles fetching, updating, and storing educational content.
- **Database**: Stores user data, content metadata, and quiz results. A NoSQL database like MongoDB will be used for flexibility and scalability.

==== External Services

The server will integrate with external educational content providers to fetch and update resources. These integrations will be managed through APIs provided by the content providers.

=== Database Schema

A NoSQL database like MongoDB will be used to store the following collections:

- **Users**: Stores user profile information, including authentication details and preferences.
- **Books**: Stores metadata about the books, such as titles, authors, languages, and download links.
- **Quizzes**: Stores quiz questions, answers, and user performance data.

Here's a sample schema for each collection:

```json
// Users Collection
{
  "_id": "user_id",
  "username": "string",
  "passwordHash": "string",
  "email": "string",
  "preferences": {
    "language": "string",
    "readingHistory": ["book_id"]
  }
}

// Books Collection
{
  "_id": "book_id",
  "title": "string",
  "author": "string",
  "language": "string",
  "downloadLink": "string",
  "summary": "string",
  "content": "text"
}

// Quizzes Collection
{
  "_id": "quiz_id",
  "bookId": "book_id",
  "questions": [
    {
      "questionText": "string",
      "options": ["string"],
      "correctOption": "string"
    }
  ],
  "userScores": [
    {
      "userId": "user_id",
      "score": "number"
    }
  ]
}
