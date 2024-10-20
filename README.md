---

# Cloud Assignment 3: Docker Automation Project  

This project demonstrates how to build a Docker container that performs automated text processing using a Python script. It reads two input text files, processes the word counts, identifies top frequent words, and retrieves the machine's IP address. The container automates the entire process from execution to generating output. Additionally, the project includes optional orchestration using Kubernetes for managing multiple container replicas.

---

## Overview  

The containerized Python script (`script.py`) reads the files `IF.txt` and `AlwaysRememberUsThisWay.txt` from `/home/data` within the container. It performs the following operations:  
1. Counts the total words in each file.  
2. Computes the grand total of words across both files.  
3. Identifies the top 3 most frequent words from each file, with special handling for contractions (e.g., “I'm” becomes "I" and "am").  
4. Retrieves the IP address of the machine running the container.  
5. Outputs all results to a text file (`result.txt`) in `/home/data/output/` and prints it to the console.  

This project showcases the use of lightweight Docker images, text processing in Python, and container orchestration via Kubernetes.

---

## Project Structure  

```
cloud-assignment-3/
├── script.py          # Python script for text processing
├── Dockerfile         # Docker configuration file
├── deployment.yaml    # Kubernetes deployment file
├── data/              # Input data files folder
│   ├── IF.txt         # First text file
│   ├── AlwaysRememberUsThisWay.txt  # Second text file
├── output/            # Folder to store the output result
│   └── result.txt     # Generated results from the container
```

---

## Key Components  

### 1. **Dockerfile**  
The Dockerfile uses a lightweight Python image (`python:3.9-slim`). It copies the Python script and input files into the container and sets the Python script as the entry point to execute when the container starts.  

### 2. **Python Script (`script.py`)**  
The script performs the following:  
- Reads text files and splits words while handling contractions.  
- Counts total words in each file and finds the top 3 frequent words.  
- Retrieves the machine's IP address using the `socket` library.  
- Writes all results to `/home/data/output/result.txt`.  

### 3. **Kubernetes Deployment (deployment.yaml)**  
The Kubernetes configuration deploys two replicas of the container to ensure scalability and reliability. It allows monitoring with commands such as:  
```bash  
kubectl get pods > kube_output.txt  
cat kube_output.txt  
```

---

## Usage  

1. **Build and Run the Docker Container:**  
   Make sure Docker is installed and running on your machine.  
   ```bash  
   docker build -t cloud-assignment-3 .  
   docker run -v $(pwd)/data:/home/data cloud-assignment-3  
   ```  

2. **Output Verification:**  
   The script writes the output to `result.txt` in the `/home/data/output` directory. The container prints the following on the console:  
   - Word counts for both files.  
   - Top 3 frequent words in each file.  
   - The grand total of words.  
   - IP address of the machine running the container.  

3. **Kubernetes Orchestration (Optional):**  
   Use the following command to apply the deployment:  
   ```bash  
   kubectl apply -f deployment.yaml  
   ```  

---

## Expected Output (Sample)  

```
Total words in IF.txt: 300  
Total words in AlwaysRememberUsThisWay.txt: 250  
Grand total of words: 550  

Top 3 words in IF.txt:  
- if: 15  
- you: 10  
- can: 8  

Top 3 words in AlwaysRememberUsThisWay.txt:  
- always: 12  
- remember: 10  
- way: 8  

IP Address of the machine running the container: 192.168.1.10  
```

---

## Extra Credit (Orchestration Using Kubernetes)  

The project includes a `deployment.yaml` file that configures two replicas of the container. This ensures scalability and fault tolerance. The status of the deployed containers can be verified using:  
```bash  
kubectl get pods  
```

---

## Conclusion  

This project demonstrates the power of Docker for automating tasks and streamlining workflows. The use of Kubernetes adds an extra layer of scalability and management for container deployments. Through this assignment, you have implemented an end-to-end solution that reads, processes, and outputs data efficiently inside a container.

---

