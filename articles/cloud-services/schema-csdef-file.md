---
title: Azure Cloud Services Tanım Şeması (.csdef dosyası) | Microsoft Docs
ms.custom: ''
ms.date: 04/14/2015
services: cloud-services
ms.reviewer: ''
ms.service: cloud-services
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: reference
ms.assetid: b7735dbf-8e91-4d1b-89f7-2f17e9302469
caps.latest.revision: 42
author: jpconnock
ms.author: jeconnoc
manager: timlt
ms.openlocfilehash: 9cb78362b5c0613d6ed6820bbf8e6d3275ab4787
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51250671"
---
# <a name="azure-cloud-services-definition-schema-csdef-file"></a>Azure Cloud Services Tanım Şeması (.csdef dosyası)
Hizmet tanım dosyası, bir uygulama için hizmet modelini tanımlar. Dosya bir bulut hizmeti için kullanılabilir olan rolleri için tanımları içerir, hizmet uç noktalarını belirtir ve hizmeti için yapılandırma ayarlarını oluşturur. Yapılandırma ayarı değerleri, hizmet yapılandırma dosyasında ayarlanır, açıklandığı [bulut hizmeti (Klasik) yapılandırma şeması](https://msdn.microsoft.com/library/b1ae68cd-cc95-48cb-a4a4-da91dc708a35).

Azure tanılama yapılandırma şema dosyası varsayılan olarak yüklü `C:\Program Files\Microsoft SDKs\Windows Azure\.NET SDK\<version>\schemas` dizin. Değiştirin `<version>` 'ın yüklü sürümü ile [Azure SDK'sı](http://www.windowsazure.com/develop/downloads/).

.Csdef Hizmet tanım dosyası için varsayılan uzantısıdır.

## <a name="basic-service-definition-schema"></a>Temel Hizmet tanım Şeması
Bir hizmet tanımı dosyası içermelidir `ServiceDefinition` öğesi. Hizmet tanımı en az bir rol içermesi gerekir (`WebRole` veya `WorkerRole`) öğesi. Tek bir tanım içinde tanımlanan en fazla 25 rollerini içerebilir ve rol türleri karıştırabilirsiniz. Hizmet tanımı ayrıca isteğe bağlı içeren `NetworkTrafficRules` hangi rollerin kısıtlayan öğesi için belirtilen iç uç nokta iletişim kurabilir. Hizmet tanımı ayrıca isteğe bağlı içeren `LoadBalancerProbes` Müşteri içeren öğe tanımlanan uç nokta sistem durumu araştırmaları.

Hizmet tanım dosyası temel biçimi aşağıdaki gibidir.

```xml
<ServiceDefinition name="<service-name>" topologyChangeDiscovery="<change-type>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" upgradeDomainCount="<number-of-upgrade-domains>" schemaVersion="<version>">
  
  <LoadBalancerProbes>
         …
  </LoadBalancerProbes>
  
  <WebRole …>
         …
  </WebRole>
  
  <WorkerRole …>
         …
  </WorkerRole>
  
  <NetworkTrafficRules>
         …
  </NetworkTrafficRules>

</ServiceDefinition>
```

## <a name="schema-definitions"></a>Şema tanımları
Aşağıdaki konularda, şema açıklanmaktadır:

- [LoadBalancerProbe Şeması](schema-csdef-loadbalancerprobe.md)
- [WebRole Şeması](schema-csdef-webrole.md)
- [WorkerRole Şeması](schema-csdef-workerrole.md)
- [NetworkTrafficRules Şeması](schema-csdef-networktrafficrules.md)

##  <a name="ServiceDefinition"></a> ServiceDefinition öğesi
`ServiceDefinition` Öğesidir üst düzey hizmet tanımı dosyası.

Aşağıdaki tabloda özniteliklerini açıklayan `ServiceDefinition` öğesi.

| Öznitelik               | Açıklama |
| ----------------------- | ----------- |
| ad                    |Gereklidir. Hizmetin adı. Ad hizmet hesabı içinde benzersiz olmalıdır.|
| topologyChangeDiscovery | İsteğe bağlı. Topoloji değişikliği bildirimi türünü belirtir. Olası değerler şunlardır:<br /><br /> -   `Blast` -Güncelleştirmeleri tüm rol örneklerine olabildiğince çabuk gönderir. Seçeneği tercih ederseniz, rolü topoloji güncelleştirmeyi yeniden olmadan işleyebilir olması gerekir.<br />-   `UpgradeDomainWalk` – Güncelleştirme her rol örneği için sıralı bir şekilde önceki örneği başarıyla güncelleştirmeyi kabul ettikten sonra gönderir.|
| schemaVersion           | İsteğe bağlı. Hizmet tanım düzenini sürümünü belirtir. Şema sürümü yan yana Visual Studio SDK'ın birden fazla sürümü yüklüyse, şema doğrulama için kullanılacak doğru SDK Araçları seçmenizi sağlar.|
| upgradeDomainCount      | İsteğe bağlı. Yükseltme etki alanları arasında bu hizmeti rollerinde ayrılmasına sayısını belirtir. Hizmet dağıtılırken, rol örneklerinin bir yükseltme etki alanına ayrılır. Daha fazla bilgi için [bir bulut hizmeti rolü veya dağıtımını güncelleştirme](cloud-services-how-to-manage-portal.md#update-a-cloud-service-role-or-deployment).<br /><br /> En fazla 20 yükseltme etki alanları belirtebilirsiniz. Belirtilmezse, yükseltme etki alanlarının varsayılan sayısı 5'tir.|