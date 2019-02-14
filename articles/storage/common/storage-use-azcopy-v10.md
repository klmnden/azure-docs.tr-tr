---
title: Verileri Azure depolama ile AzCopy v10 taşıma veya kopyalama (Önizleme) | Microsoft Docs
description: AzCopy v10 kullanın (Önizleme) yardımcı programı, taşımaya veya konumda veri veya blob, veri gölü ve dosya içeriğini kopyalayın. Yerel dosyaları Azure depolama alanına veri kopyalama veya içinde veya depolama hesapları arasında verileri kopyalayabilirsiniz. Kolayca verilerinizi Azure Depolama'ya geçirin.
services: storage
author: artemuwka
ms.service: storage
ms.topic: article
ms.date: 10/09/2018
ms.author: artemuwka
ms.subservice: common
ms.openlocfilehash: c9009e898b00212dba4dec9bf38af2bfa057b8ea
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56244615"
---
# <a name="transfer-data-with-the-azcopy-v10-preview"></a>AzCopy v10 ile veri aktarımı (Önizleme)

AzCopy v10 (Önizleme) yeniden tasarlanan bir komut satırı arabirimi ve yeni mimari, yüksek performanslı, güvenilir veri aktarımları için sunar, Microsoft Azure Blob ve dosya depolama içine/dışına veri kopyalamak için tasarlanan yeni nesil komut satırı yardımcı olan. AzCopy kullanarak bir dosya sistemi ve depolama hesabı arasında veya depolama hesapları arasında verileri kopyalayabilirsiniz.

## <a name="whats-new-in-azcopy-v10"></a>AzCopy v10 yenilikler

- Azure Blob veya tam tersi bir dosya sistemi eşitleyin. Kullanım `azcopy sync <source> <destination>`. Artımlı kopyalama senaryoları için idealdir.
- Azure Data Lake depolama Gen2 API'leri destekler. Kullanım `myaccount.dfs.core.windows.net` ADLS Gen2 API'leri çağırmak için bir URI olarak.
- Tüm bir hesabı (yalnızca Blob hizmeti) başka bir hesaba kopyalamayı destekler.
- Hesap kopyalanacak hesabı artık yeni kullanıyor [URL'den Put](https://docs.microsoft.com/rest/api/storageservices/put-block-from-url) API'leri. İstemciye veri aktarımı olmadan daha hızlı bir aktarım getiren gerekli!
- Liste/Kaldır dosyalar ve bloblar belirli bir yolda.
- Joker karakter düzenleri, bir yolda yanı sıra dahil etme ve dışlama bayrakları----destekler.
- Geliştirilmiş dayanıklılık: her AzCopy örnek bir iş sırasını ve ilgili günlük dosyasını oluşturur. Görüntüleyebilir ve önceki işlerini yeniden başlatın ve sürdürme başarısız olan işler. AzCopy aktarım bir hatadan sonra da otomatik olarak yeniden deneyecek.
- Genel performans geliştirmeleri.

## <a name="download-and-install-azcopy"></a>AzCopy yükleyip

### <a name="latest-preview-version-v10"></a>En son önizleme sürümünü (v10)

En güncel AzCopy önizleme sürümünü indirin:
- [Windows](https://aka.ms/downloadazcopy-v10-windows)
- [Linux](https://aka.ms/downloadazcopy-v10-linux)
- [MacOS](https://aka.ms/downloadazcopy-v10-mac)

### <a name="latest-production-version-v81"></a>En güncel üretim sürümüne (v8.1)

İndirme [için AzCopy Windows son üretim sürümüne](https://aka.ms/downloadazcopy).

### <a name="azcopy-supporting-table-storage-service-v73"></a>Tablo depolama hizmetini (v7.3) destekleyen AzCopy

İndirme [/Microsoft Azure tablo depolama hizmetinin veri deposundan veri kopyalamayı destekleyen AzCopy v7.3](https://aka.ms/downloadazcopynet).

## <a name="post-installation-steps"></a>Yükleme sonrası adımlar

AzCopy v10 yükleme gerektirmez. Tercih edilen bir komut satırı uygulamasını açın ve klasöre gidin. burada `azcopy.exe` yürütülebilir bulunur. İsterseniz, klasör AzCopy konumunu sistem yolunuza ekleyebilirsiniz.

## <a name="authentication-options"></a>Kimlik doğrulama seçenekleri

AzCopy v10 Azure depolama ile kimlik doğrulaması yapılırken aşağıdaki seçenekleri kullanmanıza olanak tanır:
- **Azure Active Directory [desteklenen Blob ve ADLS Gen2 hizmeti]**. Kullanım ```.\azcopy login``` için Azure Active Directory kullanarak oturum açın.  Kullanıcının olmalıdır ["Depolama Blob verileri katkıda bulunan" rolü atanmış](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac) Azure Active Directory kimlik doğrulamasını kullanarak Blob depolama alanına yazılacak.
- **SAS belirteçleri [desteklenen Blob ve Dosya Hizmetleri için]**. SAS belirteci kullanmak için komut satırında blob yolu ekleyin. Azure portalını kullanarak bir SAS belirteci oluşturabilir [Depolama Gezgini](https://blogs.msdn.microsoft.com/jpsanders/2017/10/12/easily-create-a-sas-to-download-a-file-from-azure-storage-using-azure-storage-explorer/), [PowerShell](https://docs.microsoft.com/powershell/module/az.storage/new-azstorageblobsastoken), veya tercih ettiğiniz diğer araçlar. Daha fazla bilgi için [örnekler](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2).

> [!IMPORTANT]
> Bir destek isteği Microsoft Support (veya bir 3. taraf ilgili sorun giderme) Lütfen herkesle SAS sağlamak için yürütülecek çalıştığınız komut sonuç kısaltılmıştır sürümünü yanlışlıkla paylaşılmıyor paylaşımı gönderirken. Günlük dosyasının başında sonuç kısaltılmıştır sürümü bulabilirsiniz. Daha fazla bilgi için bu makalenin ilerleyen bölümlerinde sorun giderme bölümünü gözden geçirin.

## <a name="getting-started"></a>Başlarken

AzCopy v10 basit Self belgelenmiş sözdizimi geçersiz. Genel sözdizimi, Azure Active Directory ile oturum açıldığında şu şekilde görünür:

```azcopy
.\azcopy <command> <arguments> --<flag-name>=<flag-value>
# Examples if you have logged into the Azure Active Directory:
.\azcopy copy <source path> <destination path> --<flag-name>=<flag-value>
.\azcopy cp "C:\local\path" "https://account.blob.core.windows.net/container" --recursive=true
.\azcopy cp "C:\local\path\myfile" "https://account.blob.core.windows.net/container/myfile"
.\azcopy cp "C:\local\path\*" "https://account.blob.core.windows.net/container"

# Examples if you are using SAS tokens to authenticate:
.\azcopy cp "C:\local\path" "https://account.blob.core.windows.net/container?sastoken" --recursive=true
.\azcopy cp "C:\local\path\myfile" "https://account.blob.core.windows.net/container/myfile?sastoken"
```

İşte nasıl kullanılabilir komutların bir listesini alabilirsiniz:

```azcopy
.\azcopy -help
# Using the alias instead
.\azcopy -h
```

Örnekler için aşağıdaki komutu çalıştırın, belirli bir komut ve Yardım sayfasını görmek için:

```azcopy
.\azcopy <cmd> -help
# Example:
.\azcopy cp -h
```

## <a name="create-a-blob-container-or-file-share"></a>Bir Blob kapsayıcısı veya dosya paylaşımı oluşturma 

**Blob kapsayıcısı oluşturma**

```azcopy
.\azcopy make "https://account.blob.core.windows.net/container-name"
```

**Dosya paylaşımı oluşturma**

```azcopy
.\azcopy make "https://account.file.core.windows.net/share-name"
```

**ADLS Gen2'ı kullanarak Blob kapsayıcısı oluşturma**

Hiyerarşik ad alanları, blob depolama hesabınızda etkinleştirdiyseniz, böylece için dosyaları karşıya yükleyebilir, yeni bir dosya sistemi (Blob kapsayıcısı) oluşturmak için aşağıdaki komutu kullanabilirsiniz.

```azcopy
.\azcopy make "https://account.dfs.core.windows.net/top-level-resource-name"
```

## <a name="copy-data-to-azure-storage"></a>Azure depolama alanına veri kopyalama

Kaynaktan hedefe veri aktarmayı Kopyala komutunu kullanın. Kaynak/hedef y: olabilir
- Yerel dosya sistemi
- Azure Blob/sanal dizin/kapsayıcı URİ'si
- Azure dosya/dizin/dosya paylaşımı URI'sini
- Azure Data Lake depolama Gen2 dosya/dizin/dosya URI'si

```azcopy
.\azcopy copy <source path> <destination path> --<flag-name>=<flag-value>
# Using alias instead
.\azcopy cp <source path> <destination path> --<flag-name>=<flag-value>
```

Aşağıdaki komutu klasörü altındaki tüm dosyaları yükler `C:\local\path` yinelemeli olarak kapsayıcıya `mycontainer1` oluşturma `path` kapsayıcısında dizin:

```azcopy
.\azcopy cp "C:\local\path" "https://account.blob.core.windows.net/mycontainer1<sastoken>" --recursive=true
```

Aşağıdaki komutu klasörü altındaki tüm dosyaları yükler `C:\local\path` (olmadan alt dizinleri içinde recursing) kapsayıcıya `mycontainer1`:

```azcopy
.\azcopy cp "C:\local\path\*" "https://account.blob.core.windows.net/mycontainer1<sastoken>"
```

Daha fazla örnek almak için aşağıdaki komutu kullanın:

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
> Komut tüm blob kapsayıcılarını listeleme ve bunları hedef hesabına kopyalayın. Şu anda yalnızca blok bloblarını iki depolama hesapları arasında kopyalama AzCopy v10 destekler. (Ekleme blobları, sayfa blobları, dosyalar, tablolar ve Kuyruklar), diğer tüm depolama hesabı nesneleri atlanacak.

## <a name="copy-a-vhd-image-to-a-storage-account"></a>Bir depolama hesabına VHD görüntüsü kopyalayın

AzCopy v10 varsayılan olarak, blok bloblarınızdan veri yükler. Bir kaynak dosyası vhd uzantısı içeriyorsa, Bununla birlikte, AzCopy v10 varsayılan olarak, sayfa blob yükleyeceksiniz. Bu davranış, şu anda yapılandırılabilir değildir.

## <a name="sync-incremental-copy-and-delete-blob-storage-only"></a>Eşitleme: artımlı kopyalar ve siler (yalnızca Blob Depolama)

> [!NOTE]
> Sync komutunun içeriğini kaynaktan hedefe eşitler ve bu kaynakta mevcut değilse bu hedef dosyaların SİLİNMESİNİ içerir. Eşitlemek istediğiniz hedef kullandığınızdan emin olun.

Bir depolama hesabı için yerel dosya sisteminize eşitlemek için aşağıdaki komutu kullanın:

```azcopy
.\azcopy sync "C:\local\path" "https://account.blob.core.windows.net/mycontainer1<sastoken>" --recursive=true
```

Aynı şekilde, bir Blob kapsayıcısına bir yerel dosya sistemi aşağı eşitleyebilirsiniz:

```azcopy
# If you're using Azure Active Directory authentication the sastoken is not required
.\azcopy sync "https://account.blob.core.windows.net/mycontainer1" "C:\local\path" --recursive=true
```

Komutu artımlı olarak üzerinde son değiştirilen zaman damgaları göre hedef kaynağı eşitleme sağlar. AzCopy v10 ekleyin ya da kaynak dosya silme, aynı hedef yapar. Silme işleminden önce dosyaları silmeyi onaylamak için AzCopy ister.

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

Eş zamanlı istek sayısını yapılandırın ve kaynak tüketimi ve işleme performansını denetlemek için AZCOPY_CONCURRENCY_VALUE ortam değişkenini ayarlayın. Değer 300 varsayılan olarak ayarlanır. Değer azaltma AzCopy v10 tarafından kullanılan CPU ve bant genişliği sınırlar.

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

## <a name="troubleshooting"></a>Sorun giderme

AzCopy v10 günlük ve tüm işler için plan dosyalarını oluşturur. Araştırmak ve olası sorunları gidermek için günlükleri'ni kullanabilirsiniz. Günlükleri (UPLOADFAILED COPYFAILED ve DOWNLOADFAILED), hata durumunu içerecek tam yolunu ve hatanın nedenini. İş günlüklerini ve planı dosyalarını % USERPROFILE bulunur\\Windows ya da $HOME .azcopy klasörü\\Mac ve Linux'ta .azcopy klasör.

> [!IMPORTANT]
> Bir destek isteği Microsoft Support (veya bir 3. taraf ilgili sorun giderme) Lütfen herkesle SAS sağlamak için yürütülecek çalıştığınız komut sonuç kısaltılmıştır sürümünü yanlışlıkla paylaşılmıyor paylaşımı gönderirken. Günlük dosyasının başında sonuç kısaltılmıştır sürümü bulabilirsiniz.

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
# If the value is blank then the default value is currently in use
```

### <a name="review-the-logs-for-errors"></a>Hatalar için günlükleri gözden geçirin

Aşağıdaki komutu 04dc9ca9-158f-7945-5933-564021086c79 günlüğünden UPLOADFAILED durumundaki tüm hataları alırsınız:

```azcopy
cat 04dc9ca9-158f-7945-5933-564021086c79.log | grep -i UPLOADFAILED
```

### <a name="view-and-resume-jobs"></a>İşleri görüntüleme ve devam et

Her bir aktarım işlemi AzCopy işi oluşturur. Aşağıdaki komutu kullanarak iş geçmişini görüntüleyebilirsiniz:

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

Tanımlayıcısını (güvenlik nedeniyle kalıcı değildir) bir SAS belirteci ile birlikte kullanarak başarısız/iptal işi devam edebilir:

```azcopy
.\azcopy jobs resume <jobid> --sourcesastokenhere --destinationsastokenhere
```

### <a name="change-the-default-log-level"></a>Varsayılan günlük düzeyi değiştirme

Varsayılan olarak, AzCopy günlük düzeyi bilgileri ayarlanır. Disk alanından kazanmak için günlük ayrıntı azaltmak istiyorsanız, ayarı kullanarak üzerine ``--log-level`` seçeneği. Kullanılabilir günlük düzeyleri şunlardır: Hata ayıklama, bilgi, uyarı, hata, PANİK ve önemli

## <a name="next-steps"></a>Sonraki adımlar

Geri bildiriminiz her zaman Hoş Geldiniz. Herhangi bir sorunuz varsa sorunları veya genel bir geri bildirim gönderin, bunları https://github.com/Azure/azure-storage-azcopy. Teşekkür ederiz!
