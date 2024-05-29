# VercelBackBuild-version1

### Project Introduction

**VercelBackBuild** is a **backend automated build and deployment system** inspired by Vercel. This project focuses on implementing **continuous integration and deployment (CI/CD)** for web applications, reducing manual operations through automated workflows and enhancing deployment efficiency. The system utilizes **Node.js and Express.js** to build backend services, **Docker** for encapsulating and running applications, **AWS S3** for storing build images, and **AWS ECS & ECR** for scalable container management and image storage. Additionally, it integrates **Redis, Kafka,WebSocket and ClickHouse** to provide real-time log feedback and storage during the build process.

### 💡Differences between Version 1 and Version 2

- Database Integration: New version integrates **PostgreSQL with Prisma ORM**. Automated database migrations using Prisma Migrate are introduced to handle schema changes efficiently and reduce manual SQL scripting errors.
- Log Management: In version 1, I used Redis for log data publishing/subscribing, which now is replaced by **Kafka**. Kafka is used to consume log data efficiently, and the logs are persisted in **ClickHouse**, which is optimized for querying large volumes of data.
- Log API Endpoint: A new API endpoint /logs/:id is added for querying logs based on deployment IDs directly from ClickHouse.

### Technologies Used

- Backend: Node.js, Express.js
- Containers: Docker, AWS ECS, AWS ECR
- Storage: AWS S3
- Databases: PostgreSQL, Prisma ORM
- Log Collections Pipeline: WebSocket, Kafka, ClickHouse
- Others: CI/CD, Reverse Proxy, System Design, Postman API

### Project Structure and architecture diagram

- `api-server`: HTTP API Server for REST API's to handle request.
- `build-server`: Docker Image for executing code builds, cloning repositories, building projects, and pushing to S3
- `s3-reverse-proxy`: Reverse Proxy the subdomains and domains to static resources in S3 buckets.

Services would be running:

| Service            | PORT    |
| ------------------ | ------- |
| `api-server`       | `:9000` |
| `socket.io-server` | `:9002` |
| `s3-reverse-proxy` | `:8000` |

![Architecture diagram](https://github.com/Reneechang17/VercelBackBuild-v2-Kafka/blob/main/system-design-pic/v2.jpg)

### API request on Postman

- POST request for Project
  - Input: Project name and Github URL
  - Get: Project ID and Subdomain name

![POST request for Project](https://github.com/Reneechang17/VercelBackBuild-v2-Kafka/blob/main/Postman-test-pic/project-post.jpg)

- POST request for Deploy
  - Input: Project ID
  - Get: Deployment ID

![POST request for Deploy](https://github.com/Reneechang17/VercelBackBuild-v2-Kafka/blob/main/Postman-test-pic/deploy-post.jpg)

- Subdomain.localhost:8000

![Sccess](https://github.com/Reneechang17/VercelBackBuild-v2-Kafka/blob/main/Postman-test-pic/run%20on%208000.jpg)

- GET request for Logs
  - logs/deploymentID
  - Get: Logs

![GET request for Logs](https://github.com/Reneechang17/VercelBackBuild-v2-Kafka/blob/main/Postman-test-pic/logs-get.jpg)
