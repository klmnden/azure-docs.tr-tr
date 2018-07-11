---
title: Hızlı başlangıç - Azure portalda Windows sanal makinesi oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta Azure portalı kullanarak Windows sanal makinesi oluşturmayı öğrenirsiniz
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/03/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: d5f44c634b953194ad4f112722d82f282d8c8f1a
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37444620"
---
# <a name="quickstart-create-a-windows-virtual-machine-in-the-azure-portal"></a>Hızlı başlangıç: Azure portalda Windows sanal makinesi oluşturma

Azure sanal makineleri (VM’ler), Azure portalı üzerinden oluşturulabilir. Bu yöntem, sanal makineleri ve tüm ilgili kaynaklarını oluşturmaya yönelik tarayıcı tabanlı bir kullanıcı arabirimi sağlar. Bu hızlı başlangıçta Azure portalı kullanarak Azure’da Windows Server 2016 çalıştıran bir sanal makinenin (VM) nasıl dağıtılacağı gösterilir. VM’ye RDP oluşturup IIS web sunucusunu yükleyerek VM’nizin çalıştığını görebilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-virtual-machine"></a>Sanal makine oluşturma

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesini seçin.

2. Azure Market kaynaklarının listesi üzerindeki arama kutusunda, **Windows Server 2016 Datacenter**’ı arayıp seçin ve ardından **Oluştur**’u belirleyin.

3. *myVM* gibi bir VM adı girin, disk türünü *SSD* olarak bırakın ve *azureuser* gibi bir kullanıcı adı girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](faq.md#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.

    ![Portal dikey penceresinde VM’niz ile ilgili temel bilgileri girin](./media/quick-create-portal/create-windows-vm-portal-basic-blade.png)

5. **Yeni oluştur**’u seçerek yeni bir kaynak grubu oluşturun ve ardından *myResourceGroup* gibi bir ad girin. **Konum**’u ve ardından **Tamam**’ı seçin.

4. VM için bir boyut seçin. Örneğin, *İşlem türü* veya *Disk türü*’ne göre filtreleyebilirsiniz. Önerilen VM boyutu: *D2s_v3*. Boyut belirledikten sonra **Seç**'e tıklayın.

    ![VM boyutlarını gösteren ekran görüntüsü](./media/quick-create-portal/create-windows-vm-portal-sizes.png)

5. **Ayarlar** sayfasının **Ağ** > **Ağ Güvenlik Grubu** > **Ortak gelen bağlantı noktası seçin** bölümündeki açılan menüden **HTTP** ve **RDP (3389)** seçimlerini yapın. Diğer varsayılan ayarları olduğu gibi bırakın ve **Tamam**’ı seçin.

6. Özet sayfasında **Oluştur**’u seçerek sanal makine dağıtımını başlatın.

7. VM, Azure portalı panosuna sabitlenir. Dağıtım tamamlandıktan sonra VM özeti otomatik olarak açılır.

## <a name="connect-to-virtual-machine"></a>Sanal makineye bağlanma

Sanal makine ile bir uzak masaüstü bağlantısı oluşturun. Bu yönergeler VM’nize bir Windows bilgisayarından nasıl bağlanacağınızı gösterir. Mac bilgisayarlarda, Mac App Store’dan bu [Uzak Masaüstü İstemcisi](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) gibi bir RDP istemcisi indirmeniz gerekir.

1. Sanal makine özellikleri sayfasında **Bağlan** düğmesine tıklayın. 

    ![Portaldan bir Azure sanal makinesine bağlanma](./media/quick-create-portal/quick-create-portal/portal-quick-start-9.png)
    
2. **Sanal makineye bağlan** sayfasında, 3389 numaralı bağlantı noktası üzerinden DNS adına göre bağlanmak için varsayılan seçenekleri olduğu gibi bırakın ve **RDP dosyasını indir**’e tıklayın.

2. İndirilen RDP dosyasını açın ve istendiğinde **Bağlan**’a tıklayın. 

3. **Windows Güvenliği** penceresinde **Diğer seçenekler**'i ve ardından **Başka bir hesap kullanın**'ı seçin. Kullanıcı adını *vmname*\*username* olarak yazın, sanal makine için oluşturduğunuz parolayı girin ve ardından **Tamam**’a tıklayın.

4. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bağlantıyı oluşturmak için **Evet** veya **Devam**’a tıklayın.

## <a name="install-web-server"></a>Web sunucusunu yükleyin

Sanal makinenizin çalıştığını görmek için IIS web sunucusunu yükleyin. VM’de bir PowerShell istemi açın ve şu komutu çalıştırın:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

İşiniz bittiğinde, RDP’nin sanal makine bağlantısını kapatın.


## <a name="view-the-iis-welcome-page"></a>IIS karşılama sayfasını görüntüleme

Portalda VM'yi seçin ve VM özetinde IP adresinin sağ tarafındaki **Kopyalamak için tıklayın** düğmesini kullanarak adresi kopyalayın ve bir tarayıcı sekmesine yapıştırın. Varsayılan IIS karşılama sayfası açılır ve şu şekilde görünmelidir:

![Varsayılan IIS sitesi](./media/quick-create-powershell/default-iis-website.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, sanal makineyi ve tüm ilişkili kaynakları silebilirsiniz. Bunu yapmak için sanal makinenin kaynak grubunu ve **Sil**’i seçip silinecek kaynak grubunun adını onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, basit bir sanal makine dağıttınız, web trafiği için bir ağ bağlantı noktası açtınız ve temel bir web sunucusu yüklediniz. Azure sanal makineleri hakkında daha fazla bilgi için Windows VM’lerine yönelik öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Windows sanal makine öğreticileri](./tutorial-manage-vm.md)
