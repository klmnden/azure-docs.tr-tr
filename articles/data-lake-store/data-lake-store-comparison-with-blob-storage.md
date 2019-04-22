---
title: Azure depolama blobu ile Azure Data Lake depolama Gen1 karşılaştırması | Microsoft Docs
description: Azure depolama blobu ile Azure Data Lake depolama Gen1 karşılaştırması
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: b199525b-84de-4f79-9eb6-69a613b8b217
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: twooley
ms.openlocfilehash: 478c261bb909cbc931a7dbbaa9cb6c61152970e4
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58885545"
---
# <a name="comparing-azure-data-lake-storage-gen1-and-azure-blob-storage"></a>Azure Data Lake depolama Gen1 ve Azure Blob Depolama ile karşılaştırma

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)] 

Bu makalede tabloda büyük veri işleme önemli bazı yönlerini boyunca Azure Data Lake depolama Gen1 ve Azure Blob Depolama alanı arasındaki farklar özetlenmektedir. Azure Blob Depolama, genel amaç, çeşitli depolama senaryoları için tasarlanan ölçeklenebilir nesne deposu. Azure Data Lake depolama Gen1 büyük veri analizi iş yükleri için en iyi duruma getirilmiş hiper ölçekli bir depodur.

|  | Azure Data Lake Storage Gen1 | Azure Blob Depolama |
| --- | --- | --- |
| Amaç |Büyük veri analizi iş yükleri için iyileştirilmiş depolama |Genel amaçlı depolama senaryolarında, büyük veri analizi dahil olmak üzere çok çeşitli için nesne deposu |
| Kullanım Örnekleri |Toplu iş, etkileşimli, günlük dosyaları, IOT verileri, tıklama akışlarından, büyük veri kümeleri gibi analiz ve makine öğrenme veri akışı |Herhangi bir uygulama gibi bir metin veya ikili veri türü, son yedekleme verileri, akış ve genel amaçlı veriler için medya depolama yedekleyin. Buna ek olarak, destek analizi iş yükleri için tam; Toplu iş, etkileşimli, günlük dosyaları, IOT verileri, tıklama akışlarından, büyük veri kümeleri gibi analiz ve makine öğrenme veri akışı |
| Önemli Kavramlar |Data Lake depolama Gen1 hesabı sırayla dosyaları olarak depolanan veriler içeren klasör içerir |Depolama hesabı kapsayıcıları, blobları biçiminde veri sırayla olan içeriyor |
| Yapı |Hiyerarşik dosya sistemi |Düz ad alanına sahip nesne deposu |
| API |HTTPS üzerinden REST API |HTTP/HTTPS üzerinden REST API |
| Sunucu tarafı API'si |[WebHDFS ile uyumlu REST API](https://msdn.microsoft.com/library/azure/mt693424.aspx) |[Azure Blob Depolama REST API'si](https://msdn.microsoft.com/library/azure/dd135733.aspx) |
| Hadoop dosya sistemi istemci |Evet |Evet |
| Veri işlemleri - kimlik doğrulaması |Temel [Azure Active Directory kimlikleri](../active-directory/develop/authentication-scenarios.md) |Paylaşılan gizlilikleri üzerinde - tabanlı [hesap erişim anahtarlarını](../storage/common/storage-account-manage.md#access-keys) ve [paylaşılan erişim imzası anahtarları](../storage/common/storage-dotnet-shared-access-signature-part-1.md). |
| Veri işlemleri - kimlik doğrulama protokolü |OAuth 2.0. Azure Active Directory tarafından verilen bir geçerli (JSON Web belirteci) JWT çağrıları içermelidir |Karma tabanlı ileti kimlik doğrulama kodu (HMAC). Çağrıları Base64 kodlamalı SHA-256 karma HTTP isteğinin bir bölümü içermelidir. |
| Veri işlemleri - yetkilendirme |POSIX erişim denetim listeleri (ACL'ler).  Azure Active Directory kimliği temel ACL'leri dosya ve klasör düzeyinde ayarlanabilir. |Hesap düzeyinde yetkilendirme için – kullanan [hesap erişim anahtarlarını](../storage/common/storage-account-manage.md#access-keys)<br>Hesabı, kapsayıcı veya blob yetkilendirme - kullanın [paylaşılan erişim imzası anahtarlar](../storage/common/storage-dotnet-shared-access-signature-part-1.md) |
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


