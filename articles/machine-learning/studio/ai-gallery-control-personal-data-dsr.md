---
title: Görüntüleyin ve verilerinizi Azure AI galerisinden Sil | Microsoft Docs
description: Dışarı aktarma ve ürün kullanıcı verilerinizi Azure AI arabirimi veya AI galeri Kataloğu API'sini kullanarak Galerisi'nden silin. Bu makale size nasıl gösterir.
services: machine-learning
author: heatherbshapiro
ms.author: hshapiro
manager: cgronlun
ms.reviewer: jmartens, mldocs
ms.service: machine-learning
ms.component: studio
ms.topic: conceptual
ms.date: 05/25/2018
ms.openlocfilehash: bc6ffa912d7914c8662dbde623e04947540ac149
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655778"
---
# <a name="view-and-delete-in-product-user-data-from-azure-ai-gallery"></a>Görüntüleyin ve ürün kullanıcı verilerini Azure AI galerisinden Sil

Görüntüleyebilir ve ürün kullanıcı verilerinizi Azure AI arabirimi veya AI galeri Kataloğu API'sini kullanarak Galerisi'nden silebilirsiniz. Bu makalede bunun nasıl yapılacağı açıklanmaktadır.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="view-your-data-in-ai-gallery-with-the-ui"></a>Kullanıcı Arabirimi ile AI Galerisi'nde verilerinizi görüntüleme

Azure AI galeri Web kullanıcı Arabirimi yayımlanan öğeleri görüntüleyebilirsiniz. Kullanıcılar, hem genel hem de listelenmemiş çözümleri, projeler, denemeler ve diğer yayımlanan öğeleri görüntüleyebilir:

1.  Oturum [Azure AI galeri](https://gallery.azure.ai/).
2.  Profil resmi sağ üst köşesinde ve profil sayfanızı yüklemek için hesap adını tıklatın.
3.  Profil sayfası listelenmemiş girişleri dahil olmak üzere Galerisine yayımlanan tüm öğeleri görüntüler.

## <a name="use-the-ai-gallery-catalog-api-to-view-your-data"></a>Verilerinizi görüntülemek için AI galeri Kataloğu API'sini kullanın

Programlı olarak erişilebilir adresindeki AI galeri Kataloğu API'si aracılığıyla toplanan verileri görüntüleme https://catalog.cortanaanalytics.com/entities. Verileri görüntülemek için kimliğinizi Yazar gerekir Kataloğu API'si aracılığıyla listelenmemiş varlıkları görüntülemek için bir erişim belirteci gerekir.

Katalog yanıtları JSON biçiminde döndürülür.

### <a name="get-an-author-id"></a>Bir yazar kimlik alma
Yazar Kimliği Azure AI Galerisi'ne yayımlarken kullanılan e-posta adresini temel alır. Değişmez:

1.  Oturum [Azure AI galeri](https://gallery.azure.ai/).
2.  Profil resmi sağ üst köşesinde ve profil sayfanızı yüklemek için hesap adını tıklatın.
3.  Adres çubuğundaki URL'yi alfasayısal kimliği aşağıdaki görüntüler `authorId=`. Örneğin, URL için: `https://gallery.cortanaintelligence.com/Home/Author?authorId=99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA`
        
    Yazar Kimliği: `99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA`

### <a name="get-your-access-token"></a>Erişim belirteci alın

Kataloğu API'si aracılığıyla listelenmemiş varlıkları görüntülemek için bir erişim belirteci gerekir. Bir erişim belirteci, kullanıcılar genel varlıkları ve diğer kullanıcı bilgileri görüntüleyebilirsiniz.

Bir erişim belirteci almak için incelemesi gereken `DataLabAccessToken` tarayıcı yapar katalog oturum açtığınızda API'sine HTTP isteği üstbilgisi:

1.  Oturum [Azure AI galeri](https://gallery.azure.ai/).
2.  Profil resmi sağ üst köşesinde ve profil sayfanızı yüklemek için hesap adını tıklatın.
3.  F12 tuşuna basarak tarayıcının geliştirici araçları bölmesini açın, ağ sekmesini seçin ve sayfayı yenileyin. 
4. Dize isteklerinde filtre *katalog* filtre metin kutusuna yazarak.
5.  İstek URL `https://catalog.cortanaanalytics.com/entities`, bir GET isteği bulun ve seçin *üstbilgileri* sekmesi. Ekranı aşağı kaydırarak *istek üstbilgileri* bölümü.
6.  Başlığı altında `DataLabAccessToken` alfasayısal belirteci. Verilerinizi güvenli tutmaya yardımcı olmak için bu belirteç paylaşmayın.

### <a name="view-user-information"></a>Kullanıcı bilgilerini görüntüleme
Önceki adımda aldığınız Yazar Kimliğini kullanarak, bir kullanıcının profili değiştirerek bilgilerini `[AuthorId]` aşağıdaki URL'de:

    https://catalog.cortanaanalytics.com/users/[AuthorID]

Örneğin, bu URL isteği:
    
    https://catalog.cortanaanalytics.com/users/99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA

Gibi bir yanıt döndürür:

    {"entities_count":9,"contribution_score":86.351575190956922,"scored_at":"2018-05-07T14:30:25.9305671+00:00","contributed_at":"2018-05-07T14:26:55.0381756+00:00","created_at":"2017-12-15T00:49:15.6733094+00:00","updated_at":"2017-12-15T00:49:15.6733094+00:00","name":"First Last","slugs":["First-Last"],"tenant_id":"14b2744cf8d6418c87ffddc3f3127242","community_id":"9502630827244d60a1214f250e3bbca7","id":"99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA","_links":{"self":"https://catalog.azureml.net/tenants/14b2744cf8d6418c87ffddc3f3127242/communities/9502630827244d60a1214f250e3bbca7/users/99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA"},"etag":"\"2100d185-0000-0000-0000-5af063010000\""}


### <a name="view-public-entities"></a>Genel varlıkları görüntüle

Katalog API doğrudan görüntüleyebilirsiniz Azure AI Galeriye yayımlanan varlıkları hakkındaki bilgileri depolar [AI Galerisi Web sitesi](https://gallery.azure.ai/). 

Yayımlanan varlıkları görüntülemek için aşağıdaki URL'yi ziyaret değiştirme `[AuthorId]` elde Yazar kimlikli [bir Yazar Kimliği almak](#get-an-author-ID) yukarıda.

    https://catalog.cortanaanalytics.com/entities?$filter=author/id eq '[AuthorId]'

Örneğin:

    https://catalog.cortanaanalytics.com/entities?$filter=author/id eq '99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA'

### <a name="view-unlisted-and-public-entities"></a>Listede bulunmayan ve genel varlıklar görüntüleyin

Bu sorgu yalnızca genel varlıklar görüntüler. Listede bulunmayan olanlar da dahil olmak üzere tüm varlıklarınızı, görüntülemek için erişim sağlamak belirteç elde önceki bölümden.

1.  Gibi bir araç kullanarak [Postman](https://www.getpostman.com), kataloğu URL'si için bir HTTP GET isteği açıklandığı gibi oluşturmak [erişim belirteci almak](#get-your-access-token).
2.  Adlı bir HTTP isteği üstbilgisini oluşturmak `DataLabAccessToken`, erişim belirtecine ayarlanan değer ile.
3.  HTTP isteği gönderin.

> [!TIP]
> Listede bulunmayan varlıklar katalog API'sinden yanıtlarındaki görünmüyorsa, kullanıcı geçersiz olabilir ya da erişim belirtecinin süresi. Oturum dışında Azure AI Galerisi ve adımlarını yineleyin [erişim belirteci almak](#get-your-access-token) belirteç yenilemek için. 
