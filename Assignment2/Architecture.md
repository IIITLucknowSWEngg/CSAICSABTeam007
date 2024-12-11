# 1. C4 Diagrams
# 1.1 System Context Diagram

<img width="1428" alt="Screenshot 2024-12-10 at 10 17 00 PM" src="https://github.com/user-attachments/assets/231c0c96-4c1e-4216-a615-e3f3a244765d">

```
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml

LAYOUT_WITH_LEGEND()
title System Context Diagram - YouTube Clone

Person(viewer, "Viewer", "Watches videos, comments, likes, subscribes")
Person(creator, "Content Creator", "Uploads videos, manages channel")
Person(admin, "Admin", "Platform management, content moderation")

System(youtubeClone, "YouTube Clone Platform", "Video sharing and streaming platform")

System_Ext(storage, "Cloud Storage", "AWS S3/GCS for video storage")
System_Ext(cdn, "CDN", "Cloudflare/Akamai for content delivery")
System_Ext(email, "Email Service", "SendGrid for notifications")
System_Ext(analytics, "Analytics", "User behavior tracking")
System_Ext(transcoder, "Transcoding Service", "FFmpeg cloud service")
System_Ext(search, "Search Engine", "Elasticsearch")
System_Ext(ml, "ML Service", "TensorFlow recommendation engine")

Rel(viewer, youtubeClone, "Uses", "HTTPS")
Rel(creator, youtubeClone, "Uses", "HTTPS")
Rel(admin, youtubeClone, "Manages", "HTTPS")

Rel(youtubeClone, storage, "Stores videos", "API")
Rel(youtubeClone, cdn, "Delivers content", "HTTPS")
Rel(youtubeClone, email, "Sends emails", "SMTP")
Rel(youtubeClone, analytics, "Tracks usage", "API")
Rel(youtubeClone, transcoder, "Processes videos", "API")
Rel(youtubeClone, search, "Indexes content", "API")
Rel(youtubeClone, ml, "Gets recommendations", "gRPC")
@enduml
```
# 1.2 Container Diagram

<img width="1039" alt="Screenshot 2024-12-10 at 11 25 22 PM" src="https://github.com/user-attachments/assets/3728147d-edea-4db8-8671-45659797a392">



```
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

LAYOUT_WITH_LEGEND()
title Container Diagram - YouTube Clone

Person(viewer, "Viewer", "Watches videos")
Person(creator, "Content Creator", "Uploads content")

System_Boundary(youtube, "YouTube Clone") {
    Container(web, "Web Application", "React", "Browser-based interface")
    Container(mobile, "Mobile App", "React Native", "Mobile interface")
    Container(api, "API Gateway", "Node.js", "API routing layer")
    
    Container(video, "Video Service", "Node.js", "Video management")
    Container(user, "User Service", "Node.js", "User management")
    Container(comment, "Comment Service", "Node.js", "Interactions")
    Container(search, "Search Service", "Elasticsearch", "Content search")
    Container(recommend, "Recommendation Service", "Python", "ML recommendations")
    Container(analytics, "Analytics Service", "Python", "Usage tracking")
    Container(notification, "Notification Service", "Node.js", "Push notifications")
    Container(transcode, "Transcode Service", "Python", "Video processing")
    
    ContainerDb(videodb, "Video DB", "MongoDB", "Video metadata")
    ContainerDb(userdb, "User DB", "PostgreSQL", "User data")
    ContainerDb(interactiondb, "Interaction DB", "MongoDB", "Social data")
    ContainerDb(analyticsdb, "Analytics DB", "ClickHouse", "Metrics")
    ContainerDb(searchdb, "Search DB", "Elasticsearch", "Search index")
    ContainerDb(cache, "Cache", "Redis", "Data caching")
    ContainerDb(queue, "Message Queue", "RabbitMQ", "Event processing")
}

System_Ext(storage, "Cloud Storage", "Video storage")
System_Ext(cdn, "CDN", "Content delivery")

Rel(viewer, web, "Uses", "HTTPS")
Rel(viewer, mobile, "Uses", "HTTPS")
Rel(creator, web, "Uses", "HTTPS")
Rel(creator, mobile, "Uses", "HTTPS")

Rel(web, api, "Uses", "HTTPS")
Rel(mobile, api, "Uses", "HTTPS")

Rel(api, video, "Uses", "gRPC")
Rel(api, user, "Uses", "gRPC")
Rel(api, comment, "Uses", "gRPC")
Rel(api, search, "Uses", "gRPC")

Rel(video, videodb, "Reads/Writes", "MongoDB Wire")
Rel(user, userdb, "Reads/Writes", "PostgreSQL")
Rel(comment, interactiondb, "Reads/Writes", "MongoDB Wire")
Rel(analytics, analyticsdb, "Reads/Writes", "TCP")
Rel(search, searchdb, "Reads/Writes", "HTTP")

Rel(video, storage, "Stores", "API")
Rel(video, cdn, "Distributes", "HTTPS")
Rel(notification, queue, "Subscribes", "AMQP")
Rel(transcode, queue, "Subscribes", "AMQP")
@enduml
```

# 1.3 Component Diagram 
## 1.3.1 Creator Features
<img width="1430" alt="Screenshot 2024-12-10 at 11 14 26 PM" src="https://github.com/user-attachments/assets/8905d8fa-8304-4bda-b53a-1bfe849adac5">


```
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Component.puml

LAYOUT_WITH_LEGEND()
title Component Diagram - Creator Features

Container_Boundary(creator, "Creator Features") {
    Component(upload, "Upload Manager", "Node.js", "Handles video uploads")
    Component(studio, "Creator Studio", "React", "Content management")
    Component(analytics, "Analytics Dashboard", "React", "Performance tracking")
    Component(community, "Community", "Node.js", "Audience interaction")
    
    Component(processor, "Upload Processor", "Python", "File processing")
    Component(thumbnail, "Thumbnail Generator", "Python", "Preview creation")
    Component(metadata, "Metadata Editor", "React", "Video information")
    Component(playlist, "Playlist Manager", "React", "Content organization")
    Component(endscreen, "End Screen Editor", "React", "Outro editing")
    Component(cards, "Card Editor", "React", "Overlay management")
    
    Component(comments, "Comment Manager", "Node.js", "Moderation")
}

System_Ext(storage, "Cloud Storage", "Video storage")
System_Ext(cdn, "CDN", "Content delivery")
System_Ext(analytics_ext, "Analytics Engine", "Metrics")

Rel(upload, storage, "Stores videos", "API")
Rel(upload, processor, "Processes", "Internal")
Rel(processor, thumbnail, "Generates", "Internal")

Rel(studio, metadata, "Manages", "Internal")
Rel(studio, playlist, "Organizes", "Internal")
Rel(studio, endscreen, "Configures", "Internal")
Rel(studio, cards, "Sets up", "Internal")

Rel(community, comments, "Moderates", "Internal")

Rel(analytics, analytics_ext, "Tracks", "API")
@enduml
```


## 1.3.2 Viewer Features
<img width="1382" alt="Screenshot 2024-12-10 at 11 39 15 PM" src="https://github.com/user-attachments/assets/6d8dd0da-d32a-4ef7-bb90-69557c46c425">

```
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Component.puml

LAYOUT_WITH_LEGEND()
title Component Diagram - Viewer Features

Container_Boundary(viewer, "Viewer Features") {
    Component(home, "Home Feed", "React", "Personalized content recommendation")
    Component(player, "Video Player", "React", "Media playback")
    Component(interact, "Interaction Manager", "Node.js", "Likes, comments, shares")
    Component(recommend, "Recommendation Engine", "Python", "Content suggestions")
    Component(search, "Search Interface", "React", "Content discovery")
    Component(profile, "User Profile", "React", "Personal settings")
    
    Component(watch_later, "Watch Later", "React", "Saved content list")
    Component(history, "Watch History", "Node.js", "Viewed content tracking")
    Component(playlists, "Playlist Manager", "React", "Custom content collections")
    
    Component(subscriptions, "Subscription Manager", "Node.js", "Channel following")
    Component(notifications, "Notification Center", "Node.js", "Updates and alerts")
    
    Component(comments, "Comment System", "Node.js", "Discussion management")
    Component(reactions, "Reaction Tracker", "Node.js", "Likes and interactions")
    
    Component(accessibility, "Accessibility Features", "React", "Subtitles, playback controls")
}

System_Ext(cdn, "CDN", "Content delivery")

Rel(home, recommend, "Fetches suggestions", "Internal")
Rel(home, cdn, "Loads content", "HTTPS")

Rel(player, cdn, "Streams video", "HTTPS")
Rel(player, accessibility, "Configures", "Internal")

Rel(interact, reactions, "Manages", "Internal")
Rel(interact, comments, "Handles", "Internal")

Rel(profile, history, "Tracks", "Internal")
Rel(profile, watch_later, "Manages", "Internal")
Rel(profile, playlists, "Organizes", "Internal")

Rel(subscriptions, notifications, "Triggers", "Internal")
@enduml
```
# 2. Deployment Diagram 

<img width="1382" alt="Screenshot 2024-12-10 at 11 43 48 PM" src="https://github.com/user-attachments/assets/214d1748-788c-4d34-81e2-611593057e73">

```
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Deployment.puml

LAYOUT_WITH_LEGEND()
title Deployment Diagram - YouTube Clone

Deployment_Node(client, "Client Tier", "User Devices") {
    Deployment_Node(browser, "Web Browser") {
        Container(web, "Web App", "React", "SPA")
    }
    Deployment_Node(mobile, "Mobile Device") {
        Container(app, "Mobile App", "React Native")
    }
    Deployment_Node(tv, "Smart TV") {
        Container(tv_app, "TV App", "Native")
    }
}

Deployment_Node(api, "API Tier", "Kubernetes") {
    Deployment_Node(api_cluster, "API Cluster", "Load Balanced") {
        Container(gateway, "API Gateway", "Node.js")
        Container(services, "Microservices", "Node.js/Python")
    }
    Deployment_Node(processing, "Processing Cluster") {
        Container(transcoder, "Transcoder", "FFmpeg")
        Container(thumb_gen, "Thumbnail Gen", "ImageMagick")
    }
    Deployment_Node(ml, "ML Cluster", "GPU") {
        Container(recommender, "Recommender", "TensorFlow")
    }
}

Deployment_Node(data, "Data Tier", "Storage") {
    Deployment_Node(db_cluster, "Database Cluster") {
        ContainerDb(postgres, "PostgreSQL", "Users")
        ContainerDb(mongo, "MongoDB", "Content")
        ContainerDb(elastic, "Elasticsearch", "Search")
        ContainerDb(redis, "Redis", "Cache")
        ContainerDb(rabbitmq, "RabbitMQ", "Queue")
    }
}

Deployment_Node(storage, "Storage Tier", "Global") {
    Deployment_Node(object_store, "Object Storage") {
        Container(video_store, "Video Storage", "S3/GCS")
        Container(asset_store, "Asset Storage", "S3/GCS")
    }
    Deployment_Node(cdn_nodes, "CDN Nodes") {
        Container(cdn, "CDN", "Cloudflare")
    }
}

Deployment_Node(monitoring, "Monitoring") {
    Container(logging, "Logging", "ELK Stack")
    Container(metrics, "Metrics", "Prometheus")
    Container(tracing, "Tracing", "Jaeger")
    Container(alerts, "Alerts", "PagerDuty")
}

Rel(web, gateway, "Uses", "HTTPS")
Rel(app, gateway, "Uses", "HTTPS")
Rel(tv_app, gateway, "Uses", "HTTPS")

Rel(gateway, services, "Routes to", "gRPC")
Rel(services, postgres, "Uses", "PostgreSQL")
Rel(services, mongo, "Uses", "MongoDB Wire")
Rel(services, redis, "Caches in", "Redis")
Rel(services, rabbitmq, "Messages via", "AMQP")

Rel(transcoder, video_store, "Stores in", "API")
Rel(cdn, video_store, "Caches from", "API")
Rel(web, cdn, "Streams from", "HTTPS")
Rel(app, cdn, "Streams from", "HTTPS")
@enduml
```



