# 🐳 CoderCo Containers Challenge
🚀 Building a Multi-Container Flask + Redis Application
--

This project was part of the CoderCo Containers Challenge, where I built a multi-container application using Docker and Docker Compose.

The idea was simple — but powerful:
Create a lightweight Flask web app that connects to a Redis database to count how many times a page has been visited. Everything runs inside containers and works together through Docker Compose.

This challenge helped me understand how real containerized applications communicate, scale, and persist data.

🎯 Objectives
--
Build a Flask application with two routes:

/ → Displays a welcome message

/count → Increments and shows a Redis-stored counter

Use Redis as a fast in-memory database

Containerize both services with Docker

Link them using Docker Compose

Add persistent storage and environment variables

Test scaling the web app to multiple containers

🧱 Project Structure
--
Multi-Container-Application/
│
├── app/
│   ├── app.py
│   ├── requirements.txt
│   └── Dockerfile
│
└── docker-compose.yml

⚙️ Tools & Technologies
--
Tool	Purpose
🐍 Python (Flask)	Backend web framework

💾 Redis	In-memory key-value store

🐳 Docker	Containerization

⚙️ Docker Compose	Multi-container orchestration

📦 Volumes	Persistent data for Redis

🔐 Environment Variables	Configuring container communication

🌍 How It Works
--
The Flask app runs inside one container

The Redis server runs inside another

Docker Compose creates a shared internal network so they can talk to each other

Every time the /count route is visited:

Flask connects to Redis

Redis increments a stored value

Flask displays the updated count

This simulates real microservice communication using containers.

🧩 Docker Compose Setup
--
services:
  web:
    build: ./app
    ports:
      - "5002:5002"
    environment:
      - REDIS_HOST=redis
      - REDIS_PORT=6379
    depends_on:
      - redis

  redis:
    image: redis:alpine
    volumes:
      - redis_data:/data

volumes:
  redis_data:

▶️ How to Run the Application
--
1. Build and start all containers
docker-compose up --build

▶️ Open your browser
--

🌐 http:\/\/localhost:5002 → Welcome page

🔢 http:\/\/localhost:5002/count (increments each refresh)

▶️ Stop and clean up
--
docker-compose down --volumes

💡 Challenges & How I Solved Them
--
### 🧩 1. Flask Module Not Found

Issue: ModuleNotFoundError: No module named 'flask'

Fix: Added requirements.txt and installed dependencies in the Dockerfile:

RUN pip install --no-cache-dir -r requirements.txt

### ⚙️ 2. Redis Connection Error

Issue: Flask couldn’t connect using localhost

Fix: Containers communicate using service names, not localhost:

redis_host = os.getenv("REDIS_HOST", "redis")

### 💾 3. Data Lost After Restart

Issue: Redis counter reset every run

Fix: Added a named volume:

volumes:
  redis_data:/data


Now Redis keeps data between runs.

### ⚖️ 4. Scaling Flask Containers

To simulate a real web service, I scaled the web container:

docker-compose up --scale web=3 --build


Multiple Flask containers all shared the same Redis counter — great for demonstrating horizontal scaling.

🧠 Key Learnings
--

How Docker Compose handles networking internally

Why environment variables matter for container communication

Using volumes for persistent storage

Debugging container logs with docker-compose logs

Real DevOps workflow: build → test → deploy → scale

Understanding how microservices communicate through internal networks

🏁 Outcome
--

✅ Fully working Flask + Redis multi-container application

✅ Redis data persists between runs

✅ Services communicate over Docker’s internal network

✅ Application can scale horizontally

✅ Clear understanding of container networking and orchestration
