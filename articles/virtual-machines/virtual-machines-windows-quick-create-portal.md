---
title: "Azure Hızlı Başlangıç - Windows VM Portal oluşturma | Microsoft Belgeleri"
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
ms.date: 03/21/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: a13ac5ab425ccbbe53d77cb9f5a8ebf02d009370
ms.lasthandoff: 03/22/2017

---

# <a name="create-a-windows-virtual-machine-with-the-azure-portal"></a>Azure portal ile Windows sanal makinesi oluşturma

Azure sanal makineleri, Azure portalı üzerinden oluşturulabilir. Bu yöntem, sanal makineleri ve tüm ilgili kaynakları oluşturup yapılandırmaya yönelik tarayıcı tabanlı bir kullanıcı arabirimi sağlar. Azure portalı kullanarak sanal makine oluşturmaya yönelik Hızlı Başlangıç adımları. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma 

http://portal.azure.com sayfasından Azure portalda oturum açın.

## <a name="create-virtual-machine"></a>Sanal makine oluşturma

2. Azure portalının sol üst köşesinde bulunan **Yeni** düğmesine tıklayın.

3. **Yeni** dikey penceresinden **İşlem**’i, **İşlem** dikey penceresinden **Windows Server 2016 Datacenter**’ı seçin ve ardından **Oluştur** düğmesine tıklayın.

4. Sanal makine **Temel Bilgiler** formunu doldurun. Burada girilen kullanıcı adı ve parola, sanal makinede oturum açarken kullanılır. **Kaynak grubu** için yeni bir tane oluşturun. Kaynak grubu, Azure kaynaklarının oluşturulup toplu olarak yönetildiği bir mantıksal kapsayıcıdır. İşlem tamamlandığında **Tamam**’a tıklayın.

    ![Portal dikey penceresinde VM’niz ile ilgili temel bilgileri girin](./media/virtual-machine-quick-start/create-windows-vm-portal-basic-blade.png)  

5. VM için boyut seçip **Seç** öğesine tıklayın. 

6. Ayarlar dikey penceresinde, **Yönetilen diskleri kullan** altında **Evet**’i seçin, kalan ayarları varsayılan değerlerinde bırakın ve **Tamam**’a tıklayın.

7. Özet sayfasında **Tamam**’a tıklayarak sanal makine dağıtımını başlatın.

8. Dağıtım durumunu izlemek için sanal makineye tıklayın. VM, Azure portal panosunda veya sol menüdeki **Sanal Makineler** seçilerek bulunabilir. VM oluşturulduğunda, **Dağıtılıyor** olan durumu **Çalışıyor** olarak değişir.

## <a name="connect-to-virtual-machine"></a>Sanal makineye bağlanma

Dağıtım tamamlandıktan sonra sanal makine ile bir uzak masaüstü bağlantısı oluşturun.

1. Sanal makine dikey penceresindeki **Bağlan** düğmesine tıklayın. Uzak Masaüstü Protokolü dosyasını (.rdp dosyası) oluşturulup indirilir.

    ![Portal 9](./media/virtual-machine-quick-start/portal-quick-start-9.png) 

2. VM'nize bağlanmak için indirilen RDP dosyasını açın. İstenirse, **Bağlan**’a tıklayın. Mac bilgisayarlarda, Mac App Store’dan bu [Uzak Masaüstü İstemcisi](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) gibi bir RDP istemcisi indirmeniz gerekir.

3. Sanal makine oluştururken belirttiğiniz kullanıcı adı ile parolayı girin ve **Tamam**’a tıklayın.

4. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bağlantıya devam etmek için **Evet** veya **Devam**’a tıklayın.

## <a name="delete-virtual-machine"></a>Sanal makineyi silme

Artık gerekli olmadığında kaynak grubunu, sanal makineyi ve tüm ilişkili kaynakları silin. Bunu yapmak için sanal makine dikey penceresinden kaynak grubunu seçip **Sil**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

[Rol yükleme ve güvenlik duvarı yapılandırma öğreticisi](./virtual-machines-windows-hero-role.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[VM dağıtımı CLI örneklerini keşfedin](./virtual-machines-windows-cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
