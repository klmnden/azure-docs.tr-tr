---
title: Azure automation'da Python 2 paketlerini yönetme
description: Bu makalede, Azure automation'da Python 2 paketlerini yönetmek açıklar.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 09/11/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: de0998dffeac54db5311bbcde1c9499488b23556
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54434981"
---
# <a name="manage-python-2-packages-in-azure-automation"></a>Azure automation'da Python 2 paketlerini yönetme

Azure Otomasyonu Linux karma Runbook çalışanları ve Azure üzerinde Python 2 runbook'ları çalıştırmanıza olanak sağlar. Runbook'ların basitleştirmesi yardımcı olmak için gerek duyduğunuz modülleri içeri aktarmak için Python paketlerini kullanabilirsiniz. Bu makalede nasıl yönetmek ve Python paketleri, Azure Automation'da kullanma açıklanır.

## <a name="import-packages"></a>Paketleri içeri aktarma

Otomasyon hesabınızı seçin **Python 2 paketleri** altında **paylaşılan kaynakları**. Tıklayın **+ Python 2 paket ekleme**.

![Python paketi ekleme](media/python-packages/add-python-package.png)

Üzerinde **Python 2 Paketi Ekle** sayfasında, yüklemek için yerel bir paket seçin. Paket olabilir bir `.whl` dosya veya `.tar.gz` dosya. Bu onay kutusu seçildiğinde, tıklayın **Tamam** paketini karşıya yüklemek için.

![Python paketi ekleme](media/python-packages/upload-package.png)

Paketi içeri aktarıldıktan sonra listelenmiş olup **Python 2 paketleri** Otomasyon hesabınızdaki sayfası. Paket kaldırma gerekiyorsa, paket seçip **Sil** paketi sayfasında.

![Paket listesi](media/python-packages/package-list.png)

## <a name="use-a-package-in-a-runbook"></a>Bir runbook'ta bir paket kullanma

Bir paketi içe aktardıktan sonra artık bir runbook'ta kullanabilirsiniz. Aşağıdaki örnekte [ Azure Otomasyonu yardımcı programı paket](https://github.com/azureautomation/azure_automation_utility). Bu paket, Azure Otomasyonu ile Python kullanma kolaylaştırır. Paketini kullanmak için GitHub deposunda yönergeleri izleyin ve kullanarak runbook'a ekleme `from azure_automation_utility import get_automation_runas_credential` örneğin farklı çalıştır hesabı almak için işlev içeri aktarmak.

```python
import azure.mgmt.resource
import automationassets
from azure_automation_utility import get_automation_runas_credential

# Authenticate to Azure using the Azure Automation RunAs service principal
runas_connection = automationassets.get_automation_connection("AzureRunAsConnection")
azure_credential = get_automation_runas_credential()

# Intialize the resource management client with the RunAs credential and subscription
resource_client = azure.mgmt.resource.ResourceManagementClient(
    azure_credential,
    str(runas_connection["SubscriptionId"]))

# Get list of resource groups and print them out
groups = resource_client.resource_groups.list()
for group in groups:
    print group.name
```

## <a name="develop-and-test-runbooks-offline"></a>Geliştirin ve runbook'ları, çevrimdışı test

Geliştirmek ve Python 2 runbook'larınızı çevrimdışı test etmek için kullanabileceğiniz [Azure Otomasyonu python benzetilmiş varlıklar](https://github.com/azureautomation/python_emulated_assets) github'da modülü. Bu modül, kimlik bilgileri, değişkenler, bağlantılar ve sertifikalar gibi paylaşılan kaynaklara başvuran olanak tanır.

## <a name="next-steps"></a>Sonraki adımlar

Python 2 runbook'larını kullanmaya başlamak için bkz: [ilk Python 2 runbook Uygulamam](automation-first-runbook-textual-python2.md)
