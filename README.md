# 14848 - Prabaht Pandey HW#3 
Submissions of Prabhat Pandey (prabhatp)

## Updates made to the project
- Updates in sa-webapp:
    - Dockerfile: Separate build stage and run stage
        - Use maven image in the build stage. User openjdk in the run stage
- Updates in sa-frontend:
    - Update the package-lock file to latest version
    - Added .dockerignore file
    - Dockerfile: Separate build stage and run stage
    - fetch url: Dynamically choose the webapp url based on NODE_ENV

## DockerHub images created
These are public dockerhub images: 
- https://hub.docker.com/repository/docker/prabhatcheesecake/hw3-sa-frontend
- https://hub.docker.com/repository/docker/prabhatcheesecake/hw3-sa-webapp
- https://hub.docker.com/repository/docker/prabhatcheesecake/hw3-sa-logic

## Video recordings of the submission
- App running on the GKE: https://drive.google.com/file/d/1Wjyivri092oXHgzeJBUEU4cV3Y6wzh9C/view?usp=sharing
- Code walkthrough: https://drive.google.com/file/d/1TvHXUD76puQ-Htko7MoXDuaBGyegoZqw/view?usp=sharing

## Steps to complete assignment
- Examing the repository, dockerfile and k8 configs
- Update the codebase as described above
- build images and push them to dockerhub
- pull images from dockerhub in gcloud
- create artifact registry in gcloud
- tag docker images and push them to artifact registry
    - Since I'm on arm machine, I also built the images in gcloud shell and pushed the amd image in the registry
- Create kunernetes cluster
- Connect to the cluster
- Create deployments and services in the cluster

## Comamnds used in the assignment


Tag docker images of personal repository
```
# Webapp
docker tag prabhatcheesecake/hw3-sa-webapp:v1.0.0 northamerica-northeast2-docker.pkg.dev/cmu-14848-471600/homeworks-14848/prabhatcheesecake/hw3-sa-webapp:v1.0.0

docker tag prabhatcheesecake/hw3-sa-frontend:v1.0.0 northamerica-northeast2-docker.pkg.dev/cmu-14848-471600/homeworks-14848/prabhatcheesecake/hw3-sa-frontend:v1.0.0

docker tag prabhatcheesecake/hw3-sa-logic:v1.0.0 northamerica-northeast2-docker.pkg.dev/cmu-14848-471600/homeworks-14848/prabhatcheesecake/hw3-sa-logic:v1.0.0
```
Push the docker images in the artifact registry
```
docker push northamerica-northeast2-docker.pkg.dev/cmu-14848-471600/homeworks-14848/prabhatcheesecake/hw3-sa-webapp:v1.0.0
docker push northamerica-northeast2-docker.pkg.dev/cmu-14848-471600/homeworks-14848/prabhatcheesecake/hw3-sa-frontend:v1.0.0
docker push northamerica-northeast2-docker.pkg.dev/cmu-14848-471600/homeworks-14848/prabhatcheesecake/hw3-sa-logic:v1.0.0

```
Create GKE cluster
```
gcloud container clusters create \
--machine-type e2-standard-2 \
--num-nodes 2 \
--zone northamerica-northeast2 \
--cluster-version latest \
--disk-size 25 \
myhw3cluster
```
Note: 
- Added disk-size to be under quota of 500GB
- Info: Your Pod address range (`--cluster-ipv4-cidr`) can accommodate at most 1008 node(s)
Path of images in the Artifacts Registry
```
northamerica-northeast2-docker.pkg.dev/cmu-14848-471600/homeworks-14848/prabhatcheesecake/hw3-sa-frontend:v1.0.0

northamerica-northeast2-docker.pkg.dev/cmu-14848-471600/homeworks-14848/prabhatcheesecake/hw3-sa-webapp:v1.0.0

northamerica-northeast2-docker.pkg.dev/cmu-14848-471600/homeworks-14848/prabhatcheesecake/hw3-sa-logic:v1.0.0
```

K8s commands to create and apply pods, deployments and services:
```
kubectl create -f resource-manifests/sa-frontend-pod.yaml
kubectl get pod
kubectl create -f resource-manifests/service-sa-frontend-lb.yaml
kubectl get service

kubectl apply -f sa-frontend-pod.yaml

kubectl apply -f sa-web-app-deployment.yaml --record
kubectl apply -f service-sa-web-app-lb.yaml

kubectl apply -f sa-logic-deployment.yaml --record
kubectl apply -f service-sa-logic.yaml
```

