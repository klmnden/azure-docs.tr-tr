---
title: Azure Blob Depolama, dosya sistemi olarak Linux üzerinde bağlama nasıl | Microsoft Docs
description: Linux üzerinde Azure Blob Depolama kapsayıcısına FUSE ile bağlama
services: storage
author: seguler
ms.service: storage
ms.topic: article
ms.date: 05/10/2018
ms.author: seguler
ms.openlocfilehash: 9964aa4d263e0b75eb59b4e1434a9b3f0aac6ea1
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39400444"
---
# <a name="how-to-mount-blob-storage-as-a-file-system-with-blobfuse"></a>BLOB Depolama blobfuse ile bir dosya sistemi olarak takmak nasıl

## <a name="overview"></a>Genel Bakış
[Blobfuse](https://github.com/Azure/azure-storage-fuse) Linux dosya sistemi üzerinden mevcut blok blobu verileriniz depolama hesabınızda erişmenize olanak sağlayan Azure Blob Depolama, sanal dosya sistemi sürücüsü içindir. Azure Blob Depolama, bir nesne depolama hizmetidir ve bu nedenle bir hiyerarşik ad alanı yok. Blobfuse ayırıcı olarak eğik çizgi-', '/' kullanarak sanal dizin şeması kullanarak bu ad alanı sağlar.  

Bu kılavuzda blobfuse kullanın ve Blob Depolama kapsayıcısı üzerinde Linux ve verilere bağlama gösterilmektedir. Blobfuse hakkında daha fazla bilgi edinmek için Ayrıntılar oku [blobfuse depo](https://github.com/Azure/azure-storage-fuse).

> [!WARNING]
> Yalnızca isteklerine çevirir Blobfuse % 100 POSIX uyumluluk garantilemez [Blob REST API'leri](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api). Örneğin, yeniden adlandırma işlemleri POSIX ancak içinde olmayan blobfuse atomiktir.
> Bir yerel dosya sistemi ve blobfuse arasındaki farklar tam listesi için ziyaret [blobfuse kaynak kodu deposu](https://github.com/azure/azure-storage-fuse).
> 

## <a name="install-blobfuse-on-linux"></a>Linux'ta blobfuse yükleme
Blobfuse ikili dosyaları, üzerinde kullanılabilir [Linux için Microsoft yazılım depoları](https://docs.microsoft.com/windows-server/administration/Linux-Package-Repository-for-Microsoft-Software). Blobfuse yükleyebilmek için bu depolar birini yapılandırın.

### <a name="configure-the-microsoft-package-repository"></a>Microsoft Paket Deposu yapılandırın
Yapılandırma [Microsoft ürünleri için Linux Paket Deposu](https://docs.microsoft.com/windows-server/administration/Linux-Package-Repository-for-Microsoft-Software).

Örneğin, bir Enterprise Linux 6 dağıtım:
```bash
sudo rpm -Uvh https://packages.microsoft.com/config/rhel/6/packages-microsoft-prod.rpm
```

Benzer şekilde, url değiştirme `.../rhel/7/...` bir Enterprise Linux 7 dağıtımına işaret edecek şekilde.

Ubuntu 14.04 başka örneğinde:
```bash
wget https://packages.microsoft.com/config/ubuntu/14.04/packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
sudo apt-get update
```

Benzer şekilde, url değiştirme `.../ubuntu/16.04/...` bir Ubuntu 16.04 dağıtımına işaret edecek şekilde.

### <a name="install-blobfuse"></a>Blobfuse yükleyin

Bir Ubuntu/Debian dağıtım:
```bash
sudo apt-get install blobfuse
```

Bir Enterprise Linux dağıtımı:
```bash
sudo yum install blobfuse
```

## <a name="prepare-for-mounting"></a>Bağlama için hazırlama
Blobfuse, arabellek ve önbelleğe yardımcı olan yerel benzer performans sunar, tüm açık dosyaları dosya sistemindeki bir geçici yol gerektirir. Bu geçici bir yol için en yüksek performanslı disk seçin veya bir ramdisk en iyi performans için kullanın. 

> [!NOTE]
> Blobfuse geçici yolu tüm açık dosya içeriğini depolar. Tüm açık dosyalar uyum sağlamak için yeterli alana sahip olduğundan emin olun. 
> 

### <a name="optional-use-a-ramdisk-for-the-temporary-path"></a>(İsteğe bağlı) Geçici yol için bir ramdisk kullanın
Aşağıdaki örnek, bir ramdisk 16 GB olarak blobfuse için bir dizin oluşturma oluşturur. Gereksinimlerinize göre boyutu seçin. Bu ramdisk için blobfuse sağlayan açık dosyaları en fazla 16 GB cinsinden boyutu. 
```bash
sudo mount -t tmpfs -o size=16g tmpfs /mnt/ramdisk
sudo mkdir /mnt/ramdisk/blobfusetmp
sudo chown <youruser> /mnt/ramdisk/blobfusetmp
```

### <a name="use-an-ssd-for-temporary-path"></a>Geçici yol için bir SSD kullanma
Azure'da blobfuse için düşük gecikmeli bir arabelleği sağlamak için sanal makinelerinizde geçici diskler (SSD) kullanabilirsiniz. Ubuntu dağıtımları bu kısa ömürlü disk üzerinde takılı ' / mnt' üzerinde oluşturulmuş ise ' / mnt/kaynak /', Red Hat, CentOS dağıtımları.

Geçici yol, kullanıcı erişiminin emin olun:
```bash
sudo mkdir /mnt/resource/blobfusetmp
sudo chown <youruser> /mnt/resource/blobfusetmp
```

### <a name="configure-your-storage-account-credentials"></a>Depolama hesabı kimlik bilgilerinizi yapılandırın
Aşağıdaki biçimde bir metin dosyasında saklanan kimlik bilgilerinizi Blobfuse gerektirir: 

```
accountName myaccount
accountKey myaccesskey==
containerName mycontainer
```

Bu dosyayı oluşturduktan sonra başka bir kullanıcı okuyabilmesi için erişimi kısıtlamak emin olun.
```bash
chmod 700 fuse_connection.cfg
```

### <a name="create-an-empty-directory-for-mounting"></a>Bağlama için boş bir dizin oluşturun
```bash
mkdir ~/mycontainer
```

## <a name="mount"></a>Bağlama

> [!NOTE]
> Bağlama seçeneklerinin tam listesi için kontrol [blobfuse depo](https://github.com/Azure/azure-storage-fuse#mount-options).  
> 

Sırayla bağlama blobfuse kullanıcı ile aşağıdaki komutu çalıştırın. Bu komut belirtilen kapsayıcı bağlar ' / path/to/fuse_connection.cfg' konumu üzerine ' / mycontainer'.

```bash
blobfuse ~/mycontainer --tmp-path=/mnt/resource/blobfusetmp  --config-file=/path/to/fuse_connection.cfg -o attr_timeout=240 -o entry_timeout=240 -o negative_timeout=120
```

Şimdi, normal dosya sistemi API'leri üzerinden, blok blobları için erişimi olmalıdır. Bağlı dizini yalnızca erişim güvenliğini sağlar, bağlama kullanıcı tarafından erişilebilir unutmayın. Tüm kullanıcılar erişime izin vermek istiyorsanız, seçeneği bağlayabilir ```-o allow_other```. 

```bash
cd ~/mycontainer
mkdir test
echo "hello world" > test/blob.txt
```

## <a name="next-steps"></a>Sonraki adımlar

* [Blobfuse giriş sayfası](https://github.com/Azure/azure-storage-fuse#blobfuse)
* [Rapor blobfuse sorunları](https://github.com/Azure/azure-storage-fuse/issues) 

