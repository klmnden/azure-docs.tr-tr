---
title: Yapılandırma, en iyi duruma getirmek ve Azure Storage ile AzCopy sorunlarını giderme | Microsoft Docs
description: Yapılandırma, en iyi duruma getirmek ve AzCopy giderebilirsiniz.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 05/14/2019
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: a160591ef0a47eed097ce8db373878f32965de9b
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66247132"
---
# <a name="configure-optimize-and-troubleshoot-azcopy"></a>Yapılandırma, en iyi duruma getirmek ve AzCopy sorunlarını giderme

AzCopy, BLOB veya dosyalar için veya bir depolama hesabına kopyalamak için kullanabileceğiniz bir komut satırı yardımcı programıdır. Bu makalede, Gelişmiş yapılandırma görevleri gerçekleştirmenize yardımcı olur ve AzCopy kullanırken ortaya çıkabilecek sorunları gidermenize yardımcı olur.

> [!NOTE]
> AzCopy ile çalışmaya başlamanıza yardımcı olmak için içeriği arıyorsanız aşağıdaki makalelerden herhangi birine bakın:
> - [Azcopy'i kullanmaya başlama](storage-use-azcopy-v10.md)
> - [AzCopy ve blob depolama ile veri aktarma](storage-use-azcopy-blobs.md)
> - [AzCopy ve dosya depolama ile veri aktarma](storage-use-azcopy-files.md)
> - [AzCopy ve Amazon S3 demetleri ile veri aktarma](storage-use-azcopy-s3.md)

## <a name="configure-proxy-settings"></a>Proxy ayarlarını yapılandırma

AzCopy için proxy ayarlarını yapılandırmak için `https_proxy` ortam değişkeni.

| İşletim sistemi | Komut  |
|--------|-----------|
| **Windows** | `set https_proxy=<proxy IP>:<proxy port>` |
| **Linux** | `export https_proxy=<proxy IP>:<proxy port>` |
| **MacOS** | `export https_proxy=<proxy IP>:<proxy port>` |

Şu anda, AzCopy NTLM veya Kerberos kimlik doğrulaması gerektiren proxy'leri desteklemez.

## <a name="optimize-throughput"></a>Aktarım hızını iyileştirme

Ayarlama `AZCOPY_CONCURRENCY_VALUE` eşzamanlı istek sayısını yapılandırmak ve aktarım hızı performans ve kaynak tüketimi denetlemek için ortam değişkeni. Bilgisayarınızı 5'ten az CPU sahip sonra bu değişkenin değerini ayarlamak `32`. Aksi takdirde, varsayılan değer 16 CPU sayı ile çarpılan eşittir. Bu değişken varsayılan en fazla değeri `300`, ancak daha yüksek veya düşük değer el ile ayarlayabilirsiniz.

| İşletim sistemi | Komut  |
|--------|-----------|
| **Windows** | `set AZCOPY_CONCURRENCY_VALUE=<value>` |
| **Linux** | `export AZCOPY_CONCURRENCY_VALUE=<value>` |
| **MacOS** | `export AZCOPY_CONCURRENCY_VALUE=<value>` |

Kullanım `azcopy env` bu değişkenin geçerli değeri denetlemek için.  Değer boşsa, sonra `AZCOPY_CONCURRENCY_VALUE` değişkeni varsayılan değerine ayarlanmış `300`.

## <a name="change-the-location-of-the-log-files"></a>Günlük dosyalarının konumunu değiştirin

Varsayılan olarak, günlük dosyalarının yer `%USERPROFILE\\.azcopy` klasör veya Windows üzerindeki `$HOME\\.azcopy` Mac ve Linux üzerinde klasör. Şu komutları kullanarak ihtiyacınız varsa, bu konuma değiştirebilirsiniz.

| İşletim sistemi | Komut  |
|--------|-----------|
| **Windows** | `set AZCOPY_LOG_LOCATION=<value>` |
| **Linux** | `export AZCOPY_LOG_LOCATION=<value>` |
| **MacOS** | `export AZCOPY_LOG_LOCATION=<value>` |

Kullanım `azcopy env` bu değişkenin geçerli değeri denetlemek için. Ardından, değer boş ise, günlükleri varsayılan konuma yazılır.

## <a name="change-the-default-log-level"></a>Varsayılan günlük düzeyi değiştirme

Varsayılan olarak, AzCopy günlük düzeyini ayarlamak `INFO`. Disk alanından kazanmak için günlük ayrıntı azaltmak istiyorsanız, bu ayarı kullanarak üzerine ``--log-level`` seçeneği. 

Kullanılabilir günlük düzeyleri: `DEBUG`, `INFO`, `WARNING`, `ERROR`, `PANIC`, ve `FATAL`.

## <a name="troubleshoot-issues"></a>Sorunları giderme

AzCopy günlük ve planı dosyaları her işi oluşturur. Araştırmak ve olası sorunları gidermek için günlükleri'ni kullanabilirsiniz. 

Günlükleri hata durumunu içerir (`UPLOADFAILED`, `COPYFAILED`, ve `DOWNLOADFAILED`), tam yolunu ve hatanın nedenini.

Varsayılan olarak, günlük ve planı dosyaları yer `%USERPROFILE\\.azcopy` Windows klasöründe veya `$HOME\\.azcopy` Mac ve Linux üzerinde klasör.

> [!IMPORTANT]
> Microsoft Support (veya herhangi bir üçüncü taraf ilgili sorun giderme), bir isteği gönderirken çalıştırmak istediğiniz komutun sonuç kısaltılmıştır sürümü paylaşabilir. Bu, SAS yanlışlıkla herkesle paylaşılmaz sağlar. Günlük dosyasının başında sonuç kısaltılmıştır sürümü bulabilirsiniz.

### <a name="review-the-logs-for-errors"></a>Hatalar için günlükleri gözden geçirin

Aşağıdaki komut, tüm hataları alırsınız `UPLOADFAILED` durumundan `04dc9ca9-158f-7945-5933-564021086c79` günlüğü:

**Windows**

```
Select-String UPLOADFAILED .\04dc9ca9-158f-7945-5933-564021086c79.log
```

**Linux**

```
grep UPLOADFAILED .\04dc9ca9-158f-7945-5933-564021086c79.log
```

### <a name="view-and-resume-jobs"></a>İşleri görüntüleme ve devam et

Her bir aktarım işlemi AzCopy işi oluşturur. İş geçmişini görüntülemek için aşağıdaki komutu kullanın:

```
azcopy jobs list
```

İş istatistiklerini görüntülemek için aşağıdaki komutu kullanın:

```
azcopy jobs show <job-id>
```

Aktarımları durumuna göre filtre uygulamak için aşağıdaki komutu kullanın:

```
azcopy jobs show <job-id> --with-status=Failed
```

Başarısız/iptal bir işi sürdürmek için aşağıdaki komutu kullanın. Güvenlik nedeniyle kalıcı değil olarak bu komut tanımlayıcısını SAS belirteci ile birlikte kullanır:

```
azcopy jobs resume <job-id> --source-sas="<sas-token>"
azcopy jobs resume <job-id> --destination-sas="<sas-token>"
```

Bir iş devam edince, AzCopy iş planı dosyasını arar. Plan dosyası, proje ilk oluşturulduğunda işlemek için tanımlanmış olan tüm dosyaları listeler. Bir iş devam edince, AzCopy aktarılan olmayan planı dosyasında listelenen dosyaların tümünü aktarım dener.