---
title: Azure Data Box Gateway ile verileri aktarma | Microsoft Docs
description: Data Box Gateway cihazında paylaşımları eklemeyi ve bunlara bağlanmayı öğrenin.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: tutorial
ms.date: 03/08/2019
ms.author: alkohli
ms.openlocfilehash: d930b1db48e3a5c4bda96f0b7d80a9c9f24d53d9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60755652"
---
# <a name="tutorial-transfer-data-with-azure-data-box-gateway"></a>Öğretici: Azure veri kutusu ağ geçidi ile veri aktarma


## <a name="introduction"></a>Giriş

Bu makalede, eklemek ve veri kutusu ağ geçidinizi paylaşımlarına bağlanmak açıklar. Paylaşımları ekledikten sonra veri kutusu ağ geçidi cihazı Azure'a veri aktarabilir.

Bu yordamın tamamlanması 10 dakika kadar sürebilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Paylaşım ekleme
> * Paylaşıma bağlanma


## <a name="prerequisites"></a>Önkoşullar

Data Box Gateway cihazınıza paylaşım eklemeden önce aşağıdakilerden emin olun:

- Sağlanan sanal cihazı ve içinde ayrıntılı olarak bağlı [veri kutusu ağ geçidi Hyper-V'de sağlama](data-box-gateway-deploy-provision-hyperv.md) veya [veri kutusu ağ geçidi vmware'de sağlama](data-box-gateway-deploy-provision-vmware.md).

- Sanal cihaz açıklanan etkinleştirildikten sonra [Connect ve Azure veri kutusu Gateway'i etkinleştirme](data-box-gateway-deploy-connect-setup-activate.md).

- Cihazınız paylaşım oluşturmak ve veri aktarmak için hazır durumda.

## <a name="add-a-share"></a>Paylaşım ekleme

Aşağıdaki yordam bir paylaşımını yapın oluşturmak için:

1. İçinde [Azure portalında](https://portal.azure.com/), veri kutusu ağ geçidi kaynağınızı seçin ve ardından Git **genel bakış**. Cihazınız çevrimiçi olması. Seçin **+ Ekle paylaşımı** cihaz komut çubuğunda.
   
   ![Paylaşım ekleme](./media/data-box-gateway-deploy-add-shares/click-add-share.png)

4. İçinde **paylaşımı Ekle**, aşağıdaki yordamı yapın:

    1. Paylaşımınız için benzersiz bir ad sağlayın. Paylaşım adları yalnızca küçük harf, sayı ve kısa çizgi olabilir. Paylaşım adı, 3 ila 63 karakter uzunluğunda olması ve bir harf veya rakam ile başlar. Her kısa çizginin önünde ve arkasında kısa çizgi dışında bir karakter bulunmalıdır.
    
    2. Paylaşım için **Tür** seçin. Tür SMB veya NFS olabilir; varsayılan tür SMB'dir. SMB Windows istemcilerinin standardıdır ve NFS de Linux istemcilerinde kullanılır. SMB paylaşımları mı yoksa NFS paylaşımları mı seçtiğinize bağlı olarak, gösterilen seçenekler biraz farklı olur.

    3. Paylaşım bulunacağı bir depolama hesabı sağlayın. Bir kapsayıcı zaten yoksa, yeni oluşturulan bir paylaşım adı ile depolama hesabı oluşturulur. Kapsayıcı zaten mevcutsa, bu kapsayıcı kullanılır.
    
    4. Blok blobundan, sayfa blobundan veya dosyadan **Depolama hizmeti**'ni seçin. Seçilen hizmetin türü, verilerin Azure'da hangi biçimde tutulmasını istediğinize bağlıdır. Örneğin, buradaki örnekte biz verilerin Azure'da blob blokları olarak tutulmasını istediğimiz için Blok Blobunu seçtik. Sayfa Blobunu seçerseniz, verilerinizi 512 bayt hizalı olduğundan emin olmalısınız. Örneğin VHDX her zaman 512 bayt hizalıdır.
   
    5. Bu adım SMB paylaşımı mı yoksa NFS paylaşımı mı oluşturduğunuza bağlıdır.
     
    - **SMB paylaşımı** - altında **tüm ayrıcalıklı yerel kullanıcı**seçin **Yeni Oluştur** veya **var olanı kullan**. Yeni bir yerel kullanıcı oluşturursanız, girin bir **kullanıcıadı** ve **parola**, ardından **parolayı onaylayın**. Bu eylem, yerel kullanıcı izinleri atar. Buradan izinler atadıktan sonra bu izinleri değiştirmek için dosya Gezgini'ni kullanabilirsiniz.
    
        ![SMB paylaşımı ekleme](./media/data-box-gateway-deploy-add-shares/add-share-smb-1.png)
        
        Seçerseniz **yalnızca okuma işlemlerini izin** onay kutusu Bu paylaşımı verileri için salt okunur kullanıcılar belirtebilirsiniz.
        
    - **NFS paylaşım** -paylaşıma erişen istemcilerin izin verilen IP adreslerini girin.

        ![NFS paylaşımı ekleme](./media/data-box-gateway-deploy-add-shares/add-share-nfs-1.png)
   
9. Seçin **Oluştur** paylaşımı oluşturmak için.
    
    Paylaşımı oluşturma sürüyor bildirim alırsınız. Belirtilen ayarlarla paylaşım oluşturulduktan sonra **paylaşımları** kutucuğunda yeni bir paylaşım yansıtacak şekilde güncelleştirmeleri.
    
    ![Güncelleştirilmiş paylaşımları kutucuğu](./media/data-box-gateway-deploy-add-shares/updated-list-of-shares.png) 

## <a name="connect-to-the-share"></a>Paylaşıma bağlanma

Artık, bir veya daha çok, son adımda oluşturduğunuz paylaşım bağlanabilirsiniz. Bir SMB veya NFS paylaşımını olmasına bağlı olarak, adımları farklılık gösterebilir.

### <a name="connect-to-an-smb-share"></a>SMB paylaşımına bağlanma

Veri kutusu Gateway'e bağlı Windows Server istemciniz üzerindeki komutları girerek bir SMB paylaşımına bağlanın:


1. Bir komut penceresinde şunu yazın:

    `net use \\<IP address of the device>\<share name>  /u:<user name for the share>`

    İstendiğinde paylaşımın parolasını girin. Burada, bu komutun örnek çıkışı gösterilir.

    ```powershell
    Microsoft Windows [Version 18.8.16299.192) 
    (c) 2017 microsoft Corporation. All rights reserved . 
    
    C: \Users\GatewayUser>net use \\10.10.10.60\newtestuser /u:Tota11yNewUser 
    Enter the password for 'TotallyNewUser' to connect to '10.10.10.60'  
    The command completed successfully. 
    
    C: \Users\GatewayUser>
    ```   


2. Windows + r klavyenizdeki seçin 
3. İçinde **çalıştırma** penceresinde belirtin `\\<device IP address>` seçip **Tamam**. Dosya Gezgini'ni açar. Artık oluşturduğunuz klasör paylaşımları görmeye olmalıdır. Dosya Gezgini'nde, içeriği görüntülemek için bir paylaşım (klasör) çift tıklayın.
 
    ![SMB paylaşımına bağlanma](./media/data-box-gateway-deploy-add-shares/connect-to-share2.png)-->

    Bu paylaşımlara oluşturulduğu anda veriler yazılır ve cihaz verileri buluta gönderir.

### <a name="connect-to-an-nfs-share"></a>NFS paylaşımına bağlanma

Veri kutusu Edge cihazınıza bağlı Linux istemciniz üzerinde aşağıdaki yordamı uygulayın:

1. İstemci NFSv4 istemcisinin yüklü olduğundan emin olun. NFS istemcisini yüklemek için aşağıdaki komutu kullanın:

   `sudo apt-get install nfs-common`

    Daha fazla bilgi için [NFSv4 istemcisini yükleme](https://help.ubuntu.com/community/SettingUpNFSHowTo#NFSv4_client) konusuna gidin.

2. NFS istemcisi yüklendikten sonra, Data Box Gateway cihazınızda oluşturduğunuz NFS paylaşımını bağlamak için aşağıdaki komutu kullanın:

   `sudo mount -t nfs -o sec=sys,resvport <device IP>:/<NFS shares on device> /home/username/<Folder on local Linux computer>`

    Bağlamaları ayarlamadan önce, yerel bilgisayarınızda bağlama noktası işlevi görecek dizinlerin zaten oluşturulmuş olduğundan ve hiçbir dosya veya alt klasör içermediğinden emin olun.

    Aşağıdaki örnekte, Gateway cihazındaki bir paylaşıma NFS yoluyla nasıl bağlanılacağı gösterilir. Sanal cihaz IP'si `10.10.10.60`dır, `mylinuxshare2` ubuntuVM'ye bağlanmıştır ve bağlama noktası `/home/databoxubuntuhost/gateway`dir.

    `sudo mount -t nfs -o sec=sys,resvport 10.10.10.60:/mylinuxshare2 /home/databoxubuntuhost/gateway`

> [!NOTE] 
> Bu sürümde aşağıdaki uyarılar geçerlidir:
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


