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
ms.date: 02/21/2017
ms.author: jeffgilb
ms.openlocfilehash: 7c4f030018f388302c3b60a41086bbd97c86513d
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="azure-stack-storage-differences-and-considerations"></a>Azure yığın depolama: Farklar ve ilgili önemli noktalar

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure yığın depolama Microsoft Azure yığın depolama bulut hizmetleri kümesidir. Azure yığın depolama blob, tablo, kuyruk ve Azure tutarlı semantiği ile hesabı yönetimi işlevselliğini sağlar.

Bu makalede, Azure depolama biriminden bilinen Azure yığın depolama farkları özetler. Ayrıca, Azure yığın dağıtırken göz önünde bulundurmanız gereken diğer noktalar özetlenmektedir. Azure yığını ve Azure arasında üst düzey farklılıklar hakkında bilgi edinmek için [anahtar konuları](azure-stack-considerations.md) konu.

## <a name="cheat-sheet-storage-differences"></a>Kopya sayfası: depolama farklar

| Özellik | Azure (Genel) | Azure Stack |
| --- | --- | --- |
|Dosya depolama|Bulut tabanlı SMB dosya paylaşımları desteklenen|Henüz desteklenmiyor
|Bekleyen Veri için Azure Storage Hizmeti Şifreleme|256 bit AES şifreleme|BitLocker 128 bit AES şifreleme
|Depolama hesabı türü|Genel amaçlı ve Azure Blob storage hesapları|Genel amaçlı yalnızca.
|Çoğaltma seçenekleri|Yerel olarak yedekli depolama, coğrafi olarak yedekli depolama, coğrafi olarak yedekli depolamaya okuma erişimi ve bölge olarak yedekli depolama|Yerel olarak yedekli depolama.
|Premium depolama|Tam olarak desteklenir|Performans sınır sağlanabilir veya garanti.
|Yönetilen diskler|Premium ve standart desteklenen|Henüz desteklenmiyor.
|Blob adı|1024 karakter (2.048 bayt)|880 karakterleri (1,760 bayt)
|Blok blobu en büyük boyutu|4.75 TB (100 MB X 50.000 blok)|1802 güncelleştirme veya daha yeni bir sürümü için 4.75 TB (100 MB x 50.000 blok). Önceki sürümler için 50.000 x 4 MB (yaklaşık 195 GB'den).
|Sayfa blob anlık görüntü kopyalama|Desteklenen bir çalışan VM'ye ekli yedekleme Azure yönetilmeyen VM diskleri|Henüz desteklenmiyor.
|Sayfa blob artımlı anlık görüntü kopyalama|Premium ve desteklenen standart Azure sayfa BLOB'ları|Henüz desteklenmiyor.
|Blob storage için depolama katmanları|Hot, seyrek erişimli ve depolama katmanları arşiv.|Henüz desteklenmiyor.
Blob storage için geçici silme|Önizleme|Henüz desteklenmiyor.
|Sayfa blob en büyük boyutu|8 TB|1 TB
|Sayfa blob sayfa boyutu|512 bayt|4 KB
|Tablo bölüm anahtarını ve satır anahtar boyutu|1024 karakter (2.048 bayt)|400 karakter (800 bayt)

### <a name="metrics"></a>Ölçümler
Depolama ölçümleri ile bazı farklar vardır:
* Depolama ölçümleri işlem verilerinde iç veya dış ağ bant genişliği ayırt etmez.
* Depolama ölçümleri işlem verilerde bağlı diskler sanal makine erişimi içermez.

## <a name="api-version"></a>API sürümü
Aşağıdaki sürümler ile Azure yığın depolama desteklenir:

Azure depolama API'leri Hizmetleri:

1802 güncelleştirmek veya yeni:
 - [2017-04-17](https://docs.microsoft.com/en-us/rest/api/storageservices/version-2017-04-17)
 - [2016-05-31](https://docs.microsoft.com/en-us/rest/api/storageservices/version-2016-05-31)
 - [2015-12-11](https://docs.microsoft.com/en-us/rest/api/storageservices/version-2015-12-11)
 - [2015-07-08 ](https://docs.microsoft.com/en-us/rest/api/storageservices/version-2015-07-08)
 - [2015-04-05](https://docs.microsoft.com/en-us/rest/api/storageservices/version-2015-04-05)

Önceki sürümler:
 - [2015-04-05](https://docs.microsoft.com/en-us/rest/api/storageservices/version-2015-04-05)


Azure Depolama Yönetimi API Hizmetleri:

 - [2015-05-01-Önizleme](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
 - [2015-06-15](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
 - [2016-01-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)

## <a name="sdk-versions"></a>SDK sürümleri

Aşağıdaki istemci kitaplıklarından Azure yığın depolama ile desteklenir:

| İstemci kitaplığı | Azure desteklenen yığın sürümü | Bağlantı                                                                                                                                                                                                                                                                                                                                     | Uç nokta belirtimi       |
|----------------|-------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------|
| .NET           | 8.7.0 için 6.2.0.          | Nuget paketi:<br>https://www.nuget.org/packages/WindowsAzure.Storage/<br> <br>GitHub sürüm:<br>https://github.com/Azure/azure-storage-net/releases                                                                                                                                                                                    | app.config dosyası              |
| Java           | 4.1.0'da 6.1.0 için           | Maven paketi:<br>http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage<br> <br>GitHub sürüm:<br>https://github.com/Azure/azure-storage-java/releases                                                                                                                                                                    | Bağlantı dizesi kurulumu      |
| Node.js        | 1.1.0 2.7.0 için           | NPM bağlantı:<br>https://www.npmjs.com/package/azure-storage<br>(Örneğin: Çalıştır "npm yükleme azure-storage@2.7.0")<br> <br>Github sürüm:<br>https://github.com/Azure/azure-storage-node/releases                                                                                                                                         | Hizmet örneği bildirimi |
| C++            | 2.4.0 3.1.0 için           | Nuget paketi:<br>https://www.nuget.org/packages/wastorage.v140/<br> <br>GitHub sürüm:<br>https://github.com/Azure/azure-storage-cpp/releases                                                                                                                                                                                          | Bağlantı dizesi kurulumu      |
| PHP            | 0.15.0 1.0.0 için          | GitHub sürüm:<br>https://github.com/Azure/azure-storage-php/releases<br> <br>Oluşturucu yükleyin (aşağıda yer alan ayrıntılara bakın)                                                                                                                                                                                                                  | Bağlantı dizesi kurulumu      |
| Python         | 0.30.0 1.0.0 için          | GitHub sürüm:<br>https://github.com/Azure/azure-storage-python/releases                                                                                                                                                                                                                                                                | Hizmet örneği bildirimi |
| Ruby           | 0.12.1 1.0.1 için          | RubyGems paketi:<br>Ortak:<br>https://rubygems.org/gems/azure-storage-common/<br>Blob: https://rubygems.org/gems/azure-storage-blob/<br>Queue: https://rubygems.org/gems/azure-storage-queue/<br>Table: https://rubygems.org/gems/azure-storage-table/<br> <br>GitHub sürüm:<br>https://github.com/Azure/azure-storage-ruby/releases | Bağlantı dizesi kurulumu      |

## <a name="next-steps"></a>Sonraki adımlar

* [Azure yığın depolama geliştirme araçları ile çalışmaya başlama](azure-stack-storage-dev.md)
* [Azure yığın depolama giriş](azure-stack-storage-overview.md)

