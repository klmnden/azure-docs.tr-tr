---
title: "Azure bulut Hizmetleri rolü şema | Microsoft Docs"
ms.custom: 
ms.date: 12/07/2016
ms.prod: azure
ms.reviewer: 
ms.service: cloud-services
ms.suite: 
ms.tgt_pltfrm: 
ms.topic: reference
ms.assetid: e4fbffc1-98eb-449c-971c-de415e45ab34
caps.latest.revision: "12"
author: thraka
ms.author: adegeo
manager: timlt
ms.openlocfilehash: c25e7d139c7b7fd7c5da6bde8cfb9050eec8a88e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-cloud-services-config-role-schema"></a>Config rol şema Azure bulut Hizmetleri

`Role` Yapılandırma dosyasının öğesinin hizmetindeki tüm yapılandırma ayarlarının değerleri her rol için dağıtmak için rol örneklerinin sayısını belirtir ve bir rolle ilişkili herhangi bir sertifika parmak izleri.

Azure hizmet yapılandırma şeması hakkında daha fazla bilgi için bkz: [bulut hizmeti (Klasik) yapılandırma şeması](schema-cscfg-file.md). Azure hizmet tanımı şeması hakkında daha fazla bilgi için bkz: [bulut hizmeti (Klasik) tanım Şeması](schema-csdef-file.md).

##  <a name="Role"></a>Rol öğesi
Aşağıdaki örnekte gösterildiği `Role` öğesi ve kendi alt öğelerini.

```xml 
<ServiceConfiguration>
  <Role name="<role-name>" vmName="<vm-name>">
    <Instances count="<number-of-instances>"/>
    <ConfigurationSettings>
      <Setting name="<setting-name>" value="<setting-value>" />
    </ConfigurationSettings>
    <Certificates>
      <Certificate name="<certificate-name>" thumbprint="<certificate-thumbprint>" thumbprintAlgorithm="<algorithm>"/>
    </Certificates>
  </Role>
</ServiceConfiguration>
```

Aşağıdaki tabloda özniteliklerini açıklar `Role` öğesi.

| Öznitelik | Açıklama |
| --------- | ----------- |
| ad   | Gereklidir. Rolün adını belirtir. Adı, hizmet tanımı dosyası rolünde için sağlanan adıyla eşleşmelidir.|
| vmName | İsteğe bağlı. Bir sanal makine için DNS adını belirtir. Ad 10 karakter olmalıdır veya daha az.|

Aşağıdaki tabloda alt öğeleri açıklanmıştır `Role` öğesi.

| Öğesi | Açıklama |
| ------- | ----------- |
| Örnekler | Gereklidir. Role dağıtılacak örnek sayısını belirtir. Örnek sayısı bir tamsayı tarafından tanımlanan `count` özniteliği.|
| Ayar   | İsteğe bağlı. Ayar adı ve değeri bir rol için ayarlar koleksiyonu belirtir. Ayar adı için bir dize tarafından tanımlanan `name` özniteliği ve ayar değerinin tarafından tanımlanan bir dize `value` özniteliği.|
| Sertifika | İsteğe bağlı. Adını, parmak izi ve rol ile ilişkili olması için bir hizmet sertifikası algoritmasını belirtir. Sertifika adı için bir dize tarafından tanımlanan `name` özniteliği. Sertifika parmak izi için boşluk içermeyen onaltılık rakam dizisini tarafından tanımlanan `thumbprint` özniteliği. Onaltılık sayı, sayılar ve büyük harf alfasayısal karakterler kullanılarak temsil edilebilir. Sertifika algoritması için bir dize tarafından tanımlanan `thumbprintAlgorithm` özniteliği.|

## <a name="see-also"></a>Ayrıca Bkz.
[Bulut hizmeti (Klasik) yapılandırma şeması](schema-cscfg-file.md)