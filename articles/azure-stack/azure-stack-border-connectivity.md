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
ms.date: 08/30/2018
ms.author: jeffgilb
ms.reviewer: wamota
ms.openlocfilehash: 12219e2df875d317aece73cabebdfb55115f7b41
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54021093"
---
# <a name="border-connectivity"></a>Kenarlık bağlantısı 
Tümleştirme ağ planlaması, başarılı Azure Stack tümleşik sistemleri dağıtımı, operasyon ve yönetimi için önemli bir önkoşuldur. Kenarlık bağlantı planlama, sınır ağ geçidi Protokolü (BGP) dinamik yönlendirme kullanılıp kullanılmayacağı seçerek başlar. Bu bir 16 bit BGP Otonom sistem numarası (genel veya özel) atama gerektirir veya statik yönlendirme kullanarak, burada statik bir varsayılan yol kenarlığı cihazlara atanır.

> [!IMPORTANT]
> Raf üstü (TOR) anahtarları üst katman 3 yukarı bağlantılar ile noktadan noktaya IP'ler gerektirir (/ 30 ağlar) fiziksel arabirimleri üzerinde yapılandırılmış. Katman 2 Yukarı bağlantılar ile Azure Stack işlemlerini destekleyen TOR anahtarlarını kullanmak için desteklenmiyor. 

## <a name="bgp-routing"></a>BGP yönlendirme
BGP gibi dinamik yönlendirme protokolü kullanarak sisteminizi her zaman ağ değişikliklerden haberdar olur ve yönetimini kolaylaştırır garanti eder. Gelişmiş güvenlik için bir parola TOR ve kenarlığı arasında eşleme BGP üzerinde ayarlanabilir. 

Aşağıdaki diyagramda gösterildiği gibi özel IP'si ile reklam izlerinin TOR anahtarında alan bir ön ek listesini kullanarak engellenir. Ön ek listesini özel ağ'ın Tanıtımı reddeder ve TOR kenarlığı arasındaki bağlantıyı haritada rota olarak uygulanır.

VIP adresleri dinamik olarak tanıtabilir miyim için Azure Stack çözüm içinde çalışan yazılım yük dengeleyici (SLB) TOR cihazlara eşler.

Konaklar ve HSRP veya sağlayan VRRP toplama emin olmak için kullanıcı trafiğinin hemen ve şeffaf bir şekilde hata verdi kurtarır, VPC veya MLAG yapılandırılmış TOR cihazlar arasında çok kasa bağlantısı kullanımına izin verir ve yedeklilik IP ağları için ağ.

![BGP yönlendirme](media/azure-stack-border-connectivity/bgp-routing.png)

## <a name="static-routing"></a>Statik yönlendirme
Statik yönlendirme, sınır cihazlar için ek yapılandırma gerektirir. Daha fazla el ile müdahale gerektirir ve yönetim ve bunun yanı sıra herhangi bir değişikliği ve bir yapılandırma hatası nedeniyle sorunları önce kapsamlı analiz bağlı yapılan değişiklikleri geri almak için daha fazla zaman alabilir. Önerilen yönlendirme yöntemi değildir, ancak desteklenir.

Azure Stack kullanarak statik yönlendirme ağ ortamınıza tümleştirmek için tüm dört fiziksel bağlantı kenarlık ve TOR cihaz arasında bağlanması gerekir ve nasıl statik yönlendirmenin çalıştığını nedeniyle yüksek kullanılabilirlik garanti edilemez.

Sınır cihazı hedefleyen trafiği için TOR cihazlara P2P işaret eden statik yollar ile yapılandırılmalıdır *dış* ağ veya ortak VIP ve *altyapı* ağ. Statik yollara gerektirecek *BMC* ve *dış* dağıtımı için ağ. İşleçleri bulunan yönetim kaynaklarına erişmek için kenarlık statik yollar bırakmayı tercih edebilir *BMC* ağ. Statik yollar ekleme *anahtar altyapı* ve *geçiş Yönetim* ağları isteğe bağlı.

TOR cihazların tüm trafiği kenarlık cihazlara gönderme statik varsayılan bir yol ile yapılandırılmış olarak sunulur. Varsayılan kuralın tek istisnası trafiği üzerinde TOR kenarlık bağlantı için geçerli erişim denetim listesi kullanarak engellenen özel alanı içindir.

TOR ve kenarlık anahtarları arasında yukarı bağlantılar için yalnızca statik yönlendirme uygulanır. BGP dinamik yönlendirme, SLB ve diğer bileşenleri için önemli bir araçtır ve devre dışı ya da kaldırılmış olduğundan rafa içinde kullanılır.

![Statik yönlendirme](media/azure-stack-border-connectivity/static-routing.png)

<sup>\*</sup> BMC ağ dağıtımdan sonra isteğe bağlıdır.

<sup>\*\*</sup> Anahtar altyapı ağı, isteğe bağlı aynıdır tüm ağ anahtarı yönetimi ağ dahil edilebilir.

<sup>\*\*\*</sup> Anahtar Yönetim ağ gereklidir ve anahtar altyapı ağ üzerinden ayrı olarak eklenebilir.

## <a name="transparent-proxy"></a>Saydam Ara
Veri merkezinizi bir ara sunucu kullanmak için tüm trafik gerektiriyorsa, yapılandırmalısınız bir *saydam proxy* ağınızdaki bölgeler arasındaki trafiğin ayrılması, ilkesine göre işlemek için rafa giden tüm trafiği işlemek için.

> [!IMPORTANT]
> Azure Stack çözüm normal web proxy'leri desteklemez.  

Saydam bir proxy (olarak da bilinen bir kesintiye, satır içi veya zorlamalı proxy), herhangi bir özel istemci yapılandırma gerektirmeden ağ katmanında normal iletişimi kesintiye uğratır. İstemci proxy varlığını değil bilmeniz gerekir.

![Saydam Ara](media/azure-stack-border-connectivity/transparent-proxy.png)

## <a name="next-steps"></a>Sonraki adımlar
[DNS tümleştirme](azure-stack-integrate-dns.md)
