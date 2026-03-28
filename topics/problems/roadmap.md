# Roadmap

1. Chat system
Core problems:

Real-time delivery
Ordering
Presence (online/offline)
Fanout (group chat)

Typical architecture:

WebSocket servers (persistent connections)
Message queue (buffer + async delivery)
Database (message storage)
Push notifications (offline users)

Key patterns inside:

Pub/Sub
Message queue
Event-driven
Idempotency (avoid duplicate messages)

2. News feed / timeline 
Core problems:

Fanout to followers
Huge read volume
Personalization

Two known approaches:

Fanout on write (push model)
Fanout on read (pull model)

Typical architecture:

Write → queue → fanout workers
Cache (precomputed timelines)
DB (posts)

Key patterns:

Queue + workers
Heavy caching
Sharding

3. File storage system
Core problems:

Store large files
Efficient download/upload
Metadata management

Typical architecture:

Blob storage (S3-like)
Metadata DB
Chunking (split files)
CDN for delivery

Key patterns:

Object storage
CDN
Chunking + parallel upload

4. Video streaming
Core problems:

Massive bandwidth
Global delivery
Encoding formats

Typical architecture:

Upload → processing pipeline (encoding)
Store in blob storage
CDN for streaming

Key patterns:

Async processing (queues)
CDN
Preprocessing (transcoding)

5. Ride-hailing
Core problems:

Real-time location updates
Matching drivers ↔ riders
Geo queries

Typical architecture:

Location service (in-memory / fast DB)
Matching service
Event streaming

Key patterns:

Pub/Sub (location updates)
Geospatial indexing
Real-time processing

6. Search system
Core problems:

Fast text search
Ranking
Indexing

Typical architecture:

Crawlers → indexing pipeline
Inverted index
Query service

Key patterns:

Indexing
Distributed search
Sharding

7. Notification system
Core problems:

Send at scale (email, SMS, push)
Retry on failure
Rate limiting

Typical architecture:

API → queue → workers → providers

Key patterns:

Queue
Retry + DLQ
Rate limiting

8. Job Scheduler / Background Processing
Core problems:

Delayed jobs
Retries
Execution guarantees

Typical architecture:

Scheduler → queue → workers

Key patterns:

Delay queues
Idempotency
Distributed workers

