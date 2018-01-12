---
title: "Son kullanıcı kimlik doğrulaması: Data Lake Store Azure Active Directory ile | Microsoft Docs"
description: "Son kullanıcı kimlik doğrulaması Azure Active Directory'yi kullanarak Data Lake Store ile elde öğrenin"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/09/2018
ms.author: nitinme
ms.openlocfilehash: dca040fba78d6501bc835fdac402e69149d493b5
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2018
---
# <a name="end-user-authentication-with-data-lake-store-using-azure-active-directory"></a>Azure Active Directory kullanarak Data Lake Store ile son kullanıcı kimlik doğrulaması
> [!div class="op_single_selector"]
> * [Son kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-using-active-directory.md)
> * [Hizmetten hizmete kimlik doğrulaması](data-lake-store-service-to-service-authenticate-using-active-directory.md)
> 
> 

Azure Data Lake Store, Azure Active Directory kimlik doğrulaması için kullanır. Azure Data Lake Store veya Azure Data Lake Analytics ile çalışan bir uygulama geliştirme önce uygulamanızı Azure Active Directory (Azure AD) ile kimlik doğrulaması yapmayı karar vermeniz gerekir. İki ana seçenekleri şunlardır:

* Son kullanıcı kimlik doğrulaması (Bu makalede)
* Hizmetten hizmete kimlik doğrulaması (açılan yukarıdaki bu seçeneği seçin)

Azure Data Lake Store veya Azure Data Lake Analytics yapılan her isteği iliştirilmiş bir OAuth 2.0 belirteç ile sağlanan uygulamanızda hem bu seçenekleri sonuçlanır.

Bu makalede ettiği nasıl oluşturulacağı hakkında bir **son kullanıcı kimlik doğrulaması için Azure AD yerel uygulaması**. Hizmetten hizmete kimlik doğrulaması için Azure AD uygulama yapılandırma hakkında yönergeler için bkz: [hizmeti için kimlik doğrulaması Azure Active Directory kullanarak Data Lake Store ile](data-lake-store-authenticate-using-active-directory.md).

## <a name="prerequisites"></a>Önkoşullar
* Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* Abonelik kimliği Azure portalından alabilir. Örneğin, Data Lake Store hesabı dikey penceresinden kullanılabilir.
  
    ![Abonelik kimliği alma](./media/data-lake-store-end-user-authenticate-using-active-directory/get-subscription-id.png)

* Azure AD etki alanı adı. Azure portalının sağ üst köşedeki fare gelerek alabilir. Aşağıdaki ekran görüntüsünde etki alanı adıdır **contoso.onmicrosoft.com**, ve köşeli ayraçlar içinde GUID Kiracı kimliğidir. 
  
    ![AAD etki alanı alma](./media/data-lake-store-end-user-authenticate-using-active-directory/get-aad-domain.png)

* Azure Kiracı kimliğinizi Kiracı kimliği alma hakkında daha fazla yönerge için bkz: [Kiracı Kimliğinizi alma](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).

## <a name="end-user-authentication"></a>Son kullanıcı kimlik doğrulaması
Uygulamanızı Azure AD ile oturum açmak için bir son kullanıcı istiyorsanız, bu kimlik doğrulama mekanizması için önerilen yaklaşımdır. Uygulamanızı daha sonra oturum açmış aynı düzeyde erişim son kullanıcı ile Azure kaynaklarına erişebilir. Son kullanıcıların kimlik bilgilerini düzenli olarak erişim, uygulamanız için sırayla sağlaması gerekir.

Son kullanıcı oturum açma sahip sonucunu uygulamanız bir erişim belirteci ve bir yenileme belirteci verilmesidir. Erişim belirteci Data Lake Store veya Data Lake Analytics yapılan her isteği bağlı ve varsayılan olarak bir saat için geçerli olduğundan. İki haftaya varsayılan olarak geçerli olduğundan ve yenileme belirtecini yeni bir erişim belirteci almak için kullanılabilir. Son kullanıcı oturum açma için iki farklı yaklaşım kullanabilirsiniz.

### <a name="using-the-oauth-20-pop-up"></a>OAuth 2.0 açılır kullanma
Uygulamanız son kullanıcı kimlik bilgilerini girebilirsiniz bir OAuth 2.0 yetkilendirme açılır, tetikleyebilir. Bu açılır pencere gerekiyorsa, Azure AD iki öğeli kimlik doğrulamasını (2FA) işlemi ile de çalışır. 

> [!NOTE]
> Bu yöntem henüz Azure AD Authentication Library (ADAL), Python veya Java için desteklenmiyor.
> 
> 

### <a name="directly-passing-in-user-credentials"></a>Kullanıcı kimlik bilgilerini doğrudan geçirme
Uygulamanızı Azure AD'ye doğrudan kullanıcı kimlik bilgilerini sağlayabilir. Bu yöntem yalnızca Kuruluş Kimliği kullanıcı hesaplarıyla çalışır; uyumlu değil kişisel ile / "live ID" biten hesaplar kullanıcı hesaplarını @outlook.com veya @live.com. Ayrıca, bu yöntem, Azure AD iki öğeli kimlik doğrulamasını (2FA) gerektiren kullanıcı hesaplarını ile uyumlu değil.

### <a name="what-do-i-need-for-this-approach"></a>Bu yaklaşım için ne yapmalıyım?
* Azure AD etki alanı adı. Bu gereksinim, zaten bu makalede önkoşul olarak listelenir.
* Azure AD Kiracı kimliği. Bu gereksinim, zaten bu makalede önkoşul olarak listelenir.
* Azure AD **yerel uygulama**
* Azure AD yerel uygulama için uygulama kimliği
* Azure AD yerel uygulama için yeniden yönlendirme URI'si
* Yetki verilmiş izinleri ayarlayın


## <a name="step-1-create-an-active-directory-native-application"></a>1. adım: Active Directory yerel uygulama oluşturma

Oluşturun ve Azure AD yerel uygulaması son kullanıcı kimlik doğrulaması için Azure Data Lake Azure Active Directory'yi kullanarak Store ile yapılandırın. Yönergeler için bkz: [Azure AD uygulaması oluşturmasına](../azure-resource-manager/resource-group-create-service-principal-portal.md).

Bağlantıyı bölümündeki yönergeleri izleyerek sırasında seçtiğinizden emin olun **yerel** aşağıdaki ekran görüntüsünde gösterildiği gibi uygulama türü için:

![Web uygulaması oluşturma](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-create-native-app.png "yerel uygulama oluştur")

## <a name="step-2-get-application-id-and-redirect-uri"></a>2. adım: uygulama kimliği alma ve yeniden yönlendirme URI'si

Bkz: [uygulama kimliği alma](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key) (Azure Klasik Portalı'nda istemci kimliği olarak da bilinir) Azure AD yerel uygulamanın uygulama Kimliğini almak için.

Yeniden yönlendirme URI'si almak için aşağıdaki adımları uygulayın.

1. Azure portalından seçin **Azure Active Directory**, tıklatın **uygulama kayıtlar**ve ardından bulmak ve oluşturduğunuz Azure AD yerel uygulama'yı tıklatın.

2. Gelen **ayarları** uygulama için dikey tıklayın **yeniden yönlendirme URI'ler**.

    ![Get yeniden yönlendirme URI'si](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-redirect-uri.png)

3. Görüntülenen değer kopyalayın.


## <a name="step-3-set-permissions"></a>3. adım: İzinleri ayarlama

1. Azure portalından seçin **Azure Active Directory**, tıklatın **uygulama kayıtlar**ve ardından bulmak ve oluşturduğunuz Azure AD yerel uygulama'yı tıklatın.

2. Gelen **ayarları** uygulama için dikey tıklayın **gerekli izinleri**ve ardından **Ekle**.

    ![İstemci kimliği](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-1.png)

3. İçinde **API erişimini eklemek** dikey penceresinde tıklatın **bir API seçin**, tıklatın **Azure Data Lake**ve ardından **seçin**.

    ![İstemci kimliği](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-2.png)
 
4.  İçinde **eklemek API erişimini** dikey penceresinde tıklatın **izinler seçeneğini belirleyin**, vermek için bu onay kutusunu seçin **tam erişim Data Lake Store'a**ve ardından **seçin**.

    ![İstemci kimliği](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-3.png)

    **Bitti**’ye tıklayın.

5. İzinleri vermek için son iki adımı yineleyin **Windows Azure Hizmet Yönetimi API'si** de.
   
## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure AD yerel uygulaması oluşturulur ve .NET SDK'sı, Java SDK'sı, REST API vb. kullanarak Yazar istemci uygulamalarınızda gereksinim duyduğunuz bilgileri toplanmaz. İlk Data Lake Store ile kimlik doğrulaması ve depolama üzerinde başka işlemler gerçekleştirmek için Azure AD web uygulaması kullanma hakkında konuşun aşağıdaki makalelere şimdi devam edebilirsiniz.

* [Uç-kullanıcı kimlik doğrulama Java SDK'yı kullanarak Data Lake Store ile](data-lake-store-end-user-authenticate-java-sdk.md)
* [.NET SDK kullanarak Data Lake Store ile son kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-net-sdk.md)
* [Python kullanarak Data Lake Store ile son kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-python.md)
* [REST API kullanarak Data Lake Store ile son kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-rest-api.md)

