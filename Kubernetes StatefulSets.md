## Task 1: Understand the Problem

Create a Deployment with 3 replicas using nginx
Check the pod names — they are random (app-xyz-abc)
Delete a pod and notice the replacement gets a different random name
This is fine for web servers but not for databases where you need stable identity.

Feature	              Deployment	      StatefulSet
Pod names	            Random	         Stable, ordered (app-0, app-1)
Startup order	      All at once	     Ordered: pod-0, then pod-1, then pod-2
Storage	               Shared PVC	     Each pod gets its own PVC
Network identity	 No stable hostname 	Stable DNS per pod

Delete the Deployment before moving on.