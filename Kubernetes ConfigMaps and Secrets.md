## Task 1: Create a ConfigMap from Literals

Use kubectl create configmap with --from-literal to create a ConfigMap called app-config with keys APP_ENV=production, APP_DEBUG=false, and APP_PORT=8080

            - kubectl create configmap app-config --from-literal=APP_ENV=production --from-literal=APP_DEBUG=false --from-literal=APP_PORT=8080

Inspect it with 
    
             - kubectl describe configmap app-config 

View YAML

             - kubectl get configmap app-config -o yaml

Notice the data is stored as plain text — no encoding, no encryption

## Task 2: Create a ConfigMap from a File

Write a custom Nginx config file that adds a /health endpoint returning "healthy"

Create a ConfigMap 
   
           - Go to my-nginx.conf

from this file using

           -  kubectl create configmap nginx-config --from-file=default.conf=<your-file>

The key name (default.conf) becomes the filename when mounted into a Pod

         kubectl get configmap nginx-config -o yaml (show the file contents)

## Task 3: Use ConfigMaps in a Pod

Write a Pod manifest that uses envFrom with configMapRef to inject all keys from app-config as environment variables. Use a busybox container that prints the values.

               - Go to env-pod.yaml

Write a second Pod manifest that mounts nginx-config as a volume at /etc/nginx/conf.d. Use the nginx image.

               - Go to nginx-config-pod.yaml

Test that the mounted config works: kubectl exec <pod> -- curl -s http://localhost/health

Use environment variables for simple key-value settings. Use volume mounts for full config files.

## Task 4: Create a Secret
Use 

       kubectl create secret generic db-credentials with --from-literal to store DB_USER=admin and DB_PASSWORD=s3cureP@ssw0rd

Inspect with 
 
       kubectl get secret db-credentials -o yaml — the values are base64-encoded

Decode a value: echo '<base64-value>' | base64 --decode

On Windows PowerShell use:

        [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String("YWRtaW4="))

Decode Password Example

         [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String("czNjdXJlUEBzc3cwcmQ="))        

base64 is encoding, not encryption. Anyone with cluster access can decode Secrets. The real advantages are RBAC separation, tmpfs storage on nodes, and optional encryption at rest.

## Task 5: Use Secrets in a Pod

Write a Pod manifest that injects DB_USER as an environment variable using secretKeyRef

           - Go to secret-pod.yaml

           - kubectl apply -f secret-pod.yaml
             
             kubectl get pods

In the same Pod, mount the entire db-credentials Secret as a volume at /etc/db-credentials with readOnly: true

           - Verify 1: Check Environment Variable
             Run:
             kubectl exec secret-test-pod -- printenv DB_USER

           - Verify 2: See Mounted Files
             Run:
             kubectl exec secret-test-pod -- ls /etc/db-credentials  

           - Verify 3: Read File Contents
             Run:
             kubectl exec secret-test-pod -- cat /etc/db-credentials/DB_USER  

             kubectl exec secret-test-pod -- cat /etc/db-credentials/DB_PASSWORD

Verify: each Secret key becomes a file, and the content is the decoded plaintext value

## Task 6: Update a ConfigMap and Observe Propagation

Create a ConfigMap live-config with a key message=hello
Write a Pod that mounts this ConfigMap as a volume and reads the file in a loop every 5 seconds
Update the ConfigMap: kubectl patch configmap live-config --type merge -p '{"data":{"message":"world"}}'
Wait 30-60 seconds — the volume-mounted value updates automatically
Environment variables from earlier tasks do NOT update — they are set at pod startup only
Verify: Did the volume-mounted value change without a pod restart?

## Task 7: Clean Up
Delete all pods, ConfigMaps, and Secrets you created.

            -   kubectl delete pod env-test --ignore-not-found
                kubectl delete pod nginx-config-test --ignore-not-found
                kubectl delete pod secret-test-pod --ignore-not-found
                kubectl delete pod live-config-pod --ignore-not-found
                kubectl delete pod dapi-test-pod --ignore-not-found
                kubectl delete pod test-client --ignore-not-found
                kubectl delete pod dns-test --ignore-not-found

            - Step 2: Delete ConfigMaps
                kubectl delete configmap app-config --ignore-not-found
                kubectl delete configmap nginx-config --ignore-not-found
                kubectl delete configmap live-config --ignore-not-found
                kubectl delete configmap special-config --ignore-not-found 

            - Step 3: Delete Secret
                kubectl delete secret db-credentials --ignore-not-found   

            - Step 4: Verify Cleanup
                Check Pods

                 - kubectl get pods    

            - Check ConfigMaps
                 kubectl get configmaps   

            - Check Secrets
                 kubectl get secrets             