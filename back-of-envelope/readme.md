In system desing interview sometimes you are asked to estimate system capacity or performance
requirements using a back-of-the-envelope estimation


## Power of two

- 2^10 ~ Thousand = 1kb
- 2^20 ~ Million = 1mb
- 2^30 ~ Billion = 1gb
- 2^40 ~ Trillion = 1tb
- 2^50 ~ Quadrillion = 1pb

# Example

Estimate twitter QPS and storage requirements

Assumptions:
- 300 million montly active users
- 50% users use twitter daily
- Users post 2 tweets per day only
- 10% of tweets contain media
- Data is stored for 5 years


DAU: 300 million * 50% = 150 million

### Total Tweets Per Day

Given each active user posts 2 tweets/day:
Tweets/day=150 M×2=300 million tweets/day

### QPS for Tweet Creation

We convert tweets/day to tweets/second:
QPS=  300 M tweets/day / 86400s day
​
So ~3.5K writes/sec just for tweet creation.

### Read QPS (Timeline Fetch)
Reads are much higher than writes.
Let’s assume each DAU views their timeline 10 times/day (pull or push-based fetching):

Timeline reads/day=150 M×10=1.5 B/day

Read QPS= 1.5 B / 86400 ≈ 17,361 reads/sec

### Storage Needs 

Tweets/day = 300M

Retention = 5 years (~1825 days):

Total tweets=300 M/day×1825≈547.5 B tweets

If each tweet = 300 bytes (text only, excluding media):

Storage for tweets = 547.5 B × 300 B ≈ 164 TB

For media:
Assume 1 MB average per media tweet:
Media storage= 30 M/day×1 MB×1825≈54.75 PB


| Metric               | Value      |
| -------------------- | ---------- |
| DAU                  | 150M       |
| Tweets/day           | 300M       |
| Tweet write QPS      | \~3.5K     |
| Media upload QPS     | \~350      |
| Timeline read QPS    | \~17K      |
| 5-year text storage  | \~164 TB   |
| 5-year media storage | \~54.75 PB |
