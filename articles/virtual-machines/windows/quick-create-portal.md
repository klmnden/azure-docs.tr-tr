---
title: Azure Hızlı Başlangıç - Windows VM Portal oluşturma | Microsoft Docs
description: Azure Hızlı Başlangıç - Windows VM Portal oluşturma
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/11/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 42e17e2eb4ef64f8d0e80713041d22e6fe182538
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="create-a-windows-virtual-machine-with-the-azure-portal"></a>Azure portal ile Windows sanal makinesi oluşturma

Azure sanal makineleri, Azure portalı üzerinden oluşturulabilir. Bu yöntem, sanal makineleri ve tüm ilgili kaynakları oluşturup yapılandırmaya yönelik tarayıcı tabanlı bir kullanıcı arabirimi sağlar. Bu hızlı başlangıç, sanal makine oluşturma ve VM’ye web sunucusu yüklemeye yönelik adımları gösterir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-virtual-machine"></a>Sanal makine oluşturma

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.

2. **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin. 

3. Sanal makine bilgilerini girin. Burada girilen kullanıcı adı ve parola, sanal makinede oturum açarken kullanılır. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](faq.md#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır. İşlem tamamlandığında **Tamam**’a tıklayın.

    ![Portal dikey penceresinde VM’niz ile ilgili temel bilgileri girin](./media/quick-create-portal/create-windows-vm-portal-basic-blade.png)  

4. VM için bir boyut seçin. Daha fazla boyut görmek için **Tümünü görüntüle**’yi seçin veya **Desteklenen disk türü** filtresini değiştirin. 

    ![VM boyutlarını gösteren ekran görüntüsü](./media/quick-create-portal/create-windows-vm-portal-sizes.png)  

5. **Ayarlar** altında varsayılan değerleri koruyun ve **Tamam**'a tıklayın. 

6. Özet sayfasında **Tamam**’a tıklayarak sanal makine dağıtımını başlatın.

7. VM, Azure portalı panosuna sabitlenir. Dağıtım tamamlandıktan sonra VM özeti otomatik olarak açılır.


## <a name="connect-to-virtual-machine"></a>Sanal makineye bağlanma

Sanal makine ile bir uzak masaüstü bağlantısı oluşturun.

1. Sanal makine özelliklerinde **Bağlan** düğmesine tıklayın. Uzak Masaüstü Protokolü dosyasını (.rdp dosyası) oluşturulup indirilir.

    ![Portal 9](./media/quick-create-portal/quick-create-portal/portal-quick-start-9.png) 

2. VM'nize bağlanmak için indirilen RDP dosyasını açın. İstenirse, **Bağlan**’a tıklayın. Mac bilgisayarlarda, Mac App Store’dan bu [Uzak Masaüstü İstemcisi](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) gibi bir RDP istemcisi indirmeniz gerekir.

3. Sanal makine oluştururken belirttiğiniz kullanıcı adı ile parolayı girin ve **Tamam**’a tıklayın.

4. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bağlantıya devam etmek için **Evet** veya **Devam**’a tıklayın.


## <a name="install-iis-using-powershell"></a>PowerShell kullanarak IIS yükleme

IIS yüklemek için sanal makinede bir PowerShell oturumu başlatıp aşağıdaki komutu çalıştırın.

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

İşiniz bittiğinde, RDP oturumundan çıkıp Azure portalında VM özelliklerine geri dönün.

## <a name="open-port-80-for-web-traffic"></a>Web trafiği için 80 numaralı bağlantı noktasını açın 

Ağ güvenlik grubu (NSG), gelen ve giden trafiğin güvenliğini sağlar. Azure portalından bir VM oluşturulduğunda, RDP bağlantıları için 3389 numaralı bağlantı noktasında bir gelen kuralı oluşturulur. Bu VM bir web sunucusunu barındırdığından, 80 numaralı bağlantı noktası için bir NSG kuralının oluşturulması gerekir.

1. Sanal makinede **Kaynak grubunun** adına tıklayın.
2. **Ağ güvenlik grubu**’nu seçin. NSG, **Tür** sütunu kullanılarak tanımlanabilir. 
3. Sol menüdeki ayarlar altında **Gelen güvenlik kuralları**’na tıklayın.
4. **Ekle**'ye tıklayın.
5. **Ad** alanına **http** yazın. **Bağlantı noktası aralığı** değerinin 80, **Eylem** ayarının **İzin Ver** olarak belirlendiğinden emin olun. 
6. **Tamam**’a tıklayın.


## <a name="view-the-iis-welcome-page"></a>IIS karşılama sayfasını görüntüleme

IIS yüklüyken ve 80 numaralı bağlantı noktası sanal makineniz için açıkken, web sunucusuna İnternet üzerinden erişilebilir. Bir web tarayıcısı açın ve VM’nin ortak IP adresini girin. Genel IP adresi, Azure portalında *Sanal Makineler* altında bulunur.

![Varsayılan IIS sitesi](./media/quick-create-powershell/default-iis-website.png) 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, sanal makineyi ve tüm ilişkili kaynakları silin. Bunu yapmak için VM’nin kaynak grubunu seçin ve **Sil**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta basit bir sanal makine ve bir ağ güvenlik grubu kuralı dağıtıp, bir web sunucusu yüklediniz. Azure sanal makineleri hakkında daha fazla bilgi için Windows VM’lerine yönelik öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Windows sanal makine öğreticileri](./tutorial-manage-vm.md)
