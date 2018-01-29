---
title: "Kenarlık bağlantısı ağ tümleştirme konuları Azure tümleşik yığını sistemler için | Microsoft Docs"
description: "Birden çok düğümlü Azure yığını ile veri merkezi kenarlık ağ bağlantısı planlamak için yapabileceğinizi öğrenin."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeffgilb
ms.reviewer: wamota
ms.openlocfilehash: c1a5496f3ab9a625d7d97c3096ae89100b7c5592
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="border-connectivity"></a>Kenarlık bağlantısı 
Tümleştirme ağ planlaması, başarılı Azure tümleşik yığını systems dağıtımı, işlem ve Yönetim için önemli bir önkoşuldur. Bağlantı kenarlık planlama dinamik sınır ağ geçidi Protokolü (BGP) yönlendirme kullanılıp kullanılmayacağını seçerek başlar. Bu bir 16 bit BGP Otonom sistem numarası (genel veya özel) atama gerektirir veya statik yönlendirme kullanarak, burada statik bir varsayılan yol kenarlığı cihazlara atanır.

> [!IMPORTANT]
> Raf üstü (TOR) anahtarları üst katman 3 yukarı noktadan noktaya IP'leri ile gerektirir (/ 30 ağlar) fiziksel arabiriminde yapılandırılmış. Katman 2 Yukarı bağlantılar Azure yığın işlemlerini destekleyen TOR anahtarlarla kullanmak için desteklenmiyor. 

## <a name="bgp-routing"></a>BGP yönlendirme
BGP gibi dinamik yönlendirme protokolü kullanan sisteminizin her zaman ağ değişikliklerden haberdar ve yönetimini kolaylaştırır güvence altına alır. 

Aşağıdaki çizimde gösterildiği gibi özel IP'si reklam TOR anahtarında alan öneki listesini kullanarak sınırlıdır. Önek listeler, özel IP alt ağları ve TOR kenarlığı arasında bağlantı haritada rota olarak uygulama reddeder.

VIP adresleri dinamik olarak tanıtabilirsiniz şekilde Azure yığın çözüm içinde çalışan yazılım yük dengeleyici (SLB) TOR aygıtlarda eşlere.

Ana bilgisayarlara ve HSRP veya sağlar VRRP toplama emin olmak için kullanıcı trafiğinin hemen ve saydam hatasından kurtarır, VPC veya MLAG TOR cihazlar arasında yapılandırılmış birden çok kasa bağlantı kullanılmasına izin verir ve artıklık IP ağları için ağ.

![BGP yönlendirme](media/azure-stack-border-connectivity/bgp-routing.png)

## <a name="static-routing"></a>statik yönlendirme
Statik yollar kullanarak daha fazla sabit yapılandırma kenarlık ve TOR aygıtları ekler. Herhangi bir değişiklikten önce kapsamlı analiz gerektirir. Bir yapılandırma hatası nedeniyle sorunları bağlı olarak yapılan değişiklikleri geri almak için daha fazla zaman alabilir. Önerilen yönlendirme yöntemi değildir, ancak desteklenmiyor.

Bu yöntemi kullanarak ağ ortamınıza Azure yığın tümleştirmek için kenarlık cihaz dış ağa giden trafiğe TOR cihazlara işaret eden statik yollar ortak VIP'ler yapılandırılması gerekir.

TOR aygıtları kenarlık aygıtlar için tüm trafiği göndermeye statik bir varsayılan yol ile yapılandırılmalıdır. Bu kural için bir trafik özel için özel, TOR üzerinde kenarlık bağlantı için geçerli bir erişim denetim listesi kullanılarak engellenir alanıdır.

Şey ilk yöntem ile aynı olmalıdır. Onun SLB ve diğer bileşenleri için önemli bir araçtır ve devre dışı veya kaldırılmış olduğundan BGP dinamik yönlendirme hala raf içinde kullanılır.

![statik yönlendirme](media/azure-stack-border-connectivity/static-routing.png)

## <a name="transparent-proxy"></a>Saydam proxy
Veri merkeziniz bir proxy sunucu kullanmak için tüm trafiği gerektiriyorsa, yapılandırmalısınız bir *saydam proxy* ağınızdaki bölgeler arasındaki trafiğin ayrılması ilkesine göre işlemek için raf gelen tüm trafiği işlemeye.

> [!IMPORTANT]
> Azure yığın çözüm normal web proxy'leri desteklemez.  

Saydam bir proxy (olarak da bilinen bir kesintiye uğratan, satır içi veya Zorlanmış proxy), herhangi bir özel istemci yapılandırma gerektirmeden ağ katmanında normal iletişimi kesintiye uğratır. İstemcileri proxy varlığını değil bilmeniz gerekir.

![Saydam proxy](media/azure-stack-border-connectivity/transparent-proxy.png)

## <a name="next-steps"></a>Sonraki adımlar
[DNS tümleştirmesi](azure-stack-integrate-dns.md)