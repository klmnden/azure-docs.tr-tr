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
ms.date: 12/20/2017
ms.author: renash
ms.openlocfilehash: cca0d315a815faca5db07099b8e8e451ef55fad5
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="use-azure-files-with-linux"></a>Azure dosyaları'ı Linux ile kullanma
[Azure Dosyaları](storage-files-introduction.md), Windows'un kolay kullanılan bulut dosya sistemidir. Azure dosya paylaşımları kullanarak Linux dağıtımları içinde takılı [CIFS çekirdek istemci](https://wiki.samba.org/index.php/LinuxCIFS). Bu makale bir Azure dosya paylaşımı bağlamak için iki yol gösterir: isteğe bağlı ile `mount` komut ve üzerinde önyükleme bir girişe oluşturarak `/etc/fstab`.

> [!NOTE]  
> İçinde şirket içi gibi veya farklı bir Azure bölgesindeki barındırıldığı Azure bölgesi dışında bir Azure dosya paylaşımını bağlama için işletim sistemi SMB 3.0 şifreleme işlevlerini desteklemesi gerekir.

## <a name="prerequisites-for-mounting-an-azure-file-share-with-linux-and-the-cifs-utils-package"></a>Linux ve CIFS yardımcı programları paket Azure dosya paylaşımıyla bağlanması için Önkoşullar
* **Yüklü CIFS yardımcı programları paketi olabilir Linux dağıtımı seçin.**  
    Aşağıdaki Linux dağıtımları Azure galerisinde kullanılabilir:

    * Ubuntu Server 14.04+
    * RHEL 7+
    * CentOS 7+
    * Debian 8 +
    * openSUSE 13.2+
    * SUSE Linux Enterprise Server 12

* <a id="install-cifs-utils"></a>**CIFS yardımcı programları paketi yüklenir.**  
    CIFS yardımcı programları paket tercih ettiğiniz Linux dağıtım noktasında Paket Yöneticisi kullanılarak yüklenebilir. 

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

* <a id="smb-client-reqs"></a>**SMB istemci gereksinimlerini anlayın.**  
    Azure dosyaları, SMB 2.1 ve SMB 3.0 üzerinden bağlanabilir. İstemcilerin şirket içi veya başka Azure bölgelerindeki gelen bağlantılar için SMB 2.1 (veya SMB 3.0 şifreleme olmadan) Azure dosyaları reddeder. Varsa *güvenli aktarımı gerekli* etkin bir depolama hesabı için Azure dosyaları yalnızca şifrelemesi ile SMB 3.0 kullanan bağlantılara izin verir.
    
    SMB 3.0 şifreleme desteği Linux çekirdek sürüm 4.11 sunulmuştur ve popüler Linux dağıtımları için eski çekirdek sürümlere backported olmuştur. Bu belgenin yayın aynı anda bu özellik Azure galerisinden aşağıdaki dağıtımları destekler:

    - Ubuntu Server 16.04+
    - openSUSE 42.3 +
    - SUSE Linux Enterprise Server 12 SP3+
    
    Linux dağıtımınız burada listede yoksa aşağıdaki komutu kullanarak Linux çekirdek sürümü görmek için kontrol edebilirsiniz:

    ```
    uname -r
    ```

* **Bağlanılan paylaşımı dizin/dosya izinlerini karar**: izni aşağıdaki örneklerde `0777` olan okuma vermek için kullanılan, yazma ve Yürütme izinleri tüm kullanıcılara. Diğer değiştirebileceğiniz [chmod izinleri](https://en.wikipedia.org/wiki/Chmod) istenen şekilde. 

* **Depolama hesabı adı**: Azure dosya paylaşımını bağlama için depolama hesabı adı gerekir.

* **Depolama hesabı anahtarı**: Azure dosya paylaşımı bağlamak için birincil (veya ikincil) depolama anahtarı gerekir. SAS anahtarları şu an bağlama için desteklenmemektedir.

* **445 bağlantı noktası açık olduğundan emin olun**: SMB 445 - TCP bağlantı noktası üzerinden iletişim kurar, güvenlik duvarının TCP engellemediğinden varsa görmek için istemci makineden 445 bağlantı noktalarını kontrol edin.

## <a name="mount-the-azure-file-share-on-demand-with-mount"></a>Azure dosya paylaşımı isteğe bağlı ile bağlama`mount`
1. **[Linux dağıtımınızı CIFS yardımcı programları paketini Yükle](#install-cifs-utils)**.

2. **Bağlama noktası için bir klasör oluşturun**: bir bağlama noktası için bir klasör dosya sisteminde herhangi bir yerden oluşturulabilir, ancak bu altında oluşturmak için ortak bir kuraldır `/mnt` klasör. Örneğin:

    ```
    mkdir /mnt/MyAzureFileShare
    ```

3. **Azure dosya paylaşımını bağlama için bağlama komutunu kullanın**: değiştirmek unutmayın `<storage-account-name>`, `<share-name>`, `<smb-version>`, `<storage-account-key>`, ve `<mount-point>` ortamınız için uygun bilgilerle. Linux dağıtımınız şifrelemesi ile SMB 3.0 destekliyorsa (bkz [anlamak SMB istemci gereksinimleri](#smb-client-reqs) daha fazla bilgi için), kullanın `3.0` için `<smb-version>`. SMB 3.0 şifrelemesi ile desteklemeyen Linux dağıtımları için kullanmak `2.1` için `<smb-version>`. Azure dosya paylaşımının yalnızca bir Azure bölgesi dışında bağlanabilir unutmayın (şirket içi dahil olmak üzere veya farklı bir Azure bölgesindeki) SMB 3.0 ile. 

    ```
    sudo mount -t cifs //<storage-account-name>.file.core.windows.net/<share-name> <mount-point> -o vers=<smb-version>,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
    ```

> [!Note]  
> İşiniz bittiğinde Azure dosya paylaşımı kullanarak `sudo umount <mount-point>` paylaşım çıkaramadı.

## <a name="create-a-persistent-mount-point-for-the-azure-file-share-with-etcfstab"></a>Azure dosya paylaşımının için kalıcı bağlama noktası oluştur`/etc/fstab`
1. **[Linux dağıtımınızı CIFS yardımcı programları paketini Yükle](#install-cifs-utils)**.

2. **Bağlama noktası için bir klasör oluşturun**: bir bağlama noktası için bir klasör dosya sisteminde herhangi bir yerden oluşturulabilir, ancak bu altında oluşturmak için ortak bir kuraldır `/mnt` klasör. Bu oluşturduğunuz her yerde, mutlak yolu klasör unutmayın. Örneğin, aşağıdaki komut altında yeni bir klasör oluşturur `/mnt` (yolu mutlak bir yol değil).

    ```
    sudo mkdir /mnt/MyAzureFileShare
    ```

3. **Aşağıdaki satırı eklemek için aşağıdaki komutu kullanın `/etc/fstab`** : değiştirmek unutmayın `<storage-account-name>`, `<share-name>`, `<smb-version>`, `<storage-account-key>`, ve `<mount-point>` için uygun bilgilerle, ortam. Linux dağıtımınız şifrelemesi ile SMB 3.0 destekliyorsa (bkz [anlamak SMB istemci gereksinimleri](#smb-client-reqs) daha fazla bilgi için), kullanın `3.0` için `<smb-version>`. SMB 3.0 şifrelemesi ile desteklemeyen Linux dağıtımları için kullanmak `2.1` için `<smb-version>`. Azure dosya paylaşımının yalnızca bir Azure bölgesi dışında bağlanabilir unutmayın (şirket içi dahil olmak üzere veya farklı bir Azure bölgesindeki) SMB 3.0 ile. 

    ```
    sudo bash -c 'echo "//<storage-account-name>.file.core.windows.net/<share-name> <mount-point> cifs nofail,vers=<smb-version>,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino" >> /etc/fstab'
    ```

> [!Note]  
> Kullanabileceğiniz `sudo mount -a` düzenledikten sonra Azure dosya paylaşımını bağlama için `/etc/fstab` yerine yeniden başlatılıyor.

## <a name="feedback"></a>Geri Bildirim
Linux kullanıcıları, sizden duymak istiyoruz!

Azure dosyaları Linux Kullanıcıları grubu için bir forum değerlendirin ve File storage Linux'ı benimsemeyi olarak geri bildirim paylaşmanızı sağlar. E-posta [Azure dosyaları Linux kullanıcıları](mailto:azurefileslinuxusers@microsoft.com) kullanıcıların Gruba katılmak için.

## <a name="next-steps"></a>Sonraki adımlar
Azure Dosyaları hakkında daha fazla bilgi edinmek için şu bağlantılara göz atın.
* [Azure dosyaları giriş](storage-files-introduction.md)
* [Bir Azure dosyaları dağıtımını planlama](storage-files-planning.md)
* [SSS](../storage-files-faq.md)
* [Sorun giderme](storage-troubleshoot-linux-file-connection-problems.md)