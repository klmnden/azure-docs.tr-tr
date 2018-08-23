---
title: Ortak PowerShell komutları için Azure sanal makineler | Microsoft Docs
description: Oluşturma ve yönetme, azure'da Windows VM'ler başlamanıza yardımcı olmak için ortak PowerShell komutları.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ba3839a2-f3d5-4e19-a5de-95bfb1c0e61e
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/01/2018
ms.author: cynthn
ms.openlocfilehash: cb9f03ab87079ba33135840e3e7599d2e8cc95e9
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2018
ms.locfileid: "42061486"
---
# <a name="common-powershell-commands-for-creating-and-managing-azure-virtual-machines"></a>Ortak PowerShell komutları için oluşturma ve Azure sanal makineleri yönetme

Bu makale, oluşturma ve Azure aboneliğinizdeki sanal makineleri yönetmek için kullanabileceğiniz Azure PowerShell komutlarının bazı kapsar.  Daha ayrıntılı belirli komut satırı anahtarları ve seçenekleri konusunda yardım için kullanabileceğiniz **Get-Help** *komut*.

Azure PowerShell’in en son sürümünü yükleme, aboneliğinizi seçme ve hesabınızda oturum açma hakkında bilgi almak için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview).

Birden fazla komutlar bu makaledeki çalışıyorsa bu değişkenler için yararlı olabilir:

- $location - sanal makine konumu. Kullanabileceğiniz [Get-AzureRmLocation](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermlocation) bulmak için bir [coğrafi](https://azure.microsoft.com/regions/) için çalışır.
- $myResourceGroup - sanal makineyi içeren kaynak grubunun adı.
- $myVM - sanal makinenin adı.

## <a name="create-a-vm---simplified"></a>Basitleştirilmiş bir VM - oluşturma

| Görev | Komut |
| ---- | ------- |
| Basit bir VM oluşturma | [Yeni-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) -$myVM adı <BR></BR><BR></BR> Yeni-AzureRMVM sahip bir dizi *Basitleştirilmiş* parametreleri, tüm gerekli olan tek bir ad olduğu. Değer için - ad tüm yeni bir VM oluşturmak için gereken kaynakları adı olarak kullanılacaktır. Daha fazla belirtebilirsiniz, ancak gereken budur.|
| Özel görüntüden VM oluşturma | Yeni-AzureRmVm - ResourceGroupName $myResourceGroup-adı "Myımage" $myVM IMAGENAME-$location konumu  <BR></BR><BR></BR>Kendi oluşturmuş olmanız gerekir [yönetilen bir görüntü](capture-image-resource.md). Birden çok, bir görüntü kullanabilirsiniz birbirinin aynısı olan Vm'leri. |



## <a name="create-a-vm-configuration"></a>Bir VM yapılandırması oluşturun

| Görev | Komut |
| ---- | ------- |
| Bir VM yapılandırması oluşturun |$vm = [New-AzureRmVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig) - VMName $myVM - VMSize "Standard_D1_v1"<BR></BR><BR></BR>VM yapılandırması tanımlayın veya VM için ayarları güncelleştirmek için kullanılır. Yapılandırma ve VM adını başlatılır ve kendi [boyutu](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). |
| Yapılandırma ayarları Ekle |$vm = [kümesi AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) - VM $vm-Windows - ComputerName $myVM-$cred - ProvisionVMAgent kimlik bilgisi - EnableAutoUpdate<BR></BR><BR></BR>İşletim sistemi ayarları dahil olmak üzere [kimlik bilgilerini](https://technet.microsoft.com/library/hh849815.aspx) New-AzureRmVMConfig kullanarak daha önce oluşturduğunuz yapılandırma nesnesine eklenmelidir. |
| Bir ağ arabirimi ekleyin |$vm = [Add-Azurermvmnetworkınterface](https://docs.microsoft.com/powershell/module/azurerm.compute/Add-AzureRmVMNetworkInterface) - VM $vm-kimliği $nic Kimliği<BR></BR><BR></BR>Bir VM olmalıdır bir [ağ arabirimi](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) bir sanal ağda iletişim kurabilmek için. Ayrıca [Get-Azurermnetworkınterface](https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) var olan bir ağ arabirimi nesnesi alınamıyor. |
| Platform görüntüsü belirtin |$vm = [kümesi AzureRmVMSourceImage](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmsourceimage) - VM $vm - PublisherName "publisher_name"-"publisher_offer" Teklif - Skus "product_sku"-"son" sürüm<BR></BR><BR></BR>[Görüntü bilgilerini](cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) New-AzureRmVMConfig kullanarak daha önce oluşturduğunuz yapılandırma nesnesine eklenmelidir. Bu komuttan döndürülen nesne yalnızca işletim sistemi diski, bir platform görüntüsü kullanmasına izin kümesi kullanılır. |
| VM oluşturma |[Yeni-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) - ResourceGroupName $myResourceGroup-konum $location - VM $vm<BR></BR><BR></BR>Tüm kaynaklar oluşturulan bir [kaynak grubu](../../azure-resource-manager/powershell-azure-resource-manager.md). Bu komutu çalıştırmadan önce New-AzureRmVMConfig, Set-AzureRmVMOperatingSystem, Set-AzureRmVMSourceImage, Add-Azurermvmnetworkınterface ve Set-AzureRmVMOSDisk çalıştırın. |
| Bir VM'yi güncelleştirin |[Update-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvm) - ResourceGroupName $myResourceGroup - VM $vm<BR></BR><BR></BR>Get-AzureRmVM kullanarak geçerli VM yapılandırmasını almak, VM nesnesini yapılandırma ayarlarını değiştirin ve ardından bu komutu çalıştırın. |

## <a name="get-information-about-vms"></a>VM'ler hakkında bilgi edinin

| Görev | Komut |
| ---- | ------- |
| Bir abonelikte Vm'leri listeleme |[Get-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvm) |
| Bir kaynak grubundaki Vm'leri listeleme |Get-AzureRmVM - ResourceGroupName $myResourceGroup<BR></BR><BR></BR>Aboneliğinizde kaynak gruplarının bir listesini almak için kullanın [Get-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermresourcegroup). |
| VM hakkında bilgi alma |Get-AzureRmVM - ResourceGroupName $myResourceGroup-$myVM adı |

## <a name="manage-vms"></a>VM’leri yönetme
| Görev | Komut |
| --- | --- |
| VM başlatma |[Start-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/start-azurermvm) - ResourceGroupName $myResourceGroup-$myVM adı |
| VM durdurma |[Stop-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/stop-azurermvm) - ResourceGroupName $myResourceGroup-$myVM adı |
| Çalışan bir sanal Makineyi yeniden başlatın |[Restart-Azurermvm'ye](https://docs.microsoft.com/powershell/module/azurerm.compute/restart-azurermvm) - ResourceGroupName $myResourceGroup-$myVM adı |
| VM silme |[Remove-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/remove-azurermvm) - ResourceGroupName $myResourceGroup-$myVM adı |


## <a name="next-steps"></a>Sonraki adımlar
* Bir sanal makine oluşturmak için temel adımları görmek [Resource Manager ve PowerShell kullanarak bir Windows VM oluşturma](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

