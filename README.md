# info-k8s

Para verificar versão a ser usada dos componentes do serverNodes
Use:

```
kubectl api-resourses
```



Existem 4 categorias que não podem faltar na criação de um manifestor
São eles:
- apiVersion (Para saber qual use o comando *kubectl api-resources)
- kind (Tipo de objeto a ser criado: Pod, ReplicaSet ....)
- metadata: (Dados a serem armazenados, inclusic labels) 
- spec: (Especificações)

# Replicaset
Replicaset garante
 - Resiliencia - Ao derrubar um pod ele poem outro no lugar
 - Escalabilidade: *Podemos subir mais do que um POD escalando o serviço (Us o **scale** ou o comando inline **kubectl scale replicaset [replicaset_name] --replicas [num_replicas])*

 # Deployment
 Garante o **Gerenciamento de versão**. Ele cria um outro replicaset em paralelo e gradativamente vai alterando oos pods do replicaset antigo para o novo. Somente quando tudo estiver OK ele derruba o replicaset anterior
 Caso de algum erro no novo replicaset fica facil retornar (rollback) para o anterior
 ## Voltando versão
 ```
 kubectl rollout undo deployment [deployment_name]
 ```

 # Services
 Com o services voce pode expor sua aplicação para não ter que dar fast-forward para cada vez que quiser direcionar um pod para uma porta de sua maquina. Sem o srvices usariamos o comand
 ```
 kubectl port-forward pod/[pod_name] 8080:80
 ```

 Existem três tipos de services, sao eles
 - ClusterIp
 - NodePort
 - LoadBalancer

 ## ClusterIP
 Usado apenas para se comunicar internamente no cluster kubernetes, sempre que ele é usado é gerado um ip de acesso ao ClusterIP. Este ip não deve ser usado de modo algum. Acesse usando o proprio nome dele gerado.

 ## NodePort
 Já é acessivel externamente mas sempre que for expor externamente ele usa uma porta que fica entre 30.000 e 32767.   
 Para acessar o nodePort você usaria o numero de ip de seu cluster juntamente com a porta que ele foi criado no range informado acima.   
 Utilizado em cenários onPremisses onde não tenho uso de tecnologia cloud

 Para não precisar de descobrir o IP do container a qual deseja acessar e incluir o numero da port de range do service você pode criar o k3d, lembrando que isto é para trabalho local, um bind de porta
 ```
k3d create cluster [cluster_name] --servers [servers_numbers] --agents [agents_number] -p "8080:80@loadbalancer"
 ```

 ## LoadBalancer
 O usar serviços do tipo Nuvem pode ser usado loadbalancer. Sempre que cria um service deste tipo é criado um loadBalancer com o Ip que dá acesso a este service utilizando srviço de nuvem.   
 Existe um jeito de usa-lo em cenários onPremisses usando uma ferramente chamada matlelb


# Rodar o k3x Graphical
flatpak run com.github.inercia.k3x
