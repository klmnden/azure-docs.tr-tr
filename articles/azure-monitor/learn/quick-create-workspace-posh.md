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
ms.topic: conceptual
ms.date: 03/12/2019
ms.author: magoedte
ms.openlocfilehash: 6f27aeb65cb9077011e662c165ca26202546db26
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58905741"
---
# <a name="create-a-log-analytics-workspace-with-azure-powershell"></a>Azure PowerShell ile bir Log Analytics çalışma alanı oluşturma

Azure PowerShell modülü, PowerShell komut satırından veya betik içinden Azure kaynakları oluşturmak ve yönetmek için kullanılır. Bu hızlı başlangıçta Azure İzleyici'de bir Log Analytics çalışma alanı dağıtmak için Azure PowerShell modülünü kullanmayı gösterir. Bir Log Analytics çalışma alanı, Azure İzleyici günlük verileri için benzersiz bir ortamdır. Kendi veri deposu ve yapılandırma her çalışma alanına sahiptir ve veri kaynakları ve çözümleri belirli bir çalışma alanında, verilerini depolamak için yapılandırılır. Aşağıdaki kaynaklardan veri toplama işlemini düşünüyorsanız, Log Analytics çalışma gerektirir:

* Aboneliğinizdeki Azure kaynakları  
* Şirket içi bilgisayarlar System Center Operations Manager tarafından izlenen  
* System Center Configuration Manager cihaz koleksiyonları  
* Azure depolama biriminden tanılama veya günlük verileri  
 
Azure sanal makinelerini ve Windows veya Linux Vm'leri, ortamınızda gibi diğer kaynakları için aşağıdaki konulara bakın:

* [Azure sanal makinelerden veri toplama](../learn/quick-collect-azurevm.md)
* [Karma Linux bilgisayarından verileri toplama](../learn/quick-collect-linux-computer.md)
* [Karma Windows bilgisayardan veri topla](quick-collect-windows-computer.md)

Azure aboneliğiniz yoksa, oluşturma [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) başlamadan önce.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz Bu öğretici Azure PowerShell Az modül gerektirir. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma
Bir çalışma alanı ile [yeni AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment). Aşağıdaki örnekte adlı bir çalışma alanı oluşturur *TestWorkspace* kaynak grubundaki *Laboratuvar* içinde *eastus* yerel bir Resource Manager şablonu kullanarak konumu Makine. JSON şablonunu, çalışma alanının adı için yalnızca isteyecek şekilde yapılandırılmış ve büyük olasılıkla ortamınızdaki standart bir yapılandırma olarak kullanılacak diğer parametreler için varsayılan bir değer belirtir. 

Desteklenen bölgeleri hakkında daha fazla bilgi için bkz: [Log Analytics'in sunulduğu bölgeler](https://azure.microsoft.com/regions/services/) ve Azure İzleyici'deki Ara **ürünü arama** alan. 

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
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
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
            "apiVersion": "2015-11-01-preview",
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

2. Gereksinimlerinizi karşılayacak şekilde şablonunu düzenleyin. Gözden geçirme [Microsoft.OperationalInsights/workspaces şablon](https://docs.microsoft.com/azure/templates/microsoft.operationalinsights/workspaces) başvuru hangi özellikler ve değerler desteklendiğini öğrenin. 
3. Bu dosyayı farklı Kaydet **deploylaworkspacetemplate.json** yerel bir klasöre.   
4. Bu şablonu dağıtmaya hazırsınız. Şablonu içeren klasörden aşağıdaki komutları kullanın:

    ```powershell
        New-AzResourceGroupDeployment -Name <deployment-name> -ResourceGroupName <resource-group-name> -TemplateFile deploylaworkspacetemplate.json
    ```

Dağıtımın tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonuç içeren aşağıdakine benzer bir ileti görürsünüz:

![Dağıtım tamamlandığında örnek sonucu](media/quick-create-workspace-posh/template-output-01.png)

## <a name="next-steps"></a>Sonraki adımlar
Bir çalışma alanı kullanılabilir olduğuna göre telemetri izleme koleksiyonunu yapılandırma, bu verileri çözümlemek için günlük aramaları çalıştıran ve ek veriler ve hakkında analitik bilgiler sağlamak için bir yönetim çözümünü ekleyin.  

* Azure Tanılama veya Azure depolama ile Azure kaynaklarından veri toplamayı etkinleştirmek için bkz: [toplamak Azure hizmeti günlükleri ve ölçümleri kullanılmak üzere Azure İzleyici](../platform/collect-azure-metrics-logs.md).  
* Ekleme [System Center Operations Manager veri kaynağı olarak](../platform/om-agents.md) , Operations Manager yönetim grubuna bildirimde bulunan aracılardan veri toplamak ve Log Analytics çalışma alanınızda depolamak için.  
* Connect [Configuration Manager](../platform/collect-sccm.md) hiyerarşideki koleksiyona üye olan bilgisayarlara aktarmak için.  
* Gözden geçirme [izleme çözümleri](../insights/solutions.md) kullanılabilir ve ekleme veya bir çözüm çalışma alanınızdan kaldırın.
