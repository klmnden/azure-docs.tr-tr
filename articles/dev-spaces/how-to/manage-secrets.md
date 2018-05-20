---
title: Bir Azure Dev alanıyla çalışırken parolaları yönetme | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: article
ms.technology: azds-kubernetes
description: Kapsayıcılar ve Azure üzerinde mikro ile hızlı Kubernetes geliştirme
keywords: Docker, Kubernetes, Azure, AKS, Azure kapsayıcı hizmeti, kapsayıcıları
manager: douge
ms.openlocfilehash: b77d862f578ddc374dbb58117b4ea58eb973e5fe
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="how-to-manage-secrets-when-working-with-an-azure-dev-space"></a>Bir Azure Dev alanıyla çalışırken parolaları yönetme

Hizmetlerinizin belirli parolalar, bağlantı dizeleri ve diğer parolaları gibi veritabanları veya güvenli diğer Azure Hizmetleri için gerektirebilir. Yapılandırma dosyalarında bu Sırları değerlerini ayarlayarak, bunları kodunuzu bulunan ortam değişkenleri olarak yapabilirsiniz.  Bu gizli anahtarları güvenliği tehlikeye önlemek için dikkatle ele alınması gerekir.

Azure Dev alanları sağlayan iki önerilen parolaları depolamak için seçenekleri: values.dev.yaml dosya ve satır doğrudan azds.yaml içinde. Bu values.yaml parolaları depolamak için önerilmez.
 
## <a name="method-1-valuesdevyaml"></a>Yöntem 1: values.dev.yaml
1. Azure Dev alanları için etkinleştirilen projeniz ile VS Code'da açın.
2. Adlı bir dosya eklemek _values.dev.yaml_ aynı klasörde mevcut olarak _values.yaml_ ve gizli anahtarı ve değerleri, aşağıdaki örnekteki gibi tanımlayın:

    ```yaml
    secrets:
      redis:
        port: "6380"
        host: "contosodevredis.redis.cache.windows.net"
        key: "secretkeyhere"
    ```
     
3. Güncelleştirme _azds.yaml_ yeni kullanmak için Azure Dev alanları bildirmek için _values.dev.yaml_ dosya. Bunu yapmak için bu yapılandırma configurations.develop.container bölümünde ekleyin:

    ```yaml
           container:
             values:
             - "charts/webfrontend/values.dev.yaml"
    ```
 
4. Aşağıdaki örnekte olduğu gibi ortam değişkenleri olarak bu Sırları başvurmak için hizmet kodunuzun değiştirin:

    ```
    var redisPort = process.env.REDIS_PORT
    var host = process.env.REDIS_HOST
    var theKey = process.env.REDIS_KEY
    ```
    
5. Bu değişiklikler kümenizde çalışan hizmetleri güncelleştirin. Komut satırında komutu çalıştırın:

    ```
    azds up
    ```
 
6. (İsteğe bağlı) Komut satırından bu gizli anahtarları oluşturulup oluşturulmadığını denetleyin:

      ```
      kubectl get secret --namespace default -o yaml 
      ```

7. Eklediğiniz emin olun _values.dev.yaml_ için _.gitignore_ gizli kaynak denetiminde yürüten önlemek için dosya.
 
 
## <a name="method-2-inline-directly-in-azdsyaml"></a>Yöntem 2: Satır doğrudan azds.yaml içinde
1.  İçinde _azds.yaml_, gizli yaml bölüm yapılandırmaları/geliştirme/yükleme ayarlayın. Girdiğiniz olsa da, çünkü Önerilmemesine doğrudan gizli değerleri _azds.yaml_ kaynak denetimine iade. Bunun yerine, "$PLACEHOLDER" sözdizimini kullanarak yer tutucuları ekleyin.

    ```yaml
    configurations:
      develop:
        ...
        install:
          set:
            secrets:
              redis:
                port: "$REDIS_PORT_DEV"
                host: "$REDIS_HOST_DEV"
                key: "$REDIS_KEY_DEV"
    ```
     
2.  Oluşturma bir _.env_ aynı klasöre dosyasında _azds.yaml_. Standart anahtar kullanılarak parolaları girin = değer gösterimi. Yürütme yok _.env_ dosya kaynak denetimine. (Kaynak denetiminden git tabanlı sürüm denetim sistemleri atlamak için eklemeniz _.gitignore_ dosyası.) Aşağıdaki örnekte gösterildiği bir _.env_ dosyası:

    ```
    REDIS_PORT_DEV=3333
    REDIS_HOST_DEV=myredishost
    REDIS_KEY_DEV=myrediskey
    ```
2.  Kodda, aşağıdaki örnekte olduğu gibi bu Sırları başvurmak için hizmet kaynak kodu değiştirin:

    ```
    var redisPort = process.env.REDIS_PORT
    var host = process.env.REDIS_HOST
    var theKey = process.env.REDIS_KEY
    ```
 
3.  Bu değişiklikler kümenizde çalışan hizmetleri güncelleştirin. Komut satırında komutu çalıştırın:

    ```
    azds up
    ```

4.  (isteğe bağlı) Görünüm gizli kubectl gelen:

    ```
    kubectl get secret --namespace default -o yaml
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu yöntemlerle şimdi güvenli bir şekilde bir veritabanı, Redis önbelleği veya erişim güvenli Azure Hizmetleri bağlayabilirsiniz.
 