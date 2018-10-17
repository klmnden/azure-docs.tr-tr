---
title: Azure Data Box Edge ile veri aktarma | Microsoft Docs
description: Data Box Edge cihazında paylaşımları eklemeyi ve bunlara bağlanmayı öğrenin.
services: databox-edge-gateway
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox-edge-gateway
ms.devlang: NA
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/08/2018
ms.author: alkohli
ms.custom: ''
Customer intent: As an IT admin, I need to understand how to add and connect to shares on Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: b5c63a255547bae03acbec771593eac97d474003
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2018
ms.locfileid: "48833099"
---
# <a name="tutorial-transfer-data-with-azure-data-box-edge-preview"></a>Öğretici: Azure Data Box Edge ile veri aktarma (Önizleme)

Bu öğreticide Data Box Edge'de paylaşımları ekleme ve bunlara bağlanma işlemleri açıklanır. Paylaşımlar eklendikten sonra, Data Box Edge cihazı verileri Azure'a aktarabilir.

Bu yordamın tamamlanması 10 dakika kadar sürebilir. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Paylaşım ekleme
> * Paylaşıma bağlanma

> [!IMPORTANT]
> Data Box Edge, önizleme aşamasındadır. Sipariş vermeden ve bu çözümü dağıtmadan önce [Önizleme için Azure hizmet şartlarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) gözden geçirin. 
 
## <a name="prerequisites"></a>Ön koşullar

Data Box Edge cihazınıza paylaşım eklemeden önce aşağıdakilerden emin olun:

* Fiziksel cihazınızı [Data Box Edge kurulumu](data-box-edge-deploy-install.md) sayfasında anlatılan şekilde kurdunuz. 

    Fiziksel cihaz [Azure Data Box Edge cihazınızı bağlama ve etkinleştirme](data-box-edge-deploy-connect-setup-activate.md) sayfasında açıklanan şekilde etkinleştirildi. Cihazınız paylaşım oluşturmak ve veri aktarmak için hazır durumda.


## <a name="add-a-share"></a>Paylaşım ekleme

Paylaşım oluşturmak için [Azure portalda](https://portal.azure.com/) aşağıdaki adımları gerçekleştirin.

1. Azure portalına dönün. **Tüm kaynaklar**'a gidin ve Data Box Edge kaynağınız için arama yapın.
    
2. Filtrelenen kaynak listesinde Data Box Edge kaynağınızı seçin ve **Genel Bakış**'a gidin. Cihazın komut çubuğunda **+ Paylaşım ekle**'ye tıklayın.
   
   ![Paylaşım ekleme](./media/data-box-edge-deploy-add-shares/click-add-share.png)

3. **Paylaşım Ekle**'de, paylaşım ayarlarını belirtin. Paylaşımınız için benzersiz bir ad sağlayın. 

   Paylaşım adları yalnızca rakam, küçük harf ve kısa çizgiler içerebilir. Paylaşım adı 3 ile 63 karakter arası uzunlukta olmalı ve bir harf veya rakamla başlamalıdır. Her kısa çizginin önünde ve arkasında kısa çizgi dışında bir karakter bulunmalıdır.
    
    1. Paylaşım için **Tür** seçin. Tür SMB veya NFS olabilir; varsayılan tür SMB'dir. SMB Windows istemcilerinin standardıdır ve NFS de Linux istemcilerinde kullanılır. 

    2. SMB paylaşımları mı yoksa NFS paylaşımları mı seçtiğinize bağlı olarak, gösterilen seçenekler biraz farklı olur. 

    3. Paylaşımın duracağı depolama hesabını sağlamanız gerekir. Henüz kapsayıcı yoksa, depolama hesabında paylaşım adıyla bir kapsayıcı oluşturulur. Kapsayıcı zaten varsa, bu var olan kapsayıcı kullanılır. 
    
    4. Blok blobundan, sayfa blobundan veya dosyadan **Depolama hizmeti**'ni seçin. Seçilen hizmetin türü, verilerin Azure'da hangi biçimde tutulmasını istediğinize bağlıdır. Örneğin, buradaki örnekte biz verilerin Azure'da blob blokları olarak kalmasını istediğimiz için Blok Blobunu seçtik. Sayfa Blobunu seçerseniz, verilerinizin 512 bayt hizalı olduğundan emin olun. Örneğin VHDX her zaman 512 bayt hizalıdır.
   
    5. Bu adım SMB paylaşımı mı yoksa NFS paylaşımı mı oluşturduğunuza bağlıdır. 
     
        - **SMB paylaşımı oluşturuyorsanız** - Tüm ayrıcalıklara sahip yerel kullanıcı alanında **Yeni oluştur**'u veya **Var olanı kullan**'ı seçin. Yeni bir yerel kullanıcı oluşturuluyorsa, **kullanıcıadı** ve **parola** sağlayın, sonra da **parolayı onaylayın**. Bu, yerel kullanıcıya izinleri atar. Burada izinleri atadıktan sonra, Dosya Gezgini'ni kullanarak bu izinlerde değişiklik yapabilirsiniz.

            Bu paylaşımın verileri için **Yalnızca okuma işlemlerine izin ver**'i işaretlerseniz, salt okuma kullanıcılarını belirtme seçeneği sağlanır.

            ![SMB paylaşımı ekleme](./media/data-box-edge-deploy-add-shares/add-share-smb-1.png)
   
        - **NFS paylaşımı oluşturuluyorsa** - Paylaşıma erişmesine izin verilen istemcilerin IP adreslerini sağlamanız gerekir.

            ![NFS paylaşımı ekleme](./media/data-box-edge-deploy-add-shares/add-share-nfs-1.png)
   
4. Paylaşımı oluşturmak için **Oluştur**'a tıklayın. 
    
    Paylaşım oluşturma işleminin devam ettiği size bildirilir. Paylaşım belirtilen ayarlarla oluşturulduktan sonra, **Paylaşımlar** dikey penceresi yeni paylaşımı yansıtacak şekilde güncelleştirilir. 
    
    ![Güncelleştirilmiş paylaşım listesi](./media/data-box-edge-deploy-add-shares/updated-list-of-shares.png) 

## <a name="connect-to-the-share"></a>Paylaşıma bağlanma

Şimdi önceki adımda oluşturduğunuz bir veya daha fazla paylaşıma bağlanabilirsiniz. Paylaşımın SMB veya NFS olma durumuna göre adımlar değişiklik gösterebilir. 

### <a name="connect-to-an-smb-share"></a>SMB paylaşımına bağlanma

Bu adımları, paylaşımlara bağlanmak üzere Data Box Edge'inize bağlanmış Windows Server istemcinizde gerçekleştirin.


1. Komut penceresi açın. Komut istemine şunları yazın:

    `net use \\<IP address of the device>\<share name>  /u:<user name for the share>`

    İstendiğinde paylaşımın parolasını girin. Burada, bu komutun örnek çıkışı gösterilir.

    ```powershell
    Microsoft Windows [Version 10.0.16299.192) 
    (c) 2017 microsoft Corporation. All rights reserved . 
    
    C: \Users\DataBoxEdgeUser>net use \\10.10.10.60\newtestuser /u:Tota11yNewUser 
    Enter the password for 'TotallyNewUser' to connect to '10.10.10.60': 
    The command completed successfully. 
    
    C: \Users\DataBoxEdgeUser>
    ```   


2. Windows + R tuşlarına basın. **Çalıştır** penceresinde `\\<device IP address>` değerini belirtin. **Tamam** düğmesine tıklayın. Dosya Gezgini açılır. Artık oluşturduğunuz paylaşımları klasörler olarak görebiliyor olmalısınız. İçeriğini görmek için paylaşımı (klasörü) seçin ve çift tıklayın.
 
    ![SMB paylaşımına bağlanma](./media/data-box-edge-deploy-add-shares/connect-to-share2.png)

    Bu paylaşımlara oluşturulduğu anda veriler yazılır ve cihaz verileri buluta gönderir.

### <a name="connect-to-an-nfs-share"></a>NFS paylaşımına bağlanma

Bu adımları, Data Box Edge'inize bağlı Linux istemcinizde gerçekleştirin.

1. İstemcide NFSv4 istemcisinin yüklü olduğundan emin olun. NFS istemcisini yüklemek için aşağıdaki komutu kullanın:

   `sudo apt-get install nfs-common`

    Daha fazla bilgi için [NFSv4 istemcisini yükleme](https://help.ubuntu.com/community/SettingUpNFSHowTo#NFSv4_client) konusuna gidin.

2. NFS istemcisi yüklendikten sonra, Data Box Edge cihazınızda oluşturduğunuz NFS paylaşımını bağlamak için aşağıdaki komutu kullanın:

   `sudo mount <device IP>:/<NFS share on device> /home/username/<Folder on local Linux computer>`

    Paylaşımları bağlamadan önce yerel bilgisayarınızda bağlama noktası olarak kullanılacak dizinlerin önceden oluşturulmuş olduğundan emin olun. Bu dizinlerde dosya veya alt klasör bulunmamalıdır.

    Aşağıdaki örnekte, Data Box Edge cihazındaki bir paylaşıma NFS yoluyla nasıl bağlanılacağı gösterilir. Cihaz IP adresi: `10.10.10.60`. `mylinuxshare2` paylaşımı ubuntuVM öğesine bağlanmış. Paylaşımı bağlama noktası: `/home/databoxubuntuhost/edge`.

    `sudo mount -t nfs 10.10.10.60:/mylinuxshare2 /home/databoxubuntuhost/Edge`

> [!NOTE] 
> Aşağıdaki uyarılar önizleme sürümü için geçerlidir:
> - Paylaşımlarda dosya oluşturulduktan sonra, dosyanın yeniden adlandırılması desteklenmez. 
> - Paylaşımdan dosya silindiğinde, depolama hesabındaki girdi silinmez.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki Data Box Edge konuları hakkında bilgi edindiniz:

> [!div class="checklist"]
> * Paylaşım ekleme
> * Paylaşıma bağlanma


Verilerinizi Data Box Edge ile dönüştürme hakkında bilgi almak için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Data Box Edge ile veri dönüştürme](./data-box-edge-deploy-configure-compute.md)


