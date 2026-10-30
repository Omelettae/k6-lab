# K6 lab results
## Part 0
- Step 0.1, http_req_duration: avg=245.58ms min=245.58ms med=245.58ms max=245.58ms p(90)=245.58ms p(95)=245.58ms
- Step 0.2, requests, rate and p(95), and why the rate stays below 5: Each virtual user sends one request per iteration, and an iteration lasts about 1.29 s. So one user sends about 0.78 requests/s x 5 users = 3.9 requests/s which is below 5.
- Step 0.6, exit code of the normal run and of the run with P95=300: Normal: 0, P95=300: 99
- Step 0.7, rules that passed, p(95), p(99), exit code after your change: 
    ✓ 'p(95)<1000' p(95)=363.37ms 
    ✓ 'p(99)<1500' p(99)=369.84ms
    After change:
    ✗ 'p(95)<0' p(95)=374.13ms
    exit code: 99
- Step 0.8, what was wrong, and how the checks showed it:
    ✗ no pepperoni
      ↳  89% — ✓ 67 / ✗ 8
    ✗ no knife needed
      ↳  74% — ✓ 56 / ✗ 19
    the problem was lowercase name ('pepperoni', 'knife'),
    one manual request might not have found it because it might not random into no pepperoni or no knife.
## Part A results
| Step | Test type    | Users | Req/s       | p(95)     | Max       | Failed  | Checks   |
| ---- | ------------ | ----- | ----------- | --------- | --------- | ------  | -------- |
| A1   | Smoke        | 1     | 0.551678/s  | 360.33ms  | 360.99ms  | 0.00%   | 100.00%  |
| A2   | Average load | 3     | 1.34/s      | 370ms     | 805ms     | 0.0%    | 100.0%   |
| A3   | Stress       | 5     | 2.3/s       | 360ms     | 827ms     | 0.0%    | 100.0%   |
| A4   | Spike        | 5     | 1.09/s      | 362ms     | 874ms     | 0.0%    | 100.0%   |
| A5   | Soak         | 2     |             |           |           |         |          |
## Part B
1.
2. Each virtual user sends one request per iteration, and an iteration lasts about 1.29 s. So one user sends about 0.78 requests/s x 5 users = 3.9 requests/s which is below 5.
3. No, the response time didn't grow from the average-load test to the stress test. Which mean the system had not yet reached the point where response time starts to climb.
4. Around 40s (Request Rate at 21:20:46 spike to 2/s-2.8/s and drop back to 0.9/s at around 21:21:26)
5. No, the response time and the error rate did not get worse over time. Memory leak is a defect that a soak test of several hours could reveal.
6. p(95) catch slowdowns that the average miss.
7. System in a lab could give different numbers because of latency from network.
8. Sending requests with hundreds of users against a site can slow down the site and could consider denial-of-service attack.
9. Vague requirement: Line man app should load quickly.
   Testable requirement: With 50 users Line man app at the same time for 5 minutes, 95% of requests are answered within 2 seconds, 99% within 4 seconds, and fewer than 1% of requests fail.
    thresholds: {
        http_req_duration: ['p(95)<2000', 'p(99)<4000'],
        http_req_failed: ['rate<0.01'],
        checks: ['rate>0.99'],
    },