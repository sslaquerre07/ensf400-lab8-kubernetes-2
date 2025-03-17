# ensf400-lab8-kubernetes-2

## Objectives
This lab will teach us the scheduling, configmaps, canary and blue-green deployment strategies of Kubernetes. Through Minikube, a simplified Kubernetes engine running on a single computer, we practice the key concepts and usage of Kubernetes.

## Environment

### Set Up Your GitHub CodeSpaces Instance

Same as Lab 6, this lab will be performed in [GitHub CodeSpaces](https://github.com/codespaces). Create an instance using GitHub Codespaces. Choose repository `denoslab/ensf400-lab8-kubernetes-2`.


```bash
$ minikube start --nodes 3 -p ensf400
```

This step will start the Minikube service with 3 virtual nodes, stored in a profile called `ensf400`. If for some reason your Codespaces instance was stopped (e.g., not using it for a while). You can restart the minikube service using this profile by running:

```bash
$ minikube start -p ensf400
```

## Steps

Go to Section 7 - 10 and complete the steps for each section. The steps can be found in the `README.md` files in each subdirectory.

## Have Your Work Checked By a TA

The TA will check the completion of the following tasks:

- Output of Section 7.
- Output of Section 8.
- Output of Section 9.
- Output of Section 10.


Each member of the group should be able to answer all of the following questions. The TA will ask each person one question selected at random, and the student must be able to answer the question to get credit for the lab.

- Q1: Explain the scheduling strategy of Node Affinity and the scenarios to use it.
    - Strategy: Allow you to only schedule pods to specific nodes. 
    - Scenarios:
        - Pod is required to run on a node/set of nodes with specific resources.
        - If certain pods/workloads must run on certain nodes for security/regulatory reasons
- Q2: Explain the scheduling strategy of Pod Anti-Affinity and the scenarios to use it.
    - Strategy: Spreads out pods amongst available nodes to avoid a central point of failure
    - Scenarios:
        - Applications that require high availability and fault tolerance (minimize central point of failure)
        - Applications that priortize load balancing (not one node carrying out all of the work)
- Q3: Explain the deployment strategy of blue-green deployment. How to switch between the two versions of deployments?
    - Strategy: Essentially you can set an alternate version to host resource requests whilst the main version of the code gets updated/maintained. This allows for system maintainence without any downtime.
    - How to switch: 
        - Deploy the alternate version of the code with another selector (for this case we'll call it the green version)
        - Switch the versions with the following command: kubectl patch service myapp -p '{"spec":{"selector":{"app": "green"}}}' (selector is changed for the service carrying out the application)
- Q4: Explain the deployment strategy of canary deployment. How to adjust the ratio of users getting serviced by the canary deployment?
    - Strategy: This strategy involves slowly exposing users to an updated version of the software. The new version exists alongside the old one to test the behaviours of the two to verify the functionality before releasing the updated version.
    - How to adjust ratio: An Ingress definition is ideal for exposing multiple services, and it also has some parameters to specify a canary deployment and how what percentage of traffic should be directed to the canary service:
        - nginx.ingress.kubernetes.io/canary: Either true or false, indicates whether a service is a canary or not
        - nginx.ingress.kubernetes.io/canary-weight: (e.g "20"), percentage of traffic to be redirected
