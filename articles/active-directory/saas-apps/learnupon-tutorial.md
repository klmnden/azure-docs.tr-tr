---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile LearnUpon | Microsoft Docs'
description: Azure Active Directory ve LearnUpon arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b11c6315-c79d-4f34-9610-bd17070ab7c7
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 574c21dc2713f10513ac296e7db538e20a94c9d6
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65406524"
---
# <a name="tutorial-azure-active-directory-integration-with-learnupon"></a>Öğretici: LearnUpon ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile LearnUpon tümleştirme konusunda bilgi edinin.
Azure AD ile LearnUpon tümleştirme ile aşağıdaki avantajları sağlar:

* LearnUpon erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) LearnUpon için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile LearnUpon yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Abonelik LearnUpon çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.


* LearnUpon destekler **IDP** tarafından başlatılan

* LearnUpon destekler **zamanında** kullanıcı sağlama


## <a name="adding-learnupon-from-the-gallery"></a>Galeriden LearnUpon ekleme

Azure AD'de LearnUpon tümleştirmesini yapılandırmak için LearnUpon Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden LearnUpon eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **LearnUpon**seçin **LearnUpon** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde LearnUpon](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma LearnUpon adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının LearnUpon ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma LearnUpon ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[LearnUpon çoklu oturum açmayı yapılandırma](#configure-learnupon-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[LearnUpon test kullanıcısı oluşturma](#create-learnupon-test-user)**  - kullanıcı Azure AD gösterimini bağlı LearnUpon Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile LearnUpon yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **LearnUpon** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![LearnUpon etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-reply.png)

    İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<companyname>.learnupon.com/saml/consumer`

    > [!NOTE]
    > Değer, gerçek değil. Değerini gerçek yanıt URL'si ile güncelleştirin. İlgili kişi [LearnUpon istemci Destek ekibine](https://www.learnupon.com/features/support/) değeri alınamıyor. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında, bulmak **parmak İZİ** -LearnUpon SAML ayarlarınızı eklenir.

    ![Sertifika indirme bağlantısı](common/certificateraw.png)

6. Üzerinde **LearnUpon kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-learnupon-single-sign-on"></a>LearnUpon tek oturum açmayı yapılandırın

1. Başka bir tarayıcı örneğinde açın ve LearnUpon bir yönetici hesabıyla oturum açın.

1. Tıklayın **ayarları** sekmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/learnupon-tutorial/tutorial_learnupon_06.png)

1. Tıklayın **çoklu oturum açma - SAML**ve ardından **genel ayarlar** SAML ayarlarını yapılandırmak için.
   
    ![Çoklu oturum açmayı yapılandırın](./media/learnupon-tutorial/tutorial_learnupon_07.png) 

1. İçinde **genel ayarlar** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı yapılandırın](./media/learnupon-tutorial/tutorial_learnupon_08.png)  
  
    a. **Etkin**’i seçin.

    b. Seçin **sürüm** olarak **2.0**.

    c. Seçin **Atla koşullar** olarak **Hayır**.

    d. İçinde **SAML belirteci sonrası parametre adı** metin doğrulandı ve kimlik doğrulaması - örneğin için SAML onayı içeriyor SAML tüketici URL'sine istek post parametresinin adı belirtilen yukarıda kutusuna **SAMLResponse** .

    e. İçinde **ad tanımlayıcı biçimi** metin kutusuna, SAML onayı kullanıcılar tanımlayıcı (e-posta adresi) nerede belirten değeri bulunduğu - örneğin `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`.
  
    f. İçinde **sağlayıcı konumu belirlemek** metin, Azure portalı oturum açma ekranından, karşıya yüklenen simgesine tıklarsanız burada kullanıcıların yönlendirildiği belirten bir değer yazın.
  
    g. İçinde **oturum kapatma URL'si** metin kutusu, yapıştırma **oturum kapatma URL'si** Azure portaldan kopyaladığınız değeri.

    h. Tıklayın **parmak yazdırır yönetme**ve ardından indirilen sertifikanızın parmak izi karşıya yükleyin.

1. Tıklayın **kullanıcı ayarları**ve ardından aşağıdaki adımları gerçekleştirin:

     ![Çoklu oturum açmayı yapılandırın](./media/learnupon-tutorial/tutorial_learnupon_11.png)  

    a. İçinde **ad tanımlayıcı biçimi** metin kutusuna, SAML onaylama işlemi burada kullanıcılar firstname de bize bildiren değerin bulunduğu - örneğin: `https://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.
  
    b. İçinde **son ad tanımlayıcı biçimi** metin kutusuna, SAML onaylama işlemi burada kullanıcılar lastname, bize bildiren değerin bulunduğu - örneğin: `https://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü `brittasimon@yourcompanydomain.extension`. Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için LearnUpon erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **LearnUpon**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **LearnUpon**.

    ![Uygulamalar listesinde LearnUpon bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-learnupon-test-user"></a>LearnUpon test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı LearnUpon oluşturulur. LearnUpon just-ın-time kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Bir kullanıcı LearnUpon içinde zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur. Bir kullanıcı el ile oluşturmanız gerekiyorsa, iletişime geçmeniz [LearnUpon Destek ekibine](https://www.learnupon.com/features/support/).

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli LearnUpon kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama LearnUpon için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)