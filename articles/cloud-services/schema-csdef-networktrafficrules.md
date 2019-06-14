---
title: Azure bulut Hizmetleri def olarak NetworkTrafficRules şeması | Microsoft Docs
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
author: jpconnock
ms.author: jeconnoc
manager: timlt
ms.openlocfilehash: 8925943b0a5d151d55adedcfe3f01b5a14c63c1b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60613877"
---
# <a name="azure-cloud-services-definition-networktrafficrules-schema"></a>Azure Cloud Services tanım NetworkTrafficRules şeması
`NetworkTrafficRules` Rolleri birbirleri ile nasıl iletişim kuracağını belirtir Hizmet tanım dosyası isteğe bağlı bir öğedeki bir düğümdür. Sınırlar hangi rollerin belirli rolünün iç Uç noktalara erişebilir. `NetworkTrafficRules` Değil bir tek başına öğesi; bir hizmet tanımı dosyasında iki veya daha fazla rol ile birleştirilir.

.Csdef Hizmet tanım dosyası için varsayılan uzantısıdır.

> [!NOTE]
>  `NetworkTrafficRules` Düğüm, yalnızca Azure SDK'sı sürüm 1.3 kullanılarak kullanılabilirlik veya daha yüksek.

## <a name="basic-service-definition-schema-for-the-network-traffic-rules"></a>Temel Hizmet tanım düzenini ağ trafiği kuralları
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
`NetworkTrafficRules` Hizmet tanım dosyası düğümünün, bu konunun sonraki bölümlerinde ayrıntılı olarak açıklanan, bu öğeleri içerir:

[NetworkTrafficRules Element](#NetworkTrafficRules)

[OnlyAllowTrafficTo öğesi](#OnlyAllowTrafficTo)

[Hedefleri öğesi](#Destinations)

[RoleEndpoint öğesi](#RoleEndpoint)

AllowAllTraffic öğesi

[WhenSource öğesi](#WhenSource)

[FromRole öğesi](#FromRole)

##  <a name="NetworkTrafficRules"></a> NetworkTrafficRules öğesi
`NetworkTrafficRules` Öğesi belirtir hangi rollerin hangi rolü başka bir uç noktasıyla iletişim kurabilir. Bir hizmet içerebilir `NetworkTrafficRules` tanımı.

##  <a name="OnlyAllowTrafficTo"></a> OnlyAllowTrafficTo öğesi
`OnlyAllowTrafficTo` Hedef uç noktaları ve onlarla iletişim rolleri koleksiyonunu açıklar. Birden çok belirtebilirsiniz `OnlyAllowTrafficTo` düğümleri.

##  <a name="Destinations"></a> Hedefleri öğesi
`Destinations` Öğesi ile bildirilmesi daha RoleEndpoints koleksiyonunu açıklar.

##  <a name="RoleEndpoint"></a> RoleEndpoint öğesi
`RoleEndpoint` Öğesi ile iletişime izin vermek için bir rol üzerinde bir uç nokta açıklar. Birden çok belirtebilirsiniz `RoleEndpoint` rolünde birden fazla uç nokta varsa öğeleri.

| Öznitelik      | Tür     | Açıklama |
| -------------- | -------- | ----------- |
| `endpointName` | `string` | Gereklidir. Trafiğe izin vermek için uç nokta adı.|
| `roleName`     | `string` | Gereklidir. Web rolü için iletişim sağlamak üzere adı.|

## <a name="allowalltraffic-element"></a>AllowAllTraffic öğesi
`AllowAllTraffic` Öğedir tanımlanan uç noktaları ile iletişim kurmak için tüm roller izin verecek bir kural `Destinations` düğümü.

##  <a name="WhenSource"></a> WhenSource öğesi
`WhenSource` Açıklar rolleri koleksiyonu içinde tanımlanan uç noktaları ile iletişim kurabilmesi daha `Destinations` düğümü.

| Öznitelik | Tür     | Açıklama |
| --------- | -------- | ----------- |
| `matches` | `string` | Gereklidir. Kuralın iletişimleri izin verirken uygulanmasını belirtir. Şu anda geçerli olan tek değer olduğu `AnyRule`.|
  
##  <a name="FromRole"></a> FromRole öğesi
`FromRole` Öğe içinde tanımlanan uç noktaları ile iletişim kurabilen rolleri belirtir `Destinations` düğümü. Birden çok belirtebilirsiniz `FromRole` uç noktaları ile iletişim kurabilen birden fazla rol varsa öğeleri.

| Öznitelik  | Tür     | Açıklama |
| ---------- | -------- | ----------- |
| `roleName` | `string` | Gereklidir. İletişime izin vermek üzere rol adı.|

## <a name="see-also"></a>Ayrıca Bkz.
[Bulut hizmeti (Klasik) tanım Şeması](schema-csdef-file.md)
