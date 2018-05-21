---
title: Azure bulut Hizmetleri def LoadBalancerProbe şema | Microsoft Docs
ms.custom: ''
ms.date: 04/14/2015
services: cloud-services
ms.reviewer: ''
ms.service: cloud-services
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: reference
ms.assetid: 113374a8-8072-4994-9d99-de391a91e6ea
caps.latest.revision: 14
author: thraka
ms.author: adegeo
manager: timlt
ms.openlocfilehash: 6cd56c9b04fc4657cedf845e7f111005a8dee183
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="azure-cloud-services-definition-loadbalancerprobe-schema"></a>Tanım LoadBalancerProbe şeması Azure bulut Hizmetleri
Yük Dengeleyici araştırmasını bir müşteri tanımlı sistem durumu araştırması UDP uç noktaları ve rol örnekleri uç noktalarını ' dir. `LoadBalancerProbe` Bir tek başına öğe; değil web rolü veya bir hizmet tanımı dosyasında çalışan rolü ile birleştirilir. A `LoadBalancerProbe` birden fazla rol tarafından kullanılabilir.

.Csdef hizmet tanımı dosyası için varsayılan uzantısıdır.

## <a name="the-function-of-a-load-balancer-probe"></a>Yük Dengeleyici araştırmasını işlevi
Azure yük dengeleyici rolü örneklerinizi yönlendirme gelen trafiği sorumludur. Yük Dengeleyici hangi örnekleri düzenli olarak her örneği bu örneğinin sistem durumunu belirlemek için yoklama tarafından trafiği alabileceğini belirler. Yük dengeleyici her örneği dakikada birden çok kez araştırmaları. Özel yük dengeleyici araştırması, hangi .csdef dosyasında LoadBalancerProbe tanımlayarak uygulanır veya yük dengeleyici – varsayılan yük dengeleyici araştırmasını örneği sağlık sağlamak için iki farklı seçenekler vardır.

Varsayılan yük dengeleyici araştırmasını dinler ve yalnızca örneği (örnek olmadığında meşgul, geri dönüştürme, durdurma, vb. durumları) gibi hazır durumda olduğunda bir HTTP 200 Tamam yanıtıyla yanıt sanal makine içinde Konuk Aracısı'nı kullanır. Konuk Aracısı ile HTTP 200 Tamam yanıt vermiyorsa, Azure yük dengeleyici örneği yanıt olarak işaretler ve trafiği için bu örneği göndermeye durdurur. Azure yük dengeleyici örneği ping işlemi devam eder ve bir HTTP 200 ile Konuk aracısı yanıt verirse, Azure yük dengeleyici trafiği için bu örneği yeniden gönderir. Web rolü kullanırken Web sitesi kodunuz genellikle Azure fabric tarafından izlenmeyen w3wp.exe veya w3wp.exe hatalar (ör. anlamına gelir Konuk aracısı çalışır HTTP 500 yanıtları) olması bildirilmedi Konuk aracısı ve yük dengeleyici döndürme dışında bu örneği yapılacak hakkında bilgi sahibi değildir.

Özel yük dengeleyici araştırmasını varsayılan Konuk aracı araştırması geçersiz kılar ve rol örneğinin sistem durumunu belirlemek için kendi özel mantık oluşturmanıza olanak sağlar. Bir TCP ACK veya HTTP 200 (varsayılan 31 saniye cinsinden) zaman aşımı süresi içinde yanıt verirse yük dengeleyici uç noktanızı (15 dakikada, varsayılan olarak) ve örnek araştırmalar dönüş düzenli olarak dikkate. Bu yük dengeleyici döndürme, 200 durum % 90 CPU örneğiyse örneğin döndürme örnekleri kaldırmak için kendi mantığını uygulamak yararlı olabilir. W3wp.exe kullanarak web rolleri için bu da otomatik al anlamına gelir, Web sitesini, Web sitesi kodunuzdaki hataları için yük dengeleyici araştırmasını 200 olmayan durumu geri beri izleme. .Csdef dosyasında bir LoadBalancerProbe tanımlamayın sonra (daha önce açıklanan) varsayılan yük dengeleyici davranışı kullanılır.

Özel yük dengeleyici araştırmasını kullanırsanız, mantığınızı RoleEnvironment.OnStop yöntemi dikkate alır emin olmalısınız. Varsayılan yük dengeleyici araştırmasını kullanırken, örnek çağrılan OnStop önce döndürme dışında alınır, ancak özel yük dengeleyici araştırmasını bir 200 Tamam OnStop olayı sırasında geri dönmek devam edebilirsiniz. OnStop olay kullanıyorsanız Önbelleği Temizle, hizmet veya aksi halde hizmet çalışma zamanını etkileyebilecek değişiklikler durdurmak için özel yük dengeleyici araştırması mantığınızı döndürme örneği kaldırır emin olmak gerekir.

## <a name="basic-service-definition-schema-for-a-load-balancer-probe"></a>Bir yük dengeleyici araştırması için temel hizmeti tanımı şeması
 Yük Dengeleyici araştırmasını içeren bir hizmet tanımı dosyası temel biçimi aşağıdaki gibidir.

```xml
<ServiceDefinition …>
   <LoadBalancerProbes>
      <LoadBalancerProbe name="<load-balancer-probe-name>" protocol="[http|tcp]" path="<uri-for-checking-health-status-of-vm>" port="<port-number>" intervalInSeconds="<interval-in-seconds>" timeoutInSeconds="<timeout-in-seconds>"/>
   </LoadBalancerProbes>
</ServiceDefinition>
```

## <a name="schema-elements"></a>Şema öğeleri
`LoadBalancerProbes` Hizmet tanımı dosyası öğesinin aşağıdaki öğeleri içerir:

- [LoadBalancerProbes öğesi](#LoadBalancerProbes)
- [LoadBalancerProbe öğesi](#LoadBalancerProbe)

##  <a name="LoadBalancerProbes"></a> LoadBalancerProbes öğesi
`LoadBalancerProbes` Öğesi yük dengeleyici araştırmalar koleksiyonunu açıklar. Bu öğe üst öğesidir [LoadBalancerProbe öğesi](#LoadBalancerProbe). 

##  <a name="LoadBalancerProbe"></a> LoadBalancerProbe öğesi
`LoadBalancerProbe` Öğe, model durumu araştırması tanımlar. Birden fazla yük dengeleyici araştırmalar tanımlayabilirsiniz. 

Aşağıdaki tabloda özniteliklerini açıklayan `LoadBalancerProbe` öğe:

|Öznitelik|Tür|Açıklama|
| ------------------- | -------- | -----------------|
| `name`              | `string` | Gereklidir. Yük Dengeleyici araştırmasını adı. Ad benzersiz olmalıdır.|
| `protocol`          | `string` | Gereklidir. Uç noktası Protokolü belirtir. Olası değerler: `http` veya `tcp`. Varsa `tcp` belirtilirse, alınan bir ACK araştırma başarılı olması gereklidir. Varsa `http` belirtilirse, belirtilen URI'ye 200 Tamam yanıttan araştırma başarılı olması gereklidir.|
| `path`              | `string` | Sistem durumu VM'den istemek için kullanılan URI. `path` gerekmiyorsa `protocol` ayarlanır `http`. Aksi takdirde, buna izin verilmez.<br /><br /> Varsayılan değer yoktur.|
| `port`              | `integer` | İsteğe bağlı. Araştırma iletişim için bağlantı noktası. Bu, aynı bağlantı noktasını daha sonra araştırması için kullanılacak gibi herhangi bir uç nokta için isteğe bağlıdır. Bunların, da yoklama için farklı bir bağlantı yapılandırabilirsiniz. Olası değerler 1 ile arasında 65535 (dahil).<br /><br /> Varsayılan değer bitiş noktası tarafından ayarlanır.|
| `intervalInSeconds` | `integer` | İsteğe bağlı. Saniye cinsinden nasıl sık uç nokta durumu için yoklama aralığı. Genellikle aralık izin veren iki Tam Araştırmalar döndürme dışında örneği çıkarmadan önce biraz daha az yarısı ayrılan zaman aşımı süresi (saniye cinsinden) olur.<br /><br /> Varsayılan değer 15'tir, en düşük değer 5'tir.|
| `timeoutInSeconds`  | `integer` | İsteğe bağlı. Saniye cinsinden zaman aşımı süresi yanıt daha fazla uç noktasına teslim edilmesini gelen trafiği durdurmak burada neden olacak araştırma uygulanır. Bu değer gerçekleştirilecek uç noktaları sağlar döndürme daha hızlı veya (olduğu varsayılan değerler) tipik kez Azure'da kullanılan daha yavaş dışında.<br /><br /> Varsayılan değer 31, 11 en küçük değerdir.|

## <a name="see-also"></a>Ayrıca Bkz.
[Bulut hizmeti (Klasik) tanım Şeması](schema-csdef-file.md)