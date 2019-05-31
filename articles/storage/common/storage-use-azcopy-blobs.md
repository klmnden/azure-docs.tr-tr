---
title: AzCopy v10 kullanarak ya da Azure Blob depolamadan/depolamaya veri aktarımı | Microsoft Docs
description: Bu makale, AzCopy koleksiyonunu içerir. örnek yardımcı olan komutlar kapsayıcıları oluşturma, dosyaları kopyalayın ve klasör yerel dosya sistemleri ve kapsayıcılar arasında eşitleyin.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 05/14/2019
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: 98e33f838ee9b6f506bf1dc01e1dd61ad587aa05
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66299404"
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
> Örneğin: `https://<storage-account-name>.blob.core.windows.net/<container-name>?<SAS-token>"`.

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

AzCopy kullanabilirsiniz `make` bir kapsayıcı oluşturmak için komutu. Bu bölümdeki örneklerde adlı bir kapsayıcı oluşturun `mycontainer`.

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy make "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>` |
| **Örnek** | `azcopy make "https://mystorageaccount.blob.core.windows.net/mycontainer"` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy make "https://mystorageaccount.dfs.core.windows.net/mycontainer"` |

## <a name="upload-files"></a>Dosyaları karşıya yükleme

AzCopy kullanabilirsiniz `copy` dosyaları ve klasörleri yerel bilgisayarınızdan yüklemek için komutu.

Bu bölüm aşağıdaki örnekleri içerir:

> [!div class="checklist"]
> * Dosyayı karşıya yükleme
> * Bir klasörü karşıya yükleme
> * Joker karakterler kullanarak karşıya dosya yükleme

> [!NOTE]
> AzCopy otomatik olarak hesaplar ve dosya md5 karma kod saklamak değil. Bunu yapmak için AzCopy istiyorsanız, ardından ekleme `--put-md5` bayrağı her kopyalama komutu. Blob yüklendiğinde, bu şekilde, AzCopy bir MD5 karma değeri veri indirilir ve MD5 karma değeri blob içinde depolanan doğrular hesaplar `Content-md5` özelliği hesaplanan karma eşleşir.

### <a name="upload-a-file"></a>Dosyayı karşıya yükleme

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy cp "<local-file-path>" "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<blob-name>"` |
| **Örnek** | `azcopy copy "C:\myFolder\myTextFile.txt" "https://mystorageaccount.blob.core.windows.net/mycontainer/myTextFile.txt"` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy "C:\myFolder\myTextFile.txt" "https://mystorageaccount.dfs.core.windows.net/mycontainer/myTextFile.txt"` |

> [!NOTE]
> AzCopy varsayılan olarak, blok bloblarınızdan veri yükler. Ekleme Blobları ve sayfa Blobları bayrağını kullanırken dosyaları karşıya yüklemek için `--blob-type=[BlockBlob|PageBlob|AppendBlob]`.

### <a name="upload-a-folder"></a>Bir klasörü karşıya yükleme

Bu örnek, bir blob kapsayıcısı için bir klasör (ve tüm bu klasördeki dosyalar) kopyalar. Sonuç, aynı adı taşıyan bir kapsayıcıdaki bir klasördür.

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy copy "<local-folder-path>" "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>" --recursive` |
| **Örnek** | `azcopy copy "C:\myFolder" "https://mystorageaccount.blob.core.windows.net/mycontainer" --recursive` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy "C:\myFolder" "https://mystorageaccount.dfs.core.windows.net/mycontainer" --recursive` |

Kapsayıcı içindeki bir klasöre kopyalamak için yalnızca bu klasörün adını komut dizenizi belirtin.

|    |     |
|--------|-----------|
| **Örnek** | `azcopy copy "C:\myFolder" "https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobFolder" --recursive` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy "C:\myFolder" "https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobFolder" --recursive` |

Kapsayıcıda varolmayan bir klasörün adını belirtirseniz, AzCopy bu adla yeni bir klasör oluşturur.

### <a name="upload-the-contents-of-a-folder"></a>Bir klasörünün içeriği karşıya yükleme

(*) Joker karakter sembolünü kullanarak içeren klasöre kopyalama olmadan bir klasörünün içeriği karşıya yükleyebilirsiniz.

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy copy "<local-folder-path>\*" "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<folder-path>` |
| **Örnek** | `azcopy copy "C:\myFolder\*" "https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobFolder"` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy "C:\myFolder\*" "https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobFolder"` |

> [!NOTE]
> Append `--recursive` tüm alt klasörlerdeki dosyaları karşıya yüklemek için bayrak.

## <a name="download-files"></a>Dosyaları indirme

AzCopy kullanabilirsiniz `copy` bloblar, klasörler ve kapsayıcıları yerel bilgisayarınıza indirmek için komutu.

Bu bölüm aşağıdaki örnekleri içerir:

> [!div class="checklist"]
> * Dosya indirme
> * Bir klasöre indirin
> * Joker karakterler kullanarak dosyaları indirme

> [!NOTE]
> Varsa `Content-md5` bir blobun özellik değerini içeren bir karma, AzCopy indirilen veriler için bir MD5 karma değeri hesaplar ve MD5 karma değeri blob içinde depolanan doğrular `Content-md5` özelliği hesaplanan karma eşleşir. Bu değerler eşleşmiyorsa, ekleyerek bu davranışı geçersiz kılma sürece indirme başarısız `--check-md5=NoCheck` veya `--check-md5=LogOnly` kopyalama komutu.

### <a name="download-a-file"></a>Dosya indirme

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy copy "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<blob-path>" "<local-file-path>"` |
| **Örnek** | `azcopy copy "https://mystorageaccount.blob.core.windows.net/mycontainer/myTextFile.txt" "C:\myFolder\myTextFile.txt"` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy "https://mystorageaccount.dfs.core.windows.net/mycontainer/myTextFile.txt" "C:\myFolder\myTextFile.txt"` |

### <a name="download-a-folder"></a>Bir klasöre indirin

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy copy "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<folder-path>" "<local-folder-path>" --recursive` |
| **Örnek** | `azcopy copy "https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobFolder "C:\myFolder"  --recursive` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy "https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobFolder "C:\myFolder"  --recursive` |

Bu örnek adlı bir klasörde sonuçlanır `C:\myFolder\myBlobFolder` indirilen dosyaların tümünü içerir.

### <a name="download-the-contents-of-a-folder"></a>Bir klasörün içeriğini indirin

(*) Joker karakter sembolünü kullanarak içeren klasöre kopyalama olmadan bir klasörün içeriğini indirebilirsiniz.

> [!NOTE]
> Şu anda, bu senaryo, bir hiyerarşik ad alanı olmayan hesapları desteklenir.

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy copy "https://<storage-account-name>.blob.core.windows.net/<container-name>/*" "<local-folder-path>/"` |
| **Örnek** | `azcopy copy "https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobFolder/*" "C:\myFolder"` |

> [!NOTE]
> Append `--recursive` tüm alt klasörlerdeki dosyaları indirmek için bayrak.

## <a name="copy-blobs-between-storage-accounts"></a>Blobları depolama hesapları arasında kopyalama

Diğer depolama hesapları için BLOB'ları kopyalamak için AzCopy kullanabilirsiniz. Kopyalama işleminin zaman uyumlu olduğundan komutu geri döndüğünde, tüm dosyaları kopyalandığını gösterir.

> [!NOTE]
> Şu anda, bu senaryo, bir hiyerarşik ad alanı olmayan hesapları desteklenir.

AzCopy kullanan [URL'den blok yerleştirme](https://docs.microsoft.com/rest/api/storageservices/put-block-from-url) verileri doğrudan depolama sunucular arasında kopyalanır bu nedenle API. Bilgisayarınızın ağ bant genişliğini bu kopyalama işlemlerini kullanmayın.

Bu bölüm aşağıdaki örnekleri içerir:

> [!div class="checklist"]
> * Bir blobu başka bir depolama hesabına kopyalayın.
> * Başka bir depolama hesabına bir klasöre kopyalayın
> * Bir kapsayıcı başka bir depolama hesabına kopyalayın.
> * Tüm kapsayıcı, klasör ve dosyalarının, başka bir depolama hesabına kopyalayın

### <a name="copy-a-blob-to-another-storage-account"></a>Bir blobu başka bir depolama hesabına kopyalayın.

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy cp "https://<source-storage-account-name>.blob.core.windows.net/<container-name>/<blob-path>" "https://<destination-storage-account-name>.blob.core.windows.net/<container-name>/<blob-path>"` |
| **Örnek** | `azcopy cp "https://mysourceaccount.blob.core.windows.net/mycontainer/myTextFile.txt" "https://mydestinationaccount.blob.core.windows.net/mycontainer/myTextFile.txt"` |

### <a name="copy-a-folder-to-another-storage-account"></a>Başka bir depolama hesabına bir klasöre kopyalayın

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy cp "https://<source-storage-account-name>.blob.core.windows.net/<container-name>/<folder-path>" "https://<destination-storage-account-name>.blob.core.windows.net/<container-name>/<folder-path>" --recursive` |
| **Örnek** | `azcopy cp "https://mysourceaccount.blob.core.windows.net/mycontainer/myBlobFolder" "https://mydestinationaccount.blob.core.windows.net/mycontainer/myBlobFolder" --recursive` |

### <a name="copy-a-containers-to-another-storage-account"></a>Bir kapsayıcı başka bir depolama hesabına kopyalayın.

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy cp "https://<source-storage-account-name>.blob.core.windows.net/<container-name>" "https://<destination-storage-account-name>.blob.core.windows.net/<container-name>" --recursive` |
| **Örnek** | `azcopy cp "https://mysourceaccount.blob.core.windows.net/mycontainer" "https://mydestinationaccount.blob.core.windows.net/mycontainer" --recursive` |

### <a name="copy-all-containers-folders-and-files-to-another-storage-account"></a>Tüm kapsayıcı, klasör ve dosyalarının, başka bir depolama hesabına kopyalayın

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy cp "https://<source-storage-account-name>.blob.core.windows.net/" "https://<destination-storage-account-name>.blob.core.windows.net/" --recursive"` |
| **Örnek** | `azcopy cp "https://mysourceaccount.blob.core.windows.net" "https://mydestinationaccount.blob.core.windows.net" --recursive` |

## <a name="synchronize-files"></a>Dosyaları eşitle

Bir blob kapsayıcısı için bir yerel dosya sistemi içeriğini eşitleyebilirsiniz. Bu gibi durumlarda, bir blob kapsayıcısına bir yerel dosya sistemi Ayrıca bilgisayarınızda eşitleyebilirsiniz. Eşitleme tek yönlüdür. Diğer bir deyişle, bu iki uç nokta kaynak olduğu ve hangi bağlantılardan hedef seçin.

> [!NOTE]
> Güncel AzCopy sürümünü diğer kaynaklar ve hedefler arasında eşitleme değil (örneğin: Dosya depolama veya Amazon Web Services (AWS) S3 demet).

`sync` Karşılaştırır, dosya adları ve zaman damgaları son değiştirme komutu. Ayarlama `--delete-destination` isteğe bağlı bayrak değerini `true` veya `prompt` varsa bu dosyaların artık kaynak klasördeki dosyaları hedef klasöre silinemedi.

Ayarlarsanız `--delete-destination` bayrak `true` AzCopy bir istem sağlamadan dosyaları siler. AzCopy siler önce görünür bir dosya için bir istem istiyorsanız ayarlayın `--delete-destination` bayrak `prompt`.

> [!NOTE]
> Yanlışlıkla silinmekten önlemek için etkinleştirdiğinizden emin olun [geçici silme](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) kullanmadan önce özellik `--delete-destination=prompt|true` bayrağı.

### <a name="synchronize-a-container-to-a-local-file-system"></a>Yerel dosya sisteminde bir kapsayıcıya Eşitle

Bu durumda, yerel dosya sistemine kaynağı haline gelir ve hedef kapsayıcıdır.

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy sync "<local-folder-path>" "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>" --recursive` |
| **Örnek** | `azcopy sync "C:\myFolder" "https://mystorageaccount.blob.core.windows.net/mycontainer" --recursive` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy sync "C:\myFolder" "https://<storage-account-name>.dfs.core.windows.net/mycontainer" --recursive` |


### <a name="synchronize-a-local-file-system-to-a-container"></a>Bir kapsayıcıya yerel dosya sistemi Eşitle

Bu durumda, kapsayıcı kaynağı haline gelir ve hedef yerel dosya sistemidir.

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy sync "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>" "C:\myFolder" --recursive` |
| **Örnek** | `azcopy sync "https://mystorageaccount.blob.core.windows.net/mycontainer" "C:\myFolder" --recursive` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy sync "https://mystorageaccount.dfs.core.windows.net/mycontainer" "C:\myFolder" --recursive` |

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla örnek, tüm bu makaleler bulun:

- [Azcopy'i kullanmaya başlama](storage-use-azcopy-v10.md)

- [Öğretici: Bulut depolama azcopy komutunu kullanarak şirket içi verileri geçirme](storage-use-azcopy-migrate-on-premises-data.md)

- [AzCopy ve dosya depolama ile veri aktarma](storage-use-azcopy-files.md)

- [AzCopy ve Amazon S3 demetleri ile veri aktarma](storage-use-azcopy-s3.md)

- [Yapılandırma, en iyi duruma getirmek ve AzCopy sorunlarını giderme](storage-use-azcopy-configure.md)