---
title: Azure Service Fabric kaynak modeli giriş | Microsoft Docs
description: Service Fabric Örgü uygulamalar tanımlamak için basitleştirilmiş bir yaklaşım Service Fabric kaynak modeli hakkında bilgi edinin.
services: service-fabric-mesh
documentationcenter: .net
author: vturecek
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/12/2018
ms.author: vturecek
ms.custom: mvc, devcenter
ms.openlocfilehash: 6a96995a46cd2ad0a1afe4f4afe49d62409f2b68
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39076482"
---
# <a name="introduction-to-service-fabric-resource-model"></a>Service Fabric kaynak modeli giriş

Service Fabric kaynak modeli, bir Service Fabric Mesh uygulaması oluşturan kaynakları tanımlamak için kolay bir yaklaşım açıklanmaktadır. Aşağıdaki türdeki kaynakları şu anda bu modelde desteklenir:

- Uygulamalar
- Hizmetler
- Birimler
- Ağlar

Her kaynak bildirimli olarak basit bir YAML kaynak dosyasını, veya Mesh uygulaması açıklar ve Service Fabric platform tarafından sağlanan JSON belgesinde açıklanmıştır.

## <a name="resource-overview"></a>Kaynak genel bakış

### <a name="applications-and-services"></a>Uygulamalar ve hizmetler

Uygulama kaynağı dağıtım, sürüm ve Mesh uygulaması ömrünü birimidir. Bir veya daha fazla temsil eden bir mikro hizmet kaynaklarının oluşur. Her hizmet kaynağı, sırasıyla bir veya daha fazla aşağıdakiler dahil olmak üzere kod paket ile ilişkili kapsayıcı görüntüsünü çalıştırmak için gereken her şeyi açıklayan kod paketleri oluşur:

- Kapsayıcı adı, sürümü ve kayıt defteri
- Her kapsayıcı için gerekli CPU ve bellek kaynakları
- Ağ uç noktaları
- Ayrı birim kaynağına başvuran kapsayıcısında bağlamak için birimler.

Bir hizmet kaynağı bir parçası olarak tanımlanan tüm kod paketlerinin dağıtıldığı ve bir grup olarak birlikte etkinleştirildi. Hizmet kaynağı ayrıca kaç örneği çalıştırmak için hizmeti açıklar ve ayrıca, bağımlı diğer kaynaklar (örneğin, ağ kaynağı) başvuruyor.

Bir Mesh uygulaması birden fazla hizmet oluşuyorsa, bunlar birlikte aynı düğümde çalışan garanti edilmez. Ayrıca, uygulama bir yükseltme sırasında tek bir hizmet yükseltme hatası, önceki sürüme geri alınan tüm hizmetleri neden olur.



```yaml
    services:
      - name: helloWorldService
        properties:
          description: Hello world service.
          osType: linux
          codePackages:
            - name: helloworld
              image: myapp:1.0-alpine
              resources:
                requests:
                  cpu: 2
                  memoryInGB: 2
              endpoints:
                - name: helloWorldEndpoint
                  port: 8080
    replicaCount: 3
    networkRefs:
      - name: mynetwork
```

Daha önce alluded gibi her uygulama örneği yaşam döngüsü bağımsız olarak yönetilebilir. Örneğin, bir uygulama örneği bağımsız olarak diğer uygulama örneğinin yükseltilebilir. Yönetmek için bu duruma bir uygulamaya, daha zor yerleştirdiğiniz diğer hizmetler bağımsız olarak her servis genellikle, hizmet sayısı bir uygulamada oldukça küçük tutun.

### <a name="networks"></a>Ağlar

Ağ kaynak ayrı ayrı dağıtılabilen bağımsız olarak, bağımlılık olarak başvurduğu bir uygulama veya hizmet kaynağı kaynaktır. Uygulamalarınız için özel bir ağ oluşturmak için kullanılır. Birden çok farklı uygulamalar hizmetlerinden aynı ağın parçası olabilir.

> [!NOTE]
> Geçerli Önizleme yalnızca uygulamaları ve ağlar arasında bire bir eşleme destekler

### <a name="volumes"></a>Birimler

Birimler durumu kalıcı hale getirmek için kullanabileceğiniz kapsayıcı örneklerinizin içinde oluşturulmuş dizinleri listelenmiştir. Birim kaynağına bir dizin nasıl bağlandığını açıklamak için bildirim temelli bir yöntemini ve yedekleme depolama alanı için ' dir.

## <a name="programming-models"></a>Programlama modelleri
Hizmet kaynağını yalnızca bu kaynakla ilgili kod paketleri içinde başvurulan çalıştırmak için kapsayıcı görüntüsü gerektirir. Bilmek gerek kalmadan kapsayıcı içinde herhangi bir çerçeveyi kullanarak herhangi bir dilde yazılmış herhangi bir kod çalıştırın ya da, Service Fabric Mesh özel API'leri kullanmak. 

Uygulama kodunuza Service Fabric bile Mesh dışında taşınabilir kalır ve uygulama dağıtımlarınızı dil veya çerçeveye hizmetlerinizi uygulamak için kullanılan bağımsız olarak tutarlı kalır. Uygulamanızı, ASP.NET Core, Git veya yalnızca işlemler ve betikler bir dizi olup Service Fabric Mesh kaynak dağıtım modeli aynı kalır. 

## <a name="deployment"></a>Dağıtım

Service Fabric için Kafes dağıtırken kaynakları HTTP üzerinden Azure'a veya Azure CLI'yı Azure Resource Manager şablonları olarak dağıtılır. 


## <a name="next-steps"></a>Sonraki adımlar 
Service Fabric Mesh hakkında daha fazla bilgi için genel bakış okuyun:
- [Service Fabric Mesh genel bakış](service-fabric-mesh-overview.md)
