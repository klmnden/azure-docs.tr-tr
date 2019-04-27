---
title: Azure AI Gallery verileri yönetme
titleSuffix: Azure Machine Learning Studio
description: Dışarı aktarma ve ürün içi kullanıcı verilerinizi arabirimi veya yapay ZEKA Galerisi'ni Kataloğu API'sini kullanarak Azure AI Gallery Sil. Bu makalede, nasıl gösterir.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 05/25/2018
ms.reviewer: jmartens, mldocs
ms.openlocfilehash: 44ff2a5b723c086604acf39e9f975deb53759ae1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60752054"
---
# <a name="view-and-delete-in-product-user-data-from-azure-ai-gallery"></a>Ve Azure AI Gallery ürün içi kullanıcı verilerini silme

Görüntüleyebilir ve ürün içi kullanıcı verilerinizi arabirimi veya yapay ZEKA Galerisi'ni Kataloğu API'sini kullanarak Azure AI Gallery Sil. Bu makalede nasıl yapılacağı açıklanmaktadır.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="view-your-data-in-ai-gallery-with-the-ui"></a>AI Galerisi'nde kullanıcı Arabirimi ile verilerinizi görüntüleyin

Azure AI Gallery Web UI yayımlanan öğeleri görüntüleyebilir. Kullanıcılar, hem genel hem de listelenmemiş çözümler, projeler, denemeler ve yayımlanan diğer öğeleri görüntüleyebilir:

1.  Oturum [Azure AI Gallery](https://gallery.azure.ai/).
2.  Profil resmi sağ üst köşedeki ve hesap adını, böylece profil sayfanızı yüklemek için tıklayın.
3.  Profil sayfasında listelenmeyen girişleri de dahil olmak üzere galeride yayımlanmış tüm öğeleri görüntüler.

## <a name="use-the-ai-gallery-catalog-api-to-view-your-data"></a>Verilerinizi görüntülemek için yapay ZEKA Galerisi'ni Kataloğu API'sini kullanın.

Programlı olarak adresinden erişilebilen yapay ZEKA Galerisi'ni Kataloğu API'si aracılığıyla toplanan verileri görüntüleme https://catalog.cortanaanalytics.com/entities. Verileri görüntülemek için yazar kimliğinizi gerekir Katalog API aracılığıyla listelenmemiş olan varlıkları görüntülemek için bir erişim belirteci gerekir.

Katalog yanıtları JSON biçiminde döndürülür.

### <a name="get-an-author-id"></a>Bir yazar Kimliğini alın
Yazar Kimliği için Azure AI Gallery yayımlama sırasında kullanılan e-posta adresini temel alır. Değişmez:

1.  Oturum [Azure AI Gallery](https://gallery.azure.ai/).
2.  Profil resmi sağ üst köşedeki ve hesap adını, böylece profil sayfanızı yüklemek için tıklayın.
3.  Alfasayısal kimliği aşağıdaki adres çubuğundaki URL'yi görüntüler `authorId=`. Örneğin, URL için: `https://gallery.azure.ai/Home/Author?authorId=99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA`
        
    Yazar Kimliği: `99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA`

### <a name="get-your-access-token"></a>Erişim belirteci alma

Katalog API aracılığıyla listelenmemiş olan varlıkları görüntülemek için bir erişim belirteci ihtiyacınız vardır. Bir erişim belirteci, kullanıcılar hala genel varlıklar ve diğer kullanıcı bilgileri görüntüleyebilir.

Erişim belirteci almak için incelemesi gereken `DataLabAccessToken` tarayıcı yapar Kataloğu oturumunuz açıkken API'sine HTTP isteği üstbilgisi:

1.  Oturum [Azure AI Gallery](https://gallery.azure.ai/).
2.  Profil resmi sağ üst köşedeki ve hesap adını, böylece profil sayfanızı yüklemek için tıklayın.
3.  F12 tuşuna basarak tarayıcı geliştirici araçları bölmesini açın, ağ sekmesini seçin ve sayfayı yenileyin. 
4. Filtre dizesi isteklerinde *Kataloğu* filtre metin kutusuna yazarak.
5.  İstek URL'si `https://catalog.cortanaanalytics.com/entities`bir GET isteği bulup seçin *üstbilgileri* sekmesi. Ekranı aşağı kaydırarak *istek üstbilgileri* bölümü.
6.  Başlık altında `DataLabAccessToken` alfasayısal belirteç. Verilerinizin güvenliğini sağlamanıza yardımcı olmak için bu belirteci paylaşmayın.

### <a name="view-user-information"></a>Kullanıcı bilgilerini görüntüleme
Önceki adımda aldığınız Yazar Kimliğini kullanarak, bilgileri kullanıcının profilinde değiştirerek görüntülemek `[AuthorId]` aşağıdaki URL:

    https://catalog.cortanaanalytics.com/users/[AuthorID]

Örneğin, bu URL isteği:
    
    https://catalog.cortanaanalytics.com/users/99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA

Gibi bir yanıt döndürür:

    {"entities_count":9,"contribution_score":86.351575190956922,"scored_at":"2018-05-07T14:30:25.9305671+00:00","contributed_at":"2018-05-07T14:26:55.0381756+00:00","created_at":"2017-12-15T00:49:15.6733094+00:00","updated_at":"2017-12-15T00:49:15.6733094+00:00","name":"First Last","slugs":["First-Last"],"tenant_id":"14b2744cf8d6418c87ffddc3f3127242","community_id":"9502630827244d60a1214f250e3bbca7","id":"99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA","_links":{"self":"https://catalog.azureml.net/tenants/14b2744cf8d6418c87ffddc3f3127242/communities/9502630827244d60a1214f250e3bbca7/users/99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA"},"etag":"\"2100d185-0000-0000-0000-5af063010000\""}


### <a name="view-public-entities"></a>Görünüm genel varlıklar

Kataloğu API'sini doğrudan görüntüleyebilirsiniz Azure AI Gallery için yayımlanan varlıkları hakkında bilgi depolar [yapay ZEKA Galerisi'ni Web sitesi](https://gallery.azure.ai/). 

Yayımlanmış olan varlıkları görüntülemek için aşağıdaki URL'yi ziyaret edin değiştirerek `[AuthorId]` elde Yazar kimlikli [Yazar Kimliğini alın](#get-an-author-id) yukarıda.

    https://catalog.cortanaanalytics.com/entities?$filter=author/id eq '[AuthorId]'

Örneğin:

    https://catalog.cortanaanalytics.com/entities?$filter=author/id eq '99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA'

### <a name="view-unlisted-and-public-entities"></a>Listede bulunmayan ve ortak olan varlıkları görüntülemek

Bu sorgu, yalnızca genel varlıklar görüntüler. Listelenmemiş olanlar da dahil olmak üzere tüm varlıklarınızı, görüntülemek için erişim sağlamak önceki bölümden alınan belirteci.

1.  Bir aracı gibi kullanarak [Postman](https://www.getpostman.com), açıklandığı Kataloğu URL'si için bir HTTP GET isteği oluşturmak [erişim belirteci alma](#get-your-access-token).
2.  Adlı bir HTTP isteği üstbilgisini oluşturmak `DataLabAccessToken`, değeri için erişim belirteci olarak ayarlanmış.
3.  HTTP isteği gönderin.

> [!TIP]
> Listelenmemiş varlıkları Kataloğu API'sinden yanıtlarındaki görünmüyorsa, kullanıcı geçersiz olabilir ya da erişim belirtecinizin süresi. Oturum dışında Azure AI Gallery ve adımları yineleyin [erişim belirteci alma](#get-your-access-token) belirteci yenileme. 
