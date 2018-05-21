---
title: Azure bulut Hizmetleri def NetworkTrafficRules şema | Microsoft Docs
ms.custom: ''
ms.date: 04/14/2015
services: cloud-services
ms.reviewer: ''
ms.service: cloud-services
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: reference
ms.assetid: 351b369f-365e-46c1-82ce-03fc3655cc88
caps.latest.revision: 17
author: thraka
ms.author: adegeo
manager: timlt
ms.openlocfilehash: 779d3b42aeab04bb93756439a0482f32ade6557e
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="azure-cloud-services-definition-networktrafficrules-schema"></a>Tanım NetworkTrafficRules şeması Azure bulut Hizmetleri
`NetworkTrafficRules` Rolleri birbirleri ile nasıl iletişim kuracağını belirten hizmet tanımı dosyası isteğe bağlı bir öğedeki düğümdür. Sınırlar hangi rollerin belirli rol iç uç noktalarına erişebilirsiniz. `NetworkTrafficRules` Bir tek başına öğe; değil bir hizmet tanımı dosyasında iki veya daha fazla rol ile birleştirilir.

.Csdef hizmet tanımı dosyası için varsayılan uzantısıdır.

> [!NOTE]
>  `NetworkTrafficRules` Düğüm, yalnızca kullanılabilir Azure SDK'sı sürüm 1.3 kullanarak ya da daha.

## <a name="basic-service-definition-schema-for-the-network-traffic-rules"></a>Ağ trafiği kuralları için temel hizmeti tanımı şeması
Ağ trafiği tanımlarını içeren bir hizmet tanımı dosyası temel biçimi aşağıdaki gibidir.

```xml
<ServiceDefinition …>
   <NetworkTrafficRules>
      <OnlyAllowTrafficTo>
         <Destinations>
            <RoleEndpoint endpointName="<name-of-the-endpoint>" roleName="<name-of-the-role-containing-the-endpoint>"/>
         </Destinations>
         <AllowAllTraffic/>
         <WhenSource matches="[AnyRule]">
            <FromRole roleName="<name-of-the-role-to-allow-traffic-from>"/>
         </WhenSource>
      </OnlyAllowTrafficTo>
   </NetworkTrafficRules>
</ServiceDefinition>
```

## <a name="schema-elements"></a>Şema öğeleri
`NetworkTrafficRules` Hizmet tanımı dosyası düğümünün bu konunun sonraki bölümlerinde ayrıntılı olarak açıklanan bu öğeleri içerir:

[NetworkTrafficRules öğesi](#NetworkTrafficRules)

[OnlyAllowTrafficTo öğesi](#OnlyAllowTrafficTo)

[Hedefleri öğesi](#Destinations)

[RoleEndpoint öğesi](#RoleEndpoint)

[AllowAllTraffic öğesi](#AllowAllTraffic)

[WhenSource öğesi](#WhenSource)

[FromRole öğesi](#FromRole)

##  <a name="NetworkTrafficRules"></a> NetworkTrafficRules öğesi
`NetworkTrafficRules` Öğesi belirttiğinden hangi rollerin başka bir rolü hangi uç noktasıyla iletişim kurabilir. Bir hizmet içerebilir `NetworkTrafficRules` tanımı.

##  <a name="OnlyAllowTrafficTo"></a> OnlyAllowTrafficTo öğesi
`OnlyAllowTrafficTo` Öğesi hedef uç noktaları ve onlarla iletişim kurabilmesi rolleri koleksiyonunu açıklar. Birden çok belirtebilirsiniz `OnlyAllowTrafficTo` düğümleri.

##  <a name="Destinations"></a> Hedefleri öğesi
`Destinations` Öğesi ile iletilen daha RoleEndpoints koleksiyonunu açıklar.

##  <a name="RoleEndpoint"></a> RoleEndpoint öğesi
`RoleEndpoint` Öğesi iletişime izin veren bir rol üzerinde bir uç nokta açıklar. Birden çok belirtebilirsiniz `RoleEndpoint` rolünün birden fazla uç noktası yoksa öğe.

| Öznitelik      | Tür     | Açıklama |
| -------------- | -------- | ----------- |
| `endpointName` | `string` | Gereklidir. Trafiğine izin verecek şekilde uç nokta adı.|
| `roleName`     | `string` | Gereklidir. Web rolü için iletişim sağlamak için adı.|

## <a name="allowalltraffic-element"></a>AllowAllTraffic öğesi
`AllowAllTraffic` Öğesidir tanımlanan uç noktalar ile iletişim kurmak tüm rolleri sağlayan bir kural `Destinations` düğümü.

##  <a name="WhenSource"></a> WhenSource öğesi
`WhenSource` Öğesi açıklar rolleri koleksiyonu içinde tanımlanan uç noktalar ile iletişim kurabilir daha `Destinations` düğümü.

| Öznitelik | Tür     | Açıklama |
| --------- | -------- | ----------- |
| `matches` | `string` | Gereklidir. Kuralın iletişimleri izin verirken uygulanmasını belirtir. Tek geçerli değer şu anda olduğu `AnyRule`.|
  
##  <a name="FromRole"></a> FromRole öğesi
`FromRole` Öğesi belirttiğinden tanımlanan uç noktalar ile iletişim kurabilir rolleri `Destinations` düğümü. Birden çok belirtebilirsiniz `FromRole` uç ile iletişim kurabilen birden fazla rol olduğunda öğeleri.

| Öznitelik  | Tür     | Açıklama |
| ---------- | -------- | ----------- |
| `roleName` | `string` | Gereklidir. İletişime izin vermek üzere rol adı.|

## <a name="see-also"></a>Ayrıca Bkz.
[Bulut hizmeti (Klasik) tanım Şeması](schema-csdef-file.md)