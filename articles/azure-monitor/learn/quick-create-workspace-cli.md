---
title: Azure CLI kullanarak Log Analytics çalışma alanı oluşturma | Microsoft Docs
description: Azure CLI ile Bulut ve şirket içi ortamınızı gelen yönetim çözümleri ve veri toplamayı etkinleştirmek için Log Analytics çalışma alanı oluşturmayı öğrenin.
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
ms.date: 10/02/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: 635e73a9cbff1c7a18f274db09125be19e1c6bb7
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53074477"
---
# <a name="create-a-log-analytics-workspace-with-azure-cli-20"></a>Azure CLI 2.0 ile Log Analytics çalışma alanı oluşturma

Azure CLI 2.0, komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu hızlı başlangıçta Azure CLI 2.0 bir Log Analytics çalışma alanını kendi veri deposu, veri kaynakları ve çözümleri olan benzersiz bir ortamı olan azure'da dağıtmak için nasıl kullanılacağını gösterir.  Bu makalede açıklanan adımları aşağıdaki kaynaklardan veri toplama işlemini istiyorsanız gereklidir:

* Aboneliğinizdeki Azure kaynakları  
* Şirket içi bilgisayarlar System Center Operations Manager tarafından izlenen  
* System Center Configuration Manager cihaz koleksiyonları  
* Azure depolama biriminden tanılama veya günlük verileri  
 
Azure sanal makinelerini ve Windows veya Linux Vm'leri, ortamınızda gibi diğer kaynakları için aşağıdaki konulara bakın:

* [Azure sanal makinelerden veri toplama](../../azure-monitor/learn/quick-collect-azurevm.md)
* [Karma Linux bilgisayarından verileri toplama](../../azure-monitor/learn/quick-collect-linux-computer.md)
* [Karma Windows bilgisayardan veri topla](quick-collect-windows-computer.md)

Azure aboneliğiniz yoksa, oluşturma [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) başlamadan önce.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.30 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma
Bir çalışma alanı ile oluşturma [az grubu dağıtım oluşturma](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create). Aşağıdaki örnekte adlı bir çalışma alanı oluşturur *TestWorkspace* kaynak grubundaki *Laboratuvar* içinde *eastus* yerel bir Resource Manager şablonu kullanarak konumu Makine. JSON şablonunu, çalışma alanının adı için yalnızca isteyecek şekilde yapılandırılmış ve büyük olasılıkla ortamınızdaki standart bir yapılandırma olarak kullanılacak diğer parametreler için varsayılan bir değer belirtir. Veya, kuruluşunuzda paylaşılan erişim için bir Azure depolama hesabında şablonu depolayabilirsiniz. Şablonları ile çalışma hakkında daha fazla bilgi için bkz: [kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](../../azure-resource-manager/resource-group-template-deploy-cli.md)

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

    ```azurecli
    azure group deployment create --resource-group <my-resource-group> --name <my-deployment-name> --template-file deploylaworkspacetemplate.json
    ```

Dağıtımın tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonuç içeren aşağıdakine benzer bir ileti görürsünüz:

![Dağıtım tamamlandığında örnek sonucu](media/quick-create-workspace-cli/template-output-01.png)

## <a name="next-steps"></a>Sonraki adımlar
Bir çalışma alanı kullanılabilir olduğuna göre telemetri izleme koleksiyonunu yapılandırma, bu verileri çözümlemek için günlük aramaları çalıştıran ve ek veriler ve hakkında analitik bilgiler sağlamak için bir yönetim çözümünü ekleyin.  

* Azure Tanılama veya Azure depolama ile Azure kaynaklarından veri toplamayı etkinleştirmek için bkz: [toplamak Azure hizmeti günlükleri ve Log analytics'teki kullanım ölçümlerini](../../azure-monitor/platform/collect-azure-metrics-logs.md).  
* Ekleme [System Center Operations Manager veri kaynağı olarak](../../azure-monitor/platform/om-agents.md) , Operations Manager yönetim grubuna bildirimde bulunan aracılardan veri toplamak ve Log Analytics çalışma alanınızda depolamak için.  
* Connect [Configuration Manager](../../azure-monitor/platform/collect-sccm.md) hiyerarşideki koleksiyona üye olan bilgisayarlara aktarmak için.  
* Gözden geçirme [yönetim çözümleri](../../azure-monitor/insights/solutions.md) kullanılabilir ve ekleme veya bir çözüm çalışma alanınızdan kaldırın.
