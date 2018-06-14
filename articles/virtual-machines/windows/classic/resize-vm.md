---
title: Klasik dağıtım modelinde - Azure Windows VM yeniden boyutlandırmak | Microsoft Docs
description: Azure Powershell kullanarak Klasik dağıtım modelinde oluşturulmuş bir Windows sanal makine yeniden boyutlandırın.
services: virtual-machines-windows
documentationcenter: ''
author: Drewm3
manager: timlt
editor: ''
tags: azure-service-management
ms.assetid: e3038215-001c-406e-904d-e0f4e326a4c7
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/19/2016
ms.author: drewm
ms.openlocfilehash: 4277bc8394c7ba140291e9dc776162e87deab96b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23928247"
---
# <a name="resize-a-windows-vm-created-in-the-classic-deployment-model"></a>Windows Klasik dağıtım modelinde oluşturulan bir VM'yi yeniden boyutlandırın
Bu makalede Windows Azure Powershell kullanarak Klasik dağıtım modelinde oluşturulan bir VM'yi yeniden boyutlandırın gösterilmiştir.

Bir VM yeniden boyutlandırma yeteneği değerlendirirken, sanal makine yeniden boyutlandırmak kullanılabilir boyutları denetlemenize iki kavram vardır. İlk VM dağıtıldığı bölge kavramdır. Azure bölgeleri web sayfası Hizmetleri sekmesi altında bölgede kullanılabilir VM boyutları listesidir. Şu anda, VM barındıran fiziksel donanımı ikinci kavramdır. Sanal makineleri barındıran fiziksel sunucuları ortak fiziksel donanım kümelerde birlikte gruplandırılır. İstenen yeni VM boyutu şu anda VM barındırma donanım küme tarafından desteklenip desteklenmediğini VM boyutunu değiştirme yöntemi bağlı olarak farklılık gösterir.

> [!IMPORTANT] 
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Ayrıca [Resource Manager dağıtım modelinde oluşturulan bir VM'yi yeniden boyutlandırın](../resize-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="add-your-account"></a>Hesabınızı ekleme
Klasik Azure kaynakları ile çalışmak için Azure PowerShell yapılandırmanız gerekir. Azure Klasik kaynakları yönetmek için PowerShell yapılandırmak için aşağıdaki adımları izleyin.

1. PowerShell komut isteminde yazın `Add-AzureAccount` tıklatıp **Enter**. 
2. Azure aboneliğinizle ilişkili e-posta adresini yazın ve tıklatın **devam**. 
3. Hesabınızın parolasını yazın. 
4. Tıklatın **oturum**. 

## <a name="resize-in-the-same-hardware-cluster"></a>Aynı donanım kümeye yeniden boyutlandırma
VM VM barındırma donanım kümedeki kullanılabilir boyut yeniden boyutlandırmak için aşağıdaki adımları gerçekleştirin.

1. VM içeren bulut hizmeti barındırma donanım kümedeki kullanılabilir VM boyutları listelemek için aşağıdaki PowerShell komutunu çalıştırın.
   
    ```powershell
    Get-AzureService | where {$_.ServiceName -eq "<cloudServiceName>"}
    ```
2. VM yeniden boyutlandırmak için aşağıdaki komutları çalıştırın.
   
    ```powershell
    Get-AzureVM -ServiceName <cloudServiceName> -Name <vmName> | Set-AzureVMSize -InstanceSize <newVMSize> | Update-AzureVM
    ```

## <a name="resize-on-a-new-hardware-cluster"></a>Yeni bir donanım kümede yeniden boyutlandırma
VM VM, bulut barındırma donanım kümede kullanılamaz bir boyuta yeniden boyutlandırmak için hizmet ve bulut hizmetindeki tüm sanal makineleri yeniden oluşturulmalıdır. Bulut hizmetindeki tüm sanal makineleri bir donanım kümesine desteklenen bir boyutta olması gerekir böylece her bir bulut hizmeti bir tek bir donanım kümesine barındırılır. Aşağıdaki adımlar, silme ve bulut hizmeti yeniden VM yeniden boyutlandırmak nasıl anlatmaktadır.

1. Bölgede VM boyutları listelemek için aşağıdaki PowerShell komutunu çalıştırın. 
   
    ```powershell
    Get-AzureLocation | where {$_.Name -eq "<locationName>"}
    ```
2. VM yeniden boyutlandırılıp içeren bulut hizmeti her VM için tüm yapılandırma ayarlarını not edin. 
3. Her VM için diskleri korumak için bu seçenek belirlendiğinde bulut hizmetindeki tüm sanal makineleri silin.
4. İstenen VM boyutu kullanarak yeniden boyutlandırılıp VM yeniden oluşturun.
5. Şimdi bulut hizmetini barındıran donanım kümede bir VM boyutu kullanarak bulut hizmeti olan diğer tüm VM'ler yeniden oluşturun.

Silme ve yeni bir VM boyutu kullanarak bir bulut hizmeti yeniden oluşturmak için örnek betik bulunabilir [burada](https://github.com/Azure/azure-vm-scripts). 

## <a name="next-steps"></a>Sonraki adımlar
* [Resource Manager dağıtım modelinde oluşturulan bir VM'yi yeniden boyutlandırın](../resize-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

