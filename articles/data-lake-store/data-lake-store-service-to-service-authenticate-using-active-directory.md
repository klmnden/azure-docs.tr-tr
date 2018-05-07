---
title: 'Hizmetten hizmete kimlik doğrulaması: Data Lake Store Azure Active Directory ile | Microsoft Docs'
description: Hizmetten hizmete kimlik doğrulaması Azure Active Directory'yi kullanarak Data Lake Store ile elde öğrenin
services: data-lake-store
documentationcenter: ''
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
ms.openlocfilehash: 58f269fa9c153a37a792d9d4efdaf0bd74eb265a
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="service-to-service-authentication-with-data-lake-store-using-azure-active-directory"></a>Azure Active Directory kullanarak Data Lake Store ile hizmet kimlik doğrulaması
> [!div class="op_single_selector"]
> * [Son kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-using-active-directory.md)
> * [Hizmetten hizmete kimlik doğrulaması](data-lake-store-service-to-service-authenticate-using-active-directory.md)
> 
>  

Azure Data Lake Store, Azure Active Directory kimlik doğrulaması için kullanır. Azure Data Lake Store ile çalışan bir uygulama geliştirme önce uygulamanızı Azure Active Directory (Azure AD) ile kimlik doğrulaması yapmayı karar vermeniz gerekir. İki ana seçenekleri şunlardır:

* Son kullanıcı kimlik doğrulaması 
* Hizmetten hizmete kimlik doğrulaması (Bu makalede) 

Azure Data Lake Store için yapılan her isteği iliştirilmiş bir OAuth 2.0 belirteç ile sağlanan uygulamanızda hem bu seçenekleri sonuçlanır.

Bu makalede ettiği nasıl oluşturulacağı hakkında bir **hizmeti için kimlik doğrulaması için Azure AD web uygulaması**. Son kullanıcı kimlik doğrulaması için Azure AD uygulama yapılandırma yönergeleri için bkz: [son kullanıcı kimlik doğrulaması Azure Active Directory kullanarak Data Lake Store ile](data-lake-store-end-user-authenticate-using-active-directory.md).

## <a name="prerequisites"></a>Önkoşullar
* Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

## <a name="step-1-create-an-active-directory-web-application"></a>1. adım: bir Active Directory web uygulaması oluşturma

Oluşturun ve Azure AD web uygulaması hizmeti için kimlik doğrulaması için Azure Data Lake Azure Active Directory'yi kullanarak Store ile yapılandırın. Yönergeler için bkz: [Azure AD uygulaması oluşturmasına](../azure-resource-manager/resource-group-create-service-principal-portal.md).

Yukarıdaki bağlantıyı kısmındaki yönergeleri izleyerek sırasında seçtiğinizden emin olun **Web uygulaması / API** aşağıdaki ekran görüntüsünde gösterildiği gibi uygulama türü için:

![Web uygulaması oluşturma](./media/data-lake-store-authenticate-using-active-directory/azure-active-directory-create-web-app.png "web uygulaması oluştur")

## <a name="step-2-get-application-id-authentication-key-and-tenant-id"></a>2. adım: uygulama kimliği, kimlik doğrulama anahtarı ve Kiracı kimliği alma
Program aracılığıyla oturum açarken, uygulamanız için kimliği gerekir. Uygulama kendi kimlik bilgileri altında çalışıyorsa, ayrıca bir kimlik doğrulama anahtarı gerekir.

* Uygulamanız için (istemci gizli anahtarı olarak da bilinir) uygulama kimliği ile kimlik doğrulama anahtarı alma hakkında daha fazla yönerge için bkz: [Get uygulama kimliği ile kimlik doğrulama anahtarı](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key).

* Kiracı kimliği alma hakkında daha fazla yönerge için bkz: [alma Kiracı kimliği](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).

## <a name="step-3-assign-the-azure-ad-application-to-the-azure-data-lake-store-account-file-or-folder"></a>3. adım: Azure AD uygulaması Azure Data Lake Store hesabına dosya veya klasöre atama


1. [Azure portalı](https://portal.azure.com) üzerinde oturum açın. Daha önce oluşturduğunuz Azure Active Directory uygulama ile ilişkilendirmek istediğiniz Azure Data Lake Store hesabını açın.
2. Data Lake Store hesabı dikey pencerenizde, **Veri Gezgini**'ne tıklayın.
   
    ![Data Lake Store hesabında dizin oluşturmak](./media/data-lake-store-authenticate-using-active-directory/adl.start.data.explorer.png "Data Lake hesabında dizinler oluşturma")
3. İçinde **Veri Gezgini** dikey penceresinde, dosya veya Azure AD uygulaması erişim sağlamak ve ardından istediğiniz klasörü tıklatın **erişim**. Bir dosyaya erişimi yapılandırmak için tıklatmalısınız **erişim** gelen **Dosya Önizleme** dikey.
   
    ![Data Lake dosya sisteminde ACL'leri ayarlamak](./media/data-lake-store-authenticate-using-active-directory/adl.acl.1.png "Data Lake dosya sistemi üzerindeki ACL'lerin ayarlayın")
4. **Erişim** standart erişim ve kök atanmış özel erişim dikey penceresinde listelenir. Tıklatın **Ekle** Özel düzey ACL eklemek için simge.
   
    ![Liste standart ve özel erişim](./media/data-lake-store-authenticate-using-active-directory/adl.acl.2.png "listesinde standart ve özel erişim")
5. Tıklatın **Ekle** açmak için simgesini **eklemek özel erişim** dikey. Bu dikey pencerede tıklatın **kullanıcı veya Grup Seç**ve ardından **kullanıcı veya Grup Seç** dikey penceresinde, önceden oluşturduğunuz Azure Active Directory Uygulama arayın. Gelen aramak için birçok gruplarınız varsa, metin kutusunun üstündeki grup adına filtrelemek için kullanın. Ekleyin ve ardından istediğiniz Grup tıklatın **seçin**.
   
    ![Grup ekleme](./media/data-lake-store-authenticate-using-active-directory/adl.acl.3.png "grup ekleme")
6. Tıklatın **Select izinleri**, izinleri seçin ve varsayılan olarak ACL izinleri atamak istediğiniz olup olmadığını ACL ya da her ikisini de erişim. **Tamam**’a tıklayın.
   
    ![Grup için izinleri atayın](./media/data-lake-store-authenticate-using-active-directory/adl.acl.4.png "gruplandırmak için izinler atama")
   
    Data Lake Store ve varsayılan/erişim ACL izinleri hakkında daha fazla bilgi için bkz: [Data Lake Store'da erişim denetimi](data-lake-store-access-control.md).
7. İçinde **eklemek özel erişim** dikey penceresinde tıklatın **Tamam**. Yeni eklenen grupla ilişkili izinler listelenir **erişim** dikey.
   
    ![Grup için izinleri atayın](./media/data-lake-store-authenticate-using-active-directory/adl.acl.5.png "gruplandırmak için izinler atama")

> [!NOTE]
> Azure Active Directory uygulamanızı belirli bir klasöre kısıtlama planlıyorsanız, de çok aynı Azure Active directory uygulamayı vermeniz gerekir **yürütme** permisison aracılığıyla dosya oluşturma erişimi etkinleştirmek için kök dizini. NET SDK.

> [!NOTE]
> Bir Data Lake Store hesabı oluşturmak için SDK'ları kullanmak istiyorsanız, bir rol olarak Azure AD web uygulamasının Data Lake Store hesabı oluşturmak kaynak grubuna atamanız gerekir.
> 
>

## <a name="step-4-get-the-oauth-20-token-endpoint-only-for-java-based-applications"></a>4. adım: OAuth 2.0 belirteç uç noktası (yalnızca Java tabanlı uygulamalar için) alın.

1. Oturum [Azure portal](https://portal.azure.com) ve Active Directory sol bölmeden'ı tıklatın.

2. Sol bölmeden tıklatın **uygulama kayıtlar**.

3. Uygulama kayıtlar dikey üstten tıklatın **uç noktaları**.

    ![OAuth belirteç uç noktası](./media/data-lake-store-authenticate-using-active-directory/oauth-token-endpoint.png "OAuth belirteç uç noktası")

4. Uç noktalar listesinden OAuth 2.0 belirteç uç noktası kopyalayın.

    ![OAuth belirteç uç noktası](./media/data-lake-store-authenticate-using-active-directory/oauth-token-endpoint-1.png "OAuth belirteç uç noktası")   

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure AD web uygulaması oluşturulur ve .NET SDK'sı, Java, Python, REST API vb. kullanarak Yazar istemci uygulamalarınızda gereksinim duyduğunuz bilgileri toplanmaz. İlk Data Lake Store ile kimlik doğrulaması ve depolama üzerinde başka işlemler gerçekleştirmek için Azure AD yerel uygulaması kullanma hakkında konuşun aşağıdaki makalelere şimdi devam edebilirsiniz.

* [Java kullanarak Data Lake Store ile hizmet kimlik doğrulaması](data-lake-store-service-to-service-authenticate-java.md)
* [.NET SDK kullanarak Data Lake Store ile hizmet kimlik doğrulaması](data-lake-store-service-to-service-authenticate-net-sdk.md)
* [Python kullanarak Data Lake Store ile hizmet kimlik doğrulaması](data-lake-store-service-to-service-authenticate-python.md)
* [REST API kullanarak Data Lake Store ile hizmet kimlik doğrulaması](data-lake-store-service-to-service-authenticate-rest-api.md)


