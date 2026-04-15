# Desing a URL SHORTENER

Like Bitly,

this kind of software usually takes a long url and maps it to a special unique identifier which must be short of course:

If you pass:

google.com.mx -> abc123

If you see the frontend Idea is pretty simple

Now lets to do some assumptions about this system:

- The shortUrl must be unique
- the length of this short url is 8 chars
- We will assume 10M DAU
- 10% of the DAU shorten URL per day
- This is a read heavy system more than it is a write system so we expect the latency when redirecting to the long url is short
- Assuming the ratio of the read operation is 1:100

Back of the envelope:
1Kb = 2^10 = 1024      = ~10^3
1MB = 2^20 = 1,000,000 = 10^6
1GB = 2^30 = Billion   = 10^9
1TB = 2^40 = Trillion = 10^12
1PB = 2^50 = Quadrillion = 10^15

- 10M DAU
- 1M = ~10^6 shortened urls daily
- 100M = 10^8 reads per day
- Writes per second = 10^6 / 10^5 = 10 per second
- Reads per second = 10^8 / 10^5 = 10^2 = 1000 per second
- Assume the average URL len is 100 bytes

For the storage:
URL ~ 100bytes
ShortUrl = 8bytes
Metadata = ~50 bytes

~150 bytes per entry

10^6×150=150×10^6 bytes
= 1.5x10^8 = 150MB/day

A year

150 MB/day x 365 = 55GB/year

High level design:
