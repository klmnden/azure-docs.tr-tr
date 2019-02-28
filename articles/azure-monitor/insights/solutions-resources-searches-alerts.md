---
title: Yönetim çözümlerine kayıtlı aramalar | Microsoft Docs
description: Yönetim çözümleri genellikle çözüm tarafından toplanan verileri çözümlemek için Log analytics'te kayıtlı aramalar içerir. Bu makalede, Log Analytics yönetim çözümleri eklenmesi için bir Resource Manager şablonunda kayıtlı aramalar tanımlanacağını açıklar.
services: monitoring
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2019
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 64b79d613463ac2097a89a8e3ca3870b885a3332
ms.sourcegitcommit: 1afd2e835dd507259cf7bb798b1b130adbb21840
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2019
ms.locfileid: "56985266"
---
# <a name="adding-log-analytics-saved-searches-to-management-solution-preview"></a>Yönetim çözümü (Önizleme) ekleyerek bir Log Analytics kayıtlı aramalar

> [!IMPORTANT]
> Resource Manager şablonu kullanarak bir uyarı oluşturma hakkında ayrıntılar bu makalenin önceki bir sürümü dahil. Bu bilgiler olan / tarihi artık [Log Analytics uyarılarını, Azure İzleyici Genişletilmiş](../platform/alerts-extend.md). Resource Manager şablonu ile günlük uyarısı oluşturma hakkında daha fazla bilgi edinmek için bkz: [Azure kaynak şablonu kullanarak yönetme günlük uyarıları](../platform/alerts-log.md#managing-log-alerts-using-azure-resource-template).

> [!NOTE]
> Şu anda Önizleme aşamasında olan yönetim çözümleri oluşturmak için başlangıç belgeleri budur. Aşağıda açıklanan herhangi bir şema tabi bir değişikliktir.

Bu makalede, Log Analytics kayıtlı aramaları tanımlamak açıklanır bir [kaynak yönetimi şablonu](../../azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal.md) olarak eklenebilir böylece [yönetim çözümleri](solutions-creating.md).

> [!NOTE]
> Bu makaledeki örnekleri parametreler ve değişkenler gerekli olduğunu veya yönetim çözümleri için yaygın olduğunu ve açıklanan kullanmak [tasarım ve derleme Azure Yönetimi çözümünde](solutions-creating.md)

## <a name="prerequisites"></a>Önkoşullar
Bu makale, zaten nasıl hakkında bilgi sahibi olduğunuzu varsayar [yönetimi çözümü oluşturmak](solutions-creating.md) ve yapısı bir [Resource Manager şablonu](../../azure-resource-manager/resource-group-authoring-templates.md) ve çözüm dosyası.


## <a name="log-analytics-workspace"></a>Log Analytics Çalışma Alanı
Log Analytics tüm kaynaklarda bulunan bir [çalışma](../../azure-monitor/platform/manage-access.md). Bölümünde anlatıldığı gibi [Log Analytics çalışma alanını ve Otomasyon hesabı](solutions.md#log-analytics-workspace-and-automation-account), çalışma yönetimi çözümünde dahil değildir, ancak çözüm yüklenmeden önce mevcut olması gerekir. Ardından, kullanılabilir durumda değilse, çözüm yükleme başarısız olur.

Çalışma alanı adına her bir Log Analytics kaynak adıdır. Bu çözüm ile gerçekleştirilir **çalışma** SavedSearch kaynağının aşağıdaki örnekteki gibi parametre.

    "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearchId'))]"

## <a name="log-analytics-api-version"></a>Günlük analizi API'si sürümü
Bir Resource Manager şablonunda tanımlı tüm Log Analytics kaynakları bir özelliği olan **apiVersion** kullanması gereken kaynak API sürümünü tanımlar.

Aşağıdaki tabloda, bu örnekte kullanılan kaynak için API sürümü listeler.

| Kaynak türü | API sürümü | Sorgu |
|:---|:---|:---|
| savedSearches | 2017-03-15-Önizleme | Olay &#124; burada EventLevelName "Error" ==  |


## <a name="saved-searches"></a>Kayıtlı Aramalar
Dahil [kayıtlı aramalar](../../azure-monitor/log-query/log-query-overview.md) çözümünüz tarafından toplanan verileri sorgulamak için kullanıcıların bir çözümde. Kayıtlı aramalar altında görünen **kayıtlı aramalar** Azure portalında. Kayıtlı bir aramayı, her uyarı için de gereklidir.

[Log Analytics kayıtlı araması](../../azure-monitor/log-query/log-query-overview.md) kaynaklara sahip bir tür `Microsoft.OperationalInsights/workspaces/savedSearches` ve aşağıdaki yapıya sahiptir. Kopyalayabilir ve bu kod parçacığı, çözüm dosyasına yapıştırın ve parametre adlarını değiştirmek için bu genel değişkenler ve parametreler içerir.

    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
        ],
        "tags": { },
        "properties": {
            "etag": "*",
            "query": "[variables('SavedSearch').Query]",
            "displayName": "[variables('SavedSearch').DisplayName]",
            "category": "[variables('SavedSearch').Category]"
        }
    }

Kayıtlı bir aramayı her bir özellik aşağıdaki tabloda açıklanmıştır.

| Özellik | Açıklama |
|:--- |:--- |
| category | Kayıtlı arama için kategori.  Konsolunda birlikte gruplanır, böylece aynı çözüm içindeki tüm kayıtlı aramalar genellikle tek bir kategori paylaşın. |
| DisplayName | Portalı'nda kayıtlı arama için görüntülenecek ad. |
| sorgu | Çalıştırılacak sorgu. |

> [!NOTE]
> JSON olarak yorumlanabilecek karakterler içeriyorsa, kaçış karakterleri sorguda kullanmanız gerekebilir. Örneğin, sorgunuz varsa **AzureActivity | OperationName:"Microsoft.Compute/virtualMachines/write"**, çözüm dosyasındaki yazılmalıdır **AzureActivity | OperationName: /\"Microsoft.Compute/virtualMachines/write\"**.


## <a name="next-steps"></a>Sonraki adımlar
* [Görünümler ekleme](solutions-resources-views.md) Yönetimi çözümünüz için.
* [Otomasyon runbook'ları ve diğer kaynaklar ekleme](solutions-resources-automation.md) Yönetimi çözümünüz için.
