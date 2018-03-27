---
title: Azure Hızlı Başlangıç - VM Portalı oluşturma | Microsoft Docs
description: Azure Hızlı Başlangıç - VM Portalı oluşturma
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: d4bd596257b7430a03ec81a0f378286fda8cbace
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="create-a-linux-virtual-machine-with-the-azure-portal"></a>Azure portal ile Linux sanal makinesi oluşturma

Azure sanal makineleri, Azure portalı üzerinden oluşturulabilir. Bu yöntem, sanal makineleri ve tüm ilgili kaynakları oluşturup yapılandırmaya yönelik tarayıcı tabanlı bir kullanıcı arabirimi sağlar. Bu hızlı başlangıç, sanal makine oluşturma ve VM’ye web sunucusu yüklemeye yönelik adımları gösterir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-ssh-key-pair"></a>SSH anahtar çifti oluşturma

Bu hızlı başlangıcı tamamlamak için bir SSH anahtar çifti gerekir. Bir SSH anahtar çiftiniz varsa bu adımı atlayabilirsiniz.

Bir Bash kabuğundan bu komutu çalıştırın ve ekrandaki yönergeleri izleyin. Komut çıktısı genel anahtar dosyasının dosya adını içerir. Ortak anahtar dosyasının içeriğini (`cat ~/.ssh/id_rsa.pub`) panoya kopyalayın. Linux için Windows Alt Sistemini kullanıyorsanız, çıkıştaki satır sonu karakterlerini kopyalamadığınızdan emin olun. Daha sonra kullanmak üzere özel anahtar dosyasının dosya adını not edin.

```bash
ssh-keygen -t rsa -b 2048
```

Bu işlemle ilgili daha ayrıntılı bilgiyi [burada](https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys) bulabilirsiniz

## <a name="log-in-to-azure"></a>Azure'da oturum açma 

http://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-virtual-machine"></a>Sanal makine oluşturma

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.

2. **İşlem**'i ve ardından **Ubuntu Server 16.04 LTS**'yi seçin. 

3. Sanal makine bilgilerini girin. **Kimlik doğrulama türü** için **SSH ortak anahtarı**’nı seçin. SSH ortak anahtarınıza yapıştırırken, önce veya sonra gelen tüm boşlukları kaldırmaya dikkat edin. İşlem tamamlandığında **Tamam**’a tıklayın.

    ![Portal dikey penceresinde VM’niz ile ilgili temel bilgileri girin](./media/quick-create-portal/create-vm-portal-basic-blade.png)

4. VM için bir boyut seçin. Daha fazla boyut görmek için **Tümünü görüntüle**’yi seçin veya **Desteklenen disk türü** filtresini değiştirin. 

    ![VM boyutlarını gösteren ekran görüntüsü](./media/quick-create-portal/create-linux-vm-portal-sizes.png)  

5. **Ayarlar** altında varsayılan değerleri koruyun ve **Tamam**'a tıklayın.

6. Özet sayfasında **Tamam**’a tıklayarak sanal makine dağıtımını başlatın.

7. VM, Azure portalı panosuna sabitlenir. Dağıtım tamamlandıktan sonra VM özeti otomatik olarak açılır.


## <a name="connect-to-virtual-machine"></a>Sanal makineye bağlanma

Sanal makine ile bir SSH bağlantısı oluşturun.

1. Sanal makine özelliklerinde **Bağlan** düğmesine tıklayın. Bağlan düğmesi, sanal makineye bağlanmak için kullanılabilen bir SSH bağlantı dizesi gösterir.

    ![Portal 9](./media/quick-create-portal/portal-quick-start-9.png) 

2. Bir SSH oturumu oluşturmak için aşağıdaki komutu çalıştırın. Bağlantı dizesini Azure portaldan kopyaladığınız dizeyle değiştirin.

```bash 
ssh azureuser@40.112.21.50
```

## <a name="install-nginx"></a>NGINX yükleme

Paket kaynaklarını güncelleştirmek ve en son NGINX paketini yüklemek için aşağıdaki bash betiğini kullanın. 

```bash 
#!/bin/bash

# update package source
sudo apt-get -y update

# install NGINX
sudo apt-get -y install nginx
```

İşiniz bittiğinde, SSH oturumundan çıkıp Azure portalında VM özelliklerine geri dönün.


## <a name="open-port-80-for-web-traffic"></a>Web trafiği için 80 numaralı bağlantı noktasını açın 

Ağ güvenlik grubu (NSG), gelen ve giden trafiğin güvenliğini sağlar. Azure portalından bir VM oluşturulduğunda, SSH bağlantıları için 22 numaralı bağlantı noktasında bir gelen kuralı oluşturulur. Bu VM bir web sunucusunu barındırdığından, 80 numaralı bağlantı noktası için bir NSG kuralının oluşturulması gerekir.

1. Sanal makinede **Kaynak grubunun** adına tıklayın.
2. **Ağ güvenlik grubu**’nu seçin. NSG, **Tür** sütunu kullanılarak tanımlanabilir. 
3. Sol menüdeki ayarlar altında **Gelen güvenlik kuralları**’na tıklayın.
4. **Ekle**'ye tıklayın.
5. **Ad** alanına **http** yazın. **Kaynak Bağlantı Noktası aralığı**’nın `*`, **Hedef Bağlantı Noktası aralığı**’nın *80* ve **Eylem**’in *İzin Ver* olarak ayarlandığından emin olun. 
6. **Tamam**’a tıklayın.


## <a name="view-the-nginx-welcome-page"></a>NGINX karşılama sayfasını görüntüleme

NGINX yüklüyken ve 80 numaralı bağlantı noktası sanal makineniz için açıkken, web sunucusuna İnternet üzerinden erişilebilir. Bir web tarayıcısı açın ve VM’nin ortak IP adresini girin. Genel IP adresi, Azure portalındaki VM özelliklerinde bulunabilir.

![Varsayılan NGINX sitesi](./media/quick-create-cli/nginx.png) 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, sanal makineyi ve tüm ilişkili kaynakları silin. Bunu yapmak için sanal makine için kaynak grubunu seçip **Sil**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta basit bir sanal makine ve bir ağ güvenlik grubu kuralı dağıtıp, bir web sunucusu yüklediniz. Azure sanal makineleri hakkında daha fazla bilgi için Linux VM’lerine yönelik öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Linux sanal makine öğreticileri](./tutorial-manage-vm.md)
