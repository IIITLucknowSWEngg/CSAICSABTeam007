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
## Creator Features
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


## Viewer Features
![component](https://www.plantuml.com/plantuml/png/ZLHDR-Cs4BthLx0-9K0INthgQLCdkmdW94RiTD6JCIR74hiKQN16SRr5_tj9ob8OoGPwqb1uy_ZcW-yBCNXkQYCcCrVld4NBuav3wQ4Mr-8FZNlquYi2DnvfKloYhJIDGb4nx-YtwTJ3_gRKN3MRXj1BPnajtqXNJtS6R6I-9bP6RwQpNwyNyTVRqtnQ0p7wy6lTttgp2zYYcD_--VAsMdz_N3siv_VVxf_lBYuBrcnG_0-1HJ5pba5RzElVNMCL-CF5Uy9SYScmcalnJo64-22-g5oD0VWGFW66QX8EhmYIOx10Jyw2qJzHYMEbmcEejKQhWBMpayi1OMZ2uNYtGYSMwVY9zGcL1X5n6v0_HWJQCle03SZ7DeptY2UmKBPCpqxXpLyKmxd-WNGbMaKS8gh08ur8Uz59I5w0kBUbjfZAFN3bR8ncnrgfAKka21jJ4eANLG0lKo2Is2r8x8lDcPGcwTxH7yODyswhJHp26w4NY_PuSWw2a5dRiXCpODe3osfjW5F_lyUJc0CFsh-4zsoQHXEF95MQsFb3J_BGdWUzZykLCR4FqmpYJcv4l8PY9SSu6sVMguROrJsXTCPW6lwPUgdPaFHwbo2niknyPbzc5LYBHcm3jzkVKciTwwsMq94-PsSn2uWHvzjE0IC9i4g0GSzdv7PB6cMqeLWUYB4UyDrfaWrHlAzEHKJeYOs6jZTT1oD-5MTm-f4aUTtREjzJvVRME51Hnp4yqygNxTPeYzsuHW91IYJI6sqqnrMwpS-z9UKB40UOZ8-kUdj8zDwPAFVVecWRjhx_ExHJsTZ9k-TFpmoDRbzP3WKBvi1QKfJHnTd3ZmyltzyijJRHiK1MiTGvWhSHCdOHIYaXjtX5StJLt7I-8iiAwPEZT0vdmOJ4Zxosiha3HNrteqUlLejbTqDdiorsoHwX9l4UFJV7vj31G6BFjhfiF0wLnApCXlDDIwimJ1ZWipr_0AlCQVxEFbE3lo6Jy61Z_nFwTl7O0Nj__72bjEPZ4Hasjy9SzTc4tAHUV0bM_pofUsWwGxjOUHq6x8UfsNJpPMmBoPzbLdq-kppf5NVECuc6eCGF-1UqggdDVm00)
```
@startuml "Component Diagram - Viewer Features"
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
    
    Component(download, "Download Manager", "Node.js", "Offline content")
    Component(accessibility, "Accessibility Features", "React", "Subtitles, playback controls")
}

System_Ext(cdn, "CDN", "Content delivery")
System_Ext(analytics, "Analytics Engine", "User tracking")
System_Ext(ml, "Machine Learning", "Recommendation system")

Rel(home, recommend, "Fetches suggestions", "Internal")
Rel(home, cdn, "Loads content", "HTTPS")

Rel(player, cdn, "Streams video", "HTTPS")
Rel(player, accessibility, "Configures", "Internal")

Rel(interact, reactions, "Manages", "Internal")
Rel(interact, comments, "Handles", "Internal")

Rel(search, ml, "Gets rankings", "API")

Rel(profile, history, "Tracks", "Internal")
Rel(profile, watch_later, "Manages", "Internal")
Rel(profile, playlists, "Organizes", "Internal")

Rel(subscriptions, notifications, "Triggers", "Internal")

Rel(home, analytics, "Tracks interactions", "API")
Rel(player, analytics, "Reports usage", "API")
@enduml
```


## Advertiser Features
![component](https://www.plantuml.com/plantuml/png/ZLHDJoCv4xxdLzIR4qWlSDdJdYOa38i4J4HWLdjgLTf5npl-QBcgOQBL_lUrswGndPxHtEoowwcFf-hvn891EcjWllIszOwSm4fZ4z326IpK2mNHJ04-4qeNYEUp_sbNcquHx4HQ_ltY8k3hUQDbrsqxfb1x9n7blFRseZNe4lePYp9wUx7yxMoTJ4ztjnSMMIX4KpL4Fc_ZtzdiTl7NbwV7wi-Rnp-gswlhg_lLoUbCj1Y2NqnoDbjw9wWTXUhITqvXs9_WyE__C9-gx1J-cG70Uv2J6csBkd6LHOTDxxXyCy5TDisZxTuhEl-Rvpqk79oW3eIYlPkVZc05GqEYNPCSEgN9rGIF1sE2MUzbvrqwhSZwM6YxqpKm6Qed4RUTYixn-sL_W8KnlaRnu_GsBJaLOq1YANXp3AHLUi-LNEOBRFOiPCULTdqga5jZe_CRs0WnT-65AfPOgayjVBF0fhSai0V2MWxzomC4YbWtnwMYghhMU5JvzuAPv0TaVDMAFA1Je6ryXkp7ntoGRGqAPSJ7jnlSwct0iF-GtphGMKwC51ZS-qvukcvqQFQYQyweQmhFFbXCLA-GTrkFGHNGnMHf5_5tSfod9GaPy2wVu35W_Ot2U6G4jI45RH6pJuVrCMXizmi5rjwb78PB2LsqzE7B3LX2xi9FADzgO_8GNURJz0IjSPyme0s-9kRuxoWvxLwyhZFMpU4yWlciNQnGeu50hGyoXTFcK9Mbs2HLnBxh3UNyP3wAt5NVpX-mNCUJqQd3cURbS8VbZewvMNkZwpt49kjdNK_B0tTjgkEZIcsoSLJxicFnbW8WisP9SIFSlxDPRdXrzVszzWQ5Nd5V57-TBH-RA04TOx_RiUOI25NbI5vz-9PhlS-NURufpHBqjiibbNxMLANYtTs-QnriABpec7IXXCFyboZ5FbNqFVKcsYX46omE3mcaN3D94vodOVP0PaBDoxHMzAmTCU1R6lD-v7ACY30DCEZkXeJ1QAk5VyrnqDayUfmiOzViEzRFKW2Vig8b-cawyjYxqBed9Xxkax6B_en8l2RXWQYUlVNDmQjGldV5MZHDe2O50eM24waEVyTCfyt0G5pgsIZae3YZHH_QEuvN-fGYSqsEGfzb5fEVEaviLfOouiDRsPlfIVYW85_HQ3N1nhJhXuLS7VQFE3wCuXOgNvBoa6k4gEgZW9_8gSwQ_m00)
```
@startuml "Component Diagram - Advertiser Features"
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Component.puml

LAYOUT_WITH_LEGEND()
title Component Diagram - Advertiser Features

Container_Boundary(advertiser, "Advertiser Features") {
    Component(campaign_manager, "Campaign Manager", "Node.js", "Ad campaign creation")
    Component(targeting, "Audience Targeting", "Python", "Demographic selection")
    Component(budget, "Budget Allocator", "Node.js", "Spending control")
    Component(bidding, "Ad Bidding System", "Node.js", "Auction management")
    
    Component(creative_studio, "Creative Studio", "React", "Ad content design")
    Component(ad_upload, "Ad Asset Manager", "Node.js", "Video and image uploads")
    Component(template, "Ad Template Library", "React", "Pre-designed layouts")
    
    Component(analytics, "Ad Performance Dashboard", "React", "Campaign insights")
    Component(metrics, "Metrics Tracker", "Python", "Detailed performance analysis")
    Component(conversion, "Conversion Tracker", "Node.js", "ROI measurement")
    
    Component(billing, "Billing System", "Node.js", "Payment processing")
    Component(invoicing, "Invoicing", "Node.js", "Financial reporting")
    Component(payment_method, "Payment Method Manager", "React", "Billing details")
    
    Component(compliance, "Ad Compliance Checker", "Python", "Policy verification")
    Component(support, "Advertiser Support", "Node.js", "Customer assistance")
}

System_Ext(payment_gateway, "Payment Gateway", "Financial transactions")
System_Ext(ad_network, "Ad Network", "Ad distribution")
System_Ext(ml_targeting, "ML Targeting Service", "Audience insights")
System_Ext(analytics_ext, "External Analytics", "Performance tracking")

Rel(campaign_manager, targeting, "Defines audience", "Internal")
Rel(campaign_manager, budget, "Sets limits", "Internal")
Rel(campaign_manager, bidding, "Manages bids", "Internal")

Rel(creative_studio, ad_upload, "Uploads assets", "Internal")
Rel(creative_studio, template, "Uses templates", "Internal")

Rel(targeting, ml_targeting, "Gets insights", "API")

Rel(analytics, metrics, "Aggregates data", "Internal")
Rel(metrics, analytics_ext, "Compares performance", "API")

Rel(billing, payment_method, "Manages", "Internal")
Rel(billing, invoicing, "Generates reports", "Internal")
Rel(billing, payment_gateway, "Processes payments", "API")

Rel(campaign_manager, compliance, "Validates", "Internal")
Rel(campaign_manager, ad_network, "Distributes ads", "API")

Rel(conversion, metrics, "Reports ROI", "Internal")
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



