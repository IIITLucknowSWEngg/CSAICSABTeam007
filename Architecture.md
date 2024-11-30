# System Context Diagram

![Screenshot_20241130_193149](https://github.com/user-attachments/assets/81dbb0df-d379-421f-a33f-66032d9bfb09)


```
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml

LAYOUT_WITH_LEGEND()
title System Context Diagram - YouTube Clone

Person(viewer, "Viewer", "Watches videos, comments, likes, subscribes")
Person(creator, "Content Creator", "Uploads videos, manages channel")
Person(admin, "Admin", "Platform management, content moderation")
Person(advertiser, "Advertiser", "Creates and manages ad campaigns")

System(youtubeClone, "YouTube Clone Platform", "Video sharing and streaming platform")

System_Ext(storage, "Cloud Storage", "AWS S3/GCS for video storage")
System_Ext(cdn, "CDN", "Cloudflare/Akamai for content delivery")
System_Ext(payment, "Payment Gateway", "Stripe/PayPal integration")
System_Ext(email, "Email Service", "SendGrid for notifications")
System_Ext(analytics, "Analytics", "User behavior tracking")
System_Ext(adNetwork, "Ad Network", "Programmatic advertising")
System_Ext(transcoder, "Transcoding Service", "FFmpeg cloud service")
System_Ext(search, "Search Engine", "Elasticsearch")
System_Ext(ml, "ML Service", "TensorFlow recommendation engine")
System_Ext(moderation, "Content Moderation", "AI-based screening")

Rel(viewer, youtubeClone, "Uses", "HTTPS")
Rel(creator, youtubeClone, "Uses", "HTTPS")
Rel(admin, youtubeClone, "Manages", "HTTPS")
Rel(advertiser, youtubeClone, "Uses", "HTTPS")

Rel(youtubeClone, storage, "Stores videos", "API")
Rel(youtubeClone, cdn, "Delivers content", "HTTPS")
Rel(youtubeClone, payment, "Processes payments", "API")
Rel(youtubeClone, email, "Sends emails", "SMTP")
Rel(youtubeClone, analytics, "Tracks usage", "API")
Rel(youtubeClone, adNetwork, "Serves ads", "API")
Rel(youtubeClone, transcoder, "Processes videos", "API")
Rel(youtubeClone, search, "Indexes content", "API")
Rel(youtubeClone, ml, "Gets recommendations", "gRPC")
Rel(youtubeClone, moderation, "Screens content", "API")
@enduml
```
# Container Diagram

![container](https://github.com/user-attachments/assets/4270db57-cf3d-4853-a2e2-bfd0b8934f78)

```
@startuml "Container Diagram"
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

LAYOUT_WITH_LEGEND()
title Container Diagram - YouTube Clone

Person(viewer, "Viewer", "Watches videos")
Person(creator, "Content Creator", "Uploads content")
Person_Ext(advertiser, "Advertiser", "Manages ads")

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
    Container(monetization, "Monetization Service", "Node.js", "Payments")
    Container(moderation, "Moderation Service", "Python", "Content screening")
    Container(streaming, "Streaming Service", "Node.js", "Live streams")
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
System_Ext(payment, "Payment Gateway", "Payment processing")

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
Rel(monetization, payment, "Processes", "API")

Rel(video, queue, "Publishes", "AMQP")
Rel(notification, queue, "Subscribes", "AMQP")
Rel(transcode, queue, "Subscribes", "AMQP")
@enduml
```

# Component Diagram 

![component](https://github.com/user-attachments/assets/4c337d10-acb5-44eb-bd09-846a81350ee1)

```
@startuml "Component Diagram - Creator Features"
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Component.puml

LAYOUT_WITH_LEGEND()
title Component Diagram - Creator Features

Container_Boundary(creator, "Creator Features") {
    Component(upload, "Upload Manager", "Node.js", "Handles video uploads")
    Component(studio, "Creator Studio", "React", "Content management")
    Component(analytics, "Analytics Dashboard", "React", "Performance tracking")
    Component(monetize, "Monetization", "Node.js", "Revenue management")
    Component(community, "Community", "Node.js", "Audience interaction")
    Component(stream, "Live Streaming", "Node.js", "Broadcasting")
    
    Component(processor, "Upload Processor", "Python", "File processing")
    Component(thumbnail, "Thumbnail Generator", "Python", "Preview creation")
    Component(metadata, "Metadata Editor", "React", "Video information")
    Component(playlist, "Playlist Manager", "React", "Content organization")
    Component(endscreen, "End Screen Editor", "React", "Outro editing")
    Component(cards, "Card Editor", "React", "Overlay management")
    
    Component(revenue, "Revenue Tracker", "Node.js", "Income monitoring")
    Component(ads, "Ad Manager", "Node.js", "Ad campaign management")
    Component(merch, "Merchandise Store", "Node.js", "Product sales")
    Component(membership, "Membership Manager", "Node.js", "Channel memberships")
    
    Component(chat, "Chat Manager", "Node.js", "Live chat")
    Component(polls, "Poll Manager", "Node.js", "Audience polls")
    Component(comments, "Comment Manager", "Node.js", "Moderation")
}

System_Ext(storage, "Cloud Storage", "Video storage")
System_Ext(cdn, "CDN", "Content delivery")
System_Ext(payment, "Payment Gateway", "Payments")
System_Ext(analytics_ext, "Analytics Engine", "Metrics")
System_Ext(ad_network, "Ad Network", "Ad serving")
System_Ext(stream_provider, "Stream Provider", "Live streaming")

Rel(upload, storage, "Stores videos", "API")
Rel(upload, processor, "Processes", "Internal")
Rel(processor, thumbnail, "Generates", "Internal")

Rel(studio, metadata, "Manages", "Internal")
Rel(studio, playlist, "Organizes", "Internal")
Rel(studio, endscreen, "Configures", "Internal")
Rel(studio, cards, "Sets up", "Internal")

Rel(monetize, revenue, "Tracks", "Internal")
Rel(monetize, ads, "Manages", "Internal")
Rel(monetize, merch, "Handles", "Internal")
Rel(monetize, membership, "Manages", "Internal")

Rel(community, chat, "Manages", "Internal")
Rel(community, polls, "Creates", "Internal")
Rel(community, comments, "Moderates", "Internal")

Rel(stream, stream_provider, "Uses", "RTMP")
Rel(monetize, payment, "Processes", "API")
Rel(analytics, analytics_ext, "Tracks", "API")
Rel(ads, ad_network, "Serves", "API")
@enduml
```




# Deployment Diagram 

![Screenshot_20241130_194016](https://github.com/user-attachments/assets/8f1ea1b0-b432-498f-abe9-6044f1324cf9)

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
    Deployment_Node(stream_cluster, "Streaming Cluster") {
        Container(stream_server, "Stream Server", "Node.js")
        Container(rtmp, "RTMP Server", "Nginx")
    }
    Deployment_Node(processing, "Processing Cluster") {
        Container(transcoder, "Transcoder", "FFmpeg")
        Container(thumb_gen, "Thumbnail Gen", "ImageMagick")
    }
    Deployment_Node(ml, "ML Cluster", "GPU") {
        Container(recommender, "Recommender", "TensorFlow")
        Container(moderator, "Moderator", "PyTorch")
    }
}

Deployment_Node(data, "Data Tier", "Storage") {
    Deployment_Node(db_cluster, "Database Cluster") {
        ContainerDb(postgres, "PostgreSQL", "Users")
        ContainerDb(mongo, "MongoDB", "Content")
        ContainerDb(elastic, "Elasticsearch", "Search")
        ContainerDb(redis, "Redis", "Cache")
        ContainerDb(clickhouse, "ClickHouse", "Analytics")
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



