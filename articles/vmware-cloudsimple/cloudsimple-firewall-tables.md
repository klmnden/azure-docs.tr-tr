---
title: Güvenlik Duvarı - CloudSimple çözümüyle VMware - Azure tabloları
description: CloudSimple özel bulut güvenlik duvarı tablolar ve güvenlik duvarı kuralları hakkında bilgi edinin.
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 861c2e86d623c46c14366f19457d1f689386a316
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64577353"
---
# <a name="firewall-tables-overview"></a>Güvenlik Duvarı tablolar genel bakış

Bir güvenlik duvarı tabloda, özel bulut kaynakları gelen ve giden ağ trafiği filtrelemeye yönelik kurallar listelenmiştir. Bunları bir VLAN ya da alt ağı uygulayabilirsiniz. Kurallar, ardından kaynak ağ veya IP adresi ve bir hedef ağ veya IP adresi arasındaki ağ trafiğini denetler.

## <a name="firewall-rules"></a>Güvenlik duvarı kuralları

Aşağıdaki tabloda, bir güvenlik duvarı kuralı parametreleri açıklar.

| Özellik | Ayrıntılar |
| ---------| --------|
| **Ad** | Güvenlik duvarı kuralı ve amacını benzersiz olarak tanımlayan bir ad. |
| **Öncelik** | 100 ile en yüksek öncelikli olan 100 ile 4096 arasında bir sayı. Kurallar öncelik sırasına göre işlenir. Trafik kuralı eşleşme arasında geldiğinde, kuralı işlemeyi durdurur. Sonuç olarak, yüksek önceliğe sahip kurallar aynı özniteliklere sahip alt önceliklere sahip mevcut herhangi bir kuralın işlenen değil.  Çakışan kurallar kaçınmak için dikkatli olun. |
| **Durum İzleme** | İzleme, durum bilgisi olmayan (özel bulut, Internet veya VPN) olabilir ya da durum bilgisi olan (genel IP).  |
| **Protokolü** | Seçenekler herhangi biri, TCP veya UDP içerir. ICMP gerektiriyorsa, kullanın. |
| **Yön** | Kuralın gelen veya giden trafiğe uygulanma seçeneği. |
| **Eylem** | İzin vermek veya trafiği kuralda tanımlanan türünün reddedin. |
| **Kaynak** | Bir IP adresi, blok classless Inter-Domain yönlendirme (CIDR) (örneğin, 10.0.0.0/24) veya herhangi.  Bir aralığı belirtmek, hizmet etiketi veya uygulama güvenlik grubu, daha az güvenlik kuralları oluşturmanıza olanak sağlar. |
| **Kaynak bağlantı noktası** | Hangi ağ üzerinden trafiğin kaynaklandığı bağlantı noktası.  Bir tek bağlantı noktası veya bağlantı noktası 443'ü veya 8000-8080 gibi aralığını belirtebilirsiniz. Aralık belirterek oluşturmanız gereken güvenlik kuralı sayısını azaltabilirsiniz. |
| **Hedef** | Bir IP adresi, blok classless Inter-Domain yönlendirme (CIDR) (örneğin, 10.0.0.0/24) veya herhangi.  Bir aralığı belirtmek, hizmet etiketi veya uygulama güvenlik grubu, daha az güvenlik kuralları oluşturmanıza olanak sağlar.  |
| **Hedef bağlantı noktası** | Bağlantı noktası, ağ trafiğini akar.  Bir tek bağlantı noktası veya bağlantı noktası 443'ü veya 8000-8080 gibi aralığını belirtebilirsiniz. Aralık belirterek oluşturmanız gereken güvenlik kuralı sayısını azaltabilirsiniz.|

### <a name="stateless"></a>Durum bilgisi olmayan

Durum bilgisi olmayan bir kural yalnızca tek tek paketlerini görünür ve bunları kurala göre filtreler.  
Ek kurallar ters yönde trafik akışı için gerekli olabilir.  Aşağıdaki noktaları arasındaki trafik için durum bilgisi olmayan kuralları kullan:

* Özel bulut alt ağlar
* Şirket içi alt ağ ve bir özel bulut alt ağ
* Özel Bulutları Internet trafiğini

### <a name="stateful"></a>Durum bilgisi olan

 Durum bilgisi olan kural üzerinden geçen bağlantıları farkındadır. Var olan bağlantılar için bir akış kaydı oluşturulur. Akış kaydının bağlantı durumuna göre iletişime izin verilir veya iletişim reddedilir.  İnternet'ten gelen trafiği filtrelemek için genel IP adresleri için bu kural türünü kullanın.

### <a name="default-rules"></a>Varsayılan kurallar

Aşağıdaki varsayılan kuralları her güvenlik duvarı masasında oluşturulur.

|Öncelik|Ad|Durum İzleme|Direction|Trafik türü|Protocol|source|Kaynak Bağlantı Noktası|Hedef|Hedef Bağlantı Noktası|Eylem|
|--------|----|--------------|---------|------------|--------|------|-----------|-----------|----------------|------|
|65000|izin ver-all-Internet|Durum bilgisi olan|Giden|Genel IP veya internet trafiği|Tümü|Tüm|Tüm|Tüm|Tüm|İzin Ver|
|65001|reddetme-all-gelen-Internet|Durum bilgisi olan|Gelen|Genel IP veya internet trafiği|Tümü|Tüm|Tüm|Tüm|Tüm|Reddet|
|65002|izin ver-all-için-intranet|Durum bilgisi olmayan|Giden|Özel bulut iç ya da VPN trafiği|Tümü|Tüm|Tüm|Tüm|Tüm|İzin Ver|
|65003|izin ver-all-gelen-intranet|Durum bilgisi olmayan|Gelen|Özel bulut iç ya da VPN trafiği|Tümü|Tüm|Tüm|Tüm|Tüm|İzin Ver|

## <a name="next-steps"></a>Sonraki adımlar

* [Güvenlik Duvarı tablolar ve kurallarını ayarlama](https://docs.azure.cloudsimple.com/firewall/)