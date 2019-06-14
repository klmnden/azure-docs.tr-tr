---
title: Azure bulut Hizmetleri def olarak LoadBalancerProbe şeması | Microsoft Docs
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
author: jpconnock
ms.author: jeconnoc
manager: timlt
ms.openlocfilehash: de365de7bf93c0a612f102b3ec2b25c79d1c3d18
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60613872"
---
# <a name="azure-cloud-services-definition-loadbalancerprobe-schema"></a>Azure Cloud Services tanım LoadBalancerProbe şeması
Yük Dengeleyici araştırması, müşteri tanımlı bir durum araştırması UDP uç noktaları ve rol örneklerinde uç noktaları olur. `LoadBalancerProbe` Bir tek başına öğesi; değil web rolü veya çalışan rolü bir hizmet tanımı dosyasında ile birleştirilir. A `LoadBalancerProbe` birden fazla rol için kullanılabilir.

.Csdef Hizmet tanım dosyası için varsayılan uzantısıdır.

## <a name="the-function-of-a-load-balancer-probe"></a>Bir yük dengeleyici araştırması işlevi
Azure Load Balancer, rol örneklerinizin gelen trafiği yönlendirmekten sorumludur. Yük dengeleyicinin trafiği düzenli olarak her örneğinin sistem durumunu bu örneği için yoklama işlemi tarafından hangi örneklerinin alabilecek belirler. Yük Dengeleyici birden çok kez dakika başına her örnek araştırmaları. Yük dengeleyiciye – varsayılan yük dengeleyici araştırması, örnek durumu sağlamak için iki farklı seçeneğiniz vardır veya özel yük dengeleyici araştırması, hangi .csdef dosyasında LoadBalancerProbe tanımlayarak uygulanır.

Varsayılan yük dengeleyici araştırması içinde dinleyen ve yalnızca bir örneği (örnek olmadığında meşgul, geri dönüştürme, durdurma, durumları vb.) gibi hazır durumda olan bir HTTP 200 OK yanıtı ile yanıt verdiği sanal makine, Konuk Aracısı'nı kullanır. Konuk Aracısı HTTP 200 OK ile yanıt vermezse, Azure Load Balancer örnek yanıt vermiyor olarak işaretler ve bu örneğe trafik göndermeyi durdurur. Azure Load Balancer örneği ping işlemi devam eder ve bir HTTP 200 Konuk aracısı yanıt verirse, Azure Load Balancer trafiği bu örneğe yeniden gönderir. Web sitesi kodunuz genellikle, Azure fabric tarafından izlenmiyor w3wp.exe veya w3wp.exe hataları (örn. yani Konuk Aracısı çalıştıran bir web rolü kullanırken HTTP 500 yanıt) olması bildirilmedi Konuk aracısı ve yük dengeleyici rotasyon dışında bu örneğe gerçekleştirilecek getireceğini bilmiyor.

Özel yük dengeleyici araştırması, varsayılan Konuk aracı araştırması geçersiz kılar ve rol örneğinin durumunu belirlemek için kendi özel mantığı oluşturmanızı sağlar. (Varsayılan değer olan 31 saniye) zaman aşımı süresi içinde bir ACK TCP veya HTTP 200 yanıt verirse yük dengeleyici uç noktanızı (15 saniyede, varsayılan olarak) ve örnek bir araştırmalarla dönüş düzenli olarak kabul. Bu, yük dengeleyici rotasyonuna % 90 CPU üzerinde örneğiyse 200 durumu örneğin döndüren, örnekleri kaldırmak için kendi mantığını uygulamak yararlı olabilir. W3wp.exe kullanarak web rolleri için aynı zamanda otomatik alma başka bir deyişle, Web sitesi yük dengeleyici araştırması için Web sitesi kodunuzdaki hataları dönüş bir 200 durumu olduğundan izleme. .Csdef dosyasında bir LoadBalancerProbe tanımlamazsanız sonra varsayılan yük dengeleyici davranış (daha önce açıklandığı gibi) kullanılabilir.

Özel yük dengeleyici araştırması kullanırsanız, mantığınızı RoleEnvironment.OnStop yöntemi dikkate alır emin olmanız gerekir. Varsayılan yük dengeleyici araştırması kullanırken, örnek çağrılan OnStop önce döndürme dışına alınır, ancak özel yük dengeleyici araştırması 200 Tamam OnStop olayı sırasında geri dönmek devam edebilirsiniz. OnStop olay kullanıyorsanız Önbelleği Temizle, hizmet veya aksi halde, hizmet çalışma zamanı davranışını etkileyen değişiklikler yapmak için özel yük dengeleyici araştırması mantığınızı döndürme örneği kaldırır emin olmak gerekir.

## <a name="basic-service-definition-schema-for-a-load-balancer-probe"></a>Temel Hizmet tanım düzenini bir yük dengeleyici araştırması
 Bir yük dengeleyici araştırması içeren bir hizmet tanımı dosyası temel biçimi aşağıdaki gibidir.

```xml
<ServiceDefinition …>
   <LoadBalancerProbes>
      <LoadBalancerProbe name="<load-balancer-probe-name>" protocol="[http|tcp]" path="<uri-for-checking-health-status-of-vm>" port="<port-number>" intervalInSeconds="<interval-in-seconds>" timeoutInSeconds="<timeout-in-seconds>"/>
   </LoadBalancerProbes>
</ServiceDefinition>
```

## <a name="schema-elements"></a>Şema öğeleri
`LoadBalancerProbes` Öğesinin hizmet tanım dosyası aşağıdaki öğeleri içerir:

- [LoadBalancerProbes öğesi](#LoadBalancerProbes)
- [LoadBalancerProbe öğesi](#LoadBalancerProbe)

##  <a name="LoadBalancerProbes"></a> LoadBalancerProbes öğesi
`LoadBalancerProbes` Yük dengeleyici araştırmalarını koleksiyonunu açıklar. Bu öğenin üst öğesi olan [LoadBalancerProbe öğesi](#LoadBalancerProbe). 

##  <a name="LoadBalancerProbe"></a> LoadBalancerProbe öğesi
`LoadBalancerProbe` Durum yoklaması için bir model öğe tanımlar. Birden fazla yük dengeleyici araştırmalarını tanımlayabilirsiniz. 

Aşağıdaki tabloda özniteliklerini açıklayan `LoadBalancerProbe` öğesi:

|Öznitelik|Tür|Açıklama|
| ------------------- | -------- | -----------------|
| `name`              | `string` | Gereklidir. Yük Dengeleyici araştırması adı. Adın benzersiz olması gerekir.|
| `protocol`          | `string` | Gereklidir. Uç noktanın Protokolü belirtir. Olası değerler: `http` veya `tcp`. Varsa `tcp` belirtilirse, alınan bir ACK araştırma başarılı olması gereklidir. Varsa `http` belirtilirse, belirtilen URI'deki 200 Tamam yanıtı araştırması başarılı olması gereklidir.|
| `path`              | `string` | Sistem durumu VM'den istemek için kullanılan URI. `path` gerekmiyorsa `protocol` ayarlanır `http`. Aksi takdirde, buna izin verilmez.<br /><br /> Varsayılan değer yoktur.|
| `port`              | `integer` | İsteğe bağlı. Araştırma iletişim için bağlantı noktası. Aynı bağlantı noktasını daha sonra araştırması için kullanılacağından bu tüm uç noktası için isteğe bağlıdır. Kullanıcıların, de yoklaması için farklı bir bağlantı noktası yapılandırabilirsiniz. Olası değerler aralığı 1'den 65535 (dahil).<br /><br /> Varsayılan değer bitiş noktası tarafından ayarlanır.|
| `intervalInSeconds` | `integer` | İsteğe bağlı. Saniye cinsinden nasıl sık uç nokta sistem durumu için yoklama aralığı. Genellikle, döndürme dışına örneği almadan önce iki tam bir araştırmalarla sağlayan biraz daha az yarısı ayrılan zaman aşımı süresi (saniye cinsinden) aralığıdır.<br /><br /> Varsayılan değer 15, en düşük değer 5'tir.|
| `timeoutInSeconds`  | `integer` | İsteğe bağlı. Saniye cinsinden zaman aşımı süresi, daha fazla uç noktaya teslim edilen gelen trafiği durdurmaya yanıt burada sonuçlanır araştırma uygulanır. Bu değer, gerçekleştirilecek uç noktaları sağlar. daha hızlı veya daha yavaş (hangi varsayılanlardır) tipik zamanlar Azure'da kullanılan sistemin dışında bırakır.<br /><br /> Varsayılan değer 31, 11 minimum değerdir.|

## <a name="see-also"></a>Ayrıca Bkz.
[Bulut hizmeti (Klasik) tanım Şeması](schema-csdef-file.md)