---
title: Ortak PowerShell komutları için Azure sanal makineleri | Microsoft Docs
description: Oluşturma ve yönetme, Windows Vm'lerini azure'da başlamanıza yardımcı olmak için ortak PowerShell komutları.
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
ms.openlocfilehash: ff861c21250a042191651ab4a4cffbf3928e4f26
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34738573"
---
# <a name="common-powershell-commands-for-creating-and-managing-azure-virtual-machines"></a>Oluşturma ve Azure sanal makineleri yönetmek için ortak PowerShell komutları

Bu makalede oluşturmak ve sanal makineler Azure aboneliğinizde yönetmek için kullanabileceğiniz Azure PowerShell komutlarını bazıları yer almaktadır.  Daha ayrıntılı yardım belirli komut satırı anahtarları ve seçenekleri için kullanabileceğiniz **Get-Help** *komutu*.

Azure PowerShell’in en son sürümünü yükleme, aboneliğinizi seçme ve hesabınızda oturum açma hakkında bilgi almak için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview).

Birden fazla komutların bu makalede çalışıyorsa bu değişkenleri için yararlı olabilir:

- $location - sanal makine konumu. Kullanabileceğiniz [Get-AzureRmLocation](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermlocation) bulmak için bir [coğrafi bölge](https://azure.microsoft.com/regions/) için çalışır.
- $myResourceGroup - sanal makineyi içeren kaynak grubunun adı.
- $myVM - sanal makinenin adı.

## <a name="create-a-vm---simplified"></a>Basitleştirilmiş bir VM - oluşturma

| Görev | Komut |
| ---- | ------- |
| Basit bir VM oluşturma | [Yeni-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) -ad $myVM <BR></BR><BR></BR> Yeni-AzureRMVM sahip bir dizi *Basitleştirilmiş* tek bir ad olduğu gerekli tüm parametreleri. Değer-ad için yeni bir VM oluşturmak için gereken kaynakların tümünü adı olarak kullanılacaktır. Daha fazla belirtebilirsiniz, ancak gerekli olan tek şey budur.|
| Özel görüntüden VM oluşturma | Yeni-AzureRmVm - ResourceGroupName $myResourceGroup-ad $myVM görüntü adı "myImage"-Konum $location  <BR></BR><BR></BR>Kendi oluşturmuş gerek [yönetilen resim](capture-image-resource.md). Birden çok, bir görüntü kullanabilirsiniz aynı VM'ler. |



## <a name="create-a-vm-configuration"></a>Bir VM yapılandırması oluştur

| Görev | Komut |
| ---- | ------- |
| Bir VM yapılandırması oluştur |$vm = [yeni AzureRmVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig) - VMName $myVM - VMSize "Standard_D1_v1"<BR></BR><BR></BR>VM yapılandırması tanımlayın veya VM için ayarları güncelleştirmek için kullanılır. VM adı ile yapılandırma başlatılır ve kendi [boyutu](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). |
| Yapılandırma ayarları ekleme |$vm = [kümesi AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) - VM $vm-Windows - ComputerName $myVM-$cred - ProvisionVMAgent kimlik bilgisi - EnableAutoUpdate<BR></BR><BR></BR>İşletim sistemi ayarları dahil olmak üzere [kimlik bilgileri](https://technet.microsoft.com/library/hh849815.aspx) daha önce oluşturduğunuz yeni AzureRmVMConfig kullanarak yapılandırma nesnesine eklenmelidir. |
| Bir ağ arabirimi ekleyin |$vm = [Ekle AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.5.0/Add-AzureRmVMNetworkInterface) - VM $vm-kimliği $NIC Kimliği<BR></BR><BR></BR>Bir VM'ye sahip olması gereken bir [ağ arabirimi](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) sanal bir ağa iletişim kurmak için. Aynı zamanda [Get-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) var olan bir ağ arabirimi nesnesi alınamadı. |
| Bir platform görüntüsü belirtin |$vm = [kümesi AzureRmVMSourceImage](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmsourceimage) - VM $vm - PublisherName "publisher_name"-"publisher_offer" sunar - SKU "product_sku"-"son" sürüm<BR></BR><BR></BR>[Görüntü bilgileri](cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) daha önce oluşturduğunuz yeni AzureRmVMConfig kullanarak yapılandırma nesnesine eklenmelidir. Bir platform görüntüsü kullanmasına izin işletim sistemi diski ayarladığınızda, bu komuttan döndürülen nesne yalnızca kullanılır. |
| VM oluşturma |[Yeni-AzureRmVM]() - ResourceGroupName $myResourceGroup-konum $location - VM $vm<BR></BR><BR></BR>Tüm kaynaklar oluşturulan bir [kaynak grubu](../../azure-resource-manager/powershell-azure-resource-manager.md). Bu komutu çalıştırmadan önce yeni AzureRmVMConfig, Set-AzureRmVMOperatingSystem, Set-AzureRmVMSourceImage, Add-AzureRmVMNetworkInterface ve Set-AzureRmVMOSDisk çalıştırın. |
| Bir VM güncelleştir |[Update-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvm) - ResourceGroupName $myResourceGroup - VM $vm<BR></BR><BR></BR>Get-AzureRmVM kullanarak geçerli VM yapılandırmasını almak, VM nesnesini yapılandırma ayarlarını değiştirin ve ardından bu komutu çalıştırın. |

## <a name="get-information-about-vms"></a>Sanal makineleri hakkında bilgi edinin

| Görev | Komut |
| ---- | ------- |
| Bir abonelikte listesi VM'ler |[Get-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvm) |
| Bir kaynak grubunda listesi VM'ler |Get-AzureRmVM - ResourceGroupName $myResourceGroup<BR></BR><BR></BR>Aboneliğinizdeki kaynak gruplarının bir listesini almak için [Get-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermresourcegroup). |
| VM hakkında bilgi alma |Get-AzureRmVM - ResourceGroupName $myResourceGroup-ad $myVM |

## <a name="manage-vms"></a>VM’leri yönetme
| Görev | Komut |
| --- | --- |
| VM başlatma |[Start-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/start-azurermvm) - ResourceGroupName $myResourceGroup-ad $myVM |
| VM durdurma |[Stop-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/stop-azurermvm) - ResourceGroupName $myResourceGroup-ad $myVM |
| Çalışan bir VM yeniden başlatma |[Yeniden başlatma-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/restart-azurermvm) - ResourceGroupName $myResourceGroup-ad $myVM |
| VM silme |[Remove-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/remove-azurermvm) - ResourceGroupName $myResourceGroup-ad $myVM |


## <a name="next-steps"></a>Sonraki adımlar
* Bir sanal makine oluşturmak için temel adımlar bkz [Resource Manager ve PowerShell kullanarak bir Windows VM oluşturma](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

