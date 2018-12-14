---
title: Azure Data Box Edge ile veri aktarma | Microsoft Docs
description: Data Box Edge cihazında paylaşımları eklemeyi ve bunlara bağlanmayı öğrenin.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 10/08/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to add and connect to shares on Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: 6c6553ace250aa9cbc06dfdfea77fc5e1637cd41
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53384828"
---
# <a name="tutorial-transfer-data-with-azure-data-box-edge-preview"></a>Öğretici: Azure veri kutusu Edge (Önizleme) ile veri aktarma

Bu öğreticide, ekleme ve veri kutusu Edge Cihazınızda paylaşımlarını bağlanma açıklanır. Paylaşımları ekledikten sonra veri kutusu Edge Azure'a veri aktarabilir.

Bu yordamın tamamlanması 10 dakika kadar sürebilir. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Paylaşım ekleme
> * Paylaşıma bağlanma

> [!IMPORTANT]
> Data Box Edge, önizleme aşamasındadır. Sipariş ve bu çözümü dağıtın önce gözden [Azure Önizleme için hizmet koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 
 
## <a name="prerequisites"></a>Önkoşullar

Veri kutusu kenarına paylaşımları eklemeden önce emin olun:
* Bölümünde anlatıldığı gibi fiziksel Cihazınızı yüklediğiniz [yükleme Azure veri kutusu Edge](data-box-edge-deploy-install.md). 

* Fiziksel cihaz açıklandığı etkinleştirildikten sonra [bağlanma, ayarlamak ve Azure veri kutusu Edge etkinleştirme](data-box-edge-deploy-connect-setup-activate.md). 

* Cihazınız paylaşım oluşturmak ve veri aktarmak için hazır durumda.


## <a name="add-a-share"></a>Paylaşım ekleme

Bir paylaşımı oluşturmak için aşağıdaki yordamı yapın:

1. İçinde [Azure portalında](https://portal.azure.com/)Git **tüm kaynakları**ve ardından veri kutusu Edge kaynak arayın.
    
1. Kaynak filtrelenmiş listesinde veri kutusu Edge kaynağınızı seçin.

1. Sol bölmede seçin **genel bakış**ve ardından **Ekle paylaşımı**.
   
   ![Paylaşım ekleme](./media/data-box-edge-deploy-add-shares/click-add-share.png)

1. İçinde **Ekle paylaşımı** bölmesinde, aşağıdaki yordamı yapın:

    a. İçinde **adı** kutusunda, paylaşımınıza için benzersiz bir ad sağlayın.  
    Paylaşım adı yalnızca küçük harfler, rakamlar ve kısa çizgi olabilir. 3 ila 63 karakter olması ve bir harf veya bir sayı ile başlaması gerekir. Kısa çizgi önce ve bir harf ya da bir sayısal sonra gelmelidir.
    
    b. Paylaşım için **Tür** seçin.  
    Tür **SMB** veya **NFS** olabilir; varsayılan tür SMB'dir. SMB Windows istemcilerinin standardıdır ve NFS de Linux istemcilerinde kullanılır.  
    SMB veya NFS paylaşımlarını seçmenize bağlı olarak, diğer seçenekleri değişir biraz. 

    c. Paylaşım depolanacağı depolama hesabı sağlayın.  
    Bir kapsayıcı zaten yoksa, yeni oluşturulan bir paylaşım adı ile depolama hesabı oluşturulur. Kapsayıcı zaten mevcutsa, bu kapsayıcı kullanılır. 
    
    d. İçinde **depolama hizmeti** aşağı açılan listesinden **blok blobu**, **sayfa blobu**, veya **dosyaları**.  
    Hangisini Azure'da kullanmak istediğiniz verilerin şirket seçtiğiniz hizmet türüne bağlıdır. Bu örnekte, Azure, blob bloklarında olarak verileri depolamak istediğimizden seçiyoruz **blok blobu**. Sayfa blobu seçerseniz, verilerinizi 512 bayt hizalı olduğundan emin olun. Örneğin VHDX her zaman 512 bayt hizalıdır.
   
    e. İster bir SMB paylaşımı veya bir NFS paylaşımına oluşturduğunuz bağlı olarak, aşağıdaki adımlardan birini uygulayın: 
     
    - **SMB paylaşımı**: Altında **tüm ayrıcalıklı yerel kullanıcı**seçin **Yeni Oluştur** veya **var olanı kullan**. Yeni bir yerel kullanıcı oluşturursanız, bir kullanıcı adı ve parola girin ve ardından Parolayı onaylayın. Bu eylem, yerel kullanıcı izinleri atar. Buradan izinler atadıktan sonra bunları değiştirmek için dosya Gezgini'ni kullanabilirsiniz.

        Seçerseniz **yalnızca okuma işlemlerini izin** Bu paylaşımı verileri için onay kutusunu, salt okunur kullanıcılar belirtebilirsiniz.

        ![SMB paylaşımı ekleme](./media/data-box-edge-deploy-add-shares/add-share-smb-1.png)
   
    - **NFS paylaşım**: Paylaşıma erişmek izin verilen istemcileri IP adreslerini girin.

        ![NFS paylaşımı ekleme](./media/data-box-edge-deploy-add-shares/add-share-nfs-1.png)
   
1. Seçin **Oluştur** paylaşımı oluşturmak için. 
    
    Paylaşımı oluşturma sürüyor bildirim alırsınız. Belirtilen ayarlarla paylaşım oluşturulduktan sonra **paylaşımları** yeni gönderebilmeleri için bilgilerinizi bölümü güncellenmiştir. 
    
    ![Güncelleştirilmiş paylaşım listesi](./media/data-box-edge-deploy-add-shares/updated-list-of-shares.png) 

## <a name="connect-to-the-share"></a>Paylaşıma bağlanma

Artık, bir veya daha çok, son adımda oluşturduğunuz paylaşım bağlanabilirsiniz. Bir SMB veya NFS paylaşımını olmasına bağlı olarak, adımları farklılık gösterebilir. 

### <a name="connect-to-an-smb-share"></a>SMB paylaşımına bağlanma

Veri kutusu Edge cihazınıza bağlı Windows Server istemciniz üzerindeki komutları girerek bir SMB paylaşımına bağlanın:


1. Bir komut penceresinde şunu yazın:

    `net use \\<IP address of the device>\<share name>  /u:<user name for the share>`

1. Bunu yapmak için istendiğinde paylaşım için parolayı girin.  
   Burada, bu komutun örnek çıkışı gösterilir.

    ```powershell
    Microsoft Windows [Version 10.0.16299.192) 
    (c) 2017 Microsoft Corporation. All rights reserved. 
    
    C: \Users\DataBoxEdgeUser>net use \\10.10.10.60\newtestuser /u:Tota11yNewUser 
    Enter the password for 'TotallyNewUser' to connect to '10.10.10.60': 
    The command completed successfully. 
    
    C: \Users\DataBoxEdgeUser>
    ```   


1. Windows + r klavyenizdeki seçin 

1. İçinde **çalıştırma** penceresinde belirtin `\\<device IP address>`ve ardından **Tamam**.  
   Dosya Gezgini'ni açar. Artık oluşturduğunuz klasör paylaşımları görmeye olmalıdır. Dosya Gezgini'nde, içeriği görüntülemek için bir paylaşım (klasör) çift tıklayın.
 
    ![SMB paylaşımına bağlanma](./media/data-box-edge-deploy-add-shares/connect-to-share2.png)

    Bu paylaşımlara oluşturulduğu anda veriler yazılır ve cihaz verileri buluta gönderir.

### <a name="connect-to-an-nfs-share"></a>NFS paylaşımına bağlanma

Veri kutusu Edge cihazınıza bağlı Linux istemciniz üzerinde aşağıdaki yordamı uygulayın:

1. İstemci NFSv4 istemcisinin yüklü olduğundan emin olun. NFS istemcisini yüklemek için aşağıdaki komutu kullanın:

   `sudo apt-get install nfs-common`

    Daha fazla bilgi için [NFSv4 istemcisini yükleme](https://help.ubuntu.com/community/SettingUpNFSHowTo#NFSv4_client) konusuna gidin.

1. NFS İstemcisi yüklendikten sonra aşağıdaki komutu kullanarak veri kutusu Edge Cihazınızda oluşturduğunuz NFS paylaşımını:

   `sudo mount <device IP>:/<NFS share on device> /home/username/<Folder on local Linux computer>`

    > [!IMPORTANT]
    > Paylaşımını bağlamadan önce yerel bilgisayarınızda bağlama görecek dizinleri önceden oluşturulduğunu emin olun. Herhangi bir dosya veya alt klasörleri bu dizinleri içermemelidir.

    Aşağıdaki örnekte, Data Box Edge cihazındaki bir paylaşıma NFS yoluyla nasıl bağlanılacağı gösterilir. Cihaz IP adresi: `10.10.10.60`. `mylinuxshare2` paylaşımı ubuntuVM öğesine bağlanmış. Paylaşımı bağlama noktası: `/home/databoxubuntuhost/edge`.

    `sudo mount -t nfs 10.10.10.60:/mylinuxshare2 /home/databoxubuntuhost/Edge`

> [!NOTE] 
> Aşağıdaki uyarılar önizleme sürümü için geçerlidir:
> - Bir dosya paylaşımı oluşturulduktan sonra dosyanın yeniden adlandırma desteklenmiyor. 
> - Bir dosya silindiğinde bir paylaşımdan depolama hesabı girişinde silmez.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki veri kutusu Edge konuları hakkında bilgi edindiniz:

> [!div class="checklist"]
> * Paylaşım ekleme
> * Paylaşıma bağlanma

Veri kutusu Edge kullanarak veri dönüştürme hakkında bilgi edinmek için sonraki öğreticiye geçin:

> [!div class="nextstepaction"]
> [Data Box Edge ile veri dönüştürme](./data-box-edge-deploy-configure-compute.md)


