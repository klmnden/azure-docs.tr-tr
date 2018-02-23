---
title: "Azure depolama Blob ile Azure Data Lake Store karşılaştırma | Microsoft Docs"
description: "Azure depolama Blob ile Azure Data Lake Store karşılaştırma"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: b199525b-84de-4f79-9eb6-69a613b8b217
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/09/2018
ms.author: nitinme
ms.openlocfilehash: a8831278ef0e83571bf05d48e5d671f8dee92850
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="comparing-azure-data-lake-store-and-azure-blob-storage"></a>Azure Data Lake Store ve Azure Blob Depolama karşılaştırma
Bu makalede tabloda büyük veri işleme anahtar bazı yönlerini boyunca Azure Data Lake Store ile Azure Blob Storage arasındaki farklar özetlenmektedir. Azure Blob Depolama genel amaçlı, çok çeşitli depolama senaryoları için tasarlanan ölçeklenebilir nesne deposu değil. Azure Data Lake Store, büyük veri analitik iş yükleri için en iyi duruma getirilmiş hiper ölçekli bir depodur.

|  | Azure Data Lake Store | Azure Blob Depolama |
| --- | --- | --- |
| Amaç |Büyük veri analitik iş yükleri için en iyi duruma getirilmiş depolama |Genel amaçlı nesne deposu çok çeşitli büyük veri analizi dahil olmak üzere depolama senaryoları için |
| Kullanım örnekleri |Toplu iş, etkileşimli, gibi akış analizi ve makine öğrenme veri günlük dosyaları, IOT veri, Akışlar, büyük veri kümeleri'ı tıklatın |Herhangi bir uygulama gibi metin veya ikili veri türü, son, yedekleme verilerini, akış ve genel amaçlı verileri için medya depolama yedekleyin. Ayrıca, destek analizi iş yükleri için tam; Toplu iş, etkileşimli, gibi akış analizi ve makine öğrenme veri günlük dosyaları, IOT veri, Akışlar, büyük veri kümeleri'ı tıklatın |
| Önemli Kavramlar |Data Lake Store hesabı sırayla dosyaları olarak depolanan veriler içeren klasörleri içerir. |Depolama hesabı kapsayıcıları, sırayla BLOB formundaki verileri sahip olan |
| yapısı |Hiyerarşik dosya sistemi |Düz ad alanına sahip nesne deposu |
| API |HTTPS üzerinden REST API'si |HTTP/HTTPS üzerinden REST API'si |
| Sunucu tarafı API |[WebHDFS ile uyumlu REST API'si](https://msdn.microsoft.com/library/azure/mt693424.aspx) |[Azure Blob Storage REST API'si](https://msdn.microsoft.com/library/azure/dd135733.aspx) |
| Hadoop dosya sistemi istemcisi |Evet |Evet |
| Veri işlemleri - kimlik doğrulama |Temel [Azure Active Directory kimlikleri](../active-directory/active-directory-authentication-scenarios.md) |Paylaşılan gizlilikler - tabanlı [hesabı erişim anahtarlarını](../storage/common/storage-create-storage-account.md#manage-your-storage-account) ve [paylaşılan erişim imzası anahtarlar](../storage/common/storage-dotnet-shared-access-signature-part-1.md). |
| Veri işlemleri - kimlik doğrulama protokolü |OAuth 2.0. Azure Active Directory tarafından verilen geçerli JWT (JSON Web belirteci) çağrıları içermelidir |Karma tabanlı ileti kimlik doğrulama kodu (HMAC). Çağrıları Base64 ile kodlanmış SHA-256 karma HTTP isteğinin bir parçası içermesi gerekir. |
| Veri işlemleri - yetkilendirme |POSIX erişim denetim listeleri (ACL'ler).  Azure Active Directory kimliği temel ACL'ler dosya ve klasör düzeyinde ayarlanabilir. |Hesap düzeyinde yetkilendirme için – kullanmak [hesap erişim tuşları](../storage/common/storage-create-storage-account.md#manage-your-storage-account)<br>Hesap, kapsayıcısı veya blob yetkilendirme - kullanın [paylaşılan erişim imzası anahtarlar](../storage/common/storage-dotnet-shared-access-signature-part-1.md) |
| Veri işlemleri - denetleme |Kullanılabilir. Bkz: [burada](data-lake-store-diagnostic-logs.md) bilgi. |Kullanılabilir |
| Rest verileri şifreleme |<ul><li>Saydam sunucu tarafı</li> <ul><li>Hizmet tarafından yönetilen anahtarlarla</li><li>Müşteri tarafından yönetilen anahtarlarla Azure KeyVault içinde</li></ul></ul> |<ul><li>Saydam sunucu tarafı</li> <ul><li>Hizmet tarafından yönetilen anahtarlarla</li><li>Müşteri tarafından yönetilen anahtarlarında ile Azure KeyVault (Önizleme)</li></ul><li>İstemci Tarafında Şifreleme</li></ul> |
| Yönetim işlemlerini (örneğin hesabı oluşturun) |[Rol tabanlı erişim denetimi](../active-directory/role-based-access-control-what-is.md) (RBAC) hesap yönetimi için Azure tarafından sağlanan |[Rol tabanlı erişim denetimi](../active-directory/role-based-access-control-what-is.md) (RBAC) hesap yönetimi için Azure tarafından sağlanan |
| Geliştirici SDK'ları |.NET, Java, Python, Node.js |.Net, Java, Python, Node.js, C++, Ruby, PHP, Go, Android, iOS |
| Analytics iş yükü performansı |Paralel analytics iş yükleri için en iyi duruma getirilmiş performans. Yüksek verimlilik ve IOPS. |Paralel analytics iş yükleri için en iyi duruma getirilmiş performans. |
| Boyutu sınırları |Hesap boyutları, dosya boyutları veya dosya sayısı sınırı |Belgelenen belirli sınırları [burada](../storage/common/storage-scalability-targets.md). Büyük hesap sınırlar kullanılabilir başvurarak [Azure desteği](https://azure.microsoft.com/support/faq/) |
| Coğrafi artıklık |Yerel olarak yedekli (bir Azure bölgesindeki verilerin birden çok kopya) |Yerel olarak yedekli (LRS), bölge olarak yedekli (ZRS), genel olarak yedekli (GRS), okuma erişimi genel yedekli (RA-GRS). Bkz: [burada](../storage/common/storage-redundancy.md) daha fazla bilgi için |
| Hizmet durumu |Genel olarak kullanılabilir |Genel olarak kullanılabilir |
| Bölgesel kullanılabilirlik |Bkz: [burada](https://azure.microsoft.com/regions/#services) |Kullanılabilir tüm Azure bölgeleri |
| Fiyat |Bkz: [fiyatlandırma](https://azure.microsoft.com/pricing/details/data-lake-store/) |Bkz: [fiyatlandırma](https://azure.microsoft.com/pricing/details/storage/) |

### <a name="next-steps"></a>Sonraki adımlar
* [Azure Data Lake Store'a Genel Bakış](data-lake-store-overview.md)
* [Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-portal.md)

