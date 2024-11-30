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


# Deployment Diagram Code

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
