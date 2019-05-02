---
title: Azure Ağ İzleyicisi ile VPN ağ geçidi üzerinden Tanıla şirket içi bağlantı | Microsoft Docs
description: Bu makalede, Azure Ağ İzleyicisi sorun giderme kaynak ile VPN gateway aracılığıyla şirket içi bağlantıyı tanılama açıklar.
services: network-watcher
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
ms.assetid: aeffbf3d-fd19-4d61-831d-a7114f7534f9
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: kumud
ms.openlocfilehash: 95c6e1f015e519bd1e753fce9a2c6f064a854456
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64713774"
---
# <a name="diagnose-on-premises-connectivity-via-vpn-gateways"></a>VPN ağ geçitleri üzerinden şirket içi bağlantıyı tanılama

Azure VPN ağ geçidi, şirket içi ağınız ile Azure sanal ağınız arasında güvenli bir bağlantı için gereken adres karma çözüm oluşturmak amacıyla sağlar. Gereksinimlerinizi benzersiz olduğundan, bu nedenle şirket içi VPN cihazının seçenektir. Azure şu anda destekler [birkaç VPN cihazları](../vpn-gateway/vpn-gateway-about-vpn-devices.md#devicetable) , sürekli doğrulanır cihaz satıcılarıyla iş ortaklığı. Cihaza özgü yapılandırma ayarlarını, şirket içi VPN Cihazınızı yapılandırmadan önce gözden geçirin. Benzer şekilde, Azure VPN ağ geçidi kümesi ile yapılandırılmış [IPSec parametreleri desteklenen](../vpn-gateway/vpn-gateway-about-vpn-devices.md#ipsec) bağlantıları kurmak için kullanılır. Şu anda Azure VPN gateway'den belirli bir birleşimi IPSec parametreleri bir yolu yoktur. Şirket içi ve Azure arasında başarılı bir bağlantı kurmak için şirket içi VPN cihaz ayarları, Azure VPN Gateway tarafından belirlenen IPSec parametreleri uygun olmalıdır. Ayarlar doğru var ise bağlantı kaybı ve şimdiye kadar bu sorunları gidermek Önemsiz değildi ve genellikle belirlemek ve sorunu gidermek için saat sürüyordu.

Azure Ağ İzleyicisi ile sorun giderme özelliği, ağ geçidi ve bağlantılarıyla sorunları tanılayın ve dakikalar içinde sorunu düzeltmek için bilinçli bir karar vermeniz için yeterli bilgiye sahip.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="scenario"></a>Senaryo

Azure ve şirket içi arasında siteden siteye bağlantı yapılandırma istediğiniz FortiGate şirket içi VPN ağ geçidi olarak kullanma. Bu senaryo elde etmek için aşağıdaki Kurulum gerekir:

1. Sanal ağ geçidi - azure'da VPN ağ geçidi
1. Yerel ağ geçidi - [şirket içi (FortiGate) VPN ağ geçidi](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md#LocalNetworkGateway) Azure bulutunda gösterimi
1. Siteden siteye bağlantı (rota tabanlı) - [VPN Gateway ve şirket içi yönlendiricileri arasındaki bağlantı](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal#createconnection)
1. [FortiGate yapılandırma](https://github.com/Azure/Azure-vpn-config-samples/blob/master/Fortinet/Current/Site-to-Site_VPN_using_FortiGate.md)

Siteden siteye bir yapılandırma için ayrıntılı adım adım rehberlik ederek bulunabilir: [Azure portalını kullanarak siteden siteye bağlantı ile VNet oluşturma](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).

Kritik yapılandırma adımlarının birini IPSec iletişimi parametreleri yapılandırma, şirket içi ağınız ve Azure arasında bağlantı kaybı için herhangi bir yanlış yapılandırma yol açar. Şu anda Azure VPN ağ geçitleri, 1. Aşama için aşağıdaki IPSec parametreleri desteklemek üzere yapılandırılır. Bu ayarlar değiştirilemez daha önce belirtildiği gibi unutmayın.  Aşağıdaki tabloda görebileceğiniz gibi Azure VPN ağ geçidi tarafından desteklenen şifreleme algoritmalarını AES256, AES128 ve 3DES ' dir.

### <a name="ike-phase-1-setup"></a>IKE Aşama 1 Kurulumu

| **Özellik** | **PolicyBased** | **RouteBased ve standart ya da yüksek performanslı VPN ağ geçidi** |
| --- | --- | --- |
| IKE Sürümü |IKEv1 |IKEv2 |
| Diffie-Hellman Grubu |Grup 2 (1024 bit) |Grup 2 (1024 bit) |
| Kimlik Doğrulama Yöntemi |Önceden Paylaşılan Anahtar |Önceden Paylaşılan Anahtar |
| Şifreleme Algoritmaları |AES256 AES128 3DES |AES256 3DES |
| Karma Algoritma |SHA1(SHA128) |SHA1(SHA128), SHA2(SHA256) |
| Aşama 1 Güvenlik İlişkisi (SA) Yaşam Süresi (Zaman) |28.800 saniye |10.800 saniye |

Bir kullanıcı olarak, FortiGate yapılandırmak için gerekli olacak, bir örnek yapılandırma bulunabilir [GitHub](https://github.com/Azure/Azure-vpn-config-samples/blob/master/Fortinet/Current/fortigate_show%20full-configuration.txt). Farkında olmadan, FortiGate karma algoritması SHA-512 kullanmak için yapılandırılmış. Bu algoritma, ilke tabanlı bağlantıları için desteklenen bir algoritması değil olarak VPN bağlantınızı çalışır.

Bu sorunları gidermek daha zordur ve kök nedenleri genellikle anlaşılamayacak. Bu durumda sorunu çözme konusunda yardım almak için bir destek bileti açabilirsiniz. Ancak Azure Ağ İzleyicisi ile API sorun giderme, bu sorunları kendiniz belirleyebilirsiniz.

## <a name="troubleshooting-using-azure-network-watcher"></a>Azure Ağ İzleyicisi'ni kullanarak sorun giderme

Bağlantınızı tanılamak için Azure PowerShell'i bağlayın ve başlatma `Start-AzNetworkWatcherResourceTroubleshooting` cmdlet'i. Bu cmdlet, kullanımıyla ilgili ayrıntılar bulabilirsiniz [sorun giderme sanal ağ geçidini ve bağlantıları - PowerShell](network-watcher-troubleshoot-manage-powershell.md). Bu cmdlet tamamlanması birkaç dakika sürebilir.

Cmdlet tamamlandıktan sonra cmdlet günlükleri ve sorun hakkında ayrıntılı bilgi almak belirtilen depolama konumuna gidebilirsiniz. Azure Ağ İzleyicisi aşağıdaki günlük dosyalarını içeren bir zip klasörünü oluşturur:

![1][1]

IKEErrors.txt adlı dosyayı açın ve şirket içi ile ilgili bir sorun gösteren aşağıdaki hatayı alırsanız görüntüler IKE ayarı yanlış yapılandırma.

```
Error: On-premises device rejected Quick Mode settings. Check values.
     based on log : Peer sent NO_PROPOSAL_CHOSEN notify
```

Bu durumda olduğunu bahsetmeleri gibi sayfasından Scrubbed wfpdiag.txt hata hakkında ayrıntılı bilgi edinebilirsiniz `ERROR_IPSEC_IKE_POLICY_MATCH` düzgün çalışmıyor bağlantı sağlama.

Başka bir yaygın hatalı yapılandırma belirten yanlış paylaşılan anahtarlar var. Yukarıdaki örnekte, farklı bir paylaşılan anahtarlar belirlemiş olsaydık IKEErrors.txt aşağıdaki hatayı gösterir: `Error: Authentication failed. Check shared key`.

Azure Ağ İzleyicisi sorun giderme tanılayın ve basit bir PowerShell cmdlet'i bir kolayca ile VPN Gateway ve bağlantı sorunlarını giderme özelliği sağlar. Şu anda biz aşağıdaki durumlarını tanılamada desteği ve daha fazla koşul ekleme doğrultusunda çalışıyoruz.

### <a name="gateway"></a>Ağ geçidi

| Hata türü | Neden | Günlük|
|---|---|---|
| NoFault | Herhangi bir hata algılandığında. |Evet|
| GatewayNotFound | Ağ geçidi veya ağ geçidi sağlanmamış bulunamıyor. |Hayır|
| PlannedMaintenance |  Ağ geçidinin bakım yapılıyor.  |Hayır|
| UserDrivenUpdate | Ne zaman bir kullanıcı güncelleştirme devam ediyor. Bu, yeniden boyutlandırma işlemi olabilir. | Hayır |
| VipUnResponsive | Birincil ağ geçidi örneğini erişemiyor. Durum araştırması başarısız olduğunda meydana gelir. | Hayır |
| PlatformInActive | Platform ile ilgili bir sorun yoktur. | Hayır|
| ServiceNotRunning | Temel alınan hizmet çalışmıyor. | Hayır|
| NoConnectionsFoundForGateway | Ağ geçidi üzerinde bağlantı var. Bu yalnızca bir uyarıdır.| Hayır|
| ConnectionsNotConnected | Bağlantıları hiçbiri bağlı. Bu yalnızca bir uyarıdır.| Evet|
| GatewayCPUUsageExceeded | Geçerli ağ geçidi CPU kullanımı > %95 kullanımdır. | Evet |

### <a name="connection"></a>Bağlantı

| Hata türü | Neden | Günlük|
|---|---|---|
| NoFault | Herhangi bir hata algılandığında. |Evet|
| GatewayNotFound | Ağ geçidi veya ağ geçidi sağlanmamış bulunamıyor. |Hayır|
| PlannedMaintenance | Ağ geçidinin bakım yapılıyor.  |Hayır|
| UserDrivenUpdate | Ne zaman bir kullanıcı güncelleştirme devam ediyor. Bu, yeniden boyutlandırma işlemi olabilir.  | Hayır |
| VipUnResponsive | Birincil ağ geçidi örneğini erişemiyor. Durum araştırması başarısız olduğunda ortaya çıkar. | Hayır |
| ConnectionEntityNotFound | Bağlantı yapılandırması eksik. | Hayır |
| ConnectionIsMarkedDisconnected | Bağlantı "bağlantısız" olarak işaretlendi |Hayır|
| ConnectionNotConfiguredOnGateway | Temel alınan hizmete yapılandırılmış bağlantı yok. | Evet |
| ConnectionMarkedStandby | Temel alınan hizmete yedek olarak işaretlenir.| Evet|
| Kimlik Doğrulaması | Önceden paylaşılan anahtarı uyuşmazlığı. | Evet|
| PeerReachability | Eş Ağ Geçidi erişilebilir değil. | Evet|
| IkePolicyMismatch | Eş Ağ geçidi, Azure tarafından desteklenen IKE ilkeleri vardır. | Evet|
| WfpParse Error | WFP günlük ayrıştırılırken bir hata oluştu. |Evet|

## <a name="next-steps"></a>Sonraki adımlar

PowerShell ve Azure Otomasyonu ile VPN ağ geçidi bağlantısı kontrol ederek öğrenin [İzleyicisi VPN ağ geçitleri, Azure Ağ İzleyicisi sorun giderme](network-watcher-monitor-with-azure-automation.md)

[1]: ./media/network-watcher-diagnose-on-premises-connectivity/figure1.png
