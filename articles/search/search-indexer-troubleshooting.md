---
title: Genel Arama Dizin Oluşturucu sorunları - Azure Search sorunlarını giderme
description: Azure Search'te hataları ve dizin oluşturucular sık karşılaşılan sorunları düzeltmek veri kaynağı bağlantısı, güvenlik duvarı ve eksik belgeler.
author: mgottein
manager: cgronlun
services: search
ms.service: search
ms.devlang: na
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: magottei
ms.custom: seodec2018
ms.openlocfilehash: 1cb3260fa11354de963318a023fec912d082eae4
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67653411"
---
# <a name="troubleshooting-common-indexer-issues-in-azure-search"></a>Azure Search'te yaygın dizin oluşturucu sorunları giderme

Dizin oluşturucular bir dizi soruna Azure Search'e veri dizin oluşturulurken çalıştırabilirsiniz. Hata ana kategoriler şunlardır:

* [Bir veri kaynağına bağlanma](#data-source-connection-errors)
* [Belge işleme](#document-processing-errors)
* [Bir dizini belge alma](#index-errors)

## <a name="data-source-connection-errors"></a>Veri kaynağı bağlantı hataları

### <a name="blob-storage"></a>Blob Depolama

#### <a name="storage-account-firewall"></a>Depolama hesabı güvenlik duvarı

Azure depolama, yapılandırılabilir bir güvenlik duvarı sağlar. Azure Search, depolama hesabınıza bağlanabilmesi için varsayılan olarak, güvenlik duvarını devre dışıdır.

Güvenlik Duvarı etkinleştirilmişse belirli bir hata iletisi yok. Güvenlik Duvarı hataları görünmesi genellikle `The remote server returned an error: (403) Forbidden`.

Güvenlik Duvarı etkin olduğunu doğrulayabilirsiniz [portalı](https://docs.microsoft.com/azure/storage/common/storage-network-security#azure-portal). Erişime izin verecek şekilde seçerek Güvenlik Duvarı'nı devre dışı bırakmak için desteklenen tek geçici çözüm olan ['Tüm ağlar'](https://docs.microsoft.com/azure/storage/common/storage-network-security#azure-portal).

Ekli bir beceri kümesi oluşturucunuz sahip değilse, _olabilir_ girişimi [bir özel durum ekleyin](https://docs.microsoft.com/azure/storage/common/storage-network-security#managing-ip-network-rules) arama hizmetinizin IP adresleri için. Ancak, bu senaryo desteklenmez ve çalışma garanti edilmez.

Sunucunun FQDN'sini ping göndererek, arama hizmetinizin IP adresini bulabilirsiniz (`<your-search-service-name>.search.windows.net`).

### <a name="cosmos-db"></a>Cosmos DB

#### <a name="indexing-isnt-enabled"></a>Dizin oluşturma etkin değil

Azure arama, Cosmos DB oluşturma dolaylı bir bağımlılığı vardır. Otomatik Cosmos DB'de dizinleme, Azure Search başarılı durumuna döner, ancak dizin kapsayıcı içeriğini başarısız olur. Ayarları denetleyin ve dizin oluşturma hakkında yönergeler için bkz: [yönetme Azure Cosmos DB'de dizinleme](https://docs.microsoft.com/azure/cosmos-db/how-to-manage-indexing-policy#use-the-azure-portal).

## <a name="document-processing-errors"></a>Belge işleme hataları

### <a name="unprocessable-or-unsupported-documents"></a>İşlenemeyen veya desteklenmeyen belge

Blob dizin oluşturucu [biçimleri belge belgeleri açıkça desteklenir.](search-howto-indexing-azure-blob-storage.md#supported-document-formats). Bazı durumlarda, bir blob depolama kapsayıcısına desteklenmeyen belgelerini içerir. Bazen sorunlu belgeleri olabilir. Bu belgelerdeki tarafından oluşturucunuz durdurma önleyebilirsiniz [yapılandırma seçeneklerinin değiştirilmesi](search-howto-indexing-azure-blob-storage.md#dealing-with-errors):

```
PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
Content-Type: application/json
api-key: [admin key]

{
  ... other parts of indexer definition
  "parameters" : { "configuration" : { "failOnUnsupportedContentType" : false, "failOnUnprocessableDocument" : false } }
}
```

### <a name="missing-document-content"></a>Belge içeriği eksik

Blob dizin oluşturucu [bulur ve bir kapsayıcıdaki blobları metin ayıklar](search-howto-indexing-azure-blob-storage.md#how-azure-search-indexes-blobs). İle metin ayıklama bazı sorunlar şunlardır:

* Belgeyi yalnızca Taranan görüntüleri içerir. Taranan görüntüleri (JPG formatından) gibi metin olmayan içeriğe sahip olan PDF BLOB'ları, standart blob dizini oluşturma ardışık düzeninde sonuçlar yok. Görüntü içeriğini metin öğeleriyle varsa, kullanabileceğiniz [bilişsel arama](cognitive-search-concept-image-scenarios.md) bulup metni ayıklayın.
* Blob dizin oluşturucu, yalnızca dizin meta verisi için yapılandırılır. İçeriğini ayıklamak için blob dizin oluşturucu için yapılandırılmalıdır [hem içeriği hem de meta verileri ayıklar](search-howto-indexing-azure-blob-storage.md#controlling-which-parts-of-the-blob-are-indexed):

```
PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
Content-Type: application/json
api-key: [admin key]

{
  ... other parts of indexer definition
  "parameters" : { "configuration" : { "dataToExtract" : "contentAndMetadata" } }
}
```

## <a name="index-errors"></a>Dizin hataları

### <a name="missing-documents"></a>Eksik belgeler

Dizin oluşturucular belgeleri bulun bir [veri kaynağı](https://docs.microsoft.com/rest/api/searchservice/create-data-source). Bazen bir belge dizine veri kaynağından bir dizini eksik gibi görünüyor. Birkaç bu hatalar oluşabilir yaygın nedenler şunlardır:

* Belge dizine alınmadı. Başarılı bir dizin oluşturucuyu çalıştırma için portalı denetleyin.
* Belge, dizin oluşturucuyu Çalıştır sonra güncelleştirildi. Dizin üzerinde ise bir [zamanlama](https://docs.microsoft.com/rest/api/searchservice/create-indexer#indexer-schedule), bu sonunda yeniden çalıştırın ve belgeyi seçin.
* [Sorgu](https://docs.microsoft.com/rest/api/searchservice/create-data-source#request-body-syntax) verilerinde belirtilen kaynak belge dışlar. Dizin oluşturucular veri kaynağının bir parçası olmayan belgelere dizin oluşturulamıyor.
* [Alan eşlemeleri](https://docs.microsoft.com/rest/api/searchservice/create-indexer#fieldmappings) veya [bilişsel arama](https://docs.microsoft.com/azure/search/cognitive-search-concept-intro) belge değiştirilmiş ve beklediğinizden farklı görünüyor.
* Kullanım [arama belge API](https://docs.microsoft.com/rest/api/searchservice/lookup-document) belgenizi bulunacak.
