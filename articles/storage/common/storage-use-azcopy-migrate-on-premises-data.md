---
title: "AzCopy kullanarak şirket içi verileri Azure depolama alanına geçirmek | Microsoft Docs"
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
ms.openlocfilehash: 3dbfb935ac0b134e127a5dccb7bc76716c688e8a
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2018
---
#  <a name="migrate-on-premises-data-to-cloud-storage-by-using-azcopy"></a>Şirket içi veri depolama AzCopy kullanarak buluta geçişi

AzCopy, veri ya da Azure Blob storage, Azure dosyaları ve Azure Table storage basit komutlarını kullanarak kopyalamak için bir komut satırı aracıdır. Komutlar, en iyi performans için tasarlanmıştır. Veri depolama hesapları arasında veya bir dosya sistemi ve depolama hesabı arasında kopyalayabilirsiniz.  

AzCopy iki sürümü yükleyebilirsiniz:

* [Linux üzerinde AzCopy](storage-use-azcopy.md) .NET Core Framework ile oluşturulmuş. Linux platformlar POSIX tipi komut satırı seçenekleri sunarak hedefler. 
* [AzCopy Windows](../storage-use-azcopy.md) .NET Framework ile oluşturulmuş. Windows stili komut satırı seçenekleri sunar. 
 
Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir depolama hesabı oluşturun. 
> * AzCopy tüm verilerinizi yüklemek için kullanın.
> * Test amaçları için verileri değiştirin.
> * Karşıya yüklemek için yeni dosyaları belirlemek üzere zamanlanmış bir görev veya cron iş oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için üzerinde en güncel AzCopy sürümünü karşıdan [Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux#download-and-install-azcopy) veya [Windows](http://aka.ms/downloadazcopy). 

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

>[!NOTE]
>Yerel depolama alanınızın bir ikincil bölge ' blobları indirmek ve tersi yönde ayarlamak isteyip istemediğinizi **çoğaltma** için **okuma-access-coğrafi olarak yedekli depolama**. Bu seçeneğin belirlenmesi oluşturur bir [coğrafi olarak yedekli depolama](https://docs.microsoft.com/azure/storage/common/storage-redundancy#geo-redundant-storage) hesabı. 
>
>

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Bloblar her zaman bir kapsayıcıya yüklenir. Kapsayıcıları dosyalarınızı bilgisayarınızda klasörlerde düzenleme gibi gruplar BLOB düzenlemek için kullanabilirsiniz. 

Bir kapsayıcı oluşturmak için aşağıdaki adımları izleyin:

1. Seçin **depolama hesapları** düğmesini ana sayfayı ve oluşturduğunuz depolama hesabını seçin.
2. Seçin **BLOB'lar** altında **Hizmetleri**ve ardından **kapsayıcı**. 

   ![Bir kapsayıcı oluşturma](media/storage-azcopy-migrate-on-premises-data/CreateContainer.png)
 
Kapsayıcı adları bir harf veya sayı ile başlamalıdır. Bunlar yalnızca harf, rakam ve kısa çizgi karakteri (-) içerebilir. Kapsayıcıları ve blobları adlandırma kuralları hakkında daha fazla bilgi için bkz. [Kapsayıcıları, blobları ve meta verileri adlandırma ve bunlara başvurma](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

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

Değiştir `<key>` ve `key` hesap anahtarınız ile. Azure portalında seçerek hesap anahtarınızı alabilir **erişim anahtarları** altında **ayarları** depolama hesabınızdaki. Bir anahtar seçin ve AzCopy komut yapıştırın. Belirtilen hedef kapsayıcı mevcut değilse, AzCopy oluşturur ve dosyayı içine yükler. Kaynak yolu veri dizininize güncelleştirmek ve değiştirme **myaccount** depolama hesabı adı ile hedef URL.

Belirtilen dizin içeriğini Blob Depolama yinelemeli olarak yüklemeye belirtin `--recursive` (Linux) veya `/S` (Windows) seçeneği. AzCopy şu seçeneklerden birini çalıştırdığınızda, tüm alt klasörleri ve bunların dosyaları da yüklenir.

## <a name="upload-modified-files-to-blob-storage"></a>Blob depolama alanına değiştirilmiş dosyaları karşıya yükleme
AzCopy için kullanabileceğiniz [dosyaları karşıya yükleme](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#other-azcopy-features) son değiştiren bunların zamanı temel alınarak. Bu denemek için değiştirin veya test amaçları için kaynak dizininizde yeni dosyalar oluşturamaz. Yalnızca güncelleştirilmiş veya yeni dosyaları karşıya yüklemek için ekleme `--exclude-older` (Linux) veya `/XO` AzCopy komutuna (Windows) parametre.

Yalnızca hedef yok, kaynak kaynak kopyalamak istiyorsanız, her ikisini de belirtin `--exclude-older` ve `--exclude-newer` (Linux) veya `/XO` ve `/XN` (Windows) parametrelerinde AzCopy komutu. AzCopy, zaman damgası dayalı yalnızca güncelleştirilmiş verileri karşıya yükleme.
 
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
Zamanlanmış görev ya da bir AzCopy komut dosyasını çalıştırır cron iş oluşturabilirsiniz. Komut dosyasını tanımlar ve yeni şirket içi verileri belirli bir zaman aralığında bulut depolama alanına yükler. 

AzCopy komut bir metin düzenleyicisine kopyalayın. AzCopy komut parametre değerlerini uygun değerlere güncelleştirin. Dosyayı Farklı Kaydet `script.sh` (Linux) veya `script.bat` (Windows) AzCopy için. 

# <a name="linuxtablinux"></a>[Linux](#tab/linux)
    azcopy --source /mnt/myfiles --destination https://myaccount.blob.core.windows.net/mycontainer --dest-key <key> --recursive --exclude-older --exclude-newer --verbose >> Path/to/logfolder/`date +\%Y\%m\%d\%H\%M\%S`-cron.log

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
    cd C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy
    AzCopy /Source: C:\myfolder  /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey: key /V /XO /XN >C:\Path\to\logfolder\azcopy%date:~-4,4%%date:~-7,2%%date:~-10,2%%time:~-11,2%%time:~-8,2%%time:~-5,2%.log
---

AzCopy verbose ile çalıştırın `--verbose` (Linux) veya `/V` (Windows) seçeneği. Çıkış, bir günlük dosyasına yönlendirilir. 

Bu öğreticide [Schtasks](https://msdn.microsoft.com/library/windows/desktop/bb736357(v=vs.85).aspx) Windows zamanlanmış bir görev oluşturmak için kullanılır. [Crontab](http://crontab.org/) komutu Linux'ta cron işi oluşturmak için kullanılır. 
 **Schtasks** oluşturmak, silmek, sorgu, değiştirme, çalıştırmak ve bir yerel veya uzak bilgisayarda zamanlanmış görevler sonlandırmak için yönetici sağlar. **Cron** komutlarını veya komut dosyalarını bir belirtilen tarih ve saatte kullanarak çalıştırmak Linux ve UNIX kullanıcıların sağlayan [cron ifadeleri](https://en.wikipedia.org/wiki/Cron#CRON_expression).


# <a name="linuxtablinux"></a>[Linux](#tab/linux)
Linux'ta cron işi oluşturmak için bir terminalde aşağıdaki komutu girin: 

```bash
crontab -e 
*/5 * * * * sh /path/to/script.sh 
```

Cron deyim belirleyerek `*/5 * * * * ` komutta belirten Kabuk betiği `script.sh` beş dakikada çalıştırmanız gerekir. Belirli bir süre boyunca her gün, aylık veya yıllık çalıştırmak için komut dosyasını zamanlayabilirsiniz. Tarih ve saat iş yürütme için ayarlama hakkında daha fazla bilgi edinmek için [cron ifadeleri](https://en.wikipedia.org/wiki/Cron#CRON_expression). 

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
Windows üzerinde zamanlanmış bir görev oluşturmak için PowerShell veya bir komut isteminde aşağıdaki komutu girin:

```cmd 
schtasks /CREATE /SC minute /MO 5 /TN "AzCopy Script" /TR C:\Users\username\Documents\script.bat
```

Komut kullanır:
- `/SC` Minute bir zamanlama belirtmek için parametre.
- `/MO` Parametresi beş dakikada bir aralık belirtin.
- `/TN` Parametresini kullanarak görev adı belirtin.
- `/TR` Yolunu belirtmek için parametreyi `script.bat` dosya. 

Windows üzerinde zamanlanmış görev oluşturma hakkında daha fazla bilgi için bkz: [Schtasks](https://technet.microsoft.com/library/cc772785(v=ws.10).aspx#BKMK_minutes).

---
 
Zamanlanmış görev/cron iş düzgün şekilde çalıştığını doğrulamak için yeni dosyalar oluşturma, `myfolder` dizin. Depolama hesabınıza yeni dosyaları karşıya olduğunu onaylamak için beş dakika bekleyin. Zamanlanmış bir görev veya cron proje çıktı günlükleri görüntülemek için günlük dizinine gidin. 

Şirket içi verileri Azure Storage ve tersi yönde taşımak için yollar hakkında daha fazla bilgi için bkz: [için ve Azure Storage veri taşıma](https://docs.microsoft.com/azure/storage/common/storage-moving-data?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).  

## <a name="next-steps"></a>Sonraki adımlar
Azure Storage ve AzCopy hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure Storage'a giriş](../storage-introduction.md)
* [Windows AzCopy ile veri aktarımı](storage-use-azcopy.md) 
* [Linux'ta AzCopy ile veri aktarımı](storage-use-azcopy-linux.md) 
* [Depolama hesabı oluşturma](../storage-create-storage-account.md)
* [BLOB Depolama Gezgini ile yönetme](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs)  






 
 

