---
title: (KULLANIM DIŞI) Azure Container Service ve Azure ile taslak kullanma kapsayıcı kayıt defteri
description: Draft ile Azure’da ilk uygulamanızı oluşturmak için bir ACS Kubernetes kümesi ve bir Azure Container Registry oluşturun.
services: container-service
author: squillace
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 09/14/2017
ms.author: rasquill
ms.custom: mvc
ms.openlocfilehash: fb34be09ec08957621517c957b3570cdbcfc0468
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59787168"
---
# <a name="deprecated-use-draft-with-azure-container-service-and-azure-container-registry-to-build-and-deploy-an-application-to-kubernetes"></a>(KULLANIM DIŞI) Taslak oluşturmak ve Kubernetes bir uygulamayı dağıtmak için Azure Container Service ve Azure Container Registry ile kullanma

> [!TIP]
> Bu makale, güncelleştirilmiş sürümü kullanan Azure Kubernetes Service için bkz: [Azure Kubernetes Service (AKS) ile kullanım taslağı](../../aks/kubernetes-draft.md).

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

[Draft](https://aka.ms/draft), Docker ve Kubernetes hakkında pek fazla bilginiz olmadan, hatta bunları yüklemeden kapsayıcı tabanlı uygulamalar geliştirmeyi ve bu uygulamaları Kubernetes kümelerine dağıtmayı kolaylaştıran yeni bir açık kaynak araçtır. Draft gibi araçların kullanılması, sizin ve ekiplerinizin altyapıya eskisi kadar dikkat etmesine gerek kalmadan Kubernetes ile uygulama oluşturmaya odaklanmasına imkan sağlar.

Draft’ı yerel kullanım dahil olmak üzere herhangi bir Docker görüntü kayıt defteri ve herhangi bir Kubernetes kümesiyle kullanabilirsiniz. Bu öğreticide, ACS Kubernetes ve ACR ile Kubernetes Draft'ı kullanarak canlı ancak güvenli Geliştirici işlem hattı oluşturmak için nasıl kullanılacağını ve Azure DNS, geliştirici işlem hattı çalıştırmasını bir etki alanına görmek diğer kullanıcıların kullanıma sunmak için nasıl kullanılacağı gösterilir.


## <a name="create-an-azure-container-registry"></a>Azure Container Registry oluşturma
Kolayca [yeni Azure Container Registry](../../container-registry/container-registry-get-started-azure-cli.md) oluşturabilirsiniz, ancak adımlar aşağıdaki gibidir:

1. ACR kayıt defterinizin ve ACS Kubernetes kümesinde yönetmek için bir Azure kaynak grubu oluşturun.
      ```azurecli
      az group create --name draft --location eastus
      ```

2. Kullanarak bir ACR görüntü kayıt defteri oluşturun [az ACT create](/cli/azure/acr#az-acr-create) olduğundan emin olun `--admin-enabled` seçeneği `true`.
      ```azurecli
      az acr create --resource-group draft --name draftacs --sku Basic
      ```


## <a name="create-an-azure-container-service-with-kubernetes"></a>Kubernetes ile Azure Container Service oluşturma

Artık [az acs create](/cli/azure/acs#az-acs-create) komutu ile `--orchestrator-type` değeri olarak Kubernetes’i kullanarak bir ACS kümesi oluşturmaya hazırsınız.
```azurecli
az acs create --resource-group draft --name draft-kube-acs --dns-prefix draft-cluster --orchestrator-type kubernetes --generate-ssh-keys
```

> [!NOTE]
> Kubernetes varsayılan düzenleyici olmadığından, `--orchestrator-type kubernetes` anahtarını kullandığınızdan emin olun.

İşlem başarılı olursa çıktı aşağıdaki gibi görünür.

```json
waiting for AAD role to propagate.done
{
  "id": "/subscriptions/<guid>/resourceGroups/draft/providers/Microsoft.Resources/deployments/azurecli14904.93snip09",
  "name": "azurecli1496227204.9323909",
  "properties": {
    "correlationId": "<guid>",
    "debugSetting": null,
    "dependencies": [],
    "mode": "Incremental",
    "outputs": null,
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
              "westus"
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
    "timestamp": "2017-05-31T10:46:29.434095+00:00"
  },
  "resourceGroup": "draft"
}
```

Artık bir kümeniz olduğuna göre, [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes) komutunu kullanarak kimlik bilgilerini içeri aktarabilirsiniz. Artık kümeniz için yerel bir yapılandırma dosyanız vardır ve Helm ile Draft’ın işlerini yapabilmesi için bu gerekir.

## <a name="install-and-configure-draft"></a>Draft’ı yükleme ve yapılandırma


1. Taslak ortamınız için indirme https://github.com/Azure/draft/releases ve böylece komut kullanılabilir, yola yükleyin.
2. Helm ortamınız için indirme https://github.com/kubernetes/helm/releases ve [komut kullanılabilir olacak şekilde, yola yüklemek](https://github.com/kubernetes/helm/blob/master/docs/install.md#installing-the-helm-client).
3. Draft’ı kayıt defterinizi kullanacak şekilde yapılandırın ve Draft’ın oluşturduğu her Helm grafiği için alt etki alanları oluşturun. Draft’ı yapılandırmak için şunlar gerekir:
   - Azure Container Registry adınız (bu örnekte `draftacsdemo` kullanılmıştır)
   - `az acr credential show -n <registry name> --output tsv --query "passwords[0].value"` dosyasından kayıt defteri anahtarınız veya parolanız.

   Çağrı `draft init` ve yapılandırma işlemi sizden yukarıdaki değerleri ister; URL biçimlendirmek için kayıt defteri URL'si Not kayıt defteri adı (Bu örnekte, `draftacsdemo`) yanı sıra `.azurecr.io`. Kullanıcı adınızı, kendi kendine kayıt defteri adıdır. İşlem ilk çalıştırıldığında aşağıdaki gibi görünür.
   ```bash
    $ draft init
    Creating /home/ralph/.draft 
    Creating /home/ralph/.draft/plugins 
    Creating /home/ralph/.draft/packs 
    Creating pack go...
    Creating pack python...
    Creating pack ruby...
    Creating pack javascript...
    Creating pack gradle...
    Creating pack java...
    Creating pack php...
    Creating pack csharp...
    $DRAFT_HOME has been configured at /home/ralph/.draft.

    In order to configure Draft, we need a bit more information...

    1. Enter your Docker registry URL (e.g. docker.io/myuser, quay.io/myuser, myregistry.azurecr.io): draftacsdemo.azurecr.io
    2. Enter your username: draftacsdemo
    3. Enter your password: 
    Draft has been installed into your Kubernetes Cluster.
    Happy Sailing!
   ```

Artık uygulama dağıtmaya hazırsınız.


## <a name="build-and-deploy-an-application"></a>Uygulama oluşturma ve dağıtma

Draft deposunda [altı basit örnek uygulama](https://github.com/Azure/draft/tree/master/examples) yer alır. Depoyu kopyalayalım ve kullanalım [Java örnek](https://github.com/Azure/draft/tree/master/examples/example-java). Örnekler/java dizinine ve türü değiştirme `draft create` uygulamayı oluşturmak için. Aşağıdaki örnekteki gibi görünmelidir.
```bash
$ draft create
--> Draft detected the primary language as Java with 91.228814% certainty.
--> Ready to sail
```

Çıktı bir Docker dosyası ve Helm grafiği içerir. Derlemek ve dağıtmak için `draft up` yazmanız yeterlidir. Çıktı kapsamlıdır, ancak aşağıdaki örnekteki gibi olmalıdır.
```bash
$ draft up
Draft Up Started: 'handy-labradoodle'
handy-labradoodle: Building Docker Image: SUCCESS ⚓  (35.0232s)
handy-labradoodle: Pushing Docker Image: SUCCESS ⚓  (17.0062s)
handy-labradoodle: Releasing Application: SUCCESS ⚓  (3.8903s)
handy-labradoodle: Build ID: 01BT0ZJ87NWCD7BBPK4Y3BTTPB
```

## <a name="securely-view-your-application"></a>Güvenli bir şekilde uygulamanızı görüntüleyin

Kapsayıcınızı ACS'de artık çalışır durumdadır. Bunu görüntülemek için kullanın `draft connect` komutu, uygulamanız için belirli bir bağlantı noktası ile küme IP güvenli bir bağlantı oluşturur ve böylece yerel olarak görüntüleyebilirsiniz. Başarılı olursa, sonra ilk satırda uygulamanıza bağlamak URL'yi bulun **başarı** göstergesi.

> [!NOTE]
> Hiçbir pod'ların hazır olduğunu belirten bir ileti alırsanız bir süre ve yeniden deneme için bekleyin veya kullanıma hazır hale pod'ların izleyebilirsiniz `kubectl get pods -w` ve bunu yaptıklarında yeniden deneyin.

```bash
draft connect
Connecting to your app...SUCCESS...Connect to your app on localhost:46143
Starting log streaming...
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See https://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
== Spark has ignited ...
>> Listening on 0.0.0.0:4567
```

Önceki örnekte, yazabilirsiniz `curl -s http://localhost:46143` yanıtın `Hello World, I'm Java!`. Ne zaman, CTRL + veya CMD + C (işletim sistemi ortamınızın) bağlı olarak güvenli bir tünel bozuk ve yineleme devam edebilirsiniz.

## <a name="sharing-your-application-by-configuring-a-deployment-domain-with-azure-dns"></a>Dağıtım etki alanının Azure DNS ile yapılandırarak uygulamanız paylaşma

Önceki adımlarda Draft, oluşturduğu Geliştirici yineleme döngüsü zaten gerçekleştirdiniz. Ancak, uygulamanız tarafından Internet'te paylaşabilirsiniz:
1. (Uygulama görüntülenecek genel bir IP adresi sağlamak için), ACS kümesindeki bir giriş yükleme
2. Özel etki alanınızı Azure DNS'ye devretme ve etki alanınızı eşleme için IP adresi ACS giriş denetleyicinize atar.

### <a name="use-helm-to-install-the-ingress-controller"></a>Giriş denetleyicisine yüklemek için Helm kullanın.
Kullanım **helm** aramak ve yüklemek için `stable/traefik`, derlemelerinize yönelik gelen istekleri etkinleştirmek için bir giriş denetleyicisine.
```bash
$ helm search traefik
NAME            VERSION DESCRIPTION
stable/traefik  1.3.0   A Traefik based Kubernetes ingress controller w...

$ helm install stable/traefik --name ingress
```
Şimdi, dış IP değeri dağıtıldığında bu değeri yakalaması için `ingress` denetleyicisinde bir izleme ayarlayın. Bu IP adresi, bir sonraki bölümde dağıtım etki alanınıza eşlenen olacaktır.

```bash
$ kubectl get svc -w
NAME                          CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
ingress-traefik               10.0.248.104   13.64.108.240   80:31046/TCP,443:32556/TCP   1h
kubernetes                    10.0.0.1       <none>          443/TCP                      7h
```

Bu durumda, dağıtım etki alanının dış IP’si şudur: `13.64.108.240`. Artık etki alanınızı bu IP ile eşleyebilirsiniz.

### <a name="map-the-ingress-ip-to-a-custom-subdomain"></a>Giriş IP için özel bir alt etki alanı eşleme

Draft, oluşturduğu her Helm grafiği (üzerinde çalıştığınız her uygulama) için bir yayın oluşturur. Her biri tarafından kullanılan bir ad oluşturulur alır **taslak** olarak bir _alt etki alanı_ kök üzerine _dağıtım etki alanının_ , sizin denetlediğiniz. (Bu örnekte, dağıtım etki alanı olarak `squillace.io`’yu kullanıyoruz.) Bu alt etki alanı davranışını etkinleştirmek istiyorsanız, oluşturulan her alt etki alanının Kubernetes kümesinin giriş denetleyicisine yönlendirilmesi için dağıtım etki alanınıza yönelik DNS girişlerinizde `'*.draft'` için bir A kaydı oluşturun. 

Etki alanı sağlayıcınız, DNS sunucularını atamak için kendi yöntemini kullanır; [Azure DNS’yi etki alanı ad sunucularınızın temsilcisi olarak atamak için](../../dns/dns-delegate-domain-azure-dns.md) aşağıdaki adımları gerçekleştirirsiniz:

1. Bölgeniz için bir kaynak grubu oluşturun.
    ```azurecli
    az group create --name squillace.io --location eastus
    {
      "id": "/subscriptions/<guid>/resourceGroups/squillace.io",
      "location": "eastus",
      "managedBy": null,
      "name": "zones",
      "properties": {
        "provisioningState": "Succeeded"
      },
      "tags": null
    }
    ```

2. Etki alanınız için bir DNS bölgesi oluşturun.
   Etki alanınız için Azure DNS’yi DNS denetimi temsilcisi olarak atamak istiyorsanız [az network dns zone create](/cli/azure/network/dns/zone#az-network-dns-zone-create) komutunu kullanarak ad sunucularını edinin.
    ```azurecli
    az network dns zone create --resource-group squillace.io --name squillace.io
    {
      "etag": "<guid>",
      "id": "/subscriptions/<guid>/resourceGroups/zones/providers/Microsoft.Network/dnszones/squillace.io",
      "location": "global",
      "maxNumberOfRecordSets": 5000,
      "name": "squillace.io",
      "nameServers": [
        "ns1-09.azure-dns.com.",
        "ns2-09.azure-dns.net.",
        "ns3-09.azure-dns.org.",
        "ns4-09.azure-dns.info."
      ],
      "numberOfRecordSets": 2,
      "resourceGroup": "squillace.io",
      "tags": {},
      "type": "Microsoft.Network/dnszones"
    }
    ```
3. Size döndürülen DNS sunucularını, dağıtım etki alanınızın etki alanı sağlayıcısına ekleyin. Bunu yaptığınızda, Azure DNS’nizi kullanarak etki alanınızı istediğiniz yeri gösterecek şekilde ayarlayabilirsiniz. Bu yaptığınız gibi etki alanına göre değişir; sağlayın [, etki alanı atamak için Azure DNS'yi temsilci](../../dns/dns-delegate-domain-azure-dns.md) bazı bilmeniz gereken ayrıntılar bulunur. 
4. Etki alanınızı Azure DNS'ye Devredilmiş sonra eşlenen dağıtım etki alanınız için bir A kayıt kümesi girişi oluşturun `ingress` önceki bölümün 2. adımdaki IP.
   ```azurecli
   az network dns record-set a add-record --ipv4-address 13.64.108.240 --record-set-name '*.draft' -g squillace.io -z squillace.io
   ```
   Çıktı şuna benzer:
   ```json
   {
    "arecords": [
      {
        "ipv4Address": "13.64.108.240"
      }
    ],
    "etag": "<guid>",
    "id": "/subscriptions/<guid>/resourceGroups/squillace.io/providers/Microsoft.Network/dnszones/squillace.io/A/*",
    "metadata": null,
    "name": "*.draft",
    "resourceGroup": "squillace.io",
    "ttl": 3600,
    "type": "Microsoft.Network/dnszones/A"
   }
   ```
5. Yeniden **taslak**

   1. Kaldırma **draftd** yazarak kümeden `helm delete --purge draft`. 
   2. Yeniden **taslak** aynı kullanarak `draft-init` komutu, ancak `--ingress-enabled` seçeneği:
      ```bash
      draft init --ingress-enabled
      ```
      Yukarıdaki ilk kez yaptığınız gibi istemleri yanıtlayın. Ancak, Azure DNS ile yapılandırdığınız tam etki alanı yolu kullanarak daha fazla soru için yanıt vermesi gerekir.

6. (Örneğin draft.example.com) girişi için üst düzey etki alanınızı girin: draft.squillace.io
7. Çağırdığınızda `draft up` bu kez, uygulamanızın görmek mümkün olmayacak (veya `curl` bunu) biçiminde URL'sinde `<appname>.draft.<domain>.<top-level-domain>`. Bu örnekte, söz konusu olduğunda `http://handy-labradoodle.draft.squillace.io`. 
   ```bash
   curl -s http://handy-labradoodle.draft.squillace.io
   Hello World, I'm Java!
   ```


## <a name="next-steps"></a>Sonraki adımlar

Artık bir ACS Kubernetes kümeniz olduğuna göre, bu senaryonun daha fazla sayıda ve daha farklı dağıtımlarını oluşturmak için [Azure Container Registry](../../container-registry/container-registry-intro.md)’yi kullanarak araştırma yapabilirsiniz. Örneğin, belirli ACS dağıtımları için ayarları daha derin bir alt etki alanından denetleyen bir draft._basedomain.toplevel_ etki alanı DNS kayıt kümesi oluşturabilirsiniz.






