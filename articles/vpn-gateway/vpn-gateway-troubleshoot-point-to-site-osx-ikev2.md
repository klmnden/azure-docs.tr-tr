---
title: Mac OS X istemcilerden Azure noktadan siteye VPN bağlantıları sorunlarını giderme | Microsoft Docs
description: P2S Mac OS X VPN istemci bağlantılarında sorun giderme adımları
services: vpn-gateway
documentationcenter: na
author: anzaman
manager: rossort
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/27/2018
ms.author: alzam
ms.openlocfilehash: 1cf8195cbf65f27c71a4db18c0c61c8a25673acd
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="troubleshoot-point-to-site-vpn-connections-from-mac-os-x-vpn-clients"></a>Mac OS X VPN istemcilerinden gelen noktadan siteye VPN bağlantılarının sorunlarını giderin

Bu makalede yerel VPN istemcisi ve Ikev2 kullanarak Mac OS X noktadan siteye bağlantı sorunlarını gidermenize yardımcı olur. Mac için Ikev2 VPN istemcisi çok basittir ve kadar özelleştirmesi için izin vermez. Denetlenmesi gereken yalnızca dört ayarları şunlardır:

* Sunucu Adresi
* Uzak Kimliği
* Yerel kimlik
* Kimlik doğrulama ayarları
* İşletim sistemi sürümü (10.11 veya üstü)


## <a name="VPNClient"></a> Sertifika tabanlı kimlik doğrulama sorunlarını giderme
1. VPN istemci ayarlarını kontrol edin. Git **ağ ayarı** tuşlarına basarak VPN istemci ayarlarını denetlemek için komut + Shift ve "VPN" yazın. Listeden araştırılması gereken VPN girişini tıklatın.

  ![Ikev2 sertifika tabanlı kimlik doğrulaması](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2cert1.jpg)
2. Doğrulayın **sunucu adresi** tam FQDN ve cloudapp.net içerir.
3. **Uzak Kimliği** sunucu adresi (ağ geçidi FQDN) aynı olmalıdır.
4. **Yerel kimliği** aynı olmalıdır **konu** istemci sertifikasının.
5. Tıklayın **kimlik doğrulama ayarlarını** kimlik doğrulama ayarları sayfasını açın.

  ![Kimlik doğrulama ayarları](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2auth2.jpg)
6. Doğrulayın **sertifika** aşağı açılır listeden seçilen.
7. Tıklatın **seçin** düğmesini tıklatın ve doğru sertifikanın seçildiğini doğrulayın. Tıklatın **Tamam** değişiklikleri kaydetmek için.

## <a name="ikev2"></a>Kullanıcı adı ve parola kimlik doğrulaması sorunlarını giderme

1. VPN istemci ayarlarını kontrol edin. Git **ağ ayarı** tuşlarına basarak VPN istemci ayarlarını denetlemek için komut + Shift ve "VPN" yazın. Listeden araştırılması gereken VPN girişini tıklatın.

  ![Ikev2 kullanıcı adı parolası](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2user3.jpg)
2. Doğrulayın **sunucu adresi** tam FQDN ve cloudapp.net içerir.
3. **Uzak Kimliği** sunucu adresi (ağ geçidi FQDN) aynı olmalıdır.
4. **Yerel kimliği** boş olabilir.
5. Tıklatın **kimlik doğrulama ayarını** düğmesini tıklatın ve "Username" aşağı açılır listeden seçili olduğundan emin olun.

  ![Kimlik doğrulama ayarları](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2auth4.jpg)
6. Doğru kimlik bilgilerini girdiğinizden emin olun.

## <a name="additional"></a>Ek adımlar

Önceki adımları deneyin ve her şeyin doğru şekilde yapılandırıldığından, indirme [Wireshark](https://www.wireshark.org/#download) ve paket yakalama gerçekleştirin.

1. Üzerindeki filtre *iskmp* ve bakmak **IKE_SA** paketler. SA Teklif Ayrıntıları altında bakmak gerekir **yükü: güvenlik ilişkisi**. 
2. İstemci ve sunucunun ortak sahip olduğunuzu doğrulayın.

  ![Paket](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/packet5.jpg)

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için bkz: [Microsoft Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
