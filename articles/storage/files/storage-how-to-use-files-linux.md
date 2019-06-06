---
title: Azure dosyaları'ı Linux ile kullanma | Microsoft Docs
description: Linux üzerinde SMB üzerinden Azure dosya paylaşımını bağlama hakkında bilgi edinin.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 03/29/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 73ed98bf950f7c9f52e2b8eeb431fe4b36bfe324
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66427932"
---
# <a name="use-azure-files-with-linux"></a>Azure Dosyaları'nı Linux ile kullanma

[Azure Dosyaları](storage-files-introduction.md), Windows'un kolay kullanılan bulut dosya sistemidir. Azure dosya paylaşımlarını kullanarak Linux dağıtımları bağlanabilir [SMB çekirdek istemci](https://wiki.samba.org/index.php/LinuxCIFS). Bu makalede Azure dosya paylaşımını bağlayabilmeniz için iki yolunu gösterir: ile isteğe bağlı `mount` komut ve üzerinde önyükleme bir girişi oluşturarak `/etc/fstab`.

> [!NOTE]  
> Örneğin, şirket içinde veya farklı bir Azure bölgesinde barındırıldığı Azure bölgesinin dışındaki bir Azure dosya paylaşımını bağlayabilmeniz için işletim Sisteminin SMB 3.0 şifreleme işlevlerini desteklemesi gerekir.

## <a name="prerequisites-for-mounting-an-azure-file-share-with-linux-and-the-cifs-utils-package"></a>Linux ve CIFS-utils paketi ile bir Azure dosya paylaşımını bağlama önkoşulları
<a id="smb-client-reqs"></a>

* **Mevcut bir Azure depolama hesabı ve dosya paylaşımını**: Bu makaleyi tamamlamak için bir depolama hesabı ve dosya paylaşımı olması gerekir. Zaten bir oluşturmadıysanız, başlangıçtan biriyle konu üzerinde bakın: [Dosya paylaşımları - oluşturma CLI](storage-how-to-use-files-cli.md).

* **Depolama hesabı adı ve anahtarı** bu makaleyi tamamlamak için depolama hesabı adı ve anahtarı gerekir. Bir bunları zaten olmalıdır CLI hızlı başlangıç adımlarını kullanarak oluşturduysanız, aksi takdirde, daha önce depolama hesabı anahtarınızı alma konusunda bilgi almak için bağlı olduğu CLI hızlı başlangıç başvurun.

* **Bağlama gereksinimlerinize uyacak şekilde bir Linux dağıtımı seçin.**  
      Azure dosyaları SMB 2.1 ve SMB 3.0 üzerinden bağlanabilir. SMB 3.0 istemciler şirket içi veya başka Azure bölgelerindeki gelen bağlantılar için kullanmanız gerekir; Azure dosyaları SMB 2.1 (veya SMB 3.0 şifreleme olmadan) reddeder. Azure dosya paylaşımı için aynı Azure bölgesindeki bir sanal makineden erişiyorsanız, dosya paylaşımını SMB 2.1 kullanıyorsa ve yalnızca şu durumlarda, erişebilir *güvenli aktarım gerekli* Azure dosya paylaşımı barındıran depolama hesabı için devre dışı bırakıldı. Her zaman güvenli aktarım gerektiren ve yalnızca SMB 3.0 şifreleme kullanarak öneririz.

    SMB 3.0 şifreleme desteği Linux çekirdek sürümü 4.11 içinde sunulmuştur ve popüler Linux dağıtımları için eski çekirdek sürümlerine backported olmuştur. Bu belgenin yayın zaman bağlama seçeneği tablo üstbilgisinde belirtilen Azure Galerisi şu dağıtımlarını destekler. 

* **Önerilen en az sürümleriyle ilgili bağlama özellikleri (SMB sürüm 2.1 ve SMB 3.0 sürümü)**    

    |   | SMB 2.1 <br>(Aynı Azure bölgesindeki VM'ler üzerinde başlatmalar) | SMB 3.0 <br>(Şirket içinde ve bölgeler arası gelen başlatmalar) |
    | --- | :---: | :---: |
    | Ubuntu Server | 14.04+ | 16.04+ |
    | RHEL | 7+ | 7.5+ |
    | CentOS | 7+ |  7.5+ |
    | Debian | 8+ |   |
    | openSUSE | 13.2+ | 42.3+ |
    | SUSE Linux Enterprise Server | 12 | 12 SP3+ |

    Linux dağıtımınıza burada listelenmiyorsa, aşağıdaki komutla Linux çekirdek sürümü görmek için kontrol edebilirsiniz:

   ```bash
   uname -r
   ```

* <a id="install-cifs-utils"></a>**CIFS-utils paketi yüklenir.**  
    CIFS-utils paketin, Paket Yöneticisi ettiğiniz Linux dağıtımını kullanarak yüklenebilir. 

    Üzerinde **Ubuntu** ve **Debian tabanlı** dağıtımları, kullanın `apt-get` Paket Yöneticisi:

    ```bash
    sudo apt-get update
    sudo apt-get install cifs-utils
    ```

    Üzerinde **RHEL** ve **CentOS**, kullanın `yum` Paket Yöneticisi:

    ```bash
    sudo yum install cifs-utils
    ```

    Üzerinde **openSUSE**, kullanın `zypper` Paket Yöneticisi:

    ```bash
    sudo zypper install cifs-utils
    ```

    Diğer dağıtımlarında, uygun paket yöneticisini kullanın veya [kaynaktan derleme](https://wiki.samba.org/index.php/LinuxCIFS_utils#Download)

* **Bağlanılan paylaşımı dizin/dosya izinlerini karar**: Aşağıda izin örneklerde `0777` olan okuma vermek için kullanılan, yazma ve Yürütme izinleri tüm kullanıcılara. Diğer değiştirebileceğiniz [chmod izinleri](https://en.wikipedia.org/wiki/Chmod) istenen şekilde.

* **Bağlantı noktası 445'in açık olduğundan emin olun**: SMB, TCP bağlantı noktası 445 üstünden iletişim kurar. İstemci makinenizde güvenlik duvarının TCP bağlantı noktaları 445’i engellemediğinden emin olun.

## <a name="mount-the-azure-file-share-on-demand-with-mount"></a>Azure dosya paylaşımı ile isteğe bağlı bağlama `mount`

1. **[Linux dağıtımınız için CIFS-utils paketini yükleyin](#install-cifs-utils)** .

1. **Bağlama noktası için bir klasör oluşturun**: Bir bağlama noktası için bir klasör dosya sisteminde herhangi bir yere oluşturulabilir, ancak bu altında yeni bir klasör oluşturmak için ortak bir kuraldır. Örneğin, aşağıdaki komut, yeni bir dizin oluşturur, değiştirin **< depolama_hesabı_adı >** ve **< file_share_name >** ortamınız için uygun bilgi ile:

    ```bash
    mkdir -p <storage_account_name>/<file_share_name>
    ```

1. **Azure dosya paylaşımını bağlayabilmeniz için bağlama komutu kullanın**: Değiştirmeyi unutmayın **< depolama_hesabı_adı >** , **< paylaşım_adı >** , **< smb_version >** , **< storage_account_key >** , ve **< mount_point >** ortamınız için uygun bilgi ile. Linux dağıtımınıza SMB 3.0 şifreleme ile destekliyorsa, (bkz [anlamak SMB istemci gereksinimleri](#smb-client-reqs) daha fazla bilgi için), kullanın **3.0** için **< smb_version >** . SMB 3.0 şifreleme ile desteklemeyen Linux dağıtımları için kullanmak **2.1** için **< smb_version >** . Azure dosya paylaşımının yalnızca bir Azure bölgesi dışında bağlanabilir (dahil olmak üzere şirket içinde veya farklı bir Azure bölgesinde) SMB 3.0 ile. 

    ```bash
    sudo mount -t cifs //<storage_account_name>.file.core.windows.net/<share_name> <mount_point> -o vers=<smb_version>,username=<storage_account_name>,password=<storage_account_key>,dir_mode=0777,file_mode=0777,serverino
    ```

> [!Note]  
> İşiniz bittiğinde Azure dosya paylaşımı kullanarak `sudo umount <mount_point>` paylaşımını ayırmak için.

## <a name="create-a-persistent-mount-point-for-the-azure-file-share-with-etcfstab"></a>Kalıcı bir bağlama noktası ile Azure dosya paylaşımı oluşturma `/etc/fstab`

1. **[Linux dağıtımınız için CIFS-utils paketini yükleyin](#install-cifs-utils)** .

1. **Bağlama noktası için bir klasör oluşturun**: Bir bağlama noktası için bir klasör dosya sisteminde herhangi bir yere oluşturulabilir, ancak bu altında yeni bir klasör oluşturmak için ortak bir kuraldır. Bu oluşturduğunuz her yerde, klasörün mutlak yolu unutmayın. Örneğin, aşağıdaki komut, yeni bir dizin oluşturur, değiştirin **< depolama_hesabı_adı >** ve **< file_share_name >** ortamınız için uygun bilgi ile.

    ```bash
    sudo mkdir -p <storage_account_name>/<file_share_name>
    ```

1. **Kullanıcı (depolama hesabı adı) ve parolasını (depolama hesabı anahtarı) dosya paylaşımı için depolamak için bir kimlik bilgileri dosyası oluşturun.** Değiştirin **< depolama_hesabı_adı >** ve **< storage_account_key >** ortamınız için uygun bilgi ile.

    ```bash
    if [ ! -d "/etc/smbcredentials" ]; then
    sudo mkdir /etc/smbcredentials
    fi
    if [ ! -f "/etc/smbcredentials/<STORAGE ACCOUNT NAME>.cred" ]; then
    sudo bash -c 'echo "username=<STORAGE ACCOUNT NAME>" >> /etc/smbcredentials/<STORAGE ACCOUNT NAME>.cred'
    sudo bash -c 'echo "password=7wRbLU5ea4mgc<DRIVE LETTER>PIpUCNcuG9gk2W4S2tv7p0cTm62wXTK<DRIVE LETTER>CgJlBJPKYc4VMnwhyQd<DRIVE LETTER>UT<DRIVE LETTER>yR5/RtEHyT/EHtg2Q==" >> /etc/smbcredentials/<STORAGE ACCOUNT NAME>.cred'
    fi
    ```

1. **Yalnızca kök okuyabilen veya değiştirebilen parola dosyası için kimlik bilgileri dosyası üzerindeki izinleri değiştirin.** Depolama hesabı anahtarını aslında bir süper yönetici parolası depolama hesabı için olduğundan, yalnızca kök gibi erişebilirsiniz dosyanın izinlerini ayarlama, daha düşük ayrıcalıklı kullanıcıyı depolama hesabı anahtarı alınamıyor böylece önemlidir.   

    ```bash
    sudo chmod 600 /etc/smbcredentials/<storage_account_name>.cred
    ```

1. **Aşağıdaki satırı eklemek için aşağıdaki komutu kullanın `/etc/fstab`** : Değiştirmeyi unutmayın **< depolama_hesabı_adı >** , **< paylaşım_adı >** , **< smb_version >** , ve **< mount_point >** ortamınız için uygun bilgi ile. Linux dağıtımınıza SMB 3.0 şifreleme ile destekliyorsa, (bkz [anlamak SMB istemci gereksinimleri](#smb-client-reqs) daha fazla bilgi için), kullanın **3.0** için **< smb_version >** . SMB 3.0 şifreleme ile desteklemeyen Linux dağıtımları için kullanmak **2.1** için **< smb_version >** . Azure dosya paylaşımının yalnızca bir Azure bölgesi dışında bağlanabilir (dahil olmak üzere şirket içinde veya farklı bir Azure bölgesinde) SMB 3.0 ile.

    ```bash
    sudo bash -c 'echo "//<STORAGE ACCOUNT NAME>.file.core.windows.net/<FILE SHARE NAME> /mount/<STORAGE ACCOUNT NAME>/<FILE SHARE NAME> cifs nofail,vers=3.0,credentials=/etc/smbcredentials/<STORAGE ACCOUNT NAME>.cred,dir_mode=0777,file_mode=0777,serverino" >> /etc/fstab'

    sudo mount /mount/<STORAGE ACCOUNT NAME>/<FILE SHARE NAME>
    ```

> [!Note]  
> Kullanabileceğiniz `sudo mount -a` düzenledikten sonra Azure dosya paylaşımını bağlayabilmeniz için `/etc/fstab` yerine yeniden başlatılıyor.

## <a name="feedback"></a>Geri Bildirim

Linux kullanıcıları, görüşlerinizi merak ediyoruz!

Azure dosyaları için Linux Kullanıcıları grubu olarak değerlendirmek ve Linux'ta dosya depolama benimseyin geri bildirim paylaşmak bir forum sağlar. E-posta [Azure dosyaları Linux kullanıcıları](mailto:azurefileslinuxusers@microsoft.com) kullanıcıların gruba katılma.

## <a name="next-steps"></a>Sonraki adımlar

Azure Dosyaları hakkında daha fazla bilgi edinmek için şu bağlantılara göz atın:

* [Azure Dosyaları dağıtımını planlama](storage-files-planning.md)
* [SSS](../storage-files-faq.md)
* [Sorun giderme](storage-troubleshoot-linux-file-connection-problems.md)
