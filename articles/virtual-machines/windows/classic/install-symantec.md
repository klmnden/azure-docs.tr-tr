---
title: "Windows Azure VM Symantec Endpoint Protection yükle | Microsoft Docs"
description: "Yükleme ve Symantec Endpoint Protection güvenlik uzantısı bir yeni veya var olan Azure Klasik dağıtım modeli kullanılarak oluşturulmuş VM'deki yapılandırın öğrenin."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 19dcebc7-da6b-4510-907b-d64088e81fa2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: iainfou
ms.openlocfilehash: 1603ebc7ee3c29277f30fbb802bdd8205b92d648
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-install-and-configure-symantec-endpoint-protection-on-a-windows-vm"></a>Bir Windows VM’de Symantec Uç Nokta Koruması yükleme ve yapılandırma
> [!IMPORTANT] 
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

Bu makalede yükleme ve Symantec Endpoint Protection istemcisini bir mevcut Windows Server çalıştıran sanal makine üzerinde (VM) yapılandırma gösterilmektedir. Bu tam istemci virüs ve casus yazılımdan koruma, güvenlik duvarı ve izinsiz giriş önleme gibi hizmetleri içerir. İstemci, VM Aracısı'nı kullanarak bir güvenlik uzantısı olarak yüklenir.

Symantec'ten bir şirket içi çözüm için mevcut bir aboneliğiniz varsa, Azure sanal makinelerinizi korumak için kullanabilirsiniz. Bir müşteri henüz değilseniz, deneme aboneliği için kaydolabilirsiniz. Bu çözüm hakkında daha fazla bilgi için bkz: [Symantec Endpoint Protection Microsoft'un Azure platformunda][Symantec]. Bu sayfa Ayrıca lisans bilgileri ve Symantec müşteri zaten iseniz istemcisini yüklemeye yönelik yönergeleri için bağlantıları vardır.

## <a name="install-symantec-endpoint-protection-on-an-existing-vm"></a>Var olan bir VM Symantec Endpoint Protection yükle
Başlamadan önce aşağıdakiler gerekir:

* Azure PowerShell modülü, sürüm 0.8.2 veya daha sonra iş bilgisayarınızın. İle yüklenen Azure PowerShell sürümü kontrol edebilirsiniz **Get-Module azure | Tablo Biçimlendir sürüm** komutu. Yönergeler ve en son sürüme bir bağlantı için bkz: [yükleme ve yapılandırma Azure PowerShell][PS]. Oturum açma kullanarak Azure aboneliği `Add-AzureAccount`.
* Azure sanal makinede çalışan VM Aracısı.

İlk olarak, VM aracısının sanal makinede zaten yüklü olduğunu doğrulayın. Bulut hizmeti adı ve sanal makine adı girin ve ardından bir yönetici düzeyi Azure PowerShell komut isteminde aşağıdaki komutları çalıştırın. Dahil olmak üzere tırnak işaretleri içindeki her şeyi değiştirin < ve > karakterleri.

> [!TIP]
> Sanal makine adları ve bulut hizmeti bilmiyorsanız, çalıştırmak **Get-AzureVM** geçerli aboneliğiniz tüm sanal makinelerin adlarını listelemek için.

```powershell
$CSName = "<cloud service name>"
$VMName = "<virtual machine name>"
$vm = Get-AzureVM -ServiceName $CSName -Name $VMName
write-host $vm.VM.ProvisionGuestAgent
```

Varsa **write-host** komutu görüntüler **doğru**, VM aracısının yüklü olduğundan. Bunu görüntülerse **False**, yönergeler ve Azure blog postası karşıdan yükleme bağlantısı bkz [VM aracısı ve uzantılar - 2. parça][Agent].

VM Aracısı yüklüyse, Symantec Endpoint Protection aracısını yüklemek için şu komutları çalıştırın.

```powershell
$Agent = Get-AzureVMAvailableExtension -Publisher Symantec -ExtensionName SymantecEndpointProtection

Set-AzureVMExtension -Publisher Symantec –Version $Agent.Version -ExtensionName SymantecEndpointProtection \
    -VM $vm | Update-AzureVM
```

Symantec güvenlik uzantısı yüklendikten ve güncel durumda olduğunu doğrulamak için:

1. Sanal makinede oturum açın. Yönergeler için bkz: [nasıl bir sanal makine çalıştıran Windows Server oturum][Logon].
2. Windows Server 2008 R2 için tıklatın **Başlat > Symantec Endpoint Protection**. Windows Server 2012 veya Windows Server 2012 R2, başlangıç ekranından yazın **Symantec**ve ardından **Symantec Endpoint Protection**.
3. Gelen **durum** sekmesinde **durum Symantec Endpoint Protection** penceresinde, güncelleştirmeleri uygulamak veya gerektiğinde yeniden başlatın.

## <a name="additional-resources"></a>Ek kaynaklar
[Nasıl yapılır Windows Server çalıştıran bir sanal makinede oturum açın][Logon]

[Azure VM uzantıları ve özellikleri][Ext]

<!--Link references-->
[Symantec]: http://www.symantec.com/connect/blogs/symantec-endpoint-protection-now-microsoft-azure

[Create]:tutorial.md

[PS]: /powershell/azureps-cmdlets-docs

[Agent]: http://go.microsoft.com/fwlink/p/?LinkId=403947

[Logon]:connect-logon.md

[Ext]: http://go.microsoft.com/fwlink/p/?linkid=390493
