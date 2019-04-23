---
title: Yönetim çözümleri, Azure Automation kaynaklarını | Microsoft Docs
description: Yönetim çözümleri, toplama ve izleme verilerini işleme gibi işlemleri otomatik hale getirmek için Azure Otomasyonu'nda runbook'ları genellikle dahil edilir.  Bu makalede, bir çözümde runbook'ları ve bunlarla ilişkili kaynakları içerecek şekilde açıklar.
services: monitoring
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 5281462e-f480-4e5e-9c19-022f36dce76d
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1c9b13f44dae068597cb82a0aa803283ad5e67bc
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2019
ms.locfileid: "57763615"
---
# <a name="adding-azure-automation-resources-to-a-management-solution-preview"></a>Bir yönetim çözümü (Önizleme) Azure Automation kaynaklarını ekleme
> [!NOTE]
> Şu anda Önizleme aşamasında olan yönetim çözümleri oluşturmak için başlangıç belgeleri budur. Aşağıda açıklanan herhangi bir şema tabi bir değişikliktir.   


[Yönetim çözümleri]( solutions.md) toplama ve izleme verilerini işleme gibi işlemleri otomatik hale getirmek için Azure Otomasyonu'nda runbook'ları genellikle içerecektir.  Otomasyon hesapları runbook'lara ek olarak, değişkenler ve çözümde kullanılan runbook'ları destekleyen zamanlamalar gibi varlıklar içerir.  Bu makalede, bir çözümde runbook'ları ve bunlarla ilişkili kaynakları içerecek şekilde açıklar.

> [!NOTE]
> Bu makaledeki örnekleri parametreler ve değişkenler gerekli olduğunu veya yönetim çözümleri için yaygın olduğunu ve açıklanan kullanmak [tasarım ve derleme Azure Yönetimi çözümünde]( solutions-creating.md) 


## <a name="prerequisites"></a>Önkoşullar
Bu makale, zaten aşağıdaki bilgilerle ilgili bilgi sahibi olduğunuzu varsayar.

- Nasıl yapılır [yönetimi çözümü oluşturmak]( solutions-creating.md).
- Yapısı bir [çözüm dosyası]( solutions-solution-file.md).
- Nasıl yapılır [Resource Manager şablonları yazma](../../azure-resource-manager/resource-group-authoring-templates.md)

## <a name="automation-account"></a>Otomasyon hesabı
Azure Otomasyonu'nda tüm kaynaklar içerdiği bir [Otomasyon hesabı](../../automation/automation-security-overview.md#automation-account-overview).  Bölümünde anlatıldığı gibi [Log Analytics çalışma alanını ve Otomasyon hesabı]( solutions.md#log-analytics-workspace-and-automation-account) Otomasyon hesabı Yönetimi çözümünde dahil değildir, ancak çözüm yüklenmeden önce mevcut olması gerekir.  Mevcut değilse, çözüm yükleme başarısız olur.

Her Otomasyon kaynağın adını, Otomasyon hesabının adını içerir.  Bu çözüm ile gerçekleştirilir **accountName** runbook kaynağının aşağıdaki örnekteki gibi parametre.

    "name": "[concat(parameters('accountName'), '/MyRunbook'))]"


## <a name="runbooks"></a>Runbook'lar
Çözüm yüklendiğinde yük oluşturulan çözüm dosyasını çözüm tarafından kullanılan tüm runbook'ları içermelidir.  Burada çözümünüzü yükleme herhangi bir kullanıcı tarafından erişilebilen ortak bir konuma runbook'u yayımlamanız gerekir böylece Şablonu'nda runbook'un gövdesinin yine de içeremez.

[Azure Otomasyonu runbook'u](../../automation/automation-runbook-types.md) kaynaklara sahip bir tür **Microsoft.Automation/automationAccounts/runbooks** ve aşağıdaki yapıya sahiptir. Kopyalayabilir ve bu kod parçacığı, çözüm dosyasına yapıştırın ve parametre adlarını değiştirmek için bu genel değişkenler ve parametreler içerir. 

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


Runbook'ları için özellikler aşağıdaki tabloda açıklanmıştır.

| Özellik | Açıklama |
|:--- |:--- |
| runbookType |Runbook türlerini belirtir. <br><br> Betiği - PowerShell Betiği <br>PowerShell - PowerShell iş akışı <br> GraphPowerShell - grafik PowerShell Betiği runbook <br> GraphPowerShellWorkflow - grafik PowerShell iş akışı runbook'u |
| logProgress |Belirtir olup olmadığını [ilerleme durumu kayıtlarını](../../automation/automation-runbook-output-and-messages.md) runbook için oluşturulması gerekir. |
| logVerbose |Belirtir olup olmadığını [ayrıntılı kayıtları](../../automation/automation-runbook-output-and-messages.md) runbook için oluşturulması gerekir. |
| description |Runbook için isteğe bağlı bir açıklama. |
| publishContentLink |Runbook'un içeriğini belirtir. <br><br>Uri - runbook içeriğinin URI'si.  Bu, PowerShell ve komut dosyası runbook'ları için bir .ps1 dosyası ve bir graf runbook için dışarı aktarılan grafik runbook dosyası olacaktır.  <br> Sürüm - runbook kendi izleme için sürümü. |


## <a name="automation-jobs"></a>Otomasyon işleri
Azure Automation'da bir runbook başlattığınızda bir Otomasyon işi oluşturur.  Yönetim çözümü yüklendiğinde otomatik olarak bir runbook başlatmak için çözümünüze bir Otomasyon iş kaynağı ekleyebilirsiniz.  Bu yöntem, genellikle çözümün ilk yapılandırma için kullanılan runbook'ları başlatmak için kullanılır.  Düzenli aralıklarla bir runbook başlatmak için oluşturun bir [zamanlama](#schedules) ve [iş zamanlaması](#job-schedules)

İş kaynaklarına sahip bir tür **Microsoft.Automation/automationAccounts/jobs** ve aşağıdaki yapıya sahiptir.  Kopyalayabilir ve bu kod parçacığı, çözüm dosyasına yapıştırın ve parametre adlarını değiştirmek için bu genel değişkenler ve parametreler içerir. 

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
| runbook |Tek ad varlık başlatmak için runbook'un adına sahip. |
| parameters |Varlık için runbook tarafından gerekli her parametre değeri. |

İş, runbook adı ve runbook'a gönderilecek tüm parametre değerlerini içerir.  İş gereken [bağımlı]( solutions-solution-file.md#resources) beri runbook başlatılıyor runbook işinden önce oluşturulması gerekir.  Başlatılacak birden çok runbook varsa, ilk çalıştırılması gereken diğer işleri üzerinde bağımlı bir işlem sağlayarak sıralarına tanımlayabilirsiniz.

İş kaynağı adı genellikle parametre tarafından atanan bir GUID içermelidir.  Daha fazla GUID parametrelerinde hakkında [yönetim çözüm dosyası, Azure'da]( solutions-solution-file.md#parameters).  


## <a name="certificates"></a>Sertifikalar
[Azure Otomasyonu sertifika](../../automation/automation-certificates.md) bir türüne sahip **Microsoft.Automation/automationAccounts/certificates** ve aşağıdaki yapıya sahiptir. Kopyalayabilir ve bu kod parçacığı, çözüm dosyasına yapıştırın ve parametre adlarını değiştirmek için bu genel değişkenler ve parametreler içerir. 

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



Sertifikaları kaynakların özellikleri aşağıdaki tabloda açıklanmıştır.

| Özellik | Açıklama |
|:--- |:--- |
| base64Value |Sertifikayı Base 64 değeri. |
| thumbprint |Sertifikanın parmak izi. |



## <a name="credentials"></a>Kimlik Bilgileri
[Azure Otomasyonu kimlik](../../automation/automation-credentials.md) bir türüne sahip **Microsoft.Automation/automationAccounts/credentials** ve aşağıdaki yapıya sahiptir.  Kopyalayabilir ve bu kod parçacığı, çözüm dosyasına yapıştırın ve parametre adlarını değiştirmek için bu genel değişkenler ve parametreler içerir. 


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

Kimlik bilgisi kaynakların özellikleri aşağıdaki tabloda açıklanmıştır.

| Özellik | Açıklama |
|:--- |:--- |
| userName |Kimlik bilgisi için kullanıcı adı. |
| password |Parola kimlik bilgisi için. |


## <a name="schedules"></a>Zamanlamalar
[Azure Otomasyon zamanlamaları](../../automation/automation-schedules.md) bir türüne sahip **Microsoft.Automation/automationAccounts/schedules** ve aşağıdaki yapıya sahiptir. Kopyalayabilir ve bu kod parçacığı, çözüm dosyasına yapıştırın ve parametre adlarını değiştirmek için bu genel değişkenler ve parametreler içerir. 

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

Zamanlama kaynakların özellikleri aşağıdaki tabloda açıklanmıştır.

| Özellik | Açıklama |
|:--- |:--- |
| description |Zamanlama için isteğe bağlı bir açıklama. |
| startTime |Başlangıç zamanı, zamanlamanın bir DateTime nesnesi olarak belirtir. Geçerli bir DateTime türüne dönüştürülebilir ise bir dize sağlanabilir. |
| isEnabled |Zamanlama etkin olup olmadığını belirtir. |
| interval |Zamanlama için aralık türü.<br><br>gün<br>saat |
| frequency |Zamanlama gün veya saat cinsinden yangın sıklığı. |

Zamanlamaları bir başlangıç saati geçerli saatten büyük bir değere sahip olması gerekir.  Olacağından, ne zaman yüklenmesini geçiyor olduğunu bilmesinin imkanı bu değere sahip bir değişken sağlayamaz.

Bir çözümde zamanlama kaynaklar kullanırken aşağıdaki iki stratejileri kullanın.

- Başlangıç zamanı zamanlama için bir parametre kullanın.  Bu çözümü yükledikleri sırada bir değer girmesini ister.  Birden çok zamanlama varsa, tek bir parametre birden fazlası için kullanabilirsiniz.
- Çözüm yüklendiğinde başlayan bir runbook kullanarak zamanlamaları oluşturun.  Bu bir saat belirtmenin kullanıcının gereksinimini ortadan kaldırır, ancak çözüm kaldırıldığında kaldırılacak çözümünüzde zamanlama içeremez.


### <a name="job-schedules"></a>İş zamanlamaları
İş zamanlaması kaynakları bir runbook zamanlama ile bağlayın.  Bir türü sahip oldukları **Microsoft.Automation/automationAccounts/jobSchedules** ve aşağıdaki yapıya sahiptir.  Kopyalayabilir ve bu kod parçacığı, çözüm dosyasına yapıştırın ve parametre adlarını değiştirmek için bu genel değişkenler ve parametreler içerir. 

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


İş zamanlamaları için özellikler aşağıdaki tabloda açıklanmıştır.

| Özellik | Açıklama |
|:--- |:--- |
| schedule name |Tek **adı** planının adı olan varlık. |
| runbook name  |Tek **adı** runbook'un adı olan varlık.  |



## <a name="variables"></a>Değişkenler
[Azure Otomasyonu değişken](../../automation/automation-variables.md) bir türüne sahip **Microsoft.Automation/automationAccounts/variables** ve aşağıdaki yapıya sahiptir.  Kopyalayabilir ve bu kod parçacığı, çözüm dosyasına yapıştırın ve parametre adlarını değiştirmek için bu genel değişkenler ve parametreler içerir.

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

Değişken kaynakların özellikleri aşağıdaki tabloda açıklanmıştır.

| Özellik | Açıklama |
|:--- |:--- |
| description | Değişken için isteğe bağlı bir açıklama. |
| isEncrypted | Değişken şifrelenmesi gerekip gerekmediğini belirtir. |
| type | Bu özellik şu anda hiçbir etkisi olmaz.  Değişkenin veri türü ilk değere göre belirlenir. |
| value | Değişken için değeri. |

> [!NOTE]
> **Türü** özelliği şu anda oluşturulan değişken üzerinde hiçbir etkisi yok.  Değişken için veri türü değeri tarafından belirlenir.  

Değişken için ilk değeri ayarlarsanız, doğru veri türü olarak yapılandırılmalıdır.  Aşağıdaki tabloda, izin verilen farklı veri türleri ve kendi sözdizimi sağlar.  JSON değerleri her zaman tırnak içindeki herhangi bir özel karakter tırnak işaretleri içine beklenir unutmayın.  Örneğin, bir dize değeri tırnak işaretlerini dize tarafından belirtilecek (çıkış karakterini kullanma (\\)) tırnak işareti bir dizi sayısal bir değer belirtilebilir ancak.

| Veri türü | Açıklama | Örnek | Çözümler |
|:--|:--|:--|:--|
| string   | Değer, çift tırnak içine alın.  | "\"Merhaba Dünya\"" | "Hello world" |
| numeric  | Tek tırnak işaretleri ile sayısal değer.| "64" | 64 |
| boolean  | **doğru** veya **false** tırnak içinde.  Bu değer küçük harfli olması gerektiğini unutmayın. | "true" | true |
| datetime | Seri hale getirilmiş bir tarih değeri.<br>Bu değer için belirli bir tarih oluşturmak için PowerShell'de ConvertTo-Json cmdlet'ini kullanabilirsiniz.<br>Örnek: get-date "5/24/2017 13:14:57" \| ConvertTo-Json | "\\/Date(1495656897378)\\/" | 2017-05-24 13:14:57 |

## <a name="modules"></a>Modüller
Yönetim çözümünüzü tanımlamak gerekmez [genel modüller](../../automation/automation-integration-modules.md) bunlar her zaman Otomasyon hesabınızda kullanılabilir olacağı için runbook'larınız tarafından kullanılır.  Runbook'larınız tarafından kullanılan başka bir modül için kaynak eklemeniz gerekir.

[Tümleştirme modülleri](../../automation/automation-integration-modules.md) bir türüne sahip **Microsoft.Automation/automationAccounts/modules** ve aşağıdaki yapıya sahiptir.  Kopyalayabilir ve bu kod parçacığı, çözüm dosyasına yapıştırın ve parametre adlarını değiştirmek için bu genel değişkenler ve parametreler içerir.

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


Modül kaynakların özellikleri aşağıdaki tabloda açıklanmıştır.

| Özellik | Açıklama |
|:--- |:--- |
| contentLink |Modül içeriğini belirtir. <br><br>Uri - modülün içeriğinin URI'si.  Bu, PowerShell ve komut dosyası runbook'ları için bir .ps1 dosyası ve bir graf runbook için dışarı aktarılan grafik runbook dosyası olacaktır.  <br> Sürüm - kendi izleme için Modül sürümü. |

Runbook için önce runbook'u oluşturduğunuzdan emin olun modülü kaynağı bağlı olmalıdır.

### <a name="updating-modules"></a>Modülleri güncelleştirme
Ardından bir zamanlama kullanan bir runbook'u içeren bir yönetim çözümü güncelleştirmek ve bu runbook tarafından kullanılan yeni bir modül çözümünüzün yeni sürüme sahip, runbook modülünün eski sürümünü kullanabilirsiniz.  Aşağıdaki runbook'lar çözümünüze ekleyin ve diğer runbook'ların önce çalıştırılacak bir iş oluşturmak gerekir.  Bunun herhangi bir modül olarak güncelleştirildiğinden emin olmanızı sağlar runbook'lar yüklenmeden önce gerekli.

* [Güncelleştirme ModulesinAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/) tüm çözümünüzdeki runbook'lar tarafından kullanılan modüller en son sürüm olduğundan emin olun.  
* [ReRegisterAutomationSchedule-MS-Mgmt](https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/) tüm runbook'ları bunlara ile kullanmak üzere en son modülleri bağlı emin olmak için zamanlama kaynakları yeniden kaydettirin.




## <a name="sample"></a>Örnek
Aşağıdaki kaynakları içeren bir içeren bir çözümü bir örnek aşağıda verilmiştir:

- Runbook.  Bir ortak GitHub deposu içinde depolanan bir örnek runbook budur.
- Çözüm yüklendiğinde runbook'u başlatan Otomasyon işi.
- Zamanlama ve iş zamanlaması, düzenli aralıklarla runbook'u başlatın.
- Sertifika.
- Kimlik bilgisi.
- değişken.
- Modül.  Bu [OMSIngestionAPI Modülü](https://www.powershellgallery.com/packages/OMSIngestionAPI/1.5) Log Analytics'e veri yazmak için. 

Örnek kullanır [standart çözüm parametreleri]( solutions-solution-file.md#parameters) yaygın olarak kaynak tanımlarında runbook'a kod değerleri aksine bir çözümde kullanılan değişkenler.


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
            "Description": "Start time for schedule."
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
* [Çözümünüze bir görünüm ekleyin]( solutions-resources-views.md) toplanan verileri görselleştirmek için.
