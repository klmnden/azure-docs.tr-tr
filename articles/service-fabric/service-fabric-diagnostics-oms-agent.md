---
title: Azure Service Fabric - performans günlük analizi ile izleme | Microsoft Docs
description: Kapsayıcılar ve Azure Service Fabric kümeleri için performans sayaçları izleme için OMS aracısının ayarlanacağını öğrenin.
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
ms.openlocfilehash: a3ce72e51477c1eda99461b3910bfeeac207be55
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="performance-monitoring-with-log-analytics"></a>Performans günlük analizi ile izleme

Bu makalede kümenize uzantısı bir sanal makine ölçek kümesi gibi günlük analizi, olarak da bilinen OMS Aracısı ekleyin ve, mevcut Azure günlük analizi çalışma alanına bağlayın adımları yer almaktadır. Bu kapsayıcı, uygulamaları ve performans izleme hakkında tanılama veri toplama sağlar. Bu uzantı olarak sanal makine ölçek kümesi kaynağı ekleyerek, Azure Resource Manager her düğümde yüklü bile küme ne zaman ölçeklendirme sağlar.

> [!NOTE]
> Bu makalede daha önce ayarlamış bir Azure günlük analizi çalışma alanı sahibi olduğunuzu varsayar. Bunu yapmazsanız, üzerinden için head [Azure günlük analizi ayarlayın](service-fabric-diagnostics-oms-setup.md)

## <a name="add-the-agent-extension-via-azure-cli"></a>Azure CLI aracılığıyla Aracısı uzantısı Ekle

OMS Aracısı, kümeye eklemek için en iyi yolu, Azure CLI ile API sanal makine ölçek ayarlanır. Azure CLI henüz ayarlanan yoksa, Azure portalına head üzerinden ve açık bir [bulut Kabuk](../cloud-shell/overview.md) örneği veya [Azure CLI 2.0 yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli).

1. Bulut Kabuk istenen sonra kaynağınız ile aynı abonelikte çalıştığından emin olun. Bu kontrol `az account show` ve "name" değeri, kümenizin abonelik eşleştiğinden emin olun.

2. Portalda, günlük analizi çalışma alanınız bulunduğu kaynak grubuna gidin. Günlük analizi kaynağını (kaynak türü günlük analizi olacaktır) tıklayın. Kaynak Genel Bakış sayfasına olduktan sonra tıklayın **Gelişmiş ayarları** sol menüde ayarları bölümünde.

    ![Günlük analizi Özellikler sayfası](media/service-fabric-diagnostics-oms-agent/oms-advanced-settings.png)
 
3. Tıklayın **Windows sunucuları** Windows Küme, duran varsa ve **Linux sunucuları** Linux kümesi oluşturuyorsanız. Bu sayfada, gösterilir, `workspace ID` ve `workspace key` (birincil anahtar olarak portalda listelenen). Sonraki adım için her ikisini de gerekir.

4. Kümenizde üzerine OMS Aracısı'nı yüklemek için komutu çalıştırmak kullanarak `vmss extension set` API bulut Kabuğu'nda:

    Windows Küme için:
    
    ```sh
    az vmss extension set --name MicrosoftMonitoringAgent --publisher Microsoft.EnterpriseCloud.Monitoring --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType> --settings "{'workspaceId':'<OMSworkspaceId>'}" --protected-settings "{'workspaceKey':'<OMSworkspaceKey>'}"
    ```

    Linux kümesi için:

    ```sh
    az vmss extension set --name OmsAgentForLinux --publisher Microsoft.EnterpriseCloud.Monitoring --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType> --settings "{'workspaceId':'<OMSworkspaceId>'}" --protected-settings "{'workspaceKey':'<OMSworkspaceKey>'}"
    ```

    Bir Windows kümeye eklenen OMS Aracısı bir örneği burada verilmiştir.

    ![OMS Aracısı CLI komutu](media/service-fabric-diagnostics-oms-agent/cli-command.png)
 
5. Bu aracı, düğümlere başarıyla eklemek için küçüktür 15 dakika sürer. Aracıları kullanarak eklendiğini doğrulayabilirsiniz `az vmss extension list` API'si:

    ```sh
    az vmss extension list --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType>
    ```

## <a name="add-the-agent-via-the-resource-manager-template"></a>Resource Manager şablonu aracılığıyla aracısı ekleme

Bir Azure günlük analizi çalışma alanı dağıtma ve düğümlerin her birinde sizin için bir aracı ekleyin örnek Resource Manager şablonları için kullanılabilen [Windows](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Windows) veya [Linux](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Linux).

Karşıdan yükle ve en iyi sonucu gereksinimlerinize uyan bir küme dağıtmak için bu şablonu değiştirin.

## <a name="view-performance-counters-in-the-log-analytics-portal"></a>Günlük analizi portalında performans sayaçları görüntüleyin

OMS Aracısı, head eklediğiniz göre hangi performans sayaçlarını seçmek için günlük analizi portal üzerinden toplamak istediğiniz. 

1. Azure portalında Service Fabric analiz çözümü oluşturduğunuz kaynak grubuna gidin. Seçin **ServiceFabric\<nameOfOMSWorkspace\>**  ve kendi genel bakış sayfasına gidin. En üstte OMS Portalı'na gitmek için bağlantıyı tıklatın.

2. Portalda olduğunuzda, her biri için Service Fabric dahil olmak üzere etkin çözümleri için bir grafik biçiminde bir kutucuk görürsünüz. Service Fabric analiz çözümü devam etmek için burayı tıklatın. 

3. Şimdi işletimsel kanal ve güvenilir hizmetler olayları grafikleri birkaç kutucuk göreceksiniz. Sağ taraftaki ayarları sayfasına gitmek için dişli simgesine tıklayın.

    ![OMS ayarları](media/service-fabric-diagnostics-oms-agent/oms-solutions-settings.png)

4. Ayarlar sayfasında veri tıklatın ve Windows veya Linux performans sayaçlarını seçin. Varsayılan değerleri etkinleştirmeyi seçebilirsiniz listesi vardır ve koleksiyon için bir aralığı çok ayarlayabilirsiniz. Ayrıca ekleyebileceğiniz [ek performans sayaçları](service-fabric-diagnostics-event-generation-perf.md) toplanacak. Bu doğru biçimde başvurulan [makale](https://msdn.microsoft.com/library/windows/desktop/aa373193(v=vs.85).aspx).

Sayaçlarınızın yapılandırılmış sonra head yedekleme çözümleri sayfasına ve veri giriş ve altında grafiklerde görüntülenen akan yakında görürsünüz **düğümü ölçümleri**. Performans sayacı verileri kümesi olayları ve düğümler, performans sayaç adı ve Kusto sorgu dili kullanarak değerlere filtre benzer şekilde sorgulayabilirsiniz. 

![OMS performans sayacı sorgu](media/service-fabric-diagnostics-oms-agent/oms-perf-counter-query.png)

## <a name="next-steps"></a>Sonraki adımlar

* Toplama ilgili [performans sayaçları](service-fabric-diagnostics-event-generation-perf.md). Belirli bir performans sayaçları toplamak için OMS aracısının yapılandırmak için gözden [veri kaynaklarını yapılandırma](../log-analytics/log-analytics-data-sources.md#configuring-data-sources).
* Ayarlamak için günlük analizi yapılandırma [uyarı otomatik](../log-analytics/log-analytics-alerts.md) algılama ve tanılama yardımcı olmak için
* Alternatif olarak, performans sayaçları aracılığıyla toplayabilirsiniz [Azure tanılama uzantısını ve Application Insights'a gönderme](service-fabric-diagnostics-event-analysis-appinsights.md#add-the-ai-sink-to-the-resource-manager-template)