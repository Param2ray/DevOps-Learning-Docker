# 🐳 CoderCo Containers Challenge
## 🚀 Building a Multi-Container Flask + Redis Application

🐳 CoderCo Containers Challenge
🚀 Building a Multi-Container Flask + Redis Application
📘 Overview

This project was part of the CoderCo Containers Challenge, where I built a multi-container web application using Docker and Docker Compose.

The goal was simple but powerful:
Create a lightweight Flask web app that connects to a Redis database to count page visits — all running inside containers and orchestrated seamlessly.

🎯 Objectives

✅ Build a Flask application with two routes:

/ → Displays a welcome message

/count → Increments and displays a visit count stored in Redis

✅ Use Redis as a key-value store
✅ Dockerize both services
✅ Connect everything using Docker Compose
✅ Add persistent storage, environment variables, and scaling

🧱 Project Structure
Multi-Container-Application/
│
├── app/
│   ├── app.py
│   ├── requirements.txt
│   └── Dockerfile
│
└── docker-compose.yml

⚙️ Tools & Technologies
Tool	Purpose
🐍 Python (Flask)	Backend web framework
💾 Redis	In-memory database for counting visits
🐳 Docker	Containerization of both services
⚙️ Docker Compose	Multi-container orchestration
📦 Volumes	Data persistence for Redis
🔐 Environment Variables	Configuration management
🌍 How It Works

The Flask app runs inside one container.

The Redis database runs in another.

Both communicate through Docker’s internal network created by Compose.

Every time /count is visited, Flask connects to Redis, increments a key, and displays the updated count.

🧩 Docker Compose Setup
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

▶️ How to Run
# Build and start all containers
docker-compose up --build


Then open your browser:

- 🌐 `http://localhost:5002` → Welcome message  
- 🔢 `http://localhost:5002/count` → Visit count (increments each refresh)


To stop and clean up:

docker-compose down --volumes

💡 Challenges & How I Overcame Them
🧩 1. Flask Module Not Found

Issue: The container threw ModuleNotFoundError: No module named 'flask'.
Fix: I added a requirements.txt file and installed dependencies inside the container:

RUN pip install --no-cache-dir -r requirements.txt


✅ This ensured Flask and Redis were installed in the image during build.

⚙️ 2. Redis Connection Error

Issue: Flask couldn’t connect to Redis using localhost.
Fix: In Docker Compose, containers connect via service names, not localhost.
I updated the code to use:

redis_host = os.getenv("REDIS_HOST", "redis")


✅ Flask now communicates with Redis through the shared Docker network.

💾 3. Data Lost After Container Restart

Issue: Redis data reset every time I stopped the containers.
Fix: Added a named volume for Redis data:

volumes:
  - redis_data:/data


✅ Redis now stores its data persistently.

⚖️ 4. Scaling the Flask App

Goal: Run multiple instances of Flask sharing one Redis counter.
Solution:

docker-compose up --scale web=3 --build


✅ Multiple Flask containers now share the same Redis backend, maintaining a single counter.

🧠 Key Learnings

How multi-container networking works inside Docker Compose

The importance of environment variables for flexible configurations

Managing persistent data using Docker volumes

Reading and troubleshooting logs with docker-compose logs

The real-world DevOps flow: build → test → deploy → scale

🏁 Outcome

✅ Fully working Flask + Redis multi-container app
✅ Data persists between runs
✅ Containers connect via internal network
✅ Ready for scaling and further production-style enhancements

🌟 Next Steps

Add health checks to ensure Redis starts before Flask

Add NGINX for load balancing multiple web containers

Deploy this setup to AWS ECS or EKS for cloud orchestration practice

👨‍💻 Author

Paramjyot Tooray
🌐 Aspiring DevOps & Cloud Engineer

🔗 GitHub: Param2ray

🔗 LinkedIn
