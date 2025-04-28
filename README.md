# elk-installer
This project allow to install ECK on Kubernetes after that also install Elasticsearch and Kibana

### Note. this certicated on Kubernetes 1.32.0

## Requerimients
### Hardware
Minimum 3 nodes with each one of 64 GB of RAM, Disk 400 gb and 16 vcore cpu

### Software
Kubernetes 1.32.0
UPDATE the variable called KUBECOFIG in this repository with the k8s cluster wished

## Installer
Despues que el pipeline github action a terminado OK realizar estos pasos de forma manual

Access lb-kibana svc

![image](https://github.com/user-attachments/assets/2ece4594-1593-42df-af9b-8be25ebeb425)


user: elastic
password: la recuperada del secreto k8s

![image](https://github.com/user-attachments/assets/42a2dd82-44a4-4231-b433-2df142b1782d)

Seleccionar: Explore on my own


luego aparece esto

![image](https://github.com/user-attachments/assets/3d24be5e-d7c4-4d9d-8c49-4cc3d6e1ebb9)

Hasta ahora no hay nada configurado

Luego ir a integrations, para incluir la Integration agent de Kubernetes monitoring

![image](https://github.com/user-attachments/assets/5316e35b-e815-4cd5-acfa-603b9b30a2af)

Seleccionar option containers

![image](https://github.com/user-attachments/assets/c64ac3ca-39f5-4ebe-83e3-b802b3606126)

Seleccione Kubernetes

![image](https://github.com/user-attachments/assets/f19b61cb-89ce-4c81-be34-66ccf27040aa)

Luego dar al boton siguiente

![image](https://github.com/user-attachments/assets/969fe0bf-6e39-4917-aa6a-133e47eca598)

activar todas las opciones disponibles
Luego guardar y continuar en el siguiente boton

![image](https://github.com/user-attachments/assets/8154ce90-8861-4d7f-a405-a383023590db)


Luego aparecera el siguiente modal y se debe presionar el boton azul

![image](https://github.com/user-attachments/assets/547aff69-4d08-4609-99a0-590e9c2447ad)

luego incluir los agentes, en el siguiente boton

![image](https://github.com/user-attachments/assets/3a620011-ffaf-498f-a11a-44f4213cb52b)

Luego seleccionar run standalone

![image](https://github.com/user-attachments/assets/6a8cbced-9d23-49a7-a2f1-9d5ab03004cc)

Muestra esto, con la cual se debe darle al boton generar CREATE API KEY 

![image](https://github.com/user-attachments/assets/b0c40441-7847-441a-8dc8-5bc6441f12f7)

Luego en una shell con acceso al usuario, set o export la variable KUBECONFIG apuntando hacia el cluster k8s en cuestion
defina las siguientes 2 variables adicionales con los valores recuperados en los pasos previos y en el output del pipeline:

 `export API_KEY=THEVALUEGENERATED`
 `export PASSWORD=THEVALUEGENERATED`



Aplicar el manifiesto que esta en este mismo repositorio:

`envsubst < agent-full.yaml | kubectl apply -f -`

Verifique que el agent inicio correctamente:

`kubectl get pods -n kube-system`


### Instalar un monitor de microservicio
Si requiere monitorear un microservicio particular con el objetivo de validar incluso alertas posteriores de SLO
debe instalar un agente en el cluster con el mediante el uso de los siguientes pasos

1. Identifique direccion IP externa o interna del microservicio y definina una variable en un ambiente que tenga KUBECONFIG definido como variables y accesible hacia el k8s target
   
   `export IPMICROSERVICE=THEVALUEGENERATED`
   
2. Defina la variable password de elastic en una variable tambien asi
 
   `export PASSWORD=THEVALUEGENERATED`

Aplique los cambios asi:
`envsubst < agent-uptime.yml | kubectl apply -f -`

Si todo esta OK puede revisar el uptime en el dashboard del Kibana similar a este:
![image](https://github.com/user-attachments/assets/531b7304-ecf1-41f7-979a-7099fabca812)


