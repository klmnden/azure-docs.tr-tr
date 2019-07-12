---
title: Ortak PowerShell komutları için Azure sanal makineler | Microsoft Docs
description: Oluşturma ve yönetme, azure'da Windows VM'ler başlamanıza yardımcı olmak için ortak PowerShell komutları.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
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
ms.openlocfilehash: cb7e6dd6569cdb05b769f9f79b8dd55e234adcde
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723021"
---
# <a name="common-powershell-commands-for-creating-and-managing-azure-virtual-machines"></a>Ortak PowerShell komutları için oluşturma ve Azure sanal makineleri yönetme

Bu makale, oluşturma ve Azure aboneliğinizdeki sanal makineleri yönetmek için kullanabileceğiniz Azure PowerShell komutlarının bazı kapsar.  Daha ayrıntılı belirli komut satırı anahtarları ve seçenekleri konusunda yardım için kullanabileceğiniz **Get-Help** *komut*.

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

Birden fazla komutlar bu makaledeki çalışıyorsa bu değişkenler için yararlı olabilir:

- $location - sanal makine konumu. Kullanabileceğiniz [Get-AzLocation](https://docs.microsoft.com/powershell/module/az.resources/get-azlocation) bulmak için bir [coğrafi](https://azure.microsoft.com/regions/) için çalışır.
- $myResourceGroup - sanal makineyi içeren kaynak grubunun adı.
- $myVM - sanal makinenin adı.

## <a name="create-a-vm---simplified"></a>Basitleştirilmiş bir VM - oluşturma

| Görev | Komut |
| ---- | ------- |
| Basit bir VM oluşturma | [Yeni-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) -$myVM adı <BR></BR><BR></BR> Yeni-AzVM sahip bir dizi *Basitleştirilmiş* parametreleri, tüm gerekli olan tek bir ad olduğu. Değer için - ad tüm yeni bir VM oluşturmak için gereken kaynakları adı olarak kullanılacaktır. Daha fazla belirtebilirsiniz, ancak gereken budur.|
| Özel görüntüden VM oluşturma | Yeni-AzVm - ResourceGroupName $myResourceGroup-adı "Myımage" $myVM IMAGENAME-$location konumu  <BR></BR><BR></BR>Kendi oluşturmuş olmanız gerekir [yönetilen bir görüntü](capture-image-resource.md). Birden çok, bir görüntü kullanabilirsiniz birbirinin aynısı olan Vm'leri. |



## <a name="create-a-vm-configuration"></a>Bir VM yapılandırması oluşturun

| Görev | Komut |
| ---- | ------- |
| Bir VM yapılandırması oluşturun |$vm = [yeni AzVMConfig](https://docs.microsoft.com/powershell/module/az.compute/new-azvmconfig) - VMName $myVM - VMSize "Standard_D1_v1"<BR></BR><BR></BR>VM yapılandırması tanımlayın veya VM için ayarları güncelleştirmek için kullanılır. Yapılandırma ve VM adını başlatılır ve kendi [boyutu](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). |
| Yapılandırma ayarları Ekle |$vm = [kümesi AzVMOperatingSystem](https://docs.microsoft.com/powershell/module/az.compute/set-azvmoperatingsystem) - VM $vm-Windows - ComputerName $myVM-$cred - ProvisionVMAgent kimlik bilgisi - EnableAutoUpdate<BR></BR><BR></BR>İşletim sistemi ayarları dahil olmak üzere [kimlik bilgilerini](https://technet.microsoft.com/library/hh849815.aspx) yeni AzVMConfig kullanarak daha önce oluşturduğunuz yapılandırma nesnesine eklenmelidir. |
| Bir ağ arabirimi ekleyin |$vm = [Ekle AzVMNetworkInterface](https://docs.microsoft.com/powershell/module/az.compute/Add-AzVMNetworkInterface) - VM $vm-kimliği $nic Kimliği<BR></BR><BR></BR>Bir VM olmalıdır bir [ağ arabirimi](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) bir sanal ağda iletişim kurabilmek için. Ayrıca [Get-AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.compute/add-azvmnetworkinterface) var olan bir ağ arabirimi nesnesi alınamıyor. |
| Platform görüntüsü belirtin |$vm = [kümesi AzVMSourceImage](https://docs.microsoft.com/powershell/module/az.compute/set-azvmsourceimage) - VM $vm - PublisherName "publisher_name"-"publisher_offer" Teklif - Skus "product_sku"-"son" sürüm<BR></BR><BR></BR>[Görüntü bilgilerini](cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) yeni AzVMConfig kullanarak daha önce oluşturduğunuz yapılandırma nesnesine eklenmelidir. Bu komuttan döndürülen nesne yalnızca işletim sistemi diski, bir platform görüntüsü kullanmasına izin kümesi kullanılır. |
| VM oluşturma |[Yeni-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) - ResourceGroupName $myResourceGroup-konum $location - VM $vm<BR></BR><BR></BR>Tüm kaynaklar oluşturulan bir [kaynak grubu](../../azure-resource-manager/manage-resource-groups-powershell.md). Bu komutu çalıştırmadan önce yeni AzVMConfig, Set-AzVMOperatingSystem, Set-AzVMSourceImage, AzVMNetworkInterface Ekle ve Set-AzVMOSDisk çalıştırın. |
| Bir VM'yi güncelleştirin |[Update-AzVM](https://docs.microsoft.com/powershell/module/az.compute/update-azvm) - ResourceGroupName $myResourceGroup - VM $vm<BR></BR><BR></BR>Get-AzVM kullanarak geçerli VM yapılandırmasını almak, VM nesnesini yapılandırma ayarlarını değiştirin ve ardından bu komutu çalıştırın. |

## <a name="get-information-about-vms"></a>VM'ler hakkında bilgi edinin

| Görev | Komut |
| ---- | ------- |
| Bir abonelikte Vm'leri listeleme |[Get-AzVM](https://docs.microsoft.com/powershell/module/az.compute/get-azvm) |
| Bir kaynak grubundaki Vm'leri listeleme |Get-AzVM - ResourceGroupName $myResourceGroup<BR></BR><BR></BR>Aboneliğinizde kaynak gruplarının bir listesini almak için kullanın [Get-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/get-azresourcegroup). |
| VM hakkında bilgi alma |Get-AzVM - ResourceGroupName $myResourceGroup-$myVM adı |

## <a name="manage-vms"></a>VM’leri yönetme
| Görev | Komut |
| --- | --- |
| VM başlatma |[Start-AzVM](https://docs.microsoft.com/powershell/module/az.compute/start-azvm) - ResourceGroupName $myResourceGroup-$myVM adı |
| VM durdurma |[Stop-AzVM](https://docs.microsoft.com/powershell/module/az.compute/stop-azvm) - ResourceGroupName $myResourceGroup-$myVM adı |
| Çalışan bir sanal Makineyi yeniden başlatın |[Yeniden başlatma-AzVM](https://docs.microsoft.com/powershell/module/az.compute/restart-azvm) - ResourceGroupName $myResourceGroup-$myVM adı |
| VM silme |[Remove-AzVM](https://docs.microsoft.com/powershell/module/az.compute/remove-azvm) - ResourceGroupName $myResourceGroup-$myVM adı |


## <a name="next-steps"></a>Sonraki adımlar
* Bir sanal makine oluşturmak için temel adımları görmek [Resource Manager ve PowerShell kullanarak bir Windows VM oluşturma](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

