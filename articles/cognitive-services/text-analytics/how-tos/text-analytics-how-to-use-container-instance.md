---
title: Kubernetes hizmet çalıştırma
titleSuffix: Text Analytics -  Azure Cognitive Services
description: Dil algılama kapsayıcı ile çalışan bir örnek, Azure Kubernetes hizmetine dağıtın ve bir web tarayıcısında test.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 01/22/2019
ms.author: diberry
ms.openlocfilehash: 3541376331725fddcd58d94625f5d761ef159c97
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59526513"
---
# <a name="deploy-the-language-detection-container-to-azure-kubernetes-service"></a>Dil algılama için Azure Kubernetes hizmeti dağıtma

Dil algılama kapsayıcısı dağıtmayı öğrenin. Bu yordam, nasıl yerel Docker kapsayıcılarını oluşturun, kapsayıcılar, kendi özel kapsayıcı kayıt defterine iletin, bir Kubernetes kümesi içinde kapsayıcı çalıştırmanızın ve bir web tarayıcısında test gösterir. 

## <a name="prerequisites"></a>Önkoşullar

Bu yordam, yüklü ve yerel olarak çalıştırma çeşitli araçlar gerektirir. Azure Cloud Shell'i kullanmayın. 

* Bir Azure aboneliği kullanın. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* [Git](https://git-scm.com/downloads) kopyalamanızı, bir işletim sistemi için [örnek](https://github.com/Azure-Samples/cognitive-services-containers-samples) bu yordamda kullanılan. 
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). 
* [Docker altyapısı](https://www.docker.com/products/docker-engine) ve Docker CLI'yı bir konsol penceresi içinde çalıştığını doğrulayın.
* [kubectl](https://storage.googleapis.com/kubernetes-release/release/v1.13.1/bin/windows/amd64/kubectl.exe). 
* Doğru fiyatlandırma katmanı ile bir Azure kaynağı. Tüm fiyatlandırma katmanları bu kapsayıcısı ile çalışır:
    * **Metin analizi** kaynak F0 veya standart fiyatlandırma katmanlarını yalnızca.
    * **Bilişsel Hizmetler** kaynak fiyatlandırma katmanı S0 ile.

## <a name="running-the-sample"></a>Örneği çalıştırma

Bu yordam, yükler ve dil algılama için Bilişsel Hizmetleri kapsayıcı örneği çalıştırır. Örnek istemci uygulaması biri diğeri Bilişsel hizmetler kapsayıcısı için iki kapsayıcı vardır. Hem bu görüntüleri kendi Azure Container Registry'ye gönderme gerekir. Kendi kayıt defterini oldukları sonra bu görüntüleri erişmek ve kapsayıcıları çalıştırmak için Azure Kubernetes hizmeti oluşturun. Kapsayıcıları çalışırken kullanması **kubectl** kapsayıcıları performansını izlemek için CLI. İstemci uygulaması ile bir HTTP isteği erişmek ve sonuçlarını görebilirsiniz. 

![Örnek kapsayıcı çalıştırmanın kavramsal fikir](../media/how-tos/container-instance-sample/containers.png)

## <a name="the-sample-containers"></a>Örnek kapsayıcı

Örnek, bir ön uç Web sitesi için iki kapsayıcı görüntülerine sahiptir. İkinci görüntü, metin (kültür) algılanan dilin döndüren dil algılama kapsayıcısıdır. İşiniz bittiğinde her iki kapsayıcıları bir dış IP erişilebilir. 

### <a name="the-language-frontend-container"></a>Dil ön uç kapsayıcısı

Bu Web sitesi, dil algılama uç nokta isteği yapan kendi istemci-tarafı uygulaması eşdeğerdir. Yordamı tamamladığınızda, bir tarayıcı ile Web sitesi kapsayıcısında erişerek bir karakter dizesi, algılanan dilin elde `http://<external-IP>/<text-to-analyze>`. Bu URL'yi örneğidir `http://132.12.23.255/helloworld!`. Tarayıcıda sonucudur `English`.

### <a name="the-language-container"></a>Dil kapsayıcı

Dil algılama kapsayıcısında belirli Bu yordam, herhangi bir dış isteğinin erişilebilir. Standart Bilişsel hizmetler kapsayıcı özgü dil algılama API'si kullanılabilir olacak şekilde herhangi bir yolla kapsayıcısı değişip değişmediğini. 

Bu, bu API bir POST isteği dil algılama için kapsayıcıdır. Barındırılan Swagger bilgi, kapsayıcıdan hakkında daha fazla bilgi tüm Bilişsel hizmetler kapsayıcılarla gibi `http://<external-IP>:5000/swagger/index.html`. 

Bağlantı noktası 5000 Bilişsel hizmetler kapsayıcılar ile kullanılan varsayılan bağlantı noktasıdır. 

## <a name="create-azure-container-registry-service"></a>Azure Container Registry hizmeti oluşturma

Kapsayıcıyı dağıtmak için Azure Kubernetes Service için kapsayıcı görüntülerini erişilebilir olması gerekir. Görüntüleri barındırmak için kendi Azure Container Registry hizmeti oluşturun. 

1. Azure CLI için oturum açın 

    ```azurecli
    az login
    ```

1. Adlı bir kaynak grubu oluşturma `cogserv-container-rg` bu yordamda oluşturduğunuz her kaynak tutacak.

    ```azurecli
    az group create --name cogserv-container-rg --location westus
    ```

1. Kendi Azure Container Registry adınız biçimiyle oluşturup `registry`, gibi `pattyregistry`. Kullanmayın tire veya adında alt çizgi.

    ```azurecli
    az acr create --resource-group cogserv-container-rg --name pattyregistry --sku Basic
    ```

    Sonuçları almak için Kaydet **loginServer** özelliği. Bu barındırılan kapsayıcının adresi, daha sonra kullanılan bir parçası olacak `language.yml` dosya.

    ```console
    >az acr create --resource-group cogserv-container-rg --name pattyregistry --sku Basic
    {
        "adminUserEnabled": false,
        "creationDate": "2019-01-02T23:49:53.783549+00:00",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/cogserv-container-rg/providers/Microsoft.ContainerRegistry/registries/pattyregistry",
        "location": "westus",
        "loginServer": "pattyregistry.azurecr.io",
        "name": "pattyregistry",
        "provisioningState": "Succeeded",
        "resourceGroup": "cogserv-container-rg",
        "sku": {
            "name": "Basic",
            "tier": "Basic"
        },
        "status": null,
        "storageAccount": null,
        "tags": {},
        "type": "Microsoft.ContainerRegistry/registries"
    }
    ```

1. Kapsayıcı kayıt defterinizde oturum açın. Kayıt defterinize görüntü gönderebilmeniz oturum açmanız gerekir.

    ```azurecli
    az acr login --name pattyregistry
    ```

## <a name="get-website-docker-image"></a>Web sitesi Docker görüntüsünü Al 

1. Bu yordamda kullanılan örnek kod Bilişsel hizmetler kapsayıcı örnekleri depodur. Örneği yerel bir kopyasına sahip depoyu kopyalayın.

    ```console
    git clone https://github.com/Azure-Samples/cognitive-services-containers-samples
    ```

    Depoyu yerel bilgisayarınızda eklendiğinde, Web sitesi bulma [\dotnet\Language\FrontendService](https://github.com/Azure-Samples/cognitive-services-containers-samples/tree/master/dotnet/Language/FrontendService) dizin. Bu Web sitesi, dil algılama API'si dil algılama kapsayıcıda barındırılan çağırma istemci uygulaması olarak görev yapar.  

1. Bu Web sitesi için Docker görüntüsü oluşturun. Konsol olduğundan emin olun [\FrontendService](https://github.com/Azure-Samples/cognitive-services-containers-samples/tree/master/dotnet/Language/FrontendService) Dockerfile bulunduğu aşağıdaki komutu çalıştırdığınızda, dizin:

    ```console
    docker build -t language-frontend -t pattiyregistry.azurecr.io/language-frontend:v1 . 
    ```

    Ekleme gibi etiketi bir sürüm biçimi ile kapsayıcı kayıt defterinizin sürümünü izlemek için `v1`. 

1. Görüntüyü kapsayıcı kayıt defterinize gönderin. Bu birkaç dakika sürebilir. 

    ```console
    docker push pattyregistry.azurecr.io/language-frontend:v1
    ```

    Alırsanız bir `unauthorized: authentication required` hata, oturum açma ile `az acr login --name <your-container-registry-name>` komutu. 

    İşlem tamamlandığında, sonuçları şuna benzer olmalıdır:

    ```console
    > docker push pattyregistry.azurecr.io/language-frontend:v1
    The push refers to repository [pattyregistry.azurecr.io/language-frontend]
    82ff52ee6c73: Pushed
    07599c047227: Pushed
    816caf41a9a1: Pushed
    2924be3aed17: Pushed
    45b83a23806f: Pushed
    ef68f6734aa4: Pushed
    v1: digest: sha256:31930445deee181605c0cde53dab5a104528dc1ff57e5b3b34324f0d8a0eb286 size: 1580
    ```

## <a name="get-language-detection-docker-image"></a>Dil algılama Docker görüntüsünü Al 

1. Docker görüntüsünün son sürümünü yerel makineye çekin. Bu birkaç dakika sürebilir. Bu kapsayıcı daha yeni bir sürümü varsa, değerini değiştirmek `1.1.006770001-amd64-preview` sürüme. 

    ```console
    docker pull mcr.microsoft.com/azure-cognitive-services/language:1.1.006770001-amd64-preview
    ```

1. Kapsayıcı kayıt defterinizde görüntüsüyle etiketi. En son sürümü bulmak ve sürümü değiştirin `1.1.006770001-amd64-preview` daha yeni bir sürüme sahipseniz. 

    ```console
    docker tag mcr.microsoft.com/azure-cognitive-services/language pattiyregistry.azurecr.io/language:1.1.006770001-amd64-preview
    ```

1. Görüntüyü kapsayıcı kayıt defterinize gönderin. Bu birkaç dakika sürebilir. 

    ```console
    docker push pattyregistry.azurecr.io/language:1.1.006770001-amd64-preview
    ```

## <a name="get-container-registry-credentials"></a>Kapsayıcı kayıt defteri kimlik bilgilerini alma

Aşağıdaki adımlar, bu yordamın sonraki adımlarında oluşturduğunuz Azure Kubernetes hizmeti ile kapsayıcı kayıt defterinizde bağlanmak için gerekli bilgileri almak için gereklidir.

1. Hizmet sorumlusu oluşturma.

    ```azurecli
    az ad sp create-for-rbac --skip-assignment
    ```

    Sonuçları Kaydet `appId` 3. adımda atanan parametresi için değer `<appId>`. Kaydet `password` sonraki bölüm CLIENT-secret parametresi için `<client-secret>`.

    ```console
    > az ad sp create-for-rbac --skip-assignment
    {
      "appId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "displayName": "azure-cli-2018-12-31-18-39-32",
      "name": "http://azure-cli-2018-12-31-18-39-32",
      "password": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "tenant": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    }
    ```

1. Kapsayıcı kayıt defteri kimliğinizi alma

    ```azurecli
    az acr show --resource-group cogserv-container-rg --name pattyregistry --query "id" --o table
    ```

    Çıktı kapsam parametre değeri için Kaydet `<acrId>`, sonraki adımda. Bunu şu şekilde görünür:

    ```console
    > az acr show --resource-group cogserv-container-rg --name pattyregistry --query "id" --o table
    /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/cogserv-container-rg/providers/Microsoft.ContainerRegistry/registries/pattyregistry
    ```

    Bu bölümdeki 3. adım için tam değerini kaydedin. 

1. Kapsayıcı kayıt defterinizde depolanmış görüntüleri kullanmasına olanak AKS kümesi için doğru erişim vermek için bir rol ataması oluşturun. Değiştirin `<appId>` ve `<acrId>` önceki iki adımda toplanan değerlere sahip.

    ```azurecli
    az role assignment create --assignee <appId> --scope <acrId> --role Reader
    ```

## <a name="create-azure-kubernetes-service"></a>Azure Kubernetes hizmeti oluşturma

1. Kubernetes kümesi oluşturun. Önceki bölümlerde name parametresi dışında tüm parametre değerlerini arasındadır. Bir ad seçin ve amacı, gibi oluşturan kişiyi gösterir `patty-kube`. 

    ```azurecli
    az aks create --resource-group cogserv-container-rg --name patty-kube --node-count 2  --service-principal <appId>  --client-secret <client-secret>  --generate-ssh-keys
    ```

    Bu adım birkaç dakika sürebilir. Sonuç olur: 

    ```console
    > az aks create --resource-group cogserv-container-rg --name patty-kube --node-count 2  --service-principal <appId>  --client-secret <client-secret>  --generate-ssh-keys
    {
      "aadProfile": null,
      "addonProfiles": null,
      "agentPoolProfiles": [
        {
          "count": 2,
          "dnsPrefix": null,
          "fqdn": null,
          "maxPods": 110,
          "name": "nodepool1",
          "osDiskSizeGb": 30,
          "osType": "Linux",
          "ports": null,
          "storageProfile": "ManagedDisks",
          "vmSize": "Standard_DS1_v2",
          "vnetSubnetId": null
        }
      ],
      "dnsPrefix": "patty-kube--65a101",
      "enableRbac": true,
      "fqdn": "patty-kube--65a101-341f1f54.hcp.westus.azmk8s.io",
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/cogserv-container-rg/providers/Microsoft.ContainerService/managedClusters/patty-kube",
      "kubernetesVersion": "1.9.11",
      "linuxProfile": {
        "adminUsername": "azureuser",
        "ssh": {
          "publicKeys": [
            {
              "keyData": "ssh-rsa AAAAB3NzaC...ohR2d81mFC
            }
          ]
        }
      },
      "location": "westus",
      "name": "patty-kube",
      "networkProfile": {
        "dnsServiceIp": "10.0.0.10",
        "dockerBridgeCidr": "172.17.0.1/16",
        "networkPlugin": "kubenet",
        "networkPolicy": null,
        "podCidr": "10.244.0.0/16",
        "serviceCidr": "10.0.0.0/16"
      },
      "nodeResourceGroup": "MC_patty_westus",
      "provisioningState": "Succeeded",
      "resourceGroup": "cogserv-container-rg",
      "servicePrincipalProfile": {
        "clientId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "keyVaultSecretRef": null,
        "secret": null
      },
      "tags": null,
      "type": "Microsoft.ContainerService/ManagedClusters"
    }
    ```

    Hizmeti oluşturuldu ancak bu Web kapsayıcısı veya dil algılama kapsayıcı henüz gerekli değildir.  

1. Kubernetes küme kimlik bilgilerini alın. 

    ```azurecli
    az aks get-credentials --resource-group cogserv-container-rg --name patty-kube
    ```

## <a name="load-the-orchestration-definition-into-your-kubernetes-service"></a>Kubernetes hizmetinizi düzenleme tanımı yüklenemiyor

Bu bölümde kullanan **kubectl** CLI ile Azure Kubernetes hizmeti bahsedeceğiz. 

1. Orchestration tanımı yüklemeden önce kontrol **kubectl** düğümleri erişebilir.

    ```console
    kubectl get nodes
    ```

    Yanıt şu şekilde görünür:

    ```console
    > kubectl get nodes
    NAME                       STATUS    ROLES     AGE       VERSION
    aks-nodepool1-13756812-0   Ready     agent     6m        v1.9.11
    aks-nodepool1-13756812-1   Ready     agent     6m        v1.9.11
    ```

1. Aşağıdaki dosya kopyalayın ve adlandırın `language.yml`. Dosya bir `service` bölümü ve bir `deployment` her iki kapsayıcı türleri için bölüm `language-frontend` Web kapsayıcısını ve `language` algılama kapsayıcı. 

    [!code-yml[Kubernetes orchestration file for the Cognitive Services containers sample](~/samples-cogserv-containers/Kubernetes/language/language.yml "Kubernetes orchestration file for the Cognitive Services containers sample")]

1. Dil ön uç dağıtım satırlarını değiştirin `language.yml` kendi kapsayıcı kayıt defteri görüntü adları, istemci gizli anahtarı ve metin analizi ayarları eklemek için aşağıdaki tabloyu temel alan.

    Dil ön uç dağıtım ayarları|Amaç|
    |--|--|
    |Satır 32<br> `image` Özelliği|Ön uç görüntüyü kapsayıcı kayıt defteriniz için görüntü konumu<br>`<container-registry-name>.azurecr.io/language-frontend:v1`|
    |Satır 44<br> `name` Özelliği|Görüntüyü kapsayıcı kayıt defteri gizliliğini denir `<client-secret>` bir önceki bölümdeki.|

1. Dil dağıtım satırlarını değiştirin `language.yml` kendi kapsayıcı kayıt defteri görüntü adları, istemci gizli anahtarı ve metin analizi ayarları eklemek için aşağıdaki tabloyu temel alan.

    |Dil dağıtım ayarları|Amaç|
    |--|--|
    |Satır 78<br> `image` Özelliği|Dil görüntüyü kapsayıcı kayıt defteriniz için görüntü konumu<br>`<container-registry-name>.azurecr.io/language:1.1.006770001-amd64-preview`|
    |Line 95<br> `name` Özelliği|Görüntüyü kapsayıcı kayıt defteri gizliliğini denir `<client-secret>` bir önceki bölümdeki.|
    |Satır 91<br> `apiKey` Özelliği|Metin analizi kaynak anahtarınız|
    |Satır 92<br> `billing` Özelliği|Metin analizi kaynağınızın fatura uç noktası.<br>`https://westus.api.cognitive.microsoft.com/text/analytics/v2.0`|

    Çünkü **apiKey** ve **uç nokta faturalama** ayarlanır Kubernetes düzenleme tanımının bir parçası, Web kapsayıcısı, isteğin bir parçası geçirileceğini ya da bunlar hakkında bilmeniz gerekmez. Web sitesi kapsayıcı orchestrator adıyla dil algılama kapsayıcıya başvuran `language`. 

1. Bu örnek için düzenleme tanım dosyasını oluşturduğunuz ve kaydettiğiniz yeri klasörden yüklemek `language.yml`. 

    ```console
    kubectl apply -f language.yml
    ```

    Yanıt.:

    ```console
    > kubectl apply -f language.yml
    service "language-frontend" created
    deployment.apps "language-frontend" created
    service "language" created
    deployment.apps "language" created
    ```

## <a name="get-external-ips-of-containers"></a>Dış IP'ler kapsayıcıların Al

İki kapsayıcı için doğrulama `language-frontend` ve `language` hizmetlerinin çalışmakta olduğunu ve dış IP adresini alın. 

```console
kubectl get all
```

```console
> kubectl get all
NAME                                     READY     STATUS    RESTARTS   AGE
pod/language-586849d8dc-7zvz5            1/1       Running   0          13h
pod/language-frontend-68b9969969-bz9bg   1/1       Running   1          13h

NAME                        TYPE           CLUSTER-IP    EXTERNAL-IP     PORT(S)          AGE
service/kubernetes          ClusterIP      10.0.0.1      <none>          443/TCP          14h
service/language            LoadBalancer   10.0.39.169   104.42.172.68   5000:30161/TCP   13h
service/language-frontend   LoadBalancer   10.0.42.136   104.42.37.219   80:30943/TCP     13h

NAME                                      DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
deployment.extensions/language            1         1         1            1           13h
deployment.extensions/language-frontend   1         1         1            1           13h

NAME                                                 DESIRED   CURRENT   READY     AGE
replicaset.extensions/language-586849d8dc            1         1         1         13h
replicaset.extensions/language-frontend-68b9969969   1         1         1         13h

NAME                                DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/language            1         1         1            1           13h
deployment.apps/language-frontend   1         1         1            1           13h

NAME                                           DESIRED   CURRENT   READY     AGE
replicaset.apps/language-586849d8dc            1         1         1         13h
replicaset.apps/language-frontend-68b9969969   1         1         1         13h
```

Varsa `EXTERNAL-IP` için IP adresi bir sonraki adıma geçmeden önce gösterilen kadar hizmet beklemede, yeniden gibi komut gösterilir. 

## <a name="test-the-language-detection-container"></a>Test dil algılama kapsayıcısı

Bir tarayıcı açın ve gidin dış IP `language` önceki bölümde kapsayıcı: `http://<external-ip>:5000/swagger/index.html`. Kullanabileceğiniz `Try it` dil algılama uç nokta test etmek için API özelliğidir. 

![Kapsayıcının swagger belgelerini görüntüleyin](../media/how-tos/container-instance-sample/language-detection-container-swagger-documentation.png)

## <a name="test-the-client-application-container"></a>Test istemci uygulama kapsayıcısı

Dış IP, tarayıcıya URL'yi değiştirmeniz `language-frontend` aşağıdaki biçimi kullanarak kapsayıcı: `http://<external-ip>/helloworld`. Metnin İngilizce kültüründe `helloworld` olarak tahmin `English`.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Küme ile işiniz bittiğinde, Azure kaynak grubunu silin. 

```azure-cli
az group delete --name cogserv-container-rg
```

## <a name="related-information"></a>İlgili bilgiler

* [kubectl Docker kullanıcıları için](https://kubernetes.io/docs/reference/kubectl/docker-cli-to-kubectl/)

## <a name="next-steps"></a>Sonraki adımlar 

* Daha fazla kullanmanız [Bilişsel Hizmetleri kapsayıcıları](../../cognitive-services-container-support.md)
* Bağlı hizmet kullanım metin analizi] (.. / vs-metin-bağlı-service.md)


<!--
kubectl get secrets

>az aks browse --resource-group diberry-cogserv-container-rg --name diberry-kubernetes-languagedetection

kubectl proxy

http://localhost:8001/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy/#!/pod/default/language-frontend-6d65bdb77c-8f4qv?namespace=default

kubectl describe pod language-frontend-6d65bdb77c
-->
