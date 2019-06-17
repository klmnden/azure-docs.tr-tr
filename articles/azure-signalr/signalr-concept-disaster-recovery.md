---
title: Azure SignalR hizmeti dayanıklılık ve olağanüstü durum kurtarma
description: Dayanıklılık ve olağanüstü durum kurtarma sağlamak için birden çok SignalR hizmet örneğini konusunda bir genel bakış
author: chenkennt
ms.service: signalr
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: kenchen
ms.openlocfilehash: eb70e65db4a086afc60e91cadf55a8844b102591
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61402158"
---
# <a name="resiliency-and-disaster-recovery"></a>Dayanıklılık ve olağanüstü durum kurtarma

Dayanıklılık ve olağanüstü durum kurtarma, çevrimiçi sistemler için ortak bir gereksinimidir. Azure SignalR hizmeti, zaten % 99,9 oranında kullanılabilirliği garanti eder, ancak bunu hala bölgesel bir hizmettir.
Hizmet örneğinizi her zaman tek bir bölgede çalışıyor ve başka bir bölgeye bölge çapında kesinti olduğunda devredilmesini olmaz.

Bunun yerine, hizmetimiz SDK birden çok SignalR hizmet örnekleri destekler ve bunlardan bazıları mevcut olmadığı durumlarda diğer örneklerine otomatik olarak geçiş yapmak için bir işlevsellik sağlar.
Bu özellik, olağanüstü durum gerçekleşmeden, ancak kendiniz doğru sistemi topolojisini ayarlayın gerekir olduğunda kurtarmanız mümkün olacaktır. Bu belgede bunu öğreneceksiniz.

## <a name="high-available-architecture-for-signalr-service"></a>SignalR hizmeti için yüksek kullanılabilirlik mimarisine

Çapraz bölge dayanıklılığı SignalR hizmeti için sahip olmak için farklı bölgelerde birden fazla hizmet örneği ayarlamanız gerekir. Bu nedenle diğer tek bir bölge kapalı olduğunda, yedekleme olarak kullanılabilir.
Birden çok hizmeti örneği uygulama sunucusuna bağlanırken, birincil ve ikincil olmak üzere iki rol vardır.
Çevrimiçi trafiği sürüyor örneği birincil ve bir birincil site için tam olarak işlevsel ancak yedekleme örneği ikincil.
SDK kararlılığımızın anlaşma normal durumda istemciler yalnızca birincil Uç noktalara bağlanmak için yalnızca birincil uç noktalarını döndürür.
Ancak birincil örneği kapalı olduğunda, anlaşma istemci bağlantıları olabilmeniz ikincil uç noktalarını döndürür.
Birincil örnek hem de uygulama sunucusu normal sunucu bağlantıları bağlı, ancak ikincil örneği ve uygulama sunucusu bağlantı zayıf bağlantısı adı özel bir tür bağlanır.
Ana zayıf bağlantısının ikincil örneği başka bir bölgede yer aldığından, istemci bağlantı yönlendirmesi, kabul etmez farktır. Bir istemci için başka bir bölge yönlendirme (gecikme süresi artar) en iyi bir seçim değil.

Bir hizmet örneği, birden çok uygulama sunucuya bağlanırken farklı rollere sahip olabilir.
Çapraz bölge senaryo bir tipik kurulumu için SignalR hizmet örnekleri ve uygulama sunucuları iki (veya daha fazla) çiftleri sağlamaktır.
Her bir çifti uygulama sunucusu ve SignalR hizmeti aynı bölgede bulunan ve SignalR hizmeti birincil rolü olarak uygulama sunucusuna bağlanır.
Arasındaki her çiftleri uygulama sunucusu ve SignalR Hizmeti ayrıca bağlı, ancak SignalR ikincil bir bölgede başka bir sunucuya bağlanırken haline gelir.

Bu topoloji, bir sunucudan ileti yine de tüm istemcilerin tüm uygulama sunucuları olarak dağıtılabilecek ve SignalR hizmet örneklerini birbirine bağlıdır.
Ancak, bir istemci bağlıysa, bu her zaman en iyi ağ gecikme süresi elde etmek için aynı bölgede uygulama sunucusuna yönlendirilir.

Bu topoloji gösteren diyagram aşağıdadır:

![topology](media/signalr-concept-disaster-recovery/topology.png)

## <a name="configure-app-servers-with-multiple-signalr-service-instances"></a>SignalR hizmet örneklerine sahip birden çok uygulama sunucularını yapılandırma

Her bölgede oluşturulan SignalR hizmet ve uygulama sunucuları oluşturduktan sonra uygulama sunucularınız tüm SignalR hizmet örneklerine bağlanmak için yapılandırabilirsiniz.

Bunu yapabilirsiniz iki yolu vardır:

### <a name="through-config"></a>Yapılandırma

Adlı bir yapılandırma girişi aracılığıyla ortam değişkenleri/uygulama settings/web.cofig aracılığıyla SignalR hizmeti bağlantı dizesini ayarlama bilinen `Azure:SignalR:ConnectionString`.
Birden çok uç noktaları varsa, birden çok yapılandırma girişi her şu biçimde ayarlayabilirsiniz:

```
Azure:SignalR:Connection:<name>:<role>
```

Burada `<name>` uç nokta adı ve `<role>` (birincil veya ikincil) kendi rolüdür.
Adı isteğe bağlıdır, ancak daha fazla arasında birden fazla uç nokta yönlendirme davranışı özelleştirmek istiyorsanız yararlı olacaktır.

### <a name="through-code"></a>Kod

Bağlantı dizesinin başka bir yerde saklamak isterseniz, ayrıca kodunuzda okuyun ve bunları çağırırken parametreleri olarak kullanma `AddAzureSignalR()` (içinde ASP.NET Core) veya `MapAzureSignalR()` (ASP.NET'te).

Örnek kod aşağıda verilmiştir:

ASP.NET Core:

```cs
services.AddSignalR()
        .AddAzureSignalR(options => options.Endpoints = new ServiceEndpoint[]
        {
            new ServiceEndpoint("<connection_string1>", EndpointType.Primary, "region1"),
            new ServiceEndpoint("<connection_string2>", EndpointType.Secondary, "region2"),
        });
```

ASP.NET:

```cs
app.MapAzureSignalR(GetType().FullName, hub,  options => options.Endpoints = new ServiceEndpoint[]
    {
        new ServiceEndpoint("<connection_string1>", EndpointType.Primary, "region1"),
        new ServiceEndpoint("<connection_string2>", EndpointType.Secondary, "region2"),
    };
```

## <a name="failover-sequence-and-best-practice"></a>Yük devretme sırasını ve en iyi uygulama

Artık doğru sistemi topolojisi Kurulumu var. Bir SignalR hizmet örneği kapalı olduğunda, çevrimiçi trafiği diğer örneklerine yönlendirilir.
Birincil bir örneğe kapalı (ve bir süre sonra kurtarır olduğunda) şunlar olur:

1. Birincil hizmet örneğidir, bu örnekteki tüm sunucu bağlantılarını bırakılır.
2. Bu örneğe bağlı tüm sunucuları çevrimdışı olarak işaretleyin ve anlaşma bu son noktayı döndürme durdurup ikincil uç noktaya döndürerek.
3. Bu örnekteki tüm istemci bağlantıları da kapatılacak, istemcileri yeniden bağlanır. Uygulama sunucuları artık ikincil uç nokta dönüş olduğundan, istemciler ikincil örneğine bağlanır.
4. Artık ikincil örnekteki tüm çevrimiçi trafiği alıyor. Sunucudan tüm iletileri istemcilere hala olarak dağıtılabilecek ikincil tüm uygulama sunucularına bağlı. Ancak, istemci sunucusu iletileri aynı bölgedeki uygulama sunucuya yalnızca yönlendirilir.
5. Birincil örneği kurtarılan ve yeniden çevrimiçi olduktan sonra uygulama sunucusu bağlantıları yeniden ve çevrimiçi olarak işaretleyin. Bu nedenle yeni istemciler birincil siteye bağlı olacaktır şimdi dönüş birincil uç noktayı yeniden anlaşma. Ancak mevcut istemcilerin bırakılan olmaz ve kendilerini bağlantısını kesmek kadar ikincil siteden yönlendirilen devam eder.

Diyagramlarda, yük devretme SignalR hizmetinde nasıl yapıldığını göstermektedir:

Fig.1 önce yük devretme ![önce yük devretme](media/signalr-concept-disaster-recovery/before-failover.png)

Fig.2 sonra Yük devretme ![sonra Yük devretme](media/signalr-concept-disaster-recovery/after-failover.png)

Fig.3 kısa süre sonra birincil kurtarır ![kısa süre sonra birincil kurtarır](media/signalr-concept-disaster-recovery/after-recover.png)

Normal örneği yalnızca birincil uygulama sunucusunda görebilir ve SignalR hizmet çevrimiçi trafiği (mavi) olmalıdır.
Yük devretme işleminden sonra ikincil uygulama sunucusu ve SignalR hizmeti de etkin olur.
Birincil SignalR hizmeti yeniden çevrimiçi olduktan sonra yeni istemciler için birincil SignalR bağlanır. Ancak her iki trafik böylece var olan istemciler için ikincil bağlanmaya devam.
Tüm istemcilerin bağlantısını kes, mevcut sisteminiz geri normal (Fig.1) olacaktır.

Bölgeler arası yüksek kullanılabilirlik mimarisine uygulamak için iki ana Düzen vardır:

1. İlki bir çift uygulama sunucusu ve tüm çevrimiçi trafiği alma SignalR hizmet örneği varsa ve yedek olarak başka bir çift sağlamaktır (Aktif/Pasif olarak adlandırılan, Fig.1 gösterilen). 
2. Başka bir yedekleme (aktif/aktif, Fig.3 için benzer olarak adlandırılır) diğer çiftleri için iki (veya daha fazla) çiftleri uygulama sunucularının ve SignalR hizmet örnekleri, her biri parçası çevrimiçi trafiği ve hizmet etmesi zorunda kalmaz.

SignalR hizmeti iki desen destekleyebilir, uygulama sunucuları nasıl uygulayacağınıza temel fark olur.
Uygulama sunucuları Aktif/Pasif ise (birincil uygulama sunucusuna yalnızca kendi birincil SignalR hizmet örneği döndürür gibi) SignalR hizmet aynı zamanda Aktif/Pasif olacaktır.
Uygulama sunucuları etkin/etkin ise (tümünün trafiği almak için tüm uygulama sunucuları birincil kendi SignalR örnekleri geri döneceğimiz) SignalR hizmet aynı zamanda aktif/aktif olacaktır.

Kullanmayı tercih desenleri ne olursa olsun, her SignalR hizmet örneği, bir uygulama sunucusunda birincil olarak bağlamanız gerekecektir kaydedilmelidir.

Ayrıca SignalR bağlantısı (uzun bir bağlantı olduğu) yapısı nedeniyle, istemcilerin bağlantı düşme olağanüstü bir durum olduğunda ve yük devretme geçtiğine karşılaşırsınız.
Böyle durumlarda, son müşterileriniz saydam yapmak için istemci tarafında işleme gerekir. Bir bağlantı kapandıktan sonra Örneğin, yeniden bağlanın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, uygulamanızı SignalR hizmeti için dayanıklılığı sağlamak için yapılandırma öğrendiniz. Sunucu/istemci bağlantısı ve SignalR hizmeti bağlantı yönlendirme hakkında daha fazla ayrıntı anlamak için okuyabilirsiniz [bu makalede](signalr-concept-internals.md) SignalR hizmeti dahili bileşenleri için.

Parçalama gibi birden fazla sayıda bağlantıları işlemek için birlikte kullanmak, senaryoları ölçeklendirme için okuma [birden çok örneği ölçeklendirme](signalr-howto-scale-multi-instances.md)?