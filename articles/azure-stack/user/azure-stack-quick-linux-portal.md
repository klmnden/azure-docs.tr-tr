---
title: "Azure hızlı başlangıç yığın - VM Portal oluşturma"
description: "Azure yığın Hızlı Başlat - Portalı'nı kullanarak bir Linux VM oluşturma"
services: azure-stack
cloud: azure-stack
author: vhorne
manager: byronr
ms.service: azure-stack
ms.topic: quickstart
ms.date: 12/11/2017
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 1e1732f48de9f95e669d0282d120e48b5fe5f0ef
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="create-a-linux-virtual-machine-with-the-azure-stack-portal"></a>Azure yığın portalıyla Linux sanal makine oluşturma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure yığın Portalı aracılığıyla Azure yığın sanal makineleri oluşturulabilir. Bu yöntem oluşturmak ve bir sanal makine yapılandırmak için bir tarayıcı tabanlı kullanıcı arabirimi sağlar ve ilişkili tüm kaynakları. Bu hızlı başlangıç hızlı bir şekilde Linux sanal makine oluşturun ve bir web sunucusu üzerinde yükleme gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

* **Azure yığın Market Linux görüntüde**

   Azure yığın Market Linux görüntü varsayılan olarak içermiyor. Linux sanal makine oluşturmadan önce bu nedenle, Azure yığın işleci indirdiği olun **Ubuntu Server 16.04 LTS** açıklanan adımları kullanarak görüntü [Azure Azure Market öğesi indirin Yığın](../azure-stack-download-azure-marketplace-item.md) konu.

* **Bir SSH istemci erişimi**

   Azure yığın Geliştirme Seti (ASDK) kullanıyorsanız, ortamınızda bir SSH istemcisi erişimi olmayabilir. Bu durumda, bir SSH istemcisi dahil çeşitli paketler arasında seçim yapabilirsiniz. Örneğin, bir SSH istemcisi ve SSH anahtarı Oluşturucu (puttygen.exe) içeren PuTTY yükleyebilirsiniz. Aşağıdaki Azure makale ilgili olası seçenekleri hakkında daha fazla bilgi için bkz: [Windows Azure üzerinde ile SSH kullanma anahtarları nasıl](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows#windows-packages-and-ssh-clients).

   Bu Hızlı Başlangıç, SSH anahtarları oluşturmak için ve Linux sanal makineye bağlanmak için PuTTY kullanır. PuTTY yükleyip için Git [http://www.putty.org/](http://www.putty.org).

## <a name="create-an-ssh-key-pair"></a>SSH anahtar çifti oluşturma

Bu hızlı başlangıç tamamlamak için bir SSH anahtar çifti gerekir. Bir SSH anahtar çiftiniz varsa bu adımı atlayabilirsiniz.

1. PuTTY yükleme klasörüne gidin (varsayılan konum ```C:\Program Files\PuTTY```) çalıştırıp ```puttygen.exe```.
2. PuTTY anahtar Oluşturucu penceresinde olun **oluşturulacak anahtar türü** ayarlanır **RSA**ve **oluşturulan anahtar bit sayısını** ayarlanır **2048**. Hazır olduğunuzda tıklatın **Generate**.

   ![puttygen.exe](media/azure-stack-quick-linux-portal/Putty01.PNG)

3. Anahtar oluşturma işlemini tamamlamak için fare imlecinizi PuTTY anahtar Oluşturucu pencereye taşıyın.
4. Anahtar oluşturma tamamlandığında tıklayın **ortak anahtarı Kaydet** ve **özel anahtarı Kaydet** ortak ve özel anahtarları dosyasına kaydetmek için.

   ![PuTTY anahtarları](media/azure-stack-quick-linux-portal/Putty02.PNG)



## <a name="sign-in-to-the-azure-stack-portal"></a>Azure yığın portalında oturum açın

Azure yığın portalında oturum açın. Azure yığın portal adresi hangi Azure yığın ürün bağlandığınız bağlıdır:

* Azure yığın Geliştirme Seti (ASDK) gidin: https://portal.local.azurestack.external.
* Bir Azure tümleşik yığını sistemi için Azure yığın operatörünüze sağlanan URL'sine gidin.

## <a name="create-the-virtual-machine"></a>Sanal makineyi oluşturma

1. Tıklatın **kaynak oluşturma** Azure yığın portal sol üst köşesindeki.

2. **İşlem**'i ve ardından **Ubuntu Server 16.04 LTS**'yi seçin.
3. **Oluştur**’a tıklayın.

4. Sanal makine bilgilerini yazın. **Kimlik doğrulama türü** için **SSH ortak anahtarı**’nı seçin. (Bu, bir dosya için daha önce kaydedilmiş) SSH ortak anahtarınızı yapıştırın, tüm başında veya sonunda boşluk kaldırmak için dikkatli olun. İşlem tamamlandığında **Tamam**’a tıklayın.

   ![Sanal makine temelleri](media/azure-stack-quick-linux-portal/linux-01.PNG)

5. Seçin **D1_V2** sanal makine için.

   ![Makine boyutu](media/azure-stack-quick-linux-portal/linux-02.PNG)

6. Üzerinde **ayarları** sayfasında, Varsayılanları tutun ve tıklayın **Tamam**.

7. Üzerinde **Özet** sayfasında, **Tamam** sanal makine dağıtımı başlatmak için.


## <a name="connect-to-the-virtual-machine"></a>Sanal makineye bağlanma

1. Tıklatın **Bağlan** sanal makine sayfasında. Bu sanal makineye bağlanmak için kullanılan bir SSH bağlantı dizesi görüntüler.

   ![Sanal makineye bağlanma](media/azure-stack-quick-linux-portal/linux-03.PNG)

2. PuTTY’yi açın.
3. Üzerinde **PuTTY Yapılandırması** altında ekran **kategori**, genişletin **SSH** ve ardından **Auth**. Tıklatın **Gözat** ve daha önce kaydettiğiniz özel anahtar dosyası seçin.

   ![PuTTY özel anahtar](media/azure-stack-quick-linux-portal/Putty03.PNG)
4. Altında **kategori**, yukarı doğru ilerleyin ve tıklatın **oturum**.
5. İçinde **ana bilgisayar adı (veya IP adresi)** kutusunda, daha önce gördüğünüz Azure yığın portalından bağlantı dizesini yapıştırın. Bu örnekte, dizedir ```asadmin@192.168.102.34```.
 
   ![PuTTY oturumu](media/azure-stack-quick-linux-portal/Putty04.PNG)
6. Tıklatın **açmak** sanal makine için bir oturum açın.

   ![Linus oturumu](media/azure-stack-quick-linux-portal/Putty05.PNG)

## <a name="install-nginx"></a>NGINX yükleme

Sanal makinede en son NGINX paketini yükleyin ve paket kaynaklarını güncelleştirmek için aşağıdaki bash komut dosyası kullanın. 

```bash 
#!/bin/bash

# update package source
sudo apt-get -y update

# install NGINX
sudo apt-get -y install nginx
```

İşiniz bittiğinde, SSH oturumu çıkın ve Azure yığın Portalı'nda sanal makineye genel bakış sayfasına dönün.


## <a name="open-port-80-for-web-traffic"></a>Web trafiği için 80 numaralı bağlantı noktasını açın 

Ağ güvenlik grubu (NSG), gelen ve giden trafiğin güvenliğini sağlar. Azure yığın Portalı'ndan bir sanal makine oluşturulduğunda, bir gelen kuralı SSH bağlantıları için bağlantı noktası 22 oluşturulur. Bu sanal makineyi barındıran bir web sunucusu olduğundan, bir NSG kuralı için bağlantı noktası 80 oluşturulması gerekir.

1. Sanal makinede **genel bakış** sayfasında, adına tıklayın **kaynak grubu**.
2. Seçin **ağ güvenlik grubu** sanal makine için. NSG, **Tür** sütunu kullanılarak tanımlanabilir. 
3. Sol taraftaki menüsünde altında **ayarları**, tıklatın **gelen güvenlik kuralları**.
4. **Ekle**'ye tıklayın.
5. **Ad** alanına **http** yazın. **Bağlantı noktası aralığı** değerinin 80, **Eylem** ayarının **İzin Ver** olarak belirlendiğinden emin olun. 
6. **Tamam**’a tıklayın.


## <a name="view-the-nginx-welcome-page"></a>NGINX karşılama sayfasını görüntüleme

Yüklü NGINX ve bağlantı noktası 80, sanal makinenizde açık ile web sunucusu artık sanal makinenin ortak IP adresinde erişilebilir. Genel IP adresi, sanal makinenin genel bakış sayfasında Azure yığın Portalı'nda bulunabilir.

Bir web tarayıcısı açın ve gidin ```http://<public IP address>```.

![Varsayılan NGINX sitesi](media/azure-stack-quick-linux-portal/linux-04.PNG)


## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, sanal makineyi ve tüm ilişkili kaynakları silin. Bunu yapmak için sanal makine sayfasından kaynak grubunu seçin ve **silmek**.

## <a name="next-steps"></a>Sonraki adımlar

Bu Hızlı Başlangıç, basit Linux sanal makine, bir ağ güvenlik grubu kural dağıtılan artık ve bir web sunucusu yüklü. Azure yığın sanal makineler hakkında daha fazla bilgi için devam [Azure yığınında sanal makineler için ilgili önemli noktalar](azure-stack-vm-considerations.md).

