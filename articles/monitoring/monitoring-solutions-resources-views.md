---
title: Yönetim çözümleri görünümlerde | Microsoft Docs
description: 'Yönetim çözümleri genellikle verileri görselleştirmek için bir veya daha fazla görünümler içerir.  Bu makalede, Görünüm Tasarımcısı tarafından oluşturulan bir görünüm dışarı aktarma ve Yönetimi çözümünde dahil açıklar. '
services: monitoring
documentationcenter: ''
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 570b278c-2d47-4e5a-9828-7f01f31ddf8c
ms.service: monitoring
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/16/2018
ms.author: bwren
ms.openlocfilehash: 27bec2b7fa53e7564841e6f89be7e4d81a9b9f1a
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50913045"
---
# <a name="views-in-management-solutions-preview"></a>Görünümlerde yönetim çözümleri (Önizleme)
> [!NOTE]
> Şu anda Önizleme aşamasında olan yönetim çözümleri oluşturmak için başlangıç belgeleri budur. Aşağıda açıklanan herhangi bir şema tabi bir değişikliktir.    


[Yönetim çözümleri](monitoring-solutions.md) genellikle verileri görselleştirmek için bir veya daha fazla görünümler içerir.  Bu makalede tarafından oluşturulan bir görünüm dışarı aktarma [Görünüm Tasarımcısı](../log-analytics/log-analytics-view-designer.md) ve Yönetimi çözümünde içerir.  

> [!NOTE]
> Bu makaledeki örnekleri parametreler ve değişkenler gerekli olduğunu veya yönetim çözümleri için yaygın olduğunu ve açıklanan kullanmak [tasarım ve derleme Azure Yönetimi çözümünde](monitoring-solutions-creating.md)
>
>

## <a name="prerequisites"></a>Önkoşullar
Bu makale, zaten nasıl hakkında bilgi sahibi olduğunuzu varsayar [yönetimi çözümü oluşturmak](monitoring-solutions-creating.md) ve çözüm dosya yapısı.

## <a name="overview"></a>Genel Bakış
Görünüm bir yönetim çözümüne dahil etmek için oluşturduğunuz bir **kaynak** içinde için [çözüm dosyası](monitoring-solutions-creating.md).  Görünümün ayrıntılı yapılandırma açıklayan JSON olsa ve bir şey tipik çözüm Yazar el ile oluşturmanız mümkün olacaktır genellikle karmaşıktır.  Görünümünü kullanarak oluşturmak için en yaygın yöntemidir [Görünüm Tasarımcısı](../log-analytics/log-analytics-view-designer.md)dışa aktarın ve ardından ayrıntılı yapılandırmasına ekleyin.

Görünüm bir çözüme eklemek için temel adımlar aşağıdaki gibidir.  Her adım, aşağıdaki bölümlerde daha ayrıntılı açıklanmıştır.

1. Görünüm bir dosyaya dışarı aktarın.
2. Görünüm kaynağı çözüm oluşturun.
3. Ayrıntıları Görüntüle ekleyin.

## <a name="export-the-view-to-a-file"></a>Görünüm bir dosyaya aktarın.
Konumundaki yönergeleri [Log Analytics Görünüm Tasarımcısı](../log-analytics/log-analytics-view-designer.md) görünümü bir dosyaya vermek için.  Dışarı aktarılan dosya JSON biçiminde aynı olacaktır [öğeleri çözüm dosyası olarak](monitoring-solutions-solution-file.md).  

**Kaynakları** görünüm öğesi bir kaynak türünde olacaktır **Microsoft.OperationalInsights/workspaces** , Log Analytics çalışma alanını temsil eder.  Bu öğe türüne sahip bir alt öğesi gerekir **görünümleri** görünümünü temsil eder ve ayrıntılı yapılandırmasını içerir.  Bu öğenin ayrıntıları kopyalayın ve çözümünüze kopyalayın.

## <a name="create-the-view-resource-in-the-solution"></a>Çözümde görünümü kaynak oluşturma
Eklemek için aşağıdaki görünümü kaynak **kaynakları** çözüm dosyanız öğesidir.  Bu, ayrıca eklemelisiniz seçeneklerdir değişkenleri kullanır.  Unutmayın **Pano** ve **OverviewTile** dışarı aktarılan görünümü dosyasından karşılık gelen özelliklerle üzerine yazılacak yer tutucuları özelliklerdir.

    {
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
        "type": "Microsoft.OperationalInsights/workspaces/views",
        "location": "[parameters('workspaceregionId')]",
        "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
        "dependson": [
            ],
        "properties": {
            "Id": "[variables('ViewName')]",
            "Name": "[variables('ViewName')]",
            "DisplayName": "[variables('ViewName')]",
            "Description": "",
            "Author": "[variables('ViewAuthor')]",
            "Source": "Local",
            "Dashboard": ,
            "OverviewTile":
        }
    }

Çözüm dosyasının değişkenleri öğeye aşağıdaki değişkenleri eklemek ve çözümünüz için bu değerleri değiştirin.

    "LogAnalyticsApiVersion": "<api-version>",
    "ViewAuthor": "Your name."
    "ViewDescription": "Optional description of the view."
    "ViewName": "Provide a name for the view here."

Tüm görünüm kaynağı verilen görünümü dosyanızdan Kopyala, ancak çözümünüzde çalışabilmesi için aşağıdaki değişiklikleri yapmanız unutmayın.  

* **Türü** görünüm için kaynak gelen değiştirilmesi gereken **görünümleri** için **Microsoft.OperationalInsights/workspaces**.
* **Adı** görünümü kaynak için özellik çalışma alanı adını içerecek şekilde değiştirilmesi gerekir.
* Bağımlılık çalışma alanının çalışma alanı kaynağına çözümde tanımlanmamış beri kaldırılması gerekiyor.
* **DisplayName** özelliği görünüme eklenmesi gerekiyor.  **Kimliği**, **adı**, ve **DisplayName** tüm eşleşmesi gerekir.
* Parametre adları, gerekli parametreleri eşleşecek şekilde değiştirilmelidir.
* Değişkenleri çözümde tanımlanan ve uygun özellikler kullanılır.

### <a name="log-analytics-api-version"></a>Günlük analizi API'si sürümü
Bir Resource Manager şablonunda tanımlı tüm Log Analytics kaynakları bir özelliği olan **apiVersion** kullanması gereken kaynak API sürümünü tanımlar.  Bu görünümlerle kullanan sorguları için farklı bir sürümdür [eski ve yükseltilen sorgu dili](../log-analytics/log-analytics-log-search.md).  

 Aşağıdaki tabloda, eski ve yükseltilen çalışma alanlarında görünümler için Log Analytics API sürümleri belirtir: 

| Çalışma alanı sürümü | API sürümü | Sorgu |
|:---|:---|:---|
| V1 (eski)   | 2015-11-01-Önizleme | Eski biçimi.<br> Örnek: Türü olay EventLevelName = hata =  |
| v2 (yükseltilmiş) | 2015-11-01-Önizleme | Eski biçimi.  Yüklemede yükseltilen biçimine dönüştürülür.<br> Örnek: Türü olay EventLevelName = hata =<br>Dönüştürülen: olay &#124; burada EventLevelName "Error" ==  |
| v2 (yükseltilmiş) | 2017-03-03-Önizleme | Yükseltme biçimi. <br>Örnek: Olay &#124; burada EventLevelName "Error" ==  |


## <a name="add-the-view-details"></a>Ayrıntıları Ekle
Dışarı aktarılan görünüm dosyası görünümü kaynak iki öğeyi de içerecek **özellikleri** adlı bir öğe **Pano** ve **OverviewTile** ayrıntılı içerir Görünüm yapılandırması.  Bu iki öğe ve bunların içeriğini içine kopyalayın **özellikleri** çözüm dosyanız görünümü kaynak öğesi.

## <a name="example"></a>Örnek
Örneğin, aşağıdaki örnek, bir basit çözüm dosyası bir görünümünü gösterir.  Üç nokta (...) için gösterilen **Pano** ve **OverviewTile** alanı nedeniyle içeriği.

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspaceName": {
                "type": "string"
            },
            "accountName": {
                "type": "string"
            },
            "workspaceRegionId": {
                "type": "string"
            },
            "regionId": {
                "type": "string"
            },
            "pricingTier": {
                "type": "string"
            }
        },
        "variables": {
            "SolutionVersion": "1.1",
            "SolutionPublisher": "Contoso",
            "SolutionName": "Contoso Solution",
            "LogAnalyticsApiVersion": "2015-11-01-preview",
            "ViewAuthor":  "user@contoso.com",
            "ViewDescription":  "This is a sample view.",
            "ViewName":  "Contoso View"
        },
        "resources": [
            {
                "name": "[concat(variables('SolutionName'), '(' ,parameters('workspacename'), ')')]",
                "location": "[parameters('workspaceRegionId')]",
                "tags": { },
                "type": "Microsoft.OperationsManagement/solutions",
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspacename'), '/views/', variables('ViewName'))]"
                ],
                "properties": {
                    "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspacename'))]",
                    "referencedResources": [
                    ],
                    "containedResources": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
                    ]
                },
                "plan": {
                    "name": "[concat(variables('SolutionName'), '(' ,parameters('workspaceName'), ')')]",
                    "Version": "[variables('SolutionVersion')]",
                    "product": "ContosoSolution",
                    "publisher": "[variables('SolutionPublisher')]",
                    "promotionCode": ""
                }
            },
            {
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
                "type": "Microsoft.OperationalInsights/workspaces/views",
                "location": "[parameters('workspaceregionId')]",
                "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
                "dependson": [
                ],
                "properties": {
                    "Id": "[variables('ViewName')]",
                    "Name": "[variables('ViewName')]",
                    "DisplayName": "[variables('ViewName')]",
                    "Description": "[variables('ViewDescription')]",
                    "Author": "[variables('ViewAuthor')]",
                    "Source": "Local",
                    "Dashboard": ...,
                    "OverviewTile": ...
                }
            }
          ]
    }




## <a name="next-steps"></a>Sonraki adımlar
* Oluşturma tüm ayrıntıları öğrenin [yönetim çözümleri](monitoring-solutions-creating.md).
* Dahil [yönetim çözümünüzü Otomasyon runbook'ları](monitoring-solutions-resources-automation.md).
