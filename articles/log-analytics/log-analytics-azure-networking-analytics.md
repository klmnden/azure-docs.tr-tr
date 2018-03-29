---
title: Günlük analizi Azure ağ Analytics çözümde | Microsoft Docs
description: Azure ağ güvenlik grubu ve Azure uygulama ağ geçidi günlükleri gözden geçirmek için günlük analizi Azure ağ analiz çözümü kullanabilirsiniz.
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
ms.topic: article
ms.date: 03/20/2018
ms.author: richrund
ms.openlocfilehash: 17dadd784d59a2cc0cab6ffbae144010f896b296
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="azure-networking-monitoring-solutions-in-log-analytics"></a>Günlük analizi çözümlerinde izleme azure ağ iletişimi

Günlük analizi, ağları izleme için aşağıdaki çözümleri sunar:
* Ağ Performans İzleyicisi'ne (NPM)
 * Ağ durumunu izleyin
* Azure uygulama ağ geçidi analytics gözden geçirmek için
 * Azure uygulama ağ geçidi günlükleri
 * Azure uygulama ağ geçidi ölçümleri
* Azure ağ güvenlik grubu analytics gözden geçirmek için
 * Azure ağ güvenlik grubu günlükleri

## <a name="network-performance-monitor-npm"></a>Ağ Performans İzleyicisi'ni (NPM)

[Ağ Performansı İzleyicisi](log-analytics-network-performance-monitor.md) yönetimi çözümüdür izleme sistem durumu, kullanılabilirliği ve ulaşılabilirliği ağların izler çözümü, bir ağ.  Arasında bağlantı izlemek için kullanılır:

* Genel Bulut ve şirket içi
* veri merkezleri ve kullanıcı konumları (şubelere)
* çok katmanlı bir uygulama çeşitli katmanlarını barındırma alt ağlar.

Daha fazla bilgi için bkz: [Ağ Performansı İzleyicisi](log-analytics-network-performance-monitor.md).

## <a name="azure-application-gateway-and-network-security-group-analytics"></a>Azure uygulama ağ geçidi ve ağ güvenlik grubu analizi
Çözümler kullanmak için:
1. Günlük analizi için yönetim çözümü ekleyin ve
2. Tanılama günlük analizi çalışma alanına yönlendirmek tanılamayı etkinleştirin. Günlükleri Azure Blob depolama alanına yazmak gerekli değildir.

Tanılama ve karşılık gelen çözümü birini veya her ikisini de uygulama ağ geçidi ve ağ güvenlik grupları için etkinleştirebilirsiniz.

Belirli bir kaynak türü için tanılama günlük kaydını etkinleştirmeyin, ancak çözümü yüklemek, bu kaynak için Pano Kanatlar boş ve bir hata iletisi görüntülenir.

> [!NOTE]
> Ocak 2017 ' günlükleri uygulama ağ geçitleri ve ağ güvenlik grupları için günlük analizi gönderme desteklenen şekilde değiştirildi. Görürseniz **Azure ağ analizi (kullanım dışı)** çözüm, başvurmak [eski ağ Analytics çözüm'den geçiş](#migrating-from-the-old-networking-analytics-solution) adımları izlemeniz gerekir.
>
>

## <a name="review-azure-networking-data-collection-details"></a>Veri toplama ayrıntıları ağ Azure gözden geçirin
Azure uygulama ağ geçidi analizi ve ağ güvenlik grubu analytics Yönetimi çözümlerini doğrudan Azure uygulama ağ geçitleri ve ağ güvenlik grupları tanılama günlüklerini toplayın. Günlükleri Azure Blob depolama alanına yazmak gerekli değildir ve aracı için veri toplama gereklidir.

Aşağıdaki tabloda, veri toplama yöntemleri ve Azure uygulama ağ geçidi analizi ve ağ güvenlik grubu analiz için verileri nasıl toplanır ilgili diğer ayrıntıları gösterir.

| Platform | Doğrudan Aracısı | Systems Center Operations Manager Aracısı | Azure | Operations Manager gerekli? | Operations Manager Aracısı verilerinin yönetim grubu gönderilen | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- |
| Azure |  |  |&#8226; |  |  |oturum açıldığında |


## <a name="azure-application-gateway-analytics-solution-in-log-analytics"></a>Azure uygulama ağ geçidi analytics çözümüne günlük analizi

![Azure uygulama ağ geçidi Analytics simgesi](./media/log-analytics-azure-networking/azure-analytics-symbol.png)

Günlükleri, uygulama ağ geçitleri için desteklenir:

* ApplicationGatewayAccessLog
* ApplicationGatewayPerformanceLog
* ApplicationGatewayFirewallLog

Aşağıdaki ölçümleri uygulama ağ geçitleri için desteklenir:

* 5 dakikalık işleme

### <a name="install-and-configure-the-solution"></a>Yükleyin ve yapılandırın
Yüklemek ve Azure uygulama ağ geçidi analiz çözümü yapılandırmak için aşağıdaki yönergeleri kullanın:

1. Azure uygulama ağ geçidi analytics çözümden etkinleştirmek [Azure Market](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureAppGatewayAnalyticsOMS?tab=Overview) veya açıklanan işlemi kullanarak [Çözümleri Galerisi eklemek günlük analizi çözümleri](log-analytics-add-solutions.md).
2. Tanılama için günlüğü etkinleştirmek [uygulama ağ geçitleri](../application-gateway/application-gateway-diagnostics.md) izlemek istediğiniz.

#### <a name="enable-azure-application-gateway-diagnostics-in-the-portal"></a>Azure uygulama ağ geçidi tanılama portalda etkinleştir

1. Azure portalında izlemek için uygulama ağ geçidi kaynağa gidin
2. Seçin *tanılama günlükleri* aşağıdaki sayfasını açmak için

   ![Azure uygulama ağ geçidi kaynak görüntüsü](./media/log-analytics-azure-networking/log-analytics-appgateway-enable-diagnostics01.png)
3. Tıklatın *tanılamayı açın* aşağıdaki sayfasını açmak için

   ![Azure uygulama ağ geçidi kaynak görüntüsü](./media/log-analytics-azure-networking/log-analytics-appgateway-enable-diagnostics02.png)
4. Tanılama'yı açmak için tıklatın *üzerinde* altında *durumu*
5. Onay kutusunu tıklatın *için günlük analizi Gönder*
6. Varolan bir günlük analizi çalışma alanını seçin veya bir çalışma alanı oluşturma
7. Altında onay kutusuna tıklayın **günlük** toplamak için günlük türlerinin her biri için
8. Tıklatın *kaydetmek* günlük analizi için tanılama günlük kaydını etkinleştirmek için

#### <a name="enable-azure-network-diagnostics-using-powershell"></a>PowerShell kullanarak Azure Ağ Tanılama'yı etkinleştir

Aşağıdaki PowerShell betiğini tanılama uygulama ağ geçitleri için günlüğü etkinleştirmek nasıl bir örnek sağlar.

```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$gateway = Get-AzureRmApplicationGateway -Name 'ContosoGateway'

Set-AzureRmDiagnosticSetting -ResourceId $gateway.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="use-azure-application-gateway-analytics"></a>Azure uygulama ağ geçidi analytics kullanın
![Azure uygulama ağ geçidi analytics döşeme görüntüsü](./media/log-analytics-azure-networking/log-analytics-appgateway-tile.png)

Tıklattıktan sonra **Azure uygulama ağ geçidi analytics** döşeme genel bakış günlüklerinizi özetlerini görüntüleyin ve aşağıdaki kategorilerde ayrıntıları için detaya:

* Uygulama ağ geçidi erişimi günlükleri
  * Uygulama ağ geçidi erişim günlükleri için istemci ve sunucu hataları
  * Her uygulama ağ geçidi için saat başına istek sayısı
  * Saat başına istekleri her uygulama ağ geçidi için başarısız oldu
  * Uygulama ağ geçitleri için kullanıcı aracısı tarafından hataları
* Uygulama ağ geçidi performansı
  * Uygulama ağ geçidi ana bilgisayar durumu
  * Uygulama ağ geçidi için en yüksek ve 95 yüzdebirlik başarısız istekleri

![Azure uygulama ağ geçidi analytics Pano görüntüsü](./media/log-analytics-azure-networking/log-analytics-appgateway01.png)

![Azure uygulama ağ geçidi analytics Pano görüntüsü](./media/log-analytics-azure-networking/log-analytics-appgateway02.png)

Üzerinde **Azure uygulama ağ geçidi analytics** Panosu, Kanatlar birinde özet bilgilerini inceleyin ve bir günlük arama sayfasında ayrıntılı bilgileri görüntülemek için tıklatın.

Herhangi bir günlük arama sayfası üzerinde sonuçları zaman, ayrıntılı sonuçları ve günlük arama geçmişi görüntüleyebilirsiniz. Sonuçları daraltmak için modelleri göre de filtre uygulayabilirsiniz.


## <a name="azure-network-security-group-analytics-solution-in-log-analytics"></a>Günlük analizi analytics çözümde Azure ağ güvenlik grubu

![Azure ağ güvenlik grubu Analytics simgesi](./media/log-analytics-azure-networking/azure-analytics-symbol.png)

Günlükleri, ağ güvenlik grupları için desteklenir:

* NetworkSecurityGroupEvent
* NetworkSecurityGroupRuleCounter

### <a name="install-and-configure-the-solution"></a>Yükleyin ve yapılandırın
Yüklemek ve Azure ağ analiz çözümü yapılandırmak için aşağıdaki yönergeleri kullanın:

1. Azure ağ güvenlik grubu analytics çözümden etkinleştirmek [Azure Market](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureNSGAnalyticsOMS?tab=Overview) veya açıklanan işlemi kullanarak [Çözümleri Galerisi eklemek günlük analizi çözümleri](log-analytics-add-solutions.md).
2. Tanılama için günlüğü etkinleştirmek [ağ güvenlik grubu](../virtual-network/virtual-network-nsg-manage-log.md) izlemek istediğiniz kaynakları.

### <a name="enable-azure-network-security-group-diagnostics-in-the-portal"></a>Azure ağ güvenlik grubu tanılama portalda etkinleştir

1. Azure portalında izlemek için ağ güvenlik grubu kaynağa gidin
2. Seçin *tanılama günlükleri* aşağıdaki sayfasını açmak için

   ![Azure ağ güvenlik grubu kaynak görüntüsü](./media/log-analytics-azure-networking/log-analytics-nsg-enable-diagnostics01.png)
3. Tıklatın *tanılamayı açın* aşağıdaki sayfasını açmak için

   ![Azure ağ güvenlik grubu kaynak görüntüsü](./media/log-analytics-azure-networking/log-analytics-nsg-enable-diagnostics02.png)
4. Tanılama'yı açmak için tıklatın *üzerinde* altında *durumu*
5. Onay kutusunu tıklatın *için günlük analizi Gönder*
6. Varolan bir günlük analizi çalışma alanını seçin veya bir çalışma alanı oluşturma
7. Altında onay kutusuna tıklayın **günlük** toplamak için günlük türlerinin her biri için
8. Tıklatın *kaydetmek* günlük analizi için tanılama günlük kaydını etkinleştirmek için

### <a name="enable-azure-network-diagnostics-using-powershell"></a>PowerShell kullanarak Azure Ağ Tanılama'yı etkinleştir

Ağ güvenlik grupları için günlük kaydı tanılama etkinleştirmek nasıl bir örneği aşağıdaki PowerShell betiğini sağlar
```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$nsg = Get-AzureRmNetworkSecurityGroup -Name 'ContosoNSG'

Set-AzureRmDiagnosticSetting -ResourceId $nsg.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="use-azure-network-security-group-analytics"></a>Azure ağ güvenlik grubu kullanın analizi
Tıklattıktan sonra **Azure ağ güvenlik grubu analytics** döşeme genel bakış günlüklerinizi özetlerini görüntüleyin ve aşağıdaki kategorilerde ayrıntıları için detaya:

* Ağ güvenlik grubu akışları engellendi
  * Ağ güvenlik grubu kural engellenen akışlarıyla
  * Engellenen akışlarıyla MAC adresleri
* Ağ güvenlik grubu akışları izin
  * İzin verilen akışları ile ağ güvenlik grubu kuralları
  * İzin verilen akışlarıyla MAC adresleri

![Azure ağ güvenlik grubu analytics Pano görüntüsü](./media/log-analytics-azure-networking/log-analytics-nsg01.png)

![Azure ağ güvenlik grubu analytics Pano görüntüsü](./media/log-analytics-azure-networking/log-analytics-nsg02.png)

Üzerinde **Azure ağ güvenlik grubu analytics** Panosu, Kanatlar birinde özet bilgilerini inceleyin ve bir günlük arama sayfasında ayrıntılı bilgileri görüntülemek için tıklatın.

Herhangi bir günlük arama sayfası üzerinde sonuçları zaman, ayrıntılı sonuçları ve günlük arama geçmişi görüntüleyebilirsiniz. Sonuçları daraltmak için modelleri göre de filtre uygulayabilirsiniz.

## <a name="migrating-from-the-old-networking-analytics-solution"></a>Eski ağ Analytics çözüm'den geçiş
Ocak 2017 ' günlükleri Azure uygulama ağ geçitleri ve Azure ağ güvenlik grupları için günlük analizi gönderme desteklenen şekilde değiştirildi. Bu değişiklikler, aşağıdaki avantajları sağlar:
+ Günlükleri doğrudan günlük analizi depolama hesabı kullanmak zorunda kalmadan yazılır
+ Günlükleri bunları günlük analizi kullanılabilir olması için ne zaman oluşturulur zamandan daha az gecikme süresi
+ Daha az yapılandırma adımları
+ Azure tanılama tüm türleri için ortak bir biçimi

Güncelleştirilmiş çözümleri kullanmak için:

1. [Azure uygulama Gateway bileşenlerinden doğrudan günlük analizi için gönderilecek tanılama Yapılandır](#enable-azure-application-gateway-diagnostics-in-the-portal)
2. [Azure ağ güvenlik gruplarındaki doğrudan günlük analizi için gönderilecek tanılama Yapılandır](#enable-azure-network-security-group-diagnostics-in-the-portal)
2. Etkinleştirme *Azure uygulama ağ geçidi Analytics* ve *Azure ağ güvenlik grubu Analytics* açıklanan işlemi kullanarak çözüm [eklemek günlük analizi çözümleri Çözümleri Galerisi](log-analytics-add-solutions.md)
3. Tüm kayıtlı sorgu, panolar veya yeni veri türünü kullanmak için uyarıları güncelleştirme
  + İçin AzureDiagnostics türüdür. Azure ağ günlüklerine filtrelemek için ResourceType kullanabilirsiniz.

    | Onun yerine: | Kullanım: |
    | --- | --- |
    | NetworkApplicationgateways &#124; burada OperationName "ApplicationGatewayAccess" == | AzureDiagnostics &#124; where ResourceType="APPLICATIONGATEWAYS" and OperationName=="ApplicationGatewayAccess" |
    | NetworkApplicationgateways &#124; burada OperationName "ApplicationGatewayPerformance" == | AzureDiagnostics &#124; where ResourceType=="APPLICATIONGATEWAYS" and OperationName=ApplicationGatewayPerformance |
    | NetworkSecuritygroups | AzureDiagnostics &#124; where ResourceType=="NETWORKSECURITYGROUPS" |

   + Sonekine sahip herhangi bir alan için \_s, \_d veya \_g adı küçük harflere ilk karakter değiştirme
   + Sonekine sahip herhangi bir alan için \_adında, verileri o iç içe geçmiş alan adlarını temel alarak tek tek alanlara bölünür.
4. Kaldırma *Azure ağ analizi (kullanım dışı)* çözümü.
  + PowerShell kullanıyorsanız, kullanın `Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that the workspace is in> -WorkspaceName <name of the log analytics workspace> -IntelligencePackName "AzureNetwork" -Enabled $false`

Değişiklik yeni çözümde görünür değil önce veri toplanmadı. Eski türü ve alan adları kullanarak bu verileri sorgulamak devam edebilirsiniz.

## <a name="troubleshooting"></a>Sorun giderme
[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Kullanım [günlük analizi aramaları oturum](log-analytics-log-searches.md) ayrıntılı Azure tanılama verilerini görüntülemek için.
