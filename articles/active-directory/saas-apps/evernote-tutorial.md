---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Evernote | Microsoft Docs'
description: Azure Active Directory ve Evernote arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/10/2019
ms.author: jeedes
ms.openlocfilehash: 2ce05e904484a6d773a0132734208b87e161f960
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59499929"
---
# <a name="tutorial-azure-active-directory-integration-with-evernote"></a>Öğretici: Evernote ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Evernote tümleştirme konusunda bilgi edinin.
Azure AD ile Evernote tümleştirme ile aşağıdaki avantajları sağlar:

* Evernote erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak Evernote'a (çoklu oturum açma) ile Azure AD hesaplarına oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Evernote yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik Evernote çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Evernote destekler **SP** ve **IDP** tarafından başlatılan

## <a name="adding-evernote-from-the-gallery"></a>Galeriden Evernote ekleme

Azure AD'de Evernote tümleştirmesini yapılandırmak için Evernote Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Evernote eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için tıklatın **yeni uygulama** iletişim kutusunun üstündeki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Evernote**seçin **Evernote** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Evernote](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Evernote adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının Evernote ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Evernote ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Evernote çoklu oturum açmayı yapılandırma](#configure-evernote-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Evernote test kullanıcısı oluşturma](#create-evernote-test-user)**  - kullanıcı Azure AD gösterimini bağlı Evernote Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile Evernote yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Evernote** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** modunda başlatılan aşağıdaki adımı uygulayın:

    ![Evernote etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-identifier.png)

    İçinde **tanımlayıcı** metin kutusuna bir URL yazın:  `https://www.evernote.com/saml2`

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![image](common/both-preintegrated-signon.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL yazın:  `https://www.evernote.com/Login.action`

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. Değiştirilecek **imzalama** Seçenekleri'ni **Düzenle** açmak için düğmeyi **SAML imzalama sertifikası** iletişim.

    ![image](common/edit-certificate.png) 

    ![image](./media/evernote-tutorial/samlassertion.png)

    a. Seçin **oturum SAML yanıtını ve onayını** seçeneğini **imzalama seçeneği**.

    b. **Kaydet**’e tıklayın

8. Üzerinde **Evernote kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-evernote-single-sign-on"></a>Evernote tek oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde Evernote şirket sitenize yönetici olarak oturum.

2. Git **'Yönetici Konsolu'**

    ![Yönetici Konsolu](./media/evernote-tutorial/tutorial_evernote_adminconsole.png)

3. Gelen **'Yönetici Konsolu'** Git **'Güvenlik'** seçip **' çoklu oturum açma '**

    ![SSO ayarı](./media/evernote-tutorial/tutorial_evernote_sso.png)

4. Aşağıdaki değerleri yapılandırın:

    ![Sertifika ayarları](./media/evernote-tutorial/tutorial_evernote_certx.png)
    
    a.  **SSO'yu etkinleştirin:** SSO, varsayılan olarak etkindir (tıklayın **devre dışı çoklu oturum açma** SSO gereksinimini kaldırmak için)

    b. Yapıştırma **oturum açma URL'si** Azure portaldan kopyaladığınız değeri **SAML HTTP isteği URL'si** metin.

    c. İndirilen sertifika Azure AD'den bir Not Defteri'nde açın ve "Sertifika başlayın" ve "END CERTIFICATE" dahil olmak üzere içeriği kopyalayın ve yapıştırın **X.509 sertifikası** metin. 

    d.Click **Değişiklikleri Kaydet**

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

Bu bölümde, Azure çoklu oturum açma Evernote'a erişim vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Evernote**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Evernote**.

    ![Uygulamalar listesinde Evernote bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-evernote-test-user"></a>Evernote test kullanıcısı oluşturma

Evernote açarken Azure AD kullanıcılarının etkinleştirmek için bunların Evernote sağlanması gerekir.  
Evernote söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesapları sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Evernote şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Tıklayın **'Yönetici Konsolu'**.

    ![Yönetici Konsolu](./media/evernote-tutorial/tutorial_evernote_adminconsole.png)

3. Gelen **'Yönetici Konsolu'** Git **'kullanıcı ekleme'**.

    ![TestUser Ekle](./media/evernote-tutorial/create_aaduser_0001.png)

4. **Takım üyeleri ekleme** içinde **e-posta** metin kullanıcı hesabının e-posta adresini yazın ve tıklayın **davet edin.**

    ![TestUser Ekle](./media/evernote-tutorial/create_aaduser_0002.png)
    
5. Davet gönderildikten sonra Azure Active Directory hesap sahibinin davetiyeyi kabul etmek için e-posta alırsınız.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Evernote kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Evernote için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

