---
title: Azure Cloud Services rolü şema | Microsoft Docs
ms.custom: ''
ms.date: 12/07/2016
services: cloud-services
ms.reviewer: ''
ms.service: cloud-services
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: reference
ms.assetid: e4fbffc1-98eb-449c-971c-de415e45ab34
caps.latest.revision: 12
author: jpconnock
ms.author: jeconnoc
manager: timlt
ms.openlocfilehash: 20f4186426152d2dc9b445981a69881c35587eb6
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39005790"
---
# <a name="azure-cloud-services-config-role-schema"></a>Azure bulut Hizmetleri rol yapılandırma şeması

`Role` Herhangi bir sertifika parmak izleri bir rolle ilişkili ve yapılandırma dosyası öğesi herhangi bir yapılandırma ayarı değerini hizmetin her rol için dağıtmak için rol örneklerinin sayısını belirtir.

Azure hizmet yapılandırma şeması hakkında daha fazla bilgi için bkz: [bulut hizmeti (Klasik) yapılandırma şeması](schema-cscfg-file.md). Azure Hizmet tanım düzenini hakkında daha fazla bilgi için bkz: [bulut hizmeti (Klasik) tanım Şeması](schema-csdef-file.md).

##  <a name="Role"></a> Rol öğesi
Aşağıdaki örnekte gösterildiği `Role` öğesi ve onun alt öğeleri.

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

Aşağıdaki tablo için öznitelikler açıklanmaktadır `Role` öğesi.

| Öznitelik | Açıklama |
| --------- | ----------- |
| ad   | Gereklidir. Rolün adını belirtir. Hizmet tanım dosyası rol için sağlanan adı eşleşmelidir.|
| vmName | İsteğe bağlı. Bir sanal makinenin DNS adını belirtir. 10 karakter adı olmalıdır ya da daha az.|

Aşağıdaki tabloda, alt öğelerini açıklar `Role` öğesi.

| Öğe | Açıklama |
| ------- | ----------- |
| Örnekler | Gereklidir. Role dağıtılacak örnek sayısını belirtir. Örnek sayısı için bir tamsayı tarafından tanımlanan `count` özniteliği.|
| Ayar   | İsteğe bağlı. Bir ayarın adını ve değerini bir rol için bir ayarlar koleksiyonu belirtir. Ayar adı için bir dize tarafından tanımlanan `name` özniteliği ve ayar değeri bir dize tarafından tanımlanır `value` özniteliği.|
| Sertifika | İsteğe bağlı. Ad, parmak izi ve rolüyle ilişkilendirilmesi için bir hizmet sertifikası algoritması belirtir. Sertifika adı için bir dize tarafından tanımlanan `name` özniteliği. Sertifika parmak izini onaltılık sayı için boşluk içeren bir dize tarafından tanımlanan `thumbprint` özniteliği. Onaltılık sayılar, rakam ve büyük harfli alfasayısal karakterler kullanılarak temsil edilebilir. Sertifika algoritma için bir dize tarafından tanımlanan `thumbprintAlgorithm` özniteliği.|

## <a name="see-also"></a>Ayrıca Bkz.
[Bulut hizmeti (Klasik) yapılandırma şeması](schema-cscfg-file.md)