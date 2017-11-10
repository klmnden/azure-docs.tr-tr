---
title: "Azure yığın depolama: Farklar ve ilgili önemli noktalar"
description: "Azure yığın depolama ve Azure Storage arasındaki farklar Azure yığın dağıtımında dikkat edilecek noktalar birlikte anlayın."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
ms.reviwer: xiaofmao
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/08/2017
ms.author: jeffgilb
ms.openlocfilehash: 1dc099fa234e217b682c88f2214fe271c916eec2
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2017
---
# <a name="azure-stack-storage-differences-and-considerations"></a>Azure yığın depolama: Farklar ve ilgili önemli noktalar

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure yığın depolama Microsoft Azure yığın depolama bulut hizmetleri kümesidir. Azure yığın depolama blob, tablo, kuyruk ve Azure tutarlı semantiği ile hesabı yönetimi işlevselliğini sağlar.

Bu makalede, Azure depolama biriminden bilinen Azure yığın depolama farkları özetler. Ayrıca, Azure yığın dağıtırken göz önünde bulundurmanız gereken diğer noktalar özetlenmektedir. Azure yığını ve Azure arasında üst düzey farklılıklar hakkında bilgi edinmek için [anahtar konuları](azure-stack-considerations.md) konu.

## <a name="cheat-sheet-storage-differences"></a>Kopya sayfası: depolama farklar

| Özellik | Azure (Genel) | Azure Stack |
| --- | --- | --- |
|Dosya depolama|Bulut tabanlı SMB dosya paylaşımları desteklenen|Henüz desteklenmiyor
|Bekleyen Veri için Azure Storage Hizmeti Şifreleme|256 bit AES şifreleme|Henüz desteklenmiyor
|Depolama hesabı türü|Genel amaçlı ve Azure Blob storage hesapları|Genel amaçlı yalnızca
|Çoğaltma seçenekleri|Yerel olarak yedekli depolama, coğrafi olarak yedekli depolama, coğrafi olarak yedekli depolamaya okuma erişimi ve bölge olarak yedekli depolama|Yerel olarak yedekli depolama
|Premium depolama|Tam olarak desteklenir|Performans sınır sağlanabilir veya garanti
|Yönetilen diskler|Premium ve standart desteklenen|Henüz desteklenmiyor
|BLOB adı|1024 karakter (2.048 bayt)|880 karakterleri (1,760 bayt)
|Blok blobu en büyük boyutu|4.75 TB (100 MB X 50.000 blok)|50.000 x 4 MB (yaklaşık 195 GB)
|Sayfa blob anlık görüntü kopyalama|Desteklenen bir çalışan VM'ye ekli yedekleme Azure yönetilmeyen VM diskleri|Henüz desteklenmiyor
|Sayfa blob artımlı anlık görüntü kopyalama|Premium ve desteklenen standart Azure sayfa BLOB'ları|Henüz desteklenmiyor
|Sayfa blob en büyük boyutu|8 TB|1 TB
|Sayfa blob sayfa boyutu|512 bayt|4 KB
|Tablo bölüm anahtarını ve satır anahtar boyutu|1024 karakter (2.048 bayt)|400 karakter (800 bayt)

### <a name="metrics"></a>Ölçümler
Depolama ölçümleri ile bazı farklar vardır:
* Depolama ölçümleri işlem verilerinde iç veya dış ağ bant genişliği ayırt etmez.
* Depolama ölçümleri işlem verilerde bağlı diskler sanal makine erişimi içermez.

## <a name="api-version"></a>API sürümü
Aşağıdaki sürümler ile Azure yığın depolama desteklenir:

* Azure Storage Veri Hizmetleri: [2015-04-05 REST API sürümü](https://docs.microsoft.com/rest/api/storageservices/Version-2015-04-05?redirectedfrom=MSDN)
* Azure Storage Yönetimi Hizmetleri: [2015-05-01-Önizleme, 2015-06-15 ve 2016-01-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN) 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure yığın depolama geliştirme araçları ile çalışmaya başlama](azure-stack-storage-dev.md)
* [Azure yığın depolama giriş](azure-stack-storage-overview.md)

