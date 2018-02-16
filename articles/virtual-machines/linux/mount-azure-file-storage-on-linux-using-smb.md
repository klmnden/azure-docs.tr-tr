---
title: "Bağlama Azure File storage SMB kullanarak Linux vm'lerinde | Microsoft Docs"
description: "Azure CLI 2.0 ile SMB kullanarak Linux vm'lerinde Azure File storage nasıl"
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/13/2017
ms.author: v-livech
ms.openlocfilehash: 4566e9b236049c336858e9149cca80066b029775
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="mount-azure-file-storage-on-linux-vms-using-smb"></a>SMB kullanarak Linux VM'ler üzerinde bağlama Azure dosya depolama

Bu makalede Azure CLI 2.0 ile SMB bağlama kullanarak bir Linux VM üzerinde Azure dosya depolama hizmeti kullanmaya nasıl gösterir. Azure File storage standart SMB protokolü kullanılarak bulutta dosya paylaşımları sağlar. Bu adımları [Azure CLI 1.0](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ile de gerçekleştirebilirsiniz. Gereksinimler şunlardır:

- [Bir Azure hesabı](https://azure.microsoft.com/pricing/free-trial/)
- [SSH ortak ve özel anahtar dosyaları](mac-create-ssh-keys.md)

## <a name="quick-commands"></a>Hızlı Komutlar

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
Bunu yapmak için aşağıdaki satırı ekleyin `/etc/fstab`:

```bash
//myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

## <a name="detailed-walkthrough"></a>Ayrıntılı kılavuz

File storage standart SMB protokolünü kullanan dosya paylaşımları bulutta sağlar. Dosya depolama en son sürümle birlikte, SMB 3.0 destekleyen herhangi bir işletim sistemi bir dosya paylaşımı bağlayabilir. Linux üzerinde bir SMB bağlama kullandığınızda, bir SLA tarafından desteklenen bir güçlü, kalıcı arşivleme depolama konumuna kolay yedeklemelerini alın.

Dosyalar bir sanal makineden File storage'ı barındırılan bir SMB bağlama taşıma hata ayıklama günlüklerini için harika bir yoludur. SMB paylaşımı yerel olarak, Mac, Linux veya Windows iş istasyonu bağlanabilir olmasıdır. SMB Linux akış için en iyi çözüm değil veya SMB protokolü gibi yoğun günlük görevlerini işlemek için yerleşik olmayan çünkü uygulama gerçek zamanlı olarak günlüğe kaydeder. Ayrılmış, birleşik günlük katman aracı Fluentd gibi Linux ve çıkış günlüğü uygulama toplanması için SMB daha iyi bir seçim olacaktır.

Bu ayrıntılı kılavuz biz ilk File storage paylaşımı oluşturmak için gereken önkoşulları oluşturur ve ardından bir Linux VM SMB bağlar.

1. Sahip bir kaynak grubu oluşturma [az grubu oluşturma](/cli/azure/group#az_group_create) dosya paylaşımında tutmak için.

    Adlı bir kaynak grubu oluşturmak için `myResourceGroup` "Batı ABD" konumunda, aşağıdaki örneği kullanın:

    ```azurecli
    az group create --name myResourceGroup --location westus
    ```

2. Bir Azure depolama hesabıyla oluşturma [az depolama hesabı oluşturma](/cli/azure/storage/account#az_storage_account_create) gerçek dosyalarını depolamak için.

    Standard_LRS depolama SKU kullanarak mystorageaccount adlı bir depolama hesabı oluşturmak için aşağıdaki örneği kullanın:

    ```azurecli
    az storage account create --resource-group myResourceGroup \
        --name mystorageaccount \
        --location westus \
        --sku Standard_LRS
    ```

3. Depolama hesabı anahtarlarını gösterir.

    Bir depolama hesabı oluşturduğunuzda, hizmet kesintisi Döndürülmüş hesabı anahtarları çiftler halinde oluşturulur. İkinci anahtar çiftindeki için geçiş yaptığınızda, yeni bir anahtar çifti oluşturun. Yeni depolama hesabı anahtarları, her zaman çiftler halinde, her zaman en az bir kullanılmayan depolama hesabı anahtarı geçmek hazır olmasını sağlayarak oluşturulur.

    İle depolama hesabı anahtarlarını görüntülemek [az depolama hesabı anahtarları listesi](/cli/azure/storage/account/keys#az_storage_account_keys_list). Depolama hesabı anahtarları adlandırılmış `mystorageaccount` aşağıda listelenmiştir:

    ```azurecli
    az storage account keys list --resource-group myResourceGroup \
        --account-name mystorageaccount
    ```

    Tek bir anahtar ayıklamak için kullanma `--query` bayrağı. Aşağıdaki örnek ilk anahtar ayıklar (`[0]`):

    ```azurecli
    az storage account keys list --resource-group myResourceGroup \
        --account-name mystorageaccount \
        --query '[0].{Key:value}' --output tsv
    ```

4. File storage paylaşımı oluşturun.

    File storage paylaşımını SMB paylaşımıyla içeren [az depolama paylaşımı oluşturmak](/cli/azure/storage/share#az_storage_share_create). Kota her zaman gigabayt (GB) cinsinden ifade edilir. Önceki geçişinde anahtarlarından birini `az storage account keys list` komutu. Aşağıdaki örnek kullanarak 10 GB kota mystorageshare adlı bir paylaşım oluşturun:

    ```azurecli
    az storage share create --name mystorageshare \
        --quota 10 \
        --account-name mystorageaccount \
        --account-key nPOgPR<--snip-->4Q==
    ```

5. Bir bağlama noktası dizin oluşturun.

    SMB paylaşımı bağlamak için Linux dosya sistemindeki yerel bir dizin oluşturun. Herhangi bir şey yazıldığı veya yerel bağlama dizininden okuma iletilir File storage'ı barındırılan SMB paylaşımına. Yerel bir dizine /mnt/mymountdirectory oluşturmak için aşağıdaki örneği kullanın:

    ```bash
    sudo mkdir -p /mnt/mymountdirectory
    ```

6. Yerel dizin SMB paylaşımı bağlayın.

    Aşağıdaki gibi kendi depolama hesap kullanıcı adı ve depolama hesabı anahtarı bağlama kimlik bilgileri sağlayın:

    ```azurecli
    sudo mount -t cifs //myStorageAccount.file.core.windows.net/mystorageshare /mnt/mymountdirectory -o vers=3.0,username=mystorageaccount,password=mystorageaccountkey,dir_mode=0777,file_mode=0777
    ```

7. SMB bağlama yeniden başlatmalar aracılığıyla kalıcı olmasını sağlar.

    Linux VM'i yeniden başlattığınızda takılı SMB paylaşımı kapatma sırasında kaldırılan değil. Önyükleme SMB paylaşımında yeniden bağlamaya yönelik Linux /etc/fstab bir satır ekleyin. Linux önyükleme işlemi sırasında bağlamak için gereken dosya sistemlerini listelemek için fstab dosyasını kullanır. SMB paylaşımı ekleme File storage paylaşımını Linux VM için kalıcı bağlı dosya sistemi olmasını sağlar. Bulut init kullandığınızda, SMB paylaşımı dosya depolama için yeni bir VM ekleme mümkündür.

    ```bash
    //myaccountname.file.core.windows.net/mystorageshare /mnt/mymountdirectory cifs vers=3.0,username=mystorageaccount,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [Bir Linux VM oluşturma sırasında özelleştirmek için bulut init kullanma](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Linux VM’ye disk ekleme](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Azure CLI kullanarak bir Linux VM disklerde şifrele](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
