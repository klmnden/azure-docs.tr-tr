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
author: thraka
ms.author: adegeo
manager: timlt
ms.openlocfilehash: 6347314e7f279356f4f3944f3238deda84f10fc0
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34358226"
---
# <a name="azure-cloud-services-config-schema-cscfg-file"></a>Config şeması (.cscfg dosyası) Azure bulut Hizmetleri
Hizmet yapılandırma dosyası, her rol hizmet dağıtmak için rol örneği sayısı, tüm yapılandırma ayarlarını ve bir rolle ilişkili herhangi bir sertifika parmak izleri değerlerini belirtir. Hizmet sanal ağın bir parçası ise, hizmet yapılandırma dosyası ve aynı zamanda sanal ağ yapılandırma dosyasındaki ağ yapılandırma bilgilerini sağlanmalıdır. Varsayılan hizmet yapılandırma dosyası için .cscfg uzantısıdır.

Hizmet modeli tarafından açıklanan [bulut hizmeti (Klasik) tanım Şeması](schema-csdef-file.md).

Azure tanılama yapılandırması şema dosyası varsayılan olarak, yüklü `C:\Program Files\Microsoft SDKs\Windows Azure\.NET SDK\<version>\schemas` dizini. Değiştir `<version>` yüklü olan sürümü ile [Azure SDK'sı](https://azure.microsoft.com/downloads/).

Rolleri bir hizmet olarak yapılandırma hakkında daha fazla bilgi için bkz: [bulut hizmeti modeli nedir](cloud-services-model-and-package.md).

## <a name="basic-service-configuration-schema"></a>Temel hizmeti yapılandırma şeması
Hizmet yapılandırma dosyasının temel biçimini aşağıdaki gibidir.

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
Aşağıdaki konular için şemayı açıklar `ServiceConfiguration` öğe:

- [Rol Şeması](schema-cscfg-role.md)
- [NetworkConfiguration Şeması](schema-cscfg-networkconfiguration.md)

## <a name="service-configuration-namespace"></a>Hizmet yapılandırma Namespace
Hizmet yapılandırma dosyası için XML ad alanı: `http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration`.

##  <a name="ServiceConfiguration"></a> ServiceConfiguration öğesi
`ServiceConfiguration` Hizmet yapılandırma dosyasının en üst düzey öğesi bir öğedir.

Aşağıdaki tabloda özniteliklerini açıklayan `ServiceConfiguration` öğesi. Tüm öznitelikleri değerleri dize türleridir.

| Öznitelik | Açıklama |
| --------- | ----------- |
|ServiceName|Gereklidir. Bulut hizmeti adı. Burada verilen adı Hizmet tanımı dosyasında belirtilen adı eşleşmelidir.|
|osFamily|İsteğe bağlı. Bulut Hizmeti rol örnekleri üzerinde çalışacak konuk işletim sistemi belirtir. Desteklenen konuk işletim sistemi sürümleri hakkında daha fazla bilgi için bkz: [Azure konuk işletim sistemi sürümleri ve SDK uyumluluk matrisi](cloud-services-guestos-update-matrix.md).<br /><br /> Dahil bir `osFamily` değeri ve ayarlanmadı `osVersion` özniteliği belirli bir konuk işletim sistemi sürümü, varsayılan değer olan 1 için kullanılır.|
|OsVersion|İsteğe bağlı. Bulut Hizmeti rol örnekleri üzerinde çalışacak konuk işletim sistemi sürümünü belirtir. Konuk işletim sistemi sürümleri hakkında daha fazla bilgi için bkz: [Azure konuk işletim sistemi sürümleri ve SDK uyumluluk matrisi](cloud-services-guestos-update-matrix.md).<br /><br /> Konuk işletim sistemi otomatik olarak en son sürüme yükseltilmelidir belirtebilirsiniz. Bunu yapmak için değerini ayarlayın `osVersion` özniteliğini `*`. Ayarlandığında `*`, rol örnekleri için belirtilen işletim sistemi ailesi konuk işletim sistemi en son sürümüne kullanılarak dağıtılır ve konuk işletim sistemi yeni sürümleri yayımlandığında otomatik olarak yükseltilecek.<br /><br /> Belirli bir sürümü el ile belirtmek için kullanın `Configuration String` tablodaki **gelecekteki, geçerli ve geçici konuk işletim sistemi sürümleri** bölümünü [Azure konuk işletim sistemi sürümleri ve SDK uyumluluk matrisi](cloud-services-guestos-update-matrix.md) .<br /><br /> İçin varsayılan değer `osVersion` özniteliği `*`.|
|schemaVersion|İsteğe bağlı. Hizmet yapılandırma şeması sürümünü belirtir. Şema sürümü yan yana SDK birden fazla sürümü yüklüyse, şema doğrulama için kullanılacak doğru SDK Araçları seçmek Visual Studio sağlar. Şema ve sürüm uyumluluğu hakkında daha fazla bilgi için bkz: [Azure konuk işletim sistemi sürümleri ve SDK uyumluluk matrisi](cloud-services-guestos-update-matrix.md)|

Hizmet yapılandırma dosyasının bir içermelidir `ServiceConfiguration` öğesi. `ServiceConfiguration` Öğesi, herhangi bir sayıda içerebilir `Role` öğeleri ve sıfır ya da 1 `NetworkConfiguration` öğeleri.