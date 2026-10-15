# 09 - Global Infrastructure Section


## 1. Why Build a Global Application?

A **global application** is an application deployed in more than one
geographical location. On AWS, these locations can include different
**Regions** and/or **Edge Locations**.

Imagine you built a student portal and your server is only in the USA.
Students in India and Australia must send their requests to the USA. The
longer network distance can make the website feel slow.

### Main reasons

**1. Lower latency**

-   **Latency** means the time taken for data to travel from one point
    to another.
-   A user far away from the server usually experiences more network
    delay.
-   Deploying the application closer to users, or serving cached content
    from an edge location, can reduce delay.

**Example:** A student in Pune opens a page hosted on a server in the
USA. The request travels a long distance. If the application is also
deployed in an appropriate nearby Region, or static images are cached at
a nearby CloudFront edge location, the user may get a faster response.

**2. Disaster Recovery (DR)**

Disaster Recovery means preparing a way to continue service if a major
failure happens.

**Example:** Your application runs in Region A. If that Region has a
serious outage, a properly designed DR setup can send users to a working
deployment in Region B. This requires planning and configuration; a
second Region by itself does not automatically create failover.

**3. Better resilience and attack protection**

A distributed infrastructure can help an application tolerate some
failures and can work with AWS protection services. It does not mean an
application becomes impossible to attack. Security controls are still
required.

### Remember

-   Global deployment can improve user experience and disaster recovery.
-   More locations also add design, data synchronization, testing, and
    cost complexity.
-   Choose the architecture based on user location, availability needs,
    data rules, and budget.

------------------------------------------------------------------------

## 2. AWS Global Infrastructure

The slides introduce three important building blocks:

### A. AWS Region

A **Region** is a separate geographical area where AWS has
infrastructure. Region names look like `us-east-1` or `eu-west-3`.

**Use a Region when:** You need to choose where to run resources such as
EC2, databases, or other regional services.

**Example:** A college app mainly used by students in India might
consider an AWS Region in India, subject to service availability, data
requirements, performance, and price.

### B. Availability Zone (AZ)

An **Availability Zone** is an isolated location inside a Region. An AZ
consists of one or more data centers with independent, redundant power
and networking. AZs in a Region are connected using high-bandwidth,
low-latency networking.

**Why it matters:** If an application is designed to run across multiple
AZs, it can continue working through some individual AZ failures.

**Example:** Run the web application on EC2 in AZ-A and another EC2
instance in AZ-B, with a load balancer distributing requests. If AZ-A
has a problem, the healthy deployment in AZ-B may continue serving users
if the architecture and health checks are configured correctly.

### C. Edge Location (Point of Presence)

An **Edge Location** is a point in AWS's global network used by services
such as CloudFront to deliver content closer to users.

**Why it matters:** A user can receive cached content from a nearby edge
location instead of downloading the same file from a distant origin
every time.

**Example:** Your original image is stored in an S3 bucket. A student in
Pune requests it through CloudFront. If the image is cached at a nearby
edge location, CloudFront can return that cached copy.

### Easy comparison

  -----------------------------------------------------------------------
  Component               Think of it as          Main purpose
  ----------------------- ----------------------- -----------------------
  Region                  A geographical AWS area Choose where to deploy
                                                  application resources

  Availability Zone       An isolated location    Improve availability
                          inside a Region         and fault tolerance

  Edge Location           A nearby                Deliver cached content
                          content-delivery point  with lower latency
  -----------------------------------------------------------------------

**Memory trick:** Region = where you deploy; AZ = where you spread
resources for resilience; Edge = where content can be delivered closer
to users.

------------------------------------------------------------------------

## 3. Amazon Route 53

**Amazon Route 53** is AWS's managed **DNS (Domain Name System)**
service.

### What does DNS do?

People prefer names such as `www.example.com`; computers communicate
using IP addresses. DNS helps translate a domain name into the
information needed to reach the correct destination.

**Simple flow:**

1.  You type `www.example.com` in a browser.
2.  The browser/device needs DNS information for that name.
3.  Route 53 answers according to the configured DNS record and routing
    policy.
4.  The browser connects to the selected endpoint.

DNS selection does not itself host the website. The application or
content must be available at the destination.

### Common DNS records in the slides

-   **A record:** Maps a name to an IPv4 address.
-   **AAAA record:** Maps a name to an IPv6 address.
-   **CNAME record:** Maps one hostname to another hostname.
-   **Alias record (Route 53-specific feature):** Points a supported DNS
    name to an AWS resource such as a CloudFront distribution or load
    balancer. Alias records can be used at the zone apex, such as
    `example.com`, where a regular CNAME is not allowed.

### Route 53 routing policies

The slides say these need to be understood at a high level for the Cloud
Practitioner exam.

#### 1. Simple routing

Use simple routing when you want a straightforward DNS answer for a
resource.

-   The slide describes simple routing as having no health checks.
-   It is suitable for a basic setup that does not need policy-based
    selection among multiple destinations.

**Example:** `app.example.com` points to one application endpoint.

#### 2. Weighted routing

Weighted routing distributes DNS responses among records according to
configured weights. Weights are relative, not necessarily exact
guarantees of request percentages.

**Example:** You want to send most DNS traffic to a new version while
testing it: - Version A: weight 70 - Version B: weight 20 - Version C:
weight 10

This represents a 70:20:10 weighting for eligible DNS responses, not a
promise that exactly 70%, 20%, and 10% of all application requests will
arrive at each version. DNS caching and client behavior can affect
actual traffic.

**Use it for:** Controlled traffic distribution and gradual rollout.

#### 3. Latency-based routing

Latency routing directs users to the AWS endpoint expected to provide
the lowest network latency based on AWS latency measurements.

**Example:** Your app has deployments in Europe and the USA. A user in
Europe may be routed to the European endpoint, while a user in the USA
may be routed to the US endpoint, when those endpoints are configured
and available.

**Important:** It does not mean Route 53 physically moves your
application. You must deploy the application in the locations you want
to serve.

#### 4. Failover routing

Failover routing supports a primary/secondary design. Health checks can
help Route 53 stop returning the primary endpoint when it is considered
unhealthy and return the secondary endpoint instead.

**Example:** - Primary: application in Region A. - Secondary: standby
application in Region B. - If the primary fails and the
health-check/failover setup detects it, DNS can direct new lookups to
the secondary.

**Important:** DNS failover is not instant for every user because DNS
answers may be cached. The backup must also be ready and contain the
data/configuration needed to serve traffic.

### Route 53 exam clue

-   "Route users to the deployment with the lowest latency" → **Latency
    routing**
-   "Send traffic using relative weights" → **Weighted routing**
-   "Switch from primary to secondary during failure" → **Failover
    routing**
-   "Basic DNS answer" → **Simple routing**

------------------------------------------------------------------------

## 4. Amazon CloudFront

**Amazon CloudFront** is AWS's **Content Delivery Network (CDN)**.

A CDN delivers content from locations closer to users. CloudFront uses
AWS edge locations and caches eligible content to improve read
performance.

### What is caching?

Caching means temporarily keeping a copy of content so it can be served
again without fetching it from the original source every time.

**Example: college website**

Your website has: - `logo.png` - `college-building.jpg` - CSS and
JavaScript files - a student dashboard that shows personal data

The logo, building photo, and versioned static files may be good
candidates for caching. Personal dashboard information may need
different caching rules or no shared caching at all.

### How CloudFront works: step by step

1.  A user requests `https://www.example.com/beach.jpg`.
2.  The request reaches a CloudFront edge location.
3.  CloudFront checks whether a valid cached copy exists.
4.  **Cache hit:** If the cached copy is still valid, CloudFront returns
    it.
5.  **Cache miss:** If no valid copy exists, CloudFront forwards the
    request to the configured **origin**.
6.  The origin returns the content.
7.  CloudFront sends the content to the user and may cache it according
    to the cache settings.

A cached object remains for a configured period called **TTL (Time to
Live)**, subject to the cache policy and invalidation/expiration
behavior.

### Benefits

-   Lower latency for cached content.
-   Reduced repeated requests to the origin.
-   Better user experience for static assets and other cacheable
    content.
-   Integration with AWS security services such as AWS Shield and AWS
    WAF, when configured appropriately.

### When should you use CloudFront?

Use it when many users in different places need content such as images,
videos, downloads, or web assets. It can also sit in front of dynamic
applications, but caching behavior must be designed carefully.

**Important:** CloudFront is not the same as copying your whole
application database to every edge location. It mainly caches and
delivers content; requests that cannot be served from cache can still go
to the origin.

------------------------------------------------------------------------

## 5. CloudFront Origins and Request Flow

An **origin** is the source from which CloudFront obtains content when
it cannot serve a request from cache.

The slides list three origin categories.

### A. Amazon S3 bucket as origin

Use an S3 origin to distribute files such as images, downloads, and
static website assets.

-   CloudFront can cache eligible objects from S3.
-   For a private S3 bucket, use **Origin Access Control (OAC)** with an
    appropriate bucket policy so users access content through CloudFront
    rather than making the bucket public.
-   CloudFront can also be configured to send uploads to an S3 origin
    where that use case and permissions are set up.

**Example:** - Original `logo.png` is stored in S3. - CloudFront is
configured with that bucket as its origin. - A user requests the logo. -
If the object is cached, CloudFront returns the cached copy. -
Otherwise, CloudFront retrieves it from S3 and may cache it.

### B. VPC origin

The slides describe VPC origins for applications in VPC private subnets,
including supported private Application Load Balancers, Network Load
Balancers, or EC2 instances.

**Why useful?** You can put a private application behind CloudFront
without making the origin directly public, subject to supported
configurations and networking/security requirements.

**Example:** A company keeps its application load balancer in private
infrastructure and uses CloudFront as the public entry point. The
CloudFront-to-origin configuration must be set up correctly.

### C. Custom HTTP origin

A custom HTTP origin is a publicly reachable HTTP backend, such as a
public Application Load Balancer. The slides also list an S3 static
website endpoint as a custom origin; for that case, the bucket must
first be configured for static website hosting.

**Example:** Your Node.js website runs behind a public Application Load
Balancer. You configure the ALB as CloudFront's custom origin. Users
request the website through CloudFront; a cache miss or a request that
must go to the backend is forwarded to the ALB.

### What happens to a request?

``` text
User's browser
      |
      v
CloudFront Edge Location
      |
      +-- Cache hit --> Return cached content to user
      |
      +-- Cache miss / forward request
                    |
                    v
             Configured Origin
             (S3 / private VPC origin /
              public HTTP backend)
                    |
                    v
             Response returns through
             CloudFront to the user
```

### Important distinction

-   **Edge location:** Helps serve content close to the user.
-   **Origin:** The source that provides content when CloudFront needs
    to fetch it.
-   **CloudFront does not automatically make a private origin public.**
    Access and origin configuration still matter.

------------------------------------------------------------------------

## 6. CloudFront vs S3 Cross-Region Replication

These services solve different problems.

### CloudFront

-   Uses a global edge network.
-   Caches content for a configured TTL.
-   Good for distributing content to users around the world.
-   A cached copy is not the same as a separately managed, permanent
    replica of the S3 bucket.

### S3 Cross-Region Replication (CRR)

-   Copies objects from a source S3 bucket to a destination bucket in
    another Region when replication is configured.
-   Replication rules must be set up for the desired destination.
-   Useful when data needs to exist in more than one Region, including
    for regional access or disaster-recovery designs.
-   Replication is asynchronous; do not assume every object is copied
    instantly.
-   Replicated objects can be read from the destination bucket. Writes
    and data management must be designed according to the replication
    setup.

### Example

You store product images in an S3 bucket in one Region.

-   If your main goal is to serve images quickly to users worldwide,
    choose **CloudFront**.
-   If your goal is to maintain copies of objects in another Region,
    choose **S3 Cross-Region Replication**.
-   A real architecture may use both: CRR for regional copies and
    CloudFront for delivery to users.

  -----------------------------------------------------------------------
  Question                            Better fit
  ----------------------------------- -----------------------------------
  "Serve cached images quickly        CloudFront
  worldwide"                          

  "Keep S3 object copies in another   S3 Cross-Region Replication
  Region"                             

  "Need regional copies and global    Potentially both
  content delivery"                   
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## 7. S3 Transfer Acceleration

**S3 Transfer Acceleration** can speed up transfers to and from an S3
bucket by allowing a user to transfer data through a nearby AWS edge
location, then use the AWS network toward the bucket's Region.

### Flow

``` text
User far from the bucket
        |
        v
Nearby AWS Edge Location
        |
        v
AWS network toward the bucket's Region
        |
        v
Destination S3 bucket
```

### Example

A video team in the USA regularly uploads large files to an S3 bucket in
Australia. A transfer path through a nearby edge location may improve
transfer performance, depending on network conditions and the user's
location.

### Key points

-   It is for **faster transfers to/from S3**, especially across long
    distances.
-   It does not mean that the S3 bucket itself is moved to the edge
    location.
-   Performance improvements depend on the route and conditions;
    acceleration is not guaranteed for every transfer.
-   Check the current AWS pricing and test performance before enabling
    it for a real workload.

**Exam clue:** "Speed up global uploads/downloads to an S3 bucket" →
**S3 Transfer Acceleration**.

------------------------------------------------------------------------

## 8. AWS Global Accelerator

**AWS Global Accelerator** improves global application availability and
network performance by using the AWS global network.

The slides describe two **Anycast static IP addresses** for the
accelerator. Traffic enters the AWS network at an edge location and is
routed toward configured application endpoints in AWS Regions.

### What is Anycast, in simple words?

With Anycast, the same IP address can be announced from multiple
locations. Network routing directs the user to an appropriate nearby
entry point. Global Accelerator then routes traffic across the AWS
global network toward the configured endpoint.

### Example: global online game

Suppose a game backend has endpoints in India and Europe. Players need
to exchange live game data with the backend.

1.  The player connects to the accelerator's static IP.
2.  Traffic enters AWS at an edge location.
3.  AWS routes the traffic over its network toward a configured healthy
    endpoint.
4.  Global Accelerator can direct traffic to another healthy endpoint if
    the setup and health checks support failover.

### Why use it?

-   Provides static IP addresses as a fixed entry point.
-   Can improve network paths for TCP/UDP applications.
-   Supports health-based endpoint selection and regional failover when
    configured.
-   Does **not** cache application files.

**Important:** Global Accelerator does not automatically deploy your app
in multiple Regions. You configure the endpoints and build the
multi-Region design you need.

------------------------------------------------------------------------

## 9. Global Accelerator vs CloudFront

Both use AWS's global network and edge locations, but they work
differently.

  -----------------------------------------------------------------------
  Feature                 CloudFront              AWS Global Accelerator
  ----------------------- ----------------------- -----------------------
  Main purpose            CDN for delivering      Network entry and
                          content                 routing to application
                                                  endpoints

  Caching                 Yes, for eligible       No
                          content                 

  Where response content  Edge cache on a cache   Application endpoint;
  comes from              hit; origin on a        accelerator forwards
                          miss/forwarded request  traffic rather than
                                                  serving cached files

  Typical examples        Images, video, static   TCP/UDP applications,
                          files, web assets       games, APIs,
                                                  applications needing
                                                  static entry IPs

  Static anycast IPs      Not its main purpose    Provides two static
                                                  Anycast IPv4 addresses
                                                  per accelerator

  Regional failover       Can work with an        Supports endpoint
                          architecture using      health and failover
                          multiple origins and    behavior when
                          suitable configuration  configured
  -----------------------------------------------------------------------

### Easy way to remember

-   **CloudFront = cache and deliver content near the user.**
-   **Global Accelerator = route network traffic over AWS's global
    network toward the application.**

### Scenario examples

**Scenario A:** Users worldwide repeatedly download the same product
images.

Choose **CloudFront**, because caching can serve images from the edge.

**Scenario B:** A multiplayer game uses TCP/UDP traffic and needs fixed
IP addresses and quick regional failover.

Consider **AWS Global Accelerator**.

**Scenario C:** Your website needs both fast delivery of static images
and reliable routing to a dynamic backend.

CloudFront and Global Accelerator may both be useful, depending on the
architecture. They are not interchangeable.

------------------------------------------------------------------------

## 10. AWS Outposts

**AWS Outposts** extends selected AWS infrastructure and services into a
customer's own on-premises data center.

This is a **hybrid cloud** approach: part of the IT environment runs in
the customer's facility and part runs in AWS Regions.

### How it works

1.  An organization has its own data center.
2.  AWS Outposts racks are installed in that facility.
3.  The organization can run supported AWS services locally using
    familiar AWS tools and APIs.
4.  The Outposts environment connects to its parent AWS Region for AWS
    management and service integration.

AWS manages the Outposts infrastructure service, while the customer is
responsible for physical security of the Outposts racks in its facility,
as described in the slides.

### Example

A factory has equipment that needs very fast access to a local
application, and certain data must remain on-site. The factory can use
Outposts to run supported AWS services in its own data center rather
than sending every local operation to a distant Region.

### Benefits in the slides

-   Low-latency access to on-premises systems.
-   Local data processing.
-   Data residency needs.
-   Easier migration for some workloads.
-   Managed AWS infrastructure experience.

The slides list examples of supported services such as EC2, EBS, S3,
EKS, ECS, RDS, and EMR. Actual availability depends on the Outposts
offering and current AWS support.

### When to think of Outposts

Think **"AWS infrastructure in my own data center."**

It is not the same as an Edge Location. An Edge Location is part of
AWS's content-delivery network; Outposts is infrastructure deployed at
the customer's premises.

------------------------------------------------------------------------

## 11. AWS Wavelength

**AWS Wavelength** places selected AWS infrastructure in
telecommunications providers' facilities at the edge of 5G networks.
These deployments are called **Wavelength Zones**.

### Why?

Some applications need very low latency and large amounts of data must
move between the mobile device and the application. Bringing compute
closer to the 5G network can reduce the distance the traffic travels.

### Example: connected vehicle

A connected vehicle sends data through a 5G network to an application
that needs to respond quickly. With a suitable Wavelength architecture,
the application can run closer to the carrier's network instead of
relying on a distant Region for every interaction.

### Use cases from the slides

-   Smart cities.
-   Machine-learning-assisted diagnostics.
-   Connected vehicles.
-   Interactive live video.
-   Augmented reality / virtual reality (AR/VR).
-   Real-time gaming.

### Key idea

Think **"AWS services close to the 5G mobile network."**

Wavelength is not the same as Local Zones: Wavelength is specifically
placed at the edge of telecom 5G networks.

------------------------------------------------------------------------

## 12. AWS Local Zones

**AWS Local Zones** place selected AWS services---such as compute,
storage, and databases---closer to large population centers or end users
for latency-sensitive applications.

The slides describe Local Zones as an **extension of an AWS Region**. A
VPC can be extended to supported Local Zones.

### Example

A company has its main AWS Region in Northern Virginia, but it has users
in a nearby city who need a latency-sensitive application. A Local Zone
in that city may allow selected application resources to run closer to
those users.

The slides give examples of Local Zones around the Northern Virginia
Region, such as Boston, Chicago, Dallas, Houston, and Miami. These
examples are from the course slides; locations and service availability
can change over time.

### When to use it

-   A user-facing application is sensitive to network delay.
-   Selected compute or database resources need to be closer to users.
-   The application benefits from running in a Local Zone connected to a
    parent Region.

### Key idea

Think **"Selected AWS resources closer to a city or user group."**

Local Zones are not a substitute for every Region or every Availability
Zone. They provide selected services in additional locations.

------------------------------------------------------------------------

## 13. Global Application Architectures

The slides compare four architecture patterns. Each pattern trades off
availability, global latency, and complexity.

### A. Single Region, Single AZ

``` text
Users
  |
  v
One AWS Region
  |
  v
One Availability Zone
  |
  v
Application instance
```

**Advantages:** Simplest architecture and often easiest to learn or
test.

**Disadvantages:** An AZ failure can interrupt the application. Users
far from the Region may experience higher latency.

**Use for:** Learning, prototypes, or workloads that can accept limited
availability (subject to business needs).

### B. Single Region, Multi-AZ

``` text
Users
  |
  v
One AWS Region
  |
  +---- AZ-A: Application instance
  |
  +---- AZ-B: Application instance
```

**Advantages:** Better resilience against an individual AZ failure when
the app, load balancer, and data layer are designed for it.

**Limitations:** The application is still deployed in one Region, so it
does not provide the same protection against a full Region outage as a
multi-Region design. It may not reduce latency for users on another
continent.

**Use for:** A production application that needs high availability
within a Region.

### C. Multi-Region, Active-Passive

``` text
Users
  |
  v
Primary Region (ACTIVE)
  |
  |  Failover when needed
  v
Secondary Region (PASSIVE / STANDBY)
```

The primary Region normally handles production traffic. The secondary
Region is prepared to take over if the primary fails.

**Advantages:** Can provide disaster recovery across Regions.

**Trade-offs:** Requires data replication, recovery procedures, health
checks, testing, and potentially more time to scale or activate the
standby. Cost depends on how much standby capacity is running.

**Use for:** Applications that need regional disaster recovery but do
not need every Region to handle normal traffic.

### D. Multi-Region, Active-Active

``` text
              Users
                |
        Global routing layer
           /          \
          v            v
     Region A        Region B
      ACTIVE          ACTIVE
```

Both Regions serve traffic during normal operation.

**Advantages:** Users can be served from a closer deployment, and the
application may continue operating if one Region fails, depending on the
design.

**Trade-offs:** More complex. Data consistency, writes, conflict
handling, deployment, monitoring, and failover need careful design.

**Use for:** Applications that require global performance and high
availability and can justify the extra complexity and cost.

### Architecture comparison

  -----------------------------------------------------------------------
  Architecture      Availability /    Global latency    Complexity
                    resilience                          
  ----------------- ----------------- ----------------- -----------------
  Single Region,    Lowest of these   Depends on user   Lowest
  Single AZ         patterns          distance          

  Single Region,    Better protection Still mainly one  Medium
  Multi-AZ          against an AZ     Region            
                    failure                             

  Multi-Region,     Regional DR with  Reads may be      High
  Active-Passive    standby           served globally   
                                      if designed for   
                                      it; standby may   
                                      not serve normal  
                                      traffic           

  Multi-Region,     Can serve users   Can provide low   Highest
  Active-Active     from multiple     latency for users 
                    active Regions    near a deployment 
  -----------------------------------------------------------------------

**Remember:** Multi-AZ is about resilience within one Region.
Multi-Region is about operating across separate Regions, often for
global performance or regional disaster recovery. Neither pattern is
automatically highly available without correct configuration and
testing.

------------------------------------------------------------------------

## 14. Quick Service Selection Guide

  -----------------------------------------------------------------------
  Requirement in a        Service / design to     Why
  question                consider                
  ----------------------- ----------------------- -----------------------
  Route DNS users to an   Route 53 latency        DNS routing based on
  endpoint with lowest    routing                 expected network
  latency                                         latency

  Distribute DNS answers  Route 53 weighted       Weight-based DNS
  using relative weights  routing                 selection

  Switch DNS traffic from Route 53 failover       Supports
  primary to secondary    routing                 disaster-recovery
                                                  routing

  Deliver cached          CloudFront              CDN with edge caching
  images/videos/files                             
  worldwide                                       

  Speed up global         S3 Transfer             Uses an edge location
  uploads/downloads to S3 Acceleration            and AWS network path

  Route TCP/UDP           AWS Global Accelerator  Anycast entry points
  application traffic                             and traffic routing
  using static IPs                                

  Keep S3 object copies   S3 Cross-Region         Replicates objects
  in a different Region   Replication             between buckets

  Run AWS infrastructure  AWS Outposts            On-premises extension
  in a customer's data                            of selected AWS
  center                                          services

  Bring AWS services      AWS Wavelength          Wavelength Zones at
  close to a 5G carrier                           telecom network edge
  network                                         

  Bring selected AWS      AWS Local Zones         Extends a parent Region
  services closer to a                            to additional locations
  city                                            

  Improve resilience      Multi-AZ architecture   Application resources
  against one AZ failing                          span AZs

  Prepare for a Region    Multi-Region DR         A separate Region can
  outage                  architecture            provide recovery
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## 15. Exam Revision Questions

Try to answer before checking the answers.

### Questions

1.  What is the difference between an AWS Region and an Availability
    Zone?
2.  Which AWS service is a Content Delivery Network (CDN)?
3.  A user wants to serve cached images from locations close to users
    around the world. Which service fits best?
4.  Which Route 53 routing policy sends users to an endpoint based on
    lowest expected latency?
5.  Which Route 53 policy uses relative weights to distribute DNS
    responses?
6.  A company needs DNS failover from a primary endpoint to a secondary
    endpoint. Which policy should it consider?
7.  A team needs faster global transfers to an S3 bucket. Which feature
    should it consider?
8.  Does Global Accelerator cache images at edge locations?
9.  A game needs TCP/UDP traffic routing and fixed entry IP addresses.
    Which service is a good candidate?
10. Which service extends selected AWS infrastructure into a customer's
    own data center?
11. Which AWS service brings selected services close to the edge of a
    telecom provider's 5G network?
12. Which service brings selected compute/storage/database services
    closer to users in a city?
13. What is the main difference between CloudFront and S3 Cross-Region
    Replication?
14. What is the main difference between Multi-AZ and Multi-Region
    architecture?
15. Which architecture normally has both Regions actively serving
    traffic: active-passive or active-active?

### Answers with short explanations

1.  A Region is a geographical AWS area; an AZ is an isolated location
    inside a Region.
2.  Amazon CloudFront.
3.  CloudFront, because it can cache and serve eligible content at edge
    locations.
4.  Route 53 latency-based routing.
5.  Route 53 weighted routing.
6.  Route 53 failover routing, with appropriate health-check and
    endpoint configuration.
7.  S3 Transfer Acceleration.
8.  No. Global Accelerator forwards traffic to application endpoints;
    CloudFront is the caching CDN.
9.  AWS Global Accelerator.
10. AWS Outposts.
11. AWS Wavelength.
12. AWS Local Zones.
13. CloudFront caches content for delivery; Cross-Region Replication
    copies S3 objects to another Region.
14. Multi-AZ spreads resources across AZs in one Region; Multi-Region
    uses separate Regions.
15. Active-active.

------------------------------------------------------------------------

## Final Memory Sheet

-   **Region:** Geographical AWS area for deploying resources.
-   **Availability Zone:** Isolated location inside a Region; helps with
    resilience.
-   **Edge Location:** Nearby point used for edge delivery and caching.
-   **Route 53:** Managed DNS; routing policies include simple,
    weighted, latency, and failover.
-   **CloudFront:** CDN; caches eligible content near users.
-   **CloudFront origin:** The source CloudFront fetches from when it
    cannot serve a request from cache.
-   **S3 Cross-Region Replication:** Copies S3 objects to another
    Region.
-   **S3 Transfer Acceleration:** Speeds up transfers to/from S3 using
    edge entry and the AWS network.
-   **Global Accelerator:** Routes application traffic over the AWS
    global network; no caching.
-   **Outposts:** Selected AWS infrastructure at your own data center.
-   **Wavelength:** AWS services close to telecom 5G networks.
-   **Local Zones:** Selected AWS services closer to a city or end
    users.
-   **Multi-AZ:** Resilience within one Region.
-   **Multi-Region active-passive:** One Region normally active, another
    ready for recovery.
-   **Multi-Region active-active:** Multiple Regions serve traffic.

