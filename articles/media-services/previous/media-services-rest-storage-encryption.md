---
title: İçeriğinizi AMS REST API kullanarak depolama şifreleme ile şifreleme
description: AMS REST API'lerini kullanarak depolama şifreleme ile içeriğinizi şifrelemeyi öğreneceksiniz.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: a0a79f3d-76a1-4994-9202-59b91a2230e0
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: 30ac6a94142c9b9d987fb3fd32b3483cc6dc130c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64867587"
---
# <a name="encrypting-your-content-with-storage-encryption"></a>İçeriğinizi depolama şifreleme ile şifreleme 

> [!NOTE]
> Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Ayrıntılar için bkz [Azure ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/).   > Hiçbir yeni özellikler veya işlevsellikler eklenmektedir Media Services v2 ile. <br/>En son sürüm olan [Media Services v3](https://docs.microsoft.com/azure/media-services/latest/)’ü inceleyin. Ayrıca bkz [geçiş kılavuzuna v2'den v3](../latest/migrate-from-v2-to-v3.md)
>   

İçeriğinizi yerel olarak AES-256 bit şifreleme kullanarak şifrelemek ve bekleme sırasında şifrelenmiş Azure depolandığı depolama yüklemek için önerilir.

Bu makalede, AMS depolama şifrelemesi genel bir bakış sağlar ve depolama şifreli içeriği karşıya yükleme işlemini gösterir:

* Bir içerik anahtarı oluşturun.
* Bir varlık oluşturun. Varlık oluşturmak için StorageEncryption AssetCreationOption ayarlanır.
  
     Şifrelenmiş içerik anahtarı ile ilişkili varlıklardır.
* İçerik anahtarı varlığına bağlayın.  
* Şifrelemeyle ilgili parametreleri AssetFile varlıklarda ayarlayın.

## <a name="considerations"></a>Dikkat edilmesi gerekenler 

Bir depolama şifrelenmiş varlık iletmek istiyorsanız, varlık teslim ilkesini yapılandırmanız gerekir. Varlığınızı akışla önce akış sunucusu depolama şifrelemesi kaldırır ve belirtilen teslim ilkesini kullanarak içeriğinizi akışları. Daha fazla bilgi için [varlık teslim ilkelerini yapılandırma](media-services-rest-configure-asset-delivery-policy.md).

Varlıklar Media Services erişirken, HTTP isteklerini özel üstbilgi alanlarını ve değerlerini ayarlamanız gerekir. Daha fazla bilgi için [Media Services REST API geliştirme için Kurulum](media-services-rest-how-to-use.md). 

### <a name="storage-side-encryption"></a>Depolama tarafında şifreleme

|Şifreleme seçeneği|Açıklama|Media Services v2|Media Services v3|
|---|---|---|---|
|Media Services'ı depolama şifrelemesi|Media Services tarafından yönetilen AES-256 şifreleme anahtarı|Desteklenen<sup>(1)</sup>|Desteklenmeyen<sup>(2)</sup>|
|[Bekleyen veriler için depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Sunucu tarafı şifrelemesi, Azure Depolama tarafından sunulan anahtarı Azure tarafından veya müşteri tarafından yönetilen|Desteklenen|Desteklenen|
|[Depolama istemci tarafı şifreleme](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Azure depolama, anahtar Kasası'nda müşteri tarafından yönetilen bir kiracı anahtarı tarafından sunulan istemci tarafı şifreleme|Desteklenmiyor|Desteklenmiyor|

<sup>1</sup> sırada Media Services, işleme içeriğinin desteklemez, açıkta/herhangi bir biçimde şifreleme olmadan, bunu yapmanız bu nedenle önerilmez.

<sup>2</sup> , Media Services v3 (AES-256 şifreleme) depolama şifrelemesi, yalnızca varlıklarınızı Media Services v2 ile oluşturulduğunda için geriye dönük uyumluluk desteklenir. Var olan depolama ile v3 çalışır anlamı varlıklar şifreli ancak yenilerini oluşturulmasına izin vermez.

## <a name="connect-to-media-services"></a>Media Services’e bağlanmak

AMS API'ye bağlanma hakkında daha fazla bilgi için bkz: [Azure AD kimlik doğrulamasıyla Azure Media Services API'sine erişim](media-services-use-aad-auth-to-access-ams-api.md). 

## <a name="storage-encryption-overview"></a>Depolama şifrelemesi genel bakış
AMS depolama şifrelemesi uygulayan **AES-CTRL** dosyanın tamamı için şifreleme modu.  AES-CTRL doldurma için gerek kalmadan rastgele uzunluktaki verileri şifrelemek bir blok şifreleme modudur. AES algoritması ve ardından XOR-şifrelemek veya şifresini çözmek için ing AES çıktısını verilerle bir sayaç bloğu şifreleyerek çalışır.  Sayaç değeri 0 ile 7 bayt InitializationVector değerini kopyalayarak kullanılan sayaç blok oluşturulur ve bayt 8-15 sayaç değeri sıfır olarak ayarlanır. Bir sonraki her veri bloğu için artan bir basit 64-bit işaretsiz tamsayıyı işlenir ve ağ sırasıyla tutulur 16 baytlık sayacı bloğunu bayt 8-15 (diğer bir deyişle, en az önemli bayt) kullanılır. Bu tamsayı en büyük değeri (0xFFFFFFFFFFFFFFFF) ulaşırsa da artırmadan diğer 64 bit (diğer bir deyişle, bayt 0 ile 7) sayacın etkilemeden (bayt olarak 8 ile 15) blok sayacı sıfırlar.   CTRL AES şifrelemesi güvenliğini korumak için her bir içerik anahtarı için belirli bir anahtar tanımlayıcı InitializationVector değeri her dosya için benzersiz olacaktır ve dosyaları, 2'den olacaktır ^ 64 bloklarında uzunluğu.  Bu benzersiz bir değer, bir sayaç değeri hiçbir zaman belirli bir anahtar ile yeniden sağlamaktır. CTRL modu hakkında daha fazla bilgi için bkz. [Bu wiki sayfası](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CTR) (wiki makalesinde "InitializationVector" yerine "Nonce" terimini kullanır).

Kullanım **depolama şifrelemesi** Temizle içeriğinizi şifrelemek için AES-256'yı kullanarak yerel olarak bit şifreleme ve Azure bunu depolandığı bekleme sırasında şifrelenmiş depolama alanına yükleyin. Depolama şifrelemesi ile korunan varlıklar otomatik olarak şifrelenerek ve kodlama önce şifrelenmiş bir dosya sistemine yerleştirilir ve isteğe bağlı olarak yeni çıktı varlığı şeklinde geri bir yüklemeden önce yeniden şifrelenir. Depolama şifrelemesinin birincil kullanım durumu, yüksek kaliteli girdi medya dosyalarınızı bekleyen güçlü şifrelemeyle diskte güvenliğini sağlamak istediğiniz durumdur.

Bir depolama şifrelenmiş varlık teslim etmek için Media Services, içeriğinizi teslim etmek istediğiniz nasıl olduğunu bilmesi için varlık teslim ilkesini yapılandırmanız gerekir. Varlığınızı akışla önce akış sunucusu depolama şifrelemesi kaldırır ve belirtilen teslim ilkesini (örneğin, AES, ortak şifreleme veya şifreleme) kullanarak içeriğinizi akışları.

## <a name="create-contentkeys-used-for-encryption"></a>Şifreleme için kullanılan ContentKeys oluşturma
Şifrelenmiş depolama şifreleme anahtarlarıyla ilgili önemli varlıklardır. Varlık dosyaları oluşturmadan önce şifreleme için kullanılacak içerik anahtarı oluşturun. Bu bölümde, içerik anahtarının nasıl oluşturulacağı açıklanmaktadır.

Şifrelenmesini istediğiniz varlıkların ile ilişkilendirmek, içerik anahtarı oluşturmak için genel adımlar aşağıdadır. 

1. Depolama şifrelemesi için rastgele bir 32 bayt AES anahtarı oluşturur. 
   
    32 bayt AES şifre çözme sırasında aynı içerik anahtarı kullanan varlık gereken ilişkilendirilmiş tüm dosyalar anlamına gelir, varlık için içerik anahtarını anahtardır. 
2. Çağrı [GetProtectionKeyId](https://docs.microsoft.com/rest/api/media/operations/rest-api-functions#getprotectionkeyid) ve [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) içerik anahtarınızı şifrelemek için kullanılacak doğru X.509 sertifikası almak için yöntemleri.
3. İçerik anahtarınız X.509 sertifikasının ortak anahtarla şifreler. 
   
   Media Services .NET SDK ile OAEP RSA şifreleme yaparken kullanır.  Bir .NET örnekte gördüğünüz [EncryptSymmetricKeyData işlevi](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
4. Anahtar tanımlayıcısı ve içerik anahtarı kullanarak hesaplanan bir sağlama toplamı değeri oluşturun. Aşağıdaki .NET örnek anahtar tanımlayıcısı ve Temizle içerik anahtarı GUID kısmını kullanarak sağlama toplamını hesaplar.

    ```csharp
            public static string CalculateChecksum(byte[] contentKey, Guid keyId)
            {
                const int ChecksumLength = 8;
                const int KeyIdLength = 16;

                byte[] encryptedKeyId = null;

                // Checksum is computed by AES-ECB encrypting the KID
                // with the content key.
                using (AesCryptoServiceProvider rijndael = new AesCryptoServiceProvider())
                {
                    rijndael.Mode = CipherMode.ECB;
                    rijndael.Key = contentKey;
                    rijndael.Padding = PaddingMode.None;

                    ICryptoTransform encryptor = rijndael.CreateEncryptor();
                    encryptedKeyId = new byte[KeyIdLength];
                    encryptor.TransformBlock(keyId.ToByteArray(), 0, KeyIdLength, encryptedKeyId, 0);
                }

                byte[] retVal = new byte[ChecksumLength];
                Array.Copy(encryptedKeyId, retVal, ChecksumLength);

                return Convert.ToBase64String(retVal);
            }
    ```

5. İçerik anahtarı ile oluşturmak **EncryptedContentKey** (base64 ile kodlanmış dizeye dönüştürülür) **ProtectionKeyId**, **ProtectionKeyType**,  **ContentKeyType**, ve **sağlama toplamı** önceki adımlarda almış değerleri.

    Depolama şifrelemesi için aşağıdaki özellikleri, istek gövdesinde eklenmelidir.

    İstek gövdesi özelliği    | Açıklama
    ---|---
    Kimlik | Aşağıdaki biçimi kullanarak oluşturulan ContentKey kimliği "nb:kid:UUID:\<yeni GUID >".
    ContentKeyType | İçerik anahtarı anahtarı tanımlayan bir tamsayı türüdür. Depolama şifreleme biçimini, değer 1'dir.
    EncryptedContentKey | 256 bitlik (32 bayt) bir değer olan yeni bir içerik anahtar değer oluştururuz. Anahtarı, Microsoft Azure Media Services'den GetProtectionKeyId ve GetProtectionKey yöntemleri için bir HTTP GET isteği yürüterek alıyoruz depolama şifreleme X.509 sertifikası kullanılarak şifrelenir. Örneğin, aşağıdaki .NET kodu görmek: **EncryptSymmetricKeyData** tanımlanan yöntemi [burada](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
    ProtectionKeyId | Bu koruma, anahtar kimliği bizim içerik anahtarını şifrelemek için kullanılan depolama şifreleme X.509 sertifikası.
    ProtectionKeyType | İçerik anahtarını şifrelemek için kullanılan koruma anahtarı için şifreleme türü budur. Bu değer, Bizim örneğimizde StorageEncryption(1) olur.
    Sağlama toplamı |Hesaplanan için MD5 sağlama toplamı içerik anahtarı. İçerik anahtarı ile içerik Kimliğini şifreleyerek hesaplanır. Örnek kod, sağlama toplamını hesaplamak gösterilmektedir.


### <a name="retrieve-the-protectionkeyid"></a>ProtectionKeyId alma
Aşağıdaki örnek, içerik anahtarınız şifrelerken kullanmak sertifika için bir sertifika parmak izi ProtectionKeyId almak gösterilmektedir. Makinenizde uygun sertifikayı zaten sahip olduğunuzdan emin olmak için bu adımı uygulayın.

İstek:

    GET https://media.windows.net/api/GetProtectionKeyId?contentKeyType=0 HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.17
    Host: media.windows.net

Yanıt:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 139
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    x-ms-request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:42:52 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String","value":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C"}

### <a name="retrieve-the-protectionkey-for-the-protectionkeyid"></a>İçin ProtectionKeyId ProtectionKey alma
Aşağıdaki örnek, önceki adımda aldığınız ProtectionKeyId kullanılarak X.509 sertifikası almak gösterilmektedir.

İstek:

    GET https://media.windows.net/api/GetProtectionKey?ProtectionKeyId='7D9BB04D9D0A4A24800CADBFEF232689E048F69C' HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.17
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    Host: media.windows.net

Yanıt:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1227
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    x-ms-request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 05 Feb 2015 07:52:30 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String",
    "value":"MIIDSTCCAjGgAwIBAgIQqf92wku/HLJGCbMAU8GEnDANBgkqhkiG9w0BAQQFADAuMSwwKgYDVQQDEyN3YW1zYmx1cmVnMDAxZW5jcnlwdGFsbHNlY3JldHMtY2VydDAeFw0xMjA1MjkwNzAwMDBaFw0zMjA1MjkwNzAwMDBaMC4xLDAqBgNVBAMTI3dhbXNibHVyZWcwMDFlbmNyeXB0YWxsc2VjcmV0cy1jZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzR0SEbXefvUjb9wCUfkEiKtGQ5Gc328qFPrhMjSo+YHe0AVviZ9YaxPPb0m1AaaRV4dqWpST2+JtDhLOmGpWmmA60tbATJDdmRzKi2eYAyhhE76MgJgL3myCQLP42jDusWXWSMabui3/tMDQs+zfi1sJ4Ch/lm5EvksYsu6o8sCv29VRwxfDLJPBy2NlbV4GbWz5Qxp2tAmHoROnfaRhwp6WIbquk69tEtu2U50CpPN2goLAqx2PpXAqA+prxCZYGTHqfmFJEKtZHhizVBTFPGS3ncfnQC9QIEwFbPw6E5PO5yNaB68radWsp5uvDg33G1i8IT39GstMW6zaaG7cNQIDAQABo2MwYTBfBgNVHQEEWDBWgBCOGT2hPhsvQioZimw8M+jOoTAwLjEsMCoGA1UEAxMjd2Ftc2JsdXJlZzAwMWVuY3J5cHRhbGxzZWNyZXRzLWNlcnSCEKn/dsJLvxyyRgmzAFPBhJwwDQYJKoZIhvcNAQEEBQADggEBABcrQPma2ekNS3Wc5wGXL/aHyQaQRwFGymnUJ+VR8jVUZaC/U/f6lR98eTlwycjVwRL7D15BfClGEHw66QdHejaViJCjbEIJJ3p2c9fzBKhjLhzB3VVNiLIaH6RSI1bMPd2eddSCqhDIn3VBN605GcYXMzhYp+YA6g9+YMNeS1b+LxX3fqixMQIxSHOLFZ1G/H2xfNawv0VikH3djNui3EKT1w/8aRkUv/AAV0b3rYkP/jA1I0CPn0XFk7STYoiJ3gJoKq9EMXhit+Iwfz0sMkfhWG12/XO+TAWqsK1ZxEjuC9OzrY7pFnNxs4Mu4S8iinehduSpY+9mDd3dHynNwT4="}

### <a name="create-the-content-key"></a>İçerik anahtarı oluşturun
X.509 sertifikası alınır ve kendi ortak anahtar, içerik anahtarını şifrelemek için kullanılan sonra oluşturma bir **ContentKey** varlık ve değerlerini uygun şekilde kendi özellik kümesi.

İçeriği ne zaman ayarlamalısınız değerlerinden oluşturma türü anahtardır. Depolama şifrelemesi kullanırken, değeri '1' olarak ayarlanması gerekir. 

Aşağıdaki örnek nasıl oluşturulacağını gösterir. bir **ContentKey** ile bir **ContentKeyType** ayarlamak için depolama şifrelemesi ("1") ve **ProtectionKeyType** göstermek için "0" olarak ayarlayın koruma anahtarı X.509 sertifika parmak izini kimliğidir.  

İstek

    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.17
    Host: media.windows.net
    {
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C", 
    "ContentKeyType":"1", 
    "ProtectionKeyType":"0",
    "EncryptedContentKey":"your encrypted content key",
    "Checksum":"calculated checksum"
    }

Yanıt:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 777
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/ContentKeys('nb%3Akid%3AUUID%3A9c8ea9c6-52bd-4232-8a43-8e43d8564a99')
    Server: Microsoft-IIS/8.5
    request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    x-ms-request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:37:46 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeys/@Element",
    "Id":"nb:kid:UUID:9c8ea9c6-52bd-4232-8a43-8e43d8564a99","Created":"2015-02-04T02:37:46.9684379Z",
    "LastModified":"2015-02-04T02:37:46.9684379Z",
    "ContentKeyType":1,
    "EncryptedContentKey":"your encrypted content key",
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C",
    "ProtectionKeyType":0,
    "Checksum":"calculated checksum"}

## <a name="create-an-asset"></a>Bir varlık oluşturun
Aşağıdaki örnek, bir varlık oluşturma işlemi gösterilmektedir.

**HTTP isteği**

    POST https://media.windows.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.17
    Host: media.windows.net

    {"Name":"BigBuckBunny" "Options":1}

**HTTP yanıtı**

Başarılı olursa, aşağıdaki yanıt döndürülür:

    HTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 452
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    request-id: e98be122-ae09-473a-8072-0ccd234a0657
    x-ms-request-id: e98be122-ae09-473a-8072-0ccd234a0657
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny.mp4",
       "Options":1,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }

## <a name="associate-the-contentkey-with-an-asset"></a>ContentKey bir varlıkla ilişkilendirme
ContentKey oluşturduktan sonra aşağıdaki örnekte gösterildiği gibi $links işlemi kullanarak, varlıkla ilişkilendirin:

İstek:

    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Afbd7ce05-1087-401b-aaae-29f16383c801')/$links/ContentKeys HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.17
    Host: media.windows.net

    {"uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeys('nb%3Akid%3AUUID%3A01e6ea36-2285-4562-91f1-82c45736047c')"}

Yanıt:

    HTTP/1.1 204 No Content 

## <a name="create-an-assetfile"></a>Bir AssetFile oluşturma
[AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) varlığı, bir blob kapsayıcısında depolanan bir video veya ses dosyasını temsil eder. Bir varlık dosyası her zaman bir varlıkla ilişkilidir ve bir varlık, bir veya daha çok varlık dosyaları içerebilir. Bir varlık dosyası nesne bir blob kapsayıcısında dijital bir dosyayla ilişkili değilse, medya Hizmetleri Kodlayıcı görev başarısız olur.

**AssetFile** örneği ve gerçek medya dosyası olan iki farklı bir nesne. Medya dosyası gerçek medya içeriği içerirken AssetFile örneği medya dosyası hakkındaki meta verileri içerir.

Bir blob kapsayıcıya dijital medya dosyanızı karşıya yüklenmesinin ardından, kullanacağınız **birleştirme** (Bu makalede gösterilen değil), medya dosyası hakkındaki bilgilerle AssetFile güncelleştirmeye yönelik HTTP isteği. 

**HTTP isteği**

    POST https://media.windows.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.17
    Host: media.windows.net
    Content-Length: 164

    {  
       "IsEncrypted":"true",
       "EncryptionScheme" : "StorageEncryption", 
       "EncryptionVersion" : "1.0",       
       "EncryptionKeyId" : "nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector" : "397304628502661816</d:InitializationVector",
       "Options":0,
       "IsPrimary":"false",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }

**HTTP yanıtı**

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 535
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5')
    Server: Microsoft-IIS/8.5
    request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    x-ms-request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 00:34:07 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Files/@Element",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "Name":"BigBuckBunny.mp4",
       "ContentFileSize":"0",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "EncryptionVersion": "1.0",
       "EncryptionScheme": "StorageEncryption",
       "IsEncrypted":true,
       "EncryptionKeyId":"nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector":"397304628502661816</d:InitializationVector",
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }
