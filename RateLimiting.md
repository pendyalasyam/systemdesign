Rate Limiting: 

Why To Rate Limit:
1. Protecting Resources from bots
2. Protecting business from intentional bad users
3. Protecting downstream from overwhelming
4. Being Fairness to all users
5. Accepting thirdparty SLAs
6. Reduce blast radius

Rate Limiting Basis
1. Per IP
2. Per User
3. Per Device
4. Global

Rate Limiting Procedure
1. Request Hits API Gateway
2. API Gateway checks with Rate Limiter service if it is ok go ahead with the request
3. If  Rate Limiter Service says yes, then API Gateway forwards request to responsible micro service
4. Or it will be rejected with Http Status Code: 429
