---
title: "Azure Automation kaynaklarını OMS çözümlerinde | Microsoft Docs"
description: "OMS çözümlerinde, toplama ve izleme verilerini işleme gibi işlemleri otomatik hale getirmek için Azure Automation runbook'ları genellikle dahil edilir.  Bu makalede, runbook'ları ve ilgili kaynaklarını bir çözüme eklemek açıklar."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 5281462e-f480-4e5e-9c19-022f36dce76d
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c1909183a33ed03d8165671cff25cc8b83b77733
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="adding-azure-automation-resources-to-an-oms-management-solution-preview"></a>Azure Automation kaynaklarınız OMS yönetim çözümünü (Önizleme) ekleme
> [!NOTE]
> Bu, şu anda önizlemede OMS yönetim çözümleri oluşturmak için başlangıç belgesidir. Aşağıda açıklanan herhangi bir şema değiştirilebilir ' dir.   


[OMS yönetim çözümlerine](operations-management-suite-solutions.md) toplama ve izleme verilerini işleme gibi işlemleri otomatik hale getirmek için Azure Automation runbook'ları tipik olarak içerecektir.  Runbook'ları yanı sıra Automation hesapları değişkenleri ve çözümde kullanılan runbook'ları destek zamanlamaları gibi varlıkları içerir.  Bu makalede, runbook'ları ve ilgili kaynaklarını bir çözüme eklemek açıklar.

> [!NOTE]
> Bu makaledeki örnekler parametreleri ve gerekli olduğunu veya yönetim çözümleri için ortak olduğunu ve açıklanan değişkenleri kullanma [Operations Management Suite (OMS) yönetimi çözümleri oluşturma](operations-management-suite-solutions-creating.md) 


## <a name="prerequisites"></a>Ön koşullar
Bu makale, zaten aşağıdaki bilgilerle aşina olduğunuzu varsayar.

- Nasıl yapılır [bir yönetim çözümü oluşturma](operations-management-suite-solutions-creating.md).
- Yapısı bir [çözüm dosyasını](operations-management-suite-solutions-solution-file.md).
- Nasıl yapılır [Resource Manager şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md)

## <a name="automation-account"></a>Otomasyon hesabı
Azure Otomasyonu tüm kaynakları bulunan bir [Otomasyon hesabı](../automation/automation-security-overview.md#automation-account-overview).  Bölümünde açıklandığı gibi [OMS çalışma ve Automation hesabı](operations-management-suite-solutions.md#oms-workspace-and-automation-account) Otomasyon hesabı Yönetimi çözümünde dahil değildir ancak çözüm yüklenmeden önce mevcut olması gerekir.  Kullanılabilir değilse, çözüm yükleme başarısız olur.

Her Otomasyon kaynağın adını kendi Otomasyon hesabının adını içerir.  Bu çözümle yapılır **accountName** bir runbook kaynağın aşağıdaki örnekteki gibi parametre.

    "name": "[concat(parameters('accountName'), '/MyRunbook'))]"


## <a name="runbooks"></a>Runbook'lar
Çözüm yüklendiğinde, oluşturuldukları Çözüm dosyasındaki çözüm tarafından kullanılan tüm runbook içermelidir.  Burada çözümünüzü yükleme herhangi bir kullanıcı tarafından erişilebilir bir ortak konuma runbook'u yayımlamanız gerekir böylece şablon runbook'ta gövdesi yine de içeremez.

[Azure Otomasyonu runbook'u](../automation/automation-runbook-types.md) kaynaklarınız türü **Microsoft.Automation/automationAccounts/runbooks** ve aşağıdaki yapı ayarlanmıştır. Kopyalayabilir ve çözüm dosyanıza Bu kod parçacığını yapıştırın ve parametre adlarını değiştirmek için bu ortak değişkenleri ve parametreleri içerir. 

    {
        "name": "[concat(parameters('accountName'), '/', variables('Runbook').Name)]",
        "type": "Microsoft.Automation/automationAccounts/runbooks",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "dependsOn": [
        ],
        "location": "[parameters('regionId')]",
        "tags": { },
        "properties": {
            "runbookType": "[variables('Runbook').Type]",
            "logProgress": "true",
            "logVerbose": "true",
            "description": "[variables('Runbook').Description]",
            "publishContentLink": {
                "uri": "[variables('Runbook').Uri]",
                "version": [variables('Runbook').Version]"
            }
        }
    }


Runbook'lar için özellikler aşağıdaki tabloda açıklanmıştır.

| Özellik | Açıklama |
|:--- |:--- |
| runbookType |Runbook türlerini belirtir. <br><br> Komut dosyası - PowerShell komut dosyası <br>PowerShell - PowerShell iş akışı <br> GraphPowerShell - grafik PowerShell komut dosyası runbook <br> GraphPowerShellWorkflow - grafik PowerShell iş akışı runbook |
| logProgress |Belirtir olup olmadığını [ilerleme kayıtlarını](../automation/automation-runbook-output-and-messages.md) runbook için oluşturulması gerekir. |
| logVerbose |Belirtir olup olmadığını [ayrıntılı kayıtları](../automation/automation-runbook-output-and-messages.md) runbook için oluşturulması gerekir. |
| açıklama |Runbook için isteğe bağlı bir açıklama. |
| publishContentLink |Runbook içeriğini belirtir. <br><br>URI - runbook'unun içeriğini URI.  Bu, PowerShell ve komut dosyası runbook'lar için bir .ps1 dosyası ve bir grafik runbook'u dışarı aktarılan grafik runbook dosyası olacaktır.  <br> Sürüm - runbook kendi izleme için sürümü. |


## <a name="automation-jobs"></a>Otomasyon işleri
Azure Automation'da bir runbook başlattığınızda bir Otomasyonu işi oluşturur.  Yönetim çözümü yüklendiğinde otomatik olarak bir runbook'u başlatmak için çözümünüzün bir Otomasyon iş kaynağı ekleyebilirsiniz.  Bu yöntem, genellikle çözümün ilk yapılandırma için kullanılan runbook'ları başlatmak için kullanılır.  Düzenli aralıklarla bir runbook'u başlatmak için oluşturma bir [zamanlama](#schedules) ve [iş zamanlaması](#job-schedules)

İş kaynaklarınız türü **Microsoft.Automation/automationAccounts/jobs** ve aşağıdaki yapı ayarlanmıştır.  Kopyalayabilir ve çözüm dosyanıza Bu kod parçacığını yapıştırın ve parametre adlarını değiştirmek için bu ortak değişkenleri ve parametreleri içerir. 

    {
      "name": "[concat(parameters('accountName'), '/', parameters('Runbook').JobGuid)]",
      "type": "Microsoft.Automation/automationAccounts/jobs",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('Runbook').Name)]"
      ],
      "tags": { },
      "properties": {
        "runbook": {
          "name": "[variables('Runbook').Name]"
        },
        "parameters": {
          "Parameter1": "[[variables('Runbook').Parameter1]",
          "Parameter2": "[[variables('Runbook').Parameter2]"
        }
      }
    }

Otomasyon işleri için özellikler aşağıdaki tabloda açıklanmıştır.

| Özellik | Açıklama |
|:--- |:--- |
| runbook |Başlatmak için runbook'un adına sahip tek bir ad varlık. |
| parametreler |Varlık runbook tarafından gerekli her parametre değeri. |

İş, runbook adı ve runbook'a gönderilmek üzere parametre değerlerini içerir.  İş gereken [bağımlı](operations-management-suite-solutions-solution-file.md#resources) bu yana runbook başlatma runbook işinden önce oluşturulması gerekir.  Başlatılması gereken birden çok runbook varsa, ilk çalışması gereken tüm diğer işler bağımlı bir iş sağlayarak sıralarına tanımlayabilirsiniz.

Bir iş kaynağı adı genellikle parametresi tarafından atanan bir GUID içermelidir.  Daha fazla bilgiyi GUID parametreler hakkında [Operations Management Suite (OMS) çözümleri oluşturma](operations-management-suite-solutions-solution-file.md#parameters).  


## <a name="certificates"></a>Sertifikalar
[Azure Otomasyonu sertifika](../automation/automation-certificates.md) bir türüne sahip **Microsoft.Automation/automationAccounts/certificates** ve aşağıdaki yapı ayarlanmıştır. Kopyalayabilir ve çözüm dosyanıza Bu kod parçacığını yapıştırın ve parametre adlarını değiştirmek için bu ortak değişkenleri ve parametreleri içerir. 

    {
      "name": "[concat(parameters('accountName'), '/', variables('Certificate').Name)]",
      "type": "Microsoft.Automation/automationAccounts/certificates",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "base64Value": "[variables('Certificate').Base64Value]",
        "thumbprint": "[variables('Certificate').Thumbprint]"
      }
    }



Sertifikaları kaynaklar için özellikler aşağıdaki tabloda açıklanmıştır.

| Özellik | Açıklama |
|:--- |:--- |
| base64value değeri |Sertifika Base 64 değeri. |
| parmak izi |Sertifika parmak izi. |



## <a name="credentials"></a>Kimlik Bilgileri
[Azure Otomasyonu kimlik](../automation/automation-credentials.md) bir türüne sahip **Microsoft.Automation/automationAccounts/credentials** ve aşağıdaki yapı ayarlanmıştır.  Kopyalayabilir ve çözüm dosyanıza Bu kod parçacığını yapıştırın ve parametre adlarını değiştirmek için bu ortak değişkenleri ve parametreleri içerir. 


    {
      "name": "[concat(parameters('accountName'), '/', variables('Credential').Name)]",
      "type": "Microsoft.Automation/automationAccounts/credentials",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "userName": "[parameters('credentialUsername')]",
        "password": "[parameters('credentialPassword')]"
      }
    }

Kimlik bilgisi kaynaklar için özellikler aşağıdaki tabloda açıklanmıştır.

| Özellik | Açıklama |
|:--- |:--- |
| Kullanıcı adı |Kimlik bilgisi için kullanıcı adı. |
| password |Kimlik bilgisinin parolası. |


## <a name="schedules"></a>Zamanlamalar
[Azure Otomasyon zamanlamaları](../automation/automation-schedules.md) bir türüne sahip **Microsoft.Automation/automationAccounts/schedules** ve aşağıdaki yapısı. Kopyalayabilir ve çözüm dosyanıza Bu kod parçacığını yapıştırın ve parametre adlarını değiştirmek için bu ortak değişkenleri ve parametreleri içerir. 

    {
      "name": "[concat(parameters('accountName'), '/', variables('Schedule').Name)]",
      "type": "microsoft.automation/automationAccounts/schedules",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "description": "[variables('Schedule').Description]",
        "startTime": "[parameters('scheduleStartTime')]",
        "timeZone": "[parameters('scheduleTimeZone')]",
        "isEnabled": "[variables('Schedule').IsEnabled]",
        "interval": "[variables('Schedule').Interval]",
        "frequency": "[variables('Schedule').Frequency]"
      }
    }

Zamanlama kaynaklar için özellikler aşağıdaki tabloda açıklanmıştır.

| Özellik | Açıklama |
|:--- |:--- |
| açıklama |Zamanlama için isteğe bağlı bir açıklama. |
| startTime |Zamanlama Başlangıç saati DateTime nesnesi olarak belirtir. Geçerli bir DateTime dönüştürülebilir ise bir dize sağlanabilir. |
| IsEnabled |Zamanlama etkinleştirilip etkinleştirilmeyeceğini belirtir. |
| interval |Zamanlaması için aralık türü.<br><br>günü<br>saat |
| frequency |Zamanlama gün veya saat cinsinden yangın sıklığı. |

Zamanlama Başlangıç saati geçerli saatten büyük bir değere sahip olması gerekir.  Zaman yüklenecek gittiği bilmesinin yolu yoktur beri bu değere sahip bir değişken sağlayamaz.

Aşağıdaki iki stratejiler birini zamanlama kaynakları bir çözümde kullanırken kullanın.

- Bir parametre başlangıç zamanı zamanlama için kullanın.  Bu çözüm yüklediğinizde bir değer sağlamak için kullanıcıya sorar.  Birden çok zamanlama varsa, tek bir parametre birden fazla bunlardan biri için kullanabilirsiniz.
- Çözüm yüklendiğinde başlayan bir runbook kullanarak zamanlamaları oluşturun.  Bu kullanıcı bir süre belirtebilirsiniz gerekliliğini kaldırır, ancak çözüm kaldırıldığında kaldırılacak şekilde zamanlama çözümünüzde içeremez.


### <a name="job-schedules"></a>İş zamanlamaları
İş zamanlaması kaynakları runbook zamanlama ile ilişkilendirin.  Bir türüne sahip **Microsoft.Automation/automationAccounts/jobSchedules** ve aşağıdaki yapısı.  Kopyalayabilir ve çözüm dosyanıza Bu kod parçacığını yapıştırın ve parametre adlarını değiştirmek için bu ortak değişkenleri ve parametreleri içerir. 

    {
      "name": "[concat(parameters('accountName'), '/', variables('Schedule').LinkGuid)]",
      "type": "microsoft.automation/automationAccounts/jobSchedules",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "dependsOn": [
        "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
        "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]"
      ],
      "tags": {
      },
      "properties": {
        "schedule": {
          "name": "[variables('Schedule').Name]"
        },
        "runbook": {
          "name": "[variables('Runbook').Name]"
        }
      }
    }


İş zamanlamaları özellikler aşağıdaki tabloda açıklanmıştır.

| Özellik | Açıklama |
|:--- |:--- |
| Zamanlama adı |Tek **adı** varlık zamanlama adı. |
| runbook adı  |Tek **adı** runbook'un adına sahip varlık.  |



## <a name="variables"></a>Değişkenler
[Azure Otomasyon değişkenleri](../automation/automation-variables.md) bir türüne sahip **Microsoft.Automation/automationAccounts/variables** ve aşağıdaki yapı ayarlanmıştır.  Kopyalayabilir ve çözüm dosyanıza Bu kod parçacığını yapıştırın ve parametre adlarını değiştirmek için bu ortak değişkenleri ve parametreleri içerir.

    {
      "name": "[concat(parameters('accountName'), '/', variables('Variable').Name)]",
      "type": "microsoft.automation/automationAccounts/variables",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "description": "[variables('Variable').Description]",
        "isEncrypted": "[variables('Variable').Encrypted]",
        "type": "[variables('Variable').Type]",
        "value": "[variables('Variable').Value]"
      }
    }

Değişken kaynaklar için özellikler aşağıdaki tabloda açıklanmıştır.

| Özellik | Açıklama |
|:--- |:--- |
| açıklama | Değişken için isteğe bağlı bir açıklama. |
| Isencrypted | Değişkeni şifrelenmesi gerekip gerekmediğini belirtir. |
| type | Bu özellik şu anda hiçbir etkisi yoktur.  Değişken veri türü ilk değeri tarafından belirlenir. |
| değer | Değişken için değeri. |

> [!NOTE]
> **Türü** özelliği şu anda hiçbir etkisi oluşturulan değişkeni.  Değişken veri türü değeri tarafından belirlenir.  

Değişken için ilk değeri ayarlarsanız, doğru veri türü olarak yapılandırılması gerekir.  Aşağıdaki tabloda, izin verilen farklı veri türlerini ve bunların sözdizimi sağlar.  JSON değerler her zaman tüm özel karakterleri tırnak işaretleri içindeki ile tırnak içine alınması beklenir unutmayın.  Örneğin, bir dize değeri dize tırnak tarafından belirtilmesi (kaçış karakteri kullanarak (\\)) sırada sayısal bir değer tek tırnak işareti kümesiyle belirtilmesi.

| Veri türü | Açıklama | Örnek | Çözümler |
|:--|:--|:--|:--|
| Dize   | Değeri çift tırnak içine alın.  | "\"Merhaba Dünya\"" | "Hello world" |
| sayısal  | Tek tırnak sahip bir sayısal değer.| "64" | 64 |
| Boole değeri  | **doğru** veya **false** tırnak.  Bu değer küçük harfli olması gerektiğini unutmayın. | "true" | TRUE |
| Tarih saat | Serileştirilmiş tarih değeri.<br>Bu değer için belirli bir tarih oluşturmak için PowerShell'de ConvertTo-Json cmdlet'ini kullanabilirsiniz.<br>Örnek: get-date "24/5/2017 13:14:57" \| ConvertTo-Json | "\\/Date(1495656897378)\\/" | 2017-05-24 13:14:57 |

## <a name="modules"></a>Modüller
Yönetim çözümünüzü tanımlamak gerekmez [genel modülleri](../automation/automation-integration-modules.md) bunlar her zaman Otomasyon hesabınızda kullanılabilir olacağından, runbook'lar tarafından kullanılan.  Runbook'lar tarafından kullanılan başka bir modül için bir kaynak eklemeniz gerekir.

[Tümleştirme modülleri](../automation/automation-integration-modules.md) bir türüne sahip **Microsoft.Automation/automationAccounts/modules** ve aşağıdaki yapı ayarlanmıştır.  Kopyalayabilir ve çözüm dosyanıza Bu kod parçacığını yapıştırın ve parametre adlarını değiştirmek için bu ortak değişkenleri ve parametreleri içerir.

    {
      "name": "[concat(parameters('accountName'), '/', variables('Module').Name)]",
      "type": "Microsoft.Automation/automationAccounts/modules",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "dependsOn": [
      ],
      "properties": {
        "contentLink": {
          "uri": "[variables('Module').Uri]"
        }
      }
    }


Modül kaynaklar için özellikler aşağıdaki tabloda açıklanmıştır.

| Özellik | Açıklama |
|:--- |:--- |
| contentLink |Modül içeriğini belirtir. <br><br>URI - modül içeriği URI.  Bu, PowerShell ve komut dosyası runbook'lar için bir .ps1 dosyası ve bir grafik runbook'u dışarı aktarılan grafik runbook dosyası olacaktır.  <br> Sürüm - kendi izleme modülü sürümü. |

Runbook önce runbook oluşturulduğundan emin olmak için modülü kaynak bağlı.

### <a name="updating-modules"></a>Modülleri güncelleştiriliyor
Bir zamanlama kullanan bir runbook içeren bir yönetim çözümü güncelleştirin ve bu runbook tarafından kullanılan yeni bir modül çözümünüzü yeni sürümü varsa, runbook modülünün eski sürümünü kullanabilir.  Aşağıdaki runbook'lar çözümünüzde dahil ve diğer runbook'ları önce çalıştırılacak bir işi oluşturmanız gerekir.  Bu herhangi bir modül olarak güncelleştirildiğini sağlayacak runbook'lar yüklenmeden önce gerekli.

* [Güncelleştirme ModulesinAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/DisplayScript) tüm runbook'ları çözümünüzdeki tarafından kullanılan modülleri en son sürümü olduğundan emin olun.  
* [ReRegisterAutomationSchedule MS Mgmt](https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/DisplayScript) tüm runbook'ları onlara ile kullanmak üzere en son modülleri bağlantılı emin olmak için zamanlama kaynakları yeniden kaydettirin.




## <a name="sample"></a>Örnek
Aşağıdaki kaynakları içermektedir içeren bir çözüm örneği aşağıda verilmiştir:

- Runbook.  Ortak bir GitHub deposunda bulunan bir örnek runbook budur.
- Çözüm yüklendiğinde runbook başlatır Otomasyonu işi.
- Zamanlama ve düzenli aralıklarla runbook'u başlatmak için iş zamanlama.
- Sertifika.
- Kimlik bilgileri.
- Değişkeni.
- Modül.  Bu [OMSIngestionAPI Modülü](https://www.powershellgallery.com/packages/OMSIngestionAPI/1.5) için günlük analizi veri yazmak için. 

Örnek kullanır [standart çözüm parametreleri](operations-management-suite-solutions-solution-file.md#parameters) cmdlet'e kod değerleri kaynak tanımlarında aksine bir çözümde yaygın olarak kullanılacak değişkenleri.


    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "workspaceName": {
          "type": "string",
          "metadata": {
            "Description": "Name of Log Analytics workspace."
          }
        },
        "accountName": {
          "type": "string",
          "metadata": {
            "Description": "Name of Automation account."
          }
        },
        "workspaceregionId": {
          "type": "string",
          "metadata": {
            "Description": "Region of Log Analytics workspace."
          }
        },
        "regionId": {
          "type": "string",
          "metadata": {
            "Description": "Region of Automation account."
          }
        },
        "pricingTier": {
          "type": "string",
          "metadata": {
            "Description": "Pricing tier of both Log Analytics workspace and Azure Automation account."
          }
        },
        "certificateBase64Value": {
          "type": "string",
          "metadata": {
            "Description": "Base 64 value for certificate."
          }
        },
        "certificateThumbprint": {
          "type": "securestring",
          "metadata": {
            "Description": "Thumbprint for certificate."
          }
        },
        "credentialUsername": {
          "type": "string",
          "metadata": {
            "Description": "Username for credential."
          }
        },
        "credentialPassword": {
          "type": "securestring",
          "metadata": {
            "Description": "Password for credential."
          }
        },
        "scheduleStartTime": {
          "type": "string",
          "metadata": {
            "Description": "Start time for shedule."
          }
        },
        "scheduleTimeZone": {
          "type": "string",
          "metadata": {
            "Description": "Time zone for schedule."
          }
        },
        "scheduleLinkGuid": {
          "type": "string",
          "metadata": {
            "description": "GUID for the schedule link to runbook.",
            "control": "guid"
          }
        },
        "runbookJobGuid": {
          "type": "string",
          "metadata": {
            "description": "GUID for the runbook job.",
            "control": "guid"
          }
        }
      },
      "variables": {
        "SolutionName": "MySolution",
        "SolutionVersion": "1.0",
        "SolutionPublisher": "Contoso",
        "ProductName": "SampleSolution",
    
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31",
    
        "Runbook": {
          "Name": "MyRunbook",
          "Description": "Sample runbook",
          "Type": "PowerShell",
          "Uri": "https://raw.githubusercontent.com/user/myrepo/master/samples/MyRunbook.ps1",
          "JobGuid": "[parameters('runbookJobGuid')]"
        },
    
        "Certificate": {
          "Name": "MyCertificate",
          "Base64Value": "[parameters('certificateBase64Value')]",
          "Thumbprint": "[parameters('certificateThumbprint')]"
        },
    
        "Credential": {
          "Name": "MyCredential",
          "UserName": "[parameters('credentialUsername')]",
          "Password": "[parameters('credentialPassword')]"
        },
    
        "Schedule": {
          "Name": "MySchedule",
          "Description": "Sample schedule",
          "IsEnabled": "true",
          "Interval": "1",
          "Frequency": "hour",
          "StartTime": "[parameters('scheduleStartTime')]",
          "TimeZone": "[parameters('scheduleTimeZone')]",
          "LinkGuid": "[parameters('scheduleLinkGuid')]"
        },
    
        "Variable": {
          "Name": "MyVariable",
          "Description": "Sample variable",
          "Encrypted": 0,
          "Type": "string",
          "Value": "'This is my string value.'"
        },
    
        "Module": {
          "Name": "OMSIngestionAPI",
          "Uri": "https://devopsgallerystorage.blob.core.windows.net/packages/omsingestionapi.1.3.0.nupkg"
        }
      },
      "resources": [
        {
          "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
          "location": "[parameters('workspaceRegionId')]",
          "tags": { },
          "type": "Microsoft.OperationsManagement/solutions",
          "apiVersion": "[variables('LogAnalyticsApiVersion')]",
          "dependsOn": [
            "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/jobs/', parameters('accountName'), variables('Runbook').JobGuid)]",
            "[resourceId('Microsoft.Automation/automationAccounts/certificates/', parameters('accountName'), variables('Certificate').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/credentials/', parameters('accountName'), variables('Credential').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/jobSchedules/', parameters('accountName'), variables('Schedule').LinkGuid)]",
            "[resourceId('Microsoft.Automation/automationAccounts/variables/', parameters('accountName'), variables('Variable').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/modules/', parameters('accountName'), variables('Module').Name)]"
          ],
          "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspacename'))]",
            "referencedResources": [
              "[resourceId('Microsoft.Automation/automationAccounts/modules/', parameters('accountName'), variables('Module').Name)]"
            ],
            "containedResources": [
              "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/jobs/', parameters('accountName'), variables('Runbook').JobGuid)]",
              "[resourceId('Microsoft.Automation/automationAccounts/certificates/', parameters('accountName'), variables('Certificate').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/credentials/', parameters('accountName'), variables('Credential').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/jobSchedules/', parameters('accountName'), variables('Schedule').LinkGuid)]",
              "[resourceId('Microsoft.Automation/automationAccounts/variables/', parameters('accountName'), variables('Variable').Name)]"
            ]
          },
          "plan": {
            "name": "[concat(variables('SolutionName'), '[' ,parameters('workspaceName'), ']')]",
            "Version": "[variables('SolutionVersion')]",
            "product": "[variables('ProductName')]",
            "publisher": "[variables('SolutionPublisher')]",
            "promotionCode": ""
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Runbook').Name)]",
          "type": "Microsoft.Automation/automationAccounts/runbooks",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "dependsOn": [
          ],
          "location": "[parameters('regionId')]",
          "tags": { },
          "properties": {
            "runbookType": "[variables('Runbook').Type]",
            "logProgress": "true",
            "logVerbose": "true",
            "description": "[variables('Runbook').Description]",
            "publishContentLink": {
              "uri": "[variables('Runbook').Uri]",
              "version": "1.0.0.0"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Runbook').JobGuid)]",
          "type": "Microsoft.Automation/automationAccounts/jobs",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('Runbook').Name)]"
          ],
          "tags": { },
          "properties": {
            "runbook": {
              "name": "[variables('Runbook').Name]"
            },
            "parameters": {
              "targetSubscriptionId": "[subscription().subscriptionId]",
              "resourcegroup": "[resourceGroup().name]",
              "automationaccount": "[parameters('accountName')]"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Certificate').Name)]",
          "type": "Microsoft.Automation/automationAccounts/certificates",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "Base64Value": "[variables('Certificate').Base64Value]",
            "Thumbprint": "[variables('Certificate').Thumbprint]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Credential').Name)]",
          "type": "Microsoft.Automation/automationAccounts/credentials",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "userName": "[variables('Credential').UserName]",
            "password": "[variables('Credential').Password]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Schedule').Name)]",
          "type": "microsoft.automation/automationAccounts/schedules",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "description": "[variables('Schedule').Description]",
            "startTime": "[variables('Schedule').StartTime]",
            "timeZone": "[variables('Schedule').TimeZone]",
            "isEnabled": "[variables('Schedule').IsEnabled]",
            "interval": "[variables('Schedule').Interval]",
            "frequency": "[variables('Schedule').Frequency]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Schedule').LinkGuid)]",
          "type": "microsoft.automation/automationAccounts/jobSchedules",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "dependsOn": [
            "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]"
          ],
          "tags": {
          },
          "properties": {
            "schedule": {
              "name": "[variables('Schedule').Name]"
            },
            "runbook": {
              "name": "[variables('Runbook').Name]"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Variable').Name)]",
          "type": "microsoft.automation/automationAccounts/variables",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "description": "[variables('Variable').Description]",
            "isEncrypted": "[variables('Variable').Encrypted]",
            "type": "[variables('Variable').Type]",
            "value": "[variables('Variable').Value]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Module').Name)]",
          "type": "Microsoft.Automation/automationAccounts/modules",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "dependsOn": [
          ],
          "properties": {
            "contentLink": {
              "uri": "[variables('Module').Uri]"
            }
          }
        }
    
      ],
      "outputs": { }
    }




## <a name="next-steps"></a>Sonraki adımlar
* [Çözümünüze bir görünüm ekleyin](operations-management-suite-solutions-resources-views.md) toplanan verileri görselleştirmek için.
