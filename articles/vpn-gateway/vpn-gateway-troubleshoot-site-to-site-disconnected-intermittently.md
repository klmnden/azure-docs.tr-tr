---
title: Sorun giderme Azure siteden siteye VPN bağlantısını keser aralıklı olarak | Microsoft Docs
description: Siteden siteye VPN bağlantısı düzenli olarak bağlantısı kesildi sorun giderme hakkında bilgi edinin.
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
ms.date: 10/30/2018
ms.author: genli
ms.openlocfilehash: 2fdd82c2f0c96b3bd20231911bb88cf54c172931
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60457765"
---
# <a name="troubleshooting-azure-site-to-site-vpn-disconnects-intermittently"></a>Sorun giderme: Azure siteden siteye VPN aralıklı olarak kesiliyor

Yeni veya mevcut bir Microsoft Azure siteden siteye VPN bağlantısı kararlı değilse veya düzenli olarak keser sorun yaşayabilirsiniz. Bu makalede, sorun giderme tanımlamanıza ve sorunun nedenini çözmenize yardımcı olması için adımları sağlanır. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-steps"></a>Sorun giderme adımları

### <a name="prerequisite-step"></a>Önkoşul adım

Azure sanal ağ geçidi türünü kontrol edin:

1. Git [Azure portalında](https://portal.azure.com).
2. Denetleme **genel bakış** sayfasında sanal ağ geçidi için tür bilgisi.
    
    ![Ağ geçidi genel bakış](media/vpn-gateway-troubleshoot-site-to-site-disconnected-intermittently/gatewayoverview.png)

### <a name="step-1-check-whether-the-on-premises-vpn-device-is-validated"></a>Şirket içi VPN cihazınız doğrulanan olmadığını 1 onay adım

1. Kullanmakta olduğunuz olup olmadığını denetleyin. bir [VPN cihazı ve işletim sistemi sürümü doğrulandı](vpn-gateway-about-vpn-devices.md#devicetable). VPN cihazı doğrulanmazsa, herhangi bir uyumluluk sorunu olup olmadığını görmek için cihaz üreticinize başvurun gerekebilir.
2. VPN cihazının düzgün yapılandırıldığından emin olun. Daha fazla bilgi için [cihaz yapılandırma örneklerini düzenleme](vpn-gateway-about-vpn-devices.md#editing).

### <a name="step-2-check-the-security-association-settingsfor-policy-based-azure-virtual-network-gateways"></a>2\. adım ayarları kontrol edin güvenlik ilişkisi (ilke tabanlı Azure sanal ağ geçitleri)

1. Emin olun sanal ağ alt ağları ve aralıkları **yerel ağ geçidi** tanımı Microsoft azure'da şirket içi VPN cihazı yapılandırmasına aynı.
2. Güvenlik ilişkisi ayarlarla eşleştiğini doğrulayın.

### <a name="step-3-check-for-user-defined-routes-or-network-security-groups-on-gateway-subnet"></a>Ağ geçidi alt ağı üzerinde kullanıcı tanımlı yollar veya ağ güvenlik grupları için 3. adım denetimi

Kullanıcı tanımlı bir yönlendirme ağ geçidi alt ağı üzerinde bazı trafiği kısıtlama ve diğer trafiğe izin veren. Bu VPN bağlantısının bazı trafik için güvenilir ve diğerleri için iyi olduğunu görünmesini sağlar. 

### <a name="step-4-check-the-one-vpn-tunnel-per-subnet-pair-setting-for-policy-based-virtual-network-gateways"></a>4\. adım "Bir VPN tüneli" alt ağ çifti başına denetleyin (için ilke tabanlı sanal ağ geçitleri) ayarlama

Şirket içi VPN cihazınız için ayarlandığından emin olun **alt ağ çifti başına bir VPN tüneli** ilke tabanlı sanal ağ geçitleri için.

### <a name="step-5-check-for-security-association-limitation-for-policy-based-virtual-network-gateways"></a>5\. adım (ilke tabanlı sanal ağ geçitleri için) güvenlik ilişkisi sınırlama denetle

İlke tabanlı sanal ağ geçidi alt ağı güvenlik ilişkisi çiftleri 200 sınırı vardır. Azure sanal ağ alt ağları sayısı çarpılır sürelerine göre yerel alt ağ sayısını 200 ' büyükse, ara sıra alt ağlar kesme bakın.

### <a name="step-6-check-on-premises-vpn-device-external-interface-address"></a>6\. adım onay şirket VPN cihazı dış arabirimi adresi

- Internet'e yönelik VPN cihazının IP adresi de dahil edilmeyeceğini **yerel ağ geçidi** tanımı Azure'da, ara sıra bağlantı kesilmesi yaşayabilirsiniz.
- Cihazın dış arabirimi doğrudan Internet'te olması gerekir. Herhangi bir ağ adresi çevirisi (NAT) veya Internet ve cihaz arasında güvenlik duvarı olmamalıdır.
-  Sanal IP'ler için güvenlik duvarı kümeleme yapılandırırsanız, küme Kes ve VPN gerecine doğrudan bir ağ geçidi ile arabirim oluşturmasını genel arabirimi kullanıma sunar.

### <a name="step-7-check-whether-the-on-premises-vpn-device-has-perfect-forward-secrecy-enabled"></a>Şirket içi VPN cihazının kusursuz iletme etkinleştirilmiş gizliliği olup 7 onay adım

**Kusursuz iletme gizliliği** özelliği bağlantı kesilmesi sorunlara neden olabilir. VPN cihazı varsa **kusursuz iletme gizliliği** özelliği etkin, devre dışı bırakın. Ardından [sanal ağ geçidi IPSec ilkesi güncelleştirme](vpn-gateway-ipsecikepolicy-rm-powershell.md#managepolicy).

## <a name="next-steps"></a>Sonraki adımlar

- [Bir sanal ağ için siteden siteye bağlantı yapılandırma](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Siteden siteye VPN bağlantıları için IPSec/IKE ilkesi yapılandırma](vpn-gateway-ipsecikepolicy-rm-powershell.md)

