---
title: 'Öğretici: AzCopy komutunu kullanarak Azure Depolama’ya şirket içi verileri geçirme| Microsoft Docs'
description: Bu öğreticide AzCopy komutunu kullanarak blob, tablo ve dosya içeriğine/içeriğinden verileri geçirecek veya kopyalayacaksınız. Yerel depolama alanınızdan Azure Depolama’ya kolayca verileri geçirin.
services: storage
author: roygara
ms.service: storage
ms.topic: tutorial
ms.date: 12/14/2017
ms.author: rogarana
ms.subservice: common
ms.openlocfilehash: 40138a69baf9cd621b2f287b2fe035225bfd9bec
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58877508"
---
#  <a name="tutorial-migrate-on-premises-data-to-cloud-storage-by-using-azcopy"></a>Öğretici: AzCopy komutunu kullanarak bulut depolamaya şirket içi verileri geçirme

AzCopy; basit komutlar kullanılarak Azure Blob depolamaya, Azure Dosyaları’na ve Azure Tablosu depolama alanına veya bunlardan veri kopyalamaya yönelik bir komut satırı aracıdır. Komutlar, en iyi performans için tasarlanmıştır. AzCopy ile bir dosya sistemi ile depolama hesabı arasında veya depolama hesapları arasında verileri kopyalayabilirsiniz. AzCopy, yerel verileri (şirket içi) bir depolama hesabına kopyalamak için kullanılabilir.

İşletim sisteminiz için uygun AzCopy sürümünü indirin:

* [Linux üzerinde AzCopy](storage-use-azcopy-linux.md), .NET Core Framework ile derlenir. Bu, POSIX stili komut satırı seçeneklerini sunarak Linux platformlarını hedefler. 
* [Windows üzerinde AzCopy](storage-use-azcopy.md), .NET Framework ile derlenir. Windows stili komut satırı seçenekleri sunar. 
 
Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Depolama hesabı oluşturma. 
> * AzCopy komutunu kullanarak tüm verilerinizi karşıya yükleme.
> * Test amacıyla verileri değiştirme.
> * Karşıya yüklenecek yeni dosyaları belirlemek için zamanlanmış bir görev veya sıralanmış iş oluşturma.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için, [Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux#download-and-install-azcopy) veya [Windows](https://aka.ms/downloadazcopy) üzerinde en son AzCopy sürümünü indirin.

Windows kullanıyorsanız bu öğreticide görev zamanlamak için kullanılan [Schtasks](https://msdn.microsoft.com/library/windows/desktop/bb736357(v=vs.85).aspx) uygulamasını edinmeniz gerekir. Linux kullanıcıları bunun yerine crontab komutunu kullanacaktır.

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

İlk adım bir kapsayıcı oluşturmaktır. Blobların her zaman bir kapsayıcı içine yüklenmesi gerekir. Bilgisayarınızdaki dosyaları düzenlemek için klasörleri kullandığınız gibi blob gruplarını düzenlemek için de kapsayıcılardan faydalanabilirsiniz. 

Kapsayıcı oluşturmak için şu adımları izleyin:

1. Ana sayfadan **Depolama hesapları** düğmesini seçin ve oluşturduğunuz depolama hesabını seçin.
2. **Hizmetler** bölümünden **Bloblar**’ı seçin ve sonra **Kapsayıcı**’yı seçin. 

   ![Bir kapsayıcı oluşturma](media/storage-azcopy-migrate-on-premises-data/CreateContainer.png)
 
Kapsayıcı harfleri bir harf veya sayı ile başlamalıdır. Bunlar yalnızca harf, sayı ve kısa çizgi karakterini (-) içerebilir. Kapsayıcıları ve blobları adlandırma kuralları hakkında daha fazla bilgi için bkz. [Kapsayıcıları, blobları ve meta verileri adlandırma ve bunlara başvurma](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

## <a name="upload-contents-of-a-folder-to-blob-storage"></a>Bir klasörün içeriğini Blob depolama alanına yükleme

AzCopy komutunu kullanarak, bir klasördeki tüm dosyaları, [Windows](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#upload-blobs-to-blob-storage) veya [Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux#blob-download) üzerindeki Blob depolama alanına yükleyebilirsiniz. Bir klasördeki tüm blobları karşıya yüklemek için aşağıdaki AzCopy komutunu girin:

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

    azcopy \
        --source /mnt/myfolder \
        --destination https://myaccount.blob.core.windows.net/mycontainer \
        --dest-key <key> \
        --recursive

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:<key> /S
---

`<key>` ve `key` değerinin yerine hesap anahtarınızı girin. Azure portalında, depolama hesabınızdaki **Ayarlar** bölümünden **Erişim anahtarları**’nı seçerek hesap anahtarınızı alabilirsiniz. Bir anahtar seçip AzCopy komutuna yapıştırın. Belirtilen hedef kapsayıcı mevcut değilse, AzCopy bu kapsayıcıyı oluşturur ve dosyayı kapsayıcıya yükler. Veri dizininizin kaynak yolunu güncelleştirin ve hedef URL’deki **myaccount** değerinin yerine depolama hesabınızın adını girin.

Belirtilen dizinin içeriklerini Blob depolama alanına yinelemeli olarak yüklemek için `--recursive` (Linux) veya `/S` (Windows) seçeneğini belirtin. AzCopy komutunu şu seçeneklerden biriyle çalıştırdığınızda tüm alt klasörler ve bu klasörlerin dosyaları da karşıya yüklenir.

## <a name="upload-modified-files-to-blob-storage"></a>Değiştirilen dosyaları Blob depolama alanına yükleme

Son değiştirilme zamanına göre [dosyaları karşıya yüklemek](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#other-azcopy-features) için de AzCopy kullanabilirsiniz. Bunu denemek için, test amacıyla kaynak dizininizde yeni dosyalar oluşturun veya değiştirin. Yalnızca güncelleştirilmiş veya yeni dosyaları karşıya yüklemek için, AzCopy komutuna `--exclude-older` (Linux) veya `/XO` (Windows) parametresini ekleyin.

Yalnızca hedefte bulunmayan çıkış kaynaklarını kopyalamak istiyorsanız, AzCopy komutunda `--exclude-older` ve `--exclude-newer` (Linux) veya `/XO` ve `/XN` (Windows) parametrelerini belirtin. AzCopy, yalnızca güncelleştirilmiş verileri zaman damgasına göre karşıya yükler.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

    azcopy \
    --source /mnt/myfolder \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --recursive \
    --exclude-older

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:<key> /S /XO
---

## <a name="create-a-scheduled-task"></a>Zamanlanmış görev oluşturma

AzCopy komut betiğini çalıştıran bir zamanlanmış görev veya sıralanmış iş oluşturabilirsiniz. Betik, belirli bir zaman aralığında yeni şirket içi verileri belirler ve bulut depolama alanına yükler.

AzCopy komutunu bir metin düzenleyiciye kopyalayın. AzCopy komutunun parametre değerlerini uygun değerlerle güncelleştirin. Dosyayı, AzCopy için `script.sh` (Linux) veya `script.bat` (Windows) olarak kaydedin.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

    azcopy --source /mnt/myfiles --destination https://myaccount.blob.core.windows.net/mycontainer --dest-key <key> --recursive --exclude-older --exclude-newer --verbose >> Path/to/logfolder/`date +\%Y\%m\%d\%H\%M\%S`-cron.log

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

    cd C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy
    AzCopy /Source: C:\myfolder  /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:<key> /V /XO /XN >C:\Path\to\logfolder\azcopy%date:~-4,4%%date:~-7,2%%date:~-10,2%%time:~-11,2%%time:~-8,2%%time:~-5,2%.log
---

AzCopy, ayrıntılı `--verbose` (Linux) veya `/V` (Windows) seçeneği ile çalıştırılır. Çıktı bir günlük dosyasına yeniden yönlendirilir.

Bu öğreticide, Windows üzerinde zamanlanmış görev oluşturmak için [Schtasks](https://msdn.microsoft.com/library/windows/desktop/bb736357(v=vs.85).aspx) kullanılır. Linux üzerinde bir sıralanmış iş oluşturmak için [Crontab](http://crontab.org/) komutu kullanılır.
 **Schtasks**, bir yöneticinin yerel veya uzak bilgisayarda zamanlanmış görevler oluşturmasına, silmesine, sorgulamasına, değiştirmesine, çalıştırmasına ve sonlandırmasına olanak sağlar. **Cron**, Linux ve Unix kullanıcılarının [sıralanmış iş ifadeleri](https://en.wikipedia.org/wiki/Cron#CRON_expression) kullanarak belirtilen bir tarih ve saatte komutları veya betikleri çalıştırmasına olanak sağlar.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Linux üzerinde sıralanmış iş oluşturmak için bir terminale aşağıdaki komutu girin:

```bash
crontab -e
*/5 * * * * sh /path/to/script.sh
```

Komutta `*/5 * * * *` sıralanmış iş ifadesi belirtilmesi, `script.sh` kabuk betiğinin beş dakikada bir çalıştırılacağını belirtir. Betiği, günlük, aylık veya yıllık olarak belirli bir saatte çalıştırılacak şekilde zamanlayabilirsiniz. İş yürütme için tarih ve saati ayarlama hakkında daha fazla bilgi edinmek istiyorsanız bkz. [sıralanmış iş ifadeleri](https://en.wikipedia.org/wiki/Cron#CRON_expression).

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Windows üzerinde zamanlanmış görev oluşturmak için, komut satırına veya PowerShell’e aşağıdaki komutu girin:

```cmd
schtasks /CREATE /SC minute /MO 5 /TN "AzCopy Script" /TR C:\Users\username\Documents\script.bat
```

Komut:
- Bir dakikalık zamanlamayı belirtmek için `/SC` parametresini kullanır.
- Beş dakikalık zaman aralığını belirtmek için `/MO` parametresini kullanır.
- Görev adını belirtmek için `/TN` parametresini kullanır.
- `script.bat` dosyasının yolunu belirtmek için `/TR` parametresini kullanır.

Windows üzerinde zamanlanmış görev oluşturma hakkında daha fazla bilgi için bkz. [Schtasks](https://technet.microsoft.com/library/cc772785(v=ws.10).aspx#BKMK_minutes).

---

Zamanlanmış görevin/sıralanmış işin düzgün şekilde çalıştığını doğrulamak için `myfolder` dizininizde yeni dosyalar oluşturun. Yeni dosyaların depolama hesabınıza yüklendiğini onaylamak için beş dakika bekleyin. Zamanlanmış görevin veya sıralanmış işin çıktı günlüklerini görüntülemek için günlük dizininize gidin.

## <a name="next-steps"></a>Sonraki adımlar

Şirket içi verileri Azure Depolama’ya (veya tam tersi) taşıma yolları hakkında daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [Azure Depolamadan/Depolamaya veri taşıma](https://docs.microsoft.com/azure/storage/common/storage-moving-data?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).  

AzCopy hakkında daha fazla bilgi edinmek için işletim sisteminize uygun makaleyi seçin:

> [!div class="nextstepaction"]
> [Windows üzerinde AzCopy ile veri aktarma](storage-use-azcopy.md)
> [Linux üzerinde AzCopy ile veri aktarma](storage-use-azcopy-linux.md)
