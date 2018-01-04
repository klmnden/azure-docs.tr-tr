---
title: "Azure Active Directory B2C: istemci sertifikaları kullanılarak güvenli, RESTful hizmeti"
description: "Azure AD B2C özel, REST API talep alışverişlerine istemci sertifikalarını kullanarak güvenli hale getirme"
services: active-directory-b2c
documentationcenter: 
author: yoelhor
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 09/25/2017
ms.author: yoelh
<<<<<<< HEAD
ms.openlocfilehash: 867484799020a4e65844523a88240b3d550c69f7
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: HT
=======
ms.openlocfilehash: 9547ba8c65360a03168ff1b6eba01038554e7fd3
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="secure-your-restful-service-by-using-client-certificates"></a>İstemci sertifikaları kullanılarak güvenli, RESTful hizmeti
İlgili bir makalede, [RESTful hizmetini oluşturmak](active-directory-b2c-custom-rest-api-netfw.md) Azure Active Directory B2C ile etkileşime girer (Azure AD B2C).

Bu makalede, bir istemci sertifikası kullanarak, Azure web uygulamanızın (RESTful API'si) erişimi kısıtlamak nasıl öğrenin. TLS karşılıklı kimlik doğrulaması, bu mekanizma olarak adlandırılan veya *istemci sertifikası kimlik doğrulaması*. Azure AD B2C gibi uygun sertifikalar olan hizmetleri hizmetinize erişebilir.

>[!NOTE]
>Kullanarak RESTful hizmetinizi alabilecek [HTTP temel kimlik doğrulaması](active-directory-b2c-custom-rest-api-netfw-secure-basic.md). Ancak, HTTP temel kimlik doğrulaması bir istemci sertifikası üzerinde daha az güvenli olarak kabul edilir. Bu makalede anlatıldığı gibi istemci sertifikası kimlik doğrulaması kullanarak RESTful hizmeti güvenli hale getirmek için bizim önerilir.

Bu makale ayrıntıları nasıl yapılır:
* İstemci sertifikası kimlik doğrulamasını kullanmak için web uygulamanızı ayarlayın.
* Azure AD B2C ilke anahtarları sertifikasını yükleyin.
* Özel ilkeniz istemci sertifikası kullanacak şekilde yapılandırın.

## <a name="prerequisites"></a>Ön koşullar
* Bölümündeki adımları tamamlamanız [tümleştirmek REST API talep alışverişleri](active-directory-b2c-custom-rest-api-netfw.md) makalesi.
* Geçerli bir sertifika (.pfx dosyası özel anahtarı olan) alın.

## <a name="step-1-configure-a-web-app-for-client-certificate-authentication"></a>1. adım: bir web uygulaması için istemci sertifikası kimlik doğrulamasını yapılandırma
Ayarlamak için **Azure App Service** istemci sertifikaları gerektirmek için web uygulaması ayarlayın `clientCertEnabled` site ayarına *doğru*. Bu değişikliği yapmak için REST API kullanmanız gerekir. Ayar, Azure portalında yönetim deneyimi aracılığıyla kullanılabilir. Ayar, RESTful uygulamanızın üzerinde bulmak için **ayarları** menüsü altında **geliştirme araçları**seçin **kaynak Gezgini**.

>[!NOTE]
>Azure uygulama hizmeti planınızın standart veya daha büyük olduğundan emin olun. Daha fazla bilgi için bkz: [Azure App Service planlarına ayrıntılı genel bakış](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).


Kullanım [Azure kaynak Gezgini (Önizleme)](https://resources.azure.com) ayarlamak için **clientCertEnabled** özelliğine *true*aşağıdaki görüntüde gösterildiği gibi:

![Azure kaynak Gezgini üzerinden clientCertEnabled ayarlama](media/aadb2c-ief-rest-api-netfw-secure-cert/rest-api-netfw-secure-client-cert-resource-explorer.png)

>[!NOTE]
>Ayar hakkında daha fazla bilgi için **clientCertEnabled** özelliği, bkz: [TLS yapılandırma karşılıklı kimlik doğrulaması web uygulamaları için](https://docs.microsoft.com/azure/app-service-web/app-service-web-configure-tls-mutual-auth).

>[!TIP]
>Alternatif olarak, REST API çağrısı oluşturabilir kolaylaştırmak için kullanabileceğiniz [ARMClient](https://github.com/projectkudu/ARMClient) aracı.

## <a name="step-2-upload-your-certificate-to-azure-ad-b2c-policy-keys"></a>2. adım: Azure AD B2C ilke anahtarları sertifikanızı karşıya yükle
Ayarlandıktan sonra `clientCertEnabled` için *doğru*, bir istemci sertifikası, RESTful API'si ile iletişim gerektirir. Edinme, karşıya yükleme ve istemci sertifikasının, Azure AD B2C kiracınızda depolamak için aşağıdakileri yapın: 
1. Azure AD B2C kiracınızda seçin **B2C ayarlarını** > **kimlik deneyimi Framework**.

2. Kiracınızda kullanılabilir anahtarlarını görüntülemek için seçin **İlkesi anahtarları**.

3. **Add (Ekle)** seçeneğini belirleyin.  
    **Bir anahtar oluşturma** penceresi açılır.

4. İçinde **seçenekleri** kutusunda **karşıya**.

5. İçinde **adı** kutusuna **B2cRestClientCertificate**.  
    Önek *B2C_1A_* otomatik olarak eklenir.

6. İçinde **karşıya dosya yükleme** kutusunda, özel bir anahtarla sertifikanızın .pfx dosyasını seçin.

7. İçinde **parola** sertifikanın parolası yazın.

    ![İlke anahtarını karşıya yükleyin](media/aadb2c-ief-rest-api-netfw-secure-cert/rest-api-netfw-secure-client-cert-upload.png)

7. **Oluştur**’u seçin.

8. Kiracınızda kullanılabilir anahtarlarını görüntülemek ve oluşturduğunuz olduğunu onaylamak için `B2C_1A_B2cRestClientCertificate` anahtar, select **İlkesi anahtarları**.

## <a name="step-3-change-the-technical-profile"></a>3. adım: teknik profilini değiştirme
İstemci sertifikası kimlik doğrulaması, özel ilkeniz desteklemek için aşağıdakileri yaparak teknik profili değiştirin:

1. Çalışma dizininizi açın *TrustFrameworkExtensions.xml* uzantı ilke dosyası.

2. Arama `<TechnicalProfile>` içeren düğüm `Id="REST-API-SignUp"`.

3. Bulun `<Metadata>` öğesi.

4. Değişiklik *AuthenticationType* için *ClientCertificate*aşağıdaki gibi:

    ```xml
    <Item Key="AuthenticationType">ClientCertificate</Item>
    ```

5. Kapatma hemen sonra `<Metadata>` öğesi, aşağıdaki XML parçacığını ekleyin: 

    ```xml
    <CryptographicKeys>
        <Key Id="ClientCertificate" StorageReferenceId="B2C_1A_B2cRestClientCertificate" />
    </CryptographicKeys>
    ```

    Kod parçacığını ekledikten sonra teknik profiliniz için aşağıdaki XML kodunu gibi görünmelidir:

    ![ClientCertificate kimlik doğrulaması XML öğeleri ayarlama](media/aadb2c-ief-rest-api-netfw-secure-cert/rest-api-netfw-secure-client-cert-tech-profile.png)

## <a name="step-4-upload-the-policy-to-your-tenant"></a>4. adım: ilke kiracınız için karşıya yükleme

1. İçinde [Azure portal](https://portal.azure.com), geçiş [bağlamı Azure AD B2C kiracınızın](active-directory-b2c-navigate-to-b2c-context.md)ve ardından **Azure AD B2C**.

2. Seçin **kimlik deneyimi Framework**.

3. Seçin **tüm ilkeleri**.

4. Seçin **karşıya İlkesi**.

5. Seçin **varsa ilkesi üzerine** onay kutusu.

6. Karşıya yükleme *TrustFrameworkExtensions.xml* dosya ve ardından doğrulama başarılı olduğundan emin olun.

## <a name="step-5-test-the-custom-policy-by-using-run-now"></a>5. adım: Test Şimdi Çalıştır kullanarak özel İlkesi
1. Açık **Azure AD B2C ayarlarını**ve ardından **kimlik deneyimi Framework**.

    >[!NOTE]
    >Çalışma şimdi Kiracı'preregistered için en az bir uygulama için gereklidir. Uygulamaları kaydetmek öğrenmek için Azure AD B2C bkz [başlama](active-directory-b2c-get-started.md) makale veya [uygulama kaydı](active-directory-b2c-app-registration.md) makale.

2. Açık **B2C_1A_signup_signin**, karşıya ve ardından bağlı olan taraf (RP) özel ilkesini **Şimdi Çalıştır**.

3. Sınama işlemi yazarak **Test** içinde **verilen ad** kutusu.  
    Azure AD B2C penceresinin en üstünde bir hata iletisi görüntüler.    

    ![Kimliğinizi API testi](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-test.png)

4. İçinde **verilen ad** ("Test" dışında) bir ad yazın.  
    Azure AD B2C, kullanıcı oturum açtığında ve ardından uygulamanızı bağlılık birkaç gönderir. Bu JWT örnekte numarasını not edin:

   ```
   {
     "typ": "JWT",
     "alg": "RS256",
     "kid": "X5eXk4xyojNFum1kl2Ytv8dlNP4-c57dO6QGTVBwaNk"
   }.{
     "exp": 1507125903,
     "nbf": 1507122303,
     "ver": "1.0",
     "iss": "https://login.microsoftonline.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
     "aud": "e1d2612f-c2bc-4599-8e7b-d874eaca1ee1",
     "acr": "b2c_1a_signup_signin",
     "nonce": "defaultNonce",
     "iat": 1507122303,
     "auth_time": 1507122303,
     "loyaltyNumber": "290",
     "given_name": "Emily",
     "emails": ["B2cdemo@outlook.com"]
   }
   ```

   >[!NOTE]
   >Hata iletisini alırsanız *adı geçerli değil, Lütfen geçerli bir ad sağlayın*, istemci sertifikası sunulan sırasında Azure AD B2C başarıyla RESTful hizmetinizi adlı anlamına gelir. Sertifikayı doğrulamak için sonraki adım olacaktır.

## <a name="step-6-add-certificate-validation"></a>6. adım: Sertifika doğrulama ekleme
Azure AD B2C RESTful hizmetinize gönderen istemci sertifikası doğrulama sertifikası var olup olmadığını kontrol dışında Azure Web Apps platform tarafından uygulanabilecek değil. Sertifika doğrulanırken web uygulaması sorumluluğundadır. 

Bu bölümde, kimlik doğrulama amacıyla sertifika özellikleri doğrular örnek ASP.NET kodu ekleyin.

> [!NOTE]
>Azure uygulama hizmeti için istemci sertifikası kimlik doğrulaması yapılandırma hakkında daha fazla bilgi için bkz: [TLS yapılandırma karşılıklı kimlik doğrulaması web uygulamaları için](https://docs.microsoft.com/azure/app-service-web/app-service-web-configure-tls-mutual-auth).

### <a name="61-add-application-settings-to-your-projects-webconfig-file"></a>6.1 uygulama ayarları, projenizin web.config dosyasına ekleyin.
Aşağıdaki uygulama ayarları daha önce oluşturduğunuz Visual Studio projesi eklemek *web.config* sonra dosya `appSettings` öğe:

```XML
<add key="ClientCertificate:Subject" value="CN=Subject name" />
<add key="ClientCertificate:Issuer" value="CN=Issuer name" />
<add key="ClientCertificate:Thumbprint" value="Certificate thumbprint" />
```

Sertifikanın Değiştir **konu adı**, **verenin adı**, ve **sertifika parmak izi** sertifika değerlerinizi değerlerle.

### <a name="62-add-the-isvalidclientcertificate-function"></a>6.2 add IsValidClientCertificate işlevi
Açık *Controllers\IdentityController.cs* dosya ve ardından eklemek `Identity` denetleyici sınıfını aşağıdaki işlevi: 

```C#
private bool IsValidClientCertificate()
{
    string ClientCertificateSubject = ConfigurationManager.AppSettings["ClientCertificate:Subject"];
    string ClientCertificateIssuer = ConfigurationManager.AppSettings["ClientCertificate:Issuer"];
    string ClientCertificateThumbprint = ConfigurationManager.AppSettings["ClientCertificate:Thumbprint"];

    X509Certificate2 clientCertInRequest = RequestContext.ClientCertificate;
    if (clientCertInRequest == null)
    {
        Trace.WriteLine("Certificate is NULL");
        return false;
    }

    // Basic verification
    if (clientCertInRequest.Verify() == false)
    {
        Trace.TraceError("Basic verification failed");
        return false;
    }

    // 1. Check the time validity of the certificate
    if (DateTime.Compare(DateTime.Now, clientCertInRequest.NotBefore) < 0 ||
        DateTime.Compare(DateTime.Now, clientCertInRequest.NotAfter) > 0)
    {
        Trace.TraceError($"NotBefore '{clientCertInRequest.NotBefore}' or NotAfter '{clientCertInRequest.NotAfter}' not valid");
        return false;
    }

    // 2. Check the subject name of the certificate
    bool foundSubject = false;
    string[] certSubjectData = clientCertInRequest.Subject.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
    foreach (string s in certSubjectData)
    {
        if (String.Compare(s.Trim(), ClientCertificateSubject) == 0)
        {
            foundSubject = true;
            break;
        }
    }

    if (!foundSubject)
    {
        Trace.TraceError($"Subject name '{clientCertInRequest.Subject}' is not valid");
        return false;
    }
    
    // 3. Check the issuer name of the certificate
    bool foundIssuerCN = false;
    string[] certIssuerData = clientCertInRequest.Issuer.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
    foreach (string s in certIssuerData)
    {
        if (String.Compare(s.Trim(), ClientCertificateIssuer) == 0)
        {
            foundIssuerCN = true;
            break;
        }
    }

    if (!foundIssuerCN)
    {
        Trace.TraceError($"Issuer '{clientCertInRequest.Issuer}' is not valid");
        return false;
    }

    // 4. Check the thumbprint of the certificate
    if (String.Compare(clientCertInRequest.Thumbprint.Trim().ToUpper(), ClientCertificateThumbprint) != 0)
    {
        Trace.TraceError($"Thumbprint '{clientCertInRequest.Thumbprint.Trim().ToUpper()}' is not valid");
        return false;
    }

    // 5. If you also want to test whether the certificate chains to a trusted root authority, you can uncomment the following code:
    //
    //X509Chain certChain = new X509Chain();
    //certChain.Build(certificate);
    //bool isValidCertChain = true;
    //foreach (X509ChainElement chElement in certChain.ChainElements)
    //{
    //    if (!chElement.Certificate.Verify())
    //    {
    //        isValidCertChain = false;
    //        break;
    //    }
    //}
    //if (!isValidCertChain) return false;
    return true;
}
```

Yukarıdaki örnek kodda sertifikası yalnızca aşağıdaki tüm koşullar karşılanıyorsa geçerli olarak kabul:
* Sertifika süresi dolmaz ve sunucusunda geçerli zaman için etkindir.
* `Subject` Sertifikanın adı eşittir `ClientCertificate:Subject` uygulama ayarı değeri.
* `Issuer` Sertifikanın adı eşittir `ClientCertificate:Issuer` uygulama ayarı değeri.
* `thumbprint` 'Nın sertifikasını eşittir `ClientCertificate:Thumbprint` uygulama ayarı değeri.

>[!IMPORTANT]
>Hizmetinizin duyarlılığını bağlı olarak, daha fazla doğrulama eklemeniz gerekebilir. Örneğin, bir güvenilen kök yetkilisi, veren kuruluş adı doğrulama ve benzeri sertifika zincir olup olmadığını sınamak gerekebilir.

### <a name="63-call-the-isvalidclientcertificate-function"></a>6.3 IsValidClientCertificate işlevini çağırın
Açık *Controllers\IdentityController.cs* dosyası ve daha sonra başında `SignUp()` işlev, aşağıdaki kod parçacığını ekleyin: 

```C#
if (IsValidClientCertificate() == false)
{
    return Content(HttpStatusCode.Conflict, new B2CResponseContent("Your client certificate is not valid", HttpStatusCode.Conflict));
}
```

Kod parçacığında ekledikten sonra `Identity` denetleyicisi, aşağıdaki kod gibi görünmelidir:

![Sertifika doğrulama kodu ekleme](media/aadb2c-ief-rest-api-netfw-secure-cert/rest-api-netfw-secure-client-code.png)

## <a name="step-7-publish-your-project-to-azure-and-test-it"></a>7. adım: projeniz için Azure yayımlama ve test
1. İçinde **Çözüm Gezgini**, sağ **Contoso.AADB2C.API** proje ve ardından **Yayımla**.

2. "Adım 6" yineleyin ve özel ilkeniz sertifika doğrulama ile yeniden sınayın. İlke çalıştırmak ve doğrulama ekledikten sonra her şeyi çalıştığından emin olmak deneyin.

3. İçinde *web.config* dosya, değerini değiştirme `ClientCertificate:Subject` için **geçersiz**. İlkeyi yeniden çalıştırın ve bir hata iletisi görürsünüz.

4. Değer başa değişiklik **geçerli**ve ilke REST API'si çağırabilirsiniz emin olun.

Bu adım sorun giderme gerekiyorsa, bkz: [Application Insights kullanarak günlükleri toplama](active-directory-b2c-troubleshoot-custom.md).

## <a name="optional-download-the-complete-policy-files-and-code"></a>(İsteğe bağlı) Kod ve tam ilke dosyaları indirme
* Tamamlandıktan sonra [özel ilkelerini kullanmaya başlama](active-directory-b2c-get-started-custom.md) gözden geçirme, öneririz, kendi özel ilke dosyalarını kullanarak senaryonuz derleme. Başvuru için sağladık [örnek ilke dosyaları](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw-secure-cert).
* Tam koddan indirebilirsiniz [başvurusu için örnek Visual Studio çözümü](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw/Contoso.AADB2C.API). 
