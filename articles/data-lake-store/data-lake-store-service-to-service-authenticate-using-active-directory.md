---
title: 'Hizmetten hizmete kimlik doğrulaması: Azure Active Directory ile Azure Data Lake depolama Gen1 | Microsoft Docs'
description: Azure Data Lake depolama Gen1 ile hizmetten hizmete kimlik doğrulaması Azure Active Directory'yi kullanarak elde öğrenin
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
ms.openlocfilehash: a7fdcf396f586a65efa17e489d002f1c8847a193
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58885001"
---
# <a name="service-to-service-authentication-with-azure-data-lake-storage-gen1-using-azure-active-directory"></a>Azure Data Lake depolama Gen1 ile hizmetten hizmete kimlik doğrulaması Azure Active Directory'yi kullanarak
> [!div class="op_single_selector"]
> * [Son kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-using-active-directory.md)
> * [Hizmetten hizmete kimlik doğrulaması](data-lake-store-service-to-service-authenticate-using-active-directory.md)
> 
>  

Azure Data Lake depolama Gen1 Azure Active Directory kimlik doğrulaması için kullanır. Data Lake depolama Gen1 ile çalışan bir uygulama geliştirme önce uygulamanızın Azure Active Directory (Azure AD) ile kimlik doğrulaması nasıl karar vermeniz gerekir. Kullanılabilir iki ana Seçenekler şunlardır:

* Son kullanıcı kimlik doğrulaması 
* Hizmetten hizmete kimlik doğrulaması (Bu makale) 

Bu seçeneklerin, uygulamanız için Data Lake depolama Gen1 yapılan her isteğin iliştirilmiş bir OAuth 2.0 belirteç ile sağlanan sonuçlanır.

Bu makalede nasıl oluşturulacağı hakkında konuşuyor bir **hizmetten hizmete kimlik doğrulaması için Azure AD web uygulaması**. Son kullanıcı kimlik doğrulaması için Azure AD uygulama yapılandırması hakkındaki yönergeler için bkz: [Azure Active Directory kullanarak son kullanıcı kimlik doğrulaması ile Data Lake depolama Gen1](data-lake-store-end-user-authenticate-using-active-directory.md).

## <a name="prerequisites"></a>Önkoşullar
* Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

## <a name="step-1-create-an-active-directory-web-application"></a>1. Adım: Bir Active Directory web uygulaması oluşturma

Oluşturun ve Azure AD web uygulaması için Azure Data Lake depolama Gen1 ile hizmetten hizmete kimlik doğrulaması Azure Active Directory'yi kullanarak yapılandırın. Yönergeler için [bir Azure AD uygulaması oluştur](../active-directory/develop/howto-create-service-principal-portal.md).

Önceki bağlantıda belirtilen yönergeleri takip ederken seçtiğinizden emin olun **Web uygulaması / API** aşağıdaki ekran görüntüsünde gösterildiği gibi uygulama türü için:

![Web uygulaması oluşturma](./media/data-lake-store-authenticate-using-active-directory/azure-active-directory-create-web-app.png "web uygulaması oluşturma")

## <a name="step-2-get-application-id-authentication-key-and-tenant-id"></a>2. Adım: Uygulama kimliği, kimlik doğrulama anahtarı ve Kiracı Kimliğini alma
Programlamayla oturum açılırken, uygulamanızın kimliği gerekir. Uygulama kendi kimlik bilgileriniz altında çalışıyorsa, ayrıca bir kimlik doğrulama anahtarı gerekir.

* Uygulamanız için (istemci gizli anahtarı olarak da bilinir) uygulama kimliği ve kimlik doğrulama anahtarını almak yönergeler için bkz: [uygulama kimliği ve kimlik doğrulama anahtarını Al](../active-directory/develop/howto-create-service-principal-portal.md#get-application-id-and-authentication-key).

* Kiracı Kimliğini almak yönergeler için bkz: [Kiracı kimliği alma](../active-directory/develop/howto-create-service-principal-portal.md#get-tenant-id).

## <a name="step-3-assign-the-azure-ad-application-to-the-azure-data-lake-storage-gen1-account-file-or-folder"></a>3. Adım: Azure AD uygulaması Azure Data Lake depolama Gen1 hesabı dosya veya klasörü atayın


1. [Azure portalı](https://portal.azure.com) üzerinde oturum açın. Daha önce oluşturduğunuz Azure Active Directory uygulamayla ilişkilendirmek istediğiniz Data Lake depolama Gen1 hesabınızı açın.
2. Data Lake depolama Gen1 hesabı dikey penceresinde **Veri Gezgini**.
   
    ![Data Lake depolama Gen1 hesabında dizinleri oluşturma](./media/data-lake-store-authenticate-using-active-directory/adl.start.data.explorer.png "dizinleri Data Lake hesabı oluşturma")
3. İçinde **Veri Gezgini** dikey penceresinde, dosya veya Azure AD uygulamasına erişim sağlamak ve ardından istediğiniz klasörü tıklatın **erişim**. Bir dosyaya erişimi yapılandırmak için tıklatmalısınız **erişim** gelen **dosya önizlemesi** dikey penceresi.
   
    ![Data Lake dosya sistemine ACL ayarlayın](./media/data-lake-store-authenticate-using-active-directory/adl.acl.1.png "Data Lake dosya sistemi ACL'ler ayarlayın")
4. **Erişim** dikey penceresinde, standart erişim ile köküne atanmış özel erişim listeler. Tıklayın **Ekle** simgesini Özel düzey ACL'leri ekleyebilir.
   
    ![Standart ve özel erişim listesinde](./media/data-lake-store-authenticate-using-active-directory/adl.acl.2.png "listesinde standart ve özel erişim")
5. Tıklayın **Ekle** açmak için simgeyi **Ekle özel erişim** dikey penceresi. Bu dikey pencerede tıklayın **kullanıcı veya Grup Seç**ve ardından **kullanıcı veya Grup Seç** dikey penceresinde, daha önce oluşturduğunuz Azure Active Directory Uygulama arayın. Metin kutusunun üstünde aramak için birçok gruplarınız varsa, grup adıyla filtrelemek için kullanın. Ekleyin ve ardından istediğiniz grubu seçin **seçin**.
   
    ![Grup ekleme](./media/data-lake-store-authenticate-using-active-directory/adl.acl.3.png "grup ekleme")
6. Tıklayın **Select izinleri**izinleri ACL varsayılan olarak atamak isteyip istemediğinizi ACL veya her ikisini de erişim ve izinleri seçin. **Tamam** düğmesine tıklayın.
   
    ![Grup için izinleri atayın](./media/data-lake-store-authenticate-using-active-directory/adl.acl.4.png "gruplandırmak için izin atama")
   
    Data Lake depolama Gen1 ve varsayılan/erişim ACL'leri izinler hakkında daha fazla bilgi için bkz. [erişim denetimi Data Lake depolama Gen1](data-lake-store-access-control.md).
7. İçinde **Ekle özel erişim** dikey penceresinde tıklayın **Tamam**. Yeni eklenen grubuyla ilişkili izinler listelenir **erişim** dikey penceresi.
   
    ![Grup için izinleri atayın](./media/data-lake-store-authenticate-using-active-directory/adl.acl.5.png "gruplandırmak için izin atama")

> [!NOTE]
> Azure Active Directory uygulamanızı belirli bir klasöre kısıtlayarak şirket planlıyorsanız, ayrıca, aynı Azure Active directory uygulaması vermeniz gerekir **yürütme** .NET aracılığıyla dosya oluşturma erişimi etkinleştirme izni kök SDK.

> [!NOTE]
> Bir Data Lake depolama Gen1 hesabı oluşturmak için SDK'ları kullanmak istiyorsanız, Data Lake depolama Gen1 hesabı oluşturduğunuz kaynak grubunu Azure AD web uygulaması olarak bir rol atamanız gerekir.
> 
>

## <a name="step-4-get-the-oauth-20-token-endpoint-only-for-java-based-applications"></a>4. Adım: OAuth 2.0 belirteç uç noktası (yalnızca Java tabanlı uygulamalar için) alma

1. Oturum [Azure portalında](https://portal.azure.com) ve sol bölmeden Active Directory'ye tıklayın.

2. Sol bölmeden tıklayın **uygulama kayıtları**.

3. Uygulama kayıtları dikey penceresinin üst kısmından tıklayın **uç noktaları**.

    ![OAuth belirteç uç noktası](./media/data-lake-store-authenticate-using-active-directory/oauth-token-endpoint.png "OAuth belirteç uç noktası")

4. OAuth 2.0 belirteç uç noktası uç noktalar listesinden kopyalayın.

    ![OAuth belirteç uç noktası](./media/data-lake-store-authenticate-using-active-directory/oauth-token-endpoint-1.png "OAuth belirteç uç noktası")   

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, bir Azure AD web uygulaması oluşturdunuz ve .NET SDK'sı, Java, Python, REST API, vb. kullanarak Yazar istemci uygulamalarınızda ihtiyacınız olan bilgileri toplanan. İlk Data Lake depolama Gen1 ile kimlik doğrulaması ve ardından depolama üzerinde başka işlemler gerçekleştirmek için Azure AD yerel uygulaması kullanma hakkında konuşmak aşağıdaki makaleleri şimdi geçebilirsiniz.

* [Java ile hizmetten hizmete kimlik doğrulama ile Data Lake depolama Gen1](data-lake-store-service-to-service-authenticate-java.md)
* [Hizmetten hizmete kimlik doğrulaması .NET SDK kullanarak Data Lake depolama Gen1 ile](data-lake-store-service-to-service-authenticate-net-sdk.md)
* [Data Lake depolama Gen1 ile hizmetten hizmete kimlik doğrulaması Python kullanma](data-lake-store-service-to-service-authenticate-python.md)
* [Data Lake depolama Gen1 ile hizmetten hizmete kimlik doğrulaması REST API'sini kullanma](data-lake-store-service-to-service-authenticate-rest-api.md)


