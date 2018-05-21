---
title: Hızlı başlangıç - Azure portalında Linux sanal makinesi oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta Azure portalını kullanarak Linux sanal makinesi oluşturmayı öğrenirsiniz
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/24/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 18ac0291bff2c0fbfffdd5dfa3097f8a6acb561f
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="quickstart-create-a-linux-virtual-machine-in-the-azure-portal"></a>Hızlı başlangıç: Azure portalında Linux sanal makinesi oluşturma

Azure sanal makineleri (VM’ler), Azure portalı üzerinden oluşturulabilir. Bu yöntem, sanal makineleri ve tüm ilgili kaynaklarını oluşturmaya yönelik tarayıcı tabanlı bir kullanıcı arabirimi sağlar. Bu hızlı başlangıçta Azure portalı kullanarak Azure’da Ubuntu çalıştıran bir Linux sanal makinesinin (VM) nasıl dağıtılacağı gösterilir. VM’ye SSH oluşturup NGINX web sunucusunu yükleyerek VM’nizin çalıştığını görebilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-ssh-key-pair"></a>SSH anahtar çifti oluşturma

Bu hızlı başlangıcı tamamlamak için bir SSH anahtar çifti gerekir. Bir SSH anahtar çiftiniz varsa bu adımı atlayabilirsiniz.

Bir SSH anahtar çifti oluşturarak Linux VM’lerinde oturum açmak için bir Bash kabuğundan aşağıdaki komutu çalıştırın ve ekrandaki yönergeleri izleyin. Örneğin, [Azure Cloud Shell](../../cloud-shell/overview.md) veya [Linux için Windows Substem](/windows/wsl/install-win10)’i kullanabilirsiniz. Komut çıktısı genel anahtar dosyasının dosya adını içerir. Ortak anahtar dosyasının içeriğini (`cat ~/.ssh/id_rsa.pub`) panoya kopyalayın:

```bash
ssh-keygen -t rsa -b 2048
```

PuTTy kullanımı dahil olmak üzere SSH anahtar çiftlerinin oluşturulması konusunda daha ayrıntılı bilgi edinmek için bkz. [Windows ile SSH anahtarları kullanma](ssh-from-windows.md).

## <a name="log-in-to-azure"></a>Azure'da oturum açma

http://portal.azure.com adresinden Azure portalında oturum açın

## <a name="create-virtual-machine"></a>Sanal makine oluşturma

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesini seçin.

2. Azure Market kaynaklarının listesi üzerindeki arama kutusunda, Canonical tarafından sağlanan **Ubuntu Server 16.04 LTS** işletim sistemini arayıp seçin ve ardından **Oluştur**’u seçin.

3. *myVM* gibi bir VM adı girin, disk türünü *SSD* olarak bırakın ve *azureuser* gibi bir kullanıcı adı girin.

4. . **Kimlik doğrulama türü** için **SSH genel anahtarı**,’nı seçip genel anahtarınızı metin kutusuna yapıştırın. Genel anahtarınızdan önce veya sonra gelen tüm boşlukları kaldırmaya dikkat edin.

    ![Portal dikey penceresinde VM’niz ile ilgili temel bilgileri girin](./media/quick-create-portal/create-vm-portal-basic-blade.png)

5. **Yeni oluştur**’u seçerek yeni bir kaynak grubu oluşturun ve ardından *myResourceGroup* gibi bir ad girin. İstediğiniz **Konum**’u ve ardından **Tamam**’ı seçin.

4. VM için bir boyut seçin. Örneğin, *İşlem türü* veya *Disk türü*’ne göre filtreleyebilirsiniz. Önerilen VM boyutu: *D2s_v3*.

    ![VM boyutlarını gösteren ekran görüntüsü](./media/quick-create-portal/create-linux-vm-portal-sizes.png)

5. **Ayarlar** altında, varsayılan ayarları olduğu gibi bırakın ve **Tamam**’ı seçin.

6. Özet sayfasında **Oluştur**’u seçerek sanal makine dağıtımını başlatın.

7. VM, Azure portalı panosuna sabitlenir. Dağıtım tamamlandıktan sonra VM özeti otomatik olarak açılır.

## <a name="connect-to-virtual-machine"></a>Sanal makineye bağlanma

VM ile bir SSH bağlantısı oluşturun.

1. VM’nizin genel bakış sayfasından **Bağlan** düğmesini seçin. 

    ![Portal 9](./media/quick-create-portal/portal-quick-start-9.png)

2. **Sanal makineye bağlan** sayfasında, 22 numaralı bağlantı noktası üzerinden DNS adına göre bağlanmak için varsayılan seçenekleri olduğu gibi bırakın. **VM yerel hesabı kullanarak oturum açın** bölümünde bir bağlantı komutu gösterilir. Düğmeye tıklayarak komutu kopyalayın. Aşağıdaki örnekte SSH bağlantı komutunun nasıl göründüğü gösterilmiştir:

    ```bash
    ssh azureuser@myvm-123abc.eastus.cloudapp.azure.com
    ```

3. SSH bağlantı komutunu Windows üzerine Azure Cloud Shell veya Ubuntu üzerinde Bash gibi bir kabuğa yapıştırarak bağlantıyı oluşturun. 

## <a name="install-web-server"></a>Web sunucusunu yükleyin

Sanal makinenizin çalıştığını görmek için NGINX web sunucusunu yükleyin. Paket kaynaklarını güncelleştirmek ve en son NGINX paketini yüklemek için SSH oturumunuzdan aşağıdaki komutları çalıştırın:

```bash
# update packages
sudo apt-get -y update

# install NGINX
sudo apt-get -y install nginx
```

İşiniz bittiğinde, `exit` seçeneği ile SSH oturumundan çıkıp Azure portalında VM özelliklerine geri dönün.

## <a name="open-port-80-for-web-traffic"></a>Web trafiği için 80 numaralı bağlantı noktasını açın

Ağ Güvenlik Grubu (NSG), gelen ve giden trafiğin güvenliğini sağlar. Azure portalından bir VM oluşturulduğunda, SSH bağlantıları için 22 numaralı bağlantı noktasında bir gelen kuralı oluşturulur. Bu VM bir web sunucusunu barındırdığından, 80 numaralı bağlantı noktası için bir NSG kuralının oluşturulması gerekir.

1. VM genel bakış sayfasında **Ağ**’ı seçin.
2. Mevcut gelen ve giden kuralların listesi gösterilir. **Gelen bağlantı noktası kuralı ekle**’yi seçin.
3. Üst kısımda **Temel** seçeneğini ve ardından kullanılabilir hizmetler listesinden *HTTP*’yi belirleyin. Size 80 numaralı bağlantı noktası, bir öncelik ve ad sağlanır.
4. Kuralı oluşturmak için **Ekle**’yi seçin.

## <a name="view-the-web-server-in-action"></a>Web sunucusunu iş başında görün

NGINX yüklüyken ve sanal makinenizde 80 numaralı bağlantı noktası açıkken, web sunucusuna İnternet üzerinden erişilebilir. Bir web tarayıcısı açın ve VM’nin ortak IP adresini girin. Genel IP adresi VM’ye genel bakış sayfasında veya gelen bağlantı noktası kuralını eklediğiniz *Ağ* sayfasının üstünde bulunabilir.

![Varsayılan NGINX sitesi](./media/quick-create-cli/nginx.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, sanal makineyi ve tüm ilişkili kaynakları silebilirsiniz. Bunu yapmak için sanal makinenin kaynak grubunu ve **Sil**’i seçip silinecek kaynak grubunun adını onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta basit bir sanal makine dağıttınız, bir Ağ Güvenlik Grubu ve kuralı oluşturdunuz ve temel bir web sunucusu yüklediniz. Azure sanal makineleri hakkında daha fazla bilgi için Linux VM’lerine yönelik öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Linux sanal makine öğreticileri](./tutorial-manage-vm.md)
