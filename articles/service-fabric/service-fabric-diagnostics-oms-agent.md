---
title: Azure Service Fabric - Azure İzleyici ile performans izleme günlükleri | Microsoft Docs
description: Kapsayıcıları ve Azure Service Fabric kümeleriniz için performans sayaçlarını izlemek için Log Analytics aracısını ayarlama konusunda bilgi edinin.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/16/2018
ms.author: srrengar
ms.openlocfilehash: 819f6ee4ab079361279a567bceeb74c33fe14186
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60952396"
---
# <a name="performance-monitoring-with-azure-monitor-logs"></a>Azure İzleyici günlüklerine ile performans izleme

Bu makalede, bir sanal makine ölçek kümesi uzantısını kümeniz için Log Analytics aracısını ekleyin ve mevcut Azure Log Analytics çalışma alanınıza bağlamak için ilgili adımları içermektedir. Bu kapsayıcı, uygulamaları ve performans izleme hakkında tanılama veri toplama sağlar. Bu uzantı olarak sanal makine ölçek kümesi kaynağına ekleyerek, Azure Resource Manager her düğümde yükleneceğini bile kümenin ne zaman ölçeklendirme sağlar.

> [!NOTE]
> Bu makalede daha önce ayarlamış bir Azure Log Analytics çalışma alanı sahibi olduğunuzu varsayar. Bunu yapmazsanız, attıktan [günlüklerini Azure İzleyicisi'ni ayarlayın](service-fabric-diagnostics-oms-setup.md)

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="add-the-agent-extension-via-azure-cli"></a>Azure CLI aracılığıyla Aracı Uzantısı Ekle

Log Analytics aracısını kümenize eklemek için en iyi yolu, Azure CLI ile API sanal makine ölçek ayarlanır. Azure CLI'yı henüz yoksa, Azure portalında head üzerinde ve açık bir [Cloud Shell](../cloud-shell/overview.md) örneği veya [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli).

1. Cloud Shell istendikten sonra kaynak ile aynı abonelikte çalıştığından emin olun. Bu kontrol `az account show` ve "name" değeri, kümenizin abonelik eşleştiğinden emin olun.

2. Portalda Log Analytics çalışma alanınızın bulunduğu bir kaynak grubuna gidin. Log analytics kaynağını (kaynak türünü Log Analytics çalışma alanı olacaktır) tıklayın. Kaynak genel bakış sayfasında olduktan sonra tıklayarak **Gelişmiş ayarlar** sol menüdeki Ayarlar bölümünde.

    ![Log analytics Özellikler sayfası](media/service-fabric-diagnostics-oms-agent/oms-advanced-settings.png)

3. Tıklayarak **Windows sunucuları** bir Windows kümesi, bekliyor durumunda ve **Linux sunucuları** bir Linux kümesi oluşturuyorsanız. Bu sayfa size gösterecektir, `workspace ID` ve `workspace key` (birincil anahtar olarak portalda listelenen). Sonraki adım için her ikisi de gerekir.

4. Kümenizi, Log Analytics aracısını yüklemek için komutu çalıştırmak kullanarak `vmss extension set` API, Cloud shell'de:

    Bir Windows kümesi için:

    ```sh
    az vmss extension set --name MicrosoftMonitoringAgent --publisher Microsoft.EnterpriseCloud.Monitoring --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType> --settings "{'workspaceId':'<Log AnalyticsworkspaceId>'}" --protected-settings "{'workspaceKey':'<Log AnalyticsworkspaceKey>'}"
    ```

    Bir Linux kümesi için:

    ```sh
    az vmss extension set --name OmsAgentForLinux --publisher Microsoft.EnterpriseCloud.Monitoring --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType> --settings "{'workspaceId':'<Log AnalyticsworkspaceId>'}" --protected-settings "{'workspaceKey':'<Log AnalyticsworkspaceKey>'}"
    ```

    Bir Windows kümesine eklenen Log Analytics aracısını örneği aşağıda verilmiştir.

    ![Analiz aracı CLI komutunu oturum](media/service-fabric-diagnostics-oms-agent/cli-command.png)

5. Bu aracı için düğümlerinizin başarıyla eklemek için az 15 dakika sürer. Aracıları kullanarak eklendiğini doğrulayabilirsiniz `az vmss extension list` API:

    ```sh
    az vmss extension list --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType>
    ```

## <a name="add-the-agent-via-the-resource-manager-template"></a>Resource Manager şablonu aracılığıyla Aracısı Ekle

Bir Azure Log Analytics çalışma alanı dağıtma ve her düğümleriniz bir aracı ekleyin, örnek Resource Manager şablonları, kullanılabilir [Windows](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-OMS-UnSecure) veya [Linux](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Linux).

İndirin ve en iyi sonucu gereksinimlerinize uyan bir küme dağıtmak için bu şablonu değiştirin.

## <a name="view-performance-counters"></a>Performans sayaçları görüntüleyin

Log Analytics aracısını, head üzerinde eklediğinize göre hangi performans sayaçlarını seçin için Log Analytics portalı üzerinden toplamak ister misiniz?

1. Azure portalında Service Fabric analizi çözümü oluşturduğunuz kaynak grubuna gidin. Seçin **ServiceFabric\<nameOfLog AnalyticsWorkspace\>**.

2. **Log Analytics**’i tıklayın.

3. Tıklayın **Gelişmiş ayarlar**.

4. Tıklayın **veri**, ardından **Windows veya Linux performans sayaçlarıyla**. Varsayılan sayaçları etkinleştirmek için seçebileceğiniz bir listesi bulunur ve çok koleksiyon aralığını ayarlayabilirsiniz. Ayrıca ekleyebilirsiniz [ek performans sayaçları](service-fabric-diagnostics-event-generation-perf.md) toplanacak. Bu doğru biçimde başvurulan [makale](https://msdn.microsoft.com/library/windows/desktop/aa373193(v=vs.85).aspx).

5. Tıklayın **Kaydet**, ardından **Tamam**.

6. Gelişmiş ayarlar dikey penceresini kapatın.

7. Genel başlığına tıklayın **çalışma özeti**.

8. Her bir Service Fabric için de dahil olmak üzere etkin çözüm için bir grafik biçiminde kutucuklar görürsünüz. Tıklayın **Service Fabric** Service Fabric analizi çözümü devam etmek için bir grafik.

9. İşlevsel kanal ve güvenilir hizmetler olayları grafikleriyle birkaç kutucuk görürsünüz. Seçtiğiniz sayaçları içeriye veri grafik gösterimi düğüm ölçümlerini altında görünür.

10. Ek ayrıntıları görmek için bir kapsayıcı ölçüsünü grafiğe tıklayın. Ayrıca küme olayları ve düğümler, performans sayaç adı ve Kusto sorgu dilini kullanarak değerleri bir filtreye benzer şekilde, performans sayacı verileri sorgulayabilirsiniz.

![Log Analytics performans sayacı sorgusu](media/service-fabric-diagnostics-event-analysis-oms/oms_node_metrics_table.PNG)

## <a name="next-steps"></a>Sonraki adımlar

* Toplama ilgili [performans sayaçları](service-fabric-diagnostics-event-generation-perf.md). Belirli bir performans sayaçları toplamak için Log Analytics aracısını yapılandırmak için gözden [veri kaynakları yapılandırılarak](../azure-monitor/platform/agent-data-sources.md#configuring-data-sources).
* Azure İzleyici günlüklerine ayarlamak için yapılandırma [otomatik uyarı verme](../log-analytics/log-analytics-alerts.md) algılama ve tanılama konusunda yardımcı olmak için
* Alternatif olarak, performans sayaçları aracılığıyla toplayabilirsiniz [Azure tanılama uzantısı ve bunları Application Insights'a gönderme](service-fabric-diagnostics-event-aggregation-wad.md#add-the-application-insights-sink-to-the-resource-manager-template)
