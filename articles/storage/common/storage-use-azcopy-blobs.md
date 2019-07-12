---
title: AzCopy v10 kullanarak ya da Azure Blob depolamadan/depolamaya veri aktarımı | Microsoft Docs
description: Bu makale, AzCopy koleksiyonunu içerir. örnek yardımcı olan komutlar kapsayıcıları oluşturma, dosyaları kopyalayın ve yerel dosya sistemleri ve kapsayıcılar arasında dizinleri eşitleyin.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 05/14/2019
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: 83e32a1e8f77604330a9f3aba0e011a0a0851e2f
ms.sourcegitcommit: c0419208061b2b5579f6e16f78d9d45513bb7bbc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67625602"
---
# <a name="transfer-data-with-azcopy-and-blob-storage"></a>AzCopy ve Blob Depolama ile veri aktarma

AzCopy, verileri kopyalamak için ya da depolama hesapları arasında kullanabileceğiniz bir komut satırı yardımcı programıdır. Bu makale, Blob depolamayla çalışma örnek komutlar içerir.

## <a name="get-started"></a>başlarken

Bkz: [Azcopy'i kullanmaya başlama](storage-use-azcopy-v10.md) makale AzCopy indirip depolama hizmeti için yetkilendirme kimlik bilgilerini sağlayabilir yolları hakkında bilgi edinin.

> [!NOTE]
> Bu makaledeki örneklerde, kimlik bilgilerinizi kullanarak kimlik doğrulaması yaptınız olduğunu varsayın `AzCopy login` komutu. AzCopy, Azure AD hesabınızın sonra Blob depolama alanındaki verilere erişim yetkisi vermek için kullanır.
>
> Blob verilerine erişim yetkisi vermek için bir SAS belirteci yerine kullanacağınız, her AzCopy komutunu kaynak URL'ye belirtecini ekleyebilirsiniz.
>
> Örneğin: `https://<storage-account-name>.blob.core.windows.net/<container-name>?<SAS-token>"`

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

AzCopy kullanabilirsiniz `make` bir kapsayıcı oluşturmak için komutu. Bu bölümdeki örneklerde adlı bir kapsayıcı oluşturun `mycontainer`.

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy make "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>` |
| **Örnek** | `azcopy make "https://mystorageaccount.blob.core.windows.net/mycontainer"` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy make "https://mystorageaccount.dfs.core.windows.net/mycontainer"` |

## <a name="upload-files"></a>Dosyaları karşıya yükleme

AzCopy kullanabilirsiniz `copy` dosyaları ve dizinleri yerel bilgisayarınızdan yüklemek için komutu.

Bu bölüm aşağıdaki örnekleri içerir:

> [!div class="checklist"]
> * Dosyayı karşıya yükleme
> * Bir dizin karşıya yükleme
> * Joker karakterler kullanarak karşıya dosya yükleme

> [!NOTE]
> AzCopy otomatik olarak hesaplar ve dosya md5 karma kod saklamak değil. Bunu yapmak için AzCopy istiyorsanız, ardından ekleme `--put-md5` bayrağı her kopyalama komutu. Blob yüklendiğinde, bu şekilde, AzCopy bir MD5 karma değeri veri indirilir ve MD5 karma değeri blob içinde depolanan doğrular hesaplar `Content-md5` özelliği hesaplanan karma eşleşir.

### <a name="upload-a-file"></a>Dosyayı karşıya yükleme

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy cp "<local-file-path>" "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<blob-name>"` |
| **Örnek** | `azcopy copy "C:\myDirectory\myTextFile.txt" "https://mystorageaccount.blob.core.windows.net/mycontainer/myTextFile.txt"` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy "C:\myDirectory\myTextFile.txt" "https://mystorageaccount.dfs.core.windows.net/mycontainer/myTextFile.txt"` |

> [!NOTE]
> AzCopy varsayılan olarak, blok bloblarınızdan veri yükler. Ekleme Blobları ve sayfa Blobları bayrağını kullanırken dosyaları karşıya yüklemek için `--blob-type=[BlockBlob|PageBlob|AppendBlob]`.

### <a name="upload-a-directory"></a>Bir dizin karşıya yükleme

Bu örnek, bir blob kapsayıcısına bir dizin (ve tüm dosyaları dizindeki) kopyalar. Bir dizin aynı adı taşıyan bir kapsayıcıdaki sonucudur.

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy copy "<local-directory-path>" "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>" --recursive` |
| **Örnek** | `azcopy copy "C:\myDirectory" "https://mystorageaccount.blob.core.windows.net/mycontainer" --recursive` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy "C:\myDirectory" "https://mystorageaccount.dfs.core.windows.net/mycontainer" --recursive` |

Kapsayıcı içindeki bir dizine kopyalamak için yalnızca bu dizinin adını komut dizenizi belirtin.

|    |     |
|--------|-----------|
| **Örnek** | `azcopy copy "C:\myDirectory" "https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory" --recursive` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy "C:\myDirectory" "https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobDirectory" --recursive` |

Kapsayıcıda varolmayan bir dizinin adını belirtirseniz, AzCopy bu adla yeni bir dizin oluşturur.

### <a name="upload-the-contents-of-a-directory"></a>Bir dizinin içeriklerini karşıya yükleme

(*) Joker karakter sembolünü kullanarak içeren dizin kopyalama olmadan bir dizinin içeriklerini karşıya yükleyebilirsiniz.

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy copy "<local-directory-path>\*" "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<directory-path>` |
| **Örnek** | `azcopy copy "C:\myDirectory\*" "https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory"` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy "C:\myDirectory\*" "https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobDirectory"` |

> [!NOTE]
> Append `--recursive` tüm alt dizinlerde dosyaları karşıya yüklemek için bayrak.

## <a name="download-files"></a>Dosyaları indirme

AzCopy kullanabilirsiniz `copy` kapsayıcıları, dizinleri ve blobları yerel bilgisayarınıza indirmek için komutu.

Bu bölüm aşağıdaki örnekleri içerir:

> [!div class="checklist"]
> * Dosya indirme
> * Bir dizine indirin
> * Joker karakterler kullanarak dosyaları indirme

> [!NOTE]
> Varsa `Content-md5` bir blobun özellik değerini içeren bir karma, AzCopy indirilen veriler için bir MD5 karma değeri hesaplar ve MD5 karma değeri blob içinde depolanan doğrular `Content-md5` özelliği hesaplanan karma eşleşir. Bu değerler eşleşmiyorsa, ekleyerek bu davranışı geçersiz kılma sürece indirme başarısız `--check-md5=NoCheck` veya `--check-md5=LogOnly` kopyalama komutu.

### <a name="download-a-file"></a>Dosya indirme

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy copy "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<blob-path>" "<local-file-path>"` |
| **Örnek** | `azcopy copy "https://mystorageaccount.blob.core.windows.net/mycontainer/myTextFile.txt" "C:\myDirectory\myTextFile.txt"` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy "https://mystorageaccount.dfs.core.windows.net/mycontainer/myTextFile.txt" "C:\myDirectory\myTextFile.txt"` |

### <a name="download-a-directory"></a>Bir dizine indirin

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy copy "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<directory-path>" "<local-directory-path>" --recursive` |
| **Örnek** | `azcopy copy "https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory "C:\myDirectory"  --recursive` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy "https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobDirectory "C:\myDirectory"  --recursive` |

Bu örnek adlı bir dizinde sonuçlanır `C:\myDirectory\myBlobDirectory` indirilen dosyaların tümünü içerir.

### <a name="download-the-contents-of-a-directory"></a>Bir dizinin içeriğini indirin

Bir dizinin içeriklerini içeren dizin kopyalamadan (*) joker karakter sembolünü kullanarak indirebilirsiniz.

> [!NOTE]
> Şu anda, bu senaryo, bir hiyerarşik ad alanı olmayan hesapları desteklenir.

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy copy "https://<storage-account-name>.blob.core.windows.net/<container-name>/*" "<local-directory-path>/"` |
| **Örnek** | `azcopy copy "https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory/*" "C:\myDirectory"` |

> [!NOTE]
> Append `--recursive` tüm alt dizinlerde dosyaları indirmek için bayrak.

## <a name="copy-blobs-between-storage-accounts"></a>Blobları depolama hesapları arasında kopyalama

Diğer depolama hesapları için BLOB'ları kopyalamak için AzCopy kullanabilirsiniz. Kopyalama işleminin zaman uyumlu olduğundan komutu geri döndüğünde, tüm dosyaları kopyalandığını gösterir.

> [!NOTE]
> Şu anda, bu senaryo, bir hiyerarşik ad alanı olmayan hesapları desteklenir. 

AzCopy kullanan [URL'den blok yerleştirme](https://docs.microsoft.com/rest/api/storageservices/put-block-from-url) verileri doğrudan depolama sunucular arasında kopyalanır bu nedenle API. Bilgisayarınızın ağ bant genişliğini bu kopyalama işlemlerini kullanmayın.

Bu bölüm aşağıdaki örnekleri içerir:

> [!div class="checklist"]
> * Bir blobu başka bir depolama hesabına kopyalayın.
> * Başka bir depolama hesabı için bir dizini kopyalama
> * Bir kapsayıcı başka bir depolama hesabına kopyalayın.
> * Tüm kapsayıcıları, dizinleri ve dosyaları başka bir depolama hesabına kopyalayın.

> [!NOTE]
> Geçerli sürümde, her kaynak URL'ye bir SAS belirteci gerekir. Azure Active Directory (AD) kullanarak Yetkilendirme kimlik bilgilerini sağlarsanız, SAS belirteci yalnızca hedef URL'den atlayabilirsiniz. 

### <a name="copy-a-blob-to-another-storage-account"></a>Bir blobu başka bir depolama hesabına kopyalayın.

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy cp "https://<source-storage-account-name>.blob.core.windows.net/<container-name>/<blob-path>?<SAS-token>" "https://<destination-storage-account-name>.blob.core.windows.net/<container-name>/<blob-path>"` |
| **Örnek** | `azcopy cp "https://mysourceaccount.blob.core.windows.net/mycontainer/myTextFile.txt?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D" "https://mydestinationaccount.blob.core.windows.net/mycontainer/myTextFile.txt"` |

### <a name="copy-a-directory-to-another-storage-account"></a>Başka bir depolama hesabı için bir dizini kopyalama

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy cp "https://<source-storage-account-name>.blob.core.windows.net/<container-name>/<directory-path>?<SAS-token>" "https://<destination-storage-account-name>.blob.core.windows.net/<container-name>/<directory-path>" --recursive` |
| **Örnek** | `azcopy cp "https://mysourceaccount.blob.core.windows.net/mycontainer/myBlobDirectory?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D" "https://mydestinationaccount.blob.core.windows.net/mycontainer/myBlobDirectory" --recursive` |

### <a name="copy-a-containers-to-another-storage-account"></a>Bir kapsayıcı başka bir depolama hesabına kopyalayın.

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy cp "https://<source-storage-account-name>.blob.core.windows.net/<container-name>?<SAS-token>" "https://<destination-storage-account-name>.blob.core.windows.net/<container-name>" --recursive` |
| **Örnek** | `azcopy cp "https://mysourceaccount.blob.core.windows.net/mycontainer?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D" "https://mydestinationaccount.blob.core.windows.net/mycontainer" --recursive` |

### <a name="copy-all-containers-directories-and-files-to-another-storage-account"></a>Tüm kapsayıcıları, dizinleri ve dosyaları başka bir depolama hesabına kopyalayın.

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy cp "https://<source-storage-account-name>.blob.core.windows.net/?<SAS-token>" "https://<destination-storage-account-name>.blob.core.windows.net/" --recursive"` |
| **Örnek** | `azcopy cp "https://mysourceaccount.blob.core.windows.net?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D" "https://mydestinationaccount.blob.core.windows.net" --recursive` |

## <a name="synchronize-files"></a>Dosyaları eşitle

Bir yerel dosya sistemi içeriğini blob kapsayıcısı ile eşitleyebilirsiniz. Eşitleme tek yönlüdür. Diğer bir deyişle, bu iki uç nokta kaynak olduğu ve hangi bağlantılardan hedef seçin.

> [!NOTE]
> Şu anda, bu senaryo, bir hiyerarşik ad alanı olmayan hesapları desteklenir. Güncel AzCopy sürümünü diğer kaynaklar ve hedefler arasında eşitleme değil (örneğin: Dosya depolama veya Amazon Web Services (AWS) S3 demet).

`sync` Karşılaştırır, dosya adları ve zaman damgaları son değiştirme komutu. Ayarlama `--delete-destination` isteğe bağlı bayrak değerini `true` veya `prompt` dosyaları artık kaynak dizininde mevcutsa hedef dizinde dosyaları silmek için.

Ayarlarsanız `--delete-destination` bayrak `true` AzCopy bir istem sağlamadan dosyaları siler. AzCopy siler önce görünür bir dosya için bir istem istiyorsanız ayarlayın `--delete-destination` bayrak `prompt`.

> [!NOTE]
> Yanlışlıkla silinmekten önlemek için etkinleştirdiğinizden emin olun [geçici silme](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) kullanmadan önce özellik `--delete-destination=prompt|true` bayrağı.

### <a name="update-a-container-with-changes-to-a-local-file-system"></a>Bir yerel dosya sistemine yapılan değişikliklerle bir kapsayıcıyı güncelleştir

Bu durumda, hedef kapsayıcısıdır ve kaynak yerel dosya sistemidir.

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy sync "<local-directory-path>" "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>" --recursive` |
| **Örnek** | `azcopy sync "C:\myDirectory" "https://mystorageaccount.blob.core.windows.net/mycontainer" --recursive` |

### <a name="update-a-local-file-system-with-changes-to-a-container"></a>Bir kapsayıcıya değişikliklerle bir yerel dosya sistemi güncelleştirme

Bu durumda, hedef yerel dosya sistemidir ve kapsayıcı bir kaynaktır.

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy sync "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>" "C:\myDirectory" --recursive` |
| **Örnek** | `azcopy sync "https://mystorageaccount.blob.core.windows.net/mycontainer" "C:\myDirectory" --recursive` |
|

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla örnek, tüm bu makaleler bulun:

- [Azcopy'i kullanmaya başlama](storage-use-azcopy-v10.md)

- [Öğretici: Bulut depolama azcopy komutunu kullanarak şirket içi verileri geçirme](storage-use-azcopy-migrate-on-premises-data.md)

- [AzCopy ve dosya depolama ile veri aktarma](storage-use-azcopy-files.md)

- [AzCopy ve Amazon S3 demetleri ile veri aktarma](storage-use-azcopy-s3.md)

- [Yapılandırma, en iyi duruma getirmek ve AzCopy sorunlarını giderme](storage-use-azcopy-configure.md)
