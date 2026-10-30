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
    the problem was lowercase name ('pepperoni', 'knife')
    one manual request might not have found it because it might not random into no pepperoni or no knife.
## Part A results
| Step | Test type    | Users | Req/s | p(95) | Max | Failed | Checks |
| ---- | ------------ | ----- | ----- | ----- | --- | ------ | ------ |
| A1   | Smoke        | 1     |       |       |     |        |        |
| A2   | Average load | 3     |       |       |     |        |        |
| A3   | Stress       | 5     |       |       |     |        |        |
| A4   | Spike        | 5     |       |       |     |        |        |
| A5   | Soak         | 2     |       |       |     |        |        |
## Part B
1.
2.
3.
4.
5.
6.
7.
8.
9.