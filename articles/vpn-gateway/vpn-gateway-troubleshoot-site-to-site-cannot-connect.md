---
title: Bağlantı kurulamıyor Azure bir siteden siteye VPN bağlantı sorunlarını giderme | Microsoft Docs
description: Aniden çalışmayı durduruyor ve bağlanılamaz bir siteden siteye VPN bağlantı sorunlarını giderme hakkında bilgi edinin.
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
ms.openlocfilehash: 01729971169011002fa4231f043f82f105f81cdc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60458193"
---
# <a name="troubleshooting-an-azure-site-to-site-vpn-connection-cannot-connect-and-stops-working"></a>Sorun giderme: Bir Azure siteden siteye VPN bağlantısı bağlanamıyor ve bu çalışmayı durduruyor

Bir şirket içi ağ ve bir Azure sanal ağ arasında siteden siteye VPN bağlantısı yapılandırdıktan sonra VPN bağlantısı aniden çalışmayı durduruyor ve bağlanılamaz. Bu makalede, bu sorunu gidermenize yardımcı olmak için sorun giderme adımları sağlar. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-steps"></a>Sorun giderme adımları

Sorunu gidermek için önce dener [Azure VPN Gateway'i sıfırlama](vpn-gateway-resetgw-classic.md) ve şirket içi VPN cihaz tünel sıfırlayın. Sorun devam ederse, sorunun nedenini belirlemek için şu adımları izleyin.

### <a name="prerequisite-step"></a>Önkoşul adım

Azure VPN ağ geçidi türünü kontrol edin.

1. [Azure Portal](https://portal.azure.com) gidin.

2. Denetleme **genel bakış** VPN ağ geçidi için tür bilgilerini sayfası.
    
    ![Ağ Geçidi'ne genel bakış](media/vpn-gateway-troubleshoot-site-to-site-cannot-connect/gatewayoverview.png)

### <a name="step-1-check-whether-the-on-premises-vpn-device-is-validated"></a>1. Adım Şirket içi VPN cihazının doğrulanıp doğrulanmadığını denetleyin

1. Kullanmakta olduğunuz olup olmadığını denetleyin. bir [VPN cihazı ve işletim sistemi sürümü doğrulandı](vpn-gateway-about-vpn-devices.md#devicetable). Cihaz doğrulanmış VPN cihazı değilse, bir uyumluluk sorunu olup olmadığını görmek için cihaz üreticinize başvurun gerekebilir.

2. VPN cihazının düzgün yapılandırıldığından emin olun. Daha fazla bilgi için [cihaz yapılandırma örneklerini düzenleme](vpn-gateway-about-vpn-devices.md#editing).

### <a name="step-2-verify-the-shared-key"></a>2. Adım Paylaşılan anahtar doğrulama

Azure sanal ağ anahtarların eşleştiğinden emin olmak için VPN için şirket içi VPN cihazının paylaşılan anahtar ile karşılaştırın. 

Azure VPN bağlantısı paylaşılan anahtarı görüntülemek için aşağıdaki yöntemlerden birini kullanın:

**Azure portal**

1. Oluşturduğunuz VPN gateway siteden siteye bağlantıya gidin.

2. İçinde **ayarları** bölümünde **paylaşılan anahtar**.
    
    ![Paylaşılan anahtar](media/vpn-gateway-troubleshoot-site-to-site-cannot-connect/sharedkey.png)

**Azure PowerShell**

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure Resource Manager dağıtım modeli için:

    Get-AzVirtualNetworkGatewayConnectionSharedKey -Name <Connection name> -ResourceGroupName <Resource group name>

Klasik dağıtım modeli için:

    Get-AzureVNetGatewayKey -VNetName -LocalNetworkSiteName

### <a name="step-3-verify-the-vpn-peer-ips"></a>3. Adım VPN eş IP'ler doğrulayın

-   IP tanımında **yerel ağ geçidi** nesne azure'da, şirket içi cihaz IP eşleşmelidir.
-   Şirket içi cihazda ayarlı olan Azure ağ geçidi IP tanımı Azure ağ geçidi IP eşleşmesi gerekir.

### <a name="step-4-check-udr-and-nsgs-on-the-gateway-subnet"></a>4. Adım. UDR ve Nsg'ler ağ geçidi alt ağda kontrol edin.

Denetleyin ve ağ geçidi alt ağı üzerinde kullanıcı tanımlı yönlendirme (UDR) veya ağ güvenlik grupları (Nsg'ler) kaldırın ve ardından test sonucu. Bu sorun çözülene, UDR veya NSG uygulanan ayarları doğrulayın.

### <a name="step-5-check-the-on-premises-vpn-device-external-interface-address"></a>5. Adım. Şirket içi VPN cihazı dış arabirimi adresi denetleyin

- VPN cihazının Internet'e yönelik IP adresi de dahil edilmişse **yerel ağ** tanımı Azure'da, ara sıra bağlantı kesilmesi karşılaşabilirsiniz.
- Cihazın dış arabirimi doğrudan Internet'te olması gerekir. Herhangi bir ağ adresi çevirisi veya Internet ve cihaz arasında güvenlik duvarı olmamalıdır.
- Hızlı bir şekilde bir sanal IP Güvenlik Duvarı'nı yapılandırmak için küme Kes ve VPN gerecine doğrudan bir ağ geçidi ile arabirim oluşturmasını genel arabirimi kullanıma gerekir.

### <a name="step-6-verify-that-the-subnets-match-exactly-azure-policy-based-gateways"></a>6. Adım. Alt ağlar tam olarak eşleştiğinden emin olun (Azure ilke tabanlı ağ geçitleri)

-   Sanal ağ adres alanları Azure sanal ağı ve şirket içi tanımları arasında tam olarak eşleştiğinden emin olun.
-   Alt ağlar arasında tam olarak eşleştiğinden emin olun **yerel ağ geçidi** ve şirket içi şirket içi ağ için tanımlar.

### <a name="step-7-verify-the-azure-gateway-health-probe"></a>7. Adım. Azure ağ geçidi sistem durumu araştırması doğrulayın

1. Aşağıdaki URL'ye atarak açık sistem durumu araştırması:

    `https://<YourVirtualNetworkGatewayIP>:8081/healthprobe`

2. Sertifika uyarısı aracılığıyla tıklayın.
3. Bir yanıtı alırsanız, VPN ağ geçidi sağlıklı olarak kabul edilir. Bir yanıt almazsanız, ağ geçidi iyi durumda olmayabilir veya ağ geçidi alt ağı üzerinde bir NSG soruna neden olan. Örnek yanıt aşağıda gösterilmiştir:

    &lt;? xml version = "1.0"? > <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">birincil örneği: GatewayTenantWorker_IN_1 GatewayTenantVersion: 14.7.24.6 < / string&gt;

### <a name="step-8-check-whether-the-on-premises-vpn-device-has-the-perfect-forward-secrecy-feature-enabled"></a>8. Adım Şirket içi VPN cihazının kusursuz iletme gizliliği özelliğinin etkin olup olmadığını denetleyin

Kusursuz iletme gizliliği özelliği bağlantı kesilmesi sorunlara neden olabilir. VPN cihazı etkin kusursuz iletme gizliliği varsa, özelliği devre dışı bırakın. Ardından, VPN ağ geçidi IPSec ilkesi güncelleştirin.

## <a name="next-steps"></a>Sonraki adımlar

-   [Bir sanal ağ için siteden siteye bağlantı yapılandırma](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
-   [Siteden siteye VPN bağlantıları için bir IPSec/IKE ilkesi yapılandırma](vpn-gateway-ipsecikepolicy-rm-powershell.md)
