---
title: "Bağlanamıyor bir Azure siteden siteye VPN bağlantısı sorunlarını giderme | Microsoft Docs"
description: "Aniden çalışmayı durduruyor ve bağlanılamaz bir siteden siteye VPN bağlantısı sorunlarını giderme öğrenin."
services: vpn-gateway
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/13/2017
ms.author: genli
ms.openlocfilehash: f5fe877a46586af63c0991e3c3fbb8d42f69736c
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="troubleshooting-an-azure-site-to-site-vpn-connection-cannot-connect-and-stops-working"></a>Sorun giderme: Bir Azure siteden siteye VPN bağlantısı bağlanamıyor ve çalışmayı durduruyor

Bir şirket içi ağınız ve Azure sanal ağı arasında siteden siteye VPN bağlantısı yapılandırdıktan sonra VPN bağlantısı aniden çalışmayı durdurur ve yeniden bağlanılamaz. Bu makalede, bu sorunu gidermenize yardımcı olmak için sorun giderme adımlarını sağlar. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-steps"></a>Sorun giderme adımları

Sorunu gidermek için önce dener [Azure VPN gateway sıfırlama](vpn-gateway-resetgw-classic.md) ve şirket içi VPN aygıttan tünel sıfırlayın. Sorun devam ederse, sorunun nedenini belirlemek için şu adımları izleyin.

### <a name="prerequisite-step"></a>Önkoşul adım

Azure VPN ağ geçidi türünü kontrol edin.

1. [Azure Portal](https://portal.azure.com) gidin.

2. Denetleme **genel bakış** VPN ağ geçidi türü bilgi sayfası.
    
    ![Ağ Geçidi'ne genel bakış](media\vpn-gateway-troubleshoot-site-to-site-cannot-connect\gatewayoverview.png)

### <a name="step-1-check-whether-the-on-premises-vpn-device-is-validated"></a>1. Adım Şirket içi VPN cihazı doğrulanıp doğrulanmadığını denetleyin

1. Kullanmakta olduğunuz olup olmadığını denetleyin bir [VPN cihazı ve işletim sistemi sürümü doğrulandı](vpn-gateway-about-vpn-devices.md#devicetable). Cihaz doğrulanan VPN cihazı değilse, bir uyumluluk sorunu olup olmadığını görmek için herhangi bir tuşa basın gerekebilir.

2. VPN cihazı doğru yapılandırıldığından emin olun. Daha fazla bilgi için bkz: [cihaz yapılandırma örneklerini düzenleme](vpn-gateway-about-vpn-devices.md#editing).

### <a name="step-2-verify-the-shared-key"></a>2. Adım Paylaşılan anahtar doğrulayın

Paylaşılan anahtar Azure sanal ağ anahtarların eşleştiğinden emin olmak için VPN için şirket içi VPN aygıtınızın karşılaştırın. 

Azure VPN bağlantısı için paylaşılan anahtar görüntülemek için aşağıdaki yöntemlerden birini kullanın:

**Azure portalı**

1. Oluşturduğunuz VPN ağ geçidi siteden siteye bağlantısı gidin.

2. İçinde **ayarları** 'yi tıklatın **paylaşılan anahtarı**.
    
    ![Paylaşılan anahtar](media/vpn-gateway-troubleshoot-site-to-site-cannot-connect/sharedkey.png)

**Azure PowerShell**

Azure Resource Manager dağıtım modeli için:

    Get-AzureRmVirtualNetworkGatewayConnectionSharedKey -Name <Connection name> -ResourceGroupName <Resource group name>

Klasik dağıtım modeli için:

    Get-AzureVNetGatewayKey -VNetName -LocalNetworkSiteName

### <a name="step-3-verify-the-vpn-peer-ips"></a>3. Adım VPN eş IP doğrulayın

-   IP tanımında **yerel ağ geçidi** Azure nesnesinde, şirket içi cihaz IP eşleşmelidir.
-   Şirket içi cihaz üzerinde ayarlanır Azure ağ geçidi IP tanımı Azure ağ geçidi IP eşleşmelidir.

### <a name="step-4-check-udr-and-nsgs-on-the-gateway-subnet"></a>4. Adım. Ağ geçidi alt ağı üzerinde UDR ve Nsg'ler denetleyin

Denetleyin ve kullanıcı tanımlı yönlendirme (UDR) veya ağ güvenlik grupları (Nsg'ler) ağ geçidi alt ağda kaldırın ve ardından test sonucu. Bu sorun çözülene, UDR veya NSG uygulanan ayarlarını doğrulayın.

### <a name="step-5-check-the-on-premises-vpn-device-external-interface-address"></a>5. Adım. Şirket içi VPN cihazı dış arabirimi adresi denetleyin

- VPN cihazı Internet'e yönelik IP adresini de dahil edilmişse **yerel ağ** tanımı Azure'da, kullanımın bağlantısının kesilmesi karşılaşabilirsiniz.
- Cihazın dış arabirimi doğrudan Internet'te olması gerekir. Herhangi bir ağ adresi çevirisi veya Güvenlik Duvarı Internet ve aygıt arasında olmalıdır.
- Sanal bir IP için kümeleme güvenlik duvarını yapılandırmak için küme bölme ve VPN Gereci doğrudan bir ağ geçidi ile arabirim ortak arabirimi kullanıma sunar.

### <a name="step-6-verify-that-the-subnets-match-exactly-azure-policy-based-gateways"></a>6. Adım. Alt ağlar tam olarak eşleştiğinden emin olun (Azure ilke tabanlı ağ geçitleri)

-   Alt ağlar Azure sanal ağı ve Azure sanal ağı için şirket içi tanımları arasında tam olarak eşleştiğini doğrulayın.
-   Alt ağlar arasında tam olarak eşleştiğinden emin olun **yerel ağ geçidi** ve şirket içi ağınız için tanımları.

### <a name="step-7-verify-the-azure-gateway-health-probe"></a>7. Adım. Azure ağ geçidi durumu araştırması doğrulayın

1. Sistem durumu araştırma gidin.

2. Sertifika uyarısı aracılığıyla'ı tıklatın.
3. Bir yanıtı alırsanız, VPN ağ geçidi sağlıklı olarak kabul edilir. Bir yanıt almazsanız, ağ geçidi sağlıklı olmayabilir veya ağ geçidi alt ağı üzerinde bir NSG sorunu neden oluyor. Aşağıdaki örnek yanıt metindir:

    &lt;? xml version = "1.0"? > <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">birincil örneği: GatewayTenantWorker_IN_1 GatewayTenantVersion: 14.7.24.6 < / dize&gt;

### <a name="step-8-check-whether-the-on-premises-vpn-device-has-the-perfect-forward-secrecy-feature-enabled"></a>8. adım. Şirket içi VPN cihazı kusursuz iletme gizliliği özelliğinin etkin olup olmadığını denetleyin

Kusursuz iletme gizliliği özelliği, bağlantı kesme sorunlara neden olabilir. VPN cihazı etkin kusursuz iletme gizliliği varsa, özelliği devre dışı bırakın. Ardından VPN ağ geçidi IPSec ilkesi güncelleştirin.

## <a name="next-steps"></a>Sonraki adımlar

-   [Bir sanal ağ için siteden siteye bağlantı yapılandırma](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
-   [Siteden siteye VPN bağlantıları için bir IPSec/IKE ilkesi yapılandırma](vpn-gateway-ipsecikepolicy-rm-powershell.md)
