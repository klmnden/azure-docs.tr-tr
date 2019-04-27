---
title: Mac OS X istemcilerinden gelen Azure noktadan siteye VPN bağlantı sorunlarını giderme | Microsoft Docs
description: P2S Mac OS X VPN istemcisi bağlantılarında sorun giderme adımları
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
ms.openlocfilehash: 0c058cb6547d67469d3138dc331b6181c07e6e65
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60457093"
---
# <a name="troubleshoot-point-to-site-vpn-connections-from-mac-os-x-vpn-clients"></a>Mac OS X VPN istemcilerinden noktadan siteye VPN bağlantı sorunlarını giderme

Bu makale, Mac OS X yerel VPN istemcisini ve IKEv2'yi kullanarak noktadan siteye bağlantı sorunları gidermenize yardımcı olur. Mac için Ikev2 VPN istemcisi oldukça basittir ve kadar özelleştirme için izin vermez. Denetlenmesi gereken yalnızca dört ayar vardır:

* Sunucu Adresi
* Uzak Kimliği
* Yerel bir kimliği
* Kimlik doğrulama ayarları
* İşletim sistemi sürümü (10.11 veya üzeri)


## <a name="VPNClient"></a> Sertifika tabanlı kimlik doğrulaması sorunlarını giderme
1. VPN istemci ayarlarını kontrol edin. Git **ağ ayarı** tuşuna basarak VPN istemci ayarlarını denetlemek için komut + Shift ve "VPN" yazın. Listeden araştırılması gereken VPN girişe tıklayın.

   ![Ikev2 sertifika tabanlı kimlik doğrulaması](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2cert1.jpg)
2. Doğrulayın **sunucu adresi** tam FQDN ve cloudapp.net içerir.
3. **Uzak Kimliği** sunucu adresi (ağ geçidi FQDN) ile aynı olması gerekir.
4. **Yerel kimliği** aynı olmalıdır **konu** istemci sertifikası.
5. Tıklayarak **kimlik doğrulama ayarları** kimlik doğrulama ayarları sayfasını açın.

   ![Kimlik doğrulama ayarları](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2auth2.jpg)
6. Doğrulayın **sertifika** açılan listeden seçilir.
7. Tıklayın **seçin** düğmesine tıklayın ve doğru sertifikanın seçildiğini doğrulayın. Tıklayın **Tamam** yaptığınız değişiklikleri kaydedemezsiniz.

## <a name="ikev2"></a>Kullanıcı adı ve parola kimlik doğrulaması sorunlarını giderme

1. VPN istemci ayarlarını kontrol edin. Git **ağ ayarı** tuşuna basarak VPN istemci ayarlarını denetlemek için komut + Shift ve "VPN" yazın. Listeden araştırılması gereken VPN girişe tıklayın.

   ![Ikev2 kullanıcı adı parola](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2user3.jpg)
2. Doğrulayın **sunucu adresi** tam FQDN ve cloudapp.net içerir.
3. **Uzak Kimliği** sunucu adresi (ağ geçidi FQDN) ile aynı olması gerekir.
4. **Yerel kimliği** boş olamaz.
5. Tıklayın **kimlik doğrulama ayarı** düğmesine tıklayın ve açılan menüden "Username" seçili olduğundan emin olun.

   ![Kimlik doğrulama ayarları](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2auth4.jpg)
6. Doğru kimlik bilgilerinin girildiğinden emin olun.

## <a name="additional"></a>Ek adımlar

Önceki adımları deneyin ve her şeyin doğru şekilde yapılandırıldığından, indirme [Wireshark](https://www.wireshark.org/#download) ve bir paket yakalama gerçekleştirin.

1. Filtre *ISAKMP* bakın **IKE_SA** paketler. SA Teklif Ayrıntıları altında bakmak olmalıdır **yükü: Güvenlik ilişkisi**. 
2. İstemci ve sunucu bir dizi ortak sahip olduğunuzu doğrulayın.

   ![Paket](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/packet5.jpg) 
  
3. Azure ağ geçidi Yapılandırması sayfasında Azure Portalı Web sitesinde Ikev2 protokolü etkin ağ izlerini üzerinde hiçbir sunucu yanıtı ise doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için bkz. [Microsoft Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
