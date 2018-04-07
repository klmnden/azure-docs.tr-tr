---
title: Linux sanal makineleri üzerinde bağlama Azure File storage ile Azure CLI 1.0 SMB kullanarak | Microsoft Docs
description: Azure File storage Linux sanal makinelerin SMB kullanılarak nasıl
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/07/2016
ms.author: v-livech
ms.openlocfilehash: 442c08a03ff3eb8e4c86f8190e16b74744aa9dd3
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="mount-azure-file-storage-on-linux-vms-by-using-smb-with-azure-cli-10"></a>Linux sanal makineleri üzerinde bağlama Azure File storage ile Azure CLI 1.0 SMB kullanarak

Bu makalede, Azure File storage sunucu ileti bloğu (SMB) protokolünü kullanarak bir Linux VM bağlama gösterilmektedir. File storage standart SMB protokolü aracılığıyla bulutta dosya paylaşımları sağlar. Gereksinimler şunlardır:

* Bir [Azure hesabı](https://azure.microsoft.com/pricing/free-trial/)
* [Güvenli Kabuk (SSH) ortak ve özel anahtar dosyaları](mac-create-ssh-keys.md)

## <a name="cli-versions-to-use"></a>Kullanmak için CLI sürümleri
Görevi tamamlamak için aşağıdaki komut satırı arabirimi (CLI) sürümlerinden birini kullanarak:

- [Azure CLI 1.0](#quick-commands) – bizim CLI Klasik ve kaynak yönetimi dağıtım modeline (Bu makalede)
- [Azure CLI 2.0](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)-bizim nesil CLI kaynak yönetimi dağıtım modeli için


## <a name="quick-commands"></a>Hızlı komutlar
Görevi hızlı bir şekilde gerçekleştirmek için bu bölümdeki adımları izleyin. Daha ayrıntılı bilgi ve içerik için başlamasını ["Ayrıntılı kılavuz"](mount-azure-file-storage-on-linux-using-smb.md#detailed-walkthrough) bölümü.

### <a name="prerequisites"></a>Önkoşullar
* Bir kaynak grubu
* Bir Azure sanal ağı
* Ağ güvenlik grubuyla bir SSH gelen
* Bir alt ağ
* Bir Azure depolama hesabı
* Azure depolama hesabı anahtarları
* Azure File storage paylaşımı
* A Linux VM

Tüm örnekleri kendi ayarlarınızla değiştirin.

### <a name="create-a-directory-for-the-local-mount"></a>Yerel bağlama için bir dizin oluşturun

```bash
mkdir -p /mnt/mymountpoint
```

### <a name="mount-the-file-storage-smb-share-to-the-mount-point"></a>SMB paylaşımı bağlama noktasına dosya depolama bağlama

```bash
sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mymountpoint -o vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

### <a name="persist-the-mount-after-a-reboot"></a>Bağlama yeniden başlatmanın ardından sürdürülmesi
Aşağıdaki satırı ekleyin `/etc/fstab`:

```bash
//myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

## <a name="detailed-walkthrough"></a>Ayrıntılı kılavuz

File storage standart SMB protokolünü kullanan dosya paylaşımları bulutta sağlar. Dosya depolama en son sürümle birlikte, SMB 3.0 destekleyen herhangi bir işletim sistemi bir dosya paylaşımı bağlayabilir. Linux üzerinde bir SMB bağlama kullandığınızda, bir SLA tarafından desteklenen bir güçlü, kalıcı arşivleme depolama konumuna kolay yedeklemelerini alın.

Dosyalar bir sanal makineden File storage'ı barındırılan bir SMB bağlama taşıma hata ayıklama günlüklerini için harika bir yoludur. SMB paylaşımı yerel olarak, Mac, Linux veya Windows iş istasyonu bağlanabilir olmasıdır. SMB Linux akış için en iyi çözüm değil veya SMB protokolü gibi yoğun günlük görevlerini işlemek için yerleşik olmayan çünkü uygulama gerçek zamanlı olarak günlüğe kaydeder. Ayrılmış, birleşik günlük katman aracı Fluentd gibi Linux ve çıkış günlüğü uygulama toplanması için SMB daha iyi bir seçim olacaktır.

Bu ayrıntılı kılavuz biz ilk File storage paylaşımı oluşturmak için gereken önkoşulları oluşturur ve ardından bir Linux VM SMB bağlar.

1. Aşağıdaki kodu kullanarak bir Azure depolama hesabı oluşturun:

    ```azurecli
    azure storage account create myStorageAccount \
    --sku-name lrs \
    --kind storage \
    -l westus \
    -g myResourceGroup
    ```

2. Depolama hesabı anahtarlarını gösterir.

    Bir depolama hesabı oluşturduğunuzda, hizmet kesintisi Döndürülmüş hesabı anahtarları çiftler halinde oluşturulur. İkinci anahtar çiftindeki için geçiş yaptığınızda, yeni bir anahtar çifti oluşturun. Yeni depolama hesabı anahtarları, her zaman çiftler halinde her zaman en az bir kullanılmayan depolama anahtarı geçmek hazır olduğunu sağlama oluşturulur. Depolama hesabı anahtarlarını göstermek için aşağıdaki kodu kullanın:

    ```azurecli
    azure storage account keys list myStorageAccount \
    --resource-group myResourceGroup
    ```
3. File storage paylaşımı oluşturun.

    File storage paylaşımını SMB paylaşımı içerir. Kota her zaman gigabayt (GB) cinsinden ifade edilir. File storage paylaşımı oluşturmak için aşağıdaki kodu kullanın:

    ```azurecli
    azure storage share create mystorageshare \
    --quota 10 \
    --account-name myStorageAccount \
    --account-key nPOgPR<--snip-->4Q==
    ```

4. Bağlama noktası dizini oluşturun.

    SMB paylaşımı bağlamak için Linux dosya sistemindeki yerel bir dizin oluşturmanız gerekir. Herhangi bir şey yazıldığı veya yerel bağlama dizininden okuma iletilir File storage'ı barındırılan SMB paylaşımına. Dizin oluşturmak için aşağıdaki kodu kullanın:

    ```bash
    sudo mkdir -p /mnt/mymountdirectory
    ```

5. Aşağıdaki kodu kullanarak SMB paylaşımı bağlayın:

    ```azurecli
    sudo mount -t cifs //myStorageAccount.file.core.windows.net/mystorageshare /mnt/mymountdirectory -o vers=3.0,username=myStorageAccount,password=myStorageAccountkey,dir_mode=0777,file_mode=0777
    ```

6. SMB bağlama yeniden başlatmalar aracılığıyla kalıcı olmasını sağlar.

    Linux VM'i yeniden başlattığınızda takılı SMB paylaşımı kapatma sırasında kaldırılan değil. Önyükleme SMB paylaşımında yeniden bağlamaya yönelik bir satır Linux /etc/fstab eklemeniz gerekir. Linux önyükleme işlemi sırasında bağlamak için gereken dosya sistemlerini listelemek için fstab dosyasını kullanır. SMB paylaşımı ekleme File storage paylaşımını Linux VM için kalıcı bağlı dosya sistemi olmasını sağlar. Bulut init kullandığınızda, SMB paylaşımı dosya depolama için yeni bir VM ekleme mümkündür.

    ```bash
    //myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [Bir Linux VM oluşturma sırasında özelleştirmek için bulut init kullanma](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Linux VM’ye disk ekleme](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Azure CLI kullanarak bir Linux VM disklerde şifrele](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
