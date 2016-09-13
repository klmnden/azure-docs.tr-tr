<properties
    pageTitle="Azure Portal kullanarak bir Linux VM oluşturma | Microsoft Azure"
    description="Azure Portal kullanarak bir Linux VM oluşturma"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"
/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/18/2016"
    ms.author="v-livech"
/>

# Portal kullanarak Azure’da bir Linux VM oluşturma.


Bu makalede hızlı şekilde Linux Sanal Makinesi oluşturmak için [Azure portalını](https://portal.azure.com/) nasıl kullanacağınız gösterilmektedir. [Bir Azure hesabı](https://azure.microsoft.com/pricing/free-trial/) ve [SSH ortak ve özel anahtar dosyaları](virtual-machines-linux-mac-create-ssh-keys.md) tek gerekenlerdir.


1. Azure hesabı kimliğinizle Azure portalında oturum açın, sol üst köşedeki **+ Yeni** seçeneğine tıklayın:

    ![screen1](../media/virtual-machines-linux-quick-create-portal/screen1.png)

2. **Market**’te **Azure Virtual Machines**’e, sonra **Öne çıkan Uygulamalar** görüntü listesinde **Ubuntu Server 14.04 LTS**’ye tıklayın.  Alt kısımda dağıtım modelinin `Resource Manager` olduğunu doğrulayın ve **Oluştur**’a tıklayın.

    ![screen2](../media/virtual-machines-linux-quick-create-portal/screen2.png)

3. **Temel Ayarlar** sayfasında, şunları girin:
    - VM için bir ad
    - Yönetici Kullanıcı için bir kullanıcı adı
    - **SSH ortak anahtarı** olarak ayarlanan Kimlik Doğrulama Türü
    - dize olarak SSH ortak Anahtarınız (`~/.ssh/` dizininizden)
    - bir kaynak grubu adı (veya var olan bir grubu seçin)

    **Tamam**’a tıklayarak devam edin VM boyutunu seçin; aşağıdaki gibi görünmelidir:

    ![screen3](../media/virtual-machines-linux-quick-create-portal/screen3.png)

4. Ubuntu‘yu Premium SSD üzerinde yükleyen **DS1** boyutunu seçin ve ayarları yapılandırmak için **Seç**’e tıklayın.

    ![screen4](../media/virtual-machines-linux-quick-create-portal/screen4.png)

5. **Ayarlar**’da, Depolama ve Ağ değerleri için varsayılan değerleri bırakın ve özeti görüntülemek için **Tamam**’a tıklayın.  DS1 seçerek disk türünün Premium SSD olarak ayarlandığına dikkat edin, **S** SSD’yi gösterir.

    ![screen5](../media/virtual-machines-linux-quick-create-portal/screen5.png)

6. Yeni Ubuntu VM’nizin ayarlarını onaylayın ve **Tamam**’a tıklayın.

    ![screen6](../media/virtual-machines-linux-quick-create-portal/screen6.png)

7. Portal Panosunu açın ve **ağ arabirimleri**nde NIC’inizi seçin

    ![screen7](../media/virtual-machines-linux-quick-create-portal/screen7.png)

8. NIC ayarlarında Genel IP adresleri menüsünü açın

    ![screen8](../media/virtual-machines-linux-quick-create-portal/screen8.png)

9. SSH ortak anahtarınızı kullanarak genel IP içine SSH’leyin

```
ssh -i ~/.ssh/azure_id_rsa ubuntu@13.91.99.206
```

## Sonraki Adımlar

Şimdi sınama ya da gösterim amaçları için hızlı bir şekilde kullanmak üzere bir Linux VM oluşturdunuz. Altyapınız için özelleştirilmiş bir Linux VM oluşturmak için şu makalelerden herhangi birine göz atabilirsiniz:

- [Şablonlar kullanarak Azure’da bir Linux VM oluşturma.](virtual-machines-linux-cli-deploy-templates.md)
- [Şablonları kullanarak Azure'da SSH Korumalı Linux VM oluşturma](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
- [Azure CLI kullanarak bir Linux VM oluşturma](virtual-machines-linux-create-cli-complete.md)



<!--HONumber=sep16_HO1-->


