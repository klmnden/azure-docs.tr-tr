---
title: Azure Blob Depolama, dosya sistemi olarak Linux üzerinde bağlama nasıl | Microsoft Docs
description: Linux üzerinde Azure Blob Depolama kapsayıcısına FUSE ile bağlama
services: storage
author: seguler
ms.service: storage
ms.topic: article
ms.date: 2/1/2019
ms.author: seguler
ms.openlocfilehash: eadf52afd115eb1cb642082cea4b9f338bd44914
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60392492"
---
# <a name="how-to-mount-blob-storage-as-a-file-system-with-blobfuse"></a>BLOB Depolama blobfuse ile bir dosya sistemi olarak takmak nasıl

## <a name="overview"></a>Genel Bakış
[Blobfuse](https://github.com/Azure/azure-storage-fuse) bir Azure Blob Depolama için sanal dosya sistemi sürücüsüdür. Blobfuse Linux dosya sistemi üzerinden mevcut blok blobu verileriniz depolama hesabınızda erişmenize olanak sağlar. Azure Blob Depolama, bir nesne depolama hizmetidir ve hiyerarşik ad alanı yok. Blobfuse sanal dizin şeması, ayırıcı olarak eğik ile '/' kullanarak bu ad alanı sağlar.  

Bu kılavuzda blobfuse kullanın ve Blob Depolama kapsayıcısı üzerinde Linux ve verilere bağlama gösterilmektedir. Blobfuse hakkında daha fazla bilgi edinmek için Ayrıntılar oku [blobfuse depo](https://github.com/Azure/azure-storage-fuse).

> [!WARNING]
> Yalnızca isteklerine çevirir, Blobfuse % 100 POSIX uyumluluğu garanti etmez [Blob REST API'leri](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api). Örneğin, yeniden adlandırma işlemleri POSIX ancak içinde olmayan blobfuse atomiktir.
> Bir yerel dosya sistemi ve blobfuse arasındaki farklar tam listesi için ziyaret [blobfuse kaynak kodu deposu](https://github.com/azure/azure-storage-fuse).
> 

## <a name="install-blobfuse-on-linux"></a>Linux'ta blobfuse yükleme
Blobfuse ikili dosyaları, üzerinde kullanılabilir [Linux için Microsoft yazılım depoları](https://docs.microsoft.com/windows-server/administration/Linux-Package-Repository-for-Microsoft-Software) Ubuntu ve RHEL'de dağıtımlar için. Bu dağıtımlarında blobfuse yüklemek için listenin depolarından birini yapılandırın. Ayrıca, kaynak kodu aşağıdaki ikili dosyaları oluşturabilirsiniz [Azure Depolama'ya yükleme adımlarını](https://github.com/Azure/azure-storage-fuse/wiki/1.-Installation#option-2---build-from-source) varsa hiçbir ikili dosyaları, dağıtım için kullanılabilir.

Blobfuse Ubuntu 14.04 ve 16.04 18.04 yüklemeyi destekler. Dağıtılan bu sürümlerden biri olduğundan emin olmak için şu komutu çalıştırın:
```
lsb_release -a
```

### <a name="configure-the-microsoft-package-repository"></a>Microsoft Paket Deposu yapılandırın
Yapılandırma [Microsoft ürünleri için Linux Paket Deposu](https://docs.microsoft.com/windows-server/administration/Linux-Package-Repository-for-Microsoft-Software).

Örneğin, bir Enterprise Linux 6 dağıtım:
```bash
sudo rpm -Uvh https://packages.microsoft.com/config/rhel/6/packages-microsoft-prod.rpm
```

Benzer şekilde, URL değiştirme `.../rhel/7/...` bir Enterprise Linux 7 dağıtımına işaret edecek şekilde.

Başka bir örnek bir Ubuntu 14.04 dağıtım:
```bash
wget https://packages.microsoft.com/config/ubuntu/14.04/packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
sudo apt-get update
```

Benzer şekilde, URL değiştirme `.../ubuntu/16.04/...` veya `.../ubuntu/18.04/...` başka bir Ubuntu sürümüne başvurmak için.

### <a name="install-blobfuse"></a>Blobfuse yükleyin

Bir Ubuntu/Debian dağıtımı:
```bash
sudo apt-get install blobfuse
```

Bir Enterprise Linux dağıtımı:
```bash    
sudo yum install blobfuse
```

## <a name="prepare-for-mounting"></a>Bağlama için hazırlama
Blobfuse, arabellek ve önbelleğe açık dosyaları dosya sistemindeki bir geçici yol gerektirerek yerel benzer performans sağlar. Bu geçici bir yol için en yüksek performanslı disk seçin veya bir ramdisk en iyi performans için kullanın. 

> [!NOTE]
> Blobfuse geçici yolu tüm açık dosya içeriğini depolar. Tüm açık dosyaları yerleştirmek için yeterli alanı olduğundan emin olun. 
> 

### <a name="optional-use-a-ramdisk-for-the-temporary-path"></a>(İsteğe bağlı) Geçici yol için bir ramdisk kullanın
Aşağıdaki örnek, bir ramdisk 16 GB ve blobfuse için bir dizin oluşturur. Gereksinimlerinize göre boyutu seçin. Bu ramdisk için blobfuse sağlayan açık dosyaları en fazla 16 GB cinsinden boyutu. 
```bash
sudo mount -t tmpfs -o size=16g tmpfs /mnt/ramdisk
sudo mkdir /mnt/ramdisk/blobfusetmp
sudo chown <youruser> /mnt/ramdisk/blobfusetmp
```

### <a name="use-an-ssd-as-a-temporary-path"></a>Geçici bir yolu olarak bir SSD kullanma
Azure'da blobfuse için düşük gecikmeli bir arabelleği sağlamak için sanal makinelerinizde geçici diskler (SSD) kullanabilirsiniz. Ubuntu dağıtımları bu kısa ömürlü disk üzerinde takılı ' / mnt'. Red Hat ve CentOS dağıtımları, disk üzerinde takılı ' / mnt/kaynak /'.

Geçici yol, kullanıcı erişiminin emin olun:
```bash
sudo mkdir /mnt/resource/blobfusetmp -p
sudo chown <youruser> /mnt/resource/blobfusetmp
```

### <a name="configure-your-storage-account-credentials"></a>Depolama hesabı kimlik bilgilerinizi yapılandırın
Aşağıdaki biçimde bir metin dosyasında saklanan kimlik bilgilerinizi Blobfuse gerektirir: 

```
accountName myaccount
accountKey storageaccesskey
containerName mycontainer
```
`accountName` Depolama hesabınızın - tam URL önekidir.

Bu dosya kullanarak oluşturun:

```
touch ~/fuse_connection.cfg
```

Oluşturulur ve bu dosyayı düzenleyen sonra başka hiçbir kullanıcı okuyabilmesi için erişimi kısıtlamak emin olun.
```bash
chmod 600 fuse_connection.cfg
```

> [!NOTE]
> Windows üzerindeki yapılandırma dosyasını oluşturduysanız çalıştırıldığından emin olun `dos2unix` temizleyin ve dosya UNIX biçimine dönüştürün. 
>

### <a name="create-an-empty-directory-for-mounting"></a>Bağlama için boş bir dizin oluşturun
```bash
mkdir ~/mycontainer
```

## <a name="mount"></a>Bağlama

> [!NOTE]
> Bağlama seçeneklerinin tam listesi için kontrol [blobfuse depo](https://github.com/Azure/azure-storage-fuse#mount-options).  
> 

Bağlama blobfuse için kullanıcı ile aşağıdaki komutu çalıştırın. Bu komut belirtilen kapsayıcı bağlar ' / path/to/fuse_connection.cfg' konumu üzerine ' / mycontainer'.

```bash
sudo blobfuse ~/mycontainer --tmp-path=/mnt/resource/blobfusetmp  --config-file=/path/to/fuse_connection.cfg -o attr_timeout=240 -o entry_timeout=240 -o negative_timeout=120
```

Şimdi, normal dosya sistemi API'leri üzerinden, blok blobları için erişimi olmalıdır. Dizin bağlar, erişim güvenliğini sağlar. varsayılan olarak erişebilen tek kişi kullanıcıdır. Tüm kullanıcılar erişime izin vermek için seçeneği bağlayabilir ```-o allow_other```. 

```bash
cd ~/mycontainer
mkdir test
echo "hello world" > test/blob.txt
```

## <a name="next-steps"></a>Sonraki adımlar

* [Blobfuse giriş sayfası](https://github.com/Azure/azure-storage-fuse#blobfuse)
* [Rapor blobfuse sorunları](https://github.com/Azure/azure-storage-fuse/issues) 

