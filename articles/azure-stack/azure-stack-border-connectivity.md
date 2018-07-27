---
title: Kenarlık bağlantı ağ tümleştirme konuları için Azure Stack tümleşik sistemleri | Microsoft Docs
description: Çok düğümlü Azure Stack ile veri merkezi kenarlık ağ bağlantısı planlamak için neler yapabileceğinizi öğrenin.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2018
ms.author: jeffgilb
ms.reviewer: wamota
ms.openlocfilehash: 260c58ad9099a4532c8a6558cfcf5c13f0fc8d52
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39282017"
---
# <a name="border-connectivity"></a>Kenarlık bağlantısı 
Tümleştirme ağ planlaması, başarılı Azure Stack tümleşik sistemleri dağıtımı, operasyon ve yönetimi için önemli bir önkoşuldur. Kenarlık bağlantı planlama, sınır ağ geçidi Protokolü (BGP) dinamik yönlendirme kullanılıp kullanılmayacağı seçerek başlar. Bu bir 16 bit BGP Otonom sistem numarası (genel veya özel) atama gerektirir veya statik yönlendirme kullanarak, burada statik bir varsayılan yol kenarlığı cihazlara atanır.

> [!IMPORTANT]
> Raf üstü (TOR) anahtarları üst katman 3 yukarı bağlantılar ile noktadan noktaya IP'ler gerektirir (/ 30 ağlar) fiziksel arabirimleri üzerinde yapılandırılmış. Katman 2 Yukarı bağlantılar ile Azure Stack işlemlerini destekleyen TOR anahtarlarını kullanmak için desteklenmiyor. 

## <a name="bgp-routing"></a>BGP yönlendirme
BGP gibi dinamik yönlendirme protokolü kullanarak sisteminizi her zaman ağ değişikliklerden haberdar olur ve yönetimini kolaylaştırır garanti eder. 

Aşağıdaki diyagramda gösterildiği gibi özel IP'si ile reklam TOR anahtarında temel alan bir ön ek listesini kullanarak sınırlıdır. Önek listeler, özel IP alt ağları ve yol haritası TOR kenarlığı arasındaki bağlantı olarak uygulama reddeder.

VIP adresleri dinamik olarak tanıtabilir miyim için Azure Stack çözüm içinde çalışan yazılım yük dengeleyici (SLB) TOR cihazlara eşler.

Konaklar ve HSRP veya sağlayan VRRP toplama emin olmak için kullanıcı trafiğinin hemen ve şeffaf bir şekilde hata verdi kurtarır, VPC veya MLAG yapılandırılmış TOR cihazlar arasında çok kasa bağlantısı kullanımına izin verir ve yedeklilik IP ağları için ağ.

![BGP yönlendirme](media/azure-stack-border-connectivity/bgp-routing.png)

## <a name="static-routing"></a>Statik yönlendirme
Statik yönlendirme, sınır cihazlar için ek yapılandırma gerektirir. Daha fazla el ile müdahale gerektirir ve yönetim ve bunun yanı sıra herhangi bir değişikliği ve bir yapılandırma hatası nedeniyle sorunları önce kapsamlı analiz bağlı yapılan değişiklikleri geri almak için daha fazla zaman alabilir. Önerilen yönlendirme yöntemi değildir, ancak desteklenir.

Azure Stack kullanarak statik yönlendirme ağ ortamınıza tümleştirmek için tüm dört fiziksel bağlantı kenarlık ve TOR cihaz arasında bağlanması gerekir ve nasıl statik yönlendirmenin çalıştığını nedeniyle yüksek kullanılabilirlik garanti edilemez.

Sınır cihazı TOR cihazlar P2P dış ağ veya ortak VIP ve altyapı ağı hedefleyen trafiği için işaret eden statik yollar ile yapılandırılması gerekir. Dağıtım için BMC ağ için statik yollar gerektiriyor. Müşteriler BMC ağ üzerinde bulunan bazı kaynaklara erişmek için kenarlık statik yollar bırakmayı tercih edebilirsiniz.  Statik yollar ekleme *anahtar altyapı* ve *geçiş Yönetim* ağları isteğe bağlı.

TOR cihazların tüm trafiği kenarlık cihazlara gönderme statik varsayılan bir yol ile yapılandırılmış olarak sunulur. Varsayılan kuralın tek istisnası trafiği üzerinde TOR kenarlık bağlantı için geçerli erişim denetim listesi kullanarak engellenen özel alanı içindir.

TOR ve kenarlık anahtarları arasında yukarı bağlantılar için yalnızca statik yönlendirme uygulanır. BGP dinamik yönlendirme, SLB ve diğer bileşenleri için önemli bir araçtır ve devre dışı ya da kaldırılmış olduğundan rafa içinde kullanılır.

![Statik yönlendirme](media/azure-stack-border-connectivity/static-routing.png)

## <a name="transparent-proxy"></a>Saydam Ara
Veri merkezinizi bir ara sunucu kullanmak için tüm trafik gerektiriyorsa, yapılandırmalısınız bir *saydam proxy* ağınızdaki bölgeler arasındaki trafiğin ayrılması, ilkesine göre işlemek için rafa giden tüm trafiği işlemek için.

> [!IMPORTANT]
> Azure Stack çözüm normal web proxy'leri desteklemez.  

Saydam bir proxy (olarak da bilinen bir kesintiye, satır içi veya zorlamalı proxy), herhangi bir özel istemci yapılandırma gerektirmeden ağ katmanında normal iletişimi kesintiye uğratır. İstemci proxy varlığını değil bilmeniz gerekir.

![Saydam Ara](media/azure-stack-border-connectivity/transparent-proxy.png)

## <a name="next-steps"></a>Sonraki adımlar
[DNS tümleştirme](azure-stack-integrate-dns.md)