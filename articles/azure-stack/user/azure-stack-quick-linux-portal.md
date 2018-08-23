---
title: Azure Stack hızlı başlangıç - VM portalı oluşturma
description: Azure Stack hızlı başlangıç - portal kullanarak bir Linux VM oluşturma
services: azure-stack
cloud: azure-stack
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.topic: quickstart
ms.date: 08/15/2018
ms.author: mabrigg
ms.reviewer: ''
ms.custom: mvc
ms.openlocfilehash: c692bc461c116b4c0497c2378ae4e21e1b841c8f
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "42139585"
---
# <a name="quickstart-create-a-linux-server-virtual-machine-with-the-azure-stack-portal"></a>Hızlı Başlangıç: Azure Stack portal ile Linux server sanal makinesi oluşturma

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack portalını kullanarak bir Ubuntu Server 16.04 LTS sanal makine oluşturabilirsiniz. Bir sanal makine oluşturup, bu makaledeki adımları izleyin. Bu makalede ayrıca adımları sunar:

* Sanal Makine Uzak istemcisi ile bağlanın.
* NGINX web sunucusunu yükleyin.
* Kaynaklarınızı temizleme.

## <a name="prerequisites"></a>Önkoşullar

* **Azure Stack marketini içindeki bir Linux görüntüsü**

   Azure Stack marketini varsayılan olarak bir Linux görüntüsü içermiyor. Bir Linux server sanal makinesi oluşturmadan önce Azure Stack operatörü sağladığından emin olmak **Ubuntu Server 16.04 LTS** ihtiyacınız görüntü. İşleç açıklanan bu adımları kullanabilirsiniz [indirme Market öğesi Azure'dan Azure Stack'e](../azure-stack-download-azure-marketplace-item.md) makalesi.

* **Bir SSH istemcisi erişim**

   Azure Stack geliştirme Seti'ni (ASDK) kullanıyorsanız, SSH istemcisi erişimi olmayabilir. Bir istemci gerekiyorsa, bir SSH istemcisi dahil çeşitli paketler vardır. Örneğin, bir SSH istemcisi ve SSH anahtarı Oluşturucu (puttygen.exe) PuTTY içerir. Kullanılabilir paketler hakkında daha fazla bilgi için aşağıdaki Azure makalesini okuyun: [azure'da Windows ile SSH anahtarlarını kullanma nasıl](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows#windows-packages-and-ssh-clients).

   Bu hızlı başlangıçta, PuTTY SSH anahtarları oluşturun ve Linux sunucusu sanal makinesine bağlanmak için kullanılır. Putty'yi indirin ve yükleyin gidin [ http://www.putty.org/ ](http://www.putty.org).

## <a name="create-an-ssh-key-pair"></a>SSH anahtar çifti oluşturma

Bu makaledeki tüm adımları tamamlamak için bir SSH anahtar çifti gerekir. Mevcut bir SSH anahtar çiftiniz varsa, bu adımı atlayabilirsiniz.

1. PuTTY yükleme klasörüne gidin (varsayılan konum ```C:\Program Files\PuTTY```) çalıştırıp ```puttygen.exe```.
2. PuTTY anahtar Oluşturucu penceresinde olun **oluşturulacak anahtar türü** ayarlanır **RSA**ve **oluşturulan anahtarı bit sayısı** ayarlanır **2048**. Hazır olduğunuzda, tıklayın **Oluştur**.

   ![PuTTY anahtar Oluşturucu yapılandırması](media/azure-stack-quick-linux-portal/Putty01.PNG)

3. Bir anahtar oluşturmak için fare imlecinizi rastgele PuTTY anahtar Oluşturucu pencerede hareket ettirin.
4. Anahtar oluşturma tamamlandığında, **ortak anahtarı Kaydet** ve ardından **özel anahtarı Kaydet** anahtarlarınızı dosyaları kaydetmek için.

   ![PuTTY anahtar Oluşturucu sonuçları](media/azure-stack-quick-linux-portal/Putty02.PNG)

## <a name="sign-in-to-the-azure-stack-portal"></a>Azure Stack portalında oturum açın

Azure Stack portalında oturum açın. Azure Stack portal'ın adresi, Azure Stack ürünlere ilişkin bağlanmakta olduğunuz bağlıdır:

* Azure Stack geliştirme Seti'ni (ASDK) gidin: https://portal.local.azurestack.external.
* Bir Azure Stack tümleşik sistemi için Azure Stack operatörü sağlanan URL'sine gidin.

## <a name="create-the-virtual-machine"></a>Sanal makineyi oluşturma

1. Tıklayın **kaynak Oluştur** Azure Stack portal'ın sol üst köşedeki.

2. **İşlem**'i ve ardından **Ubuntu Server 16.04 LTS**'yi seçin.
3. **Oluştur**’a tıklayın.

4. Sanal makine bilgilerini yazın. **Kimlik doğrulama türü** için **SSH ortak anahtarı**’nı seçin. Yapıştırma seçeneğiyle, kaydedilen ve ardından SSH ortak anahtarını **Tamam**.

   >[!NOTE]
 Bunlar anahtarında herhangi başında veya sonunda boşluk kaldırdığınızdan emin olun.

   ![Temel panelinde - sanal makine yapılandırma](media/azure-stack-quick-linux-portal/linux-01.PNG)

5. Seçin **D1_V2** sanal makine için.

   ![Panel boyutunu - bir sanal makine boyutu seçin](media/azure-stack-quick-linux-portal/linux-02.PNG)

6. Üzerinde **ayarları** sayfasında varsayılan değerleri koruyun ve tıklayın **Tamam**.

7. Üzerinde **özeti** sayfasında **Tamam** sanal makine dağıtımını başlatın.

## <a name="connect-to-the-virtual-machine"></a>Sanal makineye bağlanma

1. Tıklayın **Connect** sanal makine sayfasında. Bu sanal makineye bağlanmak için bir SSH bağlantı dizesi görüntüler.

   ![Sanal makineye bağlanma](media/azure-stack-quick-linux-portal/linux-03.PNG)

2. PuTTY’yi açın.
3. Üzerinde **PuTTY Yapılandırması** kullanacağınız ekran **kategori** penceresinde yukarı veya aşağı kaydırın. Ekranı aşağı kaydırarak **SSH**, genişletme **SSH**ve ardından **Auth**. Tıklayın **Gözat** ve kaydedilen özel anahtar dosyası seçin.

   ![PuTTY özel anahtar seçin](media/azure-stack-quick-linux-portal/Putty03.PNG)

4. Yukarı kaydırın **kategori** penceresi ve ardından **oturumu**.
5. İçinde **ana bilgisayar adı (veya IP adresi)** kutusuna, Azure Stack portalında gösterilen bağlantı dizesini yapıştırın. Bu örnekte, dizedir ```asadmin@192.168.102.34```.

   ![PuTTY yapılandırması bağlantı dizesi](media/azure-stack-quick-linux-portal/Putty04.PNG)

6. Tıklayın **açın** sanal makine için oturum açın.

   ![Linux oturumu](media/azure-stack-quick-linux-portal/Putty05.PNG)

## <a name="install-the-nginx-web-server"></a>NGINX web sunucusunu yükleme

Paket kaynaklarını güncelleştirmek ve sanal makinede en son NGINX paketini yüklemek için aşağıdaki bash komutları kullanın.

```bash
#!/bin/bash

# update package source
sudo apt-get -y update

# install NGINX
sudo apt-get -y install nginx
```

NGINX yükleme işiniz bittiğinde, SSH oturumunu kapatın ve Azure Stack portalında sanal makineye genel bakış sayfasını açın.

## <a name="open-port-80-for-web-traffic"></a>Web trafiği için 80 numaralı bağlantı noktasını açın

Ağ güvenlik grubu (NSG), gelen ve giden trafiğin güvenliğini sağlar. Azure Stack portalında sanal makine oluşturulduğunda, SSH bağlantıları için 22 numaralı bağlantı noktasında bir gelen kuralı oluşturulur. Bu sanal makineyi barındıran bir web sunucusu olduğundan, bir NSG kuralı 80 numaralı bağlantı noktasını web trafiğine izin verecek şekilde oluşturulması gerekir.

1. Sanal makinede **genel bakış** sayfasında, adına **kaynak grubu**.
2. Seçin **ağ güvenlik grubu** sanal makine için. NSG, **Tür** sütunu kullanılarak tanımlanabilir.
3. Sol taraftaki menüsünden altında **ayarları**, tıklayın **gelen güvenlik kuralları**.
4. **Ekle**'ye tıklayın.
5. **Ad** alanına **http** yazın. **Bağlantı noktası aralığı** değerinin 80, **Eylem** ayarının **İzin Ver** olarak belirlendiğinden emin olun.
6. **Tamam**’a tıklayın.

## <a name="view-the-nginx-welcome-page"></a>NGINX karşılama sayfasını görüntüleme

NGINX ve 80 numaralı bağlantı noktası, sanal makinede, sanal makinenin genel IP adresini kullanarak web sunucusuna erişebilirsiniz. (Genel IP adresini sanal makinenin genel bakış sayfasında gösterilen.)

Bir web tarayıcısı açın ve gidin ```http://<public IP address>```.

![NGINX web-server-Hoş Geldiniz sayfası](media/azure-stack-quick-linux-portal/linux-04.PNG)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyacınız olmayan kaynakları temizleyin. Sanal makineyi ve kaynaklarını silmek için sanal makine sayfasında kaynak grubunu seçin ve ardından **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir web sunucusu ile temel bir Linux sunucusu sanal makine dağıtıldı. Azure Stack sanal makineleri hakkında daha fazla bilgi için devam [sanal Makineler'de Azure Stack için Değerlendirmeler](azure-stack-vm-considerations.md).
