---
title: "Azure Portal kullanarak bir Linux VM oluşturma | Microsoft Belgeleri"
description: "Azure Portal kullanarak bir Linux VM oluşturma"
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: cc5dc395-dc54-4402-8804-2bb15aba8ea2
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: hero-article
ms.date: 10/28/2016
ms.author: v-livech
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: b1446cd8892e14988ff428eaa03233f8e9aefb8a


---
# <a name="create-a-linux-vm-on-azure-using-the-portal"></a>Portal kullanarak Azure’da bir Linux VM oluşturma.
Bu makalede, Linux Sanal Makinesi oluşturmak için [Azure portal](https://portal.azure.com/)’ı nasıl kullanacağınız gösterilmiştir.

Gereksinimler şunlardır:

* [Bir Azure hesabı](https://azure.microsoft.com/pricing/free-trial/)
* [SSH ortak ve özel anahtar dosyaları](virtual-machines-linux-mac-create-ssh-keys.md)

## <a name="sign-in"></a>Oturum aç
Azure hesabı kimliğinizle Azure portalında oturum açın, sol üst köşedeki **+ Yeni** seçeneğine tıklayın:

![screen1](../media/virtual-machines-linux-quick-create-portal/screen1.png)

## <a name="choose-vm"></a>VM seçme
**Market**’te **Azure Virtual Machines**’e, sonra **Öne çıkan Uygulamalar** görüntü listesinde **Ubuntu Server 14.04 LTS**’ye tıklayın.  Alt kısımda dağıtım modelinin `Resource Manager` olduğunu doğrulayın ve **Oluştur**’a tıklayın.

![screen2](../media/virtual-machines-linux-quick-create-portal/screen2.png)

## <a name="enter-vm-options"></a>VM seçeneklerini girin
**Temel Ayarlar** sayfasında, şunları girin:

* VM için bir ad
* Yönetici Kullanıcı için bir kullanıcı adı
* **SSH ortak anahtarı** olarak ayarlanan Kimlik Doğrulama Türü
* dize olarak SSH ortak Anahtarınız (`~/.ssh/` dizininizden)
* bir kaynak grubu adı (veya var olan bir grubu seçin)

**Tamam**’a tıklayarak devam edin ve VM boyutunu seçin; aşağıdaki ekran görüntüsü gibi görünmelidir:

![screen3](../media/virtual-machines-linux-quick-create-portal/screen3.png)

## <a name="choose-vm-size"></a>VM boyutunu seçme
Ubuntu‘yu Premium SSD üzerinde yükleyen **DS1** boyutunu seçin ve ayarları yapılandırmak için **Seç**’e tıklayın.

![screen4](../media/virtual-machines-linux-quick-create-portal/screen4.png)

## <a name="storage-and-network"></a>Depolama ve ağ
**Ayarlar**’da, Depolama ve Ağ değerleri için varsayılan değerleri bırakın ve özeti görüntülemek için **Tamam**’a tıklayın.  DS1 seçerek disk türünün Premium SSD olarak ayarlandığına dikkat edin, **S** SSD’yi gösterir.

![screen5](../media/virtual-machines-linux-quick-create-portal/screen5.png)

## <a name="confirm-vm-settings-and-launch"></a>VM ayarlarını onaylama ve başlatma
Yeni Ubuntu VM’nizin ayarlarını onaylayın ve **Tamam**’a tıklayın.

![screen6](../media/virtual-machines-linux-quick-create-portal/screen6.png)

## <a name="find-the-vm-nic"></a>VM NIC bulma
Portal Panosunu açın ve **ağ arabirimleri**nde NIC’inizi seçin

![screen7](../media/virtual-machines-linux-quick-create-portal/screen7.png)

## <a name="find-the-public-ip"></a>Genel IP bulma
NIC ayarlarında Genel IP adresleri menüsünü açın

![screen8](../media/virtual-machines-linux-quick-create-portal/screen8.png)

## <a name="ssh-to-the-vm"></a>Sanal Makineye SSH uygulama
SSH ortak anahtarınızı kullanarak genel IP içine SSH uygulayın.  Bir Mac veya Linux iş istasyonunda Terminalden doğrudan SSH uygulayabilirsiniz. Bir Windows iş istasyonundaysanız Linux’a SSH uygulamak için PuTTY, MobaXTerm veya Cygwin kullanmanız gerekir.  Henüz elinizde yoksa, Windows iş istasyonunuzu Linux’a SSH uygulamaya hazır hale getiren belge aşağıda verilmiştir.

[Azure'da Windows ile SSH anahtarları kullanma](virtual-machines-linux-ssh-from-windows.md)

```
ssh -i ~/.ssh/azure_id_rsa ubuntu@13.91.99.206
```

## <a name="next-steps"></a>Sonraki Adımlar
Şimdi sınama ya da gösterim amaçları için hızlı bir şekilde kullanmak üzere bir Linux VM oluşturdunuz. Altyapınız için özelleştirilmiş bir Linux VM oluşturmak için şu makalelerden herhangi birine göz atabilirsiniz:

* [Şablonlar kullanarak Azure’da bir Linux VM oluşturma](virtual-machines-linux-cli-deploy-templates.md)
* [Şablonları kullanarak Azure'da SSH ile Güvenliği Sağlanmış Linux VM'si oluşturma](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
* [Azure CLI kullanarak bir Linux VM’si oluşturma](virtual-machines-linux-create-cli-complete.md)




<!--HONumber=Nov16_HO2-->


