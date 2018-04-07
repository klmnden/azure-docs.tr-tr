---
title: Ortak PowerShell komutları için Azure sanal makineleri | Microsoft Docs
description: Oluşturma ve yönetme, Windows Vm'lerini azure'da başlamanıza yardımcı olmak için ortak PowerShell komutları.
services: virtual-machines-windows
documentationcenter: ''
author: davidmu1
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ba3839a2-f3d5-4e19-a5de-95bfb1c0e61e
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/17/2017
ms.author: davidmu
ms.openlocfilehash: f84473e73a32da43cc6cc80b21deb49ab4f3ceb9
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="common-powershell-commands-for-creating-and-managing-azure-virtual-machines"></a>Oluşturma ve Azure sanal makineleri yönetmek için ortak PowerShell komutları

Bu makalede oluşturmak ve sanal makineler Azure aboneliğinizde yönetmek için kullanabileceğiniz Azure PowerShell komutlarını bazıları yer almaktadır.  Daha ayrıntılı yardım belirli komut satırı anahtarları ve seçenekleri için kullanabileceğiniz **Get-Help** *komutu*.

Azure PowerShell’in en son sürümünü yükleme, aboneliğinizi seçme ve hesabınızda oturum açma hakkında bilgi almak için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview).

Birden fazla komutların bu makalede çalışıyorsa bu değişkenleri için yararlı olabilir:

- $location - sanal makine konumu. Kullanabileceğiniz [Get-AzureRmLocation](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermlocation) bulmak için bir [coğrafi bölge](https://azure.microsoft.com/regions/) için çalışır.
- $myResourceGroup - sanal makineyi içeren kaynak grubunun adı.
- $myVM - sanal makinenin adı.

## <a name="create-a-vm"></a>VM oluşturma

| Görev | Komut |
| ---- | ------- |
| Bir VM yapılandırması oluştur |$vm = [New-AzureRmVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig) -VMName $myVM -VMSize "Standard_D1_v1"<BR></BR><BR></BR>VM yapılandırması tanımlayın veya VM için ayarları güncelleştirmek için kullanılır. VM adı ile yapılandırma başlatılır ve kendi [boyutu](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). |
| Yapılandırma ayarları ekleme |$vm = [Set-AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) -VM $vm -Windows -ComputerName $myVM -Credential $cred -ProvisionVMAgent -EnableAutoUpdate<BR></BR><BR></BR>İşletim sistemi ayarları dahil olmak üzere [kimlik bilgileri](https://technet.microsoft.com/library/hh849815.aspx) daha önce oluşturduğunuz yeni AzureRmVMConfig kullanarak yapılandırma nesnesine eklenmelidir. |
| Bir ağ arabirimi ekleyin |$vm = [Add-AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.5.0/Add-AzureRmVMNetworkInterface) -VM $vm -Id $nic.Id<BR></BR><BR></BR>Bir VM'ye sahip olması gereken bir [ağ arabirimi](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) sanal bir ağa iletişim kurmak için. Aynı zamanda [Get-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) var olan bir ağ arabirimi nesnesi alınamadı. |
| Bir platform görüntüsü belirtin |$vm = [Set-AzureRmVMSourceImage](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmsourceimage) -VM $vm -PublisherName "publisher_name" -Offer "publisher_offer" -Skus "product_sku" -Version "latest"<BR></BR><BR></BR>[Görüntü bilgileri](cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) daha önce oluşturduğunuz yeni AzureRmVMConfig kullanarak yapılandırma nesnesine eklenmelidir. Bir platform görüntüsü kullanmasına izin işletim sistemi diski ayarladığınızda, bu komuttan döndürülen nesne yalnızca kullanılır. |
| İşletim sistemi diski bir platform görüntüsü kullanacak şekilde ayarlama |$vm = [kümesi AzureRmVMOSDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmosdisk) - VM $vm-adı "myOSDisk" - VhdUri "http://mystore1.blob.core.windows.net/vhds/myOSDisk.vhd" - CreateOption FromImage<BR></BR><BR></BR>İşletim sistemi diski ve konumunda adını [depolama](../../storage/common/storage-powershell-guide-full.md) daha önce oluşturduğunuz yapılandırma nesnesine eklendi. |
| İşletim sistemi diski genelleştirilmiş bir yansıma kullanacak şekilde ayarlama |$vm = kümesi AzureRmVMOSDisk - VM $vm-adı "myOSDisk" - SourceImageUri "https://mystore1.blob.core.windows.net/system/Microsoft.Compute/Images/myimages/myprefix-osDisk.{guid}.vhd"- VhdUri"https://mystore1.blob.core.windows.net/vhds/disk_name.vhd" - CreateOption FromImage-Windows<BR></BR><BR></BR>İşletim sistemi diski, kaynak görüntüsünün konumu ve disk konumda adını [depolama](../../storage/common/storage-powershell-guide-full.md) yapılandırma nesnesine eklenmelidir. |
| İşletim sistemi diski özel bir yansıma kullanacak şekilde ayarlama |$vm = kümesi AzureRmVMOSDisk - VM $vm-adı "myOSDisk" - VhdUri "http://mystore1.blob.core.windows.net/vhds/" - CreateOption Attach - Windows |
| VM oluşturma |[New-AzureRmVM]() -ResourceGroupName $myResourceGroup -Location $location -VM $vm<BR></BR><BR></BR>Tüm kaynaklar oluşturulan bir [kaynak grubu](../../azure-resource-manager/powershell-azure-resource-manager.md). Bu komutu çalıştırmadan önce yeni AzureRmVMConfig, Set-AzureRmVMOperatingSystem, Set-AzureRmVMSourceImage, Add-AzureRmVMNetworkInterface ve Set-AzureRmVMOSDisk çalıştırın. |

## <a name="get-information-about-vms"></a>Sanal makineleri hakkında bilgi edinin

| Görev | Komut |
| ---- | ------- |
| Bir abonelikte listesi VM'ler |[Get-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvm) |
| Bir kaynak grubunda listesi VM'ler |Get-AzureRmVM -ResourceGroupName $myResourceGroup<BR></BR><BR></BR>Aboneliğinizdeki kaynak gruplarının bir listesini almak için [Get-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermresourcegroup). |
| VM hakkında bilgi alma |Get-AzureRmVM -ResourceGroupName $myResourceGroup -Name $myVM |

## <a name="manage-vms"></a>VM’leri yönetme
| Görev | Komut |
| --- | --- |
| VM başlatma |[Start-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/start-azurermvm) - ResourceGroupName $myResourceGroup-ad $myVM |
| VM durdurma |[Stop-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/stop-azurermvm) -ResourceGroupName $myResourceGroup -Name $myVM |
| Çalışan bir VM yeniden başlatma |[Yeniden başlatma-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/restart-azurermvm) - ResourceGroupName $myResourceGroup-ad $myVM |
| VM silme |[Remove-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/remove-azurermvm) -ResourceGroupName $myResourceGroup -Name $myVM |
| Bir VM generalize |[Set-AzureRmVm](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvm) -ResourceGroupName $myResourceGroup -Name $myVM -Generalized<BR></BR><BR></BR>Kaydet-AzureRmVMImage çalıştırmadan önce bu komutu çalıştırın. |
| VM yakalama |[Kaydet-AzureRmVMImage](https://docs.microsoft.com/powershell/module/azurerm.compute/save-azurermvmimage) - ResourceGroupName $myResourceGroup - VMName $myVM - DestinationContainerName "myImageContainer" - VHDNamePrefix "myImagePrefix"-"C:\filepath\filename.json" yolu<BR></BR><BR></BR>Bir sanal makine olmalıdır [hazır, kapatmak ve genelleştirilmiş](prepare-for-upload-vhd-image.md) bir görüntü oluşturmak için kullanılacak. Bu komutu çalıştırmadan önce Set-AzureRmVm çalıştırın. |
| Bir VM güncelleştir |[Update-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvm) -ResourceGroupName $myResourceGroup -VM $vm<BR></BR><BR></BR>Get-AzureRmVM kullanarak geçerli VM yapılandırmasını almak, VM nesnesini yapılandırma ayarlarını değiştirin ve ardından bu komutu çalıştırın. |
| Bir VM’ye veri diski ekleme |[Add-AzureRmVMDataDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmdatadisk) -VM $vm -Name "myDataDisk" -VhdUri "https://mystore1.blob.core.windows.net/vhds/myDataDisk.vhd" -LUN # -Caching ReadWrite -DiskSizeinGB # -CreateOption Empty<BR></BR><BR></BR>VM nesnesini almak için get-AzureRmVM kullanın. LUN numarası ve diskin boyutunu belirtin. VM yapılandırma değişiklikleri uygulamak için Update-AzureRmVM çalıştırın. Eklediğiniz disk başlatılmadı. |
| Bir VM’den veri diski kaldırma |[Remove-AzureRmVMDataDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/remove-azurermvmdatadisk) -VM $vm -Name "myDataDisk"<BR></BR><BR></BR>VM nesnesini almak için get-AzureRmVM kullanın. VM yapılandırma değişiklikleri uygulamak için Update-AzureRmVM çalıştırın. |
| Bir VM uzantısı Ekle |[Set-AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmextension) -ResourceGroupName $myResourceGroup -Location $location -VMName $myVM -Name "extensionName" -Publisher "publisherName" -Type "extensionType" -TypeHandlerVersion "#.#" -Settings $Settings -ProtectedSettings $ProtectedSettings<BR></BR><BR></BR>Bu komut uygun çalıştırmak [yapılandırma bilgilerini](template-description.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#extensions) yüklemek istediğiniz uzantısı. |
| VM uzantısı kaldırma |[Remove-AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/remove-azurermvmextension) -ResourceGroupName $myResourceGroup -Name "extensionName" -VMName $myVM |

## <a name="next-steps"></a>Sonraki adımlar
* Bir sanal makine oluşturmak için temel adımlar bkz [Resource Manager ve PowerShell kullanarak bir Windows VM oluşturma](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

