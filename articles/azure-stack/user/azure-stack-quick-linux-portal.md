---
title: Azure hızlı başlangıç yığın - VM Portal oluşturma
description: Azure yığın Hızlı Başlat - Portalı'nı kullanarak bir Linux VM oluşturma
services: azure-stack
cloud: azure-stack
author: brenduns
manager: femila
ms.service: azure-stack
ms.topic: quickstart
ms.date: 04/24/2018
ms.author: brenduns
ms.reviewer: ''
ms.custom: mvc
ms.openlocfilehash: 2ea07f04d4c566c0add39d75cad3d3a4ed81c6c8
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32152229"
---
# <a name="quickstart-create-a-linux-server-virtual-machine-with-the-azure-stack-portal"></a>Hızlı Başlangıç: Azure yığın portalıyla Linux sunucusu sanal makine oluşturma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure yığın Portalı'nı kullanarak bir Ubuntu Server 16.04 LTS sanal makine oluşturabilirsiniz. Bir sanal makine oluşturmak için bu makaledeki adımları izleyin. Bu makalede ayrıca adımlarını sağlar:

* Uzak bir istemci ile sanal makineye bağlanabilirsiniz.
* NGINX web sunucusuna yükleyin.
* Kaynakları temizlemek.

## <a name="prerequisites"></a>Önkoşullar

* **Azure yığın Market Linux görüntüde**

   Azure yığın Market Linux görüntü varsayılan olarak içermiyor. Linux sunucusu sanal makine oluşturmadan önce Azure yığın işleci sağladığından emin olun **Ubuntu Server 16.04 LTS** ihtiyacınız görüntü. İşleç açıklanan adımları kullanabilirsiniz [karşıdan Market öğesi Azure'dan Azure yığınına](../azure-stack-download-azure-marketplace-item.md) makalesi.

* **Bir SSH istemci erişimi**

   Azure yığın Geliştirme Seti (ASDK) kullanıyorsanız, bir SSH istemcisi erişimi olmayabilir. Bir istemci gerekiyorsa, bir SSH istemcisi dahil çeşitli paketler vardır. Örneğin, PuTTY SSH istemcisi ve SSH anahtarı Oluşturucu (puttygen.exe) içerir. Kullanılabilir paketler hakkında daha fazla bilgi için aşağıdaki Azure makaleyi okuyun: [Windows Azure üzerinde ile SSH kullanma anahtarları nasıl](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows#windows-packages-and-ssh-clients).

   Bu Hızlı Başlangıç, SSH anahtarları oluşturmak için ve Linux sunucusu sanal makineye bağlanmak için PuTTY kullanır. PuTTY yükleyip için Git [ http://www.putty.org/ ](http://www.putty.org).

## <a name="create-an-ssh-key-pair"></a>SSH anahtar çifti oluşturma

Bu makaledeki tüm adımları tamamlamak için bir SSH anahtar çifti gerekir. Mevcut bir SSH anahtar çiftiniz varsa, bu adımı atlayabilirsiniz.

1. PuTTY yükleme klasörüne gidin (varsayılan konum ```C:\Program Files\PuTTY```) çalıştırıp ```puttygen.exe```.
2. PuTTY anahtar Oluşturucu penceresinde olun **oluşturulacak anahtar türü** ayarlanır **RSA**ve **oluşturulan anahtar bit sayısını** ayarlanır **2048**. Hazır olduğunuzda, tıklatın **Generate**.

   ![PuTTY anahtar Oluşturucu yapılandırması](media/azure-stack-quick-linux-portal/Putty01.PNG)

3. Bir anahtar oluşturmak için fare imlecinizi rastgele PuTTY anahtar Oluşturucu penceresinde taşıyın.
4. Anahtar oluşturma bittiğinde, bilgisayarınızı **ortak anahtarı Kaydet** ve ardından **özel anahtarı Kaydet** anahtarlarınızı dosyalarının kaydedileceği.

   ![PuTTY anahtar Oluşturucu sonuçları](media/azure-stack-quick-linux-portal/Putty02.PNG)

## <a name="sign-in-to-the-azure-stack-portal"></a>Azure yığın portalında oturum açın

Azure yığın portalında oturum açın. Azure yığın portal adresi hangi Azure yığın ürün bağlandığınız bağlıdır:

* Azure yığın Geliştirme Seti (ASDK) gidin: https://portal.local.azurestack.external.
* Bir Azure tümleşik yığını sistemi için Azure yığın operatörünüze sağlanan URL'sine gidin.

## <a name="create-the-virtual-machine"></a>Sanal makineyi oluşturma

1. Tıklatın **kaynak oluşturma** Azure yığın portal sol üst köşesindeki.

2. **İşlem**'i ve ardından **Ubuntu Server 16.04 LTS**'yi seçin.
3. **Oluştur**’a tıklayın.

4. Sanal makine bilgilerini yazın. **Kimlik doğrulama türü** için **SSH ortak anahtarı**’nı seçin. Kaydedilen ve ardından SSH ortak anahtarını Yapıştır **Tamam**.

   >[!NOTE]
 Bunlar anahtarı için tüm başında veya sonunda boşluk kaldırdığınızdan emin olun.

   ![Temel kavramları panel - sanal makine yapılandırma](media/azure-stack-quick-linux-portal/linux-01.PNG)

5. Seçin **D1_V2** sanal makine için.

   ![Panel boyutu - bir sanal makine boyutu seçin](media/azure-stack-quick-linux-portal/linux-02.PNG)

6. Üzerinde **ayarları** sayfasında, Varsayılanları tutun ve tıklayın **Tamam**.

7. Üzerinde **Özet** sayfasında, **Tamam** sanal makine dağıtımı başlatmak için.

## <a name="connect-to-the-virtual-machine"></a>Sanal makineye bağlanma

1. Tıklatın **Bağlan** sanal makine sayfasında. Bu sanal makineye bağlanmak için gereken bir SSH bağlantı dizesi görüntüler.

   ![Sanal makineye bağlanma](media/azure-stack-quick-linux-portal/linux-03.PNG)

2. PuTTY’yi açın.
3. Üzerinde **PuTTY Yapılandırması** kullanacağınız ekran **kategori** penceresi yukarı veya aşağı kaydırın. Ekranı aşağı kaydırarak **SSH**, genişletin **SSH**ve ardından **Auth**. Tıklatın **Gözat** ve kaydettiğiniz özel anahtar dosyası seçin.

   ![PuTTY özel anahtar seçin](media/azure-stack-quick-linux-portal/Putty03.PNG)

4. Yukarı doğru ilerleyin, **kategori** penceresi ve ardından **oturum**.
5. İçinde **ana bilgisayar adı (veya IP adresi)** kutusunda, Azure yığın Portalı'nda gösterilen bağlantı dizesini yapıştırın. Bu örnekte, dizedir ```asadmin@192.168.102.34```.

   ![PuTTY yapılandırması bağlantı dizesi](media/azure-stack-quick-linux-portal/Putty04.PNG)

6. Tıklatın **açmak** sanal makine için bir oturum açın.

   ![Linux oturumu](media/azure-stack-quick-linux-portal/Putty05.PNG)

## <a name="install-the-nginx-web-server"></a>NGINX web sunucusunu yükleme

Paket kaynaklarını güncelleştirin ve sanal makinede en son NGINX paketini yüklemek için aşağıdaki bash komutları kullanın.

```bash
#!/bin/bash

# update package source
sudo apt-get -y update

# install NGINX
sudo apt-get -y install nginx
```

NGINX yükleme bitirdikten sonra SSH oturumu kapatıp açın Azure yığın portalında sanal makineye genel bakış sayfası.

## <a name="open-port-80-for-web-traffic"></a>Web trafiği için 80 numaralı bağlantı noktasını açın

Ağ güvenlik grubu (NSG), gelen ve giden trafiğin güvenliğini sağlar. Azure yığın Portalı'nda bir sanal makine oluşturulduğunda, bir gelen kuralı SSH bağlantıları için bağlantı noktası 22 oluşturulur. Bu sanal makineyi barındıran bir web sunucusu olduğundan, bir NSG kuralı bağlantı noktası 80 üzerinde web trafiğe izin verecek şekilde oluşturulması gerekir.

1. Sanal makinede **genel bakış** sayfasında, adına tıklayın **kaynak grubu**.
2. Seçin **ağ güvenlik grubu** sanal makine için. NSG, **Tür** sütunu kullanılarak tanımlanabilir.
3. Sol taraftaki menüsünde altında **ayarları**, tıklatın **gelen güvenlik kuralları**.
4. **Ekle**'ye tıklayın.
5. **Ad** alanına **http** yazın. **Bağlantı noktası aralığı** değerinin 80, **Eylem** ayarının **İzin Ver** olarak belirlendiğinden emin olun.
6. **Tamam**’a tıklayın.

## <a name="view-the-nginx-welcome-page"></a>NGINX karşılama sayfasını görüntüleme

Yüklü NGINX ve bağlantı noktası 80, sanal makinenizde açık ile sanal makinenin ortak IP adresini kullanarak web sunucusuna erişebilir. (Genel IP adresi sanal makinenin genel bakış sayfasında gösterilir.)

Bir web tarayıcısı açın ve gidin ```http://<public IP address>```.

![NGINX web server Karşılama sayfası](media/azure-stack-quick-linux-portal/linux-04.PNG)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmeyen kaynakları temizlemek. Sanal makine ve kaynaklarını silmek için sanal makine sayfasında kaynak grubunu seçin ve ardından **silmek**.

## <a name="next-steps"></a>Sonraki adımlar

Bu Hızlı Başlangıç, web sunucusu ile temel bir Linux sunucusu sanal makine dağıtılabilir. Azure yığın sanal makineler hakkında daha fazla bilgi için devam [Azure yığınında sanal makineler için ilgili önemli noktalar](azure-stack-vm-considerations.md).
