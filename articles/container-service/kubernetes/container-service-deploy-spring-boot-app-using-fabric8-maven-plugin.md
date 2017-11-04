---
title: "Fabric8 Maven eklentisi kullanarak bir yay önyükleme uygulaması dağıtma"
description: "Bu öğreticide, ancak Microsoft Fabric8 eklentisi için Apache Maven kullanarak Azure'da bir yay önyükleme uygulaması dağıtma adımları açıklanmaktadır."
services: container-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.service: container-service
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 09/15/2017
ms.author: yuwzho;robmcm
ms.custom: 
ms.openlocfilehash: 6ec48caef0eebeacf260b0dbc91b175368faea69
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2017
---
# <a name="deploy-a-spring-boot-app-using-the-fabric8-maven-plugin"></a>Fabric8 Maven eklentisi kullanarak bir yay önyükleme uygulaması dağıtma

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

**[Fabric8]**  üzerinde oluşturulan bir açık kaynak çözümü  **[Kubernetes]**, yardımcı olan geliştiriciler uygulamaları Linux kapsayıcı oluşturun.

Bu öğretici, bir Linux ana uygulama dağıtmak için geliştirmek için Maven Fabric8 eklentisi kullanılarak kılavuzluk [Azure kapsayıcı Hizmeti'ni (ACS)].

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşullara sahip olmanız gerekir:

* Bir Azure aboneliği; bir Azure aboneliği zaten sahip değilseniz, etkinleştirebilir, [MSDN abone Avantajlarınızı] veya kaydolun bir [ücretsiz Azure hesabı].
* [Azure komut satırı arabirimi (CLI)].
* Güncel bir [Java Geliştirme Seti (JDK)].
* Apache'nın [Maven] aracını (sürüm 3) yapılandırma.
* A [Git] istemci.
* A [Docker] istemci.

> [!NOTE]
>
> Bu öğretici sanallaştırma gereksinimleri nedeniyle, bir sanal makinede bu makaledeki adımları izleyin olamaz; sanallaştırma özellikleri etkin bir fiziksel bilgisayar kullanmanız gerekir.
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a>Yay önyükleme Docker Başlarken web uygulaması oluştur

Aşağıdaki adımlar yay önyükleme web uygulaması oluşturma ve yerel olarak test size yol.

1. Bir komut istemi açın ve uygulamanızı tutun ve bu dizine değiştirmek için yerel bir dizin oluşturun; Örneğin:
   ```shell
   md /home/GenaSoto/SpringBoot
   cd /home/GenaSoto/SpringBoot
   ```
   --ya da--
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```

1. Kopya [Docker Başlarken yay önyüklemede] örnek proje dizine.
   ```shell
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. Projeyi Dizin Değiştir; Örneğin:
   ```shell
   cd gs-spring-boot-docker/complete
   ```
   --ya da--
   ```shell
   cd gs-spring-boot-docker\complete
   ```

1. Derleme ve örnek uygulamayı çalıştırma için Maven kullanın.
   ```shell
   mvn clean package spring-boot:run
   ```

1. Web uygulaması http://localhost: 8080 veya aşağıdaki göz atarak test `curl` komutu:
   ```shell
   curl http://localhost:8080
   ```

   Görmeniz gerekir bir **Hello Docker World** ileti görüntülenir.

   ![Örnek uygulamayı yerel olarak göz atın][SB01]


## <a name="install-the-kubernetes-command-line-interface-and-create-an-azure-resource-group-using-the-azure-cli"></a>Kubernetes komut satırı arabirimini yükleyin ve Azure CLI kullanarak bir Azure kaynak grubu oluşturun

1. Bir komut istemi açın.

1. Azure hesabınızda oturum açmak için aşağıdaki komutu yazın:
   ```azurecli
   az login
   ```
   Oturum açma işlemini tamamlamak için yönergeleri izleyin
   
   Azure CLI hesaplarınızı listesi görüntülenir; Örneğin:

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "00000000-0000-0000-0000-000000000000",
       "isDefault": false,
       "name": "Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "00000000-0000-0000-0000-000000000000",
       "user": {
         "name": "Gena.Soto@wingtiptoys.com",
         "type": "user"
       }
     }
   ]
   ```

1. Zaten Kubernetes komut satırı arabirimi yoksa (`kubectl`) yüklü, Azure CLI; kullanarak yükleyebileceğiniz örneğin:
   ```azurecli
   az acs kubernetes install-cli
   ```

   > [!NOTE]
   >
   > Linux kullanıcıları bu komutla önek gerekebilir `sudo` Kubernetes CLI dağıtır beri `/usr/local/bin`.
   >
   > Zaten varsa `kubectl`) yüklü ve sürümünüz `kubectl` olduğundan, bu makalenin sonraki bölümlerinde listelenen adımları tamamlamak istediğinizde çok eski aşağıdaki örneğe benzer bir hata iletisi görebilirsiniz:
   >
   > ```
   > error: group map[autoscaling:0x0000000000 batch:0x0000000000 certificates.k8s.io
   > :0x0000000000 extensions:0x0000000000 storage.k8s.io:0x0000000000 apps:0x0000000
   > 000 authentication.k8s.io:0x0000000000 policy:0x0000000000 rbac.authorization.k8
   > s.io:0x0000000000 federation:0x0000000000 authorization.k8s.io:0x0000000000 comp
   > onentconfig:0x0000000000] is already registered
   > ```
   >
   > Bu durumda, yeniden yüklemeniz gerekecek `kubectl` sürümünüzü güncelleştirmek için.
   >

1. Bu öğreticide kullanacaksınız Azure kaynakları için bir kaynak grubu oluşturun; Örneğin:
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=westeurope
   ```
   Konumlar:  
      * *wingtiptoys kubernetes* , kaynak grubu için benzersiz bir ad  
      * *westeurope* uygulamanız için uygun bir coğrafi konum  

   Azure CLI, kaynak grubu oluşturma sonuçlarını görüntüler; Örneğin:  

   ```json
   {
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/wingtiptoys-kubernetes",
     "location": "westeurope",
     "managedBy": null,
     "name": "wingtiptoys-kubernetes",
     "properties": {
       "provisioningState": "Succeeded"
     },
     "tags": null
   }
   ```


## <a name="create-a-kubernetes-cluster-using-the-azure-cli"></a>Azure CLI kullanarak bir Kubernetes kümesi oluşturma

1. Kubernetes küme, yeni kaynak grubu oluşturmak; Örneğin:  
   ```azurecli 
   az acs create --orchestrator-type kubernetes --resource-group wingtiptoys-kubernetes --name wingtiptoys-cluster --generate-ssh-keys --dns-prefix=wingtiptoys
   ```
   Konumlar:  
      * *wingtiptoys kubernetes* bu makalenin, kaynak grubu adı  
      * *wingtiptoys küme* Kubernetes kümeniz için benzersiz bir ad
      * *wingtiptoys* uygulamanız için benzersiz bir ad DNS adı

   Azure CLI, küme oluşturma sonuçlarını görüntüler; Örneğin:  

   ```json
   {
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/wingtiptoys-kubernetes/providers/Microsoft.Resources/deployments/azurecli0000000000.00000000000",
     "name": "azurecli0000000000.00000000000",
     "properties": {
       "correlationId": "00000000-0000-0000-0000-000000000000",
       "debugSetting": null,
       "dependencies": [],
       "mode": "Incremental",
       "outputs": {
         "masterFQDN": {
           "type": "String",
           "value": "wingtiptoysmgmt.westeurope.cloudapp.azure.com"
         },
         "sshMaster0": {
           "type": "String",
           "value": "ssh azureuser@wingtiptoysmgmt.westeurope.cloudapp.azure.com -A -p 22"
         }
       },
       "parameters": {
         "clientSecret": {
           "type": "SecureString"
         }
       },
       "parametersLink": null,
       "providers": [
         {
           "id": null,
           "namespace": "Microsoft.ContainerService",
           "registrationState": null,
           "resourceTypes": [
             {
               "aliases": null,
               "apiVersions": null,
               "locations": [
                 "westeurope"
               ],
               "properties": null,
               "resourceType": "containerServices"
             }
           ]
         }
       ],
       "provisioningState": "Succeeded",
       "template": null,
       "templateLink": null,
       "timestamp": "2017-09-15T01:00:00.000000+00:00"
     },
     "resourceGroup": "wingtiptoys-kubernetes"
   }
   ```

1. Yeni Kubernetes kümeniz için kimlik bilgilerinizi yükleyin; Örneğin:  
   ```azurecli 
   az acs kubernetes get-credentials --resource-group=wingtiptoys-kubernetes --name wingtiptoys-cluster
   ```

1. Aşağıdaki komutla bağlantınızı doğrulayın:
   ```shell 
   kubectl get nodes
   ```

   Düğümler ve aşağıdaki örnekteki gibi durumlar listesini görmeniz gerekir:

   ```shell
   NAME                    STATUS                     AGE       VERSION
   k8s-agent-00000000-0    Ready                      5h        v1.6.6
   k8s-agent-00000000-1    Ready                      5h        v1.6.6
   k8s-agent-00000000-2    Ready                      5h        v1.6.6
   k8s-master-00000000-0   Ready,SchedulingDisabled   5h        v1.6.6
   ```


## <a name="create-a-private-azure-container-registry-using-the-azure-cli"></a>Azure CLI kullanarak bir özel Azure kapsayıcı kayıt defteri oluşturma

1. Özel Azure kapsayıcı kayıt Docker görüntünüzü barındırmak için kaynak grubu oluşturun; Örneğin:
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoys-kubernetes --location westeurope --name wingtiptoysregistry --sku Basic
   ```
   Konumlar:  
      * *wingtiptoys kubernetes* bu makalenin, kaynak grubu adı  
      * *wingtiptoysregistry* özel kayıt defteri için benzersiz bir ad
      * *westeurope* uygulamanız için uygun bir coğrafi konum  

   Azure CLI, kayıt defteri oluşturma sonuçlarını görüntüler; Örneğin:  

   ```json
   {
     "adminUserEnabled": true,
     "creationDate": "2017-09-15T01:00:00.000000+00:00",
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/wingtiptoys-kubernetes/providers/Microsoft.ContainerRegistry/registries/wingtiptoysregistry",
     "location": "westeurope",
     "loginServer": "wingtiptoysregistry.azurecr.io",
     "name": "wingtiptoysregistry",
     "provisioningState": "Succeeded",
     "resourceGroup": "wingtiptoys-kubernetes",
     "sku": {
       "name": "Basic",
       "tier": "Basic"
     },
     "storageAccount": {
       "name": "wingtiptoysregistr000000"
     },
     "tags": {},
     "type": "Microsoft.ContainerRegistry/registries"
   }
   ```

1. Kapsayıcı kaydınız parolasını Azure CLI üzerinden alır.
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```

   Azure CLI kaydınız parolasını görüntülenir; Örneğin:  

   ```json
   {
     "name": "password",
     "value": "AbCdEfGhIjKlMnOpQrStUvWxYz"
   }
   ```

1. Maven yüklemeniz için yapılandırma dizinine gidin (varsayılan ~/.m2/ veya C:\Users\username\.m2) ve açık *settings.xml* dosyasını bir metin düzenleyicisiyle.

1. Azure kapsayıcı kayıt defteri URL'si, kullanıcı adı ve parola yeni bir ekleme `<server>` koleksiyonunda *settings.xml* dosya.
`id` Ve `username` kayıt adı. Kullanım `password` değerinden (tırnaklar olmadan) önceki komutu.

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry.azurecr.io</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. Yay önyükleme uygulamanız için projeyi dizinine gidin (örneğin, "*C:\SpringBoot\gs-spring-boot-docker\complete*"veya"*/home/GenaSoto/SpringBoot/gs-spring-boot-docker/complete* ") ve açın *pom.xml* dosyasını bir metin düzenleyicisiyle.

1. Güncelleştirme `<properties>` koleksiyonunda *pom.xml* Azure kapsayıcı kayıt için oturum açma sunucusu değerle dosyası.

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. Güncelleştirme `<plugins>` koleksiyonunda *pom.xml* dosya böylece `<plugin>` Azure kapsayıcı kayıt için oturum açma sunucusu adresi ve kayıt defteri adını içerir.

   ```xml
   <plugin>
     <groupId>com.spotify</groupId>
     <artifactId>dockerfile-maven-plugin</artifactId>
     <version>1.3.4</version>
     <configuration>
       <repository>${docker.image.prefix}/${project.artifactId}</repository>
       <serverId>${docker.image.prefix}</serverId>
       <registryUrl>https://${docker.image.prefix}</registryUrl>
     </configuration>
   </plugin>
   ```

1. Yay önyükleme uygulamanız için projeyi dizinine gidin ve Docker kapsayıcısı oluşturmak ve görüntü için kayıt anında için aşağıdaki Maven komutunu çalıştırın:

   ```shell
   mvn package dockerfile:build -DpushImage
   ```

   Maven yapınızın sonuçlarını görüntüler; Örneğin:  

   ```shell
   [INFO] ----------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ----------------------------------------------------
   [INFO] Total time: 38.113 s
   [INFO] Finished at: 2017-09-15T02:00:00-07:00
   [INFO] Final Memory: 47M/338M
   [INFO] ----------------------------------------------------
   ```


## <a name="configure-your-spring-boot-app-to-use-the-fabric8-maven-plugin"></a>Yay önyükleme uygulamanızı Fabric8 Maven eklentisini kullanmak için yapılandırma

1. Yay önyükleme uygulamanız için projeyi dizinine gidin (örneğin: "*C:\SpringBoot\gs-spring-boot-docker\complete*"veya"*/home/GenaSoto/SpringBoot/gs-spring-boot-docker / tam*") ve açın *pom.xml* dosyasını bir metin düzenleyicisiyle.

1. Güncelleştirme `<plugins>` koleksiyonunda *pom.xml* dosya Fabric8 Maven eklenti eklemek için:

   ```xml
   <plugin>
     <groupId>io.fabric8</groupId>
     <artifactId>fabric8-maven-plugin</artifactId>
     <version>3.5.30</version>
     <configuration>
       <ignoreServices>false</ignoreServices>
       <registry>${docker.image.prefix}</registry>
     </configuration>
   </plugin>
   ```

1. Yay önyükleme uygulamanız için ana kaynak dizinine gidin (örneğin: "*C:\SpringBoot\gs-spring-boot-docker\complete\src\main*"veya"*/home/GenaSoto/SpringBoot/gs-spring-boot-docker / tamamlamak/src/main*") ve adlı yeni bir klasör oluşturun"*fabric8*".

1. Üç YAML parça dosyaları yeni oluşturma *fabric8* klasörü:

   a. Adlı bir dosya oluşturun **deployment.yml** aşağıdaki içeriğe sahip:
      ```yaml
      apiVersion: extensions/v1beta1
      kind: Deployment
      metadata:
        name: ${project.artifactId}
        labels:
          run: gs-spring-boot-docker
      spec:
        replicas: 1
        selector:
          matchLabels:
            run: gs-spring-boot-docker
        strategy:
          rollingUpdate:
            maxSurge: 1
            maxUnavailable: 1
          type: RollingUpdate
        template:
          metadata:
            labels:
              run: gs-spring-boot-docker
          spec:
            containers:
            - image: ${docker.image.prefix}/${project.artifactId}:latest
              name: gs-spring-boot-docker
              imagePullPolicy: Always
              ports:
              - containerPort: 8080
            imagePullSecrets:
              - name: mysecrets
      ```

   b. Adlı bir dosya oluşturun **secrets.yml** aşağıdaki içeriğe sahip:
      ```yaml
      apiVersion: v1
      kind: Secret
      metadata:
        name: mysecrets
        namespace: default
        annotations:
          maven.fabric8.io/dockerServerId:  ${docker.image.prefix}
      type: kubernetes.io/dockercfg
      ```

   c. Adlı bir dosya oluşturun **service.yml** aşağıdaki içeriğe sahip:
      ```yaml
      apiVersion: v1
      kind: Service
      metadata:
        name: ${project.artifactId}
        labels:
          expose: "true"
      spec:
        ports:
        - port: 80
          targetPort: 8080
        type: LoadBalancer
      ```

1. Kubernetes kaynak listesi dosyasını oluşturmak için aşağıdaki Maven komutunu çalıştırın:

   ```shell
   mvn fabric8:resource
   ```

   Bu komut tüm Kubernetes kaynak yaml dosyaları altında birleştirir *src/main/fabric8* Kubernetes küme doğrudan veya bir helm grafik Excel'e uygulanabilir Kubernetes kaynak listesini içeren bir YAML dosyasının klasör.

   Maven yapınızın sonuçlarını görüntüler; Örneğin:  

   ```shell
   [INFO] ----------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ----------------------------------------------------
   [INFO] Total time: 16.744 s
   [INFO] Finished at: 2017-09-15T02:35:00-07:00
   [INFO] Final Memory: 30M/290M
   [INFO] ----------------------------------------------------
   ```

1. Kaynak listesi dosyası Kubernetes kümenize uygulamak için aşağıdaki Maven komutunu çalıştırın:

   ```shell
   mvn fabric8:apply
   ```

   Maven yapınızın sonuçlarını görüntüler; Örneğin:  

   ```shell
   [INFO] ----------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ----------------------------------------------------
   [INFO] Total time: 14.814 s
   [INFO] Finished at: 2017-09-15T02:41:00-07:00
   [INFO] Final Memory: 35M/288M
   [INFO] ----------------------------------------------------
   ```

1. Dış IP adresi kullanarak kümeye uygulama dağıtıldığında, sorgu `kubectl` uygulama; örneğin:
   ```shell
   kubectl get svc -w
   ```

   `kubectl`İç ve dış IP adreslerinizi görüntülenir; Örneğin:
   
   ```shell
   NAME                    CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
   kubernetes              10.0.0.1     <none>        443/TCP        19h
   gs-spring-boot-docker   10.0.242.8   13.65.196.3   80:31215/TCP   3m
   ```
   
   Dış IP adresi, uygulamanızı bir web tarayıcısında açmak için kullanabilirsiniz.

   ![Örnek uygulamayı dışarıdan Gözat][SB02]

## <a name="delete-your-kubernetes-cluster"></a>Kubernetes kümenizi Sil

Kubernetes kümenizi artık gerekli olmadığında kullanabileceğiniz `az group delete` kendi ilgili kaynakların tümünü kaldırır kaynak grubunu kaldırmak için komutu örneğin:

   ```azurecli
   az group delete --name wingtiptoys-kubernetes --yes --no-wait
   ```

## <a name="next-steps"></a>Sonraki adımlar

Azure üzerinde yay önyükleme uygulamalarında kullanma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Yay önyükleme uygulamasını Azure App Service'e dağıtma](../../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)
* [Azure kapsayıcı Hizmeti'nde Linux'ta yay önyükleme uygulamasını dağıtma](container-service-deploy-spring-boot-app-on-linux.md)
* [Azure kapsayıcı hizmeti Kubernetes kümesinde yay önyükleme uygulamasını dağıtma](container-service-deploy-spring-boot-app-on-kubernetes.md)

Azure’u Java ile kullanma hakkında daha fazla bilgi edinmek için bkz. [Azure Java Geliştirici Merkezi] ve [Visual Studio Team Services için Java Araçları].

Docker örnek proje yay önyüklemede hakkında daha fazla ayrıntı için bkz: [Docker Başlarken yay önyüklemede].

Kendi yay önyükleme uygulamaları ile çalışmaya başlama hakkında bilgi için bkz: **yay Initializr** adresindeki <https://start.spring.io/>.

Yay Initializr en basit bir yay önyükleme uygulaması oluşturma ile çalışmaya başlama hakkında daha fazla bilgi için bkz: <https://start.spring.io/>.

Azure ile özel Docker görüntülerinizi kullanımı için ek örnekler için bkz: [Linux Azure Web uygulaması için özel bir Docker görüntü kullanarak].

<!-- URL List -->

[Azure komut satırı arabirimi (CLI)]: /cli/azure/overview
[Azure kapsayıcı Hizmeti'ni (ACS)]: https://azure.microsoft.com/services/container-service/
[Azure Java Geliştirici Merkezi]: https://azure.microsoft.com/develop/java/
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Linux Azure Web uygulaması için özel bir Docker görüntü kullanarak]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[Fabric8]: https://fabric8.io/
[ücretsiz Azure hesabı]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Geliştirme Seti (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Visual Studio Team Services için Java Araçları]: https://java.visualstudio.com/
[Kubernetes]: https://kubernetes.io/
[Maven]: http://maven.apache.org/
[MSDN abone Avantajlarınızı]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Docker Başlarken yay önyüklemede]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/container-service-deploy-spring-boot-app-using-fabric8-maven-plugin/SB01.png
[SB02]: ./media/container-service-deploy-spring-boot-app-using-fabric8-maven-plugin/SB02.png
