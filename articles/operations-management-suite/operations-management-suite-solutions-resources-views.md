---
title: Yönetim çözümleri görünümlerde | Microsoft Docs
description: 'Yönetim çözümleri genellikle verileri görselleştirmek için bir veya daha fazla görünümleri içerir.  Bu makalede, Görünüm Tasarımcısı tarafından oluşturulan bir görünüm vermek ve bir yönetim çözümü dahil açıklar. '
services: operations-management-suite
documentationcenter: ''
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 570b278c-2d47-4e5a-9828-7f01f31ddf8c
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/16/2018
ms.author: bwren
ms.openlocfilehash: b44763fe67b1c70c0b6ecdff73c32d8bb4fab3a4
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="views-in-management-solutions-preview"></a>Görünümlerde yönetim çözümleri (Önizleme)
> [!NOTE]
> Bu, şu anda önizlemede olan yönetim çözümleri oluşturmak için başlangıç belgesidir. Aşağıda açıklanan herhangi bir şema değiştirilebilir ' dir.    


[Yönetim çözümleri](operations-management-suite-solutions.md) genellikle verileri görselleştirmek için bir veya daha fazla görünümleri içerir.  Bu makalede tarafından oluşturulan bir görünüm dışarı aktarma [Görünüm Tasarımcısı](../log-analytics/log-analytics-view-designer.md) ve bir yönetim çözümü içerir.  

> [!NOTE]
> Bu makaledeki örnekler parametreleri ve gerekli olduğunu veya yönetim çözümleri için ortak olduğunu ve açıklanan değişkenleri kullanma [tasarım ve yapı Azure Yönetimi çözümünde](operations-management-suite-solutions-creating.md)
>
>

## <a name="prerequisites"></a>Önkoşullar
Bu makale, zaten nasıl hakkında bilgi sahibi olduğunuzu varsayar [bir yönetim çözümü oluşturma](operations-management-suite-solutions-creating.md) ve çözüm dosya yapısı.

## <a name="overview"></a>Genel Bakış
Bir yönetim çözümü bir görünüm eklemek için oluşturduğunuz bir **kaynak** içinde için [çözüm dosyasını](operations-management-suite-solutions-creating.md).  Görünümün ayrıntılı yapılandırma tanımlayan JSON ancak ve bir şey yok tipik çözüm Yazar el ile oluşturmak mümkün olacaktır genellikle karmaşıktır.  Görünümü kullanarak oluşturmak için kullanılan en yaygın yöntem olan [Görünüm Tasarımcısı](../log-analytics/log-analytics-view-designer.md)dışa aktarın ve ardından ayrıntılı yapılandırmasına ekleyin.

Görünüm bir çözüme eklemek için temel adımlar aşağıda belirtilmiştir.  Her adım, aşağıdaki bölümlerde daha ayrıntılı açıklanmıştır.

1. Görünüm bir dosyaya aktarın.
2. Görünüm kaynak çözümde oluşturun.
3. Ayrıntıları Görüntüle ekleyin.

## <a name="export-the-view-to-a-file"></a>Görünüm bir dosyaya dışarı aktarma
Bölümündeki yönergeleri izleyin [günlük analizi Görünüm Tasarımcısı](../log-analytics/log-analytics-view-designer.md) bir görünüm bir dosyaya vermek için.  Dışarı aktarılan dosyayı aynı JSON biçiminde olacaktır [öğeleri çözüm dosyası olarak](operations-management-suite-solutions-solution-file.md).  

**Kaynakları** görünüm dosyası öğe türüne sahip bir kaynak olacaktır **Microsoft.OperationalInsights/workspaces** , günlük analizi çalışma alanını temsil eder.  Bu öğe bir alt öğe türüne sahip olacaktır **görünümleri** görünümü temsil eder ve ayrıntılı yapılandırmasını içerir.  Bu öğenin ayrıntılarını kopyalamanız ve ardından çözümünüze kopyalayın.

## <a name="create-the-view-resource-in-the-solution"></a>Çözümde görünüm kaynağı oluşturma
Aşağıdaki görünüm kaynağa eklemek **kaynakları** çözüm dosyanızın öğesi.  Bu, aynı zamanda eklemelisiniz seçeneklerdir değişkenleri kullanır.  Unutmayın **Pano** ve **OverviewTile** dışarı aktarılan görünüm dosyasından karşılık gelen özelliklerle kılacak yer tutucuları özelliklerdir.

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

Çözüm dosyası değişkenleri öğesine aşağıdaki değişkenleri eklemek ve bu çözümünüz için değerleri değiştirin.

    "LogAnalyticsApiVersion": "<api-version>",
    "ViewAuthor": "Your name."
    "ViewDescription": "Optional description of the view."
    "ViewName": "Provide a name for the view here."

Dışarı aktarılan görünüm dosyanızdan tüm görünüm kaynak kopyalamak, ancak bunu çözümünüzde çalıştırmak aşağıdaki değişiklikleri yapmanız gerekir unutmayın.  

* **Türü** görünüm için kaynak gelen değiştirilmesi gereken **görünümleri** için **Microsoft.OperationalInsights/workspaces**.
* **Adı** görünüm kaynak için özellik çalışma alanı adı içerecek şekilde değiştirilmesi gerekir.
* Çalışma alanı bağımlılığını çalışma kaynak çözümde tanımlı değil bu yana kaldırılması gerekiyor.
* **DisplayName** özelliği görünümüne eklenmesi gerekiyor.  **Kimliği**, **adı**, ve **DisplayName** tüm eşleşmesi gerekir.
* Parametre adları gerekli parametrelerinin eşleşecek şekilde değiştirilmesi gerekir.
* Değişkenleri çözümde tanımlanan ve uygun özelliklerinde kullanılır.

### <a name="log-analytics-api-version"></a>Günlük analizi API sürümü
Resource Manager şablonunda tanımlanan tüm günlük analizi kaynaklarını özelliğine sahip **apiVersion** kaynak kullanması gereken API sürümü tanımlar.  Bu kullanan sorguları görünümlerle için farklı bir sürümdür [eski ve yükseltilmiş sorgu dili](../log-analytics/log-analytics-log-search-upgrade.md).  

 Aşağıdaki tabloda, eski ve yükseltilmiş çalışma alanlarında görünümler için günlük analizi API sürümleri belirtir: 

| Çalışma alanında sürümü | API sürümü | Sorgu |
|:---|:---|:---|
| V1 (eski)   | 2015-11-01-Önizleme | Eski biçimi.<br> Örnek: Yazın olay EventLevelName = hata =  |
| v2 (yükseltme) | 2015-11-01-Önizleme | Eski biçimi.  Yükleme yükseltilmiş biçimine dönüştürülür.<br> Örnek: Yazın olay EventLevelName = hata =<br>Dönüştürülen: olay &#124; burada EventLevelName "Error" ==  |
| v2 (yükseltme) | 2017-03-03-Önizleme | Yükseltme biçimi. <br>Örnek: Olay &#124; burada EventLevelName "Error" ==  |


## <a name="add-the-view-details"></a>Görünüm ayrıntılarını Ekle
Dışarı aktarılan görünüm dosyası görünüm kaynak iki öğelerinde içerecek **özellikleri** adlı öğe **Pano** ve **OverviewTile** ayrıntılı içerir Görünüm yapılandırması.  Bu iki öğenin ve içerikleri içine kopyalamak **özellikleri** çözüm dosyanızdaki görünüm kaynak öğesidir.

## <a name="example"></a>Örnek
Örneğin, aşağıdaki örnek basit çözüm dosyasını bir görünümle gösterir.  Üç nokta (...) için gösterilen **Pano** ve **OverviewTile** alanı nedeniyle içeriği.

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
* Oluşturma ayrıntılarının öğrenin [yönetim çözümleri](operations-management-suite-solutions-creating.md).
* Dahil [Otomasyon runbook'ları yönetim çözümünüzdeki](operations-management-suite-solutions-resources-automation.md).
