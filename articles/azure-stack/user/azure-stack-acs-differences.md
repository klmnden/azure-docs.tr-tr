---
title: Azure stack depolama farklılıklar ve dikkat edilmesi gerekenler | Microsoft Docs
description: Azure stack depolama ve Azure Stack dağıtım konuları yanı sıra Azure depolama arasındaki farkları.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/30/2019
ms.author: mabrigg
ms.reviwer: xiaofmao
ms.lastreviewed: 01/30/2019
ms.openlocfilehash: bbf5076c195fde6a7c5fcabd8e347b7a0d433e8f
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2019
ms.locfileid: "57763258"
---
# <a name="azure-stack-storage-differences-and-considerations"></a>Azure Stack Depolama: Farklılıklar ve dikkat edilmesi gerekenler

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack depolama Microsoft Azure stack'teki depolama bulut hizmetleri kümesidir. Azure Stack depolama, blob, tablo, kuyruk ve Azure ile tutarlı semantiğine sahip hesabı yönetimi işlevselliğini sağlar.

Bu makalede, Azure depolama hizmetlerinde bilinen Azure Stack depolama farklar özetlenmektedir. Ayrıca, Azure Stack dağıtırken göz önünde bulundurulması gerekenler listelenir. Global Azure ve Azure Stack arasında üst düzey farklılıklar hakkında bilgi edinmek için bkz. [anahtar konuları](azure-stack-considerations.md) makalesi.

## <a name="cheat-sheet-storage-differences"></a>Kopya kağıdı: Depolama farkları

| Özellik | Azure (Genel) | Azure Stack |
| --- | --- | --- |
|Dosya depolama|Desteklenen bulut tabanlı SMB dosya paylaşımları|Henüz desteklenmiyor
|Bekleyen veri için Azure depolama hizmeti şifrelemesi|256 bit AES şifreleme. Anahtar Kasası'nda müşteri tarafından yönetilen anahtarları kullanarak şifreleme destekler.|BitLocker'ı 128 bit AES şifreleme. Müşteri tarafından yönetilen anahtarları kullanarak şifreleme desteklenmiyor.
|Depolama hesabı türü|Genel amaçlı V1, V2 ve Blob Depolama hesapları|Yalnızca genel amaçlı V1.
|Çoğaltma seçenekleri|Yerel olarak yedekli depolama, coğrafi olarak yedekli depolama, okuma erişimli coğrafi olarak yedekli depolama ve bölgesel olarak yedekli depolama|Yerel olarak yedekli depolama.
|Premium depolama|Tam olarak desteklenir|Garanti ya da performans sınır sağlanabilir.
|Yönetilen diskler|Premium ve standart desteklenir|1808 veya sonraki bir sürümü kullandığınızda desteklenir.
|Blob adı|1024 karakter (2.048 bayt)|880 karakterleri (1,760 bayt)
|Blok blobu en büyük boyutu|4,75 TB (100 MB X 50.000 blok)|1802 güncelleştirme veya yeni bir sürümü için 4,75 TB (100 MB x 50.000 blok). Önceki sürümler için 50.000 x 4 MB (yaklaşık 195 GB).
|Sayfa blob anlık görüntü kopyalama|Desteklenen çalışan bir VM'ye bağlı yedekleme Azure yönetilmeyen VM diskleri|Henüz desteklenmiyor.
|Sayfa blob artımlı anlık görüntü kopyalama|Premium ve standart Azure sayfa blobları desteklenir|Henüz desteklenmiyor.
|Sayfa blob faturalandırma|Blob veya anlık görüntü olup benzersiz sayfaları için ücret geçerli olur. Temel blob güncelleştirilmesini kadar blob ile ilişkili anlık görüntüler için ek ücretleri ödemesi gerekir değil.|Temel blob ve assiociated anlık görüntüler için ücret geçerli olur. Tek tek her anlık görüntü için ek ücretleri ödemesi gerekir.
|Blob depolama için depolama katmanları|Sık erişimli, seyrek erişimli ve Arşiv depolama katmanları.|Henüz desteklenmiyor.
|Blob depolama için geçici silme|Genel kullanımda|Henüz desteklenmiyor.
|Sayfa blob en büyük boyutu|8 TB|1 TB
|Sayfa blob sayfa boyutu|512 bayt|4 KB
|Tablo bölüm anahtarını ve satır boyutu anahtarı.|1024 karakter (2.048 bayt)|400 karakter (800 bayt)
|BLOB anlık görüntüsü|Bir blobun anlık görüntüleri maksimum sayısı sınırlı değildir.|Bir blobun anlık görüntüleri maksimum sayısı 1000'dir.
|Depolama için Azure AD kimlik doğrulaması|Önizleme aşamasında|Henüz desteklenmiyor.
|Sabit BLOB'ları|Genel kullanımda|Henüz desteklenmiyor.
|Güvenlik Duvarı ve depolama için sanal ağ kuralları|Genel kullanımda|Henüz desteklenmiyor.|

Depolama ölçümleri farklılıklar vardır:

* Depolama ölçümleri işlem verilerinde iç veya dış ağ bant genişliği ayırmaz.
* Depolama ölçümleri işlem verileri, bağlama diskleri için sanal makine erişimini içermez.

## <a name="api-version"></a>API sürümü

Azure Stack depolama ile aşağıdaki sürümleri destekler:

Azure depolama API Hizmetleri:

1811 güncelleştirmesi veya daha yeni sürümleri:

- [2017-11-09](https://docs.microsoft.com/rest/api/storageservices/version-2017-11-09)
- [2017-07-29](https://docs.microsoft.com/rest/api/storageservices/version-2017-07-29)
- [2017-04-17](https://docs.microsoft.com/rest/api/storageservices/version-2017-04-17)
- [2016-05-31](https://docs.microsoft.com/rest/api/storageservices/version-2016-05-31)
- [2015-12-11](https://docs.microsoft.com/rest/api/storageservices/version-2015-12-11)
- [2015-07-08](https://docs.microsoft.com/rest/api/storageservices/version-2015-07-08)
- [2015-04-05](https://docs.microsoft.com/rest/api/storageservices/version-2015-04-05)

Önceki sürümler:

- [2017-04-17](https://docs.microsoft.com/rest/api/storageservices/version-2017-04-17)
- [2016-05-31](https://docs.microsoft.com/rest/api/storageservices/version-2016-05-31)
- [2015-12-11](https://docs.microsoft.com/rest/api/storageservices/version-2015-12-11)
- [2015-07-08](https://docs.microsoft.com/rest/api/storageservices/version-2015-07-08)
- [2015-04-05](https://docs.microsoft.com/rest/api/storageservices/version-2015-04-05)

Azure depolama hizmet yönetimi API'ları:

1811 güncelleştirmesi veya daha yeni sürümleri:

- [2017-10-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
- [2017-06-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
- [2016-12-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
- [2016-05-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
- [2016-01-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
- [2015-06-15](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
- [2015-05-01-Önizleme](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)

Önceki sürümler:

- [2016-01-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
- [2015-06-15](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
- [2015-05-01-Önizleme](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)

Azure Stack desteklenen depolama istemci kitaplıkları hakkında daha fazla bilgi için bkz: [Azure Stack depolama geliştirme araçları ile çalışmaya başlama](azure-stack-storage-dev.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stack depolama geliştirme araçları ile çalışmaya başlama](azure-stack-storage-dev.md)
* [Azure Stack depolama için veri aktarım araçları kullanın](azure-stack-storage-transfer.md)
* [Azure Stack depolamaya giriş](azure-stack-storage-overview.md)
