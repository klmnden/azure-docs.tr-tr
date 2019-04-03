---
title: 'Son kullanıcı kimlik doğrulaması: Azure Active Directory ile Azure Data Lake depolama Gen1 | Microsoft Docs'
description: Azure Active Directory kullanarak son kullanıcı kimlik doğrulaması ile Azure Data Lake depolama Gen1 elde öğrenin
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: c0fe63e395ee08cb65e9bbbadc4ce1f03032ce95
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58880092"
---
# <a name="end-user-authentication-with-azure-data-lake-storage-gen1-using-azure-active-directory"></a>Azure Active Directory kullanarak son kullanıcı kimlik doğrulaması ile Azure Data Lake depolama Gen1
> [!div class="op_single_selector"]
> * [Son kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-using-active-directory.md)
> * [Hizmetten hizmete kimlik doğrulaması](data-lake-store-service-to-service-authenticate-using-active-directory.md)
> 
> 

Azure Data Lake depolama Gen1 Azure Active Directory kimlik doğrulaması için kullanır. Data Lake depolama Gen1 veya Azure Data Lake Analytics ile çalışan bir uygulama geliştirme önce uygulamanızın Azure Active Directory (Azure AD) ile kimlik doğrulaması nasıl karar vermeniz gerekir. Kullanılabilir iki ana Seçenekler şunlardır:

* Son kullanıcı kimlik doğrulaması (Bu makale)
* Hizmetten hizmete kimlik doğrulaması (Bu seçenek açılır listeden yukarıdaki Seç)

Data Lake depolama Gen1 ya da Azure Data Lake Analytics yapılan her bir isteğe bağlı bir OAuth 2.0 belirteç ile sağlanan uygulamanız bu seçeneklerin sonuçlanır.

Bu makalede nasıl oluşturulacağı hakkında konuşuyor bir **son kullanıcı kimlik doğrulaması için Azure AD yerel uygulaması**. Hizmetten hizmete kimlik doğrulaması için Azure AD uygulama yapılandırması hakkındaki yönergeler için bkz: [hizmetten hizmete kimlik doğrulaması Azure Active Directory kullanarak Data Lake depolama Gen1 ile](data-lake-store-authenticate-using-active-directory.md).

## <a name="prerequisites"></a>Önkoşullar
* Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* Abonelik kimliğinizi Azure portalından alabilirsiniz. Örneğin, Data Lake depolama Gen1 hesabı dikey penceresinden kullanılabilir.
  
    ![Abonelik Kimliğini alın](./media/data-lake-store-end-user-authenticate-using-active-directory/get-subscription-id.png)

* Azure AD etki alanı adı. Azure portalının sağ üst köşedeki fare getirerek geri alabilirsiniz. Aşağıdaki ekran görüntüsünde etki alanı adıdır **contoso.onmicrosoft.com**, ve parantez içinde GUID değerinin Kiracı kimliği 
  
    ![AAD etki alanı Al](./media/data-lake-store-end-user-authenticate-using-active-directory/get-aad-domain.png)

* Azure Kiracı kimliğinizi Kiracı Kimliğini almak yönergeler için bkz: [Kiracı Kimliğinizi alma](../active-directory/develop/howto-create-service-principal-portal.md#get-tenant-id).

## <a name="end-user-authentication"></a>Son kullanıcı kimlik doğrulaması
Uygulamanızı Azure AD ile oturum açmak için bir son kullanıcı istiyorsanız, bu kimlik doğrulama mekanizması önerilen yaklaşımdır. Uygulamanızı daha sonra oturum açmış aynı düzeyde erişim son kullanıcı ile Azure kaynaklarına erişemez. Son kullanıcınızın uygulamanızın erişimi sürdürmek sırada düzenli aralıklarla kimlik bilgilerini sağlamaları gerekir.

Son kullanıcı oturum açma olması sonucunu, uygulamanızın bir erişim belirteci ve yenileme belirteci verilmesidir. Data Lake depolama Gen1 veya Data Lake Analytics yapılan her bir isteğe bağlı erişim belirteci ve varsayılan olarak bir saat için geçerli olduğundan. Varsayılan olarak iki haftaya kadar geçerli olduğundan ve yenileme belirtecinin yeni bir erişim belirteci almak için kullanılır. Son kullanıcı oturum açma için iki farklı yaklaşım kullanabilirsiniz.

### <a name="using-the-oauth-20-pop-up"></a>OAuth 2.0 açılır kullanma
Uygulamanızın son kullanıcı kimlik bilgilerini girebilirsiniz bir OAuth 2.0 yetkilendirme açılır, tetikleyebilirsiniz. Bu açılır pencere gerekiyorsa, Azure AD iki öğeli kimlik doğrulamayı (2FA) işlemi ile de çalışır. 

> [!NOTE]
> Bu yöntem henüz Azure AD Authentication Library (ADAL), Python veya Java için desteklenmiyor.
> 
> 

### <a name="directly-passing-in-user-credentials"></a>Kullanıcı kimlik bilgilerini doğrudan geçirme
Uygulamanız, doğrudan Azure AD'ye kullanıcı kimlik bilgilerini sağlayabilir. Bu yöntem yalnızca Kuruluş Kimliği kullanıcı hesaplarıyla çalışır; uyumlu değil kişisel ile / "live ID" sonu hesapları kullanıcı hesaplarını @outlook.com veya @live.com. Ayrıca, bu yöntem, Azure AD iki öğeli kimlik doğrulamayı (2FA) gerektiren kullanıcı hesapları ile uyumlu değil.

### <a name="what-do-i-need-for-this-approach"></a>Bu yaklaşım için ne gerekir?
* Azure AD etki alanı adı. Bu gereksinim, zaten bu makalenin önkoşul olarak listelenir.
* Azure AD Kiracı kimliği Bu gereksinim, zaten bu makalenin önkoşul olarak listelenir.
* Azure AD **yerel uygulama**
* Azure AD yerel uygulaması için uygulama kimliği
* Azure AD yerel uygulaması için yeniden yönlendirme URI'si
* Yetki verilmiş izinleri ayarlayın


## <a name="step-1-create-an-active-directory-native-application"></a>1. Adım: Yerel bir Active Directory uygulaması oluşturma

Oluşturun ve bir Azure AD yerel uygulaması Azure Active Directory kullanarak son kullanıcı kimlik doğrulama ile Data Lake depolama Gen1 yapılandırın. Yönergeler için [bir Azure AD uygulaması oluştur](../active-directory/develop/howto-create-service-principal-portal.md).

Bağlantıda bulunan yönergeleri takip ederken seçtiğinizden emin olun **yerel** aşağıdaki ekran görüntüsünde gösterildiği gibi uygulama türü için:

![Web uygulaması oluşturma](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-create-native-app.png "yerel uygulama oluştur")

## <a name="step-2-get-application-id-and-redirect-uri"></a>2. Adım: Uygulama Kimliği alma ve yeniden yönlendirme URI'si

Bkz: [uygulama kimliği alma](../active-directory/develop/howto-create-service-principal-portal.md#get-application-id-and-authentication-key) uygulama kimliğini almak için

Yeniden yönlendirme URI'si almak için aşağıdaki adımları uygulayın.

1. Azure portalından seçin **Azure Active Directory**, tıklayın **uygulama kayıtları**ve ardından bulmak ve oluşturduğunuz Azure AD yerel uygulaması'nı tıklatın.

2. Gelen **ayarları** uygulama dikey **yeniden yönlendirme URI'leri**.

    ![Get Redirect URI](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-redirect-uri.png)

3. Görüntülenen değeri kopyalayın.


## <a name="step-3-set-permissions"></a>3. Adım: İzin ayarla

1. Azure portalından seçin **Azure Active Directory**, tıklayın **uygulama kayıtları**ve ardından bulmak ve oluşturduğunuz Azure AD yerel uygulaması'nı tıklatın.

2. Gelen **ayarları** uygulama dikey **gerekli izinler**ve ardından **Ekle**.

    ![istemci kimliği](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-1.png)

3. İçinde **API erişimi Ekle** dikey penceresinde tıklayın **bir API seçin**, tıklayın **Azure Data Lake**ve ardından **seçin**.

    ![istemci kimliği](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-2.png)
 
4.  İçinde **API erişimi Ekle** dikey penceresinde tıklayın **izinleri seçin**, vermek için onay kutusunu işaretleyin **tam Data Lake Store erişim**ve ardından **seçin**.

    ![istemci kimliği](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-3.png)

    **Bitti**’ye tıklayın.

5. İzinleri vermek için son iki adımı yineleyin **Windows Azure Hizmet Yönetimi API'si** de.
   
## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure AD yerel uygulaması oluşturulur ve .NET SDK'sı, Java SDK, REST API, vb. kullanarak Yazar istemci uygulamalarınızda ihtiyacınız olan bilgileri toplanan. İlk Data Lake depolama Gen1 ile kimlik doğrulaması ve ardından depolama üzerinde başka işlemler gerçekleştirmek için Azure AD web uygulaması kullanma hakkında konuşmak aşağıdaki makaleleri şimdi geçebilirsiniz.

* [Son Kullanıcı Java SDK'sını kullanarak kimlik ile Data Lake depolama Gen1](data-lake-store-end-user-authenticate-java-sdk.md)
* [Son kullanıcı kimlik doğrulaması .NET SDK kullanarak Data Lake depolama Gen1 ile](data-lake-store-end-user-authenticate-net-sdk.md)
* [Python kullanarak son kullanıcı kimlik doğrulaması ile Data Lake depolama Gen1](data-lake-store-end-user-authenticate-python.md)
* [Data Lake depolama Gen1 ile son kullanıcı kimlik doğrulaması REST API'sini kullanma](data-lake-store-end-user-authenticate-rest-api.md)

