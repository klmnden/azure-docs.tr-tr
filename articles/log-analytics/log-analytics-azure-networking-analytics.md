---
title: Log analytics'te Azure ağ analizi çözümü | Microsoft Docs
description: Log Analytics'te Azure ağ analizi çözümü, Azure ağ güvenlik grubu günlükleri ve Azure Application Gateway günlüklerini gözden geçirmek için kullanabilirsiniz.
services: log-analytics
documentationcenter: ''
author: richrundmsft
manager: ewinner
editor: ''
ms.assetid: 66a3b8a1-6c55-4533-9538-cad60c18f28b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/21/2018
ms.author: richrund
ms.component: ''
ms.openlocfilehash: 7d93b8e37c2025ebe47f9351da26f0913107585d
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51009377"
---
# <a name="azure-networking-monitoring-solutions-in-log-analytics"></a>Azure ağ Log Analytics çözümleri izleme

Log Analytics ağlarınızı izlemek için aşağıdaki çözümleri sunar:
* Ağ Performansı İzleyicisi'ni (NPM)
 * Ağınızın durumunu izleyin
* Azure Application Gateway analytics gözden geçirmek için
 * Azure Application Gateway günlükleri
 * Azure Application Gateway ölçümleri
* Ağ bulut ağınızdaki etkinliği izlemek ve denetlemek için çözümler
* [Trafik analizi](https://docs.microsoft.com/azure/networking/network-monitoring-overview#traffic-analytics) 
* Azure Ağ Güvenlik Grubu Analizi

## <a name="network-performance-monitor-npm"></a>Ağ Performansı İzleyicisi'ni (NPM)

[Ağ Performansı İzleyicisi](https://docs.microsoft.com/azure/networking/network-monitoring-overview) yönetim çözümü olan bir ağ izleme sistem durumu, kullanılabilirliği ve ulaşılabilirliği ağların izleyen çözümü.  Arasındaki bağlantıyı izlemek için kullanılır:

* Genel Bulut ve şirket içi
* Veri merkezleri ve kullanıcı konumlarında (şubelere)
* Çok katmanlı bir uygulamanın çeşitli katmanları barındıran alt ağlar.

Daha fazla bilgi için [Ağ Performansı İzleyicisi](https://docs.microsoft.com/azure/networking/network-monitoring-overview).

## <a name="azure-application-gateway-and-network-security-group-analytics"></a>Azure Application Gateway ve ağ güvenlik grubu analizi
Çözümlerini kullanmak için:
1. Log Analytics için yönetim çözümünü ekleyin ve
2. Tanılama Log Analytics çalışma alanına yönlendirmek tanılamayı etkinleştirin. Günlükler Azure Blob depolama alanına yazmak gerekli değildir.

Tanılama ve karşılık gelen çözümü birini veya her ikisi de uygulama ağ geçidini ve ağ güvenlik grupları için etkinleştirebilirsiniz.

Belirli bir kaynak türü için tanılama günlük kaydını etkinleştirmeyin ancak çözümü yüklemek, o kaynak için Pano dikey pencereleri boş ve bir hata iletisi görüntüler.

> [!NOTE]
> Ocak 2017'de, uygulama ağ geçitleri ve ağ güvenlik grupları günlükleri Log Analytics'e gönderme desteklenen yönteminizi değiştirdik. Görürseniz **Azure ağ analizi (kullanım dışı)** çözümü başvurmak [eski ağ Analytics çözüm'den geçiş](#migrating-from-the-old-networking-analytics-solution) adımları izlemeniz gerekir.
>
>

## <a name="review-azure-networking-data-collection-details"></a>Azure ağ veri koleksiyonu ayrıntıları gözden geçirin
Azure Application Gateway analytics ve ağ güvenlik grubu analytics yönetim çözümleri doğrudan Azure uygulama ağ geçitleri ve ağ güvenlik grupları tanılama günlükleri toplayın. Günlükler Azure Blob depolama alanına yazmak gerekli değildir ve hiçbir aracısı için veri koleksiyonu gereklidir.

Veri toplama yöntemleri ve Azure Application Gateway analytics ve ağ güvenlik grubu analizi için verileri nasıl toplanır hakkında diğer ayrıntıları aşağıdaki tabloda gösterilmektedir.

| Platform | Doğrudan aracı | Systems Center Operations Manager Aracısı | Azure | Operations Manager gerekli? | Operations Manager aracısı veri yönetim grubu gönderilir. | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- |
| Azure |  |  |&#8226; |  |  |oturum açıldığında |


## <a name="azure-application-gateway-analytics-solution-in-log-analytics"></a>Log analytics'te Azure Application Gateway analytics çözümü

![Azure Application Gateway Analytics simgesi](media/log-analytics-azure-networking-analytics/azure-analytics-symbol.png)

Uygulama ağ geçitleri için aşağıdaki günlüklere desteklenir:

* ApplicationGatewayAccessLog
* ApplicationGatewayPerformanceLog
* ApplicationGatewayFirewallLog

Aşağıdaki ölçümler Application Gateway'ler için desteklenir: yeniden


* 5 dakikalık aktarım hızı

### <a name="install-and-configure-the-solution"></a>Yükleme ve çözüm yapılandırma
Yükleme ve Azure Application Gateway analytics çözümü yapılandırmak için aşağıdaki yönergeleri kullanın:

1. Azure Application Gateway analytics çözümü etkinleştirme [Azure Market](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureAppGatewayAnalyticsOMS?tab=Overview) veya açıklanan işlemi kullanarak [Log Analytics çözümleri ekleme çözüm Galerisi'ndeki](../monitoring/monitoring-solutions.md).
2. Etkinleştirmek için günlüğe kaydetme tanılama [Application Gateway'ler](../application-gateway/application-gateway-diagnostics.md) izlemek istediğiniz.

#### <a name="enable-azure-application-gateway-diagnostics-in-the-portal"></a>Portalda Azure Application Gateway tanılamayı etkinleştirme

1. Azure portalında, izlemek için Application Gateway kaynağına gidin
2. Seçin *tanılama günlükleri* aşağıdaki sayfasını açmak için

   ![Azure Application Gateway kaynak görüntüsü](media/log-analytics-azure-networking-analytics/log-analytics-appgateway-enable-diagnostics01.png)
3. Tıklayın *tanılamayı Aç* aşağıdaki sayfasını açmak için

   ![Azure Application Gateway kaynak görüntüsü](media/log-analytics-azure-networking-analytics/log-analytics-appgateway-enable-diagnostics02.png)
4. Tanılamayı etkinleştirmek için tıklayın *üzerinde* altında *durumu*
5. Onay kutusunu tıklatın *Log Analytics'e gönderme*
6. Mevcut bir Log Analytics çalışma alanını seçin veya bir çalışma alanı oluşturma
7. Altındaki onay kutusuna tıklayın **günlük** toplamak için günlük türlerinin her biri için
8. Tıklayın *Kaydet* Log Analytics için tanılama günlüğünü etkinleştirme

#### <a name="enable-azure-network-diagnostics-using-powershell"></a>PowerShell kullanarak Azure ağı tanılamayı etkinleştirme

Aşağıdaki PowerShell betiğini application gateway'ler için günlüğe kaydetme tanılama etkinleştirmek nasıl bir örnek sağlar.

```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$gateway = Get-AzureRmApplicationGateway -Name 'ContosoGateway'

Set-AzureRmDiagnosticSetting -ResourceId $gateway.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="use-azure-application-gateway-analytics"></a>Azure Application Gateway analytics kullanın
![Azure Application Gateway analytics kutucuğuna görüntüsü](media/log-analytics-azure-networking-analytics/log-analytics-appgateway-tile.png)

Tıkladıktan sonra **Azure Application Gateway analytics** kutucuğuna genel bakış'ta günlüklerinizi özetlerini görüntüleyin ve ardından aşağıdaki kategorilerde ayrıntıları için ayrıntılara girin:

* Uygulama ağ geçidi erişim günlükleri
  * Uygulama ağ geçidi günlüklerine erişim için istemci ve sunucu hataları
  * Her uygulama ağ geçidi için saat başına istek
  * Her uygulama ağ geçidi için saat başına istek başarısız oldu
  * Uygulama ağ geçitleri için Kullanıcı Aracısı hataları
* Application Gateway performansı
  * Application Gateway için konak sistem durumu
  * Application Gateway için en yüksek ve 95'lik dilim başarısız istekleri

![Azure Application Gateway analytics panosunun görüntüsü](media/log-analytics-azure-networking-analytics/log-analytics-appgateway01.png)

![Azure Application Gateway analytics panosunun görüntüsü](media/log-analytics-azure-networking-analytics/log-analytics-appgateway02.png)

Üzerinde **Azure Application Gateway analytics** Pano, dikey pencereleri biriyle özet bilgilerini gözden geçirin ve bir günlük arama sayfasında ayrıntılı bilgileri görüntülemek için tıklayın.

Herhangi bir günlük arama sayfası üzerinde sonuç zaman, ayrıntılı sonuçlarını ve günlük arama geçmişinizi görüntüleyebilirsiniz. Ayrıca, sonuçları daraltmak için modelleri göre filtreleyebilirsiniz.


## <a name="azure-network-security-group-analytics-solution-in-log-analytics"></a>Log analytics'te Azure ağ güvenlik grubu analizi çözümü

![Azure ağ güvenlik grubu analizi simgesi](media/log-analytics-azure-networking-analytics/azure-analytics-symbol.png)

> [!NOTE]
> İşlevselliğini almıştır beri topluluk desteği için ağ güvenlik grubu analizi çözümü taşınıyor [trafik analizi](../network-watcher/traffic-analytics.md).
> - Çözüm kullanıma sunulduğunu [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/oms-azurensg-solution/) ve yakında artık Azure Market'te kullanıma sunulacaktır.
> - Çözümü kendi çalışma alanına zaten eklenmiş mevcut müşteriler, değişikliği olmadan çalışmaya devam eder.
> - Microsoft, tanılama ayarları kullanarak çalışma alanınıza gönderen NSG tanılama günlüklerini desteklemeye devam edecektir.

Günlükleri, ağ güvenlik grupları için desteklenir:

* NetworkSecurityGroupEvent
* NetworkSecurityGroupRuleCounter

### <a name="install-and-configure-the-solution"></a>Yükleme ve çözüm yapılandırma
Yükleme ve Azure ağ analizi çözümü yapılandırmak için aşağıdaki yönergeleri kullanın:

1. Azure ağ güvenlik grubu analizi çözümü etkinleştirme [Azure Market](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureNSGAnalyticsOMS?tab=Overview) veya açıklanan işlemi kullanarak [Log Analytics çözümleri ekleme çözüm Galerisi'ndeki](../monitoring/monitoring-solutions.md).
2. Etkinleştirmek için günlüğe kaydetme tanılama [ağ güvenlik grubu](../virtual-network/virtual-network-nsg-manage-log.md) izlemek istediğiniz kaynakları.

### <a name="enable-azure-network-security-group-diagnostics-in-the-portal"></a>Portalda Azure ağ güvenlik grubu tanılamayı etkinleştirme

1. Azure portalında, izlemek için ağ güvenlik grubu kaynağına gidin
2. Seçin *tanılama günlükleri* aşağıdaki sayfasını açmak için

   ![Azure ağ güvenlik grubu kaynak görüntüsü](media/log-analytics-azure-networking-analytics/log-analytics-nsg-enable-diagnostics01.png)
3. Tıklayın *tanılamayı Aç* aşağıdaki sayfasını açmak için

   ![Azure ağ güvenlik grubu kaynak görüntüsü](media/log-analytics-azure-networking-analytics/log-analytics-nsg-enable-diagnostics02.png)
4. Tanılamayı etkinleştirmek için tıklayın *üzerinde* altında *durumu*
5. Onay kutusunu tıklatın *Log Analytics'e gönderme*
6. Mevcut bir Log Analytics çalışma alanını seçin veya bir çalışma alanı oluşturma
7. Altındaki onay kutusuna tıklayın **günlük** toplamak için günlük türlerinin her biri için
8. Tıklayın *Kaydet* Log Analytics için tanılama günlüğünü etkinleştirme

### <a name="enable-azure-network-diagnostics-using-powershell"></a>PowerShell kullanarak Azure ağı tanılamayı etkinleştirme

Aşağıdaki PowerShell betiğini ağ güvenlik grupları için günlüğe kaydetme tanılama etkinleştirmeye ilişkin bir örnek sağlar.
```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$nsg = Get-AzureRmNetworkSecurityGroup -Name 'ContosoNSG'

Set-AzureRmDiagnosticSetting -ResourceId $nsg.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="use-azure-network-security-group-analytics"></a>Kullanım Azure ağ güvenlik grubu analizi
Tıkladıktan sonra **Azure ağ güvenlik grubu analizi** kutucuğuna genel bakış'ta günlüklerinizi özetlerini görüntüleyin ve ardından aşağıdaki kategorilerde ayrıntıları için ayrıntılara girin:

* Ağ güvenlik grubu engellenen akışlar
  * Engellenen akışlar içeren ağ güvenlik grubu kuralları
  * Engellenen akışlar içeren MAC adresleri
* Ağ güvenlik grubu izin verilen akışlar
  * İzin verilen akışlar içeren ağ güvenlik grubu kuralları
  * İzin verilen akışlar içeren MAC adresleri

![Azure ağ güvenlik grubu analizi panosunun görüntüsü](media/log-analytics-azure-networking-analytics/log-analytics-nsg01.png)

![Azure ağ güvenlik grubu analizi panosunun görüntüsü](media/log-analytics-azure-networking-analytics/log-analytics-nsg02.png)

Üzerinde **Azure ağ güvenlik grubu analizi** Pano, dikey pencereleri biriyle özet bilgilerini gözden geçirin ve bir günlük arama sayfasında ayrıntılı bilgileri görüntülemek için tıklayın.

Herhangi bir günlük arama sayfası üzerinde sonuç zaman, ayrıntılı sonuçlarını ve günlük arama geçmişinizi görüntüleyebilirsiniz. Ayrıca, sonuçları daraltmak için modelleri göre filtreleyebilirsiniz.

## <a name="migrating-from-the-old-networking-analytics-solution"></a>Eski ağ Analytics çözüm'den geçiş
Ocak 2017'de, Azure uygulama ağ geçitleri ve Azure ağ güvenlik grupları günlükleri Log Analytics'e gönderme desteklenen yönteminizi değiştirdik. Bu değişiklikler, aşağıdaki avantajları sağlar:
+ Günlükleri Log Analytics bir depolama hesabı kullanmak zorunda kalmadan doğrudan için yazılır
+ Ne zaman bunlara Log Analytics'te almalarının günlükleri üretilir saatten daha az gecikme süresi
+ Daha az yapılandırma adımı
+ Azure tanılama her tür için ortak bir biçimi

Güncelleştirilmiş çözümlerini kullanmak için:

1. [Azure Application Gateway'ler aracılığıyla doğrudan Log Analytics'e gönderilecek tanılama Yapılandır](#enable-azure-application-gateway-diagnostics-in-the-portal)
2. [Azure ağ güvenlik grupları'ndan doğrudan Log Analytics'e gönderilecek tanılama Yapılandır](#enable-azure-network-security-group-diagnostics-in-the-portal)
2. Etkinleştirme *Azure Application Gateway Analytics* ve *Azure ağ güvenlik grubu analizi* açıklanan işlemi kullanarak çözüm [ekleme Log Analytics çözümleri Çözüm Galerisi](../monitoring/monitoring-solutions.md)
3. Tüm kaydedilmiş sorgular, panolar veya yeni veri türü kullanılacağını uyarıları güncelleştirme
  + AzureDiagnostics için türüdür. Kaynak türü için Azure ağ bağlantısı günlükleri filtrelemek için kullanabilirsiniz.

    | Onun yerine: | Kullanım: |
    | --- | --- |
    | NetworkApplicationgateways &#124; nerede OperationName "ApplicationGatewayAccess" == | AzureDiagnostics &#124; nerede ResourceType = "APPLICATIONGATEWAYS" ve OperationName "ApplicationGatewayAccess" == |
    | NetworkApplicationgateways &#124; nerede OperationName "ApplicationGatewayPerformance" == | AzureDiagnostics &#124; nerede ResourceType "APPLICATIONGATEWAYS" ve OperationName == ApplicationGatewayPerformance = |
    | NetworkSecuritygroups | AzureDiagnostics &#124; nerede ResourceType "NETWORKSECURITYGROUPS" == |

   + Bir son eki olan herhangi bir alan için \_s, \_d veya \_g adı küçük harflere ilk karakteri değiştirme
   + Bir son eki olan herhangi bir alan için \_o adı, veriler iç içe geçmiş alanı adlarını temel alarak ayrı ayrı alanlara bölünür.
4. Kaldırma *Azure ağ analizi (kullanım dışı)* çözüm.
  + PowerShell kullanıyorsanız, `Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that the workspace is in> -WorkspaceName <name of the log analytics workspace> -IntelligencePackName "AzureNetwork" -Enabled $false`

Değişiklik yeni çözümde görünür değil. önce toplanan veriler. Alan adları ve eski türünü kullanarak bu verileri sorgulamak devam edebilirsiniz.

## <a name="troubleshooting"></a>Sorun giderme
[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Kullanım [Log Analytics'te günlük aramaları](log-analytics-queries.md) ayrıntılı Azure tanılama verilerini görüntülemek için.
