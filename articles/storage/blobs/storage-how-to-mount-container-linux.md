---
title: Dosya sistemi olarak Linux üzerinde Azure Blob Depolama nasıl | Microsoft Docs
description: Linux üzerinde bir Azure Blob Depolama kapsayıcısını SİGORTASI ile bağlama
services: storage
documentationcenter: linux
author: seguler
manager: jahogg
ms.service: storage
ms.devlang: bash
ms.topic: article
ms.date: 05/10/2018
ms.author: seguler
ms.openlocfilehash: 1098eef15b559c30ef436d8e13bbe02bddb78649
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
ms.locfileid: "34072101"
---
# <a name="how-to-mount-blob-storage-as-a-file-system-with-blobfuse"></a>Bir dosya sistemi ile blobfuse olarak BLOB storage nasıl

## <a name="overview"></a>Genel Bakış
[Blobfuse](https://github.com/Azure/azure-storage-fuse) Linux dosya sistemi aracılığıyla depolama hesabınızdaki varolan blok blobu verilerinize erişmesine izin veren Azure Blob Storage sanal dosya sistemi sürücüsü içindir. Azure Blob Depolama bir nesne depolama hizmeti ve bu nedenle hiyerarşik bir ad alanı yok. Blobfuse bir ayırıcısı olarak '/' eğik çizgi-birini kullanarak sanal dizin şeması kullanarak bu ad alanı sağlar.  

Bu kılavuz blobfuse kullanın ve Linux ve erişim verilerini bir Blob Depolama kapsayıcısını bağlama gösterilmektedir. Blobfuse hakkında daha fazla bilgi için Ayrıntılar okuma [blobfuse depo](https://github.com/Azure/azure-storage-fuse).

> [!WARNING]
> Blobfuse garanti etmez % 100 POSIX uyumluluğu yalnızca isteklerine çevirir [Blob REST API'leri](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api). Örneğin, yeniden adlandırma işlemleri POSIX, ancak içinde değil blobfuse atomik.
> Bir yerel dosya sistemi ve blobfuse arasındaki farklar tam listesi için ziyaret [blobfuse kaynak kodu deposu](https://github.com/azure/azure-storage-fuse).
> 

## <a name="install-blobfuse-on-linux"></a>Linux'ta blobfuse yüklemek
Blobfuse ikili dosyaları bulunur [Linux için Microsoft yazılımı depoları](https://docs.microsoft.com/windows-server/administration/Linux-Package-Repository-for-Microsoft-Software). Blobfuse yükleyebilmek için bu depoları birini yapılandırın.

### <a name="configure-the-microsoft-package-repository"></a>Microsoft Paket Deposu yapılandırın
Yapılandırma [Microsoft ürünleri için Linux Paket Deposu](https://docs.microsoft.com/windows-server/administration/Linux-Package-Repository-for-Microsoft-Software).

Örnek olarak, bir Enterprise Linux 6 dağıtım:
```bash
sudo rpm -Uvh https://packages.microsoft.com/config/rhel/6/packages-microsoft-prod.rpm
```

Benzer şekilde, URL'ye değiştirin `.../rhel/7/...` bir Enterprise Linux 7 dağıtım noktasına için.

Ubuntu 14.04 başka bir örneğidir:
```bash
wget https://packages.microsoft.com/config/ubuntu/14.04/packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
sudo apt-get update
```

Benzer şekilde, URL'ye değiştirin `.../ubuntu/16.04/...` bir Ubuntu 16.04 dağıtım noktasına için.

### <a name="install-blobfuse"></a>Blobfuse yükleyin

Ubuntu/Debian dağıtım noktasında:
```bash
sudo apt-get install blobfuse
```

Bir Enterprise Linux dağıtımı:
```bash
sudo yum install blobfuse
```

## <a name="prepare-for-mounting"></a>Bağlama için hazırlama
Blobfuse geçici bir yolu, arabellek ve önbelleğe yardımcı olan yerel benzeri performans sunar tüm açık dosyalar için dosya sistemi gerektirir. Bu geçici yol için çoğu kullanıcı disk seçin veya bir ramdisk en iyi performans için kullanın. 

> [!NOTE]
> Blobfuse tüm açık dosya içeriklerini geçici yolunda depolar. Tüm açık dosyalar karşılamak üzere yeterli boş alan bulunduğundan emin olun. 
> 

### <a name="optional-use-a-ramdisk-for-the-temporary-path"></a>(İsteğe bağlı) Bir ramdisk geçici yolu için kullanın
Aşağıdaki örnek, 16 GB yanı blobfuse için bir dizin oluşturma ramdisk oluşturur. Gereksinimlerinize bağlı boyutu seçin. Bu ramdisk için blobfuse sağlayan açık dosyaları en fazla 16 GB cinsinden boyutu. 
```bash
sudo mount -t tmpfs -o size=16g tmpfs /mnt/ramdisk
sudo mkdir /mnt/ramdisk/blobfusetmp
sudo chown <youruser> /mnt/ramdisk/blobfusetmp
```

### <a name="use-an-ssd-for-temporary-path"></a>Geçici yol için bir SSD kullanın
Azure'da, düşük gecikme süreli arabellek için blobfuse sağlamak için Vm'leriniz kısa ömürlü disklerin (SSD) kullanılabilir kullanabilir. Ubuntu dağıtımları bu kısa ömürlü disk üzerinde takılı ' / mnt' üzerinde oluşturulmuş ancak ' / mnt/kaynak /' RedHat ve CentOS dağıtımları içinde.

Kullanıcı geçici yoluna erişimi olduğundan emin olun:
```bash
sudo mkdir /mnt/resource/blobfusetmp
sudo chown <youruser> /mnt/resource/blobfusetmp
```

### <a name="configure-your-storage-account-credentials"></a>Depolama hesabı kimlik bilgilerinizi yapılandırın
Blobfuse aşağıdaki biçimde bir metin dosyasındaki depolanması için kimlik bilgilerinizi gerektirir: 

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

## <a name="mount"></a>bağlama

> [!NOTE]
> Bağlama seçeneklerinin tam listesi için kontrol [blobfuse depo](https://github.com/Azure/azure-storage-fuse#mount-options).  
> 

Bağlama blobfuse için sırayla, kullanıcı aşağıdaki komutu çalıştırın. Bu komut belirtilen kapsayıcı bağlar ' / path/to/fuse_connection.cfg' konumu üzerine ' / mycontainer'.

```bash
blobfuse ~/mycontainer --tmp-path=/mnt/resource/blobfusetmp  --config-file=/path/to/fuse_connection.cfg -o attr_timeout=240 -o entry_timeout=240 -o negative_timeout=120
```

Şimdi, blok blobları normal dosya sistemi API'leri aracılığıyla erişiminiz. Takılı dizin yalnızca hangi erişim güvenliğini sağlar, takma kullanıcı tarafından erişilecek unutmayın. Tüm kullanıcıların erişmesine izin vermek istiyorsanız, seçeneği bağlayabilir ```-o allow_other```. 

```bash
cd ~/mycontainer
mkdir test
echo "hello world" > test/blob.txt
```

## <a name="next-steps"></a>Sonraki adımlar

* [Blobfuse giriş sayfası](https://github.com/Azure/azure-storage-fuse#blobfuse)
* [Rapor blobfuse sorunları](https://github.com/Azure/azure-storage-fuse/issues) 

