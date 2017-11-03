---
title: "Toplu işlem yönetimi çözümlerinin kimlik doğrulaması için Azure Active Directory kullanmak | Microsoft Docs"
description: "Uygulamaları Azure resource manager ile oluşturulmuş ve toplu kaynak sağlayıcısı Azure AD ile kimlik doğrulaması."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/27/2017
ms.author: tamram
ms.openlocfilehash: 26d4adf4f74f9aacc4cf8cf24be293ebdb4d63c8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="authenticate-batch-management-solutions-with-active-directory"></a>Toplu işlem yönetimi çözümleri Active Directory ile kimlik doğrulaması

Azure Batch yönetim hizmeti çağıran uygulamalar kimlik doğrulaması ile [Azure Active Directory] [ aad_about] (Azure AD). Azure AD, Microsoft'un çok kiracılı bulut tabanlı dizin ve Kimlik Yönetimi Hizmeti ' dir. Azure kendisini müşteriler, hizmet yöneticileri ve kuruluş kullanıcıların kimlik doğrulaması için Azure AD kullanır.

Batch yönetimi .NET kitaplığı, Batch hesaplarını, hesabı anahtarları, uygulamalar ve uygulama paketleri ile çalışmak için türü ortaya çıkarır. Batch yönetimi .NET kitaplığı bir Azure kaynak sağlayıcısı istemci ve ile birlikte kullanılan [Azure Resource Manager] [ resman_overview] bu kaynakları programlı olarak yönetmek için. Azure AD ve toplu işlem yönetimi .NET kitaplığı dahil olmak üzere tüm Azure kaynak sağlayıcısı istemci aracılığıyla yapılan istekleri kimlik doğrulaması için gerekli olduğunu [Azure Resource Manager][resman_overview].

Bu makalede, Azure AD Batch yönetimi .NET kitaplığı kullanan uygulamalardan kimlik doğrulaması kullanmayı keşfedin. Azure AD Abonelik Yöneticisi veya ortak yönetici, tümleşik kimlik doğrulaması kullanarak kimlik doğrulaması için nasıl kullanılacağını gösteriyoruz. Kullanırız [AccountManagment] [ acct_mgmt_sample] örnek proje, github'da Azure AD ile Batch yönetimi .NET kitaplığını kullanarak izlenecek yol için kullanılabilir.

Batch yönetimi .NET kitaplığı ve AccountManagement örnek kullanma hakkında daha fazla bilgi edinmek için [yönetmek Batch hesaplarını ve kotalarını .NET için Batch Yönetimi istemci kitaplığı ile](batch-management-dotnet.md).

## <a name="register-your-application-with-azure-ad"></a>Azure AD ile uygulamanızı kaydetme

Azure [Active Directory kimlik doğrulama Kitaplığı] [ aad_adal] (ADAL) Azure ad uygulamalarınız içinde kullanmak için programa dayalı bir arabirim sağlar. ADAL uygulamanızdan çağırmak için bir Azure AD kiracısında uygulamanızı kaydetmeniz gerekir. Uygulamanızı kaydederken, Azure AD kiracısı içinde bir ad da dahil olmak üzere uygulamanız hakkındaki bilgilerle Azure AD sağlayın. Ardından Azure AD çalışma zamanında Azure AD ile uygulamanızı ilişkilendirmek için kullandığınız bir uygulama kimliği sağlar. Uygulama kimliği hakkında daha fazla bilgi için bkz: [uygulama ve hizmet asıl nesneler Azure Active Directory'de](../active-directory/develop/active-directory-application-objects.md).

AccountManagement örnek uygulama kaydetmek için adımları [bir uygulama ekleme](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) bölümüne [uygulamaları Azure Active Directory ile tümleştirme][aad_integrate]. Belirtin **yerel istemci uygulaması** uygulama türü için. Endüstri Standart OAuth 2.0 URI'sini **yeniden yönlendirme URI'si** olan `urn:ietf:wg:oauth:2.0:oob`. Ancak, geçerli bir URI belirtebilirsiniz (gibi `http://myaccountmanagementsample`) için **yeniden yönlendirme URI'si**gibi gerçek bir uç nokta olması gerekmez:

![](./media/batch-aad-auth-management/app-registration-management-plane.png)

Kayıt işlemini tamamladıktan sonra uygulama kimliği ve uygulamanız için listelenen nesne (hizmet sorumlusu) kimliği görürsünüz.  

![](./media/batch-aad-auth-management/app-registration-client-id.png)

## <a name="grant-the-azure-resource-manager-api-access-to-your-application"></a>Uygulamanız için Azure Kaynak Yöneticisi API'si erişim

Ardından, Azure Resource Manager API uygulamanıza erişimi temsilci gerekir. Kaynak Yöneticisi API'si için Azure AD tanımlayıcısıdır **Windows Azure Hizmet Yönetimi API'si**.

Azure portalında aşağıdaki adımları izleyin:

1. Azure portalının sol gezinti bölmesinde seçin **daha Hizmetleri**, tıklatın **uygulama kayıtlar**, tıklatıp **Ekle**.
2. Listedeki uygulamanızın uygulama kayıtların adını arayın:

    ![Uygulama adınız arayın](./media/batch-aad-auth-management/search-app-registration.png)

3. Görüntü **ayarları** dikey. İçinde **API erişimini** bölümünde, select **gerekli izinleri**.
4. Tıklatın **Ekle** yeni bir gerekli izin eklemek için. 
5. 1. adımda girin **Windows Azure Hizmet Yönetimi API'si**, bu API sonuçları listesinden seçin ve tıklatın **seçin** düğmesi.
6. 2. adımda yanındaki onay kutusunu işaretleyin **erişim Azure Klasik dağıtım modeli kuruluş kullanıcılar olarak**, tıklatıp **seçin** düğmesi.
7. Tıklatın **Bitti** düğmesi.

**Gerekli izinler** dikey şimdi gösterir, uygulamanız için ADAL hem Resource Manager API'leri için izinlerin verildiğinden emin. Uygulamanızı Azure AD ile ilk kez kaydederken izinleri ADAL için varsayılan olarak verilir.

![Azure Kaynak Yöneticisi API'si temsilci izinleri](./media/batch-aad-auth-management/required-permissions-management-plane.png)

## <a name="azure-ad-endpoints"></a>Azure AD uç noktaları

Toplu işlem yönetimi çözümlerinizi Azure AD ile kimlik doğrulaması yapmak için iyi bilinen iki uç nokta gerekir.

- **Azure AD ortak uç nokta** belirli bir kiracı sağlanmadığında tümleşik kimlik doğrulaması gibi söz konusu olduğunda arabirimi toplama genel bir kimlik bilgisi sağlanır:

    `https://login.microsoftonline.com/common`

- **Azure Kaynak Yöneticisi uç noktası** toplu yönetim hizmeti isteklerine kimlik doğrulaması için bir belirteç almak üzere kullanılır:

    `https://management.core.windows.net/`

Bu uç noktalar için sabitleri AccountManagement örnek uygulama tanımlar. Bu sabitleri değiştirmeden bırakın:

```csharp
// Azure Active Directory "common" endpoint.
private const string AuthorityUri = "https://login.microsoftonline.com/common";
// Azure Resource Manager endpoint 
private const string ResourceUri = "https://management.core.windows.net/";
```

## <a name="reference-your-application-id"></a>Uygulama Kimliğinizi başvurusu 

İstemci uygulamanızı Azure AD çalışma zamanında erişmek için uygulama kimliği (istemci kimliği olarak da bilinir) kullanır. Azure Portal'da uygulamanızın kaydedildikten sonra kayıtlı uygulamanız için Azure AD tarafından sağlanan uygulama kimliği kullanmak için kodunuzu güncelleştirin. AccountManagement örnek uygulama için uygun sabiti Azure portalından uygulama Kimliğinizi kopyalayın:

```csharp
// Specify the unique identifier (the "Client ID") for your application. This is required so that your
// native client application (i.e. this sample) can access the Microsoft Azure AD Graph API. For information
// about registering an application in Azure Active Directory, please see "Adding an Application" here:
// https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
private const string ClientId = "<application-id>";
```
Ayrıca kayıt işlemi sırasında belirtilen URI yeniden yönlendirme kopyalayın. URI kodunuzda belirtilen yeniden yönlendirme yeniden yönlendirilen uygulamayı kaydolurken sağladığınız URI eşleşmesi gerekir.

```csharp
// The URI to which Azure AD will redirect in response to an OAuth 2.0 request. This value is
// specified by you when you register an application with AAD (see ClientId comment). It does not
// need to be a real endpoint, but must be a valid URI (e.g. https://accountmgmtsampleapp).
private const string RedirectUri = "http://myaccountmanagementsample";
```

## <a name="acquire-an-azure-ad-authentication-token"></a>Azure AD kimlik doğrulama belirtecini alma

Azure AD kiracısında AccountManagement örnek kaydetmek ve değerleri ile örnek kaynak kodunu güncelleştirin sonra Azure AD kullanarak kimlik doğrulaması için hazır bir örnektir. Örneği çalıştırdığınızda, ADAL kimlik doğrulama belirtecini almayı dener. Bu adımda, Microsoft kimlik bilgilerinizi ister: 

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

Kimlik bilgilerinizi sağlayın sonra örnek uygulaması Batch yönetim hizmeti kimliği doğrulanmış istekler verecek geçebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Çalıştırma hakkında daha fazla bilgi için [AccountManagement örnek uygulama][acct_mgmt_sample], bkz: [yönetmek Batch hesaplarını ve kotalarını .NET için Batch Yönetimi istemci kitaplığı ile](batch-management-dotnet.md).

Azure AD hakkında daha fazla bilgi için bkz: [Azure Active Directory belgeleri](https://docs.microsoft.com/azure/active-directory/). ADAL'nin kullanımı gösteren ayrıntılı örnekler kullanılabilir [Azure Kod örnekleri](https://azure.microsoft.com/resources/samples/?service=active-directory) kitaplığı.

Toplu hizmet uygulamaları Azure AD kullanarak kimlik doğrulaması yapmak için bkz: [Active Directory ile kimlik doğrulaması toplu hizmet çözümlerine](batch-aad-auth.md). 


[aad_about]: ../active-directory/active-directory-whatis.md "Azure Active Directory nedir?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Azure AD için kimlik doğrulama senaryoları"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Uygulamaları Azure Active Directory ile tümleştirme"
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[azure_portal]: http://portal.azure.com
[resman_overview]: ../azure-resource-manager/resource-group-overview.md
