---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 12/06/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 0c0ad6ea5a687d066c78533b45a7f531561661bf
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188238"
---
Bu SSS, Klasik dağıtım modelini kullanan P2S bağlantıları için geçerlidir.

### <a name="what-client-operating-systems-can-i-use-with-point-to-site"></a>Noktadan Siteye ile hangi istemci işletim sistemlerini kullanabilirim?

Aşağıdaki istemci işletim sistemleri desteklenmektedir:

* Windows 7 (32 bit ve 64 bit)
* Windows Server 2008 R2 (yalnızca 64 bit)
* Windows 8 (32 bit ve 64 bit)
* Windows 8.1 (32 bit ve 64 bit)
* Windows Server 2012 (yalnızca 64 bit)
* Windows Server 2012 R2 (yalnızca 64 bit)
* Windows 10

### <a name="can-i-use-any-software-vpn-client-that-supports-sstp-for-point-to-site"></a>SSTP destekleyen noktadan siteye için herhangi bir yazılım VPN istemcisi kullanabilir miyim?

Hayır. Yalnızca listelenen Windows işletim sistemi sürümleri için destek sınırlıdır.

### <a name="how-many-vpn-client-endpoints-can-exist-in-my-point-to-site-configuration"></a>Kaç VPN istemci uç noktadan siteye yapılandırma içinde bulunabilir?

En fazla 128 VPN istemcileri, aynı anda bir sanal ağa bağlanabilir.

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Noktadan Siteye bağlanabilirlik için kendi iç PKI kök CA’mı kullanabilir miyim?

Evet. Önceden, yalnızca otomatik olarak imzalanan kök sertifikalar kullanılabiliyordu. Yine de, en fazla 20 kök sertifika yükleyebilirsiniz.

### <a name="can-i-traverse-proxies-and-firewalls-by-using-point-to-site"></a>Noktadan siteye kullanarak miyim proxy ve güvenlik duvarlarını geçirebilir miyim?

Evet. Güvenlik duvarları üzerinden tünele Güvenli Yuva Tünel Protokolü (SSTP) kullanıyoruz. Bu tünel bir HTTPS bağlantısı olarak görünür.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-the-vpn-automatically-reconnect"></a>Noktadan Siteye için yapılandırılmış istemci bilgisayarını yeniden başlatırsam VPN de otomatik olarak yeniden bağlanacak mı?

Varsayılan olarak, istemci bilgisayar VPN bağlantısını otomatik olarak yeniden olmaz.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-the-vpn-clients"></a>Noktadan siteye destek otomatik yeniden bağlanmayı ve ddns'yi Destekler VPN istemcilerde?

Hayır. Otomatik yeniden ve DDNS şu anda noktadan siteye VPN'lerde desteklenmiyor.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-for-the-same-virtual-network"></a>Siteden siteye ve noktadan siteye yapılandırmaları için aynı sanal ağda olabilir mi?

Evet. Her iki çözüm de, ağ geçidiniz için yol tabanlı VPN türünüz varsa çalışacaktır. Klasik dağıtım modeli için dinamik bir ağ geçidiniz olması gerekir. Kullanmamız destek noktadan siteye statik yönlendirme VPN ağ geçitleri veya kullanan ağ geçitleri için **- VpnType PolicyBased** cmdlet'i.

### <a name="can-i-configure-a-point-to-site-client-to-connect-to-multiple-virtual-networks-at-the-same-time"></a>Aynı anda birden çok sanal ağa bağlanmak için Noktadan Siteye istemcisi yapılandırabilir miyim?

Evet. Ancak, sanal ağların, çakışan IP öneklerinin sahip olamaz ve noktadan siteye adres alanlarından sanal ağlar arasında çakışmaması gerekir.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>Siteden Siteye ve Noktadan Siteye bağlantılardan ne kadar verimlilik bekleyebilirim?

VPN tünellerinin tam verimini elde etmek zordur. IPsec ve SSTP şifrelemesi ağır VPN protokolleridir. Aktarım hızı, şirket içi ve internet arasındaki bant genişliği ve gecikme süresiyle de sınırlıdır.
