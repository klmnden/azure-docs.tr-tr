---
title: Azure Service Fabric - OMS Aracısı ile izleme ayarlama | Microsoft Docs
description: Kapsayıcılar ve Azure Service Fabric kümeleri için performans sayaçları izleme için OMS aracısının ayarlanacağını öğrenin.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/03/2018
ms.author: dekapur
ms.openlocfilehash: dbafe0da58f5026400381a3cc96cf99ade0795de
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="add-the-oms-agent-to-a-cluster"></a>OMS Aracısı bir kümeye ekleme

Bu makalede kümenize uzantısı bir sanal makine ölçek kümesi gibi OMS Aracısı ekleyin ve, mevcut Azure günlük analizi çalışma alanına bağlayın adımları yer almaktadır. Bu kapsayıcı, uygulamaları ve performans izleme hakkında tanılama veri toplama sağlar. Bu uzantı olarak ekleyerek, Azure Resource Manager her düğümde yüklü bile küme ne zaman ölçeklendirme sağlar.

> [!NOTE]
> Bu makalede daha önce ayarlamış bir Azure günlük analizi çalışma alanı sahibi olduğunuzu varsayar. Bunu yapmazsanız, üzerinden için head [Azure günlük analizi ayarlayın](service-fabric-diagnostics-oms-setup.md)

## <a name="add-the-agent-extension-via-azure-cli"></a>Azure CLI aracılığıyla Aracısı uzantısı Ekle

OMS Aracısı, kümeye eklemek için en iyi yolu, Azure CLI ile API sanal makine ölçek ayarlanır. Azure CLI henüz ayarlanan yoksa, Azure portalına head üzerinden ve açık bir [bulut Kabuk](../cloud-shell/overview.md) örneği veya [Azure CLI 2.0 yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli).

1. Bulut Kabuk istenen sonra kaynağınız ile aynı abonelikte çalıştığından emin olun. Bu kontrol `az account show` ve "name" değeri, kümenizin abonelik eşleştiğinden emin olun.

2. Portalda, günlük analizi çalışma alanınız bulunduğu kaynak grubuna gidin. (Kaynak türü günlük analizi olacaktır) günlük analizi kaynakta, sağdaki Gezinti içine tıklayın, aşağı kaydırın ve tıklayın **özellikleri**.

    ![Günlük analizi Özellikler sayfası](media/service-fabric-diagnostics-oms-agent/oms-properties.png)

    Not alın, `workspaceId`. 

3. Ayrıca gerekir, `workspaceKey` aracı dağıtmak için. Anahtarını almak için tıklayın **Gelişmiş ayarları**altında *ayarları* sol gezinti bölümü. Tıklayın **Windows sunucuları** Windows Küme, duran varsa ve **Linux sunucuları** Linux kümesi oluşturuyorsanız. İhtiyacınız vardır *birincil anahtar* , görüntülenir, aracıları olarak dağıtmak için `workspaceKey`.

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
 
5. Bu yapılandırma VM örneklerinizi zaten uygulamak için komutu çalıştırın:  

    ```sh
    az vmss update-instances
    ```

    Bu aracı, düğümlere başarıyla eklemek için küçüktür 15 dakika sürer. Aracıları kullanarak eklendiğini doğrulayabilirsiniz `az vmss extension list` API'si:

    ```sh
    az vmss extension list --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType>
    ```

## <a name="add-the-agent-via-the-resource-manager-template"></a>Resource Manager şablonu aracılığıyla aracısı ekleme

Bir Azure günlük analizi çalışma alanı dağıtma ve düğümlerin her birinde sizin için bir aracı ekleyin örnek Resource Manager şablonları için kullanılabilen [Windows](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Windows) veya [Linux](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Linux).

Karşıdan yükle ve en iyi sonucu gereksinimlerinize uyan bir küme dağıtmak için bu şablonu değiştirin.

## <a name="next-steps"></a>Sonraki adımlar

* Toplama ilgili [performans sayaçları](service-fabric-diagnostics-event-generation-perf.md). Belirli bir performans sayaçları toplamak için OMS aracısının yapılandırmak için gözden [veri kaynaklarını yapılandırma](../log-analytics/log-analytics-data-sources.md#configuring-data-sources).
* Ayarlamak için günlük analizi yapılandırma [uyarı otomatik](../log-analytics/log-analytics-alerts.md) algılama ve tanılama yardımcı olmak için
