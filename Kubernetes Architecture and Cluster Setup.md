## Task 1: Recall the Kubernetes Story

Before touching a terminal, write down from memory:

Why was Kubernetes created? What problem does it solve that Docker alone cannot?

        - Kubernetes was created to manage containers at scale. Docker is excellent for building, packaging, and running individual containers, 
          but Docker alone does not solve large-scale operational problems such as:

            Running hundreds or thousands of containers across many servers
            Automatically restarting failed containers
            Scaling applications up/down based on demand
            Load balancing traffic between containers
            Rolling updates and rollbacks without downtime
            Service discovery between containers
            Efficient resource scheduling across clusters

            Kubernetes provides container orchestration, meaning it automates deployment, scaling, networking, healing, and management of 
            containerized applications across multiple machines.


Who created Kubernetes and what was it inspired by?

         - Kubernetes was originally created by Google and open-sourced in 2014.

           It was inspired by it's google itself because they want to scale their apps but they are doing manually
           which crash and after crash they heal manually

           So they created Borg it written in Go language , it has that capabilities for auto-scaling and auto-healing

           After all this google donate it as open source , first it was linux foundation, then CNCF(Cloud native computing foundation)
           and then it becomes kubernetes.

What does the name "Kubernetes" mean?

          - The word Kubernetes comes from Greek and means helmsman, pilot, or steersman — someone who guides a ship.


Task 2:The Kubernetes Architecture

What happens when you run kubectl apply -f pod.yaml? Trace the request through each component.

           - kubectl apply -f pod.yaml sends the YAML definition to the API Server.
             The API Server validates it and stores the Pod information in etcd.
             Then the Scheduler detects an unscheduled Pod and selects the best worker node.
             That node’s kubelet receives the assignment, asks the container runtime to start the container, 
             and sends status updates back to the API Server.

What happens if the API server goes down?

           - kubectl commands fail
             No new Pods scheduled
             No config changes
             Controllers cannot update state normally
             kubelets may keep existing Pods running locally

What happens if a worker node goes down?



