# Video-Sharing-Platform
<p align="center">
<img src="https://i.imgur.com/fBz91za.png">
<p>

<h2> Architecture Goals </h2>

- Scalable (handles millions of users)
- Resilient (recover from failures)
- Cost-Optimized (no waste)
- Secure (safe content and user privacy)

<h2> Features </h2>

- Upload videos 
- Liking videos
- View videos
- Filter adult / inappropriate content

<h3> User Accesses the Website </h3> 

- This is the frontend (Presentation Layer) where users interact, view, upload, and like videos

  <h3> Uploading the Video </h3>

  - Videos are uploaded directly to cloud storage.
  - Why Object Storage? It's scalable, cost-efficient, and great for storing large media files.

 <h3> Store Metadata </h3>

 - DynamoDB (scalable)
 - Use NoSQL because it handles massive data input and scales well.

<h3> Processing Video </h3>

- Video is converted to multiple resolutions (240p, 720p, 1080p)
- This allows support across devices and internet speeds

<h3> Content Analysis </h3>

<h4>Runs in parallel with video processing:</h4>

- Frame-by-frame scanning of video
- Flags or blocks inappropriate content
- Updates metadata with content tags

<h3> Delivery Video Globally </h3>

<h4>Use a CDN (Content Delivery Network) like CloudFront</h4>

- Copies (caches) content in edge locations (closer to users)
- Reduces buffering and latency

<h3> First Time User </h3>

<h4> What Happens? </h4>

- Their device has no local cache, no app data yet.

The video, thumbnail, or metadata must be fetched from:

- The CDN edge server (if cached there)
- Or the origin storage (S3) if it's a cache miss

<h4> Caching Impact </h4>

- Slower experience (first hit = cold) 
- Triggers cache population for the next users in the same region 

<h3>Another User in the Same Region </h3>

<h4> What Happens? </h4>

If a previous user has already accessed the same video:

- CDN has cached it at a nearby edge location (CloudFront)

<h4> Caching Impact </h4>

- Faster load times
- Lower bandwidth costs
- No need to query the origin (S3) again




