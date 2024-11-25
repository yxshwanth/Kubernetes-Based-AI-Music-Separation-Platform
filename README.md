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

### 🛠 Local Development
1. **Set up Kubernetes**:
   Install Docker and Minikube (or another local Kubernetes setup).
   
2. **Deploy Redis & Min.io**:
   Use the provided script to simplify setup:
   ```bash
   ./deploy-local-dev.sh
   ```

3. **Build Docker Images**:
   ```bash
   docker build -t msas-rest:1.0 rest/
   docker build -t msas-worker:1.0 worker/
   ```

4. **Apply Kubernetes Manifests**:
   ```bash
   kubectl apply -f kubernetes/deployment.yaml
   ```

5. **Port Forward for Local Testing**:
   ```bash
   kubectl port-forward --address 0.0.0.0 service/redis 6379:6379 &
   kubectl port-forward --namespace minio-ns svc/myminio-proj 9000:9000 &
   ```

---

### ☁️ Cloud Deployment (GKE or Similar)
1. **Create a Kubernetes Cluster**:
   Use GKE or your preferred cloud provider.

2. **Push Docker Images**:
   ```bash
   docker tag msas-rest:1.0 gcr.io/<your-project-id>/msas-rest:1.0
   docker push gcr.io/<your-project-id>/msas-rest:1.0
   ```

3. **Update Kubernetes Manifests**:
   Replace local image references with your container registry URLs.

4. **Deploy to the Cluster**:
   ```bash
   kubectl apply -f kubernetes/deployment.yaml
   ```

5. **Monitor Logs**:
   ```bash
   kubectl logs -l app=rest
   kubectl logs -l app=worker
   ```

---

## 🧪 Testing

### Sample Requests
Two scripts are provided to simulate usage:
- **`sample-requests.py`**: Processes full MP3 files.
- **`short-sample-requests.py`**: Processes smaller files for faster iteration.

```bash
python sample-requests.py --host localhost --port 5000
```

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

## ✨ Recruiter Note

This project demonstrates my ability to:
- Build **scalable microservices** for real-world applications.
- Apply **cloud-native principles** to architect robust solutions.
- Write clean, maintainable code following industry best practices.
- Solve **complex problems** with innovative and efficient approaches.

If you're looking for someone who can **deliver results**, **think creatively**, and **drive projects to success**, let's connect! 🚀

---

## 🤝 Contributing

Feel free to fork this repository and submit pull requests. All suggestions are welcome!

---

## 📜 License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
