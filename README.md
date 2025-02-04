# counetr-app-helm
# Helm Chart for Counter Application

This Helm chart deploys a simple Kubernetes application with two main components:

1. **Counter Node.js Application**: 
    A frontend Node.js application that provides a user interface with a button. Clicking the button increments a counter value and stores it in the backend database.
2. **PostgreSQL Backend**: 
    A PostgreSQL database that stores the current counter value.

---

## Installation and Usage Instructions

### Prerequisites

- A Kubernetes cluster up and running.
- `kubectl` CLI installed and configured.
- `helm` CLI installed.

### Installing the Application

To deploy the Counter Application, execute the following command:

```bash
helm install counter-application .
```

### Upgrading the Application

If you make changes to the chart or the application and need to apply those updates, use:

```bash
helm upgrade counter-application .
```

### Post-Deployment Steps

1. **Connect to the PostgreSQL Pod**  
   After deploying the application, you need to manually create a table in the PostgreSQL database. Connect to the PostgreSQL pod using the following command:

   ```bash
   kubectl exec -it <postgres-pod-name> -- psql -U admin -d nodeapp
   ```

   Replace `<postgres-pod-name>` with the actual name of the PostgreSQL pod. Once inside the PostgreSQL shell, create the necessary table to store the counter value:

   ```sql
   CREATE TABLE counter (
       id SERIAL PRIMARY KEY,
       value INT NOT NULL
   );
   ```

2. **Access the UI**  
   To view and interact with the counter application UI, forward the port of the application pod to your local machine. Run the following command:

   ```bash
   kubectl port-forward pod/<app-pod-name> <local-port>:<container-port>
   ```

   Replace `<app-pod-name>` with the name of the counter application pod, `<local-port>` with the desired port on your machine, and `<container-port>` with the container's port exposed by the Node.js application.  
   Once port forwarding is set up, you can access the UI in your browser at `http://localhost:<local-port>`.

---

## Application Overview

This application demonstrates a simple microservices architecture with two core components:

### 1. Counter Node.js Application
- A frontend application built with Node.js.
- The UI consists of a button labeled **Count**.
- Each time the button is clicked, the counter value is incremented, and the updated value is stored in the PostgreSQL database.

### 2. PostgreSQL Backend
- A backend database powered by PostgreSQL.
- Stores the counter value persistently.
- Facilitates seamless integration with the frontend application.

---

## Helm Chart Details

This Helm chart automates the deployment of both the Counter Node.js application and the PostgreSQL backend:

- **Counter Application**:
  - Deployed as a Kubernetes Deployment.
  - Exposed via a Kubernetes Service.
- **PostgreSQL**:
  - Deployed as a Kubernetes Deployment.
  - Exposed via a ClusterIP Service for internal communication.

