---
title: Media Services REST ile varlıkları yönetme | Microsoft Docs
description: Media Services REST API varlıklarla yönetmeyi öğrenin.
author: juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: 95262a32-0f2a-4286-b9e2-1a1ca6399b5b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: ffbf30f2bfdf0a175513a8d2b9182b35c39f6aae
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60761718"
---
# <a name="managing-media-services-entities-with-rest"></a>Media Services REST ile varlıkları yönetme  

> [!div class="op_single_selector"]
> * [REST](media-services-rest-manage-entities.md)
> * [.NET](media-services-dotnet-manage-entities.md)
> 
> 

Microsoft Azure Media Services OData v3 oluşturulmuş bir REST tabanlı hizmetidir. Ekleme, sorgu, güncelleştirme ve diğer herhangi bir OData hizmeti mümkün olduğunca çok varlıkları aynı şekilde silin. Özel durumlar, uygun olduğunda belirtilir. OData hakkında daha fazla bilgi için bkz. [açık veri Protokolü belgeleri](https://www.odata.org/documentation/).

Bu konuda, Azure Media Services REST ile varlıkları yönetme işlemini göstermektedir.

>[!NOTE]
> 1 Nisan 2017’den itibaren, hesabınızdaki 90 günden eski olan tüm İş kayıtları, toplam kayıt sayısı üst kota sınırının altında olsa bile ilişkili Görev kayıtlarıyla birlikte otomatik olarak silinecektir. Örneğin, 1 Nisan 2017'de hesabınızda 31 Aralık 2016'dan daha eski olan tüm iş kayıtları otomatik olarak silinir. İş/görev bilgilerini arşivlemeniz gerekiyorsa, bu konuda açıklanan kodu kullanabilirsiniz.

## <a name="considerations"></a>Dikkat edilmesi gerekenler  

Varlıklar Media Services erişirken, HTTP isteklerini özel üstbilgi alanlarını ve değerlerini ayarlamanız gerekir. Daha fazla bilgi için [Media Services REST API geliştirme için Kurulum](media-services-rest-how-to-use.md).

## <a name="connect-to-media-services"></a>Media Services’e bağlanmak

AMS API'ye bağlanma hakkında daha fazla bilgi için bkz: [Azure AD kimlik doğrulamasıyla Azure Media Services API'sine erişim](media-services-use-aad-auth-to-access-ams-api.md). 

## <a name="adding-entities"></a>Varlıklar ekleme
Media Services her varlık, bir varlık kümesindeki, varlıklar gibi bir HTTP POST isteği üzerinden eklenir.

Aşağıdaki örnek, bir AccessPolicy oluşturma işlemi gösterilmektedir.

    POST https://media.windows.net/API/AccessPolicies HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN> 
    Host: media.windows.net
    Content-Length: 74
    Expect: 100-continue

    {"Name": "DownloadPolicy", "DurationInMinutes" : "300", "Permissions" : 1}

## <a name="querying-entities"></a>Varlıkları sorgulama
Sorgulamak ve varlıkları listeleyen oldukça basittir ve yalnızca GET HTTP istek ve isteğe bağlı bir OData işlemleri içerir.
Aşağıdaki örnek, tüm MediaProcessor varlıkların listesini alır.

    GET https://media.windows.net/API/MediaProcessors HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN> 
    Host: media.windows.net

Ayrıca, belirli bir varlığa veya aşağıdaki örneklerde olduğu gibi belirli bir varlık ile ilişkili tüm varlık kümelerini alabilirsiniz:

    GET https://media.windows.net/API/JobTemplates('nb:jtid:UUID:e81192f5-576f-b247-b781-70a790c20e7c') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN> 
    Host: media.windows.net

    GET https://media.windows.net/API/JobTemplates('nb:jtid:UUID:e81192f5-576f-b247-b781-70a790c20e7c')/TaskTemplates HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN> 
    Host: media.windows.net

Aşağıdaki örnek, yalnızca tüm işlerin durumu özelliğini döndürür.

    GET https://media.windows.net/API/Jobs?$select=State HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN> 
    Host: media.windows.net

Aşağıdaki örnek "SampleTemplate." adlı tüm JobTemplates döndürür

    GET https://media.windows.net/API/JobTemplates?$filter=startswith(Name,%20'SampleTemplate') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN> 
    Host: media.windows.net

> [!NOTE]
> $Expand işlemi Media Services, hem de LINQ konuları (WCF Data Services) açıklanan desteklenmeyen LINQ yöntemleri desteklenmiyor.
> 
> 

## <a name="enumerating-through-large-collections-of-entities"></a>Varlıklar büyük koleksiyonlarına numaralandırma
Varlıkları sorgulanırken ortak REST v2 1000 sonuçları için sorgu sonuçları sınırladığı için tek seferde döndürülen 1000 varlıkların bir sınır yoktur. Kullanım **atla** ve **üst** büyük varlıklar koleksiyonu numaralandırılamadı. 

Aşağıdaki örnek nasıl kullanılacağını gösterir **atla** ve **üst** ilk 2000 işleri atlayıp sonraki 1000 işleri görüntüleyin.  

    GET https://media.windows.net/api/Jobs()?$skip=2000&$top=1000 HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN>
    Host: media.windows.net

## <a name="updating-entities"></a>Varlıkları güncelleştirme
Varlık türü ve durumda olan durumuna bağlı olarak, PUT veya birleştirme HTTP isteklerinin bir düzeltme eki aracılığıyla bu varlıkta özelliklerini güncelleştirebilir. Bu işlemler hakkında daha fazla bilgi için bkz: [düzeltme eki/PUT/MERGE](https://msdn.microsoft.com/library/dd541276.aspx).

Aşağıdaki kod örneği, Name özelliği bir varlık varlığı güncelleştirmek gösterilmektedir.

    MERGE https://media.windows.net/API/Assets('nb:cid:UUID:80782407-3f87-4e60-a43e-5e4454232f60') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN>
    Host: media.windows.net
    Content-Length: 21
    Expect: 100-continue

    {"Name" : "NewName" }

## <a name="deleting-entities"></a>Varlıkları silme
Varlıklar Media Services'de bir HTTP DELETE isteği kullanılarak silinebilir. Varlık bağlı olarak, varlıklarını silme sırası önemli olabilir. Örneğin, varlıklar gibi varlıkları gerektiren, iptal etme (veya sildiğinizde) varlık silmeden önce belirli bir varlığa başvurmak tüm Bulucular.

Aşağıdaki örnek, bir dosyayı blob depolama alanına yüklemek için kullanılan bir Bulucuyu silmek gösterilmektedir.

    DELETE https://media.windows.net/API/Locators('nb:lid:UUID:76dcc8e8-4230-463d-97b0-ce25c41b5c8d') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN>
    Host: media.windows.net
    Content-Length: 0

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

