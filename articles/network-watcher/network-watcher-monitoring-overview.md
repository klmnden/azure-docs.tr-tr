---
title: Azure Ağ İzleyicisi giriş | Microsoft Docs
description: Bu sayfayı izlemek için Ağ İzleyicisi hizmeti genel bir bakış sağlar ve Azure kaynaklarında görselleştirme ağ bağlı
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.assetid: 14bc2266-99e3-42a2-8d19-bd7257fec35e
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/11/2017
ms.author: jdial
ms.openlocfilehash: 792b96e4f5ba5dc0f2f943f099a2fee339407d66
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="azure-network-monitoring-overview"></a>Azure ağı izlemeye genel bakış

Müşterilerin bir uçtan uca ağ Azure VNet, ExpressRoute, uygulama ağ geçidi, yük Dengeleyiciler ve daha fazlasını gibi çeşitli tek tek ağ kaynaklarına oluşturma ve yönetme tarafından oluşturun. İzleme kaynakların her biri ağ üzerinde kullanılabilir. Bu kaynak düzey olarak izleme için bakın.

Uçtan uca ağ karmaşık yapılandırmalar ve senaryo tabanlı ağ izlemekte gereken karmaşık senaryolar oluşturma kaynakları arasındaki etkileşimler sahip olabilir.

Bu makalede senaryo ve kaynak düzeyi izleme kapsar. Azure'da ağ izleme kapsamlı ve iki geniş kategorisi kapsar:

* [**Ağ İzleyicisi** ](#network-watcher) -senaryo tabanlı izleme Ağ İzleyicisi'deki özelliklerle sağlanır. Bu hizmet içeren paket yakalama, sonraki atlama IP akış, güvenlik grubu görünümü, NSG akış günlükleri doğrulayın. Senaryo düzeyi izleme kaynak tek tek ağ izleme aksine ağ kaynaklarına bir uçtan uca görünümünü sağlar.
* [**Kaynak İzleme** ](#network-resource-level-monitoring) -kaynak düzeyi izleme dört özellikleri, tanılama günlükleri, ölçümleri, sorun giderme ve kaynak durumu oluşur. Bu özelliklerin tümü ağ kaynak düzeyinde oluşturulur.

## <a name="network-watcher"></a>Ağ İzleyicisi

Ağ İzleyicisi İzleme ve koşullar bir ağ düzeyinde senaryo içinde gelen ve giden Azure tanılama sağlayan bölgesel bir hizmettir. Ağ Tanılama ve görselleştirme Ağ İzleyicisi ile kullanılabilen araçlar anlamak, tanılama ve Azure ağınızdaki serisidir yardımcı olur.

Ağ İzleyicisi'ni şu anda aşağıdaki özellikleri içerir:

* **[Topoloji](network-watcher-topology-overview.md)**  -çeşitli bağlantılar ve ağ kaynakları bir kaynak grubunda arasındaki ilişkileri gösteren bir ağ düzey bir görünümünü sağlar.
* **[Değişken paket yakalama](network-watcher-packet-capture-overview.md)**  -bir sanal makine ve paket verilerini yakalar. Gelişmiş filtreleme seçenekleri ve süresini ayarlamak ve sınırlamaları boyutu yapamamasına gibi ince ayar denetimleri yönlülük sağlar. Paket verileri blob Mağazası'nda veya .cap biçiminde yerel diskte depolanabilir.
* **[IP akış doğrulayın](network-watcher-ip-flow-verify-overview.md)**  -paket izin verilen veya reddedilen denetimleri temel akış bilgi 5-tanımlama grubu paket parametrelerine (hedef IP, kaynak IP, hedef bağlantı noktası, kaynak bağlantı noktası ve protokol). Paketi bir güvenlik grubu tarafından reddedilirse kural ve Paket reddedildi grubun döndürülür.
* **[Sonraki atlama](network-watcher-next-hop-overview.md)**  -kullanıcı tanımlı yollar herhangi tanılamak olanak sağlayarak Azure ağ yapıda yönlendirilen paketleri yanlış için sonraki atlama belirler.
* **[Güvenlik grubu görünümü](network-watcher-security-group-view-overview.md)**  -bir VM üzerinde uygulanan etkili ve uygulanan güvenlik kuralları alır.
* **[NSG akış günlük](network-watcher-nsg-flow-logging-overview.md)**  -akış günlükleri ağ güvenlik grupları için izin verilen ya da grubu güvenlik kuralları tarafından reddedilen trafiği ilgili günlükleri yakalamanıza olanak sağlar. Akış bir 5-tanımlama grubu bilgileriyle – kaynak IP, hedef IP, kaynak bağlantı noktası, hedef bağlantı noktası ve protokol tanımlanır.
* **[Sanal ağ geçidi ve bağlantı sorunlarını giderme](network-watcher-troubleshoot-manage-rest.md)**  -sanal ağ geçitleri ve bağlantıları sorun giderme olanağı sağlar.
* **[Ağ abonelik sınırları](#network-subscription-limits)**  -ağ kaynak kullanımı sınırları karşı görüntülemenizi sağlar.
* **[Tanılama günlük yapılandırma](#diagnostic-logs)**  – etkinleştirme veya devre dışı bir kaynak grubunda ağ kaynakları için tanılama günlükleri tek bir bölme sağlar.
* **[Bağlantı sorunlarını giderme](network-watcher-connectivity-overview.md)**  -doğrudan TCP bağlantısı bir sanal makineden Azure bağlamla zenginleştirilmiş belirli bir uç nokta oluşturma olasılığını doğrular.
* **[Bağlantı İzleyici](connection-monitor.md)**  -bir Azure sanal makinesi ve kaynak ve hedef IP adresi ve bağlantı noktasını kullanarak bir IP adresi arasındaki gecikme süresi ve yapılandırma sorunlarını izleyin.

### <a name="role-based-access-control-rbac-in-network-watcher"></a>Rol tabanlı erişim denetimi (RBAC) Ağ İzleyicisi

Ağ İzleyicisi'ni kullanan [Azure rol tabanlı erişim denetimi (RBAC) modeli](../active-directory/role-based-access-control-what-is.md). Ağ İzleyicisi tarafından aşağıdaki izinler gerekir. Ağ İzleyicisi API'leri başlatılıyor veya portaldan Ağ İzleyicisi'ni kullanmak için kullanılan rol gerekli erişimi olduğundan emin olmak önemlidir.

|Kaynak| İzin|
|---|---| 
|Microsoft.Storage/ |Okuma|
|Microsoft.Authorization/| Okuma| 
|Microsoft.Resources/subscriptions/resourceGroups/| Okuma|
|Microsoft.Storage/storageAccounts/listServiceSas/ | Eylem|
|Microsoft.Storage/storageAccounts/listAccountSas/ |Eylem|
|Microsoft.Storage/storageAccounts/listKeys/ | Eylem|
|Microsoft.Compute/virtualMachines/ |Okuma|
|Microsoft.Compute/virtualMachines/ |Yazma|
|Microsoft.Compute/virtualMachineScaleSets/ |Okuma|
|Microsoft.Compute/virtualMachineScaleSets/ |Yazma|
|Microsoft.Network/networkWatchers/packetCaptures/ |Okuma|
|Microsoft.Network/networkWatchers/packetCaptures/| Yazma|
|Microsoft.Network/networkWatchers/packetCaptures/| Sil|
|Microsoft.Network/networkWatchers/ |Yazma |
|Microsoft.Network/networkWatchers/| Okuma |
|Microsoft.Insights/alertRules/ |*|
|Microsoft.Support/ | *|

### <a name="network-subscription-limits"></a>Ağ abonelik sınırları

Ağ abonelik sınırları her bir abonelikte kullanılabilir kaynakları sayısı karşı bir bölgede ağ kaynağı kullanımını ayrıntılarını sağlayın.

![Ağ abonelik sınırı][nsl]

## <a name="network-resource-level-monitoring"></a>Ağ kaynak düzeyi izleme

Aşağıdaki özellikler, kaynak düzeyi izleme için kullanılabilir:

### <a name="audit-log"></a>Denetleme günlüğü

Ağ yapılandırmasının bir parçası gerçekleştirilen işlemleri günlüğe kaydedilir. Bu günlükler Azure Portalı'nda görüntülenebilir veya Power BI gibi Microsoft araçları veya üçüncü taraf araçlarını kullanarak alınamıyor. Denetim günlükleri, portal, PowerShell'i, CLI ve Rest API kullanılabilir. Denetim günlükleri hakkında daha fazla bilgi için bkz: [denetim işlemleri Resource Manager ile](../resource-group-audit.md)

Denetim günlükleri, tüm ağ kaynaklarına yapılan işlemleri için kullanılabilir.

### <a name="metrics"></a>Ölçümler

Performans ölçümleri ve bir süre boyunca toplanan sayaçları ölçümleridir. Ölçümleri uygulama ağ geçidi için şu anda kullanılabilir. Ölçümleri eşiğine dayalı uyarılar tetiklemek için kullanılabilir. Bkz: [uygulama ağ geçidi tanılama](../application-gateway/application-gateway-diagnostics.md) ölçümleri uyarıları oluşturmak için nasıl kullanılabileceğini görüntülemek için.

![Ölçümleri görüntüleyin][metrics]

### <a name="diagnostic-logs"></a>Tanılama günlükleri

Dönemsel ve spontaneous olayları ağ kaynakları tarafından oluşturulan ve bir olay hub'ı veya günlük analizi için gönderilen depolama hesaplarındaki günlüğe. Bu günlükleri bir kaynak sistem durumu fikir sağlar. Bu günlükler Power BI ve günlük analizi gibi araçları görüntülenebilir. Tanılama günlükleri görüntüleme konusunda bilgi için [günlük analizi](../log-analytics/log-analytics-azure-networking-analytics.md).

Tanılama günlükleri için kullanılabilir [yük dengeleyici](../load-balancer/load-balancer-monitor-log.md), [ağ güvenlik grupları](../virtual-network/virtual-network-nsg-manage-log.md), yollar ve [uygulama ağ geçidi](../application-gateway/application-gateway-diagnostics.md).

Ağ İzleyicisi görünümü bir tanılama günlükleri sağlar. Bu görünüm, tanılama günlüğünün destekleyen tüm ağ kaynaklarını içerir. Bu görünümden etkinleştirin ve ağ kaynaklarını kolayca ve hızlı bir şekilde devre dışı bırakın.

![günlükler][logs]

### <a name="troubleshooting"></a>Sorun giderme

Sorun giderme dikey penceresinde bir deneyim Portalı'nda, tek bir kaynakla ilgili genel sorunları tanılamak için bugün ağ kaynaklarında sağlanır. Bu deneyim, aşağıdaki ağ kaynaklarına yönelik - ExpressRoute, VPN ağ geçidi, uygulama ağ geçidi, ağ güvenlik günlükleri, yollar, DNS, yük dengeleyici ve trafik Yöneticisi kullanılabilir. Kaynak düzey sorun giderme hakkında daha fazla bilgi için [Tanıla ve çözümleme sorunlarını Azure sorun giderme](https://azure.microsoft.com/blog/azure-troubleshoot-diagonse-resolve-issues/)

![sorun giderme bilgileri][TS]

### <a name="resource-health"></a>Kaynak durumu

Bir ağ kaynağına durumunu düzenli olarak sağlanır. Bu tür kaynaklar VPN ağ geçidi ve VPN tüneli içerir. Kaynak durumu Azure portalında erişilemez. Kaynak durumu hakkında daha fazla bilgi için [kaynak sistem durumu genel bakış](../resource-health/resource-health-overview.md)

## <a name="next-steps"></a>Sonraki adımlar

Ağ İzleyicisi hakkında daha fazla bilgi sonra için bilgi alabilirsiniz:

Paket yakalama VM üzerinde ziyaret ederek yapmak [Azure portalında değişken paket yakalama](network-watcher-packet-capture-manage-portal.md)

Öngörülü izleme ve Tanılama'yı kullanarak gerçekleştirmek [uyarının paket yakalama](network-watcher-alert-triggered-packet-capture.md).

Güvenlik açıklarıyla algılamak [Wireshark paket yakalamayla çözümleme](network-watcher-deep-packet-inspection.md), açık kaynaklı araçları kullanarak.

Azure'un diğer önemli [ağ özelliklerinden](../networking/networking-overview.md) bazıları hakkında bilgi edinin.

<!--Image references-->
[TS]: ./media/network-watcher-monitoring-overview/troubleshooting.png
[logs]: ./media/network-watcher-monitoring-overview/logs.png
[metrics]: ./media/network-watcher-monitoring-overview/metrics.png
[nsl]: ./media/network-watcher-monitoring-overview/nsl.png











