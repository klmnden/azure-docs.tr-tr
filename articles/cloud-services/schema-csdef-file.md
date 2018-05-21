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
author: thraka
ms.author: adegeo
manager: timlt
ms.openlocfilehash: d5e086a952e18e477177634e5c197c27d4a5cc5f
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="azure-cloud-services-definition-schema-csdef-file"></a>Azure bulut Hizmetleri Tanım Şeması (.csdef dosyası)
Hizmet tanımı dosyası bir uygulama için hizmet modeli tanımlar. Dosya bir bulut hizmeti için kullanılabilecek roller için tanımları içerir, hizmet uç noktalarına belirtir ve hizmeti için yapılandırma ayarlarını oluşturur. Yapılandırma ayarı değerler tarafından açıklandığı gibi hizmet yapılandırma dosyasında ayarlanır [bulut hizmeti (Klasik) yapılandırma şeması](http://msdn.microsoft.com/library/b1ae68cd-cc95-48cb-a4a4-da91dc708a35).

Azure tanılama yapılandırması şema dosyası varsayılan olarak, yüklü `C:\Program Files\Microsoft SDKs\Windows Azure\.NET SDK\<version>\schemas` dizini. Değiştir `<version>` yüklü olan sürümü ile [Azure SDK'sı](http://www.windowsazure.com/develop/downloads/).

.Csdef hizmet tanımı dosyası için varsayılan uzantısıdır.

## <a name="basic-service-definition-schema"></a>Temel Hizmet tanım Şeması
Hizmet tanımı dosyası bir içermelidir `ServiceDefinition` öğesi. Hizmet tanımı en az bir rol içermelidir (`WebRole` veya `WorkerRole`) öğesi. Tek bir tanımında tanımlanan en fazla 25 roller içerebilir ve rol türleri karıştırabilirsiniz. Hizmet tanımı ayrıca isteğe bağlı içeren `NetworkTrafficRules` hangi rollerin kısıtlayan öğesi belirtilen iç uç noktalar ile iletişim kurabilir. Hizmet tanımı ayrıca isteğe bağlı içeren `LoadBalancerProbes` Müşteri içeren öğe tanımlanan sistem durumu araştırmalarının uç noktaları.

Hizmet tanımı dosyası temel biçimi aşağıdaki gibidir.

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
Aşağıdaki konularda şema açıklanmaktadır:

- [LoadBalancerProbe Şeması](schema-csdef-loadbalancerprobe.md)
- [WebRole Şeması](schema-csdef-webrole.md)
- [WorkerRole Şeması](schema-csdef-workerrole.md)
- [NetworkTrafficRules Şeması](schema-csdef-networktrafficrules.md)

##  <a name="ServiceDefinition"></a> ServiceDefinition öğesi
`ServiceDefinition` Hizmet tanım dosyasının en üst düzey öğesi bir öğedir.

Aşağıdaki tabloda özniteliklerini açıklayan `ServiceDefinition` öğesi.

| Öznitelik               | Açıklama |
| ----------------------- | ----------- |
| ad                    |Gereklidir. Hizmet adı. Ad hizmeti hesap içinde benzersiz olmalıdır.|
| topologyChangeDiscovery | İsteğe bağlı. Topoloji değişikliği bildirimi türünü belirtir. Olası değerler şunlardır:<br /><br /> -   `Blast` -Güncelleştirme için tüm rol örneklerini mümkün olan en kısa sürede gönderir. Seçeneği belirlerseniz, rolü yeniden olmadan topolojisi güncelleştirmesi işleyebilen olmalıdır.<br />-   `UpgradeDomainWalk` – Güncelleştirmeyi her rol örneği için bir sıralı şekilde önceki örneği başarıyla güncelleştirmeyi kabul ettikten sonra gönderir.|
| schemaVersion           | İsteğe bağlı. Hizmet tanımı şema sürümünü belirtir. Şema sürümü yan yana SDK birden fazla sürümü yüklüyse, şema doğrulama için kullanılacak doğru SDK Araçları seçmek Visual Studio sağlar.|
| upgradeDomainCount      | İsteğe bağlı. Yükseltme etki alanları arasında bu hizmet rollerinde tahsis sayısını belirtir. Hizmet dağıtılırken rol örneklerinin bir yükseltme etki alanına ayrılır. Daha fazla bilgi için bkz: [bir bulut hizmet rolü veya dağıtım güncelleştirme](cloud-services-how-to-manage-portal.md#update-a-cloud-service-role-or-deployment).<br /><br /> En fazla 20 yükseltme etki alanları belirtebilirsiniz. Belirtilmezse, yükseltme etki alanlarının varsayılan sayısı 5'tir.|