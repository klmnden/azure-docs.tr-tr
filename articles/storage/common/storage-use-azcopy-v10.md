---
title: AzCopy v10 kullanarak verileri Azure Depolama'ya taşıma veya kopyalama (Önizleme) | Microsoft Docs
description: AzCopy v10 kullanın ya da blob, veri gölü ve dosya içeriği veri taşınacak veya kopyalanacak (Önizleme) komut satırı yardımcı programı. Yerel dosyaları Azure depolama alanına veri kopyalama veya içinde veya depolama hesapları arasında verileri kopyalayabilirsiniz. Kolayca verilerinizi Azure Depolama'ya geçirin.
services: storage
author: artemuwka
ms.service: storage
ms.topic: article
ms.date: 02/24/2019
ms.author: artemuwka
ms.subservice: common
ms.openlocfilehash: ffc4a0c57681e877250c7be82f5160174178892a
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58486028"
---
# <a name="transfer-data-with-azcopy-v10-preview"></a>AzCopy v10 ile veri aktarımı (Önizleme)

AzCopy v10 olduğu için veya Microsoft Azure Blob ve dosya depolama alanından verileri kopyalamak için komut satırı yardımcı programını (Önizleme). AzCopy v10 yeniden tasarlanan bir komut satırı arabirimi sağlar ve yeni mimari için güvenilir veri aktarır. Azcopy komutunu kullanarak bir dosya sistemi ve depolama hesabı arasında veya depolama hesapları arasında verileri kopyalayabilirsiniz.

## <a name="whats-new-in-azcopy-v10"></a>AzCopy v10 yenilikler

- Azure Blob Depolama veya dosya sistemleri eşitler. Kullanım `azcopy sync <source> <destination>`. Artımlı kopyalama senaryoları için idealdir.
- Azure Data Lake depolama Gen2 API'leri destekler. Kullanım `myaccount.dfs.core.windows.net` olarak Data Lake depolama Gen2 API'leri çağırmak için bir URI.
- Tüm bir hesabı (yalnızca Blob hizmeti) başka bir hesaba kopyalamayı destekler.
- Yeni kullanan [URL'den blok yerleştirme](https://docs.microsoft.com/rest/api/storageservices/put-block-from-url) hesaba kopyalama desteklemek için API'leri. Veri aktarımı aktarımı istemcisine gerekli değilse bu yana hızlıdır.
- Listeler veya belirli bir yolda dosyalar ve bloblar kaldırır.
- Joker karakter düzenleri, yol ve dışlama bayrakları--destekler.
- Her AzCopy örneği ile bir iş sırasını ve ilgili günlük dosyasını oluşturur. Görüntüleyebilir ve önceki işlerini yeniden başlatın ve sürdürme başarısız olan işler. AzCopy aktarım bir hatadan sonra da otomatik olarak yeniden deneyecek.
- Özellikleri genel performans geliştirmeleri.

## <a name="download-and-install-azcopy"></a>AzCopy yükleyip

### <a name="latest-preview-version-v10"></a>En son önizleme sürümünü (v10)

En güncel AzCopy önizleme sürümünü indirin:
- [Windows](https://aka.ms/downloadazcopy-v10-windows) (zip)
- [Linux](https://aka.ms/downloadazcopy-v10-linux) (tar)
- [MacOS](https://aka.ms/downloadazcopy-v10-mac) (zip)

### <a name="latest-production-version-v81"></a>En güncel üretim sürümüne (v8.1)

İndirme [için AzCopy Windows son üretim sürümüne](https://aka.ms/downloadazcopy).

### <a name="azcopy-supporting-table-storage-service-v73"></a>Tablo depolama hizmetini (v7.3) destekleyen AzCopy

İndirme [/Microsoft Azure tablo depolama hizmetinin veri deposundan veri kopyalamayı destekleyen AzCopy v7.3](https://aka.ms/downloadazcopynet).

## <a name="post-installation-steps"></a>Yükleme sonrası adımlar

AzCopy v10 yükleme gerektirmez. Tercih edilen komut satırı uygulamanızı açın ve klasöre göz atın burada `azcopy.exe` bulunur. Gerekli olursa, sistem yolunuzda kullanım kolaylığı için AzCopy klasör konumunu ekleyebilirsiniz.

## <a name="authentication-options"></a>Kimlik doğrulaması seçenekleri

Azure depolama ile kimlik doğrulaması yapılırken, AzCopy v10 aşağıdaki seçeneklerini destekler:
- **Azure Active Directory** (desteklenen **Blob ve Data Lake depolama Gen2 Hizmetleri**). Kullanım ```.\azcopy login``` Azure Active Directory ile oturum açmanız.  Kullanıcının olmalıdır ["Depolama Blob verileri katkıda bulunan" rolü atanmış](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac) Azure Active Directory kimlik doğrulaması ile Blob depolama alanına yazılacak. Azure kaynakları için yönetilen kimlikleri aracılığıyla kimlik doğrulaması için `azcopy login --identity`.
- **Paylaşılan erişim imzası belirteçlerinin [desteklenen Blob ve Dosya Hizmetleri için]**. Paylaşılan erişim imzası (SAS) belirteci kullanmak için komut satırında blob yolu ekleyin. Azure portalı ile SAS belirteçleri oluşturabilirsiniz [Depolama Gezgini](https://blogs.msdn.microsoft.com/jpsanders/2017/10/12/easily-create-a-sas-to-download-a-file-from-azure-storage-using-azure-storage-explorer/), [PowerShell](https://docs.microsoft.com/powershell/module/az.storage/new-azstorageblobsastoken), veya tercih ettiğiniz diğer araçlar. Daha fazla bilgi için [örnekler](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2).

## <a name="getting-started"></a>Başlarken

> [!TIP]
> **Bir grafik kullanıcı arabirimi tercih ediyorsunuz?**
>
> [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/), yöneten Azure depolama veri basitleştiren bir masaüstü istemcisi artık AzCopy hızlandırın ve Azure depolama dışına veri aktarımı için kullanır.
>
> Depolama Gezgini'nde altında AzCopy etkinleştirme **Önizleme** menüsü.
> ![AzCopy, Azure depolama Gezgini'nde bir aktarım altyapısı olarak etkinleştir](media/storage-use-azcopy-v10/enable-azcopy-storage-explorer.jpg)

AzCopy v10, kendi kendine belgelenmiş bir söz dizimine sahiptir. Azure Active Directory ile oturum açtıktan sonra genel söz dizimini aşağıdaki gibi görünür:

```azcopy
.\azcopy <command> <arguments> --<flag-name>=<flag-value>

# Examples if you have logged into the Azure Active Directory:
.\azcopy copy <source path> <destination path> --<flag-name>=<flag-value>
.\azcopy cp "C:\local\path" "https://account.blob.core.windows.net/container" --recursive=true
.\azcopy cp "C:\local\path\myfile" "https://account.blob.core.windows.net/container/myfile"
.\azcopy cp "C:\local\path\*" "https://account.blob.core.windows.net/container"

# Examples if you're using SAS tokens to authenticate:
.\azcopy cp "C:\local\path" "https://account.blob.core.windows.net/container?sastoken" --recursive=true
.\azcopy cp "C:\local\path\myfile" "https://account.blob.core.windows.net/container/myfile?sastoken"
```

İşte nasıl kullanılabilir komutların bir listesini alabilirsiniz:

```azcopy
.\azcopy --help
# To use the alias instead
.\azcopy -h
```

Belirli bir komut için örnekler ve Yardım sayfasını görmek için aşağıdaki komutu çalıştırın:

```azcopy
.\azcopy <cmd> --help
# Example:
.\azcopy cp -h
```

## <a name="create-a-blob-container-or-file-share"></a>Bir blob kapsayıcı veya dosya paylaşımı oluşturma 

**Blob kapsayıcısı oluşturma**

```azcopy
.\azcopy make "https://account.blob.core.windows.net/container-name"
```

**Dosya paylaşımı oluşturma**

```azcopy
.\azcopy make "https://account.file.core.windows.net/share-name"
```

**Azure Data Lake depolama Gen2'ı kullanarak bir blob kapsayıcısı oluşturma**

Hiyerarşik ad alanları, Blob Depolama hesabınızda etkinleştirdiyseniz, karşıya dosya yükleme için yeni bir blob kapsayıcısı oluşturmak için aşağıdaki komutu kullanabilirsiniz.

```azcopy
.\azcopy make "https://account.dfs.core.windows.net/top-level-resource-name"
```

## <a name="copy-data-to-azure-storage"></a>Azure depolama alanına veri kopyalama

Kaynaktan hedefe veri aktarmayı Kopyala komutunu kullanın. Kaynak veya hedef y: olabilir
- Yerel dosya sistemi
- Azure Blob/sanal dizin/kapsayıcı URİ'si
- Azure dosya/dizin/dosya paylaşımı URI'sini
- Azure Data Lake depolama Gen2 dosya/dizin/dosya URI'si

```azcopy
.\azcopy copy <source path> <destination path> --<flag-name>=<flag-value>
# Using the alias instead 
.\azcopy cp <source path> <destination path> --<flag-name>=<flag-value>
```

Aşağıdaki komutu klasörü altındaki tüm dosyaları yükler `C:\local\path` yinelemeli olarak kapsayıcıya `mycontainer1`, oluşturma `path` kapsayıcısında dizin:

```azcopy
.\azcopy cp "C:\local\path" "https://account.blob.core.windows.net/mycontainer1<sastoken>" --recursive=true
```

Aşağıdaki komutu klasörü altındaki tüm dosyaları yükler `C:\local\path` (olmadan alt dizinleri içinde recursing) kapsayıcıya `mycontainer1`:

```azcopy
.\azcopy cp "C:\local\path\*" "https://account.blob.core.windows.net/mycontainer1<sastoken>"
```

Daha fazla örnek bulmak için aşağıdaki komutu kullanın:

```azcopy
.\azcopy cp -h
```

## <a name="copy-data-between-two-storage-accounts"></a>İki depolama hesabı arasında veri kopyalama

İki depolama hesabı arasında veri kopyalama kullanan [URL'den blok yerleştirme](https://docs.microsoft.com/rest/api/storageservices/put-block-from-url) API ve istemcinin ağ bant genişliği kullanmaz. AzCopy, yalnızca kopyalama işlemi düzenler ancak iki Azure depolama sunucular arasında doğrudan veri kopyalanır. Bu seçenek şu anda yalnızca Blob Depolama için kullanılabilir.

İki depolama hesabı arasında veri kopyalamak için aşağıdaki komutu kullanın:
```azcopy
.\azcopy cp "https://myaccount.blob.core.windows.net/<sastoken>" "https://myotheraccount.blob.core.windows.net/<sastoken>" --recursive=true
```

> [!NOTE]
> Bu komut tüm blob kapsayıcılarını listeleme ve bunları hedef hesabına kopyalayın. Şu anda yalnızca blok bloblarını iki depolama hesabı arasında kopyalama AzCopy v10 destekler. Bu, diğer tüm depolama hesabı nesnelerin (BLOB'lar, sayfa blobları, dosyalar, tablolar ve Kuyruklar ekleme gibi) atlar.

## <a name="copy-a-vhd-image-to-a-storage-account"></a>Bir depolama hesabına VHD görüntüsü kopyalayın

AzCopy v10 varsayılan olarak, blok bloblarınızdan veri yükler. Ancak, bir kaynak dosyası varsa, bir `.vhd` uzantısı, AzCopy v10 varsayılan sayfa blobu karşıya yükleme için. Şu anda bu eylemi yapılandırılabilir değildir.

## <a name="sync-incremental-copy-and-delete-blob-storage-only"></a>Eşitleme: artımlı kopyalar ve siler (yalnızca Blob Depolama)

Sync komutunun, bir kaynak dizin dosya adlarını karşılaştırma hedef dizinin içeriğini eşitler ve son zaman damgaları değiştirme. Bu kaynakta mevcut değilse bu işlem, isteğe bağlı bir hedef dosyaların silinmesini içerir. zaman `--delete-destination=prompt|true` bayrağı sağlanır. Silme davranışı varsayılan olarak devre dışıdır. 

> [!NOTE] 
> Kullanım `--delete-destination` bayrağı konusunda dikkatli olun. Etkinleştirme [geçici silme](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) silme davranışını hesabınızdaki yanlışlıkla silinmekten önlemek için eşitleme etkinleştirmeden önce özellik. 

> Zaman `--delete-destination` ayarlandığında true, AzCopy kullanıcıya bir istem olmadan hedef kaynakta mevcut dosyaları da siler. Onay için istemde istiyorsanız kullanın `--delete-destination=prompt`.

Bir depolama hesabı için yerel dosya sisteminize eşitlemek için aşağıdaki komutu kullanın:

```azcopy
.\azcopy sync "C:\local\path" "https://account.blob.core.windows.net/mycontainer1<sastoken>" --recursive=true
```

Ayrıca, bir blob kapsayıcısına bir yerel dosya sistemi aşağı eşitleyebilirsiniz:

```azcopy
# The SAS token isn't required for Azure Active Directory authentication.
.\azcopy sync "https://account.blob.core.windows.net/mycontainer1" "C:\local\path" --recursive=true
```

Bu komut, üzerinde son değiştirilen zaman damgaları göre hedef kaynağa artımlı olarak eşitler. AzCopy v10 ekleyin ya da kaynak dosya silme, aynı hedef yapar. Silme işleminden önce AzCopy onaylamanızı ister.

## <a name="advanced-configuration"></a>Gelişmiş yapılandırma

### <a name="configure-proxy-settings"></a>Proxy ayarlarını yapılandırma

AzCopy v10 için proxy ayarlarını yapılandırmak için aşağıdaki komutu kullanarak ortamı değişken erişmek ayarlayın:

```cmd
# For Windows:
set https_proxy=<proxy IP>:<proxy port>
# For Linux:
export https_proxy=<proxy IP>:<proxy port>
# For MacOS
export https_proxy=<proxy IP>:<proxy port>
```

### <a name="optimize-throughput"></a>Aktarım hızını iyileştirme

AZCOPY_CONCURRENCY_VALUE eşzamanlı istek sayısını yapılandırmak ve aktarım hızı performans ve kaynak tüketimi denetlemek için ortam değişkenini ayarlayın. Değer 300 varsayılan olarak ayarlanır. Değer azaltma AzCopy v10 tarafından kullanılan CPU ve bant genişliği sınırlar.

```cmd
# For Windows:
set AZCOPY_CONCURRENCY_VALUE=<value>
# For Linux:
export AZCOPY_CONCURRENCY_VALUE=<value>
# For MacOS
export AZCOPY_CONCURRENCY_VALUE=<value>
# To check the current value of the variable on all the platforms
.\azcopy env
# If the value is blank then the default value is currently in use
```

### <a name="change-the-location-of-the-log-files"></a>Günlük dosyalarının konumunu değiştirin

Gerekirse günlük dosyalarının veya işletim sistemi diski dolmaması için konumu değiştirebilirsiniz.

```cmd
# For Windows:
set AZCOPY_LOG_LOCATION=<value>
# For Linux:
export AZCOPY_LOG_LOCATION=<value>
# For MacOS
export AZCOPY_LOG_LOCATION=<value>
# To check the current value of the variable on all the platforms
.\azcopy env
# If the value is blank, then the default value is currently in use
```
### <a name="change-the-default-log-level"></a>Varsayılan günlük düzeyi değiştirme 

Varsayılan olarak, AzCopy günlük düzeyi bilgileri ayarlanır. Disk alanından kazanmak için günlük ayrıntı azaltmak istiyorsanız, ayarı kullanarak üzerine ``--log-level`` seçeneği. Kullanılabilir günlük düzeyleri şunlardır: Hata ayıklama, bilgi, uyarı, hata, PANİK ve önemli.

### <a name="review-the-logs-for-errors"></a>Hatalar için günlükleri gözden geçirin

Aşağıdaki komutu 04dc9ca9-158f-7945-5933-564021086c79 günlüğünden UPLOADFAILED durumundaki tüm hataları alırsınız:

```azcopy
cat 04dc9ca9-158f-7945-5933-564021086c79.log | grep -i UPLOADFAILED
```
## <a name="troubleshooting"></a>Sorun giderme

AzCopy v10 günlük dosyaları ve her proje için plan dosyaları oluşturur. Araştırmak ve olası sorunları gidermek için günlükleri'ni kullanabilirsiniz. Günlükleri (UPLOADFAILED COPYFAILED ve DOWNLOADFAILED), hata durumunu içerecek tam yolunu ve hatanın nedenini. İş günlüklerini ve planı dosyalarını % USERPROFILE bulunur\\Windows ya da $HOME .azcopy klasörü\\Mac ve Linux'ta .azcopy klasör.

> [!IMPORTANT]
> Microsoft Support (veya herhangi bir üçüncü taraf ilgili sorun giderme), bir isteği gönderirken çalıştırmak istediğiniz komutun sonuç kısaltılmıştır sürümü paylaşabilir. Bu, SAS yanlışlıkla herkesle paylaşılmaz sağlar. Günlük dosyasının başında sonuç kısaltılmıştır sürümü bulabilirsiniz.

### <a name="view-and-resume-jobs"></a>İşleri görüntüleme ve devam et

Her bir aktarım işlemi AzCopy işi oluşturur. İş geçmişini görüntülemek için aşağıdaki komutu kullanın:

```azcopy
.\azcopy jobs list
```

İş istatistiklerini görüntülemek için aşağıdaki komutu kullanın:

```azcopy
.\azcopy jobs show <job-id>
```

Aktarımları durumuna göre filtre uygulamak için aşağıdaki komutu kullanın:

```azcopy
.\azcopy jobs show <job-id> --with-status=Failed
```

Başarısız/iptal bir işi sürdürmek için aşağıdaki komutu kullanın. Bu komut, SAS belirteci ile birlikte tanımlayıcısını kullanır. Güvenlik nedeniyle kalıcı değildir:

```azcopy
.\azcopy jobs resume <jobid> --sourcesastokenhere --destinationsastokenhere
```

## <a name="next-steps"></a>Sonraki adımlar

Sorular, sorunları veya genel bir geri bildirim varsa, bunları gönderme [github'da](https://github.com/Azure/azure-storage-azcopy).


