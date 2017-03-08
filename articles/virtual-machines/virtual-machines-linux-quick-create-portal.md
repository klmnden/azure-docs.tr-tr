---
title: "Azure Portal kullanarak bir Linux VM oluşturma | Microsoft Belgeleri"
description: "Azure Portal kullanarak bir Linux VM oluşturma"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: cc5dc395-dc54-4402-8804-2bb15aba8ea2
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: hero-article
ms.date: 1/17/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: beff4fb41ed46b016088734054e7a7897fed1a30
ms.openlocfilehash: 7287b87b1e50e28de06a5363a1f35bd7ac34d51c
ms.lasthandoff: 02/15/2017


---
# <a name="create-a-linux-vm-on-azure-using-the-portal"></a>Portal kullanarak Azure’da bir Linux VM oluşturma.
Bu makalede, Linux Sanal Makinesi oluşturmak için [Azure portal](https://portal.azure.com/)’ı nasıl kullanacağınız gösterilmiştir.

Gereksinimler şunlardır:

* [Bir Azure hesabı](https://azure.microsoft.com/pricing/free-trial/)
* [SSH ortak ve özel anahtar dosyaları](virtual-machines-linux-mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="sign-in"></a>Oturum aç
Azure hesap kimliğinizle Azure portalında oturum açın. Sol üst köşedeki **+ Yeni** öğesine tıklayın:

![Azure kaynağı oluşturma](./media/virtual-machines-linux-quick-create-portal/create_new_resource.png)

## <a name="choose-vm"></a>VM seçme
**Market**’te **İşlem**’e tıklayın, sonra **Öne çıkan Uygulamalar** görüntü listesinde **Ubuntu Server 16.04 LTS**’yi seçin.  Alt kısımda dağıtım modelinin `Resource Manager` olduğunu doğrulayın ve **Oluştur**’a tıklayın.

![Azure Market'ten sanal makine görüntüsü seçme](./media/virtual-machines-linux-quick-create-portal/create_new_vm.png)

## <a name="enter-vm-options"></a>VM seçeneklerini girin
**Temel Ayarlar** sayfasında, şunları girin:

* VM için bir ad
* VM disk türü (varsayılan olarak SSD ya da HDD)
* yönetici kullanıcı için kullanıcı adı
* **Kimlik Doğrulama Türü**’nü **SSH ortak anahtarı** olarak ayarlayın
* dize olarak SSH ortak anahtarınız (`~/.ssh/` dizininizden)
* bir kaynak grubu adı veya var olan bir kaynak grubunu seçin

ve devam etmek için **Tamam**’a tıklayın. Dikey pencere aşağıdaki ekran görüntüsü gibi görünmelidir:

![Temel Azure VM seçeneklerini girin](./media/virtual-machines-linux-quick-create-portal/enter_basic_vm_details.png)

## <a name="choose-vm-size"></a>VM boyutunu seçme
Bir VM boyutu seçin. Aşağıdaki örnekler, Premium SSD üzerine Ubuntu yükleyen **DS1_V2 Standard** seçeneğini kullanır. VM boyutundaki **S** harfi SSD desteğini belirtir. Ayarları yapılandırmak için **Seç**’e tıklayın.

![Azure VM boyutu seçme](./media/virtual-machines-linux-quick-create-portal/select_vm_size.png)

## <a name="storage-and-network"></a>Depolama ve ağ
**Ayarlar** dikey penceresinde, sanal makineniz için Azure Yönetilen Diskleri kullanmayı seçebilirsiniz. Geçerli varsayılan ayar, yönetilmeyen diskleri kullanmaya yöneliktir. Azure Yönetilen Diskler, Azure platformu tarafından işlenir ve depolanması için bir hazırlık veya konum gerekli değildir. Azure Yönetilen Diskler hakkında daha fazla bilgi için bkz. [Azure Yönetilen Disklere genel bakış](../storage/storage-managed-disks-overview.md). Yönetilmeyen diskler için sanal sabit disklerinize ait bir depolama hesabı oluşturmanız ya da seçmeniz gerekir:

![Yönetilmeyen diskler için depolama hesabı seçme](./media/virtual-machines-linux-quick-create-portal/configure_non_managed_disks.png)

Azure Yönetilen Diskleri kullanmayı tercih ederseniz, aşağıdaki örnekte gösterildiği gibi yapılandırılması gereken başka bir seçenek yoktur:

![Portalda Azure Yönetilen Diskler seçeneğini belirleme](./media/virtual-machines-linux-quick-create-portal/select_managed_disks.png)

Diğer ağ ayarlarını varsayılan olarak bırakın.

## <a name="confirm-vm-settings-and-launch"></a>VM ayarlarını onaylama ve başlatma
Yeni Ubuntu VM’nizin ayarlarını onaylayın ve **Tamam**’a tıklayın.

![Azure VM ayarlarını gözden geçirme ve VM oluşturma](./media/virtual-machines-linux-quick-create-portal/review_final_vm_settings.png)

## <a name="select-the-vm-resource"></a>VM kaynağı seçme
Portal giriş sayfasını açın ve sol üst köşedeki menüden **Kaynak grupları**’nı seçin. Gerekirse, menünün üst kısmındaki üç çubuğa tıklayarak listeyi aşağıdaki gibi genişletin:

![Kaynak grupları listesini açma](./media/virtual-machines-linux-quick-create-portal/select_resource_group.png)

Kaynak grubunuzu seçin ve sonra yeni sanal makinenize tıklayın:

![Azure VM NIC ayarlarını bulma](./media/virtual-machines-linux-quick-create-portal/select_vm_resource.png)

## <a name="find-the-public-ip"></a>Genel IP bulma
Sanal makinenize atanmış **Genel IP adresi**’ni görüntüleyin:

![Azure VM ortak IP adresini alma](./media/virtual-machines-linux-quick-create-portal/view_public_ip_address.png)

## <a name="ssh-to-the-vm"></a>Sanal Makineye SSH uygulama
SSH ortak anahtarınızı kullanarak genel IP içine SSH uygulayın.  Bir Mac veya Linux iş istasyonunda Terminalden doğrudan SSH uygulayabilirsiniz. Bir Windows iş istasyonundaysanız Linux’a SSH uygulamak için PuTTY, MobaXTerm veya Cygwin kullanmanız gerekir.  Henüz elinizde yoksa, Windows iş istasyonunuzu Linux’a SSH uygulamaya hazır hale getiren belge aşağıda verilmiştir.

[Azure'da Windows ile SSH anahtarları kullanma](virtual-machines-linux-ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

```
ssh -i ~/.ssh/azure_id_rsa ops@40.112.255.214
```

## <a name="next-steps"></a>Sonraki Adımlar
Şimdi sınama ya da gösterim amaçları için hızlı bir şekilde kullanmak üzere bir Linux VM oluşturdunuz. Altyapınız için özelleştirilmiş bir Linux VM oluşturmak için şu makalelerden herhangi birine göz atabilirsiniz:

* [Şablonlar kullanarak Azure’da bir Linux VM oluşturma](virtual-machines-linux-cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Şablonları kullanarak Azure'da SSH ile Güvenliği Sağlanmış Linux VM'si oluşturma](virtual-machines-linux-create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure CLI kullanarak bir Linux VM’si oluşturma](virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


