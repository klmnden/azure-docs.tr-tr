---
title: "Draft’ı Azure Container Service ve Azure Container Registry ile kullanma | Microsoft Docs"
description: "Draft ile Azure’da ilk uygulamanızı oluşturmak için bir ACS Kubernetes kümesi ve bir Azure Container Registry oluşturun."
services: container-service
documentationcenter: 
author: squillace
manager: gamonroy
editor: 
tags: draft, helm, acs, azure-container-service
keywords: "Docker, Kapsayıcılar, mikro hizmetler, Kubernetes, Draft, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: rasquill
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: dc3ae52b1ec6717c7e19a160e3e7ea5d211f1f5f
ms.contentlocale: tr-tr
ms.lasthandoff: 06/28/2017



---

<a id="use-draft-with-azure-container-service-and-azure-container-registry-to-build-and-deploy-an-application-to-kubernetes" class="xliff"></a>

# Draft’ı Azure Container Service ve Azure Container Registry ile kullanarak bir uygulama oluşturma ve Kubernetes’e dağıtma

[Draft](https://aka.ms/draft), Docker ve Kubernetes hakkında pek fazla bilginiz olmadan, hatta bunları yüklemeden kapsayıcı tabanlı uygulamalar geliştirmeyi ve bu uygulamaları Kubernetes kümelerine dağıtmayı kolaylaştıran yeni bir açık kaynak araçtır. Draft gibi araçların kullanılması, sizin ve ekiplerinizin altyapıya eskisi kadar dikkat etmesine gerek kalmadan Kubernetes ile uygulama oluşturmaya odaklanmasına imkan sağlar.

Draft’ı yerel kullanım dahil olmak üzere herhangi bir Docker görüntü kayıt defteri ve herhangi bir Kubernetes kümesiyle kullanabilirsiniz. Bu öğreticide, Draft’ı kullanarak canlı bir CI/CD geliştirici işlem hattı oluşturmak amacıyla ACS’nin Kubernetes, ACR ve Azure DNS ile birlikte nasıl kullanılacağı açıklanmaktadır.


<a id="create-an-azure-container-registry" class="xliff"></a>

## Azure Container Registry oluşturma
Kolayca [yeni Azure Container Registry](../container-registry/container-registry-get-started-azure-cli.md) oluşturabilirsiniz, ancak adımlar aşağıdaki gibidir:

1. ACS’de ACR kayıt defterinizi ve Kubernetes kümenizi yönetmek için br Azure kaynak grubu oluşturun.
      ```azurecli
      az group create --name draft --location eastus
      ```

2. Şu komutu kullanarak bir ACR görüntü kayıt defteri oluşturun: [az acr create](/cli/azure/acr#create)
      ```azurecli
      az acr create -g draft -n draftacs --sku Basic --admin-enabled true -l eastus
      ```


<a id="create-an-azure-container-service-with-kubernetes" class="xliff"></a>

## Kubernetes ile Azure Container Service oluşturma

Artık [az acs create](/cli/azure/acs#create) komutu ile `--orchestrator-type` değeri olarak Kubernetes’i kullanarak bir ACS kümesi oluşturmaya hazırsınız.
```azurecli
az acs create --resource-group draft --name draft-kube-acs --dns-prefix draft-cluster --orchestrator-type kubernetes
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

Artık bir kümeniz olduğuna göre, [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials) komutunu kullanarak kimlik bilgilerini içeri aktarabilirsiniz. Artık kümeniz için yerel bir yapılandırma dosyanız vardır ve Helm ile Draft’ın işlerini yapabilmesi için bu gerekir.

<a id="install-and-configure-draft" class="xliff"></a>

## Draft’ı yükleme ve yapılandırma
Draft için yükleme yönergeleri [Draft deposunda](https://github.com/Azure/draft/blob/master/docs/install.md) bulunabilir. Bu yönergeler görece basittir, ancak bir Helm oluşturup bunu Kubernetes kümesine dağıtmak için [Helm](https://aka.ms/helm)’e bağımlı olduğundan bazı yapılandırma işlemleri gerektirir.

1. [Helm’i indirip yükleyin](https://aka.ms/helm#install).
2. `stable/traefik` aracını bulup yüklemek için Helm’i; derlemelerinize yönelik gelen istekleri etkinleştirmek için giriş denetleyicisini kullanın.
    ```bash
    $ helm search traefik
    NAME            VERSION DESCRIPTION
    stable/traefik  1.3.0   A Traefik based Kubernetes ingress controller w...

    $ helm install stable/traefik --name ingress
    ```
    Şimdi, dış IP değeri dağıtıldığında bu değeri yakalaması için `ingress` denetleyicisinde bir izleme ayarlayın. Bu IP adresi, bir sonraki bölümde [dağıtım etki alanınızla eşleştirilecek](#wire-up-deployment-domain) adrestir.

    ```bash
    kubectl get svc -w
    NAME                          CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
    ingress-traefik               10.0.248.104   13.64.108.240   80:31046/TCP,443:32556/TCP   1h
    kubernetes                    10.0.0.1       <none>          443/TCP                      7h
    ```

    Bu durumda, dağıtım etki alanının dış IP’si şudur: `13.64.108.240`. Artık etki alanınızı bu IP ile eşleyebilirsiniz.

<a id="wire-up-deployment-domain" class="xliff"></a>

## Dağıtım etki alanının bağlantılarını ayarlama

Draft, oluşturduğu her Helm grafiği (üzerinde çalıştığınız her uygulama) için bir yayın oluşturur. Her biri için, sizin denetlediğiniz kök _dağıtım etki alanı_ üzerine bir _alt etki alanı_ olarak taslak tarafından kullanılan bir ad oluşturulur. (Bu örnekte, dağıtım etki alanı olarak `squillace.io`’yu kullanıyoruz.) Bu alt etki alanı davranışını etkinleştirmek istiyorsanız, oluşturulan her alt etki alanının Kubernetes kümesinin giriş denetleyicisine yönlendirilmesi için dağıtım etki alanınıza yönelik DNS girişlerinizde `'*'` için bir A kaydı oluşturun.

Etki alanı sağlayıcınız, DNS sunucularını atamak için kendi yöntemini kullanır; [Azure DNS’yi etki alanı ad sunucularınızın temsilcisi olarak atamak için](../dns/dns-delegate-domain-azure-dns.md) aşağıdaki adımları gerçekleştirirsiniz:

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
Etki alanınız için Azure DNS’yi DNS denetimi temsilcisi olarak atamak istiyorsanız [az network dns zone create](/cli/azure/network/dns/zone#create) komutunu kullanarak ad sunucularını edinin.
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
3. Size döndürülen DNS sunucularını, dağıtım etki alanınızın etki alanı sağlayıcısına ekleyin. Bunu yaptığınızda, Azure DNS’nizi kullanarak etki alanınızı istediğiniz yeri gösterecek şekilde ayarlayabilirsiniz.
4. Önceki bölümün 2. adımında `ingress` IP’si ile eşlenen dağıtım etki alanınız için bir A kayıt kümesi girişi oluşturun.
    ```azurecli
    az network dns record-set a add-record --ipv4-address 13.64.108.240 --record-set-name '*' -g squillace.io -z squillace.io
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
      "name": "*",
      "resourceGroup": "squillace.io",
      "ttl": 3600,
      "type": "Microsoft.Network/dnszones/A"
    }
    ```

5. Draft’ı kayıt defterinizi kullanacak şekilde yapılandırın ve Draft’ın oluşturduğu her Helm grafiği için alt etki alanları oluşturun. Draft’ı yapılandırmak için şunlar gerekir:
  - Azure Container Registry adınız (bu örnekte `draft` kullanılmıştır)
  - `az acr credential show -n <registry name> --output tsv --query "passwords[0].value"` dosyasından kayıt defteri anahtarınız veya parolanız.
  - Kubernetes giriş dış IP adresi (burada `squillace.io` kullanılmıştır) ile eşlenecek şekilde yapılandırdığınız kök dağıtım etki alanı

  `draft init` çağrısı yaptığınızda, yapılandırma işlemi sizden yukarıdaki değerleri girmenizi ister. İşlem ilk çalıştırıldığında aşağıdaki gibi görünür.
    ```
    draft init
    Creating pack ruby...
    Creating pack node...
    Creating pack gradle...
    Creating pack maven...
    Creating pack php...
    Creating pack python...
    Creating pack dotnetcore...
    Creating pack golang...
    $DRAFT_HOME has been configured at /Users/ralphsquillace/.draft.

    In order to install Draft, we need a bit more information...

    1. Enter your Docker registry URL (e.g. docker.io, quay.io, myregistry.azurecr.io): draft.azurecr.io
    2. Enter your username: draft
    3. Enter your password:
    4. Enter your org where Draft will push images [draft]: draft
    5. Enter your top-level domain for ingress (e.g. draft.example.com): squillace.io
    Draft has been installed into your Kubernetes Cluster.
    Happy Sailing!
    ```

Artık uygulama dağıtmaya hazırsınız.


<a id="build-and-deploy-an-application" class="xliff"></a>

## Uygulama oluşturma ve dağıtma

Draft deposunda [altı basit örnek uygulama](https://github.com/Azure/draft/tree/master/examples) yer alır. Depoyu kopyalayalım ve [Python örneğini](https://github.com/Azure/draft/tree/master/examples/python) kullanalım. Bulunduğunuz dizini samples/Python olarak değiştirin ve uygulamayı oluşturmak için `draft create` yazın. Aşağıdaki örnekteki gibi görünmelidir.
```bash
$ draft create
--> Python app detected
--> Ready to sail
```

Çıktı bir Docker dosyası ve Helm grafiği içerir. Derlemek ve dağıtmak için `draft up` yazmanız yeterlidir. Çıktı kapsamlıdır, ancak başlangıcı aşağıdaki örnekteki gibi görünür.
```bash
$ draft up
--> Building Dockerfile
Step 1 : FROM python:onbuild
onbuild: Pulling from library/python
10a267c67f42: Pulling fs layer
fb5937da9414: Pulling fs layer
9021b2326a1e: Pulling fs layer
dbed9b09434e: Pulling fs layer
ea8a37f15161: Pulling fs layer
<snip>
```

Başarılı olduğunda ise aşağıdaki örneğe benzer bir şekilde biter.
```bash
ab68189731eb: Pushed
53c0ab0341bee12d01be3d3c192fbd63562af7f1: digest: sha256:bb0450ec37acf67ed461c1512ef21f58a500ff9326ce3ec623ce1e4427df9765 size: 2841
--> Deploying to Kubernetes
--> Status: DEPLOYED
--> Notes:

  http://gangly-bronco.squillace.io to access your application

Watching local files for changes...
```

Grafiğinizin adından bağımsız olarak artık `curl http://gangly-bronco.squillace.io` işlemiyle `Hello World!` olan yanıtı alabilirsiniz.

<a id="next-steps" class="xliff"></a>

## Sonraki adımlar

Artık bir ACS Kubernetes kümeniz olduğuna göre, bu senaryonun daha fazla sayıda ve daha farklı dağıtımlarını oluşturmak için [Azure Container Registry](../container-registry/container-registry-intro.md)’yi kullanarak araştırma yapabilirsiniz. Örneğin, belirli ACS dağıtımları için ayarları daha derin bir alt etki alanından denetleyen bir draft._basedomain.toplevel_ etki alanı DNS kayıt kümesi oluşturabilirsiniz.







