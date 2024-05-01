xample approach
Using nano editor, create a Kubernetes deployment file

nano deployment.yaml

Copy the following content

apiVersion: apps/v1
kind: Deployment
metadata:
  name: concerto-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: concerto
  template:
    metadata:
      labels:
        app: concerto
    spec:
      containers:
      - name: concerto
        image: timpamungkas/concerto:1.0.0
        imagePullPolicy: Always
        
---
 
apiVersion: v1
kind: Service
metadata:
  name: concerto-service
spec:
  type: LoadBalancer
  selector:
    app: concerto
  ports:
    - protocol: TCP
      port: 8080
      targetPort: 8080
The YAML configuration defines a Kubernetes Deployment named concerto-deployment with 2 replicas running a container based on the image timpamungkas/concerto:1.0.0.

It also specifies a Kubernetes Service named concerto-service of type LoadBalancer, directing traffic to pods concerto on port 8080.


Save it by pressing CTRL + X (Windows) or Command + X (Mac)


Press y to save your change, and then confirm the prompt by pressing Enter (Windows) or Return (Mac)


Using the kubectl command, deploy the deployment.yaml:

kubectl apply -f deployment.yaml

Wait around 2 minutes until the pods are ready. Check using this command, and ensure all pod status is Running and Ready

kubectl get pods

curl into API to create a transaction using valid data (following user-manual.txt), and ensure the API works (returning status code 2xx)

curl -i -L 'http://localhost:8080/concerto/api/transaction/create' \
-H 'Content-Type: application/json' \
-H 'Accept: application/json' \
-d '{
  "eventId": "dc368370-a09c-4fbe-89ab-09f27c6ec56d",
  "eventClass": "GOLD",
  "price": 99,
  "userId": "bc0ebf72-ad8b-45cb-8991-ada961b4415d"
}'

curl into API to get a transaction information using valid data (following user-manual.txt), and ensure the API works (returning status code 2xx)

curl -i -L 'http://localhost:8080/concerto/api/transaction/info/6797eb48-f0cc-4679-8502-560acf09a163' \
-H 'Accept: application/json'

curl into API to check out a transaction using valid data (following user-manual.txt), and ensure the API works (returning status code 2xx)

curl -i -L -X POST 'http://localhost:8080/concerto/api/transaction/checkout/3935eb5a-bc0e-4878-b0a5-0c4cc12f2da3' \
-H 'Accept: application/json'
