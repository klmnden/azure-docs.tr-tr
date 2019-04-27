---
title: Azure'da bir yönetim çözümü dosyası oluşturma | Microsoft Docs
description: Müşteriler, Azure ortamına ekleyebileceğiniz paket Yönetimi senaryoları yönetim çözümleri sunar.  Bu makale, kendi ortamınızda kullanılacak yönetim çözümleri nasıl oluşturabileceğiniz hakkında ayrıntılar sağlar veya müşterileriniz için kullanılabilir.
services: monitoring
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1915e204-ba7e-431b-9718-9eb6b4213ad8
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/09/2018
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4e5c27911fe86a6916235014f8602327df929e20
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60595760"
---
# <a name="creating-a-management-solution-file-in-azure-preview"></a>Azure'da (Önizleme) bir yönetim çözümü dosyası oluşturma
> [!NOTE]
> Şu anda Önizleme aşamasında olan Azure yönetim çözümleri oluşturmak için başlangıç belgeleri budur. Aşağıda açıklanan herhangi bir şema tabi bir değişikliktir.  

Azure yönetim çözümlerine olarak gerçekleştirilen [Resource Manager şablonları](../../azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal.md).  Yönetim çözümleri Yazar öğrenmek, ana görevin öğrenme nasıl [şablon yazma](../../azure-resource-manager/resource-group-authoring-templates.md).  Bu makale, çözümler ve tipik bir çözüm kaynaklarını nasıl yapılandıracağınızı öğrenmek için kullanılan şablonları benzersiz ayrıntılarını sağlar.


## <a name="tools"></a>Araçlar

Çözüm dosyaları ile çalışmak için herhangi bir metin düzenleyicisi kullanabilirsiniz, ancak aşağıdaki makaleler de açıklandığı gibi Visual Studio veya Visual Studio Code içinde sağlanan özelliklerden yararlanarak öneririz.

- [Oluşturma ve Visual Studio aracılığıyla Azure kaynak grupları dağıtma](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)
- [Visual Studio code'da Azure Resource Manager şablonları ile çalışma](../../azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal.md)




## <a name="structure"></a>Yapı
Bir yönetim çözümü dosyasının temel yapısı ile aynı olan bir [Resource Manager şablonu](../../azure-resource-manager/resource-group-authoring-templates.md#template-format), olduğu gibi.  Üst düzey öğeleri ve bunların içeriğini bir çözümde aşağıdaki bölümlerde açıklanmıştır.  

    {
       "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

## <a name="parameters"></a>Parametreler
[Parametreleri](../../azure-resource-manager/resource-group-authoring-templates.md#parameters) yönetimi çözümü yükledikleri sırada, kullanıcıdan gerektiren değerlerdir.  Tüm çözümler olan standart parametreler vardır ve ek parametreler gerektiği gibi belirli çözümünüz için ekleyebilirsiniz.  Çözümünüzü yükledikleri sırada kullanıcılar parametre değerlerini nasıl sağlayacak, belirli bir parametre ve çözümün nasıl yüklendiği bağlı olacaktır.

Bir kullanıcı [yönetim çözümünüzü yükleyen](solutions.md#install-a-monitoring-solution) Azure Market veya Azure hızlı başlangıç şablonları bunlar seçmeniz istenirse bir [Log Analytics çalışma alanını ve Otomasyon hesabı](solutions.md#log-analytics-workspace-and-automation-account).  Bu değerlerin her birinin standart parametreler doldurmak için kullanılır.  Doğrudan standart parametreler için değerler sağlamak için kullanıcıya sorulmaz, ancak ek parametreler için değer sağlamanız istenir.


Bir örnek parametre aşağıda gösterilmiştir.  

    "startTime": {
        "type": "string",
        "metadata": {
            "description": "Enter time for starting VMs by resource group.",
            "control": "datetime",
            "category": "Schedule"
        }

Aşağıdaki tabloda, bir parametre özniteliklerini açıklar.

| Öznitelik | Açıklama |
|:--- |:--- |
| type |Parametresi için veri türü. Kullanıcı için görüntülenen giriş denetiminin veri türüne bağlıdır.<br><br>bool - açılan kutusu<br>dize - metin kutusu<br>int - metin kutusu<br>SecureString - parola alanı<br> |
| category |Kategori parametresi isteğe bağlı.  Aynı kategoride parametreleri birlikte gruplandırılır. |
| Denetimi |Dize parametreleri için ek işlevsellik sağlar.<br><br>DateTime - Datetime denetimi görüntülenir.<br>GUID - GUID değeri otomatik olarak oluşturulur ve parametre görüntülenmez. |
| açıklama |Parametresi için isteğe bağlı bir açıklama.  Bir parametrenin yanındaki bilgi balonunda görüntülenir. |

### <a name="standard-parameters"></a>Standart Parametreler
Aşağıdaki tabloda, tüm yönetim çözümleri için standart parametreler listelenmektedir.  Bu değerler, çözümünüzü Azure Marketi'nde ya da hızlı başlangıç şablonları yüklendiğinde, bunların istenmesi yerine kullanıcının doldurulur.  Çözüm başka bir yöntemle yüklüyse kullanıcı bunlar için değer sağlamalısınız.

> [!NOTE]
> Hızlı Başlangıç şablonları ve Azure Market kullanıcı arabiriminin parametre adları tabloda bekliyor.  Farklı parametre adları kullanıyorsa kullanıcı bunlar için istenir ve bunların değil otomatik olarak doldurulur.
>
>

| Parametre | Tür | Açıklama |
|:--- |:--- |:--- |
| accountName |string |Azure Otomasyon hesabı adı. |
| pricingTier |string |Log Analytics çalışma alanı hem de Azure Otomasyon hesabı fiyatlandırma katmanı. |
| RegionID |string |Azure Otomasyonu hesabı bölgesi. |
| SolutionName |string |Çözüm adı.  Çözümünüzü hızlı başlangıç şablonları aracılığıyla dağıtıyorsanız, bunun yerine bir belirtmesini gerektiren bir dize tanımlayabilirsiniz böylece daha sonra solutionName parametre olarak tanımlamanız gerekir. |
| workspaceName |string |Log Analytics çalışma alanı adı. |
| workspaceRegionId |string |Log Analytics çalışma alanı bölgesi. |


Yapısı, çözüm dosyasına kopyalayıp standart Parametreler aşağıda verilmiştir.  

    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "description": "A valid Log Analytics workspace name"
            }
        },
        "accountName": {
               "type": "string",
               "metadata": {
                   "description": "A valid Azure Automation account name"
               }
        },
        "workspaceRegionId": {
               "type": "string",
               "metadata": {
                   "description": "Region of the Log Analytics workspace"
            }
        },
        "regionId": {
            "type": "string",
            "metadata": {
                "description": "Region of the Azure Automation account"
            }
        },
        "pricingTier": {
            "type": "string",
            "metadata": {
                "description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
        }
    }


Parametre değerlerini söz dizimi ile çözümün diğer öğeleri başvurduğu **parametreleri ('parametre adı')**.  Örneğin, çalışma alanı adı erişmek için kullanacağınız **parameters('workspaceName')**

## <a name="variables"></a>Değişkenler
[Değişkenleri](../../azure-resource-manager/resource-group-authoring-templates.md#variables) yönetimi çözümü geri kalanında kullanacağınız değerlerdir.  Bu değerler, çözüm yükleme kullanıcıya sunulmaz.  Yazar birden çok kez çözümün kullanılabilir değerleri yönetebileceği tek bir konum sağlamak için tasarlanmıştır. Herhangi bir değeri belirli çözümünüze bunları kodlamak yerine değişkenlerine koymalısınız **kaynakları** öğesi.  Bu kod daha okunabilir hale getirir ve sonraki sürümlerinde bu değerleri kolayca değiştirmenize izin verir.

Aşağıdaki örneği verilmiştir bir **değişkenleri** çözümlerinde kullanılan tipik parametrelerle öğesi.

    "variables": {
        "SolutionVersion": "1.1",
        "SolutionPublisher": "Contoso",
        "SolutionName": "My Solution",
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

Değişken değerleri söz dizimi ile çözüm aracılığıyla başvurmanız **değişkenleri ('değişken adı')**.  Örneğin, SolutionName değişkeni erişmek için kullanacağınız **variables('SolutionName')**.

Ayrıca, karmaşık değişkenleri tanımlayabilirsiniz, birden çok değerlerini ayarlar.  Burada farklı kaynak türleri için birden çok özellik tanımlama bu yönetim çözümlerine özellikle yararlı olur.  Örneğin, yukarıda gösterilen aşağıdaki çözüm değişkenlerini yeniden yapılandırma.

    "variables": {
        "Solution": {
          "Version": "1.1",
          "Publisher": "Contoso",
          "Name": "My Solution"
        },
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

Bu durumda, söz dizimi ile çözüm aracılığıyla değişken değerleri başvurmanız **variables('variable name').property**.  Örneğin, çözüm adı değişkeni erişmek için kullanacağınız **variables('Solution'). Ad**.

## <a name="resources"></a>Kaynaklar
[Kaynakları](../../azure-resource-manager/resource-group-authoring-templates.md#resources) yönetim çözümünüzü yükleyecekleri ve yapılandıracakları farklı kaynakları tanımlayın.  Bu şablon en büyük ve en karmaşık kısmı olacaktır.  Kaynak öğelerin eksiksiz bir açıklaması ve yapısı alabilirsiniz [Azure Resource Manager şablonları yazma](../../azure-resource-manager/resource-group-authoring-templates.md#resources).  Tipik tanımlayacak farklı kaynaklar diğer makalelerde, bu belgede ayrıntılı olarak belirtilir. 


### <a name="dependencies"></a>Bağımlılıklar
**DependsOn** öğeyi belirten bir [bağımlılık](../../azure-resource-manager/resource-group-define-dependencies.md) başka bir kaynak üzerinde.  Çözüm yüklendikten sonra bir kaynak tüm bağımlılıklarını oluşturulmuş kadar oluşturulmaz.  Örneğin, çözümünüz olabilir [runbook başlatma](solutions-resources-automation.md#runbooks) kullanarak yüklendiğinde bir [proje kaynak](solutions-resources-automation.md#automation-jobs).  Proje kaynak proje oluşturulmadan önce runbook oluşturulduğundan emin olmak için runbook kaynağına bağlı olacaktır.

### <a name="log-analytics-workspace-and-automation-account"></a>Log Analytics çalışma alanı ve Otomasyon hesabı
Yönetim çözümleri gerektiren bir [Log Analytics çalışma alanı](../../azure-monitor/platform/manage-access.md) görünümler içermesi ve [Otomasyon hesabı](../../automation/automation-security-overview.md#automation-account-overview) runbook'ları ve ilgili kaynakları içerecek şekilde.  Çözüm kaynakları oluşturulur ve çözümde tanımlanmamalıdır önce bunlar kullanılabilir olmalıdır.  Kullanıcının [bir çalışma alanı ve hesabı belirtin](solutions.md#log-analytics-workspace-and-automation-account) zaman çözümünüzü dağıttıkları ancak yazar olarak aşağıdaki noktaları dikkate almanız gerekir.


## <a name="solution-resource"></a>Çözüm kaynak
Bir kaynak giriş her çözüm gerektiren **kaynakları** çözüm tanımlayan öğe.  Bu bir türüne sahip **Microsoft.OperationsManagement/solutions** ve aşağıdaki yapıya sahiptir. Bu içerir [standart parametreler](#parameters) ve [değişkenleri](#variables) , genellikle çözüm özelliklerini tanımlamak için kullanılır.


    {
      "name": "[concat(variables('Solution').Name, '[' ,parameters('workspaceName'), ']')]",
      "location": "[parameters('workspaceRegionId')]",
      "tags": { },
      "type": "Microsoft.OperationsManagement/solutions",
      "apiVersion": "[variables('LogAnalyticsApiVersion')]",
      "dependsOn": [
        <list-of-resources>
      ],
      "properties": {
        "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName'))]",
        "referencedResources": [
            <list-of-referenced-resources>
        ],
        "containedResources": [
            <list-of-contained-resources>
        ]
      },
      "plan": {
        "name": "[concat(variables('Solution').Name, '[' ,parameters('workspaceName'), ']')]",
        "Version": "[variables('Solution').Version]",
        "product": "[variables('ProductName')]",
        "publisher": "[variables('Solution').Publisher]",
        "promotionCode": ""
      }
    }




### <a name="dependencies"></a>Bağımlılıklar
Çözüm kaynak olmalıdır bir [bağımlılık](../../azure-resource-manager/resource-group-define-dependencies.md) bunlar çözüm oluşturulabilmesi için önce mevcut olması gerektiğinden, çözümdeki diğer her kaynaktaki.  Her kaynak için bir giriş ekleyerek bunu **dependsOn** öğesi.

### <a name="properties"></a>Özellikler
Çözüm kaynak aşağıdaki tabloda özelliklerine sahiptir.  Bu, başvurulan ve çözüm yüklendikten sonra kaynak nasıl yönetileceğini tanımlar çözümüyle yer alan kaynakları içerir.  Çözümdeki her bir kaynak ya da listelenmelidir **referencedResources** veya **containedResources** özelliği.

| Özellik | Açıklama |
|:--- |:--- |
| workspaceResourceId |Log Analytics çalışma formundaki Kimliğini  *\<kaynak grubu kimliği > /providers/Microsoft.OperationalInsights/workspaces/\<çalışma alanı adı\>*. |
| referencedResources |Çözüm kaldırıldığında kaldırılmamalıdır çözümü kaynakların listesi. |
| containedResources |Çözümdeki çözüm kaldırıldığında veritabanınızdan kaldırılması gereken kaynakların listesi. |

Yukarıdaki örnekte, bir runbook, zamanlama ve görünümü ile çözüm içindir.  Runbook ve zamanlama olan *başvurulan* içinde **özellikleri** çözüm kaldırıldığında kaldırılmazlar şekilde öğesi.  Görünüm *bulunan* çözüm kaldırıldığında kaldırılır.

### <a name="plan"></a>Planlama
**Planı** çözüm kaynak varlık, aşağıdaki tabloda özelliklere sahiptir.

| Özellik | Açıklama |
|:--- |:--- |
| ad |Çözüm adı. |
| version |Yazar tarafından belirlendiği şekilde bu çözümün sürümü. |
| ürün |Çözüm tanımlamak için benzersiz bir dize. |
| Yayımcı |Çözümün yayımcısı. |



## <a name="next-steps"></a>Sonraki adımlar
* [Kaydedilmiş aramaları ve Uyarıları Ekle](solutions-resources-searches-alerts.md) Yönetimi çözümünüz için.
* [Görünümler ekleme](solutions-resources-views.md) Yönetimi çözümünüz için.
* [Runbook'ları ve diğer Otomasyon kaynaklarına ekleme](solutions-resources-automation.md) Yönetimi çözümünüz için.
* Ayrıntılarını öğrenmek [Azure Resource Manager şablonları yazma](../../azure-resource-manager/resource-group-authoring-templates.md).
* Arama [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/documentation/templates) farklı Resource Manager şablonu örnekleri için.
