
░█████╗░░██████╗░██████╗██╗░██████╗░███╗░░██╗███╗░░░███╗███████╗███╗░░██╗████████╗  ██████╗░
██╔══██╗██╔════╝██╔════╝██║██╔════╝░████╗░██║████╗░████║██╔════╝████╗░██║╚══██╔══╝  ╚════██╗
███████║╚█████╗░╚█████╗░██║██║░░██╗░██╔██╗██║██╔████╔██║█████╗░░██╔██╗██║░░░██║░░░  ░░███╔═╝
██╔══██║░╚═══██╗░╚═══██╗██║██║░░╚██╗██║╚████║██║╚██╔╝██║██╔══╝░░██║╚████║░░░██║░░░  ██╔══╝░░
██║░░██║██████╔╝██████╔╝██║╚██████╔╝██║░╚███║██║░╚═╝░██║███████╗██║░╚███║░░░██║░░░  ███████╗
╚═╝░░╚═╝╚═════╝░╚═════╝░╚═╝░╚═════╝░╚═╝░░╚══╝╚═╝░░░░░╚═╝╚══════╝╚═╝░░╚══╝░░░╚═╝░░░  ╚══════╝

Hi there! Welcome to CLO835 - Assignment 2 - Naveen Wijeyasekaran!

Deployment Steps -

1. Deploy remote instance, and copy files to remote machine:
    a. terraform apply -auto-approve 
    b. scp -i week5 init_kind.sh kind.yaml <remote ip>/tmp
    c. scp -i week5 ~/environment/Assignment2/* <remote ip>:/tmp 
    d. ssh -i week5 ec2-user@<remote ip>
    e. cd /tmp
	f. chmod 777 init_kind.sh


2. Run cluster and create namespace
	a. ./init_kind.sh
	b. alias k=kubectl
	c. k get nodes
	d. k create ns employee
	
3. Run MySQL (backend) pod, replicaset, deployment and service -
    a. k apply -f mysql-pod.yaml -n employee
    b. k apply -f mysql-replicaset.yaml -n employee
	c. k apply -f mysql-deployment.yaml -n employee
	d. k apply -f mysql-service.yaml -n employee

4. Run Application (frontend) pod, replicaset, deployment and service - 
    a. k apply -f frontend-pod.yaml -n employee
    b. k apply -f frontend-replicaset.yaml -n employee
    c. k apply -f frontend-deployment.yaml -n employee
    d. k apply -f frontend-service.yaml -n employee
	e. k port-forward svc/frontend 8080:80 -n employee

5. Update the image version and rollout application
	a. k edit deployment.apps/frontend -n employee
	b. Change image version to .v1 and :wq
	c. k get pods -n employee