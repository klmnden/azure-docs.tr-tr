---
title: Azure dosya depolama bağlama SMB kullanarak Linux vm'lerinde | Microsoft Docs
description: Azure dosya depolama SMB kullanarak Azure CLI ile Linux vm'lerinde takma
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: cynthn
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/28/2018
ms.author: cynthn
ms.openlocfilehash: 4b3bba1da5238655ca749f6464c539e53ca48f27
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60542790"
---
# <a name="mount-azure-file-storage-on-linux-vms-using-smb"></a>Azure dosya depolama bağlama SMB kullanarak Linux vm'lerinde

Bu makalede bir SMB bağlama ve Azure CLI kullanarak bir Linux VM'de Azure dosya depolama hizmetini kullanma işlemini gösterir. Azure File storage standart SMB protokolü kullanarak bulutta dosya paylaşımları sağlar. 

File storage standart SMB protokolünü kullanan dosya paylaşımları bulutta sağlar. SMB 3.0 destekleyen herhangi bir işletim sisteminizden bir dosya paylaşımı bağlayabilir. Linux üzerinde bir SMB bağlama kullandığınızda, bir SLA ile desteklenen bir sağlam, kalıcı arşivleme depolama konumuna kolay yedekleri alın.

Dosyaları bir VM'den dosya depolama üzerinde barındırılan bir SMB bağlama taşıma, hata ayıklama günlükleri için harika bir yoludur. Aynı SMB paylaşımını yerel olarak, Mac, Linux veya Windows iş istasyonunuzu bağlanabilir. Linux akış için en iyi çözüm SMB değil veya SMB protokolü gibi yoğun günlük görevlerini gerçekleştirmek için tasarlandığından değil çünkü uygulama gerçek zamanlı olarak günlüğe kaydeder. Ayrılmış, birleşik günlük katman aracı Fluentd gibi Linux ve çıkış günlüğü uygulama toplanması için SMB daha iyi bir seçim olacaktır.

Bu kılavuzda Azure CLI 2.0.4 sürüm çalıştırdığınız gerektirir veya üzeri. Sürümü bulmak için **az --version** komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). 


## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Adlı bir kaynak grubu oluşturma *myResourceGroup* içinde *Doğu ABD* konumu.

```bash
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Kullanarak, oluşturduğunuz kaynak grubunda yeni bir depolama hesabı oluşturma [az depolama hesabı oluşturma](/cli/azure/storage/account). Bu örnek adlı bir depolama hesabı oluşturur *mySTORAGEACCT\<rastgele sayı >* ve bu depolama hesabının adını değişkenine yerleştirilir **STORAGEACCT**. Depolama hesabı adları kullanarak benzersiz olmalıdır `$RANDOM` benzersiz olacak şekilde sonuna bir sayı ekler.

```bash
STORAGEACCT=$(az storage account create \
    --resource-group "myResourceGroup" \
    --name "mystorageacct$RANDOM" \
    --location eastus \
    --sku Standard_LRS \
    --query "name" | tr -d '"')
```

## <a name="get-the-storage-key"></a>Depolama anahtarını alma

Bir depolama hesabı oluşturduğunuzda, hizmet kesintilerini Döndürülmüş hesabı anahtarları çiftlerinde oluşturulur. Çiftin ikinci anahtarı geçiş yaptığınızda, yeni bir anahtar çifti oluşturun. Bu nedenle her zaman en az bir kullanılmayan depolama hesabı anahtarı geçmek hazır olması, yeni depolama hesabı anahtarlarını her zaman çiftler halinde oluşturulur.

Kullanarak depolama hesabı anahtarlarını görüntülemek [az depolama hesabı anahtarları listesi](/cli/azure/storage/account/keys). Bu örnekte anahtar 1 değerini depolar **depolama anahtarı** değişkeni.

```bash
STORAGEKEY=$(az storage account keys list \
    --resource-group "myResourceGroup" \
    --account-name $STORAGEACCT \
    --query "[0].value" | tr -d '"')
```

## <a name="create-a-file-share"></a>Dosya paylaşımı oluşturma

Dosya depolama paylaşımı kullanarak oluşturduğunuz [az depolama alanı paylaşımı oluşturma](/cli/azure/storage/share). 

Paylaşım adları tüm küçük harf, rakam ve tek kısa çizgilerden olması gerekir, ancak bir tire ile başlayamaz. Dosya paylaşımlarının ve dosyaların adlandırılması hakkında tüm ayrıntılara ulaşmak için bkz. [Paylaşımları, Dizinleri, Dosyaları ve Meta Verileri Adlandırma ve Bunlara Başvuruda Bulunma](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata)

Bu örnek adlı bir paylaşım oluşturur *myshare* 10 GiB kota. 

```bash
az storage share create --name myshare \
    --quota 10 \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY
```

## <a name="create-a-mount-point"></a>Bir bağlama noktası oluştur

Linux bilgisayarınızda Azure dosya paylaşımını bağlama için sahip olduğunuzdan emin olmamız gerekiyor **CIFS-utils** yüklü paket. Yükleme yönergeleri için bkz. [Linux dağıtımınız için CIFS-utils paketini yükleyin](../../storage/files/storage-how-to-use-files-linux.md#install-cifs-utils).

Azure dosyaları, TCP bağlantı noktası 445 üzerinden iletişim kuran çalışır durumda SMB protokolünü kullanır.  Azure dosya paylaşımınızı oluşturma ile ilgili sorunlar yaşıyorsanız, güvenlik duvarınızı TCP bağlantı noktası 445'i engellemediğinden emin olun.


```bash
mkdir -p /mnt/MyAzureFileShare
```

## <a name="mount-the-share"></a>Paylaşımını bağlama

Azure dosya paylaşımını yerel dizine bağlayın. 

```bash
sudo mount -t cifs //$STORAGEACCT.file.core.windows.net/myshare /mnt/MyAzureFileShare -o vers=3.0,username=$STORAGEACCT,password=$STORAGEKEY,dir_mode=0777,file_mode=0777,serverino
```

Yukarıdaki komut [bağlama](https://linux.die.net/man/8/mount) özgü seçenekleri ve Azure dosya paylaşımını bağlamak için komutu [CIFS](https://linux.die.net/man/8/mount.cifs). Özellikle, file_mode ve dir_mode = seçenekleri kümesi dosyalar ve dizinler izni `0777`. `0777` Okuma, yazma ve Yürütme izinleri tüm kullanıcılara izin verir. Bu izinleri diğer değerleri değiştirerek değiştirebilirsiniz [chmod izinleri](https://en.wikipedia.org/wiki/Chmod). Ayrıca diğer kullanabilirsiniz [CIFS](https://linux.die.net/man/8/mount.cifs) GID veya uid gibi seçenekleri. 


## <a name="persist-the-mount"></a>Kalıcı bağlama

Linux VM'yi yeniden başlatın, bağlı bir SMB paylaşım sırasında kaldırılan kapatıldı. Önyükleme SMB paylaşımında yeniden bağlamak için Linux /etc/fstab bir satır ekleyin. Linux, önyükleme işlemi sırasında bağlamak için gereken dosya sistemlerini listelemek için fstab dosyasını kullanır. SMB paylaşımı ekleme, File storage paylaşımını Linux VM için kalıcı bağlı dosya sistemi olmasını sağlar. Cloud-init kullandığınızda, SMB paylaşımı dosya depolama için yeni bir sanal makine ekleme mümkündür.

```bash
//myaccountname.file.core.windows.net/mystorageshare /mnt/mymountpoint cifs vers=3.0,username=mystorageaccount,password=myStorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```
Üretim ortamlarında Artırılmış güvenlik için kimlik bilgilerinizi fstab dışında depolamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Bir Linux VM oluşturma sırasında özelleştirmek için cloud-init kullanma](using-cloud-init.md)
- [Linux VM’ye disk ekleme](add-disk.md)
- [Azure CLI kullanarak bir Linux sanal diskleri şifreleme](encrypt-disks.md)

