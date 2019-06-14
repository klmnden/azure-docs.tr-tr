---
title: Azure automation'da Python 2 paketlerini yönetme
description: Bu makalede, Azure automation'da Python 2 paketlerini yönetmek açıklar.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 02/25/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: b53e07d6086f2a02fd1bbd158ffc09dc95b0c377
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60500149"
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

## <a name="import-packages-with-dependencies"></a>Bağımlılıkları olan paketleri içeri aktarma

Azure Otomasyonu bağımlılık python Paketlerine yönelik içeri aktarma işlemi sırasında sorunu çözmezse. Paket ile tüm bağımlılıkları almak için iki yolu vardır. Aşağıdaki adımlar yalnızca biri paketleri Otomasyon hesabınızda içeri aktarmak için kullanılması gerekir.

### <a name="manually-download"></a>El ile yükleme

Üzerinde Windows 64-bit bir makine ile [python2.7](https://www.python.org/downloads/release/latest/python2) ve [pip](https://pip.pypa.io/en/stable/) yüklü bir paketi ve tüm bağımlılıkları yüklemek için aşağıdaki komutu çalıştırın:

```cmd
C:\Python27\Scripts\pip2.7.exe download -d <output dir> <package name>
```

Paketler yüklendikten sonra bunları Otomasyon hesabınıza aktarabilirsiniz.

### <a name="runbook"></a>Runbook

Python runbook'u içeri aktar [Azure Otomasyonu hesabına pypı alma Python 2 paketlerden](https://gallery.technet.microsoft.com/scriptcenter/Import-Python-2-packages-57f7d509) Otomasyon hesabınızda Galerisi. Çalıştırma ayarları emin olun kümesine **Azure** ve parametrelerle bir runbook başlatın. Runbook bir farklı çalıştır hesabı için çalışmak için bu Otomasyon hesabı gerektirir. Her parametre için emin olun anahtarıyla birlikte aşağıdaki liste ve görüntü görüldüğü gibi başlattığınızda:

* -s \<subscriptionId\>
* -g \<resourceGroup\>
* -a \<automationAccount\>
* -m \<modulePackage\>

![Paket listesi](media/python-packages/import-python-runbook.png)

Runbook'un ne indirmek için örneğin, paket belirtmenize olanak tanır `Azure` (Dördüncü parametresinin) tüm Azure modülleri ve tüm bağımlılıklarını, yaklaşık 105 olduğu indirir.

Runbook tamamlandığında denetleyebilirsiniz **Python 2 paketleri** altındaki **paylaşılan kaynakları** doğrulamak için Otomasyon hesabınızda, paket doğru şekilde içeri aktarıldı.

## <a name="use-a-package-in-a-runbook"></a>Bir runbook'ta bir paket kullanma

Bir paket aktardıktan sonra artık bir runbook'ta kullanabilirsiniz. Aşağıdaki örnekte [ Azure Otomasyonu yardımcı programı paket](https://github.com/azureautomation/azure_automation_utility). Bu paket, Azure Otomasyonu ile Python kullanma kolaylaştırır. Paketini kullanmak için GitHub deposunda yönergeleri izleyin ve kullanarak runbook'a ekleme `from azure_automation_utility import get_automation_runas_credential` örneğin farklı çalıştır hesabı almak için işlev içeri aktarmak.

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
