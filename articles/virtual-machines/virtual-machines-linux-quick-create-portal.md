---
title: "Azure Hızlı Başlangıç - VM Portalı oluşturma | Microsoft Belgeleri"
description: "Azure Hızlı Başlangıç - VM Portalı oluşturma"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/21/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: bcfd830a5e2f39f36460990cae7e84b04d9a5fbb
ms.lasthandoff: 03/22/2017

---

# <a name="create-a-linux-virtual-machine-with-the-azure-portal"></a>Azure portal ile Linux sanal makinesi oluşturma

Azure sanal makineleri, Azure portalı üzerinden oluşturulabilir. Bu yöntem, sanal makineleri ve tüm ilgili kaynakları oluşturup yapılandırmaya yönelik tarayıcı tabanlı bir kullanıcı arabirimi sağlar. Azure portalı kullanarak sanal makine oluşturmaya yönelik Hızlı Başlangıç adımları. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="create-ssh-key-pair"></a>SSH anahtar çifti oluşturma

Bu hızlı başlangıcı tamamlamak için bir SSH anahtar çifti gerekir. Bir SSH anahtar çiftiniz varsa bu adımı atlayabilirsiniz. Windows makinesi kullanıyorsanız, [burada](./virtual-machines-linux-ssh-from-windows.md) bulunan yönergeleri takip edin. 

Bir Bash kabuğundan bu komutu çalıştırın ve ekrandaki yönergeleri izleyin. Komut çıktısı genel anahtar dosyasının dosya adını içerir. Bu dosyanın içeriği, sanal makine oluştururken gereklidir.

```bash
ssh-keygen -t rsa -b 2048
```

## <a name="log-in-to-azure"></a>Azure'da oturum açma 

http://portal.azure.com sayfasından Azure portalda oturum açın.

## <a name="create-virtual-machine"></a>Sanal makine oluşturma

1. Azure portalının sol üst köşesinde bulunan **Yeni** düğmesine tıklayın.

2. **Yeni** dikey penceresinden **İşlem**’i, **İşlem** dikey penceresinden **Ubuntu Server 16.04 LTS**’yi seçin ve ardından **Oluştur** düğmesine tıklayın.

3. Sanal makine **Temel Bilgiler** formunu doldurun. **Kimlik doğrulama türü** için **SSH**’yi seçin. **SSH ortak anahtarınıza** yapıştırırken, önce veya sonra gelen tüm boşlukları kaldırmaya dikkat edin. **Kaynak grubu** için yeni bir tane oluşturun. Kaynak grubu, Azure kaynaklarının oluşturulup toplu olarak yönetildiği bir mantıksal kapsayıcıdır. İşlem tamamlandığında **Tamam**’a tıklayın.

    ![Portal dikey penceresinde VM’niz ile ilgili temel bilgileri girin](./media/virtual-machine-quick-start/create-vm-portal-basic-blade.png)  

4. VM için boyut seçip **Seç** öğesine tıklayın. 

    ![Portal dikey penceresinde VM için bir boyut seçin](./media/virtual-machine-quick-start/create-vm-portal-size-blade.png)

5. Ayarlar dikey penceresinde, **Yönetilen diskleri kullan** altında **Evet**’i seçin, kalan ayarları varsayılan değerlerinde bırakın ve **Tamam**’a tıklayın.

6. Özet sayfasında **Tamam**’a tıklayarak sanal makine dağıtımını başlatın.

7. Dağıtım durumunu izlemek için sanal makineye tıklayın. VM, Azure portal panosunda veya sol menüdeki **Sanal Makineler** seçilerek bulunabilir. VM oluşturulduğunda, **Dağıtılıyor** olan durumu **Çalışıyor** olarak değişir.

## <a name="connect-to-virtual-machine"></a>Sanal makineye bağlanma

Dağıtım tamamlandıktan sonra sanal makine ile bir SSH bağlantısı oluşturun.

1. Sanal makine dikey penceresindeki **Bağlan** düğmesine tıklayın. Bağlan düğmesi, sanal makineye bağlanmak için kullanılabilen bir SSH bağlantı dizesi gösterir.

    ![Portal 9](./media/virtual-machine-quick-start/portal-quick-start-9.png) 

2. Bir SSH oturumu oluşturmak için aşağıdaki komutu çalıştırın. Bağlantı dizesini Azure portaldan kopyaladığınız dizeyle değiştirin.

```bash 
ssh <replace with IP address>
```
## <a name="delete-virtual-machine"></a>Sanal makineyi silme

Artık gerekli olmadığında kaynak grubunu, sanal makineyi ve tüm ilişkili kaynakları silin. Bunu yapmak için sanal makine dikey penceresinden kaynak grubunu seçip **Sil**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

[Yüksek oranda kullanılabilir sanal makine oluşturma öğreticisi](./virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[VM dağıtımı CLI örneklerini keşfedin](./virtual-machines-linux-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

