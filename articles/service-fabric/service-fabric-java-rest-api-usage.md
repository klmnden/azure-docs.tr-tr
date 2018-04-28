---
title: Azure Service Fabric Java istemci API'ları | Microsoft Docs
description: Oluşturma ve Service Fabric Java istemci API'ları kullanmak Service Fabric istemci REST API belirtimine kullanma
services: service-fabric
documentationcenter: java
author: rapatchi
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/27/2017
ms.author: rapatchi
ms.openlocfilehash: 9596e55c6c915461ef4d0bff0d7f9583aac18a1c
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="azure-service-fabric-java-client-apis"></a>Azure Service Fabric Java istemci API'ları

Service Fabric istemci API'ları dağıtma ve yönetme mikro tabanlı uygulamalar ve Service Fabric kapsayıcılarında küme Azure üzerinde sağlar şirket içi, yerel geliştirme makine ya da diğer bulut. Bu makalede oluşturmak ve Service Fabric istemci REST API üstünde Service Fabric Java istemci API kullanma

## <a name="generate-the-client-code-using-autorest"></a>AutoRest kullanarak istemci kodu oluştur

[AutoRest](https://github.com/Azure/autorest) RESTful web hizmetlerine erişme için istemci kitaplıkları oluşturan bir araçtır. AutoRest giriş OpenAPI belirtimi biçimini kullanarak REST API açıklayan bir özelliğidir. [Service Fabric istemci REST API'leri](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/servicefabric/data-plane) bu belirtimi izleyin.

AutoRest aracını kullanarak Service Fabric Java istemci kodu oluşturmak için aşağıda belirtilen adımları izleyin.

1. Makinenize nodejs ve NPM yükleme

    Linux kullanıyorsanız:
    ```bash
    sudo apt-get install npm
    sudo apt install nodejs
    ```
    Mac OS X kullanıyorsanız:
    ```bash
    brew install node
    ```

2. AutoRest NPM kullanarak yükleyin.
    ```bash
    npm install -g autorest
    ```

3. Çatalı ve kopya [azure rest API belirtimlerin](https://github.com/Azure/azure-rest-api-specs) deposu yerel makine ve makinenizin terminalde kopyalanan konumuna gidin.


4. Aşağıda, kopyalanan bağlantıların bulunması belirtilen konuma gidin.
    ```bash
    cd specification\servicefabric\data-plane\Microsoft.ServiceFabric\stable\6.0
    ```

    > [!NOTE]
    > Küme sürüm 6.0 değilse. * kararlı klasöründe uygun dizinine gidin.
    >   

5. Java istemci kodu oluşturmak için aşağıdaki autorest komutunu çalıştırın.
    
    ```bash
    autorest --input-file= servicefabric.json --java --output-folder=[output-folder-name] --namespace=[namespace-of-generated-client]
    ```
   Aşağıda autorest kullanımını gösteren bir örnektir.
   
    ```bash
    autorest --input-file=servicefabric.json --java --output-folder=java-rest-api-code --namespace=servicefabricrest
    ```
   
   Aşağıdaki komut alır ``servicefabric.json`` belirtimi dosya giriş olarak ve java istemci kodu oluşturur ``java-rest-api-     code`` klasörü ve kodda barındırır ``servicefabricrest`` ad alanı. Bu adımdan sonra iki klasör bulur ``models``, ``implemenation`` ve iki dosya ``ServiceFabricClientAPIs.java`` ve ``package-info.java`` üretildi ``java-rest-api-code`` klasör.


## <a name="include-and-use-the-generated-client-in-your-project"></a>Oluşturulan istemci projenizde kullanmak ve içerir

1. Oluşturulan kod uygun şekilde projenize ekleyin. Oluşturulan kod kullanarak bir kitaplığı oluşturun ve bu kitaplığını projenize dahil öneririz.
2. Bir kitaplığı oluşturmak sonra kitaplığınızın projesinde aşağıdaki bağımlılığı eklemek istiyorsanız. Aşağıdaki farklı bir yaklaşım sonra eklenecek bağımlılık uygun şekilde.

    ```
        GroupId:  com.microsoft.rest
        Artifactid: client-runtime
        Version: 1.2.1
    ```
    Örneğin, yapı sistem dahil aşağıdaki Maven kullanıyorsanız, ``pom.xml`` dosyası:

    ```xml
        <dependency>
          <groupId>com.microsoft.rest</groupId>
          <artifactId>client-runtime</artifactId>
          <version>1.2.1</version>
        </dependency>
    ```

3. Aşağıdaki kodu kullanarak RestClient oluşturun:

    ```java
        RestClient simpleClient = new RestClient.Builder()
            .withBaseUrl("http://<cluster-ip or name:port>")
            .withResponseBuilderFactory(new ServiceResponseBuilder.Factory())
            .withSerializerAdapter(new JacksonAdapter())
            .build();
        ServiceFabricClientAPIs client = new ServiceFabricClientAPIsImpl(simpleClient);
    ```
4. İstemci nesnesini kullanın ve gerektiği gibi uygun çağrıları yapma. Aşağıda, istemci nesnesi kullanımını gösteren bazı örnekler verilmiştir. Uygulama paketi oluşturulur ve kullanmadan önce görüntü deposuna karşıya varsayıyoruz API'nin aşağıda.
    * Bir uygulama sağlama
    
        ```java
            ApplicationTypeImageStorePath imageStorePath = new ApplicationTypeImageStorePath();
            imageStorePath.withApplicationTypeBuildPath("<application-path-in-image-store>");
            client.provisionApplicationType(imageStorePath);
        ```
    * Uygulama oluşturma

        ```java
            ApplicationDescription applicationDescription = new ApplicationDescription();
            applicationDescription.withName("<application-uri>");
            applicationDescription.withTypeName("<application-type>");
            applicationDescription.withTypeVersion("<application-version>");
            client.createApplication(applicationDescription);
        ```

## <a name="understanding-the-generated-code"></a>Oluşturulan kod anlama
Her API uygulaması dört aşırı bulacaksınız. Ardından isteğe bağlı parametreler varsa bu isteğe bağlı parametreleri de dahil olmak üzere dört daha fazla Çeşitlemeler bulur. Örneğin API göz önünde bulundurun ``removeReplica``.
 1. **Ortak void removeReplica (dize nodeName, UUID PartitionID, dize ReplicaID, Boolean forceRemove, uzun zaman aşımı)**
    * Bu removeReplica API çağrısı zaman uyumlu bir türevi değil
 2. **Ortak ServiceFuture<Void> removeReplicaAsync (dize nodeName, UUID PartitionID, dize ReplicaID, Boolean forceRemove, uzun zaman aşımı, son ServiceCallback<Void> serviceCallback)**
    * Gelecekteki tabanlı zaman uyumsuz programlama ve geri aramalar kullanmak istiyorsanız, bu API çağrısı türevi kullanılabilir
 3. **Ortak Observable<Void> removeReplicaAsync (dize nodeName, UUID PartitionID, dize ReplicaID)**
    * Geriye dönük zaman uyumsuz programlama kullanmak istiyorsanız, bu API çağrısı türevi kullanılabilir
 4. **Ortak Observable < ServiceResponse<Void>> removeReplicaWithServiceResponseAsync (dize nodeName, UUID PartitionID, dize ReplicaID)**
    * Geriye dönük zaman uyumsuz programlama kullanın ve ham rest yanıt ile mücadele etmek istiyorsanız bu API çağrısı türevi kullanılabilir

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [doku REST API'leri hizmet](https://docs.microsoft.com/rest/api/servicefabric/)

