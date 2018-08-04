---
title: Azure depolama blobu ile Azure Data Lake Store karşılaştırması | Microsoft Docs
description: Azure depolama blobu ile Azure Data Lake Store karşılaştırması
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: b199525b-84de-4f79-9eb6-69a613b8b217
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: nitinme
ms.openlocfilehash: 0b374e92a1e1d9828bc8c095e29e1dfdfd13275b
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39492920"
---
# <a name="comparing-azure-data-lake-store-and-azure-blob-storage"></a>Azure Data Lake Store ve Azure Blob Depolama ile karşılaştırma
Bu makalede tabloda büyük veri işleme önemli bazı yönlerini boyunca Azure Data Lake Store ve Azure Blob Depolama alanı arasındaki farklar özetlenmektedir. Azure Blob Depolama, genel amaç, çeşitli depolama senaryoları için tasarlanan ölçeklenebilir nesne deposu. Azure Data Lake Store, büyük veri analizi iş yükleri için iyileştirilmiş hiper ölçekli bir depodur.

|  | Azure Data Lake Store | Azure Blob Depolama |
| --- | --- | --- |
| Amaç |Büyük veri analizi iş yükleri için iyileştirilmiş depolama |Genel amaçlı depolama senaryolarında, büyük veri analizi dahil olmak üzere çok çeşitli için nesne deposu |
| Kullanım örnekleri |Toplu iş, etkileşimli, günlük dosyaları, IOT verileri, tıklama akışlarından, büyük veri kümeleri gibi analiz ve makine öğrenme veri akışı |Herhangi bir uygulama gibi bir metin veya ikili veri türü, son yedekleme verileri, akış ve genel amaçlı veriler için medya depolama yedekleyin. Buna ek olarak, destek analizi iş yükleri için tam; Toplu iş, etkileşimli, günlük dosyaları, IOT verileri, tıklama akışlarından, büyük veri kümeleri gibi analiz ve makine öğrenme veri akışı |
| Önemli Kavramlar |Data Lake Store hesabı sırayla dosyaları olarak depolanan veriler içeren klasörleri içerir. |Depolama hesabı kapsayıcıları, blobları biçiminde veri sırayla olan içeriyor |
| yapısı |Hiyerarşik dosya sistemi |Düz ad alanına sahip nesne deposu |
| API |HTTPS üzerinden REST API |HTTP/HTTPS üzerinden REST API |
| Sunucu tarafı API'si |[WebHDFS ile uyumlu REST API](https://msdn.microsoft.com/library/azure/mt693424.aspx) |[Azure Blob Depolama REST API'si](https://msdn.microsoft.com/library/azure/dd135733.aspx) |
| Hadoop dosya sistemi istemci |Evet |Evet |
| Veri işlemleri - kimlik doğrulaması |Temel [Azure Active Directory kimlikleri](../active-directory/develop/authentication-scenarios.md) |Paylaşılan gizlilikleri üzerinde - tabanlı [hesap erişim anahtarlarını](../storage/common/storage-create-storage-account.md#manage-your-storage-account) ve [paylaşılan erişim imzası anahtarları](../storage/common/storage-dotnet-shared-access-signature-part-1.md). |
| Veri işlemleri - kimlik doğrulama protokolü |OAuth 2.0. Azure Active Directory tarafından verilen bir geçerli (JSON Web belirteci) JWT çağrıları içermelidir |Karma tabanlı ileti kimlik doğrulama kodu (HMAC). Çağrıları Base64 kodlamalı SHA-256 karma HTTP isteğinin bir bölümü içermelidir. |
| Veri işlemleri - yetkilendirme |POSIX erişim denetim listeleri (ACL'ler).  Azure Active Directory kimliği temel ACL'leri dosya ve klasör düzeyinde ayarlanabilir. |Hesap düzeyinde yetkilendirme için – kullanan [hesap erişim anahtarlarını](../storage/common/storage-create-storage-account.md#manage-your-storage-account)<br>Hesabı, kapsayıcı veya blob yetkilendirme - kullanın [paylaşılan erişim imzası anahtarlar](../storage/common/storage-dotnet-shared-access-signature-part-1.md) |
| Veri işlemleri - denetim |Kullanılabilir. Bkz: [burada](data-lake-store-diagnostic-logs.md) bilgi. |Kullanılabilir |
| Bekleyen verileri şifreleme |<ul><li>Saydam sunucu tarafı</li> <ul><li>Hizmet tarafından yönetilen anahtarlarla</li><li>Azure anahtar Kasası'nda müşteri tarafından yönetilen anahtarlar ile</li></ul></ul> |<ul><li>Saydam sunucu tarafı</li> <ul><li>Hizmet tarafından yönetilen anahtarlarla</li><li>Müşteri tarafından yönetilen anahtarlarla içinde Azure anahtar kasası (Önizleme)</li></ul><li>İstemci Tarafında Şifreleme</li></ul> |
| Yönetim işlemlerini (örneğin hesap oluştur) |[Rol tabanlı erişim denetimi](../role-based-access-control/overview.md) (RBAC), hesap yönetimi için Azure tarafından sağlanan |[Rol tabanlı erişim denetimi](../role-based-access-control/overview.md) (RBAC), hesap yönetimi için Azure tarafından sağlanan |
| Geliştirici SDK'ları |.NET, Java, Python, Node.js |.NET, Java, Python, Node.js, C++, Ruby, PHP, Go, Android, iOS |
| Analiz iş yükü performansı |Paralel analiz iş yükleri için iyileştirilmiş performans. Yüksek aktarım hızı ve IOPS. |Paralel analiz iş yükleri için iyileştirilmiş performans. |
| Boyutu sınırları |Hesap boyutları, dosya boyutları veya dosya sayısı üst sınırı yoktur |Belirli sınırları belgelenen [burada](../storage/common/storage-scalability-targets.md). Daha büyük hesap sınırlar kullanılabilir başvurarak [Azure desteği](https://azure.microsoft.com/support/faq/) |
| Coğrafi yedeklilik |Yerel olarak yedekli (bir Azure bölgesindeki verilerin birden çok kopyasını) |Yerel olarak yedekli (LRS), bölgesel olarak yedekli (ZRS), genel olarak yedekli (GRS), okuma erişimli yedekli genel (RA-GRS). Bkz: [burada](../storage/common/storage-redundancy.md) daha fazla bilgi için |
| Hizmet durumu |Genel kullanıma sunuldu |Genel kullanıma sunuldu |
| Bölgesel kullanılabilirlik |Bkz: [burada](https://azure.microsoft.com/regions/#services) |Tüm Azure bölgelerinde kullanılabilir |
| Fiyat |Bkz: [fiyatlandırması](https://azure.microsoft.com/pricing/details/data-lake-store/) |Bkz: [fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/) |


