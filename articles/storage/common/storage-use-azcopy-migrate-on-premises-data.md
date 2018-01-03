---
title: "Azure Storage ile AzCopy için şirket içi veri geçirme | Microsoft Docs"
description: "Veri geçirmeyi veya için veya blob, tablo ve dosya içeriği veri kopyalamak için AzCopy kullanın. Kolayca verileri yerel depolama hesabınızdan Azure depolama alanına geçirin."
services: storage
author: ruthogunnnaike
manager: jeconnoc
ms.service: storage
ms.tgt_pltfrm: na
ms.devlang: azcopy
ms.topic: tutorial
ms.date: 12/14/2017
ms.author: v-ruogun
ms.openlocfilehash: f2c0b80248ef706394b3f84df4c3da26fb79026a
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2018
---
#  <a name="migrate-on-premises-data-to-cloud-storage-with-azcopy"></a>Bulut depolama AzCopy ile şirket içi veri geçirme

AzCopy, en iyi performans için tasarlanmış basit komutlarını kullanarak denetleyicisinden Microsoft Azure Blob, dosya ve tablo depolama, veri kopyalamak için tasarlanmış bir komut satırı yardımcı programıdır. Veri depolama hesapları arasında veya bir dosya sistemi ve depolama hesabı arasında kopyalayabilirsiniz.  

İndirebilirsiniz AzCopy iki sürümü vardır:

* [Linux üzerinde AzCopy](storage-use-azcopy.md) .NET Core POSIX stili komut satırı seçenekleri sunan Linux platformlar hedefler Framework ile oluşturulmuş. 
* [AzCopy Windows](../storage-use-azcopy.md) .NET Framework ile oluşturulan ve Windows stili komut satırı seçenekleri sunar. 
 
Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Depolama hesabı oluşturma 
> * Tüm verilerinizi karşıya yüklemek için AzCopy kullanma
> * Test amacıyla verileri değiştirme
> * Karşıya yüklemek için yeni dosyaları belirlemek üzere zamanlanmış bir görev veya cron işi oluşturma

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için: 

* Üzerinde en güncel AzCopy sürümünü karşıdan [Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux#download-and-install-azcopy) veya [Windows](http://aka.ms/downloadazcopy). 

## <a name="log-in-to-the-azure-portalhttpsportalazurecom"></a>[Azure Portal](https://portal.azure.com/)’da oturum açın.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

>[!NOTE]
>Yerel depolama alanınızın bir ikincil bölge ' blobları indirmek ve tersi yönde ayarlamak isteyip istemediğinizi **çoğaltma** için **okuma-access-coğrafi olarak yedekli depolama**. Bu seçeneğin belirlenmesi oluşturur bir [coğrafi olarak yedekli depolama](https://docs.microsoft.com/azure/storage/common/storage-redundancy#geo-redundant-storage) hesabı. 
>
>

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Bloblar her zaman bir kapsayıcıya yüklenir. Kapsayıcıları dosyalarınızı bilgisayarınızda klasörlerde düzenleme gibi gruplar BLOB düzenlemek sağlar. 

Bir kapsayıcı oluşturmak için aşağıdaki adımları izleyin:

1. Seçin **depolama hesapları** düğmesini ana sayfayı ve oluşturduğunuz depolama hesabını seçin.
2. Seçin **BLOB'lar** altında **Hizmetleri**seçeneğini belirleyip **kapsayıcı**. 

![Bir kapsayıcı oluşturma](media/storage-azcopy-migrate-on-premises-data/CreateContainer.png)
 
Kapsayıcı adları bir harf veya rakamla başlamalıdır ve yalnızca harf, rakam ve kısa çizgi (-) karakterini içerebilir. Kapsayıcıları ve blobları adlandırma kuralları hakkında daha fazla bilgi için bkz. [Kapsayıcıları, blobları ve meta verileri adlandırma ve bunlara başvurma](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

## <a name="upload-all-files-in-a-folder-to-blob-storage"></a>Blob depolama alanına bir klasördeki tüm dosyaları karşıya yükleme

AzCopy üzerinde Blob depolama alanına bir klasördeki tüm dosyaları karşıya yüklemek için kullanabileceğiniz [Windows](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#upload-blobs-to-blob-storage) veya [Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux#blob-download). Bir klasördeki tüm BLOB'lar karşıya yüklemek için aşağıdaki AzCopy komutu girin:

# <a name="linuxtablinux"></a>[Linux](#tab/linux)
    azcopy \
        --source /mnt/myfolder \
        --destination https://myaccount.blob.core.windows.net/mycontainer \
        --dest-key <key> \
        --recursive

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey: key /S
---

Değiştir  **\<anahtar\>**  hesap anahtarınız ile. Azure portalında seçerek hesap anahtarınızı alabilir **erişim anahtarları** depolama hesabınızdaki ayarlar altında. Bir anahtar seçin ve AzCopy komut yapıştırın. Belirtilen hedef kapsayıcı mevcut değilse, AzCopy oluşturur ve dosyayı içine yükler. Kaynak yolu veri dizininize güncelleştirmek ve değiştirme **myaccount** depolama hesabınızın adını hedef URL'yi.

Belirtin `--recursive` ve `/S` , Linux ve Windows'da sırasıyla belirtilen dizin içeriğini Blob Depolama yinelemeli olarak karşıya yüklemek için Seçenekler. AzCopy şu seçeneklerden birini çalıştırdığınızda, tüm alt klasörleri ve bunların dosyaları da yüklenir.

## <a name="upload-modified-files-to-blob-storage"></a>Blob depolama alanına değiştirilen dosyaları karşıya yükleme
AzCopy için kullanabileceğiniz [dosyaları karşıya yükleme](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#other-azcopy-features) son değiştiren bunların zamanı temel alınarak. Bu denemek için değiştirin veya testi amacı için kaynak dizininizde yeni dosyalar oluşturamaz. Yalnızca güncelleştirilmiş veya yeni dosyaları karşıya yüklemek için ekleme `--exclude-older`veya `/XO` parametresi AzCopy Linux ve Windows komut sırasıyla.

Yalnızca hedef yok, kaynak kaynak kopyalamak istiyorsanız, her ikisini de belirtin `--exclude-older` ve `--exclude-newer` veya `/XO` ve `/XN` komut parametreleri AzCopy Linux ve Windows sırasıyla. AzCopy yalnızca kendi zaman damgasını göre güncelleştirilmiş verileri karşıya yükleme.
 
# <a name="linuxtablinux"></a>[Linux](#tab/linux)
    azcopy \
    --source /mnt/myfolder \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --recursive \
    --exclude-older

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey: key /S /XO
---

## <a name="create-a-scheduled-task-or-cron-job"></a>Zamanlanmış bir görev veya cron iş oluşturma 
Zamanlanmış bir oluşturabilirsiniz AzCopy komut dosyası çalıştıran görev/cron işi tanımlar ve yeni şirket içi verileri belirli bir zaman aralığında bulut depolama alanına yükler. 

AzCopy komut bir metin düzenleyicisine kopyalayın. AzCopy komut parametre değerlerini uygun değerlere güncelleştirin. Dosyayı Farklı Kaydet `script.sh` veya `script.bat` AzCopy Linux ve Windows için sırasıyla. 

# <a name="linuxtablinux"></a>[Linux](#tab/linux)
    azcopy --source /mnt/myfiles --destination https://myaccount.blob.core.windows.net/mycontainer --dest-key <key> --recursive --exclude-older --exclude-newer --verbose >> Path/to/logfolder/`date +\%Y\%m\%d\%H\%M\%S`-cron.log

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
    cd C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy
    AzCopy /Source: C:\myfolder  /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey: key /V /XO /XN >C:\Path\to\logfolder\azcopy%date:~-4,4%%date:~-7,2%%date:~-10,2%%time:~-11,2%%time:~-8,2%%time:~-5,2%.log
---

AzCopy çalıştırıldığında ile ayrıntılı `--verbose` (Linux) ve `/V` (Windows) seçeneği ve çıktıyı bir günlük dosyasına yönlendirilir. 

Bu öğreticide [Schtasks](https://msdn.microsoft.com/library/windows/desktop/bb736357(v=vs.85).aspx) Windows, zamanlanmış bir görev oluşturmak için kullanılır ve [Crontab](http://crontab.org/) komutu Linux'ta cron işi oluşturmak için kullanılır. 
 **Schtasks** oluşturmak, silmek, sorgu, değiştirme, çalıştırmak ve yerel veya uzak bilgisayarda zamanlanmış görevler sonlandırmak için yönetici sağlar. **Cron** Linux ve UNIX kullanıcıların belirtilen tarihteki komutlarını veya komut dosyalarını çalıştırmak ve kullanarak istediğiniz zaman sağlayan [cron ifade](https://en.wikipedia.org/wiki/Cron#CRON_expression)


# <a name="linuxtablinux"></a>[Linux](#tab/linux)
**Linux'ta cron işi oluşturmak için**, bir terminalde aşağıdaki komutu girin. 

```bash
crontab -e 
*/5 * * * * sh /path/to/script.sh 
```

Cron deyim belirleyerek `*/5 * * * * ` komut Kabuk betiği gösterir `script.sh` beş dakikada yürütülecek. Komut dosyası, belirli bir saatte günlük, aylık veya yıllık çalıştırmak için zamanlanabilir. Bkz: [cron ifadeleri](https://en.wikipedia.org/wiki/Cron#CRON_expression) tarih ve saat iş yürütme için ayarlama hakkında daha fazla bilgi edinmek için. 

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
**Windows üzerinde zamanlanmış bir görev oluşturmak için**, komut istemi veya PowerShell aşağıdaki komutu girin.

```cmd 
schtasks /CREATE /SC minute /MO 5 /TN "AzCopy Script" /TR C:\Users\username\Documents\script.bat
```

Komut kullanır `/SC` minute bir zamanlama belirtmek için parametre ve `/MO` beş dakikada bir aralık belirtmek için parametre. `/TN` Parametresi görev adı belirtmek için kullanılır ve `/TR` yolunu belirtmek için parametreyi `script.bat` dosya. Ziyaret [schtasks](https://technet.microsoft.com/library/cc772785(v=ws.10).aspx#BKMK_minutes) Windows zamanlanmış görev oluşturma hakkında daha fazla bilgi edinmek için.

---
 
Yürütür doğru iş zamanlanmış görev/cron doğrulamak için dizininizde yeni dosyalar oluşturma `myfolder`. Depolama hesabınıza yeni dosyaları karşıya yüklenen onaylamak için beş dakika bekleyin. Zamanlanmış görev/cron işinin çıkış günlükleri görüntülemek için günlük dizinine gidin. 

Ziyaret [taşıma şirket içi veri](https://docs.microsoft.com/azure/storage/common/storage-moving-data?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) şirket içi verileri Azure depolama ve tersi yönde taşımak için yolları hakkında daha fazla bilgi edinin.  

## <a name="next-steps"></a>Sonraki adımlar
Azure Storage ve AzCopy hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

### <a name="azure-storage-documentation"></a>Azure Storage belgeleri:
* [Azure Storage'a giriş](../storage-introduction.md)
* [Windows AzCopy ile veri aktarımı](storage-use-azcopy.md) 
* [Linux'ta AzCopy ile veri aktarımı](storage-use-azcopy-linux.md) 
* [Depolama hesabı oluşturma](../storage-create-storage-account.md)
* [BLOB Depolama Gezgini ile yönetme](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs)  






 
 

