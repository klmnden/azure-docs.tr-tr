---
title: Batch Yönetimi çözümlerinin kimlik doğrulaması için Azure Active Directory'yi kullanın. | Microsoft Docs
description: Azure Resource Manager ve Batch kaynak sağlayıcısı ile oluşturulan uygulamalar, Azure AD kimlik doğrulaması.
services: batch
documentationcenter: .net
author: laurenhughes
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/27/2017
ms.author: lahugh
ms.openlocfilehash: 0f6db6d9c86e6da047c45ae7b1c43cf5f55c7e2b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64922846"
---
# <a name="authenticate-batch-management-solutions-with-active-directory"></a>Batch yönetimi çözümleri Active Directory ile kimlik doğrulaması

Azure Batch Management hizmeti çağıran uygulamalar kimlik doğrulaması ile [Azure Active Directory] [ aad_about] (Azure AD). Azure AD sağlayan bir Microsoft'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Azure, müşterilerin, hizmet yöneticileri ve kurumsal kullanıcıların kimlik doğrulaması için kendi Azure AD kullanır.

Batch yönetimi .NET kitaplığı türleri, Batch hesapları, hesap anahtarları, uygulamalar ve uygulama paketleri ile çalışmak için kullanıma sunar. Batch yönetimi .NET kitaplığı, bir Azure kaynak sağlayıcısı istemci ve ile birlikte kullanılan [Azure Resource Manager] [ resman_overview] bu kaynakları programlama yoluyla yönetme. Azure AD ve Batch yönetimi .NET kitaplığı dahil olmak üzere tüm Azure kaynak sağlayıcısı istemci aracılığıyla yapılan isteklerin kimliğini doğrulamak için gerekli [Azure Resource Manager][resman_overview].

Bu makalede, toplu işlem yönetimi .NET kitaplığını kullanan uygulamalardan kimlik doğrulaması için Azure AD kullanarak keşfedin. Azure AD aboneliğinin Yöneticisi veya ortak yönetici, tümleşik kimlik doğrulamasını kullanarak kimlik doğrulaması yapmak için nasıl kullanılacağını göstereceğiz. Kullandığımız [hesap yönetimi] [ acct_mgmt_sample] örnek proje, github'da Azure AD ile Batch yönetimi .NET kitaplığını kullanarak izlenecek yol için kullanılabilir.

Batch yönetimi .NET kitaplığı ve hesap yönetimi örnek kullanma hakkında daha fazla bilgi edinmek için [.NET için Batch Yönetimi istemci kitaplığı ile yönetme Batch hesaplarını ve kotalarını](batch-management-dotnet.md).

## <a name="register-your-application-with-azure-ad"></a>Uygulamanızı Azure AD'ye kaydetme

Azure [Active Directory Authentication Library] [ aad_adal] (ADAL), Azure ad uygulamalarınız içinde kullanmak için bir programlama arabirimi sağlar. ADAL uygulamanızdan çağırmak için bir Azure AD kiracısında uygulamanızı kaydetmeniz gerekir. Uygulamanızı kaydettiğinizde, Azure AD kiracısı içinde bir ad da dahil olmak üzere, uygulamanızla ilgili bilgileri Azure AD'ye sağlayın. Ardından Azure AD uygulamanızın çalışma zamanında Azure AD ile ilişkilendirmek için kullandığınız bir uygulama kimliği sağlar. Uygulama kimliği hakkında daha fazla bilgi için bkz: [uygulaması ve Azure Active Directory'de Hizmet sorumlusu nesneleri](../active-directory/develop/app-objects-and-service-principals.md).

Hesap Yönetimi örnek uygulamayı kaydetmek için adımları izleyin. [bir uygulama eklendiğinde](../active-directory/develop/quickstart-register-app.md) konusundaki [uygulamaları Azure Active Directory ile tümleştirme] [ aad_integrate]. Belirtin **yerel istemci uygulaması** uygulamaya türü. Sektörde standart OAuth 2.0 URI'sini **yeniden yönlendirme URI'si** olduğu `urn:ietf:wg:oauth:2.0:oob`. Ancak, geçerli bir URI belirtebilirsiniz (gibi `http://myaccountmanagementsample`) için **yeniden yönlendirme URI'si**gibi gerçek bir uç nokta olması gerekmez:

![](./media/batch-aad-auth-management/app-registration-management-plane.png)

Kayıt işlemini tamamladıktan sonra uygulama kimliği ve uygulamanız için listelenen nesne (hizmet sorumlusu) kimliği görürsünüz.  

![](./media/batch-aad-auth-management/app-registration-client-id.png)

## <a name="grant-the-azure-resource-manager-api-access-to-your-application"></a>Uygulamanız için Azure Resource Manager API erişimi verme

Ardından, Azure Resource Manager API'si, uygulamanıza erişimi devretmek gerekir. Resource Manager API'si için Azure AD tanımlayıcı **Windows Azure Hizmet Yönetimi API'si**.

Azure portalında aşağıdaki adımları izleyin:

1. Azure portalının sol taraftaki gezinti bölmesinde **tüm hizmetleri**, tıklayın **uygulama kayıtları**, tıklatıp **Ekle**.
2. Listede, uygulamanızın uygulama kayıtları adı arayın:

    ![Uygulamanızın adı arayın](./media/batch-aad-auth-management/search-app-registration.png)

3. Görüntü **ayarları** dikey penceresi. İçinde **API erişimi** bölümünden **gerekli izinler**.
4. Tıklayın **Ekle** için yeni bir gerekli izni ekleyin. 
5. 1\. adımda girin **Windows Azure Hizmet Yönetimi API'si**, bu API sonuçlar listesinden seçin ve tıklayın **seçin** düğmesi.
6. 2\. adımda yanındaki onay kutusunu işaretleyin **kuruluş kullanıcıları olarak erişim Azure Klasik dağıtım modeli**, tıklatıp **seçin** düğmesi.
7. Tıklayın **Bitti** düğmesi.

**Gerekli izinler** dikey penceresi artık gösterir hem ADAL hem de Resource Manager API'leri için uygulamanıza izin verilir. İlk uygulamanızı Azure AD'ye kaydetme izinleri için ADAL varsayılan olarak verilir.

![Azure Resource Manager API'si için temsilci izinleri](./media/batch-aad-auth-management/required-permissions-management-plane.png)

## <a name="azure-ad-endpoints"></a>Azure AD uç noktaları

Batch Yönetimi çözümlerinizi Azure AD ile kimlik doğrulamak için iyi bilinen iki uç nokta gerekir.

- **Azure AD genel uç noktası** belirli bir kiracıda sağlanmadığında, tümleşik kimlik doğrulaması gibi söz konusu olduğunda arabirimi toplama genel kimlik bilgisi sağlanır:

    `https://login.microsoftonline.com/common`

- **Azure Resource Manager uç noktasını** Batch management hizmeti isteklerinin kimliğini doğrulamak için bir belirteç almak için kullanılır:

    `https://management.core.windows.net/`

Hesap Yönetimi örnek uygulama, bu uç noktalar için sabitler tanımlar. Bu sabitler değiştirmeden bırakın:

```csharp
// Azure Active Directory "common" endpoint.
private const string AuthorityUri = "https://login.microsoftonline.com/common";
// Azure Resource Manager endpoint 
private const string ResourceUri = "https://management.core.windows.net/";
```

## <a name="reference-your-application-id"></a>Uygulama Kimliğinize başvurusu 

İstemci uygulamanızın çalışma zamanında Azure AD'ye erişmeye uygulama kimliği (istemci kimliği da bilinir) kullanır. Azure portalında uygulamanızı kaydettikten sonra kayıtlı uygulamanız için Azure AD tarafından sağlanan uygulama kimliği kullanmak için kodunuzu güncelleştirin. Hesap Yönetimi örnek uygulama, uygulama Kimliğiniz için uygun sabiti Azure portalından kopyalayın:

```csharp
// Specify the unique identifier (the "Client ID") for your application. This is required so that your
// native client application (i.e. this sample) can access the Microsoft Azure AD Graph API. For information
// about registering an application in Azure Active Directory, please see "Adding an Application" here:
// https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
private const string ClientId = "<application-id>";
```
Ayrıca kayıt işlemi sırasında belirtilen URI yeniden yönlendirmesi kopyalayın. Yeniden yönlendirme URI'si kodunuzda belirtilen yeniden yönlendirme URI'si uygulama kaydolurken sağladığınız eşleşmesi gerekir.

```csharp
// The URI to which Azure AD will redirect in response to an OAuth 2.0 request. This value is
// specified by you when you register an application with AAD (see ClientId comment). It does not
// need to be a real endpoint, but must be a valid URI (e.g. https://accountmgmtsampleapp).
private const string RedirectUri = "http://myaccountmanagementsample";
```

## <a name="acquire-an-azure-ad-authentication-token"></a>Bir Azure AD kimlik doğrulama belirtecini alma

Hesap Yönetimi örneği Azure AD kiracısında kaydedip örnek kaynak kodu değerleriniz ile güncelleştirin. sonra Azure AD kullanarak kimlik doğrulaması yapmak hazır bir örnektir. Örneği çalıştırdığında, ADAL kimlik doğrulaması belirteci almak çalışır. Bu adımda, Microsoft kimlik bilgilerinizi ister: 

```csharp
// Obtain an access token using the "common" AAD resource. This allows the application
// to query AAD for information that lies outside the application's tenant (such as for
// querying subscription information in your Azure account).
AuthenticationContext authContext = new AuthenticationContext(AuthorityUri);
AuthenticationResult authResult = authContext.AcquireToken(ResourceUri,
                                                        ClientId,
                                                        new Uri(RedirectUri),
                                                        PromptBehavior.Auto);
```

Kimlik bilgilerinizi sağladıktan sonra kimliği doğrulanmış istekler için Batch Yönetimi hizmeti vermek için örnek uygulamayı geçebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Çalıştırma hakkında daha fazla bilgi için [hesap yönetimi örnek uygulama][acct_mgmt_sample], bkz: [.NET için Batch Yönetimi istemci kitaplığı ile yönetme Batch hesaplarını ve kotalarını](batch-management-dotnet.md) .

Azure AD hakkında daha fazla bilgi edinmek için [Azure Active Directory belgeleri](https://docs.microsoft.com/azure/active-directory/). ADAL kullanan gösteren ayrıntılı örnekler kullanılabilir [Azure Kod örnekleri](https://azure.microsoft.com/resources/samples/?service=active-directory) kitaplığı.

Azure AD kullanarak Batch hizmeti uygulama kimliğini doğrulamak için bkz: [Batch hizmeti çözümlerinin kimliğini Active Directory ile](batch-aad-auth.md). 


[aad_about]:../active-directory/fundamentals/active-directory-whatis.md "Azure Active Directory nedir?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]:../active-directory/develop/authentication-scenarios.md "Azure AD için kimlik doğrulama senaryoları"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Uygulamaları Azure Active Directory ile tümleştirme"
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[azure_portal]: https://portal.azure.com
[resman_overview]: ../azure-resource-manager/resource-group-overview.md
