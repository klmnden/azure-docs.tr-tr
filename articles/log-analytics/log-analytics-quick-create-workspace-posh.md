---
title: Azure PowerShell kullanarak Log Analytics çalışma alanı oluşturma | Microsoft Docs
description: Bulut ve şirket içi ortamlarından Azure PowerShell ile yönetim çözümleri ve veri toplamayı etkinleştirmek için Log Analytics çalışma alanı oluşturmayı öğrenin.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptal
ms.date: 09/18/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: b7b8734e648e79ab22a6783e7fab31e942f08eb4
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50418441"
---
# <a name="create-a-log-analytics-workspace-with-azure-powershell"></a>Azure PowerShell ile bir Log Analytics çalışma alanı oluşturma

Azure PowerShell modülü, PowerShell komut satırından veya betik içinden Azure kaynakları oluşturmak ve yönetmek için kullanılır. Bu hızlı başlangıçta Azure PowerShell modülünün bir Log Analytics çalışma alanını kendi veri deposu, veri kaynakları ve çözümleri olan benzersiz bir ortamı olan azure'da dağıtmak için nasıl kullanılacağını gösterir.  Bu makalede açıklanan adımları aşağıdaki kaynaklardan veri toplama işlemini istiyorsanız gereklidir:

* Aboneliğinizdeki Azure kaynakları  
* Şirket içi bilgisayarlar System Center Operations Manager tarafından izlenen  
* System Center Configuration Manager cihaz koleksiyonları  
* Azure depolama biriminden tanılama veya günlük verileri  
 
Azure sanal makinelerini ve Windows veya Linux Vm'leri, ortamınızda gibi diğer kaynakları için aşağıdaki konulara bakın:

* [Azure sanal makinelerden veri toplama](log-analytics-quick-collect-azurevm.md)
* [Karma Linux bilgisayarından verileri toplama](log-analytics-quick-collect-linux-computer.md)
* [Karma Windows bilgisayardan veri topla](log-analytics-quick-collect-windows-computer.md)

Azure aboneliğiniz yoksa, oluşturma [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) başlamadan önce.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz, bu öğretici Azure PowerShell modülü 5.7.0 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma
Bir çalışma alanı ile oluşturma [New-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment). Aşağıdaki örnekte adlı bir çalışma alanı oluşturur *TestWorkspace* kaynak grubundaki *Laboratuvar* içinde *eastus* yerel bir Resource Manager şablonu kullanarak konumu Makine. JSON şablonunu, çalışma alanının adı için yalnızca isteyecek şekilde yapılandırılmış ve büyük olasılıkla ortamınızdaki standart bir yapılandırma olarak kullanılacak diğer parametreler için varsayılan bir değer belirtir. 

Aşağıdaki parametreleri varsayılan değeri ayarlayın:

* Konum - Doğu ABD için varsayılanları
* SKU - Nisan 2018 fiyatlandırma modelinde yayımlanan yeni GB başına fiyatlandırma katmanı varsayılan olarak

>[!WARNING]
>Oluşturma veya yeni Nisan 2018 fiyatlandırma modelini tercih bir Abonelikteki Log Analytics çalışma alanını yapılandırma, yalnızca geçerli Log Analytics fiyatlandırma katmanı ise **PerGB2018**. 
>

### <a name="create-and-deploy-template"></a>Şablon oluşturma ve dağıtma

1. Aşağıdaki JSON söz dizimini kopyalayıp dosyanıza yapıştırın:

    ```json
    {
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceName": {
            "type": "String",
            "metadata": {
              "description": "Specifies the name of the workspace."
            }
        },
        "location": {
            "type": "String",
            "allowedValues": [
              "eastus",
              "westus"
            ],
            "defaultValue": "eastus",
            "metadata": {
              "description": "Specifies the location in which to create the workspace."
            }
        },
        "sku": {
            "type": "String",
            "allowedValues": [
              "Standalone",
              "PerNode",
              "PerGB2018"
            ],
            "defaultValue": "PerGB2018",
            "metadata": {
            "description": "Specifies the service tier of the workspace: Standalone, PerNode, Per-GB"
        }
          }
    },
    "resources": [
        {
            "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('workspaceName')]",
            "apiVersion": "2017-03-15-preview",
            "location": "[parameters('location')]",
            "properties": {
                "sku": {
                    "Name": "[parameters('sku')]"
                },
                "features": {
                    "searchVersion": 1
                }
            }
          }
       ]
    }
    ```

2. Gereksinimlerinizi karşılayacak şekilde şablonunu düzenleyin.  Gözden geçirme [Microsoft.OperationalInsights/workspaces şablon](https://docs.microsoft.com/azure/templates/microsoft.operationalinsights/workspaces) başvuru hangi özellikler ve değerler desteklendiğini öğrenin. 
3. Bu dosyayı farklı Kaydet **deploylaworkspacetemplate.json** yerel bir klasöre.   
4. Bu şablonu dağıtmaya hazırsınız. Şablonu içeren klasörden aşağıdaki komutları kullanın:

    ```powershell
        New-AzureRmResourceGroupDeployment -Name <deployment-name> -ResourceGroupName <resource-group-name> -TemplateFile deploylaworkspacetemplate.json
    ```

Dağıtımın tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonuç içeren aşağıdakine benzer bir ileti görürsünüz:

![Dağıtım tamamlandığında örnek sonucu](media/log-analytics-quick-create-workspace-posh/template-output-01.png)

## <a name="next-steps"></a>Sonraki adımlar
Bir çalışma alanı kullanılabilir olduğuna göre telemetri izleme koleksiyonunu yapılandırma, bu verileri çözümlemek için günlük aramaları çalıştıran ve ek veriler ve hakkında analitik bilgiler sağlamak için bir yönetim çözümünü ekleyin.  

* Azure Tanılama veya Azure depolama ile Azure kaynaklarından veri toplamayı etkinleştirmek için bkz: [toplamak Azure hizmeti günlükleri ve Log analytics'teki kullanım ölçümlerini](log-analytics-azure-storage.md).  
* Ekleme [System Center Operations Manager veri kaynağı olarak](log-analytics-om-agents.md) , Operations Manager yönetim grubuna bildirimde bulunan aracılardan veri toplamak ve Log Analytics çalışma alanınızda depolamak için.  
* Connect [Configuration Manager](log-analytics-sccm.md) hiyerarşideki koleksiyona üye olan bilgisayarlara aktarmak için.  
* Gözden geçirme [yönetim çözümleri](log-analytics-add-solutions.md) kullanılabilir ve ekleme veya bir çözüm çalışma alanınızdan kaldırın.
