---
title: Bir Azure geliştirme boşluk ile çalışırken, gizli anahtarları yönetme
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 05/11/2018
ms.topic: conceptual
description: Azure’da kapsayıcılar ve mikro hizmetlerle hızlı Kubernetes geliştirme
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, kapsayıcılar
ms.openlocfilehash: 8ee50289083b12b7b2abd3b9ece2c8de345df9fe
ms.sourcegitcommit: 16cb78a0766f9b3efbaf12426519ddab2774b815
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65851428"
---
# <a name="how-to-manage-secrets-when-working-with-an-azure-dev-space"></a>Bir Azure geliştirme boşluk ile çalışırken, gizli anahtarları yönetme

Hizmetlerinizin belirli parolalar, bağlantı dizelerini ve diğer gizli dizileri gibi veritabanları veya güvenli diğer Azure Hizmetleri için gerektirebilir. Yapılandırma dosyalarında bu gizli dizileri değerleri ayarlayarak, bunları kodunuzu bulunan ortam değişkenleri olarak yapabilirsiniz.  Bu gizli dizileri güvenliğini tehlikeye kaçınmak için dikkatli işlenmelidir.

Azure geliştirme alanları, iki önerilen gizli dizileri depolamak için seçenekleri sağlar: values.dev.yaml dosya ve satır içi doğrudan azds.yaml. Values.yaml gizli dizileri depolamak için önermedi.
 
## <a name="method-1-valuesdevyaml"></a>Yöntem 1: values.dev.yaml
1. VS Code için Azure geliştirme alanları etkin projenizle açın.
2. Adlı bir dosya ekleyin _values.dev.yaml_ aynı klasörde mevcut olarak _azds.yaml_ ve gizli anahtarı ve değerleri, aşağıdaki örnekte gösterildiği gibi tanımlayın:

    ```yaml
    secrets:
      redis:
        port: "6380"
        host: "contosodevredis.redis.cache.windows.net"
        key: "secretkeyhere"
    ```
     
3. _azds.yaml_ zaten başvuruyor _values.dev.yaml_ varsa dosya. Farklı bir dosya adı tercih ederseniz, install.values bölüm güncelleştirin:

    ```yaml
    install:
      values:
      - values.dev.yaml?
      - secrets.dev.yaml?
    ```
 
4. Bu gizli dizileri için aşağıdaki örnekte olduğu gibi ortam değişkenleri olarak başvurmak için hizmet kodunuzu değiştirin:

    ```
    var redisPort = process.env.REDIS_PORT
    var host = process.env.REDIS_HOST
    var theKey = process.env.REDIS_KEY
    ```
    
5. Bu değişikliklerle kümenizde çalışan hizmetleri güncelleştirin. Komut satırında komutu çalıştırın:

    ```
    azds up
    ```
 
6. (İsteğe bağlı) Komut satırından bu gizli dizileri oluşturulmuş olduğunu kontrol edin:

      ```
      kubectl get secret --namespace default -o yaml 
      ```

7. Eklediğiniz emin _values.dev.yaml_ için _.gitignore_ gizli dizileri kaynak denetiminde yürüten önlemek için.
 
 
## <a name="method-2-inline-directly-in-azdsyaml"></a>2. yöntem: Satır içi doğrudan azds.yaml
1.  İçinde _azds.yaml_, gizli dizileri yaml bölüm yapılandırmaları/geliştirme/yükleme ayarlayın. Kolaylıkla olsa da, çünkü önerilmez doğrudan gizli değerleri _azds.yaml_ kaynak denetimine iade edildi. Bunun yerine "$PLACEHOLDER" sözdizimini kullanarak yer tutucu ekleyin.

    ```yaml
    configurations:
      develop:
        ...
        install:
          set:
            secrets:
              redis:
                port: "$REDIS_PORT"
                host: "$REDIS_HOST"
                key: "$REDIS_KEY"
    ```
     
2.  Oluşturma bir _.env_ dosya aynı klasörde _azds.yaml_. Standart bir anahtar kullanarak parolaları girin = değer gösterimi. İşleme yok _.env_ dosya kaynak denetimine. (Kaynak denetiminden git tabanlı sürüm denetim sistemleri atlamak için eklemeniz _.gitignore_ dosyası.) Aşağıdaki örnekte gösterildiği bir _.env_ dosyası:

    ```
    REDIS_PORT=3333
    REDIS_HOST=myredishost
    REDIS_KEY=myrediskey
    ```
2.  Bu kod, aşağıdaki örnekte olduğu gibi gizli başvurmak için hizmet kaynak kodunuzu değiştirin:

    ```
    var redisPort = process.env.REDIS_PORT
    var host = process.env.REDIS_HOST
    var theKey = process.env.REDIS_KEY
    ```
 
3.  Bu değişikliklerle kümenizde çalışan hizmetleri güncelleştirin. Komut satırında komutu çalıştırın:

    ```
    azds up
    ```

4.  (isteğe bağlı) Kubectl görünümü gizli diziler:

    ```
    kubectl get secret --namespace default -o yaml
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu yöntemlerle artık güvenli bir şekilde bir Azure önbelleği için Redis, bir veritabanına bağlanın veya güvenli Azure hizmetlerine erişebilirsiniz.
 
