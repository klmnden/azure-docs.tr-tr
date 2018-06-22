---
title: Azure Service Fabric - performans günlük analizi ile izleme | Microsoft Docs
description: Kapsayıcılar ve Azure Service Fabric kümeleri için performans sayaçları izleme için günlük analizi Aracısı ayarlama öğrenin.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/16/2018
ms.author: srrengar
ms.openlocfilehash: a31fe62f2e81a0e39e4c314fc736e91e72bf7517
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36301562"
---
# <a name="performance-monitoring-with-log-analytics"></a>Performans günlük analizi ile izleme

Bu makalede kümenize uzantısı bir sanal makine ölçek kümesi gibi günlük analizi Aracısı ekleyin ve, mevcut Azure günlük analizi çalışma alanına bağlayın adımları yer almaktadır. Bu kapsayıcı, uygulamaları ve performans izleme hakkında tanılama veri toplama sağlar. Bu uzantı olarak sanal makine ölçek kümesi kaynağı ekleyerek, Azure Resource Manager her düğümde yüklü bile küme ne zaman ölçeklendirme sağlar.

> [!NOTE]
> Bu makalede daha önce ayarlamış bir Azure günlük analizi çalışma alanı sahibi olduğunuzu varsayar. Bunu yapmazsanız, üzerinden için head [Azure günlük analizi ayarlayın](service-fabric-diagnostics-oms-setup.md)

## <a name="add-the-agent-extension-via-azure-cli"></a>Azure CLI aracılığıyla Aracısı uzantısı Ekle

Günlük analizi Aracısı, kümeye eklemek için en iyi yolu, Azure CLI ile API sanal makine ölçek ayarlanır. Azure CLI henüz ayarlanan yoksa, Azure portalına head üzerinden ve açık bir [bulut Kabuk](../cloud-shell/overview.md) örneği veya [Azure CLI 2.0 yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli).

1. Bulut Kabuk istenen sonra kaynağınız ile aynı abonelikte çalıştığından emin olun. Bu kontrol `az account show` ve "name" değeri, kümenizin abonelik eşleştiğinden emin olun.

2. Portalda, günlük analizi çalışma alanınız bulunduğu kaynak grubuna gidin. Günlük analizi kaynağını (kaynak türü günlük analizi olacaktır) tıklayın. Kaynak Genel Bakış sayfasına olduktan sonra tıklayın **Gelişmiş ayarları** sol menüde ayarları bölümünde.

    ![Günlük analizi Özellikler sayfası](media/service-fabric-diagnostics-oms-agent/oms-advanced-settings.png)
 
3. Tıklayın **Windows sunucuları** Windows Küme, duran varsa ve **Linux sunucuları** Linux kümesi oluşturuyorsanız. Bu sayfada, gösterilir, `workspace ID` ve `workspace key` (birincil anahtar olarak portalda listelenen). Sonraki adım için her ikisini de gerekir.

4. Günlük analizi aracısını yükler, kümeniz için komutu çalıştırmak kullanarak `vmss extension set` API bulut Kabuğu'nda:

    Windows Küme için:
    
    ```sh
    az vmss extension set --name MicrosoftMonitoringAgent --publisher Microsoft.EnterpriseCloud.Monitoring --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType> --settings "{'workspaceId':'<Log AnalyticsworkspaceId>'}" --protected-settings "{'workspaceKey':'<Log AnalyticsworkspaceKey>'}"
    ```

    Linux kümesi için:

    ```sh
    az vmss extension set --name Log AnalyticsAgentForLinux --publisher Microsoft.EnterpriseCloud.Monitoring --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType> --settings "{'workspaceId':'<Log AnalyticsworkspaceId>'}" --protected-settings "{'workspaceKey':'<Log AnalyticsworkspaceKey>'}"
    ```

    Günlük analizi Aracısı Windows Kümeye eklenmekte olan bir örneği burada verilmiştir.

    ![Günlük analizi aracı CLI komutu](media/service-fabric-diagnostics-oms-agent/cli-command.png)
 
5. Bu aracı, düğümlere başarıyla eklemek için küçüktür 15 dakika sürer. Aracıları kullanarak eklendiğini doğrulayabilirsiniz `az vmss extension list` API'si:

    ```sh
    az vmss extension list --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType>
    ```

## <a name="add-the-agent-via-the-resource-manager-template"></a>Resource Manager şablonu aracılığıyla aracısı ekleme

Bir Azure günlük analizi çalışma alanı dağıtma ve düğümlerin her birinde sizin için bir aracı ekleyin örnek Resource Manager şablonları için kullanılabilen [Windows](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Windows) veya [Linux](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Linux).

Karşıdan yükle ve en iyi sonucu gereksinimlerinize uyan bir küme dağıtmak için bu şablonu değiştirin.

## <a name="view-performance-counters"></a>Performans sayaçları görüntüleyin

Günlük analizi Aracısı, head eklediğiniz göre hangi performans sayaçlarını seçmek için günlük analizi portal üzerinden toplamak istediğiniz. 

1. Azure portalında Service Fabric analiz çözümü oluşturduğunuz kaynak grubuna gidin. Seçin **ServiceFabric\<nameOfLog AnalyticsWorkspace\>**.

2. **Log Analytics**’i tıklayın.

3. Tıklatın **Gelişmiş ayarları**.

4. Tıklatın **veri**, ardından **Windows veya Linux performans sayaçları**. Etkinleştirmeyi seçebilirsiniz varsayılan sayaçlarının listesi yoktur ve koleksiyon için bir aralığı çok ayarlayabilirsiniz. Ayrıca ekleyebileceğiniz [ek performans sayaçları](service-fabric-diagnostics-event-generation-perf.md) toplanacak. Bu doğru biçimde başvurulan [makale](https://msdn.microsoft.com/library/windows/desktop/aa373193(v=vs.85).aspx).

5. Tıklatın **kaydetmek**, ardından **Tamam**.

6. Gelişmiş ayarlar dikey penceresini kapatın.

7. Genel başlığı altında tıklatın **genel bakış**.

8. Her biri için Service Fabric dahil olmak üzere etkin çözümleri için bir grafik biçiminde kutucuklar görürsünüz. Tıklatın **Service Fabric** Service Fabric analiz çözümü devam etmek için grafik.

9. İşletimsel kanal ve güvenilir hizmetler olayları grafikleri birkaç kutucuk görürsünüz. İçinde seçtiğiniz sayaçları akan verilere grafik gösterimi düğümü ölçümleri altında görünür. 

10. Ek ayrıntıları görmek için bir kapsayıcı ölçüm grafiğe sağ tıklayın. Performans sayacı verileri kümesi olayları ve düğümler, performans sayaç adı ve Kusto sorgu dili kullanarak değerlere filtre benzer şekilde sorgulayabilirsiniz.

![Günlük analizi performans sayacı sorgu](media/service-fabric-diagnostics-event-analysis-oms/oms_node_metrics_table.PNG)

## <a name="next-steps"></a>Sonraki adımlar

* Toplama ilgili [performans sayaçları](service-fabric-diagnostics-event-generation-perf.md). Belirli bir performans sayaçları toplamak için günlük analizi aracısını yapılandırmak için gözden [veri kaynaklarını yapılandırma](../log-analytics/log-analytics-data-sources.md#configuring-data-sources).
* Ayarlamak için günlük analizi yapılandırma [uyarı otomatik](../log-analytics/log-analytics-alerts.md) algılama ve tanılama yardımcı olmak için
* Alternatif olarak, performans sayaçları aracılığıyla toplayabilirsiniz [Azure tanılama uzantısını ve Application Insights'a gönderme](service-fabric-diagnostics-event-analysis-appinsights.md#add-the-ai-sink-to-the-resource-manager-template)
