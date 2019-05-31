---
title: Amazon S3 demetten Azure depolama AzCopy v10 kullanarak veri aktarımı | Microsoft Docs
description: AzCopy ve Amazon S3 demetleri ile veri aktarma
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 04/23/2019
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: c0bc74ac0fe45f2502064340a0c3ce5b82694b06
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66245707"
---
# <a name="copy-data-from-amazon-s3-buckets-by-using-azcopy"></a>Azcopy komutunu kullanarak, Amazon S3 demetten veri kopyalama

AzCopy, BLOB veya dosyalar için veya bir depolama hesabına kopyalamak için kullanabileceğiniz bir komut satırı yardımcı programıdır. Bu makalede, yardımcı kopyalama nesneleri, klasörler ve demet Amazon Web Services (AWS) S3'ten Azure blob depolama için azcopy komutunu kullanarak.

## <a name="choose-how-youll-provide-authorization-credentials"></a>Yetkilendirme kimlik bilgilerini nasıl sağlarız seçin

* Azure depolama ile yetkilendirmek için Azure Active Directory (AD) veya paylaşılan erişim imzası (SAS) belirteci kullanın.

* AWS S3 ile yetkilendirmek için bir AWS erişim anahtarı ve gizli erişim anahtarı'nı kullanın.

### <a name="authorize-with-azure-storage"></a>Azure depolama ile yetkilendirme

Bkz: [Azcopy'i kullanmaya başlama](storage-use-azcopy-v10.md) AzCopy indirmek için makale ve yetkilendirme kimlik bilgilerini depolama hizmetine nasıl sağlarız seçin.

> [!NOTE]
> Bu makaledeki örneklerde, kimlik bilgilerinizi kullanarak kimlik doğrulaması yaptınız olduğunu varsayın `AzCopy login` komutu. AzCopy, Azure AD hesabınızın sonra Blob depolama alanındaki verilere erişim yetkisi vermek için kullanır.
>
> Blob verilerine erişim yetkisi vermek için bir SAS belirteci yerine kullanacağınız, her AzCopy komutunu kaynak URL'ye belirtecini ekleyebilirsiniz.
>
> Örneğin: `https://mystorageaccount.blob.core.windows.net/mycontainer?<SAS-token>`.

### <a name="authorize-with-aws-s3"></a>AWS S3 ile yetkilendirme

AWS erişim anahtarı ve gizli erişim anahtarı toplayın ve ardından bu ortam değişkenleri:

| İşletim sistemi | Komut  |
|--------|-----------|
| **Windows** | `set AWS_ACCESS_KEY_ID=<access-key>`<br>`set AWS_SECRET_ACCESS_KEY=<secret-access-key>` |
| **Linux** | `export AWS_ACCESS_KEY_ID=<access-key>`<br>`export AWS_SECRET_ACCESS_KEY=<secret-access-key>` |
| **MacOS** | `export AWS_ACCESS_KEY_ID=<access-key>`<br>`export AWS_SECRET_ACCESS_KEY=<secret-access-key>`|

## <a name="copy-objects-folders-and-buckets"></a>Nesneleri, klasörler ve demet kopyalayın

AzCopy kullanan [URL'den blok yerleştirme](https://docs.microsoft.com/rest/api/storageservices/put-block-from-url) verileri doğrudan AWS S3 ve depolama sunucuları arasında kopyalanır bu nedenle API. Bilgisayarınızın ağ bant genişliğini bu kopyalama işlemlerini kullanmayın.

### <a name="copy-an-object"></a>Bir nesneyi kopyalama

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy cp "https://s3.amazonaws.com/<bucket-name>/<object-name>" "https://<storage-account-name>.blob.core.windows.net/<container-name>/<blob-name>"` |
| **Örnek** | `azcopy cp "https://s3.amazonaws.com/mybucket/myobject" "https://mystorageaccount.blob.core.windows.net/mycontainer/myblob"` |

> [!NOTE]
> Bu makaledeki örneklerde, AWS S3 demetlerine için yolu stili URL'leri kullanın (örneğin: `http://s3.amazonaws.com/<bucket-name>`). 
>
> Sanal barındırılan stili URL'leri de kullanabilirsiniz (örneğin: `http://bucket.s3.amazonaws.com`). 
>
> Sanal makineyle kovaları hakkında daha fazla bilgi edinmek için [sanal barındırma, demet] bakın] (https://docs.aws.amazon.com/AmazonS3/latest/dev/VirtualHosting.html).

### <a name="copy-a-folder"></a>Bir klasöre kopyalayın

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy cp "https://s3.amazonaws.com/<bucket-name>/<folder-name>" "https://<storage-account-name>.blob.core.windows.net/<container-name>/<folder-name>" --recursive=true` |
| **Örnek** | `azcopy cp "https://s3.amazonaws.com/mybucket/myfolder" "https://mystorageaccount.blob.core.windows.net/mycontainer/myfolder" --recursive=true` |

### <a name="copy-a-bucket"></a>Bir demet kopyalayın

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy cp "https://s3.amazonaws.com/<bucket-name>" "https://<storage-account-name>.blob.core.windows.net/<container-name>" --recursive=true` |
| **Örnek** | `azcopy cp "https://s3.amazonaws.com/mybucket" "https://mystorageaccount.blob.core.windows.net/mycontainer" --recursive=true` |

### <a name="copy-all-buckets-in-all-regions"></a>Tüm bölgelerde tüm aralıkları Kopyala

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy cp "https://s3.amazonaws.com/" "https://<storage-account-name>.blob.core.windows.net" --recursive=true` |
| **Örnek** | `azcopy cp "https://s3.amazonaws.com" "https://mystorageaccount.blob.core.windows.net" --recursive=true` |

### <a name="copy-all-buckets-in-a-specific-s3-region"></a>Belirli bir S3 bölgede tüm demetleri kopyalayın

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy cp "https://s3-<region-name>.amazonaws.com/" "https://<storage-account-name>.blob.core.windows.net" --recursive=true` |
| **Örnek** | `azcopy cp "https://s3-rds.eu-north-1.amazonaws.com" "https://mystorageaccount.blob.core.windows.net" --recursive=true` |

## <a name="handle-differences-in-object-naming-rules"></a>İşleme farklılıkları nesne adlandırma kuralları

AWS S3 farklı bir Azure blob kapsayıcıları karşılaştırıldığında demet adları için adlandırma kuralları kümesi vardır. Bunlar hakkında bilgi edinebilirsiniz [burada](https://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html#bucketnamingrules). Demetleri bir grup için bir Azure depolama hesabına kopyalamak seçerseniz, kopyalama işlemi farklar adlandırma nedeniyle başarısız olabilir.

AzCopy oluşabilecek sorunların en yaygın iki işler; nokta ve art arda kısa çizgi içeren demetler içeren demetler. AWS S3 demet adları nokta ve art arda kısa çizgi içerebilir, ancak azure'da kapsayıcı olamaz. AzCopy, kısa çizgi ve art arda kısa çizgi sayısını temsil eden bir sayı ile art arda kısa çizgi ile nokta değiştirir (örneğin: adlı bir demet `my----bucket` olur `my-4-bucket`. 

Ayrıca, AzCopy dosyalarını kopyalar gibi adlandırma çakışmaları için denetler ve bunları çözümlemeye çalışır. Örneğin adıyla demetlerine ise `bucket-name` ve `bucket.name`, AzCopy çözümler adlı bir demet `bucket.name` öncelikle `bucket-name` ve sonra `bucket-name-2`.

## <a name="handle-differences-in-object-metadata"></a>Nesne meta verilerini tanıtıcı farklılıkları

Farklı karakter kümelerini, AWS S3 ve Azure Nesne anahtarları adlarında izin. AWS S3 kullandığını karakterler hakkında okuyabilirsiniz [burada](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingMetadata.html#object-keys). Azure tarafında, blob Nesne anahtarları adlandırma kurallarına uymaları [ C# tanımlayıcıları](https://docs.microsoft.com/dotnet/csharp/language-reference/).

AzCopy bir parçası olarak `copy` komutu için bir değer sağlayabilirsiniz isteğe bağlı `s2s-invalid-metadata-handle` burada dosyanın meta verilerini içeren uyumsuz anahtar adları dosyalarını işlemek nasıl istediğinizi belirten bayrak. Aşağıdaki tabloda her bir bayrak değeri açıklanmaktadır.

| Bayrak değeri | Açıklama  |
|--------|-----------|
| **ExcludeIfInvalid** | (Varsayılan seçenek) Meta veriler aktarılan nesnesinde yer almıyor. AzCopy, bir uyarı günlüğe kaydeder. |
| **FailIfInvalid** | Nesneler kopyalanmaz. AzCopy, bir hatayı günlüğe kaydeder ve aktarım özetinde görüntülenen başarısız sayısı bu hatayı içerir.  |
| **RenameIfInvalid**  | AzCopy, geçersiz meta verileri anahtar çözümler ve çözümlenen meta verileri anahtar değer çifti kullanarak Azure'a nesnesini kopyalar. Tam olarak hangi nesne anahtarları yeniden adlandırmak için AzCopy alır adımları öğrenmek için bkz [nasıl AzCopy Nesne anahtarları yeniden adlandırır](#rename-logic) bölümüne bakın. AzCopy anahtarı yeniden adlandıramadı ise, nesne kopyalanmaz. |

<a id="rename-logic" />

### <a name="how-azcopy-renames-object-keys"></a>AzCopy Nesne anahtarları nasıl yeniden adlandırır

AzCopy, aşağıdaki adımları gerçekleştirir:

1. Geçersiz karakter '_' ile değiştirir.

2. Dizesi `rename_` ' dan itibaren geçerli olan yeni bir anahtar.

   Bu anahtar, özgün metaverileri gereğince kaydetmek için kullanılacak **değer**.

3. Dizesi `rename_key_` ' dan itibaren geçerli olan yeni bir anahtar.
   Bu anahtarı geçersiz özgün metaverileri gereğince kaydetmek için kullanılan **anahtar**.
   Bu anahtar, deneyin ve Blob Depolama hizmetinin bir değeri olarak meta verileri anahtar korunur beri Azure tarafındaki meta verileri kurtarmak için kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla örnek, tüm bu makaleler bulun:

- [Azcopy'i kullanmaya başlama](storage-use-azcopy-v10.md)

- [AzCopy ve blob depolama ile veri aktarma](storage-use-azcopy-blobs.md)

- [AzCopy ve dosya depolama ile veri aktarma](storage-use-azcopy-files.md)

- [Yapılandırma, en iyi duruma getirmek ve AzCopy sorunlarını giderme](storage-use-azcopy-configure.md)