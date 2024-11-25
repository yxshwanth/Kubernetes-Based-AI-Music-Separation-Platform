# 🎵 Music-Separation-as-a-Service (MSaaS) 🚀

![Music Separation](images/music_separation.png)

Welcome to **Music-Separation-as-a-Service (MSaaS)**, a Kubernetes-powered microservices project that separates music tracks into individual components like vocals, drums, and instruments. This project combines **cutting-edge technology**, **scalable architecture**, and **cloud-native design** to deliver a powerful and efficient solution for audio processing. 

⚡ **Key Features**:
- Scalable **microservices** architecture using Kubernetes.
- Advanced music separation powered by [Demucs](https://github.com/facebookresearch/demucs).
- **Cloud-ready** integration with Min.io/S3 for object storage.
- Efficient task queuing and logging via **Redis**.
- Easily extensible and built with portability in mind.

---

## 📂 Project Structure

```
├── rest/              # REST API service
├── worker/            # Worker nodes for audio processing
├── redis/             # Redis configuration and setup
├── logs/              # Debugging and monitoring tools
├── kubernetes/        # Kubernetes manifests for deployment
├── images/            # Images for documentation
└── README.md          # Project overview (you're here!)
```

---

## 🌐 Architecture Overview

The project is designed as a **cloud-native application** with a Kubernetes cluster orchestrating the following components:

1. **REST API**: Accepts MP3 files, queues tasks, and provides status/results.
2. **Worker Nodes**: Perform waveform source separation using Demucs.
3. **Redis**: Acts as a task queue and logging mechanism.
4. **Min.io**: Stores input files and processed tracks.

![Architecture Diagram](images/architecture.png)

---


## 🚀 Deployment Steps

### 1️⃣ **Start Minikube**

1. Open a terminal and start Minikube:
   ```bash
   minikube start
   ```
   Ensure Minikube uses the Docker driver and initializes correctly.

2. To stop Minikube later (if needed):
   ```bash
   minikube stop
   ```

---

### 2️⃣ **Set Up Min.io**

1. Add the Bitnami Helm repository:
   ```bash
   helm repo add bitnami https://charts.bitnami.com/bitnami
   ```

2. Install Min.io with Helm into a specific namespace (`minio-ns`):
   ```bash
   helm install -f minio/minio-config.yaml -n minio-ns --create-namespace minio-proj bitnami/minio
   ```

3. Once deployed, note the Min.io DNS or access credentials by running:
   ```bash
   kubectl get svc -n minio-ns
   ```

4. Port forward the Min.io service for local access:
   ```bash
   kubectl port-forward --namespace minio-ns svc/minio-proj 9000:9000
   ```

---

### 3️⃣ **Deploy Redis**

1. Navigate to the Redis deployment folder:
   ```bash
   cd redis/
   ```

2. Apply the Redis deployment YAMLs:
   ```bash
   kubectl apply -f redis-deployment.yaml
   kubectl apply -f redis-service.yaml
   ```

3. Port forward Redis for local development:
   ```bash
   kubectl port-forward svc/redis 6379:6379
   ```

---

### 4️⃣ **Build and Deploy the REST Service**

1. Build the REST service Docker image:
   ```bash
   docker build -f Dockerfile -t <your-docker-hub-username>/demucs-rest .
   ```

2. Push the image to Docker Hub:
   ```bash
   docker push <your-docker-hub-username>/demucs-rest
   ```

3. Apply the REST service deployment:
   ```bash
   kubectl apply -f rest/rest-deployment.yaml
   kubectl apply -f rest/rest-service.yaml
   kubectl apply -f rest/rest-ingress.yaml
   ```

4. Port forward the REST service for local access:
   ```bash
   kubectl port-forward svc/demucs-rest 5000:5000
   ```

---

### 5️⃣ **Build and Deploy the Worker Service**

1. Navigate to the Worker service folder:
   ```bash
   cd worker/
   ```

2. Build the Worker service Docker image:
   ```bash
   docker build -f Dockerfile -t <your-docker-hub-username>/demucs-worker .
   ```

3. Push the image to Docker Hub:
   ```bash
   docker push <your-docker-hub-username>/demucs-worker
   ```

4. Apply the Worker deployment:
   ```bash
   kubectl apply -f worker/worker-deployment.yaml
   ```

---

### 6️⃣ **Verify Deployments**

1. Check if all pods are running:
   ```bash
   kubectl get pods
   ```

2. View logs for the REST or Worker services to verify functionality:
   ```bash
   kubectl logs <pod-name>
   ```

3. If any issues arise, debug using:
   ```bash
   kubectl describe pod <pod-name>
   ```

---

### 7️⃣ **Test the Setup**

1. Use the REST service endpoint exposed on port 5000 to submit a file for processing.

2. Test with the provided `sample-requests.py` or `short-sample-requests.py`:
   ```bash
   python sample-requests.py --host localhost --port 5000
   ```

---

This step-by-step guide reflects the deployment process for this project. If you face any issues, revisit logs and debug accordingly. Let’s make music separation seamless!


### Sample Output
- Processed audio tracks stored in the `output` bucket in Min.io.
- Tracks separated into components like `vocals`, `bass`, `drums`, etc.

---

## 🎯 Why This Project Stands Out

- **Real-World Complexity**: Tackles resource-intensive tasks with scalable solutions.
- **Cloud-Native Design**: Built for modern infrastructure with seamless integration into cloud environments.
- **Focus on Excellence**: Clean, modular codebase following industry best practices.

### 📸 Screenshots

#### Input & Output Buckets
- **Input (Queue Bucket)**:
  ![Queue Bucket](images/buckets.png)

- **Output (Processed Tracks)**:
  ![Output Bucket](images/output-bucket.png)

---

## 🌟 Key Technical Highlights

- **Redis-Based Task Queue**: Efficient task management with blocking pops and logs for debugging.
- **Demucs Integration**: Leverages Facebook's open-source library for advanced audio separation.
- **Min.io Storage**: Easily adaptable for AWS S3, Google Cloud Storage, or any S3-compatible service.
- **Kubernetes-Orchestrated**: Automatic scaling and fault-tolerant deployments.

---

## 🛠 Future Enhancements

- 🔒 **Authentication**: Secure API endpoints for user management.
- 🔗 **gRPC Integration**: High-performance communication for large-scale deployments.
- 🎨 **UI Development**: Build a web-based interface for uploading and downloading files.
- 📊 **Analytics Dashboard**: Provide insights into processing metrics and usage.

---

## 🤝 Contributing

Feel free to fork this repository and submit pull requests. All suggestions are welcome!

---

