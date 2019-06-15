---
title: Azure Service Fabric Java istemci API'leri | Microsoft Docs
description: Oluşturma ve Service Fabric Java istemci API'leri kullanan Service Fabric istemci REST API belirtim kullanılıyor
services: service-fabric
documentationcenter: java
author: rapatchi
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: java
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/27/2017
ms.author: rapatchi
ms.openlocfilehash: 97bba87331965b0f7ce20ec2ee089e0e18f72457
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60720289"
---
# <a name="azure-service-fabric-java-client-apis"></a>Azure Service Fabric Java istemci API'leri

Service Fabric istemci API'leri dağıtma ve yönetme mikro hizmetler azure'da, şirket içinde yerel geliştirme makineniz veya diğer bulut uygulamaları ve Service Fabric kümesinde kapsayıcıları temel sağlar. Bu makalede oluşturun ve Service Fabric istemci REST API'ler üzerinde Service Fabric Java istemci API'leri kullanma

## <a name="generate-the-client-code-using-autorest"></a>AutoRest kullanarak istemci kodu oluşturma

[AutoRest](https://github.com/Azure/autorest) , RESTful web hizmetlerine erişme için istemci kitaplıkları oluşturan bir araçtır. AutoRest giriş Openapı belirtim biçimini kullanarak REST API'sini açıklayan bir özelliğidir. [Service Fabric istemci REST API'leri](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/servicefabric/data-plane) Bu belirtim izleyin.

AutoRest aracını kullanarak Service Fabric Java istemci kodu oluşturmak üzere aşağıda belirtilen adımları izleyin.

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

3. Çatal ve kopya [azure rest API özellikleri](https://github.com/Azure/azure-rest-api-specs) yerel makine ve makinenizin terminalden kopyalanan konumuna git deposunda.


4. Kopyalanan deponuzda aşağıda belirtilen konuma gidin.
    ```bash
    cd specification\servicefabric\data-plane\Microsoft.ServiceFabric\stable\6.0
    ```

    > [!NOTE]
    > Küme sürümünüzün 6.0 değilse. * kararlı klasöründe uygun dizine gidin.
    >   

5. Java istemci kodu oluşturmak üzere aşağıdaki autorest komutu çalıştırın.
    
    ```bash
    autorest --input-file= servicefabric.json --java --output-folder=[output-folder-name] --namespace=[namespace-of-generated-client]
    ```
   Autorest kullanımını gösteren bir örnek aşağıda verilmiştir.
   
    ```bash
    autorest --input-file=servicefabric.json --java --output-folder=java-rest-api-code --namespace=servicefabricrest
    ```
   
   Aşağıdaki komutu alır ``servicefabric.json`` belirtimi dosya giriş olarak ve java istemci kodda oluşturur ``java-rest-api-     code`` klasörü ve kodda kapsayan ``servicefabricrest`` ad alanı. Bu adımdan sonra iki klasör bulacağından ``models``, ``implementation`` ve iki dosya ``ServiceFabricClientAPIs.java`` ve ``package-info.java`` oluşturulan ``java-rest-api-code`` klasör.


## <a name="include-and-use-the-generated-client-in-your-project"></a>Dahil ve projenizde oluşturulan istemciyi kullanma

1. Oluşturulan kodu uygun şekilde projenize ekleyin. Oluşturulan kod kullanarak bir kitaplığı oluşturup bu kitaplığı projenize dahil öneririz.
2. Bir kitaplık oluşturuyorsanız, ardından aşağıdaki bağımlılık kitaplığınızın projeye dahil et seçerseniz. Takip ediyorsanız farklı bir yaklaşım ardından dahil bağımlılık uygun şekilde.

    ```
        GroupId:  com.microsoft.rest
        Artifactid: client-runtime
        Version: 1.2.1
    ```
    Örneğin, yapı sistem ekleme aşağıdaki Maven kullanıyorsanız, ``pom.xml`` dosyası:

    ```xml
        <dependency>
          <groupId>com.microsoft.rest</groupId>
          <artifactId>client-runtime</artifactId>
          <version>1.2.1</version>
        </dependency>
    ```

3. Aşağıdaki kodu kullanarak bir RestClient oluşturun:

    ```java
        RestClient simpleClient = new RestClient.Builder()
            .withBaseUrl("http://<cluster-ip or name:port>")
            .withResponseBuilderFactory(new ServiceResponseBuilder.Factory())
            .withSerializerAdapter(new JacksonAdapter())
            .build();
        ServiceFabricClientAPIs client = new ServiceFabricClientAPIsImpl(simpleClient);
    ```
4. İstemci nesnesini kullanın ve gerektiğinde uygun çağrıları yapın. İstemci nesnesi kullanımını gösteren bazı örnekleri aşağıda verilmiştir. Uygulama paketi oluşturulur ve görüntü deposuna kullanmadan önce karşıya varsayıyoruz API'nin aşağıda.
    * Uygulama sağlama
    
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

## <a name="understanding-the-generated-code"></a>Oluşturulan kodu anlama
Her API için uygulama dört aşırı bulabilirsiniz. Ardından isteğe bağlı parametreler varsa bu isteğe bağlı parametreleri de dahil olmak üzere dört daha fazla çeşitleme bulun. Örneğin bir API göz önünde bulundurun ``removeReplica``.
 1. **public void removeReplica (dize nodeName, UUID PartitionID, dize ReplicaID, Boole forceRemove, uzun bir zaman aşımı)**
    * Bu, zaman uyumlu değişken removeReplica API çağrısı
 2. **Genel ServiceFuture\<Void > removeReplicaAsync (dize nodeName, UUID PartitionID, dize ReplicaID, Boole forceRemove, uzun zaman aşımı, son ServiceCallback\<Void > serviceCallback)**
    * Gelecekteki tabanlı zaman uyumsuz programlama ve geri çağırmaları kullanmak istiyorsanız, bu API çağrısı türevi kullanılabilir
 3. **Genel Observable\<Void > removeReplicaAsync (dize nodeName, UUID PartitionID, dize ReplicaID)**
    * Reaktif zaman uyumsuz programlama kullanmak istiyorsanız, bu API çağrısı çeşidini kullanılabilir
 4. **Genel Observable\<ServiceResponse\<Void >> removeReplicaWithServiceResponseAsync (dize nodeName, UUID PartitionID, dize ReplicaID)**
    * Reaktif zaman uyumsuz programlama kullanın ve ham rest yanıtı ile uğraşmak istiyorsanız bu API çağrısı çeşidini kullanılabilir

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [Service Fabric REST API'leri](https://docs.microsoft.com/rest/api/servicefabric/)

