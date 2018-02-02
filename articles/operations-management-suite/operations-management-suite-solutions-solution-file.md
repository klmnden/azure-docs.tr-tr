---
title: "Azure'da bir yönetim çözümü dosyası oluşturma | Microsoft Docs"
description: "Yönetim çözümleri müşteriler kendi Azure ortamına ekleyebileceğiniz paket Yönetimi senaryoları sağlar.  Bu makalede, kendi ortamınızda kullanılacak yönetim çözümleri nasıl oluşturabileceğinizi hakkında ayrıntılar sağlar veya müşterileriniz için kullanılabilir."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1915e204-ba7e-431b-9718-9eb6b4213ad8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/09/2018
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d896fb7c5ffed5c0fe338c2d2f1ef864aacd6f79
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="creating-a-management-solution-file-in-azure-preview"></a>Azure (Önizleme) bir yönetim çözümü dosyası oluşturma
> [!NOTE]
> Bu, şu anda önizlemede olan Azure yönetim çözümleri oluşturmak için başlangıç belgesidir. Aşağıda açıklanan herhangi bir şema değiştirilebilir ' dir.  

Azure yönetim çözümlerine olarak gerçekleştirilen [Resource Manager şablonları](../azure-resource-manager/resource-manager-template-walkthrough.md).  Yönetim çözümleri yazmak öğrenme için ana görev öğrenme nasıl [şablon Yazar](../azure-resource-manager/resource-group-authoring-templates.md).  Bu makale şablonları çözümleri ve nasıl tipik çözüm kaynaklarını yapılandırmak için kullanılan benzersiz ayrıntılarını sağlar.


## <a name="tools"></a>Araçlar

Çözüm dosyaları ile çalışmak için herhangi bir metin düzenleyicisi kullanabilirsiniz, ancak Visual Studio veya Visual Studio Code aşağıdaki makalelerde açıklanan sağlanan özellikler yararlanarak öneririz.

- [Oluşturma ve Visual Studio üzerinden Azure kaynak gruplarını dağıtma](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)
- [Visual Studio code'da Azure Resource Manager şablonları ile çalışma](../azure-resource-manager/resource-manager-vs-code.md)




## <a name="structure"></a>yapısı
Bir yönetim çözümü dosyasının temel yapısı aynıdır bir [Resource Manager şablonu](../azure-resource-manager/resource-group-authoring-templates.md#template-format), olduğu gibi.  Aşağıdaki bölümlerde, üst düzey öğeleri ve içeriklerini bir çözümde açıklar.  

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

## <a name="parameters"></a>Parametreler
[Parametreleri](../azure-resource-manager/resource-group-authoring-templates.md#parameters) yönetim çözümü yüklediğinizde, kullanıcıdan gerektiren değerlerdir.  Tüm çözümleri olan standart parametreler vardır ve ek parametreler gerektiği gibi belirli çözümünüz için ekleyebilirsiniz.  Çözümünüzü yüklediğinizde kullanıcılar parametre değerlerini nasıl sağlayacak belirli parametre ve çözümün nasıl yüklendiği değişir.

Kullanıcı Yönetimi çözümünüz aracılığıyla yüklendiğinde [Azure Marketi](operations-management-suite-solutions.md#finding-and-installing-management-solutions) veya [Azure hızlı başlangıç şablonlarını](operations-management-suite-solutions.md#finding-and-installing-management-solutions) seçim yapması istenir bir [günlük analizi çalışma alanı ve Otomasyon Hesap](operations-management-suite-solutions.md#log-analytics-workspace-and-automation-account).  Bunlar, her bir standart parametrelerinin değerleri doldurmak için kullanılır.  Kullanıcının doğrudan standart parametrelerin değerlerini sağlamasını istenmez, ancak bunlar ek parametreler için değerler sağlamanız istenir.


Kullanıcı çözümünüzü yüklendiğinde [başka bir yöntem](operations-management-suite-solutions.md#finding-and-installing-management-solutions), tüm standart parametreleri ve tüm ek parametreleri için bir değer girmelisiniz.

Bir örnek parametre aşağıda gösterilmiştir.  

    "startTime": {
        "type": "string",
        "metadata": {
            "description": "Enter time for starting VMs by resource group.",
            "control": "datetime",
            "category": "Schedule"
        }

Aşağıdaki tabloda bir parametre öznitelikleri açıklanmaktadır.

| Öznitelik | Açıklama |
|:--- |:--- |
| type |Parametre veri türü. Kullanıcı için görüntülenen giriş denetimi veri türüne bağlıdır.<br><br>bool - açılan kutuya<br>String - metin kutusu<br>int - metin kutusu<br>SecureString - parola alanı<br> |
| category |Parametre isteğe bağlı kategorisi.  Aynı kategoride parametreleri birlikte gruplandırılır. |
| denetimi |Ek işlevsellik dizesi parametreleri için.<br><br>DateTime - Datetime denetim görüntülenir.<br>GUID - GUID değeri otomatik olarak oluşturulur ve parametre görüntülenmez. |
| açıklama |Parametre isteğe bağlı bir açıklama.  Bir bilgi balonu parametresi yanında görüntülenir. |

### <a name="standard-parameters"></a>Standart Parametreler
Aşağıdaki tabloda, tüm yönetim çözümleri için standart parametreleri listeler.  Bu değerleri çözümünüzü Azure Marketi ya da Quickstart şablondan yüklendiğinde bunların istenmesi yerine kullanıcı için doldurulur.  Çözüm başka bir yöntemle yüklüyse, kullanıcı bunlar için değer sağlamalısınız.

> [!NOTE]
> Hızlı Başlangıç şablonları ve Azure Marketi kullanıcı arabiriminde parametre adları tabloda bekliyor.  Farklı parametre adları kullanırsanız, sonra kullanıcı bunlar için istenir ve bunların değil otomatik olarak doldurulur.
>
>

| Parametre | Tür | Açıklama |
|:--- |:--- |:--- |
| accountName |dize |Azure Otomasyon hesabı adı. |
| pricingTier |dize |Hem günlük analizi çalışma alanı hem de Azure Otomasyonu hesabı fiyatlandırma katmanı. |
| regionId |dize |Azure Otomasyonu hesabı bölgesi. |
| solutionName |dize |Çözüm adı.  Çözümünüzü hızlı başlangıç şablonlarıyla dağıtıyorsanız, bunun yerine bir tane belirtmek kullanıcının gerektiren bir dize tanımlamak için daha sonra solutionName parametre olarak tanımlamanız gerekir. |
| workspaceName |dize |Günlük analizi çalışma alanı adı. |
| workspaceRegionId |dize |Günlük analizi çalışma alanı bölgesi. |


Çözüm dosyanıza kopyalayıp standart parametreleri yapısını aşağıdadır.  

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


Parametre değerleri sözdizimiyle çözümün diğer öğelerinde başvurmak **parametreleri ('parametre adı')**.  Örneğin, çalışma alanı adı erişmek için kullanacağınız **parameters('workspaceName')**

## <a name="variables"></a>Değişkenler
[Değişkenleri](../azure-resource-manager/resource-group-authoring-templates.md#variables) yönetim çözümü geri kalanı kullanacağınız değerlerdir.  Bu değerleri çözüm yükleme kullanıcıya sunulmaz.  Birden çok kez çözümün kullanılabilir değerler yönetebileceği tek bir konuma yazar sağlamak için tasarlanmıştır. Herhangi bir değere belirli bunları kodlamak aksine değişkenlerine çözümünüze yerleştirileceği **kaynakları** öğesi.  Bu kodu daha okunabilir hale getirir ve sonraki sürümlerinde bu değerleri kolayca değiştirmenizi sağlar.

Aşağıdaki örneği verilmiştir bir **değişkenleri** çözümlerinde kullanılan tipik parametrelerle öğesi.

    "variables": {
        "SolutionVersion": "1.1",
        "SolutionPublisher": "Contoso",
        "SolutionName": "My Solution",
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

Değişken değerleri sözdizimiyle çözüm aracılığıyla başvurmak **değişkenleri ('değişkeni')**.  Örneğin, SolutionName değişkeni erişmek için kullanacağınız **variables('SolutionName')**.

Ayrıca, karmaşık değişkenleri tanımlayabilirsiniz, birden çok değerlerini ayarlar.  Burada farklı türdeki kaynakların için birden çok özellik tanımlama bu yönetim çözümlerine özellikle yararlı olur.  Örneğin, yukarıda aşağıdaki gösterilen çözüm değişkenleri yapılandırmayı.

    "variables": {
        "Solution": {
          "Version": "1.1",
          "Publisher": "Contoso",
          "Name": "My Solution"
        },
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

Bu durumda, söz dizimi çözümüyle değişken değerlerini başvuruda **variables('variable name').property**.  Örneğin, çözüm adı değişkeni erişmek için kullanacağınız **variables('Solution'). Ad**.

## <a name="resources"></a>Kaynaklar
[Kaynakları](../azure-resource-manager/resource-group-authoring-templates.md#resources) Yönetimi çözümünüzü yükleyecekleri ve yapılandıracakları farklı kaynakları tanımlayın.  Bu şablon en büyük ve en karmaşık kısmı olacaktır.  Kaynak öğelerin tam bir açıklaması ve yapısı elde edebilirsiniz [Azure Resource Manager şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md#resources).  Genellikle tanımlayacaksınız farklı kaynaklar bu belgede diğer makalelerinde ayrıntılı olarak belirtilir. 


### <a name="dependencies"></a>Bağımlılıklar
**DependsOn** öğesi belirttiğinden bir [bağımlılık](../azure-resource-manager/resource-group-define-dependencies.md) başka bir kaynak üzerinde.  Çözüm yüklendiğinde, tüm bağımlılıkları oluşturulmuş kadar kaynak oluşturulmaz.  Örneğin, çözümünüzü olabilir [bir runbook başlatın](operations-management-suite-solutions-resources-automation.md#runbooks) kullanarak yüklendiğinde bir [işi kaynak](operations-management-suite-solutions-resources-automation.md#automation-jobs).  İş kaynak iş oluşturulmadan önce runbook oluşturulduğundan emin olmak için runbook kaynağına bağlı olacaktır.

### <a name="log-analytics-workspace-and-automation-account"></a>Günlük analizi çalışma alanı ve Automation hesabı
Yönetim çözümleri gerektiren bir [günlük analizi çalışma alanı](../log-analytics/log-analytics-manage-access.md) görünümleri içerecek şekilde ve bir [Otomasyon hesabı](../automation/automation-security-overview.md#automation-account-overview) runbook'ları ve ilgili kaynakları içerecek şekilde.  Çözüm kaynaklarında oluşturulur ve çözümde tanımlanmamalıdır önce bu kullanılabilir olması gerekir.  Kullanıcı [çalışma ve hesabı belirtin](operations-management-suite-solutions.md#log-analytics-workspace-and-automation-account) zaman çözümünüzü dağıtmak, ancak yazarı olarak, aşağıdaki noktaları dikkate almanız gerekir.


## <a name="solution-resource"></a>Çözüm kaynağı
Kaynak girişi her çözüm gerektirir **kaynakları** çözümü tanımlar öğesi.  Bu bir türü olacak **Microsoft.OperationsManagement/solutions** ve aşağıdaki yapı ayarlanmıştır. Bu içerir [standart parametreler](#parameters) ve [değişkenleri](#variables) , genellikle çözümün özelliklerini tanımlamak için kullanılır.


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
Çözüm kaynağı olmalıdır bir [bağımlılık](../azure-resource-manager/resource-group-define-dependencies.md) gerektiği çözümü oluşturulabilmesi için önce mevcut Bu çözümdeki her bir kaynak üzerinde.  Her kaynak için bir giriş ekleyerek bunu **dependsOn** öğesi.

### <a name="properties"></a>Özellikler
Çözüm kaynağı aşağıdaki tabloda özelliklerine sahiptir.  Bu, başvurulan ve çözüm yüklendikten sonra kaynak nasıl yönetildiğini tanımlar çözümü tarafından bulunan kaynaklar içerir.  Çözümdeki her bir kaynağın ya da listelenmelidir **referencedResources** veya **containedResources** özelliği.

| Özellik | Açıklama |
|:--- |:--- |
| workspaceResourceId |Günlük analizi çalışma alanı biçiminde Kimliğini  *<Resource Group ID>/providers/Microsoft.OperationalInsights/workspaces/\<çalışma alanı adı\>*. |
| referencedResources |Çözüm kaldırıldığında kaldırılmamalıdır çözümü kaynaklarında listesi. |
| containedResources |Çözüm kaldırıldığında kaldırılmalıdır çözümü kaynaklarında listesi. |

Yukarıdaki örnekte, bir runbook, zamanlama ve görünüm ile bir çözüm içindir.  Zamanlama ve runbook *başvurulan* içinde **özellikleri** çözüm kaldırıldığında bunlar kaldırılmaz şekilde öğesi.  Görünüm *bulunan* çözüm kaldırıldığında kaldırılır.

### <a name="plan"></a>Planlama
**Planı** çözüm kaynak varlığı aşağıdaki tabloda özellikleri vardır.

| Özellik | Açıklama |
|:--- |:--- |
| ad |Çözüm adı. |
| sürüm |Yazar tarafından belirlenen çözümü sürümü. |
| ürün |Çözümü tanımlamak için benzersiz bir dize. |
| publisher |Çözüm Yayımcısı. |



## <a name="sample"></a>Örnek
Çözüm dosyaları aşağıdaki konumlarda bir çözüm kaynakla örnekleri görüntüleyebilirsiniz.

- [Otomasyon kaynakları](operations-management-suite-solutions-resources-automation.md#sample)
- [Arama ve uyarı kaynakları](operations-management-suite-solutions-resources-searches-alerts.md#sample)


## <a name="next-steps"></a>Sonraki adımlar
* [Kaydedilmiş aramaları ve Uyarıları Ekle](operations-management-suite-solutions-resources-searches-alerts.md) Yönetimi çözümünüz için.
* [Görünümler ekleme](operations-management-suite-solutions-resources-views.md) Yönetimi çözümünüz için.
* [Runbook'ları ve diğer Automation kaynaklarına eklemek](operations-management-suite-solutions-resources-automation.md) Yönetimi çözümünüz için.
* Ayrıntılarını öğrenmek [Azure Resource Manager şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md).
* Arama [Azure hızlı başlangıç şablonlarını](https://azure.microsoft.com/documentation/templates) farklı Resource Manager şablonları örneklerini için.
