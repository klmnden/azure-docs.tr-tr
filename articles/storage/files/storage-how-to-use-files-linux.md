---
title: "Azure dosyaları'ı Linux ile kullanma | Microsoft Docs"
description: "Bir Azure dosya paylaşımı üzerinden SMB Linux'ta bağlama öğrenin."
services: storage
documentationcenter: na
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 6edc37ce-698f-4d50-8fc1-591ad456175d
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: renash
ms.openlocfilehash: 0a87f8572af2620420faa0e3c2e575aa8add42ab
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="use-azure-files-with-linux"></a>Azure dosyaları'ı Linux ile kullanma
[Azure Dosyaları](storage-files-introduction.md), Windows'un kolay kullanılan bulut dosya sistemidir. Azure dosya paylaşımları kullanarak Linux dağıtımları içinde takılı [CIFS çekirdek istemci](https://wiki.samba.org/index.php/LinuxCIFS). Bu makale bir Azure dosya paylaşımı bağlamak için iki yol gösterir: isteğe bağlı ile `mount` komut ve üzerinde önyükleme bir girişe oluşturarak `/etc/fstab`.

> [!NOTE]  
> Azure dışında bir Azure dosya paylaşımı bağlamak için bu, şirket içi gibi veya farklı bir bölgede Azure, işletim sistemi barındırıldığı bölgeyi SMB 3.0 şifreleme işlevlerini desteklemesi gerekir. Linux için SMB 3.0 şifreleme özelliği 4.11 Çekirdeği'nde sunulmuştur. Bu özellik Azure dosya paylaşımının şirket içi veya farklı bir Azure bölgesindeki bağlama sağlar. Yayımlama zaman bu işlevselliği 16.04 ve yukarıdaki backported Ubuntu için bırakıldı.

## <a name="prerequisities-for-mounting-an-azure-file-share-with-linux-and-the-cifs-utils-package"></a>Bir Azure dosya bağlanması için ön koşullar paylaşımı Linux ve yardımcı programları CIFS paketi
* **Yüklü CIFS yardımcı programları paketi olabilir Linux dağıtım çekme**: Microsoft Azure görüntü Galerisi içinde aşağıdaki Linux dağıtımları önerir:

    * Ubuntu Server 14.04 +
    * RHEL 7 +
    * CentOS 7 +
    * Debian 8
    * openSUSE 13.2 +
    * SUSE Linux Enterprise Server 12

* <a id="install-cifs-utils"></a>**CIFS yardımcı programları paketinin yüklü olduğu**: yardımcı programları CIFS tercih ettiğiniz Linux dağıtım noktasında Paket Yöneticisi kullanılarak yüklenebilir. 

    Üzerinde **Ubuntu** ve **Debian tabanlı** dağıtımları kullanın `apt-get` Paket Yöneticisi:

    ```
    sudo apt-get update
    sudo apt-get install cifs-utils
    ```

    Üzerinde **RHEL** ve **CentOS**, kullanın `yum` Paket Yöneticisi:

    ```
    sudo yum install samba-client samba-common cifs-utils
    ```

    Üzerinde **openSUSE**, kullanın `zypper` Paket Yöneticisi:

    ```
    sudo zypper install samba*
    ```

    Diğer dağıtımlar üzerinde uygun Paket Yöneticisi'ni kullanın veya [derleme kaynağından](https://wiki.samba.org/index.php/LinuxCIFS_utils#Download).

* **Bağlanılan paylaşımı dizin/dosya izinlerini karar**: 0777 kullanırız aşağıdaki örneklerde, okuma, yazma ve Yürütme izinleri tüm kullanıcılara. Diğer değiştirebileceğiniz [chmod izinleri](https://en.wikipedia.org/wiki/Chmod) istenen şekilde. 

* **Depolama Hesabı Adı**: Azure Dosya paylaşımını bağlayabilmeniz için depolama hesabınızın adı gerekir.

* **Depolama Hesabı Anahtarı**: Azure Dosya paylaşımını bağlayabilmeniz için birincil (veya ikincil) depolama anahtarı gerekir. SAS anahtarları şu an bağlama için desteklenmemektedir.

* **445 bağlantı noktası açık olduğundan emin olun**: SMB 445 - TCP bağlantı noktası üzerinden iletişim kurar, güvenlik duvarının TCP engellemediğinden varsa görmek için istemci makineden 445 bağlantı noktalarını kontrol edin.

## <a name="mount-the-azure-file-share-on-demand-with-mount"></a>Azure dosya paylaşımı isteğe bağlı ile bağlama`mount`
1. **[Linux dağıtımınızı CIFS yardımcı programları paketini Yükle](#install-cifs-utils)**.

2. **Bağlama noktası için bir klasör oluşturun**: Bu dosya sisteminde herhangi bir yere yapılabilir.

    ```
    mkdir mymountpoint
    ```

3. **Azure dosya paylaşımını bağlama için bağlama komutunu kullanın**: değiştirmek unutmayın `<storage-account-name>`, `<share-name>`, ve `<storage-account-key>` uygun bilgilerle.

    ```
    sudo mount -t cifs //<storage-account-name>.file.core.windows.net/<share-name> ./mymountpoint -o vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
    ```

> [!Note]  
> İşiniz bittiğinde Azure dosya paylaşımı kullanarak `sudo umount ./mymountpoint` paylaşım çıkaramadı.

## <a name="create-a-persistent-mount-point-for-the-azure-file-share-with-etcfstab"></a>Azure dosya paylaşımının için kalıcı bağlama noktası oluştur`/etc/fstab`
1. **[Linux dağıtımınızı CIFS yardımcı programları paketini Yükle](#install-cifs-utils)**.

2. **Bağlama noktası için bir klasör oluşturun**: Bu dosya sisteminde herhangi bir yere yapılabilir, ancak klasör mutlak yolu not gerekir. Aşağıdaki örnek, kök altında bir klasör oluşturur.

    ```
    sudo mkdir /mymountpoint
    ```

3. **Aşağıdaki satırı eklemek için aşağıdaki komutu kullanın `/etc/fstab`** : değiştirmek unutmayın `<storage-account-name>`, `<share-name>`, ve `<storage-account-key>` uygun bilgilerle.

    ```
    sudo bash -c 'echo "//<storage-account-name>.file.core.windows.net/<share-name> /mymountpoint cifs nofail,vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino" >> /etc/fstab'
    ```

> [!Note]  
> Eklediğiniz emin olun `nofail` seçeneğini `/etc/fstab` VM askıda yanlış yapılandırılması veya başka bir hata durumunda önyükleme sırasında Azure dosya paylaşımına bağlanması sırasında girişi, aksi takdirde.

> [!Note]  
> Kullanabileceğiniz `sudo mount -a` düzenledikten sonra Azure dosya paylaşımını bağlama için `/etc/fstab` yerine yeniden başlatılıyor.

## <a name="feedback"></a>Geri Bildirim
Linux kullanıcıları, sizden duymak istiyoruz!

Azure dosyaları Linux Kullanıcıları grubu için bir forum değerlendirin ve File storage Linux'ı benimsemeyi olarak geri bildirim paylaşmanızı sağlar. E-posta [Azure dosyaları Linux kullanıcıları](mailto:azurefileslinuxusers@microsoft.com) kullanıcıların Gruba katılmak için.

## <a name="next-steps"></a>Sonraki adımlar
Azure Dosyaları hakkında daha fazla bilgi edinmek için şu bağlantılara göz atın.
* [Dosya Hizmeti REST API başvurusu](http://msdn.microsoft.com/library/azure/dn167006.aspx)
* [Microsoft Azure storage ile AzCopy kullanma](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)
* [Azure storage ile Azure CLI kullanma](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#create-and-manage-file-shares)
* [SSS](../storage-files-faq.md)
* [Sorun giderme](storage-troubleshoot-linux-file-connection-problems.md)
