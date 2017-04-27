---
title: "Azure Hızlı Başlangıç - VM Portalı oluşturma | Microsoft Docs"
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
ms.date: 04/13/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: ab29f01980bc7c3a8f12aaa55ff35baa3bf3f9fb
ms.lasthandoff: 04/15/2017

---

# <a name="create-a-linux-virtual-machine-with-the-azure-portal"></a>Azure portal ile Linux sanal makinesi oluşturma

Azure sanal makineleri, Azure portalı üzerinden oluşturulabilir. Bu yöntem, sanal makineleri ve tüm ilgili kaynakları oluşturup yapılandırmaya yönelik tarayıcı tabanlı bir kullanıcı arabirimi sağlar. Azure portalı kullanarak sanal makine oluşturmaya yönelik Hızlı Başlangıç adımları.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/en-us/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-ssh-key-pair"></a>SSH anahtar çifti oluşturma

Bu hızlı başlangıcı tamamlamak için bir SSH anahtar çifti gerekir. Bir SSH anahtar çiftiniz varsa bu adımı atlayabilirsiniz. Windows makinesi kullanıyorsanız, [burada](ssh-from-windows.md) bulunan yönergeleri takip edin. 

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

    ![Portal dikey penceresinde VM’niz ile ilgili temel bilgileri girin](./media/quick-create-portal/create-vm-portal-basic-blade.png)  

4. VM için bir boyut seçin. Daha fazla boyut görmek için **Tümünü görüntüle**’yi seçin veya **Desteklenen disk türü** filtresini değiştirin. 

    ![VM boyutlarını gösteren ekran görüntüsü](./media/quick-create-portal/create-linux-vm-portal-sizes.png)  

5. Ayarlar dikey penceresinde, **Yönetilen diskleri kullan** altında **Evet**’i seçin, kalan ayarları varsayılan değerlerinde bırakın ve **Tamam**’a tıklayın.

6. Özet sayfasında **Tamam**’a tıklayarak sanal makine dağıtımını başlatın.

7. Dağıtım durumunu izlemek için sanal makineye tıklayın. VM, Azure portal panosunda veya sol menüdeki **Sanal Makineler** seçilerek bulunabilir. VM oluşturulduğunda, **Dağıtılıyor** olan durumu **Çalışıyor** olarak değişir.


## <a name="open-port-80-for-web-traffic"></a>Web trafiği için 80 numaralı bağlantı noktasını açın 

Varsayılan olarak, Azure’a dağıtılmış Linux sanal makinelerinde yalnızca SSH bağlantılarına izin verilir. Bu VM bir web sunucusu olacaksa, 80 numaralı bağlantı noktasını web trafiğine açmanız gerekir. Bu adım 80 numaralı bağlantı noktasında gelen bağlantılara izin vermek üzere bir ağ güvenliği grubu (NSG) kuralı oluşturma işlemini gösterir.

1. Sanal makinenin dikey penceresindeki **Temel Bileşenler** bölümünde **Kaynak grubu** adına tıklayın.
2. Kaynak grubu dikey penceresinde, kaynak listesindeki **Ağ güvenlik grubu**’na tıklayın. NSG adı, sonuna -nsg eklenmiş VM adı olmalıdır.
3. Gelen kural listesini açmak için **Gelen Güvenlik Kuralı**’na tıklayın. Listede RDP için zaten bir kural olduğunu görürsünüz.
4. **+ Ekle**’ye tıklayarak **Gelen güvenlik kuralı ekle** dikey penceresini açın.
5. **Ad** alanına **nginx** yazın. **Bağlantı noktası aralığı** değerinin 80, **Eylem** ayarının **İzin Ver** olarak belirlendiğinden emin olun. **Tamam** düğmesine tıklayın.


## <a name="connect-to-virtual-machine"></a>Sanal makineye bağlanma

Dağıtım tamamlandıktan sonra sanal makine ile bir SSH bağlantısı oluşturun.

1. Sanal makine dikey penceresindeki **Bağlan** düğmesine tıklayın. Bağlan düğmesi, sanal makineye bağlanmak için kullanılabilen bir SSH bağlantı dizesi gösterir.

    ![Portal 9](./media/quick-create-portal/portal-quick-start-9.png) 

2. Bir SSH oturumu oluşturmak için aşağıdaki komutu çalıştırın. Bağlantı dizesini Azure portaldan kopyaladığınız dizeyle değiştirin.

```bash 
ssh <replace with IP address>
```

## <a name="install-nginx"></a>NGINX yükleme

Paket kaynaklarını güncelleştirmek ve en son NGINX paketini yüklemek için aşağıdaki bash betiğini kullanın. 

```bash 
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-the-ngix-welcome-page"></a>NGIX karşılama sayfasını görüntüleme

NGINX yüklendiğine ve VM’nizde İnternet üzerinden 80 numaralı bağlantı noktası açık olduğuna göre, varsayılan NGINX karşılama sayfasını görüntülemek için, seçtiğiniz bir web tarayıcısını kullanabilirsiniz. Varsayılan sayfayı ziyaret etmek için belgelediğiniz `publicIpAddress` seçeneğini kullandığınızdan emin olun. 

![Varsayılan NGINX sitesi](./media/quick-create-cli/nginx.png) 
## <a name="delete-virtual-machine"></a>Sanal makineyi silme

Artık gerekli olmadığında kaynak grubunu, sanal makineyi ve tüm ilişkili kaynakları silin. Bunu yapmak için sanal makine dikey penceresinden kaynak grubunu seçip **Sil**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

[Yüksek oranda kullanılabilir sanal makine oluşturma öğreticisi](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[VM dağıtımı CLI örneklerini keşfedin](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

