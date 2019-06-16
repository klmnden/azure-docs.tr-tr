---
title: İlgili Azure-yönlendirme siteye | Microsoft Docs
description: Bu makalede, noktadan siteye VPN yönlendirme nasıl davranacağını anlamanıza yardımcı olur.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 01/28/2019
ms.author: anzaman
ms.openlocfilehash: 486a910226db5dc7b36aaf873e7bb8115eb78805
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60653621"
---
# <a name="about-point-to-site-vpn-routing"></a>Noktadan Siteye VPN yönlendirme hakkında

Bu makalede Azure noktadan siteye VPN yönlendirme nasıl davranacağını anlamanıza yardımcı olur. P2S VPN yönlendirme davranışı, istemci işletim sistemi, VPN bağlantısı ve nasıl, birbirine bağlı sanal ağlar (Vnet'ler) için kullanılan protokol bağımlıdır.

Azure, şu anda uzaktan erişim için Ikev2 ve SSTP iki protokollerini destekler. Ikev2, çok sayıda istemci işletim Windows, Linux, MacOS, Android ve iOS gibi sistemlerinde desteklenir. SSTP yalnızca Windows üzerinde desteklenir. Ağınızın topolojisi için değişiklik ve Windows VPN istemciniz varsa, Windows istemcileri için VPN istemci paketi indirilir ve istemciye uygulanacak değişiklikler için sırayla yeniden yüklenir.

> [!NOTE]
> Bu makale yalnızca Ikev2 için geçerlidir.
>

## <a name="diagrams"></a>Şemaları hakkında

Bu makaledeki farklı diyagramlarda vardır. Her bölümde farklı topoloji veya yapılandırma gösterilmektedir. IPSec tünelleri hem de olduğu gibi bu makalenin amaçları için siteden siteye (S2S) ve VNet-VNet bağlantılarında aynı şekilde işlev. Bu makaledeki tüm VPN ağ geçitleri yol tabanlı.

## <a name="isolatedvnet"></a>Bir sanal ağ yalıtılmış

Bu örnekte noktadan siteye VPN ağ geçidi bağlantısı olmayan bağlı olan veya tüm diğer sanal ağ ile (VNet1 ') eşlenmiş VNet içindir. Bu örnekte, SSTP veya Ikev2 kullanan istemciler vnet1'e erişebilir.

![sanal ağ yönlendirme yalıtılmış](./media/vpn-gateway-about-point-to-site-routing/1.jpg "VNet yönlendirme yalıtılmış")

### <a name="address-space"></a>Adres alanı

* VNet1: 10.1.0.0/16

### <a name="routes-added"></a>Eklenen yollar

* Windows istemcileri için eklenen yollar: 10.1.0.0/16, 192.168.0.0/24

* Windows olmayan istemciler için eklenen yollar: 10.1.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Windows istemcileri vnet1'e erişebilir.

* Windows olmayan istemciler vnet1'e erişebilir

## <a name="multipeered"></a>Birden çok eşlenmiş sanal ağlar

Bu örnekte, noktadan siteye VPN ağ geçidi bağlantısı için VNet1 ' dir. VNet1'den vnet2'ye ile eşlenmişse. VNet 2 ile VNet3 eşlenmişse. VNet1 ile vnet4 arasında eşlenmişse. Hiçbir doğrudan VNet1 ile VNet3 arasında eşleme yoktur. VNet1 "ağ geçidi aktarımına izin ver" ve "etkin Uzak ağ geçitlerini kullan" den vnet2'ye sahiptir.

Windows kullanan istemciler doğrudan eşlenmiş Vnet'ler erişebilir, ancak sanal ağ eşlemesi veya ağ topolojisi için herhangi bir değişiklik yapılırsa VPN istemcisinin yeniden yüklenmesi gerekir. Windows olmayan istemcilerin doğrudan eşlenmiş sanal ağlar erişebilir. Erişim geçişli değildir ve yalnızca doğrudan eşlenmiş sanal ağlara sınırlıdır.

![birden çok sanal ağlar eşlenmiş](./media/vpn-gateway-about-point-to-site-routing/2.jpg "birden çok eşlenmiş sanal ağlar")

### <a name="address-space"></a>Adres alanı:

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

* VNet4: 10.4.0.0/16

### <a name="routes-added"></a>Eklenen yollar

* Windows istemcileri için eklenen yollar: 10.1.0.0/16, 10.2.0.0/16, 10.4.0.0/16, 192.168.0.0/24

* Windows olmayan istemciler için eklenen yollar: 10.1.0.0/16, 10.2.0.0/16, 10.4.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Windows istemcileri VNet1'den vnet2'ye ve vnet4 arasında erişebilir, ancak VPN istemcisi topolojisi değişikliklerin etkili olması yeniden yüklenmesi gerekir.

* Windows olmayan istemcilerin VNet1'den vnet2'ye ve vnet4 arasında erişebilirsiniz.

## <a name="multis2s"></a>Bir S2S VPN kullanarak bağlanan birden fazla sanal ağ

Bu örnekte, noktadan siteye VPN ağ geçidi bağlantısı için VNet1 ' dir. Vnet2'den vnet1'e bağlı bir siteden siteye VPN bağlantısı kullanarak. VNet2 VNet3 bağlı bir siteden siteye VPN bağlantısı kullanarak. Doğrudan bir eşlemesi veya VNet1 ile VNet3 arasında siteden siteye VPN bağlantısı yoktur. Tüm siteden siteye bağlantıları BGP yönlendirme için çalışmıyor.

Windows veya başka bir desteklenen işletim sistemi kullanan istemciler yalnızca vnet1'e erişebilir. Ek sanal ağlar erişmek için BGP kullanılması gerekir.

![birden çok sanal ağlar ve S2S](./media/vpn-gateway-about-point-to-site-routing/3.jpg "birden çok sanal ağlar ve S2S")

### <a name="address-space"></a>Adres alanı

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

### <a name="routes-added"></a>Eklenen yollar

* Windows istemcileri için eklenen yollar: 10.1.0.0/16, 192.168.0.0/24

* Windows olmayan istemciler için eklenen yollar: 10.1.0.0/16, 10.2.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Windows istemcileri yalnızca VNet1 erişebilir

* Windows olmayan istemciler yalnızca VNet1 erişebilir

## <a name="multis2sbgp"></a>Bir S2S VPN (BGP) kullanarak birden çok sanal ağlara bağlı

Bu örnekte, noktadan siteye VPN ağ geçidi bağlantısı için VNet1 ' dir. Vnet2'den vnet1'e bağlı bir siteden siteye VPN bağlantısı kullanarak. VNet2 VNet3 bağlı bir siteden siteye VPN bağlantısı kullanarak. Doğrudan bir eşlemesi veya VNet1 ile VNet3 arasında siteden siteye VPN bağlantısı yoktur. Tüm Site ve Site bağlantıları, BGP yönlendirme için çalışıyor.

İstemcileri Windows ya da başka bir desteklenen işletim sistemi kullanarak siteden siteye VPN bağlantısı kullanarak bağlı olan tüm Vnet'lerin erişebilir, ancak bağlı sanal ağlar için rotalar Windows istemcilerine el ile eklenmesi gerekir.

![birden çok sanal ağlar ve S2S (BGP)](./media/vpn-gateway-about-point-to-site-routing/4.jpg "birden çok sanal ağlar ve S2S BGP")

### <a name="address-space"></a>Adres alanı

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

### <a name="routes-added"></a>Eklenen yollar

* Windows istemcileri için eklenen yollar: 10.1.0.0/16

* Windows olmayan istemciler için eklenen yollar: 10.1.0.0/16, 10.2.0.0/16, 10.3.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Windows istemcileri VNet1'den vnet2'ye ve VNet3 erişebilir, ancak yolları'den vnet2'ye ve VNet3 el ile eklenmesi gerekir.

* Windows olmayan istemcilerin VNet1'den vnet2'ye ve VNet3 erişebilirsiniz.

## <a name="vnetbranch"></a>Bir sanal ağ ve bir şube ofisi

Bu örnekte, noktadan siteye VPN ağ geçidi bağlantısı için VNet1 ' dir. Vnet1'e bağlı olmayan / herhangi diğer sanal ağla eşlendikten ancak bir BGP çalıştırmayan bir siteden siteye VPN bağlantısı üzerinden şirket içi siteye bağlı.

Windows ve Windows olmayan istemciler, yalnızca vnet1'e erişebilir.

![bir sanal ağ ve bir şube ofisi ile yönlendirme](./media/vpn-gateway-about-point-to-site-routing/5.jpg "VNet ve şube ile yönlendirme")

### <a name="address-space"></a>Adres alanı

* VNet1: 10.1.0.0/16

* Site1: 10.101.0.0/16

### <a name="routes-added"></a>Eklenen yollar

* Windows istemcileri için eklenen yollar: 10.1.0.0/16, 192.168.0.0/24

* Windows olmayan istemciler için eklenen yollar: 10.1.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Windows istemcileri yalnızca vnet1'e erişebilir.

* Windows olmayan istemciler yalnızca VNet1 erişebilir

## <a name="vnetbranchbgp"></a>Bir sanal ağ ve bir şube ofisi (BGP)

Bu örnekte, noktadan siteye VPN ağ geçidi bağlantısı için VNet1 ' dir. Vnet1'e bağlı değil veya tüm diğer sanal ağla eşlendikten BGP çalıştıran bir siteden siteye VPN bağlantısı üzerinden şirket içi siteye (Site1) bağlı olan ancak.

Windows istemcileri VNet ve şube ofisi (Site1) erişebilir ancak Site1 yolları istemcide el ile eklenmesi gerekir. Windows olmayan istemciler, VNet ve bunun yanı sıra şirket içi şube ofisi erişebilir.

![bir sanal ağ ve bir şube ofisi (BGP)](./media/vpn-gateway-about-point-to-site-routing/6.jpg "bir sanal ağ ve bir şube ofisi")

### <a name="address-space"></a>Adres alanı

* VNet1: 10.1.0.0/16

* Site1: 10.101.0.0/16

### <a name="routes-added"></a>Eklenen yollar

* Windows istemcileri için eklenen yollar: 10.1.0.0/16, 192.168.0.0/24

* Windows olmayan istemciler için eklenen yollar: 10.1.0.0/16, 10.101.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Windows istemcileri VNet1 ve Site1 erişebilir ancak Site1 yolları el ile eklenmesi gerekir.

* Windows olmayan istemciler, VNet1 ve Site1 erişebilir.


## <a name="multivnets2sbranch"></a>S2S ve şube kullanarak bağlı birden fazla sanal ağ

Bu örnekte, noktadan siteye VPN ağ geçidi bağlantısı için VNet1 ' dir. Vnet2'den vnet1'e bağlı bir siteden siteye VPN bağlantısı kullanarak. VNet2 VNet3 bağlı bir siteden siteye VPN bağlantısı kullanarak. Doğrudan bir eşlemesi veya VNet1 ve VNet3 ağları arasında siteden siteye VPN tüneli yoktur. VNet3 bir siteden siteye VPN bağlantısı kullanarak bir şube ofisine (Site1) bağlıdır. Tüm VPN bağlantıları BGP çalışmıyor.

Tüm istemciler yalnızca vnet1'e erişebilir.

![multi-VNet S2S ve şube ofis](./media/vpn-gateway-about-point-to-site-routing/7.jpg "multi-VNet S2S ve şube ofis")

### <a name="address-space"></a>Adres alanı

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

* Site1: 10.101.0.0/16

### <a name="routes-added"></a>Eklenen yollar

* Yollar istemciler eklendi: 10.1.0.0/16, 192.168.0.0/24

* Windows olmayan istemciler için eklenen yollar: 10.1.0.0/16, 10.2.0.0/16, 10.3.0.0/16, 10.101.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Windows istemcileri VNet1 yalnızca erişebilir

* Windows olmayan istemciler yalnızca VNet1 erişebilir

## <a name="multivnets2sbranchbgp"></a>S2S ve şube ofisi (BGP) kullanarak birden çok sanal ağlara bağlı

Bu örnekte, noktadan siteye VPN ağ geçidi bağlantısı için VNet1 ' dir. Vnet2'den vnet1'e bağlı bir siteden siteye VPN bağlantısı kullanarak. VNet2 VNet3 bağlı bir siteden siteye VPN bağlantısı kullanarak. Doğrudan bir eşlemesi veya VNet1 ve VNet3 ağları arasında siteden siteye VPN tüneli yoktur. VNet3 bir siteden siteye VPN bağlantısı kullanarak bir şube ofisine (Site1) bağlıdır. Tüm VPN bağlantıları, BGP çalışıyor.

İstemcileri Windows kullanarak sanal ağlar erişebilir ve bir siteden siteye VPN bağlantısı, ancak vnet2'den, VNet3 ve Site1 yolları kullanarak bağlanan siteler için istemci el ile eklenmesi gerekir. Windows olmayan istemciler, sanal ağlar ve herhangi bir el ile müdahale olmadan bir siteden siteye VPN bağlantısı kullanarak bağlanan siteler erişebilir. Geçişli bir erişilir ve istemciler tüm bağlı sanal ağlar ve siteleri (şirket içi) kaynaklarına erişim sağlayabilir.

![multi-VNet S2S ve şube ofis](./media/vpn-gateway-about-point-to-site-routing/8.jpg "multi-VNet S2S ve şube ofis")

### <a name="address-space"></a>Adres alanı

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

* Site1: 10.101.0.0/16

### <a name="routes-added"></a>Eklenen yollar

* Yollar istemciler eklendi: 10.1.0.0/16, 192.168.0.0/24

* Windows olmayan istemciler için eklenen yollar: 10.1.0.0/16, 10.2.0.0/16, 10.3.0.0/16, 10.101.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Windows istemcileri VNet1, vnet2'den, VNet3 ve Site1 erişebilir, ancak yolları vnet2'den, VNet3 ve Site1 istemciye elle eklenmesi gerekir.

* Windows olmayan istemcilerin VNet1, vnet2'den, VNet3 ve Site1 erişebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [Azure portalını kullanarak bir P2S VPN Oluştur](vpn-gateway-howto-point-to-site-resource-manager-portal.md) , P2S VPN oluşturmaya başlayın.
