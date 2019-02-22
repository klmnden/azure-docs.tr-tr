---
title: Azure Data Box Gateway ile verileri aktarma | Microsoft Docs
description: Data Box Gateway cihazında paylaşımları eklemeyi ve bunlara bağlanmayı öğrenin.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: tutorial
ms.date: 02/21/2019
ms.author: alkohli
ms.openlocfilehash: f36e13ccf91c983c54897dcff7e1c02689fb055c
ms.sourcegitcommit: a8948ddcbaaa22bccbb6f187b20720eba7a17edc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2019
ms.locfileid: "56592664"
---
# <a name="tutorial-transfer-data-with-azure-data-box-gateway-preview"></a>Öğretici: Azure veri kutusu ağ geçidi (Önizleme) ile veri aktarma


## <a name="introduction"></a>Giriş

Bu makalede Data Box Gateway'de paylaşımları ekleme ve bunlara bağlanma işlemleri açıklanır. Paylaşımlar eklendikten sonra, Data Box Gateway cihazı verileri Azure'a aktarabilir.

Bu yordamın tamamlanması 10 dakika kadar sürebilir. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Paylaşım ekleme
> * Paylaşıma bağlanma

> [!IMPORTANT]
> - Data Box Gateway önizleme aşamasındadır. Sipariş vermeden ve bu çözümü dağıtmadan önce [Önizleme için Azure hizmet şartlarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) gözden geçirin. 
 
## <a name="prerequisites"></a>Önkoşullar

Data Box Gateway cihazınıza paylaşım eklemeden önce aşağıdakilerden emin olun:

* [Data Box Gateway'i Hyper-V'de sağlama](data-box-gateway-deploy-provision-hyperv.md) veya [Data Box Gateway'i VMware'de sağlama](data-box-gateway-deploy-provision-vmware.md) konusunda ayrıntılarıyla açıklandığı gibi bir sanal cihaz sağladınız ve bu cihaza bağlandınız. 

    [Azure Data Box Gateway'inizi bağlama ve etkinleştirme](data-box-gateway-deploy-connect-setup-activate.md) konusunda ayrıntılarıyla açıklandığı gibi sanal cihaz etkinleştirildi ve paylaşımları oluşturup verileri aktarmaya hazırsınız.


## <a name="add-a-share"></a>Paylaşım ekleme

Paylaşım oluşturmak için [Azure portalında](https://portal.azure.com/) aşağıdaki adımları gerçekleştirin.

1. Azure portalına dönün. **Tüm kaynaklar**'a gidin ve Data Box Gateway kaynağınız için arama yapın.
    
2. Filtrelenen kaynak listesinde Data Box Ağ Geçidi kaynağınızı seçin ve **Genel Bakış**'a gidin. Cihazın komut çubuğunda **+ Paylaşım ekle**'ye tıklayın.
   
   ![Paylaşım ekleme](./media/data-box-gateway-deploy-add-shares/click-add-share.png)

4. **Paylaşım Ekle**'de, paylaşım ayarlarını belirtin. Paylaşımınız için benzersiz bir ad sağlayın. 

   Paylaşım adları yalnızca rakam, küçük harf ve kısa çizgiler içerebilir. Paylaşım adı 3 ile 63 karakter arası uzunlukta olmalı ve bir harf veya rakamla başlamalıdır. Her kısa çizginin önünde ve arkasında kısa çizgi dışında bir karakter bulunmalıdır.
    
5. Paylaşım için **Tür** seçin. Tür SMB veya NFS olabilir; varsayılan tür SMB'dir. SMB Windows istemcilerinin standardıdır ve NFS de Linux istemcilerinde kullanılır. SMB paylaşımları mı yoksa NFS paylaşımları mı seçtiğinize bağlı olarak, gösterilen seçenekler biraz farklı olur. 

6. Paylaşımın duracağı depolama hesabını sağlamanız gerekir. Henüz kapsayıcı yoksa, depolama hesabında paylaşım adıyla bir kapsayıcı oluşturulur. Kapsayıcı zaten varsa, bu var olan kapsayıcı kullanılır. 
    
7. Blok blobundan, sayfa blobundan veya dosyadan **Depolama hizmeti**'ni seçin. Seçilen hizmetin türü, verilerin Azure'da hangi biçimde tutulmasını istediğinize bağlıdır. Örneğin, buradaki örnekte biz verilerin Azure'da blob blokları olarak tutulmasını istediğimiz için Blok Blobunu seçtik. Sayfa Blobunu seçerseniz, verilerinizi 512 bayt hizalı olduğundan emin olmalısınız. VHDX'in her zaman 512 bayt hizalı olduğunu unutmayın.
   
8. Bu adım SMB paylaşımı mı yoksa NFS paylaşımı mı oluşturduğunuza bağlıdır. 
     
    - **SMB paylaşımı oluşturuyorsanız** - Tüm ayrıcalıklara sahip yerel kullanıcı alanında **Yeni oluştur**'u veya **Var olanı kullan**'ı seçin. Yeni bir yerel kullanıcı oluşturuluyorsa, **kullanıcıadı** ve **parola** sağlayın, sonra da **parolayı onaylayın**. Bu, yerel kullanıcıya izinleri atar. Burada izinleri atadıktan sonra, Dosya Gezgini'ni kullanarak bu izinlerde değişiklik yapabilirsiniz.
    
        ![SMB paylaşımı ekleme](./media/data-box-gateway-deploy-add-shares/add-share-smb-1.png)
        
        Bu paylaşımın verileri için **Yalnızca okuma işlemlerine izin ver**'i işaretlerseniz, salt okuma kullanıcılarını belirtme seçeneği sağlanır.
        
    - **NFS paylaşımı oluşturuluyorsa** - Paylaşıma erişmesine izin verilen istemcilerin IP adreslerini sağlamanız gerekir.

        ![NFS paylaşımı ekleme](./media/data-box-gateway-deploy-add-shares/add-share-nfs-1.png)
   
9. Paylaşımı oluşturmak için **Oluştur**'a tıklayın. 
    
    Paylaşım oluşturma işleminin devam ettiği size bildirilir. Paylaşım belirtilen ayarlarla oluşturulduktan sonra, **Paylaşımlar** dikey penceresi yeni paylaşımı yansıtacak şekilde güncelleştirilir. 
    
    ![Güncelleştirilmiş paylaşım listesi](./media/data-box-gateway-deploy-add-shares/updated-list-of-shares.png) 

## <a name="connect-to-the-share"></a>Paylaşıma bağlanma

Bu adımları, paylaşımlara bağlanmak üzere Data Box Gateway'inize bağlanmış Windows Server istemcinizde gerçekleştirin.


1. Komut penceresi açın. Komut istemine şunları yazın:

    `net use \\<IP address of the device>\<share name>  /u:<user name for the share>`

    İstendiğinde paylaşımın parolasını girin. Burada, bu komutun örnek çıkışı gösterilir.

    ```powershell
    Microsoft Windows [Version 18.8.16299.192) 
    (c) 2817 microsoft Corporation. All rights reserved . 
    
    C: \Users\GatewayUser>net use \\10.10.10.60\newtestuser /u:Tota11yNewUser 
    Enter the password for 'TotallyNewUser' to connect to '10.10.10.60' • 
    The command completed successfully. 
    
    C: \Users\GatewayUser>
    ```   


2. Windows + R tuşlarına basın. **Çalıştır** penceresinde `\\<device IP address>` değerini belirtin. **Tamam** düğmesine tıklayın. Dosya Gezgini açılır. Artık oluşturduğunuz paylaşımları klasörler olarak görebiliyor olmalısınız. İçeriğini görmek için paylaşımı (klasörü) seçin ve çift tıklayın.
 
    ![SMB paylaşımına bağlanma](./media/data-box-gateway-deploy-add-shares/connect-to-share2.png)-->

    Bu paylaşımlara oluşturulduğu anda veriler yazılır ve cihaz verileri buluta gönderir.

### <a name="connect-to-an-nfs-share"></a>NFS paylaşımına bağlanma

Bu adımları, Data Box Edge'inize bağlı Linux istemcinizde gerçekleştirin.

1. İstemcide NFSv4 istemcisinin yüklü olduğundan emin olun. NFS istemcisini yüklemek için aşağıdaki komutu kullanın:

   `sudo apt-get install nfs-common`

    Daha fazla bilgi için [NFSv4 istemcisini yükleme](https://help.ubuntu.com/community/SettingUpNFSHowTo#NFSv4_client) konusuna gidin.

2. NFS istemcisi yüklendikten sonra, Data Box Gateway cihazınızda oluşturduğunuz NFS paylaşımını bağlamak için aşağıdaki komutu kullanın:

   `sudo mount -t nfs -o sec=sys,resvport <device IP>:/<NFS shares on device> /home/username/<Folder on local Linux computer>`

    Bağlamaları ayarlamadan önce, yerel bilgisayarınızda bağlama noktası işlevi görecek dizinlerin zaten oluşturulmuş olduğundan ve hiçbir dosya veya alt klasör içermediğinden emin olun.

    Aşağıdaki örnekte, Gateway cihazındaki bir paylaşıma NFS yoluyla nasıl bağlanılacağı gösterilir. Sanal cihaz IP'si `10.10.10.60`dır, `mylinuxshare2` ubuntuVM'ye bağlanmıştır ve bağlama noktası `/home/databoxubuntuhost/gateway`dir.

    `sudo mount -t nfs -o sec=sys,resvport 10.10.10.60:/mylinuxshare2 /home/databoxubuntuhost/gateway`

> [!NOTE] 
> Aşağıdaki uyarılar önizleme sürümü için geçerlidir:
> - Paylaşımlarda dosya oluşturulduktan sonra, dosyanın yeniden adlandırılması desteklenmez. 
> - Paylaşımdan dosya silindiğinde, depolama hesabındaki girdi silinmez.
> - Kullanıyorsanız `rsync` sonra veri kopyalamak için `rsync -a` seçeneği desteklenmemektedir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki Data Box Gateway konularını öğrendiniz:

> [!div class="checklist"]
> * Paylaşım ekleme
> * Paylaşıma bağlanma


Data Box Gateway'inizi yönetmeyi öğrenmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Data Box Gateway'i yönetmek için yerel web kullanıcı arabirimini kullanma](https://aka.ms/dbg-docs)


