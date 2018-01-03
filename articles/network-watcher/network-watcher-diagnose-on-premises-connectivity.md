---
title: "Azure Ağ İzleyicisi ile VPN ağ geçidi aracılığıyla Tanıla şirket içi bağlantı | Microsoft Docs"
description: "Bu makalede, Azure Ağ İzleyicisi kaynak sorun giderme VPN ağ geçidi aracılığıyla şirket içi bağlantı tanılamak açıklar."
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: aeffbf3d-fd19-4d61-831d-a7114f7534f9
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 94658dfcf93e821e24cabb1f010f8dce0c014700
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="diagnose-on-premises-connectivity-via-vpn-gateways"></a>VPN ağ geçitleri aracılığıyla şirket içi bağlantı tanılama

Azure VPN ağ geçidi, şirket içi ağınız ve Azure sanal ağınız arasında güvenli bir bağlantı gereksinimini adres karma çözüm oluşturmanıza olanak sağlar. Gereksinimlerinizi benzersiz olduğundan, bu nedenle şirket içi VPN aygıtının seçimdir. Azure şu anda destekler [birkaç VPN aygıtları](../vpn-gateway/vpn-gateway-about-vpn-devices.md#devicetable) , sürekli doğrulanma cihaz satıcılarıyla yöneticileriyle. Şirket içi VPN Cihazınızı yapılandırmadan önce aygıta özgü yapılandırma ayarlarını gözden geçirin. Azure VPN ağ geçidi ile bir dizi benzer şekilde, yapılandırılmış [IPSec parametreleri desteklenen](../vpn-gateway/vpn-gateway-about-vpn-devices.md#ipsec) bağlantıları kurmak için kullanılır. Şu anda belirtin veya Azure VPN ağ geçidi'nden IPSec parametreleri belirli bir bileşimini seçmek için bir yolu yoktur. Şirket içi ve Azure arasında başarılı bir bağlantı kurmak için şirket içi VPN cihaz ayarları Azure VPN ağ geçidi tarafından belirlenen IPSec parametreleri uygun olması gerekir. Ayarlar doğru var olması durumunda bağlantı kaybı ve şimdiye kadar bu sorunları gidermek Önemsiz değildi ve saatleri belirlemek ve sorunu düzeltmek için genellikle sürdü.

Özelliği Azure Ağ İzleyicisi ile ilgili sorunları giderme, ağ geçidiniz ve bağlantıları ile ilgili tüm sorunları tanılamak ve dakika içinde bu sorunu gidermek için bilinçli karar için yeterli bilgiye sahip.

## <a name="scenario"></a>Senaryo

Azure ile şirket içi arasında bir siteden siteye bağlantı yapılandırmak istediğiniz FortiGate şirket içi VPN ağ geçidi olarak kullanma. Bu senaryo elde etmek için aşağıdaki Kurulum gerekir:

1. Sanal ağ geçidi - Azure VPN ağ geçidi
1. Yerel ağ geçidi - [(FortiGate) VPN ağ geçidi şirket içi](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md#LocalNetworkGateway) Azure bulutta gösterimi
1. Siteden siteye bağlantı (rota tabanlı) - [VPN ağ geçidi ve şirket içi yönlendiricileri arasındaki bağlantı](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal#createconnection)
1. [FortiGate yapılandırma](https://github.com/Azure/Azure-vpn-config-samples/blob/master/Fortinet/Current/Site-to-Site_VPN_using_FortiGate.md)

Bir siteden siteye yapılandırma hakkında ayrıntılı adım adım kılavuz ziyaret ederek bulunabilir: [bir siteden siteye bağlantı Azure portalını kullanarak VNet oluşturma](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).

Önemli yapılandırma adımlarının birini IPSec iletişimi parametrelerini yapılandırma, şirket içi ağınız ve Azure arasında bağlantı kaybı için tüm yetersizliğini yol açar. Şu anda Azure VPN ağ geçitleri, 1. aşaması için aşağıdaki IPSec parametreleri desteklemek üzere yapılandırılmış. , Bu ayarlar değiştirilemez daha önce belirtildiği gibi unutmayın.  Aşağıdaki tabloda görüldüğü gibi Azure VPN ağ geçidi tarafından desteklenen şifreleme algoritmaları AES256, AES128 ve 3DES ' dir.

### <a name="ike-phase-1-setup"></a>IKE Aşama 1 Kurulumu

| **Özellik** | **PolicyBased** | **RouteBased ve standart veya yüksek performanslı VPN ağ geçidi** |
| --- | --- | --- |
| IKE Sürümü |IKEv1 |IKEv2 |
| Diffie-Hellman Grubu |Grup 2 (1024 bit) |Grup 2 (1024 bit) |
| Kimlik Doğrulama Yöntemi |Önceden Paylaşılan Anahtar |Önceden Paylaşılan Anahtar |
| Şifreleme Algoritmaları |AES256 AES128 3DES |AES256 3DES |
| Karma Algoritma |SHA1(SHA128) |SHA1(SHA128), SHA2(SHA256) |
| Aşama 1 Güvenlik İlişkisi (SA) Yaşam Süresi (Zaman) |28.800 saniye |10.800 saniye |

Bir kullanıcı olarak, FortiGate yapılandırmak için gerekli olacak, örnek bir yapılandırma bulunabilir [GitHub](https://github.com/Azure/Azure-vpn-config-samples/blob/master/Fortinet/Current/fortigate_show%20full-configuration.txt). Bilmeden, FortiGate SHA-512 karma algoritması olarak kullanmak üzere yapılandırılmış. Bu algoritma ilke tabanlı bağlantılar için desteklenen bir algoritma olmadığından, VPN bağlantınızı çalışır.

Bu sorunları gidermek sabit ve nedenlerini genellikle sezgisel olmayan. Bu durumda, sorun giderme konusunda yardım almak için bir destek bileti açabilirsiniz. Ancak API Azure Ağ İzleyicisi ile ilgili sorunları giderme, bu sorunları kendi tanımlayabilirsiniz.

## <a name="troubleshooting-using-azure-network-watcher"></a>Azure Ağ İzleyicisi'ni kullanarak sorun giderme

Bağlantınızı tanılamak için Azure PowerShell'i bağlayın ve başlatma `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet'i. Bu cmdlet, kullanımıyla ilgili ayrıntılar bulabilirsiniz [sorun giderme sanal ağ geçidi ve bağlantıları - PowerShell](network-watcher-troubleshoot-manage-powershell.md). Bu cmdlet tamamlanması birkaç dakika sürebilir.

Cmdlet tamamlandıktan sonra sorun ve Günlükler hakkında ayrıntılı bilgi almak cmdlet'ini belirtilen depolama konumuna gidebilirsiniz. Azure Ağ İzleyicisi aşağıdaki günlük dosyalarını içeren bir ZIP klasörü oluşturur:

![1][1]

IKEErrors.txt adlı dosyayı açın ve şirket içi ile ilgili bir sorunu belirten aşağıdaki hata görüntüler IKE ayarı yanlış yapılandırma.

```
Error: On-premises device rejected Quick Mode settings. Check values.
     based on log : Peer sent NO_PROPOSAL_CHOSEN notify
```

Bu durumda, olduğunu ilgili olarak hata hakkındaki Scrubbed wfpdiag.txt nden ayrıntılı bilgiler edinebilirsiniz `ERROR_IPSEC_IKE_POLICY_MATCH` düzgün çalışmıyor bağlantı sağlama.

Başka bir ortak yetersizliğini belirten yanlış paylaşılan anahtarlar ' dir. Önceki örnekte paylaşılan anahtarları farklı belirttiyseniz, IKEErrors.txt şu hatayı gösterir: `Error: Authentication failed. Check shared key`.

Azure Ağ İzleyicisi sorun giderme özelliği, tanılama ve VPN ağ geçidi ve bağlantı basit bir PowerShell cmdlet kolaylığı sorun giderme olanak tanır. Şu anda biz aşağıdaki koşullar tanılama destekler ve birden çok koşul eklemeden doğrultusunda çalışıyoruz.

### <a name="gateway"></a>Ağ geçidi

| Hata türü | Neden | Günlük|
|---|---|---|
| NoFault | Herhangi bir hata algılandığında. |Evet|
| GatewayNotFound | Ağ geçidi veya ağ geçidi sağlanmamış bulunamıyor. |Hayır|
| PlannedMaintenance |  Ağ geçidi örneği bakım yapılıyor.  |Hayır|
| UserDrivenUpdate | Ne zaman bir kullanıcı güncelleştirme devam ediyor. Bu, yeniden boyutlandırma işlemi olabilir. | Hayır |
| VipUnResponsive | Ağ geçidi birincil örneğini ulaşamıyor. Sistem durumu araştırması başarısız olduğunda meydana gelir. | Hayır |
| PlatformInActive | Platform ile ilgili bir sorun yoktur. | Hayır|
| ServiceNotRunning | Temel alınan hizmet çalışmıyor. | Hayır|
| NoConnectionsFoundForGateway | Ağ geçidi bağlantı var. Bu yalnızca bir uyarıdır.| Hayır|
| ConnectionsNotConnected | Bağlantıları hiçbiri bağlanır. Bu yalnızca bir uyarıdır.| Evet|
| GatewayCPUUsageExceeded | Geçerli ağ geçidi CPU kullanımı % > 95 kullanımdır. | Evet |

### <a name="connection"></a>Bağlantı

| Hata türü | Neden | Günlük|
|---|---|---|
| NoFault | Herhangi bir hata algılandığında. |Evet|
| GatewayNotFound | Ağ geçidi veya ağ geçidi sağlanmamış bulunamıyor. |Hayır|
| PlannedMaintenance | Ağ geçidi örneği bakım yapılıyor.  |Hayır|
| UserDrivenUpdate | Ne zaman bir kullanıcı güncelleştirme devam ediyor. Bu, yeniden boyutlandırma işlemi olabilir.  | Hayır |
| VipUnResponsive | Ağ geçidi birincil örneğini ulaşamıyor. Sistem durumu araştırması başarısız olduğunda gerçekleşir. | Hayır |
| ConnectionEntityNotFound | Bağlantı yapılandırması eksik. | Hayır |
| ConnectionIsMarkedDisconnected | Bağlantı "bağlantısız" olarak işaretlenmiş |Hayır|
| ConnectionNotConfiguredOnGateway | Temel alınan hizmet yapılandırılmış bağlantısı yok. | Evet |
| ConnectionMarkedStandy | Temel alınan hizmet yedek olarak işaretlenir.| Evet|
| Kimlik Doğrulaması | Önceden paylaşılmış anahtar uyuşmuyor. | Evet|
| PeerReachability | Eş Ağ Geçidi ulaşılabilir değil. | Evet|
| IkePolicyMismatch | Eş Ağ geçidi, Azure tarafından desteklenmez IKE ilkeleri vardır. | Evet|
| WfpParse hata | WFP günlük ayrıştırılırken bir hata oluştu. |Evet|

## <a name="next-steps"></a>Sonraki adımlar

VPN ağ geçidi bağlantısı, PowerShell ve Azure Otomasyonu ziyaret ederek denetleyin öğrenin [Azure Ağ İzleyicisi sorun giderme İzleyici VPN ağ geçitleri](network-watcher-monitor-with-azure-automation.md)

[1]: ./media/network-watcher-diagnose-on-premises-connectivity/figure1.png
