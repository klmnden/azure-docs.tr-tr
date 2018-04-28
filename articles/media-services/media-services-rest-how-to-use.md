---
title: Media Services operations REST API genel bakış | Microsoft Docs
description: Media Services REST API genel bakış
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.assetid: a5f1c5e7-ec52-4e26-9a44-d9ea699f68d9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 12/05/2017
ms.author: juliako;johndeu
ms.openlocfilehash: 472408f1c367984d5f4e0e435366c4a0af2e5b34
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="media-services-operations-rest-api-overview"></a>Media Services operations REST API genel bakış
[!INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

**Media Services Operations REST** API işleri, varlıklar, Canlı Kanallar ve diğer kaynakların bir Media Services hesabı oluşturmak için kullanılır. Daha fazla bilgi için bkz: [Media Services Operations REST API Başvurusu](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).

Media Services JSON veya atom + XML biçiminde pub kabul eden bir REST API sağlar. Media Services REST API her istemci Media Services, yanı sıra isteğe bağlı üstbilgi kümesi bağlanırken göndermelidir belirli HTTP üstbilgileri gerektirir. Aşağıdaki bölümlerde üstbilgileri ve ne zaman kullanabilir HTTP fiilleri istekleri oluşturma ve yanıtları Media Services'den alma.

Medya Serivces REST API için kimlik doğrulaması makalesinde özetlenen Azure Active Directory kimlik doğrulaması aracılığıyla yapılır [Azure Media Services API REST ile erişmek için kullanım Azure AD kimlik doğrulaması](media-services-rest-connect-with-aad.md)

## <a name="considerations"></a>Dikkat edilmesi gerekenler

REST kullanırken aşağıdaki maddeler geçerlidir.

* Varlıkları sorgulanırken ortak REST v2 1000 sonuçları için sorgu sonuçları sınırladığından aynı anda döndürülen 1000 varlıkların bir sınırı yoktur. Kullanmanız gereken **atla** ve **ele** (.NET) / **üst** (açıklandığı gibi REST) [bu .NET örnek](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) ve [bu REST API örnek](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). 
* JSON kullanarak ve kullanmak için belirterek **__metadata** anahtar sözcüğü isteğinde (örneğin, bağlantılı nesne başvurusu) ayarlamalısınız **kabul** başlığına [JSON Verbose biçimi](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/)(aşağıdaki örneğe bakın). OData anlamak **__metadata** isteği özelliğinde sürece, ayrıntılı ayarlayın.  
  
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
Medya Hizmetleri içine yaptığınız her çağrı için gerekli başlıklar İsteğinizde içermelidir yoktur ve ayrıca isteğe bağlı üstbilgi kümesi dahil etmek istediğiniz. Aşağıdaki tabloda gerekli üst bilgileri listeler:

| Üst bilgi | Tür | Değer |
| --- | --- | --- |
| Yetkilendirme |Taşıyıcı |Taşıyıcı yalnızca kabul edilen yetkilendirme mekanizmadır. Azure Active Directory tarafından sağlanan erişim belirtecini değeri de dahil etmelisiniz. |
| x-ms-version |Ondalık |2.17 (veya en son sürüm)|
| DataServiceVersion |Ondalık |3.0 |
| MaxDataServiceVersion |Ondalık |3.0 |

> [!NOTE]
> Media Services REST API'lerini kullanıma sunmak için OData kullandığından, DataServiceVersion ve MaxDataServiceVersion üst bilgileri de tüm istekler dahil edilmesi gereken; değilse, ancak daha sonra şu anda Media Services DataServiceVersion değeri kullanımda 3.0 olduğunu varsayar.
> 
> 

İsteğe bağlı üstbilgi kümesi verilmiştir:

| Üst bilgi | Tür | Değer |
| --- | --- | --- |
| Tarih |RFC 1123 tarih |İstek zaman damgası |
| Kabul |İçerik türü |Aşağıdaki gibi yanıtı için istenen içerik türü:<p> -Uygulama/json; odata = verbose<p> -Uygulama/atom + xml<p> Yanıtlar, yükü olarak blob akışı başarılı bir yanıt içerdiği bir blob getirme gibi farklı bir içerik türü olabilir. |
| Kabul kodlama |GZIP, deflate |GZIP ve DEFLATE, uygun olduğunda kodlama. Not: büyük kaynaklar için Media Services bu başlığı yoksay ve küçülen veri döndürür. |
| Kabul dili |"en", "es" ve benzeri. |Yanıt için tercih edilen dili belirtir. |
| Accept-Charset |"UTF-8" gibi Charset türü |Varsayılan UTF-8'dir. |
| X HTTP yöntemi |HTTP yöntemi |İstemcileri veya HTTP yöntemleri GET çağrısıyla tünelli bu yöntemleri kullanmak için PUT veya silme gibi desteklemeyen güvenlik duvarları sağlar. |
| İçerik türü |İçerik türü |PUT veya POST istek gövdesinde içerik türünü ister. |
| İstemci istek kimliği |Dize |Belirtilen istek tanımlayan bir arayan tanımlı bir değer. Belirtilmişse, bu değer yanıt iletisinde isteği eşleştirmek için bir yol olarak dahil edilir. <p><p>**Önemli**<p>Değerleri 2096b tutulabilir (2k). |

## <a name="standard-http-response-headers-supported-by-media-services"></a>Media Services tarafından desteklenen standart HTTP yanıt üstbilgileri
Aşağıdakiler için isteyen kaynak ve gerçekleştirmek için hedeflenen eylem bağlı olarak döndürülebilir üstbilgileri kümesidir.

| Üst bilgi | Tür | Değer |
| --- | --- | --- |
| İstek Kimliği |Dize |Geçerli işlem, oluşturulan hizmet için benzersiz bir tanımlayıcı. |
| İstemci istek kimliği |Dize |Varsa, özgün istekteki çağıran tarafından belirtilen bir tanımlayıcı. |
| Tarih |RFC 1123 tarih |İstek işlenmiş tarih. |
| İçerik türü |Değişir |Yanıt gövdesi içerik türü. |
| İçerik kodlama |Değişir |Gzip veya deflate uygun şekilde. |

## <a name="standard-http-verbs-supported-by-media-services"></a>Media Services tarafından desteklenen standart HTTP fiilleri
HTTP yapmak istediğinde, kullanılabilen HTTP fiilleri tam bir listesi verilmiştir:

| Fiil | Açıklama |
| --- | --- |
| AL |Bir nesnenin geçerli değeri döndürür. |
| YAYINLA |Sağlanan verileri temel alan bir nesne oluşturur veya bir komut gönderir. |
| PUT |Bir nesne değiştirir veya (uygunsa) adlı bir nesne oluşturur. |
| SİL |Bir nesneyi siler. |
| BİRLEŞTİRME |Varolan bir nesne belirtilen özellik değişikliklerle güncelleştirir. |
| HEAD |Bir nesnenin GET yanıt meta verileri döndürür. |

## <a name="discover-and-browse-the-media-services-entity-model"></a>Bul ve Media Services varlık modeli göz atın
Media Services varlıklar daha bulunabilir duruma getirmek için $metadata işlemi kullanılabilir. Tüm geçerli bir varlık türleri, varlık özellikleri, ilişkileri, İşlevler, Eylemler ve benzeri almanıza olanak tanır. Media Services REST API uç noktanızı sonuna $metadata işlemi ekleyerek bu bulma hizmeti erişebilirsiniz.

 /api/$ meta veriler.

Eklemeniz gerekir "? api-version=2.x" meta verileri bir tarayıcıda görüntülemek istediğiniz ya da x-ms-version üstbilgi İsteğinizde içermez URI sonuna.

## <a name="authenticate-with-media-services-rest-using-azure-active-directory"></a>Media Services REST Azure Active Directory'yi kullanarak kimlik doğrulaması

REST API kimlik doğrulaması Azure Active Directory(AAD) gerçekleştirilir.
Azure Portalı'ndan Media Services hesabınız için gerekli kimlik doğrulama ayrıntıları alma hakkında daha fazla bilgi için bkz: [Azure AD kimlik doğrulaması ile Azure Media Services API erişim](media-services-use-aad-auth-to-access-ams-api.md).

Azure AD kimlik doğrulamasını kullanarak REST API bağlayan kodu yazma ayrıntıları görmek için makaleyi [Azure Media Services API REST ile erişmek için kimlik doğrulamasını kullan Azure AD](media-services-rest-connect-with-aad.md).

## <a name="next-steps"></a>Sonraki adımlar
Media Services REST API ile Azure AD kimlik doğrulaması kullanmayı öğrenmek için bkz: [Azure Media Services API REST ile erişmek için kimlik doğrulamasını kullan Azure AD](media-services-rest-connect-with-aad.md).

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

