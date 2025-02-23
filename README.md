# 99-tech-problem-6
# Scoreboard API Module Specification

# 1. Module Name

Scoreboard Management Module

# 2. Purpose & Scope

This module is responsible for managing user scores and providing a live-updated scoreboard displaying the top 10 users with the highest scores.

# 3. Endpoints

## 3.1. Update User Score

Endpoint: POST /api/scores/update

Description: Updates a user’s score when an action is completed.

Request Body:
```json
{
  "userId": "string",
  "scoreIncrement": "number"
}
```
Response:
```json
{
  "success": true,
  "newScore": "number"
}
```
Authentication: Required (JWT-based authentication)

Rate Limiting: Yes, to prevent abuse.

## 3.2. Get Top Scores

Endpoint: GET /api/scores/top

Description: Retrieves the top 10 users with the highest scores for fist load

Response:
```json
{
  "topScores": [
    {
      "userId": "string",
      "score": "number"
    }
  ]
}
```
Authentication: Public or private depend on requirement

## 3.3. WebSocket for Live Scoreboard Updates

Endpoint: ws://this-web.com/scores/live

Description: Provides real-time updates when scores change.

Message Format:

```json
{
  "event": "scoreUpdate",
  "userId": "string",
  "newScore": "number"
}
```
Authentication: Public or private depend on requirement

## 4. Security & Authorization

- JWT Authentication: All requests modifying scores must be authenticated and have role.

- Server-Side Validation: Ensure userId exists and scoreIncrement is within an acceptable range.

## 5. Dependencies

- Database: PostgreSQL

- WebSocket Server: Required for real-time updates

## 6. Error Handling

400 Bad Request: Validation error in API request fields.

401 Unauthorized: User authentication failed.

404 Not Found: User not found.

422 Unprocessable Content:  valid requests that cannot be processed due to business logic constraints

500 Internal Server Error: Unexpected server issues.

Other errors: Any additional errors that arise during processing.

## 7. Logging & Monitoring

Log all API requests for auditing.

Monitor anomalies such as sudden large score increases.

Use monitoring tools like Prometheus and Grafana.

## 8. Versioning & Deprecation Strategy

Current API version: v1

Future changes will be introduced under /api/v2/scores/ namespace when necessary.

## 9. Improvement
- Optimize query, indexing DB, replication, sharding
- Rate Limiting – Prevent excessive API calls.
- Load Balancing – Distribute traffic efficiently across multiple servers.
- Caching – Use Redis for leaderboard storage to ensure fast retrieval.
- Replication, Sharding, and Load Balancing – Scale database queries efficiently.
- Role-Based Access Control (RBAC) – Restrict which clients can modify specific user groups.
- Replay Attack Prevention & Double-Click Handling:
- Frontend: Use debounce/throttle.
- Backend: Use timestamps, old-score verification, nonces, and encrypted keys.
- Message Queue (Kafka or RabbitMQ) –
Store a history of updates to prevent data loss or fraud.
Improve asynchronous processing of score updates.
- Periodic Full Leaderboard Refresh – Refresh leaderboard every 5-15 minutes.
- WebSocket Reconnection & Table Refresh –
Automatically reconnect if WebSocket disconnects.
Refresh the scoreboard if needed.
- Microservices Architecture
- Bulk write for db
- ...
