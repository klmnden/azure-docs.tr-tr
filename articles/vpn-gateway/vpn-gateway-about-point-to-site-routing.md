---
title: İlgili Azure noktası yönlendirme siteye | Microsoft Docs
description: Bu makalede, noktadan siteye VPN yönlendirme nasıl davranacağını anlamanıza yardımcı olur.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: ''
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/16/2018
ms.author: anzaman
ms.openlocfilehash: a0576e00d22b731f7ee9de3a9b021c0f52fc8ef9
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34702185"
---
# <a name="about-point-to-site-vpn-routing"></a>Noktadan Siteye VPN yönlendirme hakkında

Bu makalede, Azure noktadan siteye VPN yönlendirme nasıl davranacağını anlamanıza yardımcı olur. P2S VPN yönlendirme davranışını, istemci işletim sistemi, VPN bağlantısı ve nasıl, birbirlerine bağlı sanal ağlar (Vnet'ler) için kullanılan protokol bağımlıdır.

Azure şu anda, uzaktan erişim için Ikev2 ve SSTP iki protokollerini destekler. Ikev2, birçok istemci işletim Windows, Linux, MacOS, Android ve iOS gibi sistemlerinde desteklenir. SSTP yalnızca Windows üzerinde desteklenir. Ağınızın topolojisi için değişiklik ve Windows VPN istemcisi varsa, Windows istemcileri için VPN istemci paketi indirilir ve istemciye uygulanacak değişiklikler için sırayla yeniden yüklenir.

> [!NOTE]
> Bu makale yalnızca Ikev2 için geçerlidir.
>

## <a name="diagrams"></a>Diyagramlar hakkında

Bu makalede farklı diyagramları mevcuttur. Her bölüm farklı topoloji veya yapılandırma gösterir. Her ikisi de IPSec tünelleri olduğundan bu makalede amaçları doğrultusunda, siteden siteye (S2S) ve VNet-VNet bağlantıları aynı şekilde işlev. Bu makaledeki tüm VPN ağ geçitleri yol tabanlı.

## <a name="isolatedvnet"></a>Yalıtılmış bir sanal ağ

Bu örnekte noktadan siteye VPN gateway bağlantısı olmayan bağlı olan veya tüm diğer sanal ağ ile (VNet1) eşlenen bir VNet içindir. Bu örnekte, SSTP veya Ikev2 kullanan istemciler VNet1 erişebilirsiniz.

![VNet yönlendirme yalıtılmış](./media/vpn-gateway-about-point-to-site-routing/1.jpg "VNet yönlendirme yalıtılmış")

### <a name="address-space"></a>Adres alanı

* VNet1: 10.1.0.0/16

### <a name="routes-added"></a>Eklenen yollar

* Windows istemcileri için eklenen yollar: 10.1.0.0/16, 192.168.0.0/24

* Windows olmayan istemcilere eklenen yollar: 10.1.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Windows istemcileri VNet1 erişebilir

* Windows olmayan istemciler VNet1 erişebilir

## <a name="multipeered"></a>Birden çok eşlenmiş Vnet'lerde

Bu örnekte, noktadan siteye VPN ağ geçidi bağlantısı için VNet1 ' dir. VNet1 vnet2'yi ile eşlenen. VNet 2 ile VNet3 eşlenirse. VNet1 VNet4 ile eşlenen. Hiçbir doğrudan VNet1 ile VNet3 arasında eşleme yoktur. VNet1 "ağ geçidi transit izin ver" ve "Uzak ağ geçitleri etkin kullan" "VNet2 sahiptir.

Windows kullanan istemciler doğrudan eşlenmiş Vnet'lerde erişebilirsiniz, ancak VNet eşlemesi veya ağ topolojisi için herhangi bir değişiklik yapılırsa VPN istemcisinin yeniden yüklenmesi gerekir. Windows olmayan istemcileri doğrudan eşlenmiş Vnet'lerde erişebilir. Erişim geçişli değil ve yalnızca doğrudan eşlenmiş Vnet'ler için sınırlı olur.

![birden çok sanal ağlar eşlenen](./media/vpn-gateway-about-point-to-site-routing/2.jpg "birden çok eşlenen sanal ağlar")

### <a name="address-space"></a>Adres alanı:

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

* VNet4: 10.4.0.0/16

### <a name="routes-added"></a>Eklenen yollar

* Windows istemcileri için eklenen yollar: 10.1.0.0/16, 10.2.0.0/16, 10.4.0.0/16, 192.168.0.0/24

* Windows olmayan istemcilere eklenen yollar: 10.1.0.0/16, 10.2.0.0/16, 10.4.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Windows istemcileri VNet1, vnet2'yi ve VNet4 erişebilirsiniz, ancak VPN istemcisi hiçbir topoloji değişikliklerin etkili olması için yeniden yüklenmesi gerekir.

* Windows olmayan istemciler VNet1, vnet2'yi ve VNet4 erişebilir

## <a name="multis2s"></a>S2S VPN kullanarak bağlanan birden çok sanal ağlar

Bu örnekte, noktadan siteye VPN ağ geçidi bağlantısı için VNet1 ' dir. VNet1 vnet2'yi için bağlı bir siteden siteye VPN bağlantısı kullanarak. VNet2 vnet3 arasında bağlı bir siteden siteye VPN bağlantısı kullanarak. Doğrudan eşliği veya VNet1 ile VNet3 arasında siteden siteye VPN bağlantısı yoktur. Tüm siteden siteye bağlantıları BGP yönlendirme için çalışmıyor.

Windows veya başka bir desteklenen işletim sistemi kullanan istemciler yalnızca VNet1 erişebilir. Ek sanal ağlara erişim için BGP kullanılması gerekir.

![birden çok sanal ağlar ve S2S](./media/vpn-gateway-about-point-to-site-routing/3.jpg "birden çok sanal ağlar ve S2S")

### <a name="address-space"></a>Adres alanı

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

### <a name="routes-added"></a>Eklenen yollar

* Windows istemcileri için eklenen yollar: 10.1.0.0/16, 192.168.0.0/24

* Windows olmayan istemcilere eklenen yollar: 10.1.0.0/16, 10.2.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Windows istemcileri yalnızca VNet1 erişebilir

* Windows olmayan istemciler VNet1 yalnızca erişebilir

## <a name="multis2sbgp"></a>Bağlantılı bir S2S VPN (BGP) kullanarak birden çok sanal ağlar

Bu örnekte, noktadan siteye VPN ağ geçidi bağlantısı için VNet1 ' dir. VNet1 vnet2'yi için bağlı bir siteden siteye VPN bağlantısı kullanarak. VNet2 vnet3 arasında bağlı bir siteden siteye VPN bağlantısı kullanarak. Doğrudan eşliği veya VNet1 ile VNet3 arasında siteden siteye VPN bağlantısı yoktur. Tüm siteden siteye bağlantıları BGP yönlendirme için çalışır.

Windows veya başka bir desteklenen işletim sistemi kullanan istemciler bir siteden siteye VPN bağlantısı kullanarak bağlanan tüm sanal ağlar erişebilirsiniz, ancak bağlı sanal ağlar yolları Windows istemcileri için el ile eklenmesi gerekir.

![birden çok sanal ağlar ve S2S (BGP)](./media/vpn-gateway-about-point-to-site-routing/4.jpg "birden çok sanal ağlar ve S2S BGP")

### <a name="address-space"></a>Adres alanı

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

### <a name="routes-added"></a>Eklenen yollar

* Windows istemcileri için eklenen yollar: 10.1.0.0/16

* Windows olmayan istemcilere eklenen yollar: 10.1.0.0/16, 10.2.0.0/16, 10.3.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Windows istemcileri VNet1, vnet2'yi ve VNet3 erişebilirsiniz, ancak vnet2'yi ve VNet3 yollarını el ile eklenmesi gerekir.

* Windows olmayan istemciler VNet1, vnet2'yi ve VNet3 erişebilir

## <a name="vnetbranch"></a>Bir sanal ağ ve bir şube ofisi

Bu örnekte, noktadan siteye VPN ağ geçidi bağlantısı için VNet1 ' dir. VNet1 bağlı değil / herhangi diğer sanal ağla eşlenen ancak BGP çalıştırmayan bir siteden siteye VPN bağlantısı üzerinden bir şirket içi siteye bağlı.

Windows istemcileri VNet1 ve şube ofisi (Site1) erişebilirsiniz, ancak Site1 yollarını istemciye elle eklenmesi gerekir. Windows olmayan istemciler, şirket içi Site1 yanı sıra, VNet1 erişim sağlayabilir.

![VNet ve bir şube ofisi yönlendirme](./media/vpn-gateway-about-point-to-site-routing/5.jpg "VNet ve bir şube ofisi yönlendirme")

### <a name="address-space"></a>Adres alanı

* VNet1: 10.1.0.0/16

* Site1: 10.101.0.0/16

### <a name="routes-added"></a>Eklenen yollar

* Windows istemcileri için eklenen yollar: 10.1.0.0/16, 192.168.0.0/24

* Windows olmayan istemcilere eklenen yollar: 10.1.0.0/16, 10.101.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Windows istemcileri yalnızca VNet1 erişebilir

* Windows olmayan istemciler VNet1 yalnızca erişebilir

## <a name="vnetbranchbgp"></a>Bir sanal ağ ve bir şube ofisi (BGP)

Bu örnekte, noktadan siteye VPN ağ geçidi bağlantısı için VNet1 ' dir. VNet1 bağlı değil veya herhangi başka sanal ağla eşlenen BGP çalıştıran bir siteden siteye VPN bağlantısı üzerinden şirket içi siteye (Site1) bağlı olan ancak.

Windows istemcileri VNet ve şube ofisi (Site1) erişebilirsiniz, ancak Site1 yollarını istemciye elle eklenmesi gerekir. Windows olmayan istemciler, şirket içi şube ofisi yanı sıra sanal ağ erişim sağlayabilir.

![bir sanal ağ ve bir şube ofisi (BGP)](./media/vpn-gateway-about-point-to-site-routing/6.jpg "bir VNet ve bir şube ofisi")

### <a name="address-space"></a>Adres alanı

* VNet1: 10.1.0.0/16

* Site1: 10.101.0.0/16

### <a name="routes-added"></a>Eklenen yollar

* Windows istemcileri için eklenen yollar: 10.1.0.0/16, 192.168.0.0/24

* Windows olmayan istemcilere eklenen yollar: 10.1.0.0/16, 10.101.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Windows istemcileri VNet1 ve Site1 erişebilirsiniz, ancak Site1 yollarını el ile eklenmesi gerekir.

* Windows olmayan istemcileri VNet1 ve Site1 erişebilir.


## <a name="multivnets2sbranch"></a>S2S ve bir şube ofisi kullanarak bağlı birden çok sanal ağlar

Bu örnekte, noktadan siteye VPN ağ geçidi bağlantısı için VNet1 ' dir. VNet1 vnet2'yi için bağlı bir siteden siteye VPN bağlantısı kullanarak. VNet2 vnet3 arasında bağlı bir siteden siteye VPN bağlantısı kullanarak. Doğrudan eşliği veya VNet1 ve VNet3 ağları arasında siteden siteye VPN tüneli yoktur. Vnet3 arasında bir siteden siteye VPN bağlantısı kullanarak bir şube ofisine (Site1) bağlı. BGP tüm VPN bağlantıları çalışmıyor.

Tüm istemcilerin VNet1 yalnızca erişebilir.

![multi-VNet S2S ve şube ofis](./media/vpn-gateway-about-point-to-site-routing/7.jpg "multi-VNet S2S ve şube ofis")

### <a name="address-space"></a>Adres alanı

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

* Site1: 10.101.0.0/16

### <a name="routes-added"></a>Eklenen yollar

* Yollar eklenen istemcileri: 10.1.0.0/16, 192.168.0.0/24

* Windows olmayan istemcilere eklenen yollar: 10.1.0.0/16, 10.2.0.0/16, 10.3.0.0/16, 10.101.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Windows istemcileri VNet1 yalnızca erişebilir

* Windows olmayan istemciler VNet1 yalnızca erişebilir

## <a name="multivnets2sbranchbgp"></a>S2S ve bir şube ofisi (BGP) kullanarak birden çok sanal ağlara bağlı

Bu örnekte, noktadan siteye VPN ağ geçidi bağlantısı için VNet1 ' dir. VNet1 vnet2'yi için bağlı bir siteden siteye VPN bağlantısı kullanarak. VNet2 vnet3 arasında bağlı bir siteden siteye VPN bağlantısı kullanarak. Doğrudan eşliği veya VNet1 ve VNet3 ağları arasında siteden siteye VPN tüneli yoktur. Vnet3 arasında bir siteden siteye VPN bağlantısı kullanarak bir şube ofisine (Site1) bağlı. Tüm VPN bağlantıları BGP çalışıyor.

Windows kullanan istemciler sanal ağlar erişebilir ve bir siteden siteye VPN bağlantısı, ancak vnet2'yi, VNet3 ve Site1 yollarını kullanarak bağlanan siteler istemciye elle eklenmesi gerekir. Windows olmayan istemciler, sanal ağlar ve el ile müdahalesi olmadan bir siteden siteye VPN bağlantısı kullanarak bağlanan siteler erişim sağlayabilir. Erişim geçişli ve istemcilerin tüm bağlı sanal ağlar ve siteleri (şirket içi)'deki kaynaklara erişebilir.

![multi-VNet S2S ve şube ofis](./media/vpn-gateway-about-point-to-site-routing/8.jpg "multi-VNet S2S ve şube ofis")

### <a name="address-space"></a>Adres alanı

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

* Site1: 10.101.0.0/16

### <a name="routes-added"></a>Eklenen yollar

* Yollar eklenen istemcileri: 10.1.0.0/16, 192.168.0.0/24

* Windows olmayan istemcilere eklenen yollar: 10.1.0.0/16, 10.2.0.0/16, 10.3.0.0/16, 10.101.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Windows istemcileri VNet1, vnet2'yi, VNet3 ve Site1 erişebilirsiniz, ancak yolları vnet2'yi, VNet3 ve Site1 istemciye elle eklenmesi gerekir.

* Windows olmayan istemcileri VNet1, vnet2'yi, VNet3 ve Site1 erişebilir.

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [P2S Azure portalını kullanarak bir VPN Oluştur](vpn-gateway-howto-point-to-site-resource-manager-portal.md) P2S VPN oluşturmaya başlamak için.
