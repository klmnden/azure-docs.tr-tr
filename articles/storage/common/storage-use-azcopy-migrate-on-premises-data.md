---
title: 'Öğretici: AzCopy komutunu kullanarak Azure Depolama’ya şirket içi verileri geçirme| Microsoft Docs'
description: Bu öğreticide AzCopy komutunu kullanarak blob, tablo ve dosya içeriğine/içeriğinden verileri geçirecek veya kopyalayacaksınız. Yerel depolama alanınızdan Azure Depolama’ya kolayca verileri geçirin.
services: storage
author: normesta
ms.service: storage
ms.topic: tutorial
ms.date: 05/14/2019
ms.author: normesta
ms.reviewer: seguler
ms.subservice: common
ms.openlocfilehash: 193c00354b6222152e26476d0b06cfb1555c207e
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66754886"
---
#  <a name="tutorial-migrate-on-premises-data-to-cloud-storage-by-using-azcopy"></a>Öğretici: AzCopy komutunu kullanarak bulut depolamaya şirket içi verileri geçirme

AzCopy; basit komutlar kullanılarak Azure Blob depolamaya, Azure Dosyaları’na ve Azure Tablosu depolama alanına veya bunlardan veri kopyalamaya yönelik bir komut satırı aracıdır. Komutlar, en iyi performans için tasarlanmıştır. AzCopy ile bir dosya sistemi ile depolama hesabı arasında veya depolama hesapları arasında verileri kopyalayabilirsiniz. AzCopy, yerel verileri (şirket içi) bir depolama hesabına kopyalamak için kullanılabilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir depolama hesabı oluşturun. 
> * AzCopy komutunu kullanarak tüm verilerinizi karşıya yükleme.
> * Test amacıyla verileri değiştirme.
> * Karşıya yüklenecek yeni dosyaları belirlemek için zamanlanmış bir görev veya sıralanmış iş oluşturma.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için en güncel AzCopy sürümünü indirin. Bkz: [Azcopy'i kullanmaya başlama](storage-use-azcopy-v10.md).

Windows kullanıyorsanız bu öğreticide görev zamanlamak için kullanılan [Schtasks](https://msdn.microsoft.com/library/windows/desktop/bb736357(v=vs.85).aspx) uygulamasını edinmeniz gerekir. Linux kullanıcıları bunun yerine crontab komutunu kullanacaktır.

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

İlk adım bir kapsayıcı oluşturmaktır. Blobların her zaman bir kapsayıcı içine yüklenmesi gerekir. Bilgisayarınızdaki dosyaları düzenlemek için klasörleri kullandığınız gibi blob gruplarını düzenlemek için de kapsayıcılardan faydalanabilirsiniz.

Kapsayıcı oluşturmak için şu adımları izleyin:

1. Ana sayfadan **Depolama hesapları** düğmesini seçin ve oluşturduğunuz depolama hesabını seçin.
2. **Hizmetler** bölümünden **Bloblar**’ı seçin ve sonra **Kapsayıcı**’yı seçin.

   ![Bir kapsayıcı oluşturma](media/storage-azcopy-migrate-on-premises-data/CreateContainer.png)
 
Kapsayıcı harfleri bir harf veya sayı ile başlamalıdır. Bunlar yalnızca harf, sayı ve kısa çizgi karakterini (-) içerebilir. Kapsayıcıları ve blobları adlandırma kuralları hakkında daha fazla bilgi için bkz. [Kapsayıcıları, blobları ve meta verileri adlandırma ve bunlara başvurma](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

## <a name="download-azcopy"></a>AzCopy indirin

AzCopy V10 yürütülebilir dosyasını indirin.

- [Windows](https://aka.ms/downloadazcopy-v10-windows) (zip)
- [Linux](https://aka.ms/downloadazcopy-v10-linux) (tar)
- [MacOS](https://aka.ms/downloadazcopy-v10-mac) (zip)

AzCopy dosyasını bilgisayarınızdaki herhangi bir yere yerleştirin. Bu yürütülebilir dosyayı bilgisayarınızda herhangi bir klasörden başvurabilmeniz için dosyanın konumunu sistem path değişkenine ekleyin.

## <a name="authenticate-with-azure-ad"></a>Azure AD ile kimlik doğrulaması

İlk olarak, Ata [depolama Blob verileri katkıda bulunan](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-queue-data-contributor) kimliğinize rol. Bkz: [verilere Azure blob ve kuyruk RBAC ile Azure portalında erişim ver](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac-portal).

Ardından, bir komut istemi açın, aşağıdaki komutu yazın ve ENTER tuşuna basın.

```azcopy
azcopy login
```

Bu komut, kimlik doğrulaması kodu ve bir Web sitesinin URL'sini döndürür. Web sitesini açmak, kodu sağlayın ve ardından **sonraki** düğmesi.

![Bir kapsayıcı oluşturma](media/storage-use-azcopy-v10/azcopy-login.png)

Bir oturum açma penceresi görünür. Bu pencerede, Azure hesabı kimlik bilgilerinizi kullanarak Azure hesabınızda oturum açın. Oturumunuz başarıyla açıldıktan sonra tarayıcı penceresini kapatın ve AzCopy kullanarak başlayın.

## <a name="upload-contents-of-a-folder-to-blob-storage"></a>Bir klasörün içeriğini Blob depolama alanına yükleme

AzCopy komutunu kullanarak, bir klasördeki tüm dosyaları, [Windows](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy) veya [Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux) üzerindeki Blob depolama alanına yükleyebilirsiniz. Bir klasördeki tüm blobları karşıya yüklemek için aşağıdaki AzCopy komutunu girin:

```AzCopy
azcopy copy "<local-folder-path>" "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>" --recursive=true
```

* Değiştirin `<local-folder-path>` dosyalarını içeren bir klasörün yolu ile yer tutucu (örneğin: `C:\myFolder` veya `/mnt/myFolder`).

* Değiştirin `<storage-account-name>` depolama hesabınızın adıyla yer tutucu.

* Değiştirin `<container-name>` tutucusunu oluşturduğunuz kapsayıcısının adı.

Belirtilen dizinin içeriklerini Blob depolama alanına yinelemeli olarak yüklemek için belirtin `--recursive` seçeneği. Bu seçenekle, AzCopy çalıştırdığınızda tüm alt klasörler ve bunların dosyaları da karşıya yüklenir.

## <a name="upload-modified-files-to-blob-storage"></a>Değiştirilen dosyaları Blob depolama alanına yükleme

Kendi son değiştirilme zamanına göre dosyaları karşıya yüklemek için AzCopy kullanabilirsiniz. 

Bunu denemek için, test amacıyla kaynak dizininizde yeni dosyalar oluşturun veya değiştirin. Ardından, Azcopy'yi kullanma `sync` komutu.

```AzCopy
azcopy sync "<local-folder-path>" "https://<storage-account-name>.blob.core.windows.net/<container-name>" --recursive=true
```

* Değiştirin `<local-folder-path>` dosyalarını içeren bir klasörün yolu ile yer tutucu (örneğin: `C:\myFolder` veya `/mnt/myFolder`.

* Değiştirin `<storage-account-name>` depolama hesabınızın adıyla yer tutucu.

* Değiştirin `<container-name>` tutucusunu oluşturduğunuz kapsayıcısının adı.

Hakkında daha fazla bilgi edinmek için `sync` komutu, bkz: [eşitleme dosyaları](storage-use-azcopy-blobs.md#synchronize-files).

## <a name="create-a-scheduled-task"></a>Zamanlanmış görev oluşturma

AzCopy komut betiğini çalıştıran bir zamanlanmış görev veya sıralanmış iş oluşturabilirsiniz. Betik, belirli bir zaman aralığında yeni şirket içi verileri belirler ve bulut depolama alanına yükler.

AzCopy komutunu bir metin düzenleyiciye kopyalayın. AzCopy komutunun parametre değerlerini uygun değerlerle güncelleştirin. Dosyayı, AzCopy için `script.sh` (Linux) veya `script.bat` (Windows) olarak kaydedin. 

Bu örnekler klasörünüze adlandırıldığını varsayar `myFolder`, depolama hesabınızın adını `mystorageaccount` ve kapsayıcı adınız `mycontainer`.

> [!NOTE]
> Linux örnek bir SAS belirteci ekler. Komutunuz birinde sağlamanız gerekir. AzCopy V10'ın geçerli sürümü, Azure AD yetkilendirme cron işlerinde desteklemiyor.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

    azcopy sync "/mnt/myfiles" "https://mystorageaccount.blob.core.windows.net/mycontainer?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-05-30T06:57:40Z&st=2019-05-29T22:57:40Z&spr=https&sig=BXHippZxxx54hQn%2F4tBY%2BE2JHGCTRv52445rtoyqgFBUo%3D" --recursive=true

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

    azcopy sync "C:\myFolder" "https://mystorageaccount.blob.core.windows.net/mycontainer" --recursive=true

---

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

Bu örnek kodunuzu, bilgisayarın kök sürücüsünde bulunur, ancak betiğinizi, her yerde, istediğiniz olabilir varsayar.

```cmd
schtasks /CREATE /SC minute /MO 5 /TN "AzCopy Script" /TR C:\script.bat
```

Komut:
- Bir dakikalık zamanlamayı belirtmek için `/SC` parametresini kullanır.
- Beş dakikalık zaman aralığını belirtmek için `/MO` parametresini kullanır.
- Görev adını belirtmek için `/TN` parametresini kullanır.
- `script.bat` dosyasının yolunu belirtmek için `/TR` parametresini kullanır.

Windows üzerinde zamanlanmış görev oluşturma hakkında daha fazla bilgi için bkz. [Schtasks](https://technet.microsoft.com/library/cc772785(v=ws.10).aspx#BKMK_minutes).

---

Zamanlanmış görevin/sıralanmış işin düzgün şekilde çalıştığını doğrulamak için `myFolder` dizininizde yeni dosyalar oluşturun. Yeni dosyaların depolama hesabınıza yüklendiğini onaylamak için beş dakika bekleyin. Zamanlanmış görevin veya sıralanmış işin çıktı günlüklerini görüntülemek için günlük dizininize gidin.

## <a name="next-steps"></a>Sonraki adımlar

Şirket içi verileri Azure Depolama’ya (veya tam tersi) taşıma yolları hakkında daha fazla bilgi edinmek için bkz.:

* [Azure Depolamadan/Depolamaya veri taşıma](https://docs.microsoft.com/azure/storage/common/storage-moving-data?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).  

AzCopy hakkında daha fazla bilgi için şu makalelerden herhangi birine bakın:

* [Azcopy'i kullanmaya başlama](storage-use-azcopy-v10.md)

* [AzCopy ve blob depolama ile veri aktarma](storage-use-azcopy-blobs.md)

* [AzCopy ve dosya depolama ile veri aktarma](storage-use-azcopy-files.md)

* [AzCopy ve Amazon S3 demetleri ile veri aktarma](storage-use-azcopy-s3.md)
 
* [Yapılandırma, en iyi duruma getirmek ve AzCopy sorunlarını giderme](storage-use-azcopy-configure.md)
