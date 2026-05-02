## Task 1: See the Problem — Data Lost on Pod Deletion

Write a Pod manifest that uses an emptyDir volume and writes a timestamped message to /data/message.txt

          - Go to test-pod.yaml

Apply it, verify the data exists with kubectl exec
Delete the Pod, recreate it, check the file again — the old message is gone

         - kubectl apply -f pod.yaml
         - kubectl exec test -- cat /data/message.txt
         - kubectl delete pod emptydir-demo
         - kubectl apply -f emptydir-pod.yaml

## Task 2: Create a PersistentVolume (Static Provisioning)

Write a PV manifest with capacity: 1Gi, accessModes: ReadWriteOnce, persistentVolumeReclaimPolicy: Retain, and hostPath pointing to /tmp/k8s-pv-data

        - Go to pv.yaml

Apply it and check kubectl get pv — status should be Available

        - Apply It

            kubectl apply -f pv.yaml

         Check PV

            kubectl get pv

Access modes to know:

ReadWriteOnce (RWO) — read-write by a single node
ReadOnlyMany (ROX) — read-only by many nodes
ReadWriteMany (RWX) — read-write by many nodes
hostPath is fine for learning, not for production.

Verify: What is the STATUS of the PV?

## Task 3: Create a PersistentVolumeClaim

Write a PVC manifest requesting 500Mi of storage with ReadWriteOnce access

         - Go to pvc.yaml

Apply it and check both kubectl get pvc and kubectl get pv
Both should show Bound — Kubernetes matched them by capacity and access mode

## Task 4: Use the PVC in a Pod — Data That Survives

Write a Pod manifest that mounts the PVC at /data using persistentVolumeClaim.claimName

          - Go to pvs-pod.yaml

Write data to /data/message.txt, then delete and recreate the Pod
Check the file — it should contain data from both Pods

           - Apply Pod

                kubectl apply -f pvc-pod.yaml

             Wait until Running:

                kubectl get pods
            
            Step 3: Check File

                kubectl exec pvc-demo -- cat /data/message.txt

## Task 5: StorageClasses and Dynamic Provisioning

Run kubectl get storageclass and kubectl describe storageclass
Note the provisioner, reclaim policy, and volume binding mode
With dynamic provisioning, developers only create PVCs — the StorageClass handles PV creation automatically   

             - kubectl get sc
               kubectl describe sc <name>

## Task 6: Dynamic Provisioning

Write a PVC manifest that includes storageClassName: standard (or your cluster's default)
Apply it — a PV should appear automatically in kubectl get pv
Use this PVC in a Pod, write data, verify it works               