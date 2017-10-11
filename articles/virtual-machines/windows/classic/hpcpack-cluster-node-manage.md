---
title: "HPC Pack küme bilgi işlem düğümleri yönetme | Microsoft Docs"
description: "Ekle, Kaldır, başlatmak ve HPC Pack 2012 R2 küme işlem düğümlerine Azure durdurmak için PowerShell komut dosyası araçları hakkında bilgi edinin"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: 4193f03b-94e9-4704-a7ad-379abde063a9
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 12/29/2016
ms.author: danlep
ms.openlocfilehash: dc9f354191b9e80ff6a01bd401a874c6998bda79
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="manage-the-number-and-availability-of-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a>Azure’da bir HPC Pack kümesindeki işlem düğümlerinin sayısını ve kullanılabilirliğini yönetme
Azure Vm'lerinde bir HPC Pack 2012 R2 kümesi oluşturduysanız, kolayca eklemek, kaldırmak, (sağlayamaz) Başlat veya Durdur (deprovision) yolları isteyebilirsiniz bazı kümedeki düğüm Vm'lerle işlem. Bu görevleri gerçekleştirmek için VM baş düğümünde yüklü olan Azure PowerShell betikleri çalıştırın. Bu komut dosyaları sayısı ve HPC paketi küme kaynaklarınızın kullanılabilirliğini maliyetleri denetimi denetlemenize yardımcı.

> [!IMPORTANT] 
> Bu makale, yalnızca Azure Klasik dağıtım modeli kullanılarak oluşturulan kümelerde HPC Pack 2012 R2 için geçerlidir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.
> Ayrıca, bu makalede açıklanan PowerShell betikleri HPC Pack 2016'da kullanılabilir değildir.

## <a name="prerequisites"></a>Ön koşullar
* **Azure VM'de HPC Pack 2012 R2 küme**: Klasik dağıtım modelinde bir HPC Pack 2012 R2 kümesi oluşturun. Örneğin, Azure Marketi ve bir Azure PowerShell Betiği HPC Pack 2012 R2 VM görüntüsünü kullanarak dağıtım otomatikleştirebilirsiniz. Bilgi ve Önkoşullar için bkz: [HPC Kümesi ile HPC Pack Iaas dağıtım komut dosyası oluşturma](hpcpack-cluster-powershell-script.md).
  
    Dağıtımdan sonra düğümü yönetim komut dosyaları % CCP bulun\_giriş % bin klasörü baş düğüm. Her komut dosyalarının, yönetici olarak çalıştırın.
* **Azure yayımlama ayarları dosyası veya yönetim sertifikası**: baş düğüm aşağıdakilerden birini yapmanız gerekir:
  
  * **Yayımlama ayarları dosyasını içeri aktar Azure**. Bunu yapmak için baş düğümünde aşağıdaki Azure PowerShell cmdlet'lerini çalıştırın:
    
    ```PowerShell
    Get-AzurePublishSettingsFile
    
    Import-AzurePublishSettingsFile –PublishSettingsFile <publish settings file>
    ```
  * **Azure yönetim sertifikası baş düğümünde yapılandırma**. .Cer dosyanız varsa, CurrentUser\My sertifika deposuna içeri aktarın ve ardından Azure ortamınıza (AzureCloud veya AzureChinaCloud) için aşağıdaki Azure PowerShell cmdlet'ini çalıştırın:
    
    ```PowerShell
    Set-AzureSubscription -SubscriptionName <Sub Name> -SubscriptionId <Sub ID> -Certificate (Get-Item Cert:\CurrentUser\My\<Cert Thrumbprint>) -Environment <AzureCloud | AzureChinaCloud>
    ```

## <a name="add-compute-node-vms"></a>İşlem düğümü sanal makineleri ekleyin
İşlem düğümleri eklemek **Ekle HpcIaaSNode.ps1** komut dosyası.

### <a name="syntax"></a>Sözdizimi
```PowerShell
Add-HPCIaaSNode.ps1 [-ServiceName] <String> [-ImageName] <String>
 [-Quantity] <Int32> [-InstanceSize] <String> [-DomainUserName] <String> [[-DomainUserPassword] <String>]
 [[-NodeNameSeries] <String>] [<CommonParameters>]

```
### <a name="parameters"></a>Parametreler
* **ServiceName**: yeni düğümü VM'ler işlem bulut hizmeti adını eklenir.
* **Görüntü adı**: Klasik Azure portalında veya Azure PowerShell cmdlet'i aracılığıyla alınabilir Azure VM görüntü adı **Get-AzureVMImage**. Görüntünün aşağıdaki gereksinimleri karşılamalıdır:
  
  1. Bir Windows işletim sistemi yüklenmelidir.
  2. HPC Pack ise bilgi işlem düğümü rolü yüklü olmalıdır.
  3. Görüntüyü özel görüntü kullanıcı kategori, ortak bir Azure VM görüntüsü değil olmalıdır.
* **Miktar**: işlem düğümü sanal makinelerin eklenmesi sayısı.
* **InstanceSize**: işlem düğümü VM'ler boyutu.
* **DomainUserName**: yeni VM'ler etki alanına katılmak için kullanılan etki alanı kullanıcı adı.
* **DomainUserPassword**: etki alanı kullanıcı parolası.
* **NodeNameSeries** (isteğe bağlı): işlem düğümlerini için desen adlandırma. Biçimi olmalıdır &lt; *kök\_adı*&gt;&lt;*Başlat\_numarası*&gt;%. Örneğin, MyCN11 başlayan işlem düğümü adları dizi bir MyCN % %10 anlamına gelir. Belirtilmezse, betik HPC küme serisinde adlandırma yapılandırılan düğüm kullanır.

### <a name="example"></a>Örnek
Aşağıdaki örnek, bulut hizmeti 20 boyutu büyük işlem düğümü VM'ler ekler *hpcservice1*bağlı olarak VM görüntüsü *hpccnimage1*.

```PowerShell
Add-HPCIaaSNode.ps1 –ServiceName hpcservice1 –ImageName hpccniamge1
–Quantity 20 –InstanceSize Large –DomainUserName <username>
-DomainUserPassword <password>
```


## <a name="remove-compute-node-vms"></a>İşlem düğümü VM'ler Kaldır
İşlem düğümleri kaldırma **Kaldır HpcIaaSNode.ps1** komut dosyası.

### <a name="syntax"></a>Sözdizimi
```PowerShell
Remove-HPCIaaSNode.ps1 -Name <String[]> [-DeleteVHD] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]

Remove-HPCIaaSNode.ps1 -Node <Object> [-DeleteVHD] [-Force] [-Confirm] [<CommonParameters>]
```

### <a name="parameters"></a>Parametreler
* **Ad**: küme düğümlerinin adlarını kaldırılacak. Joker karakterleri desteklenir. Parametre kümesi adı adıdır. Her ikisini birden belirtemezsiniz **adı** ve **düğümü** parametreleri.
* **Düğüm**: HpcNode nesne HPC PowerShell cmdlet'i alınabilir kaldırılacak, düğümler için [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx). Parametre kümesi adı düğümdür. Her ikisini birden belirtemezsiniz **adı** ve **düğümü** parametreleri.
* **DeleteVHD** (isteğe bağlı): kaldırılır VM'ler için ilişkili diskler silmek için ayarı.
* **Zorla** (isteğe bağlı): kaldırmadan önce çevrimdışı HPC düğümleri zorlamak için ayarı.
* **Onayla** (isteğe bağlı): komutu çalıştırmadan önce onaylamanız için komut istemi.
* **WhatIf**: komutu çalıştırmadan komut çalıştırıldığında ne olacağını açıklamak için ayarı.

### <a name="example"></a>Örnek
Aşağıdaki örnek düğümleri başlayan adlarla çevrimdışı zorlar *HPCNode-CN -* düğümleri ve bunların ilişkili diskler kaldırır.

```PowerShell
Remove-HPCIaaSNode.ps1 –Name HPCNodeCN-* –DeleteVHD -Force
```

## <a name="start-compute-node-vms"></a>İşlem düğümü sanal makineleri Başlat
Başlangıç işlem düğümleriyle **başlangıç HpcIaaSNode.ps1** komut dosyası.

### <a name="syntax"></a>Sözdizimi
```PowerShell
Start-HPCIaaSNode.ps1 -Name <String[]> [<CommonParameters>]

Start-HPCIaaSNode.ps1 -Node <Object> [<CommonParameters>]
```
### <a name="parameters"></a>Parametreler
* **Ad**: küme düğümlerinin adlarını başlatılacak. Joker karakterleri desteklenir. Parametre kümesi adı adıdır. Her ikisini birden belirtemezsiniz **adı** ve **düğümü** parametreleri.
* **Düğüm**-HpcNode nesne HPC PowerShell cmdlet'i alınabilir başlatılacak düğümler için [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx). Parametre kümesi adı düğümdür. Her ikisini birden belirtemezsiniz **adı** ve **düğümü** parametreleri.

### <a name="example"></a>Örnek
Aşağıdaki örnek düğümleri başlayan adlarla başlatır *HPCNode-CN -*.

```PowerShell
Start-HPCIaaSNode.ps1 –Name HPCNodeCN-*
```

## <a name="stop-compute-node-vms"></a>İşlem düğümü VM'ler Durdur
İşlem düğümleriyle Durdur **Stop-HpcIaaSNode.ps1** komut dosyası.

### <a name="syntax"></a>Sözdizimi
```PowerShell
Stop-HPCIaaSNode.ps1 -Name <String[]> [-Force] [<CommonParameters>]

Stop-HPCIaaSNode.ps1 -Node <Object> [-Force] [<CommonParameters>]
```

### <a name="parameters"></a>Parametreler
* **Ad**-durdurulacak küme düğümlerinin adlarını. Joker karakterleri desteklenir. Parametre kümesi adı adıdır. Her ikisini birden belirtemezsiniz **adı** ve **düğümü** parametreleri.
* **Düğüm**: HpcNode nesne HPC PowerShell cmdlet'i alınabilir durdurulacak düğümler için [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx). Parametre kümesi adı düğümdür. Her ikisini birden belirtemezsiniz **adı** ve **düğümü** parametreleri.
* **Zorla** (isteğe bağlı): durdurmadan önce çevrimdışı HPC düğümleri zorlamak için ayarı.

### <a name="example"></a>Örnek
Aşağıdaki örnek çevrimdışı düğümleri başlayan adlarla zorlar *HPCNode-CN -* ve ardından düğümler durdurur.

```PowerShell
Stop-HPCIaaSNode.ps1 –Name HPCNodeCN-* -Force
```

## <a name="next-steps"></a>Sonraki adımlar
* Otomatik olarak büyütür veya küme düğümleri, işler ve görevleri küme üzerinde geçerli iş yükü göre daraltmak için bkz: [otomatik olarak Büyüt ve Azure HPC paketi küme kaynaklarında göre küme iş yükü küçültme](hpcpack-cluster-node-autogrowshrink.md).

