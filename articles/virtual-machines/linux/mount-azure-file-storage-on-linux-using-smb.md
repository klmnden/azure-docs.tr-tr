---
title: Bağlama Azure File storage SMB kullanarak Linux vm'lerinde | Microsoft Docs
description: SMB ile Azure CLI kullanarak Linux vm'lerinde Azure File storage nasıl
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
ms.openlocfilehash: 2019324030b2e4c469d0b9ba937fb40a9d0675f1
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37099720"
---
# <a name="mount-azure-file-storage-on-linux-vms-using-smb"></a>SMB kullanarak Linux VM'ler üzerinde bağlama Azure dosya depolama


Bu makalede bir SMB bağlama ile Azure CLI kullanarak bir Linux VM Azure dosya depolama hizmeti kullanmayı gösterir. Azure File storage standart SMB protokolü kullanılarak bulutta dosya paylaşımları sağlar. 

File storage standart SMB protokolünü kullanan dosya paylaşımları bulutta sağlar. SMB 3.0 destekleyen herhangi bir işletim sistemi bir dosya paylaşımı bağlayabilir. Linux üzerinde bir SMB bağlama kullandığınızda, bir SLA tarafından desteklenen bir güçlü, kalıcı arşivleme depolama konumuna kolay yedeklemelerini alın.

Dosyalar bir sanal makineden File storage'ı barındırılan bir SMB bağlama taşıma hata ayıklama günlüklerini için harika bir yoludur. SMB paylaşımı, Mac, Linux veya Windows iş istasyonu yerel olarak bağlanabilir. SMB Linux akış için en iyi çözüm değil veya SMB protokolü gibi yoğun günlük görevlerini işlemek için yerleşik olmayan çünkü uygulama gerçek zamanlı olarak günlüğe kaydeder. Ayrılmış, birleşik günlük katman aracı Fluentd gibi Linux ve çıkış günlüğü uygulama toplanması için SMB daha iyi bir seçim olacaktır.

Bu kılavuz, Azure CLI Sürüm 2.0.4 çalıştırdığınız gerektirir veya sonraki bir sürümü. Sürümü bulmak için **az --version** komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). 


## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Adlı bir kaynak grubu oluşturmak *myResourceGroup* içinde *Doğu ABD* konumu.

```bash
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Kullanılarak oluşturulan kaynak grubunda yeni bir depolama hesabı oluşturma [az depolama hesabı oluşturma](/cli/azure/storage/account#create). Bu örnek adlı bir depolama hesabı oluşturur *mySTORAGEACCT<random number>*  ve depolama hesabının adını değişkende yerleştirir **STORAGEACCT**. Depolama hesabı adları kullanarak benzersiz olmalıdır `$RANDOM` birtakım benzersiz hale getirmek amacıyla sonuna ekler.

```bash
STORAGEACCT=$(az storage account create \
    --resource-group "myResourceGroup" \
    --name "mystorageacct$RANDOM" \
    --location eastus \
    --sku Standard_LRS \
    --query "name" | tr -d '"')
```

## <a name="get-the-storage-key"></a>Depolama anahtarı alma

Bir depolama hesabı oluşturduğunuzda, hizmet kesintisi Döndürülmüş hesabı anahtarları çiftler halinde oluşturulur. İkinci anahtar çiftindeki için geçiş yaptığınızda, yeni bir anahtar çifti oluşturun. Her zaman en az bir kullanılmayan depolama hesabı anahtarı geçmek hazır olması için yeni depolama hesabı anahtarları her zaman çiftler halinde oluşturulur.

Depolama hesabı anahtarlarını kullanarak görüntülemek [az depolama hesabı anahtarları listesi](/cli/azure/storage/account/keys#list). Bu örnek anahtar 1 değerini depolar **depolama anahtarı** değişkeni.

```bash
STORAGEKEY=$(az storage account keys list \
    --resource-group "myResourceGroup" \
    --account-name $STORAGEACCT \
    --query "[0].value" | tr -d '"')
```

## <a name="create-a-file-share"></a>Dosya paylaşımı oluşturma

Depolama paylaşımı kullanarak dosya oluşturma [az depolama paylaşımı oluşturmak](/cli/azure/storage/share#create). 

Paylaşım adları tüm büyük küçük harf, rakam ve tek tire olması gerekir, ancak bir tire ile başlayamaz. Dosya paylaşımlarının ve dosyaların adlandırılması hakkında tüm ayrıntılara ulaşmak için bkz. [Paylaşımları, Dizinleri, Dosyaları ve Meta Verileri Adlandırma ve Bunlara Başvuruda Bulunma](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata)

Bu örnek adlı bir paylaşım oluşturur *paylaşımım* 10 Gib'den kota. 

```bash
az storage share create --name myshare \
    --quota 10 \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY
```

## <a name="create-a-mount-point"></a>Bir bağlama noktası oluştur

Azure dosya paylaşımını Linux bilgisayarınızda bağlama için olduğundan emin olun gerekir **CIFS yardımcı programları** yüklü paket. Yükleme yönergeleri için bkz: [Linux dağıtımınızı CIFS yardımcı programları paketini Yükle](../../storage/files/storage-how-to-use-files-linux.md#install-cifs-utils).

Azure dosyaları 445 numaralı TCP bağlantı noktası üzerinden iletişim kurar SMB protokolünü kullanır.  Azure dosya paylaşımınızı oluşturma ile ilgili sorunlar yaşıyorsanız, güvenlik duvarınızın TCP bağlantı noktası 445 engellemediğinden emin olun.


```bash
mkdir -p /mnt/MyAzureFileShare
```

## <a name="mount-the-share"></a>Paylaşımını bağlama

Yerel dizin Azure dosya paylaşımına bağlayın. 

```bash
sudo mount -t cifs //$STORAGEACCT.file.core.windows.net/myshare /mnt/MyAzureFileShare -o vers=3.0,username=$STORAGEACCT,password=$STORAGEKEY,dir_mode=0777,file_mode=0777,serverino
```



## <a name="persist-the-mount"></a>Bağlama Sürdür

Linux VM'i yeniden başlattığınızda takılı SMB paylaşımı kapatma sırasında kaldırılan değil. Önyükleme SMB paylaşımında yeniden bağlamaya yönelik Linux /etc/fstab bir satır ekleyin. Linux önyükleme işlemi sırasında bağlamak için gereken dosya sistemlerini listelemek için fstab dosyasını kullanır. SMB paylaşımı ekleme File storage paylaşımını Linux VM için kalıcı bağlı dosya sistemi olmasını sağlar. Bulut init kullandığınızda, SMB paylaşımı dosya depolama için yeni bir VM ekleme mümkündür.

```bash
//myaccountname.file.core.windows.net/mystorageshare /mnt/mymountpoint cifs vers=3.0,username=mystorageaccount,password=myStorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```
Üretim ortamlarında Artırılmış güvenlik için kimlik bilgilerinizi fstab dışında depolamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Bir Linux VM oluşturma sırasında özelleştirmek için bulut init kullanma](using-cloud-init.md)
- [Linux VM’ye disk ekleme](add-disk.md)
- [Azure CLI kullanarak bir Linux VM disklerde şifrele](encrypt-disks.md)

