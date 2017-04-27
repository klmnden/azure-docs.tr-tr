---
title: "Azure Hızlı Başlangıç - Windows VM Portal oluşturma | Microsoft Docs"
description: "Azure Hızlı Başlangıç - Windows VM Portal oluşturma"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 04/13/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: 8a86cf64dcd65e74285a1073f7494eba0708ddcd
ms.lasthandoff: 04/15/2017

---

# <a name="create-a-windows-virtual-machine-with-the-azure-portal"></a>Azure portal ile Windows sanal makinesi oluşturma

Azure sanal makineleri, Azure portalı üzerinden oluşturulabilir. Bu yöntem, sanal makineleri ve tüm ilgili kaynakları oluşturup yapılandırmaya yönelik tarayıcı tabanlı bir kullanıcı arabirimi sağlar. Azure portalı kullanarak sanal makine oluşturmaya yönelik Hızlı Başlangıç adımları. Dağıtım tamamlandıktan sonra sunucuya bağlanılır ve IIS yüklenir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/en-us/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

http://portal.azure.com sayfasından Azure portalda oturum açın.

## <a name="create-virtual-machine"></a>Sanal makine oluşturma

2. Azure portalının sol üst köşesinde bulunan **Yeni** düğmesine tıklayın.

3. **Yeni** dikey penceresinden **İşlem**’i, **İşlem** dikey penceresinden **Windows Server 2016 Datacenter**’ı seçin ve ardından **Oluştur** düğmesine tıklayın.

4. Sanal makine **Temel Bilgiler** formunu doldurun. Burada girilen kullanıcı adı ve parola, sanal makinede oturum açarken kullanılır. **Kaynak grubu** için yeni bir tane oluşturun. Kaynak grubu, Azure kaynaklarının oluşturulup toplu olarak yönetildiği bir mantıksal kapsayıcıdır. İşlem tamamlandığında **Tamam**’a tıklayın.

    ![Portal dikey penceresinde VM’niz ile ilgili temel bilgileri girin](./media/quick-create-portal/create-windows-vm-portal-basic-blade.png)  

5. VM için bir boyut seçin. Daha fazla boyut görmek için **Tümünü görüntüle**’yi seçin veya **Desteklenen disk türü** filtresini değiştirin. 

    ![VM boyutlarını gösteren ekran görüntüsü](./media/quick-create-portal/create-windows-vm-portal-sizes.png)  

6. Ayarlar dikey penceresinde, **Yönetilen diskleri kullan** altında **Evet**’i seçin, kalan ayarları varsayılan değerlerinde bırakın ve **Tamam**’a tıklayın.

7. Özet sayfasında **Tamam**’a tıklayarak sanal makine dağıtımını başlatın.

8. Dağıtım durumunu izlemek için sanal makineye tıklayın. VM, Azure portal panosunda veya sol menüdeki **Sanal Makineler** seçilerek bulunabilir. VM oluşturulduğunda, **Dağıtılıyor** olan durumu **Çalışıyor** olarak değişir.

## <a name="open-port-80-for-web-traffic"></a>Web trafiği için 80 numaralı bağlantı noktasını açın 

IIS trafiğine izin vermek için 80 numaralı bağlantı noktasını web trafiğine açmanız gerekir. Bu adım 80 numaralı bağlantı noktasında gelen bağlantılara izin vermek üzere bir ağ güvenliği grubu (NSG) kuralı oluşturma işlemini gösterir.

1. Sanal makinenin dikey penceresindeki **Temel Bileşenler** bölümünde **Kaynak grubu** adına tıklayın.
2. Kaynak grubu dikey penceresinde, kaynak listesindeki **Ağ güvenlik grubu**’na tıklayın. NSG adı, sonuna -nsg eklenmiş VM adı olmalıdır.
3. Gelen kural listesini açmak için **Gelen Güvenlik Kuralı**’na tıklayın. Listede RDP için zaten bir kural olduğunu görürsünüz.
4. **+ Ekle**’ye tıklayarak **Gelen güvenlik kuralı ekle** dikey penceresini açın.
5. **Ad** alanına **IIS** yazın. **Bağlantı noktası aralığı** değerinin 80, **Eylem** ayarının **İzin Ver** olarak belirlendiğinden emin olun. **Tamam** düğmesine tıklayın.


## <a name="connect-to-virtual-machine"></a>Sanal makineye bağlanma

Dağıtım tamamlandıktan sonra sanal makine ile bir uzak masaüstü bağlantısı oluşturun.

1. Sanal makine dikey penceresindeki **Bağlan** düğmesine tıklayın. Uzak Masaüstü Protokolü dosyasını (.rdp dosyası) oluşturulup indirilir.

    ![Portal 9](./media/quick-create-portal/quick-create-portal/portal-quick-start-9.png) 

2. VM'nize bağlanmak için indirilen RDP dosyasını açın. İstenirse, **Bağlan**’a tıklayın. Mac bilgisayarlarda, Mac App Store’dan bu [Uzak Masaüstü İstemcisi](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) gibi bir RDP istemcisi indirmeniz gerekir.

3. Sanal makine oluştururken belirttiğiniz kullanıcı adı ile parolayı girin ve **Tamam**’a tıklayın.

4. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bağlantıya devam etmek için **Evet** veya **Devam**’a tıklayın.


## <a name="install-iis-using-powershell"></a>PowerShell kullanarak IIS yükleme

Sanal makinede bir PowerShell istemi açın ve aşağıdaki komutu çalıştırarak IIS yükleyip, web trafiğine izin veren yerel güvenlik kuralı duvarını etkinleştirin:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-the-iis-welcome-page"></a>IIS karşılama sayfasını görüntüleme

Sanal makinenizde İnternet’ten IIS yüklenmiş ve bağlantı noktası 80 açık olduğunda, varsayılan IIS karşılama sayfasını görüntülemek için seçtiğiniz bir web tarayıcısını kullanabilirsiniz. VM dikey penceresinden **Genel IP adresini** alın ve bu adresi kullanarak varsayılan web sayfasını ziyaret edin. 

![Varsayılan IIS sitesi](./media/quick-create-powershell/default-iis-website.png) 

## <a name="delete-virtual-machine"></a>Sanal makineyi silme

Artık gerekli olmadığında kaynak grubunu, sanal makineyi ve tüm ilişkili kaynakları silin. Bunu yapmak için sanal makine dikey penceresinden kaynak grubunu seçip **Sil**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

[Rol yükleme ve güvenlik duvarı yapılandırma öğreticisi](hero-role.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[VM dağıtımı CLI örneklerini keşfedin](cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

