---
title: Azure Cloud Services Tanım Şeması (.cscfg dosyası) | Microsoft Docs
services: cloud-services
ms.custom: ''
ms.date: 12/07/2016
ms.reviewer: ''
ms.service: cloud-services
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: reference
ms.assetid: 3ddc7fea-3339-4fc0-bdf9-853c32b25f69
caps.latest.revision: 35
author: jpconnock
ms.author: jeconnoc
manager: timlt
ms.openlocfilehash: 424381e2c243420cc2a68dc776d249cb17574f98
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62130322"
---
# <a name="azure-cloud-services-config-schema-cscfg-file"></a>Azure Cloud Services yapılandırma şeması (.cscfg dosyası)
Hizmet yapılandırma dosyası, her rol için dağıtmak için rol örneklerinin sayısını, tüm yapılandırma ayarlarını ve bir rolle ilişkili herhangi bir sertifika parmak izleri değerlerini belirtir. Hizmet, bir sanal ağın parçası ise, sanal ağ yapılandırma dosyasını yanı sıra hizmet yapılandırma dosyası, ağ yapılandırma bilgileri sağlanmalıdır. Hizmet yapılandırma dosyası için varsayılan .cscfg uzantısıdır.

Hizmet modeli tarafından açıklanan [bulut hizmeti (Klasik) tanım Şeması](schema-csdef-file.md).

Azure tanılama yapılandırma şema dosyası varsayılan olarak yüklü `C:\Program Files\Microsoft SDKs\Windows Azure\.NET SDK\<version>\schemas` dizin. Değiştirin `<version>` 'ın yüklü sürümü ile [Azure SDK'sı](https://azure.microsoft.com/downloads/).

Rolleri bir hizmet olarak yapılandırma hakkında daha fazla bilgi için bkz. [bulut hizmeti modeli nedir](cloud-services-model-and-package.md).

## <a name="basic-service-configuration-schema"></a>Temel hizmet yapılandırma şeması
Hizmet yapılandırma dosyasının temel biçimi aşağıdaki gibidir.

```xml
<ServiceConfiguration serviceName="<service-name>" osFamily="<osfamily-number>" osVersion="<os-version>" schemaVersion="<schema-version>">

  <Role …>
    …
  </Role>

  <NetworkConfiguration>
    …
  </NetworkConfiguration>

</ServiceConfiguration>
```

## <a name="schema-definitions"></a>Şema tanımları
Aşağıdaki konular için şema tanımlamak `ServiceConfiguration` öğesi:

- [Rol Şeması](schema-cscfg-role.md)
- [NetworkConfiguration Şeması](schema-cscfg-networkconfiguration.md)

## <a name="service-configuration-namespace"></a>Hizmet yapılandırma Namespace
Hizmet yapılandırma dosyası için XML ad alanı: `http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration`.

##  <a name="ServiceConfiguration"></a> ServiceConfiguration öğesi
`ServiceConfiguration` Hizmet yapılandırma dosyasının en üst düzey öğesi bir öğedir.

Aşağıdaki tabloda özniteliklerini açıklayan `ServiceConfiguration` öğesi. Tüm öznitelik değerleri dize türleridir.

| Öznitelik | Açıklama |
| --------- | ----------- |
|serviceName|Gereklidir. Bulut hizmeti adı. Burada verilen ad, hizmet tanımı dosyasında belirtilen adı eşleşmelidir.|
|osFamily|İsteğe bağlı. Bulut hizmetindeki rol örneklerinde çalıştırılacak konuk işletim sistemi belirtir. Desteklenen konuk işletim sistemi sürümleri hakkında daha fazla bilgi için bkz: [Azure konuk işletim sistemi sürümleri ve SDK uyumluluk matrisi](cloud-services-guestos-update-matrix.md).<br /><br /> Dahil etmezseniz bir `osFamily` değeri ve ayarlanmadı `osVersion` özniteliği belirli bir konuk işletim sistemi sürümü, varsayılan değer olan 1'için kullanılır.|
|osVersion|İsteğe bağlı. Bulut hizmetindeki rol örneklerinde çalıştırılacak konuk işletim sistemi sürümünü belirtir. Konuk işletim sistemi sürümleri hakkında daha fazla bilgi için bkz. [Azure konuk işletim sistemi sürümleri ve SDK uyumluluk matrisi](cloud-services-guestos-update-matrix.md).<br /><br /> Konuk işletim Sisteminin en son sürüme otomatik olarak yükseltilmelidir belirtebilirsiniz. Bunu yapmak için değeri ayarlamak `osVersion` özniteliğini `*`. Ayarlandığında `*`, rol örnekleri için belirtilen işletim sistemi ailesi konuk işletim Sisteminin en son sürümü kullanılarak dağıtılır ve yeni konuk işletim sistemi sürümleri yayımlandığında otomatik olarak yükseltilecektir.<br /><br /> Belirli bir sürümünü el ile belirtmek için kullanın `Configuration String` tablosundan **gelecekteki, geçerli ve geçiş konuk işletim sistemi sürümleri** bölümünü [Azure konuk işletim sistemi sürümleri ve SDK uyumluluk matrisi](cloud-services-guestos-update-matrix.md) .<br /><br /> İçin varsayılan değer `osVersion` özniteliği `*`.|
|schemaVersion|İsteğe bağlı. Hizmet yapılandırma şeması sürümünü belirtir. Şema sürümü yan yana Visual Studio SDK'ın birden fazla sürümü yüklüyse, şema doğrulama için kullanılacak doğru SDK Araçları seçmenizi sağlar. Şema ve sürüm uyumluluğu hakkında daha fazla bilgi için bkz. [Azure konuk işletim sistemi sürümleri ve SDK uyumluluk matrisi](cloud-services-guestos-update-matrix.md)|

Hizmet yapılandırma dosyasının bir içermelidir `ServiceConfiguration` öğesi. `ServiceConfiguration` Herhangi bir sayıda öğe içerebilir `Role` öğeleri ve sıfır ya da 1 `NetworkConfiguration` öğeleri.