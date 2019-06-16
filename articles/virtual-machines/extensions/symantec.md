---
title: Bir Windows Azure VM üzerinde Symantec Endpoint Protection yükle | Microsoft Docs
description: Yükleme ve bir yeni veya var olan Azure Klasik dağıtım modeliyle oluşturulan sanal makinesinde Symantec uç nokta koruması güvenlik uzantısı yapılandırma hakkında bilgi edinin.
services: virtual-machines-windows
documentationcenter: ''
author: roiyz
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: 19dcebc7-da6b-4510-907b-d64088e81fa2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: roiyz
ms.openlocfilehash: 65b52c88741e618e8048451370918b06db73a651
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60617886"
---
# <a name="how-to-install-and-configure-symantec-endpoint-protection-on-a-windows-vm"></a>Bir Windows VM’de Symantec Uç Nokta Koruması yükleme ve yapılandırma
> [!IMPORTANT] 
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

Bu makalede yükleme ve Symantec Endpoint Protection istemcisini bir var olan Windows Server çalıştıran sanal makine üzerinde (VM) yapılandırma gösterilmektedir. Bu tam istemci virüs ve casus yazılımdan koruma ve güvenlik duvarı yetkisiz erişim önleme gibi hizmetler içerir. İstemci, VM Aracısı'nı kullanarak bir güvenlik uzantısı olarak yüklenir.

Symantec'ten bir şirket içi çözüm için mevcut bir abonelik varsa, Azure sanal makinelerinizi korumak için kullanabilirsiniz. Bir müşteri henüz değilseniz, bir deneme aboneliğine kaydolabilirsiniz. Bu çözüm hakkında daha fazla bilgi için bkz. [Symantec Endpoint Protection'ı Microsoft Azure platformu][Symantec]. Bu sayfa, lisans bilgilerini ve Symantec müşteri zaten kullanıyorsanız istemciyi yüklemeye ilişkin yönergeler bağlantıları da vardır.

## <a name="install-symantec-endpoint-protection-on-an-existing-vm"></a>Mevcut bir sanal makinesinde Symantec Endpoint Protection yükle
Başlamadan önce aşağıdakiler gerekir:

* Azure PowerShell modülü, sürüm 0.8.2 ya da daha sonra iş bilgisayarınızda. İle yüklü olduğu bir Azure PowerShell sürümünü kontrol edebilirsiniz **Get-Module azure | format-table sürüm** komutu. Yönergeler ve en son sürüme bir bağlantı için bkz. [yükleme ve yapılandırma, Azure PowerShell][PS]. Kullanarak Azure aboneliği için oturum açın `Add-AzureAccount`.
* Azure sanal Makinesi'nde çalışan VM Aracısı.

İlk olarak, VM Aracısı sanal makinede zaten yüklü olduğunu doğrulayın. Bulut hizmeti adı ve sanal makine adı girin ve ardından yönetici düzeyinde Azure PowerShell komut isteminde aşağıdaki komutları çalıştırın. Dahil olmak üzere, tırnak işaretleri içindeki her şeyi değiştirin < ve > karakterleri.

> [!TIP]
> Sanal makine adları ve bulut hizmeti bilmiyorsanız, çalıştırma **Get-AzureVM** geçerli aboneliğinizdeki tüm sanal makinelerin adlarını listelemek için.

```powershell
$CSName = "<cloud service name>"
$VMName = "<virtual machine name>"
$vm = Get-AzureVM -ServiceName $CSName -Name $VMName
write-host $vm.VM.ProvisionGuestAgent
```

Varsa **write-host** komutunu görüntüler **True**, VM Aracısı yüklenir. Bu görüntüler **False**, yönergeleri ve Azure Web günlüğü gönderisinde indirme bağlantısını [VM aracısı ve uzantıları - 2. bölüm][Agent].

VM aracısını yüklediyseniz, Symantec Endpoint Protection aracısını yüklemek için şu komutları çalıştırın.

```powershell
$Agent = Get-AzureVMAvailableExtension -Publisher Symantec -ExtensionName SymantecEndpointProtection

Set-AzureVMExtension -Publisher Symantec –Version $Agent.Version -ExtensionName SymantecEndpointProtection \
    -VM $vm | Update-AzureVM
```

Symantec güvenlik uzantısı yüklü olduğundan ve güncel olduğunu doğrulamak için:

1. Sanal makinede oturum açın. Yönergeler için [oturum açma bir sanal makine çalıştıran Windows Server][Logon].
2. Windows Server 2008 R2 için tıklatın **Başlat > Symantec Endpoint Protection**. Windows Server 2012 veya başlangıç ekranından Windows Server 2012 R2 için yazın **Symantec**ve ardından **Symantec Endpoint Protection**.
3. Gelen **durumu** sekmesinde **durumu Symantec Endpoint Protection** penceresinde güncelleştirmeleri uygulayabilir veya gerekirse yeniden başlatın.

## <a name="additional-resources"></a>Ek kaynaklar
[Nasıl yapılır Windows Server çalıştıran bir sanal makinede oturum açma][Logon]

[Azure VM uzantıları ve özellikleri][Ext]

<!--Link references-->
[Symantec]: https://www.symantec.com/connect/blogs/symantec-endpoint-protection-now-microsoft-azure

[Create]:../windows/classic/tutorial.md

[PS]: /powershell/azureps-cmdlets-docs

[Agent]: https://go.microsoft.com/fwlink/p/?LinkId=403947

[Logon]:../windows/classic/connect-logon.md

[Ext]: https://go.microsoft.com/fwlink/p/?linkid=390493
