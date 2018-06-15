---
title: Azure sorun giderme siteden siteye VPN bağlantısını keser zaman zaman | Microsoft Docs
description: Siteden siteye VPN bağlantısı düzenli olarak bağlantısı kesilmiş sorun giderme öğrenin.
services: vpn-gateway
documentationcenter: na
author: chadmath
manager: cshepard
editor: ''
tags: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/11/2018
ms.author: genli
ms.openlocfilehash: 9c827469080195054d4ff70ab72fc123365a73df
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34160607"
---
# <a name="troubleshooting-azure-site-to-site-vpn-disconnects-intermittently"></a>Sorun giderme: Azure siteden siteye VPN aralıklı keser

Yeni veya var olan bir Microsoft Azure siteden siteye VPN bağlantısı kararlı değilse veya düzenli olarak keser sorunla karşılaşabilirsiniz. Bu makalede, sorun giderme belirlemek ve sorunun nedenini çözümlemenize yardımcı olması için adımlar sağlanmaktadır. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-steps"></a>Sorun giderme adımları

### <a name="prerequisite-step"></a>Önkoşul adım

Azure sanal ağ geçidi türünü kontrol edin:

1. Git [Azure portal](https://portal.azure.com).
2. Denetleme **genel bakış** sanal ağ geçidi türü bilgileri için sayfanın.
    
    ![Ağ geçidi genel bakış](media\vpn-gateway-troubleshoot-site-to-site-disconnected-intermittently\gatewayoverview.png)

### <a name="step-1-check-whether-the-on-premises-vpn-device-is-validated"></a>Şirket içi VPN cihazı doğrulanmış olup olmadığını 1 onay adım

1. Kullanmakta olduğunuz olup olmadığını denetleyin bir [VPN cihazı ve işletim sistemi sürümü doğrulandı](vpn-gateway-about-vpn-devices.md#devicetable). VPN cihazı doğrulanmaz, herhangi bir uyumluluk sorunu olup olmadığını görmek için aygıt üreticisinin başvurmanız gerekebilir.
2. VPN cihazı doğru yapılandırıldığından emin olun. Daha fazla bilgi için bkz: [cihaz yapılandırma örneklerini düzenleme](vpn-gateway-about-vpn-devices.md#editing).

### <a name="step-2-check-the-security-association-settingsfor-policy-based-azure-virtual-network-gateways"></a>2. adım ayarları kontrol edin güvenlik ilişkisi (ilke tabanlı Azure sanal ağ geçitleri)

1. Olduğundan emin olun sanal ağ alt ağları ve, aralıkları **yerel ağ geçidi** Microsoft Azure tanımında şirket içi VPN cihazı yapılandırma ile aynı.
2. Güvenlik ilişkisinin ayarlarla eşleştiğini doğrulayın.

### <a name="step-3-check-for-user-defined-routes-or-network-security-groups-on-gateway-subnet"></a>Ağ geçidi alt ağı üzerinde kullanıcı tanımlı yollar veya ağ güvenlik grupları için 3. adım denetimi

Bir kullanıcı tarafından tanımlanan rota ağ geçidi alt ağı üzerinde bazı trafiği kısıtlama ve diğer trafiğe izin. Bu VPN bağlantısını güvenilir olmayan bir miktar trafik için ve başkaları için iyi görünmesini sağlar. 

### <a name="step-4-check-the-one-vpn-tunnel-per-subnet-pair-setting-for-policy-based-virtual-network-gateways"></a>Adım 4 denetleyin "Bir VPN tüneli" alt ağ çifti başına (ilke tabanlı sanal ağ geçitleri için) ayarlama

Şirket içi VPN cihazı için ayarlandığından emin olun **alt ağ çifti başına bir VPN tüneli** ilke tabanlı sanal ağ geçitleri için.

### <a name="step-5-check-for-security-association-limitation-for-policy-based-virtual-network-gateways"></a>Adım 5 (için ilke tabanlı sanal ağ geçitleri) güvenlik ilişkisi sınırlaması denetle

İlke tabanlı sanal ağ geçidi 200 alt ağ güvenlik ilişkisi çiftleri sınırı vardır. Azure sanal ağ alt ağları sayısının çarpımı kat sayısı, yerel alt ağları tarafından 200'den büyük olan, kesme durumlarıyla alt bakın.

### <a name="step-6-check-on-premises-vpn-device-external-interface-address"></a>Adım 6 onay içi VPN cihazı dış arabirimi adresi

- VPN cihazının IP adresi Internet'e dahil değilse **yerel ağ geçidi** tanımı Azure'da, kullanımın bağlantısının kesilmesi karşılaşabilirsiniz.
- Cihazın dış arabirimi doğrudan Internet'te olması gerekir. Hiçbir ağ adresi çevirisi (NAT) veya Güvenlik Duvarı Internet ve aygıt arasında olmalıdır.
-  Bir sanal IP sağlamak için güvenlik duvarı kümeleme yapılandırırsanız, küme bölme ve VPN Gereci doğrudan bir ağ geçidi ile arabirim ortak arabirimi kullanıma sunar.

### <a name="step-7-check-whether-the-on-premises-vpn-device-has-perfect-forward-secrecy-enabled"></a>Şirket içi VPN cihazı kusursuz iletme etkin gizliliği olup olmadığını 7 onay adım

**Kusursuz iletme gizliliği** özelliği, bağlantı kesme sorunlara neden olabilir. VPN cihazı varsa **kusursuz iletme gizliliği** özelliği etkinse, devre dışı bırakın. Ardından [sanal ağ geçidi IPSec ilkesi güncelleştirmesi](vpn-gateway-ipsecikepolicy-rm-powershell.md#managepolicy).

## <a name="next-steps"></a>Sonraki adımlar

- [Bir sanal ağ için siteden siteye bağlantı yapılandırma](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Siteden siteye VPN bağlantıları için IPSec/IKE ilkesi yapılandırma](vpn-gateway-ipsecikepolicy-rm-powershell.md)

