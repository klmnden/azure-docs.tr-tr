---
title: macOS’da SMB üzerinden Azure dosya paylaşımını bağlama | Microsoft Docs
description: macOS’da SMB üzerinden Azure dosya paylaşımını bağlamayı öğrenin.
services: storage
author: RenaShahMSFT
ms.service: storage
ms.topic: get-started-article
ms.date: 09/19/2017
ms.author: renash
ms.component: files
ms.openlocfilehash: ace77b8a15ace71b5b372564bc10c6f4845e1482
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39527227"
---
# <a name="mount-azure-file-share-over-smb-with-macos"></a>macOS’da SMB üzerinden Azure dosya paylaşımını bağlama
[Azure Dosyaları](storage-files-introduction.md), Microsoft’un kullanımı kolay bulut dosya sistemidir. Azure dosya paylaşımları macOS El Capitan 10.11+ üzerine endüstri standardı SMB 3 protokolü kullanılarak takılabilir. Bu makalede, macOS’ta Bulucu kullanıcı arabirimiyle ve Terminal kullanarak Azure dosya paylaşımını bağlamanın iki farklı yolu gösterilir.

> [!Note]  
> SMB üzerinden Azure dosya paylaşımını bağlamadan önce, SMB paket imzalamasını devre dışı bırakmanızı öneririz. Bunu yapmazsanız, macOS’tan Azure dosya paylaşımına erişirken performansta düşüş olabilir. SMB bağlantınız şifrelenir, bu sayede bağlantınızın güvenliği etkilenmez. Terminalde, aşağıdaki komutlar [SMB paket imzalamasını devre dışı bırakma üzerine Apple destek makalesinde](https://support.apple.com/HT205926) anlatıldığı gibi SMB paket imzalamasını devre dışı bırakır:  
>    ```
>    sudo -s
>    echo "[default]" >> /etc/nsmb.conf
>    echo "signing_required=no" >> /etc/nsmb.conf
>    exit
>    ```

## <a name="prerequisites-for-mounting-an-azure-file-share-on-macos"></a>macOS’ta Azure dosya paylaşımını bağlama önkoşulları
* **Depolama hesabı adı**: Azure dosya paylaşımını bağlayabilmeniz için depolama hesabınızın adı gerekir.

* **Depolama hesabı anahtarı**: Azure dosya paylaşımını bağlayabilmeniz için birincil (veya ikincil) depolama anahtarı gerekir. SAS anahtarları şu an bağlama için desteklenmemektedir.

* **Bağlantı noktası 445’in açık olduğundan emin olun**: SMB, TCP bağlantı noktası 445 üzerinden iletişim kurar. İstemci makinenizde (Mac) güvenlik duvarının TCP bağlantı noktası 445’i engellemediğinden emin olun.

## <a name="mount-an-azure-file-share-via-finder"></a>Bulucu yoluyla Azure dosya paylaşımını bağlama
1. **Bulucu’yu açın**: Bulucu varsayılan olarak macOS’ta açıktır, ancak bunun seçili durumdaki uygulama olduğundan emin olmak için dock’taki “macOS yüz simgesine” tıklayın:  
    ![macOS yüz simgesi](./media/storage-how-to-use-files-mac/mount-via-finder-1.png)

2. **”Git” menüsünden “Sunucuya Bağlan”ı seçin**: [Önkoşullar](#preq)’dan UNC adını kullanarak başlangıç çift eğik çizgisini (`\\`) `smb://` ile ve diğer tüm ters eğik çizgileri (`\`) eğik çizgiyle (`/`) değiştirin. Bağlantınız şu şekilde görünmelidir: ![“Sunucuya Bağlan” iletişim kutusu](./media/storage-how-to-use-files-mac/mount-via-finder-2.png)

3. **Kullanıcı adı ve parola istendiğinde depolama hesabı adını ve depolama hesabı anahtarını kullanın**: “Sunucuya Bağlan” iletişim kutusunda “Bağlan”a tıkladığınızda kullanıcı adı ve parola istenir (Bu alan otomatik olarak macOS kullanıcı adınızla doldurulur). macOS Anahtarlığınıza depolama hesabı adı/depolama hesabı anahtarı yerleştirebilirsiniz.

4. **Azure dosya paylaşımını istediğiniz gibi kullanın**: Kullanıcı adı ve parola yerine paylaşım adını ve depolama hesabı anahtarını koyduktan sonra paylaşım bağlanacaktır. Bunu, yerel klasör/dosya paylaşımını normalde kullandığınız gibi kullanabilirsiniz. Örneğin, dosyaları dosya paylaşımına sürükleyip bırakabilirsiniz:

    ![Bağlı bir Azure dosya paylaşımının anlık görüntüsü](./media/storage-how-to-use-files-mac/mount-via-finder-3.png)

## <a name="mount-an-azure-file-share-via-terminal"></a>Terminal yoluyla Azure dosya paylaşımını bağlama
1. `<storage-account-name>` değerini depolama hesabınızın adıyla değiştirin. İstendiğinde parola olarak Depolama Hesabı Anahtarı’nı girin. 

    ```
    mount_smbfs //<storage-account-name>@<storage-account-name>.file.core.windows.net/<share-name> <desired-mount-point>
    ```

2. **Azure dosya paylaşımını istediğiniz gibi kullanın**: Azure dosya paylaşımı, önceki komutta belirtilmiş olan bağlama noktasına bağlanır.  

    ![Bağlı Azure dosya paylaşımının anlık görüntüsü](./media/storage-how-to-use-files-mac/mount-via-terminal-1.png)

## <a name="next-steps"></a>Sonraki adımlar
Azure Dosyaları hakkında daha fazla bilgi edinmek için şu bağlantılara göz atın.

* [Apple Destek Makalesi - Mac’inizde Dosya Paylaşımı ile bağlantı kurma](https://support.apple.com/HT204445)
* [SSS](../storage-files-faq.md)
* [Windows’da sorun giderme](storage-troubleshoot-windows-file-connection-problems.md)      
* [Linux’ta sorun giderme](storage-troubleshoot-linux-file-connection-problems.md)    
