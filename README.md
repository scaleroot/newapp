# newapp

# edit your traefik deployment for adding multiple ports 
## command to edit traefik deployment
## kubectl edit deployment <your_traefik_deployment>

## add required ports in spec.containers.args section

ex: - --entrypoints.<port_name>.address=:<your_port>/tcp

## Also add same port in spec.containers.ports section

ex:  - containerPort: <your_port>
       name: <port_name>
       protocol: TCP
       
## save and exit the deployment.
## Wait for deployment to be configured according to the changes made 

## once it is configured patch the traefik service with following command

kubectl patch svc admin-traefik -p '{"spec": {"ports": [{"port": <your_port>,"targetPort": "<port_name>","name": "myport"}]}}'

## add your port name in spec.entryPoints section ingressRoute.yaml file 

ex: spec:
      entryPoints:
      - web
      - websecure
      - <port_name> ## new port
    
    
## add your port in ingressRoute.yaml file 

ex:

- kind: Rule
    match: Host(`<your_hostname>`)
    services:
    - name: <your_service_name>
      port: <your_port>
      
 ## add same port in service.yaml file also
 
 ex:  - port: <your_port>
        targetPort: <targetport> 
        protocol: <protocol>
        name: <name>
        
        
