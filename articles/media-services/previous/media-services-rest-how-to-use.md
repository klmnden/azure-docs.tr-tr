---
title: Media Services işlemlerini REST API'si genel bakış | Microsoft Docs
description: Media Services REST API'sine genel bakış
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: a5f1c5e7-ec52-4e26-9a44-d9ea699f68d9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako;johndeu
ms.openlocfilehash: fbdd9325f50e1bcb271b7ca47b9ccd3361d0d27e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64687054"
---
# <a name="media-services-operations-rest-api-overview"></a>Media Services işlemlerini REST API'si genel bakış 

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürüm olan [Media Services v3](https://docs.microsoft.com/azure/media-services/latest/)’ü inceleyin. Ayrıca bkz [geçiş kılavuzuna v2'den v3](../latest/migrate-from-v2-to-v3.md)

**Medya hizmetleri işlemlerini REST** API, işleri, varlıklar, Canlı Kanallar ve diğer kaynakları bir Media Services hesabı oluşturmak için kullanılır. Daha fazla bilgi için [Media Services işlemlerini REST API'si başvurusu](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).

Media Services, JSON veya atom + XML biçimi pub kabul eden bir REST API'si sağlar. Media Services REST API'si, Media Services, hem de isteğe bağlı üst bilgiler bir dizi bağlanırken her bir istemciye göndermesi gerekir belirli HTTP üst bilgileri gerektirir. Aşağıdaki bölümlerde üstbilgileri ve ne zaman kullanabileceğiniz HTTP fiilleri istekleri oluşturmak ve yanıtları Media Services'dan teslim alma.

Kimlik doğrulaması için Media Services REST API makalesinde özetlenen Azure Active Directory kimlik doğrulaması aracılığıyla gerçekleştirilir [REST ile Azure Media Services API'sine erişmek için kimlik doğrulamasını Azure AD kullanın](media-services-rest-connect-with-aad.md)

## <a name="considerations"></a>Dikkat edilmesi gerekenler

REST kullanırken aşağıdaki maddeler geçerlidir.

* Varlıkları sorgulanırken ortak REST v2 1000 sonuçları için sorgu sonuçları sınırladığı için tek seferde döndürülen 1000 varlıkların bir sınır yoktur. Kullanmanız gereken **atla** ve **ele** (.NET) / **üst** (REST) bölümünde anlatıldığı gibi [bu .NET örnek](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) ve [bu REST API örnek](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). 
* Zaman JSON kullanan ve kullanılacağını belirtme **__metadata** anahtar sözcüğü isteğinde (örneğin, bağlantılı nesne başvuru) ayarlamanız gerekir **kabul** başlığına [ayrıntılı JSON biçimi](https://www.odata.org/documentation/odata-version-3-0/json-verbose-format/)(aşağıdaki örneğe bakın). OData anlamak **__metadata** özellik isteğinde sürece, ayrıntılı ayarlayın.  
  
        POST https://media.windows.net/API/Jobs HTTP/1.1
        Content-Type: application/json;odata=verbose
        Accept: application/json;odata=verbose
        DataServiceVersion: 3.0
        MaxDataServiceVersion: 3.0
        x-ms-version: 2.17
        Authorization: Bearer <ENCODED JWT TOKEN> 
        Host: media.windows.net
  
        {
            "Name" : "NewTestJob", 
            "InputMediaAssets" : 
                [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aba5356eb-30ff-4dc6-9e5a-41e4223540e7')"}}]
        . . . 

## <a name="standard-http-request-headers-supported-by-media-services"></a>Media Services tarafından desteklenen standart HTTP isteği üstbilgileri
Media Services'e olun, her çağrı için gerekli üst bilgileri İsteğinizde içermelidir bir dizi yoktur ve aynı zamanda bir dizi isteğe bağlı üst bilgiler dahil etmek istediğiniz. Aşağıdaki tabloda gerekli üst bilgileri listeler:

| Üstbilgi | Tür | Değer |
| --- | --- | --- |
| Yetkilendirme |Taşıyıcı |Taşıyıcı yalnızca kabul edilen yetkilendirme mekanizmadır. Değer, Azure Active Directory tarafından sağlanan erişim belirtecini de içermelidir. |
| x-ms-version |Decimal |2.17 (veya en son sürüm)|
| DataServiceVersion |Decimal |3,0 |
| MaxDataServiceVersion |Decimal |3,0 |

> [!NOTE]
> Media Services REST API'lerini kullanıma sunmak için OData kullandığından, içindeki tüm istekleri DataServiceVersion ve MaxDataServiceVersion üstbilgileri eklenmelidir; değilse, ancak daha sonra şu anda Media Services DataServiceVersion değeri kullanılıyor 3.0 olduğunu varsayar.
> 
> 

Aşağıdaki isteğe bağlı bir üst kümesidir:

| Üstbilgi | Tür | Değer |
| --- | --- | --- |
| Tarih |RFC 1123 tarihi |İstek zaman damgası |
| Kabul et |İçerik türü |Aşağıdaki gibi yanıtı için istenen içerik türü:<p> -application/json;odata=verbose<p> -application/atom + xml şeklindedir<p> Yanıtlar, burada başarılı bir yanıt yükü olarak blob akışı içeren bir blob getirme gibi farklı bir içerik türü olabilir. |
| Kabul kodlama |Gzip, deflate |GZIP ve DEFLATE, uygun olduğunda kodlama. Not: Büyük kaynaklar için Media Services ve bu başlığı yoksay küçülen verileri döndürür. |
| Kabul dil |"en", "es" ve benzeri. |Yanıt için tercih edilen dili belirtir. |
| Accept-Charset |"UTF-8" gibi Charset türü |Varsayılan UTF-8'dir. |
| X-HTTP-Method |HTTP yöntemi |İstemciler veya bir GET çağrısı tünel, bu yöntemleri kullanmak için PUT ya da DELETE gibi HTTP yöntemleri desteklemez güvenlik duvarları sağlar. |
| İçerik türü |İçerik türü |İçerik türü istek gövdesinde PUT veya POST istekleri. |
| İstemci istek kimliği |String |Belirtilen istek tanımlayan bir çağıranın tanımlı bir değer. Belirtilmişse bu değer yanıt iletisinde istek eşleştirmek için bir yol dahil edilir. <p><p>**Önemli**<p>Değerleri 2096b tavan (2k). |

## <a name="standard-http-response-headers-supported-by-media-services"></a>Media Services tarafından desteklenen standart HTTP yanıt üst bilgileri
Aşağıdakileri yapmanızı isteyen kaynak ve gerçekleştirmek istediğiniz eylemi bağlı olarak döndürülen üst bilgiler kümesidir.

| Üstbilgi | Tür | Değer |
| --- | --- | --- |
| İstek Kimliği |String |Geçerli işlem, oluşturulan hizmet için benzersiz bir tanımlayıcı. |
| İstemci istek kimliği |String |Varsa özgün istekteki çağıran tarafından belirtilen bir tanımlayıcı. |
| Tarih |RFC 1123 tarihi |İsteği işleyen tarih. |
| İçerik türü |Değişir |Yanıt gövdesi içerik türü. |
| İçerik kodlama |Değişir |Gzip veya deflate uygun şekilde. |

## <a name="standard-http-verbs-supported-by-media-services"></a>Media Services tarafından desteklenen standart HTTP fiilleri
HTTP yapmak istediğinde kullanılabilir HTTP fiilleri tam bir listesi verilmiştir:

| Fiili | Açıklama |
| --- | --- |
| GET |Bir nesnenin geçerli değerini döndürür. |
| POST |Sağlanan verileri temel alan bir nesne oluşturur veya bir komut gönderir. |
| PUT |Nesneyle değiştirir ve (varsa) adlı bir nesne oluşturur. |
| DELETE |Nesneyi siler. |
| MERGE |Var olan bir nesne belirtilen özellik değişikliklerle güncelleştirir. |
| HEAD |Bir nesnenin GET yanıt meta verileri döndürür. |

## <a name="discover-and-browse-the-media-services-entity-model"></a>Media Services varlık modeli Gözat ve Bul
Media Services varlıkları daha bulunabilir hale getirmek için $metadata işlemi kullanılabilir. Tüm geçerli varlık türleri, varlık özellikleri, ilişkilendirmeleri, İşlevler, Eylemler ve benzeri almanıza olanak tanır. Media Services REST API uç noktanız sonuna $metadata işlemi ekleyerek bu bulma hizmete erişebilir.

 /api/$ meta verileri.

Eklemeniz gerekir "? API version=2.x" meta verilerini bir tarayıcıda görüntülemek istiyorsanız veya x-ms-version üstbilgi İsteğinizde içermez URI'nin sonuna.

## <a name="authenticate-with-media-services-rest-using-azure-active-directory"></a>Media Services REST Azure Active Directory'yi kullanarak kimlik doğrulaması

REST API kimlik doğrulamasını Azure Active Directory (aad) gerçekleştirilir.
Gerekli kimlik doğrulama ayrıntıları, Media Services hesabınız için Azure Portal'dan alma hakkında daha fazla ayrıntı için bkz. [Azure AD kimlik doğrulamasıyla Azure Media Services API'sine erişim](media-services-use-aad-auth-to-access-ams-api.md).

Azure AD kimlik doğrulamasını kullanarak REST API'sine bağlanır kod yazma hakkında ayrıntılar için bkz. makaleyi [REST ile Azure Media Services API'sine erişmek için Azure AD kullanım kimlik doğrulaması](media-services-rest-connect-with-aad.md).

## <a name="next-steps"></a>Sonraki adımlar
Media Services REST API'si ile Azure AD kimlik doğrulaması kullanmayı öğrenmek için bkz: [REST ile Azure Media Services API'sine erişmek için Azure AD kullanım kimlik doğrulaması](media-services-rest-connect-with-aad.md).

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

