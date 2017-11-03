---
title: "İçeriğinizi AMS REST API kullanarak depolama şifrelemesi ile şifreleme"
description: "İçeriğinizi AMS REST API'lerini kullanarak depolama şifrelemesi ile şifrelemek öğrenin."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: a0a79f3d-76a1-4994-9202-59b91a2230e0
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: 1979f5bf5e8cab88dab5fba49018afacf24504b3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="encrypting-your-content-with-storage-encryption"></a>İçeriğinizi depolama şifrelemesi ile şifreleme

İçeriğinizi yerel olarak AES 256 bit şifreleme kullanarak şifrelemek ve Azure nerede depolanacağını şifrelenen depolama alanına yüklemek için önerilir.

Bu makalede AMS depolama şifrelemesi genel bir bakış sunar ve şifrelenmiş depolama içerik karşıya gösterilmektedir:

* Bir içerik anahtarı oluşturun.
* Bir varlık oluşturun. AssetCreationOption StorageEncryption için varlık oluştururken ayarlayın.
  
     Şifrelenmiş varlıklar içerik anahtarı ile ilişkilendirilmiş olması gerekir.
* İçerik anahtarı varlık için bağlayın.  
* Şifreleme kümesi ilgili parametreleri AssetFile varlıklar üzerinde.

## <a name="considerations"></a>Dikkat edilmesi gerekenler 

Bir depolama şifrelenmiş varlık teslim etmek istiyorsanız, varlığın teslim ilkesini yapılandırmanız gerekir. Varlığınızı akışı önce akış sunucusu depolama şifreleme kaldırır ve belirtilen teslim ilkesini kullanarak içeriğinizi akışlarını. Daha fazla bilgi için bkz: [varlık teslim ilkeleri yapılandırma](media-services-rest-configure-asset-delivery-policy.md).

Varlıklar Media Services erişirken, HTTP istekleri özel üstbilgi alanlarını ve değerlerini ayarlamanız gerekir. Daha fazla bilgi için bkz: [Media Services REST API geliştirme için Kurulum](media-services-rest-how-to-use.md). 

## <a name="connect-to-media-services"></a>Media Services’e bağlanmak

AMS API'sine bağlanma hakkında daha fazla bilgi için bkz: [Azure AD kimlik doğrulaması ile Azure Media Services API erişim](media-services-use-aad-auth-to-access-ams-api.md). 

>[!NOTE]
>Başarıyla https://media.windows.net için bağladıktan sonra başka bir Media Services URI belirleme 301 bir yeniden yönlendirme alırsınız. Yeni bir URI yapılan sonraki çağrılar yapmanız gerekir.

## <a name="storage-encryption-overview"></a>Depolama Şifrelemesi'ne genel bakış
AMS depolama şifrelemesi uygulayan **AES CTRL** mod şifreleme dosyanın tamamı için.  AES-CTRL, rastgele uzunlukta verileri doldurma gerek kalmadan şifreleyebilirsiniz bir blok şifreleme modudur. AES algoritması ve ardından XOR-şifreleme veya şifrelerini çözme lık AES çıktı verileri olan bir sayaç bloğu şifreleyerek çalışır.  Sayaç değerinin 0-7 bayt InitializationVector değerini kopyalayarak kullanılan sayaç blok oluşturulur ve 8-15 baytını sayaç değeri sıfır olarak ayarlanır. Bir sonraki her veri bloğu için artırılır basit 64 bit işaretsiz tamsayıyı işlenir ve ağ bayt sırasında tutulur gibi 16 bayt sayacı bloğunu bayt 8-15 (yani en az önemli bayt) kullanılır. Bu tamsayı sıfırlar artırma en büyük değer (0xFFFFFFFFFFFFFFFF) blok sayaç sıfır bayta (8-15) bir 64 bit etkilemeden (yani 0 ila 7 bayt) sayacı ulaşırsa unutmayın.   AES-CTRL modu şifreleme güvenliğini korumak için her içerik anahtarı için belirli bir anahtar tanımlayıcı InitializationVector değeri her dosya için benzersiz olacaktır ve dosyaları 2'den olacaktır ^ uzunluğu 64 engeller.  Bu sayaç değeri hiçbir zaman verilen anahtarla yeniden sağlamaktır. CTRL modu hakkında daha fazla bilgi için bkz: [Bu wiki sayfa](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CTR) (wiki makalesi "InitializationVector" yerine "Nonce" terimini kullanır).

Kullanım **depolama şifreleme** Temizle içeriğinizi şifrelemek için kullanarak yerel olarak AES 256 bit şifrelemesi ve Azure depolanır şifrelenen depolama birimine yükleyin. Depolama şifrelemesi ile korunan varlıklar otomatik olarak şifrelenmemiş ve kodlamadan önce şifrelenmiş bir dosya sistemine yerleştirilir ve isteğe bağlı olarak yeni çıkış varlığı şeklinde geri bir yüklemeden önce yeniden şifrelenir. Depolama şifrelemesinin birincil kullanım durumu, güçlü şifreleme REST diskte, yüksek kaliteli giriş medya dosyalarınızın güvenliğini sağlamak istediğiniz durumdur.

Media Services, içeriğinizi teslim etmek istediğiniz nasıl bilmesi için depolama şifrelenmiş varlık teslim etmek için varlığın teslim ilkesini yapılandırmanız gerekir. Varlığınızı akışı önce akış sunucusu depolama şifreleme kaldırır ve belirtilen teslim ilkesini (örneğin, AES, ortak şifreleme veya şifreleme) kullanarak içeriğinizi akışlarını.

## <a name="create-contentkeys-used-for-encryption"></a>Şifreleme için kullanılan ContentKeys oluşturma
Şifrelenmiş varlıklar depolama şifreleme anahtarı ile ilişkilendirilmiş olması gerekir. Varlık dosyaları oluşturmadan önce şifreleme için kullanılacak içerik anahtarı oluşturmanız gerekir. Bu bölümde, bir içerik anahtarı oluşturmayı açıklar.

Şifrelenmesini istediğiniz varlık ile ilişkilendireceğiniz içerik anahtarları oluşturmak için genel adımlar verilmiştir. 

1. Depolama şifrelemesi için rastgele bir 32 baytlık AES anahtarı oluşturur. 
   
    Bu, bu varlık ile ilişkili tüm dosyalar aynı içerik anahtarı şifre çözme sırasında kullanmanız gerekecektir anlamına gelir, varlık için içerik anahtarı olacaktır. 
2. Çağrı [GetProtectionKeyId](https://docs.microsoft.com/rest/api/media/operations/rest-api-functions#getprotectionkeyid) ve [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) içerik anahtarınızı şifrelemek için kullanılması gereken doğru X.509 sertifikası almak için yöntemleri.
3. İçerik anahtarınızı X.509 sertifikasının ortak anahtarla şifreler. 
   
   Media Services .NET SDK'sı RSA şifreleme yaparken OAEP ile kullanır.  Bir .NET örnekte görebilirsiniz [EncryptSymmetricKeyData işlevi](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
4. İçerik anahtarı ve anahtarı tanımlayıcısı kullanılarak hesaplanan bir sağlama toplamı değeri oluşturun. Aşağıdaki .NET örnek anahtar tanımlayıcısı ve temizleyin içerik anahtarı GUID parçası kullanarak sağlama toplamı hesaplar.

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

1. İle içerik anahtarı oluşturun **EncryptedContentKey** (base64 ile kodlanmış dizeye dönüştürülen), **ProtectionKeyId**, **ProtectionKeyType**,  **ContentKeyType**, ve **sağlama toplamı** önceki adımlarda almış değerleri.

    Depolama şifrelemesi için aşağıdaki özellikleri istek gövdesinde yer alması gerekir.

    İstek gövdesi özelliği    | Açıklama
    ---|---
    Kimlik | Biz kendisini oluşturan ContentKey kimliği aşağıdaki biçimi kullanarak "nb:kid:UUID:<NEW GUID>".
    ContentKeyType | Bu içerik anahtarı için bir tamsayı olarak içerik anahtar türü budur. Biz depolama şifrelemesi için 1 değerini geçirin.
    EncryptedContentKey | 256 bitlik (32 bayt) bir değer olan yeni bir içerik anahtarı değeri oluşturuyoruz. Anahtar GetProtectionKeyId ve GetProtectionKey yöntemleri için bir HTTP GET isteği yürüterek Microsoft Azure Media Services'den alıyoruz depolama şifreleme X.509 sertifikası kullanılarak şifrelenir. Örnek olarak, aşağıdaki .NET koduna bakın: **EncryptSymmetricKeyData** tanımlanan yöntemi [burada](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
    ProtectionKeyId | Bu, bizim içerik anahtarı şifrelemek için kullanılan depolama şifreleme X.509 Sertifika koruma anahtar kimliğidir.
    ProtectionKeyType | İçerik anahtarı şifrelemek için kullanılan koruma anahtarı şifreleme türü budur. Bu değer StorageEncryption(1) Bizim örneğimizde olur.
    Sağlama toplamı |MD5 hesaplanan sağlama toplamı için içerik anahtarı. İçerik anahtarı kimliği içerikle şifreleyerek hesaplanır. Kod örneği, sağlama toplamı hesaplamak gösterilmiştir.


### <a name="retrieve-the-protectionkeyid"></a>ProtectionKeyId alma
Aşağıdaki örnek, içerik anahtarı şifrelerken kullandığınız sertifika için bir sertifika parmak izini ProtectionKeyId almak nasıl gösterir. Makinenizde uygun sertifika zaten sahip olduğunuzdan emin olmak için bu adımı uygulayın.

İsteği:

    GET https://media.windows.net/api/GetProtectionKeyId?contentKeyType=0 HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net

Yanıtı:

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

### <a name="retrieve-the-protectionkey-for-the-protectionkeyid"></a>ProtectionKey için ProtectionKeyId alma
Aşağıdaki örnek, önceki adımda aldığınız ProtectionKeyId kullanarak X.509 sertifikası almak nasıl gösterir.

İsteği:

    GET https://media.windows.net/api/GetProtectionKey?ProtectionKeyId='7D9BB04D9D0A4A24800CADBFEF232689E048F69C' HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    Host: media.windows.net

Yanıtı:

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
X.509 sertifikası alınır ve ortak anahtar, içerik anahtarı şifrelemek için kullanılan sonra oluşturmanız bir **ContentKey** varlık ve onun özellik değerlerinin buna uygun olarak ayarla.

Ne zaman ayarlamalısınız değerlerden biri oluşturma içerik türü bir anahtardır. Depolama şifrelemesi durumunda, '1' değeridir. 

Aşağıdaki örnekte nasıl oluşturulacağını gösterir bir **ContentKey** ile bir **ContentKeyType** depolama şifrelemenin ("1") ve **ProtectionKeyType** belirtmek için "0" olarak ayarlayın koruma kimliği X.509 sertifika parmak izi anahtardır.  

İstek

    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net
    {
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C", 
    "ContentKeyType":"1", 
    "ProtectionKeyType":"0",
    "EncryptedContentKey":"your encrypted content key",
    "Checksum":"calculated checksum"
    }

Yanıtı:

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
Aşağıdaki örnek, bir varlık oluşturulacağını gösterir.

**HTTP isteği**

    POST https://media.windows.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net

    {"Name":"BigBuckBunny" "Options":1}

**HTTP yanıtı**

Başarılı olursa, aşağıdaki verilir:

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

İsteği:

    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Afbd7ce05-1087-401b-aaae-29f16383c801')/$links/ContentKeys HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    Host: media.windows.net

    {"uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeys('nb%3Akid%3AUUID%3A01e6ea36-2285-4562-91f1-82c45736047c')"}

Yanıtı:

    HTTP/1.1 204 No Content 

## <a name="create-an-assetfile"></a>Bir AssetFile oluşturma
[AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) varlığı temsil eden bir blob kapsayıcısında depolanır ses veya video dosyası. Bir varlık dosyası her zaman bir varlıkla ilişkilidir ve bir varlığı bir veya daha çok varlık dosyaları içerebilir. Bir varlık dosyası nesne bir blob kapsayıcısında dijital bir dosyayla ilişkili değilse Media Services Kodlayıcısı görev başarısız olur.

Unutmayın **AssetFile** örneği ve gerçek medya dosyası olan iki farklı nesneler. Medya dosyasının gerçek medya içeriği içerirken AssetFile örneği medya dosyası hakkındaki meta verileri içerir.

Bir blob kapsayıcıya bir dijital medyayı dosyanızı karşıya sonra kullanacağınız **birleştirme** AssetFile (Bu konuda gösterilmez), ortam dosyası hakkındaki bilgilerle güncelleştirmek için HTTP isteği. 

**HTTP isteği**

    POST https://media.windows.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
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
