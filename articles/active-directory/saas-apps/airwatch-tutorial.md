---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile AirWatch | Microsoft Docs'
description: Azure Active Directory ve AirWatch arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 96a3bb1c-96c6-40dc-8ea0-060b0c2a62e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/07/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 301d008c8ebdb66a58674876937b13dcfa15c79d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67107198"
---
# <a name="tutorial-azure-active-directory-integration-with-airwatch"></a>Öğretici: AirWatch ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile AirWatch tümleştirme konusunda bilgi edinin.
Azure AD ile AirWatch tümleştirme ile aşağıdaki avantajları sağlar:

* AirWatch erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) AirWatch için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile AirWatch yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik AirWatch çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* AirWatch destekler **SP** tarafından başlatılan

## <a name="adding-airwatch-from-the-gallery"></a>Galeriden AirWatch ekleme

Azure AD'de AirWatch tümleştirmesini yapılandırmak için AirWatch Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden AirWatch eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **AirWatch**seçin **AirWatch** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde AirWatch](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma AirWatch adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının AirWatch ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma AirWatch ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[AirWatch çoklu oturum açmayı yapılandırma](#configure-airwatch-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[AirWatch test kullanıcısı oluşturma](#create-airwatch-test-user)**  - kullanıcı Azure AD gösterimini bağlı AirWatch Britta simon'un bir karşılığı vardır.
5. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile AirWatch yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **AirWatch** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![AirWatch etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<subdomain>.awmdm.com/AirWatch/Login?gid=companycode`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusunda, değer olarak girin: `AirWatch`

    > [!NOTE]
    > Bu değer, gerçek değil. Bu değer, gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [AirWatch istemci Destek ekibine](https://www.air-watch.com/company/contact-us/) bu değeri alınamıyor. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. AirWatch uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **kullanıcı öznitelikleri** iletişim.

    ![image](common/edit-attribute.png)

6. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda kullanarak talep Düzenle **düzenleme simgesi** veya talep kullanarak **Ekle yeni talep**SAML belirteci özniteliği yukarıdaki görüntüde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin:

    | Ad |  Kaynak özniteliği|
    |---------------|----------------|
    | KULLANICI KİMLİĞİ | User.userPrincipalName |
    | | |

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Bırakın **Namespace** boş.

    d. Kaynağı olarak **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

7. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

8. Üzerinde **AirWatch kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-airwatch-single-sign-on"></a>AirWatch tek oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde AirWatch şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Sol gezinti bölmesinden **hesapları**ve ardından **Yöneticiler**.

   ![Yöneticiler](./media/airwatch-tutorial/ic791920.png "yöneticileri")

3. Genişletin **ayarları** menüsüne ve ardından **Dizin Hizmetleri**.

   ![Ayarları](./media/airwatch-tutorial/ic791921.png "ayarları")

4. Tıklayın **kullanıcı** sekmesinde **temel DN** metin etki alanı adınızı yazın ve ardından **Kaydet**.

   ![Kullanıcı](./media/airwatch-tutorial/ic791922.png "kullanıcı")

5. Tıklayın **sunucu** sekmesi.

   ![Sunucu](./media/airwatch-tutorial/ic791923.png "sunucusu")

6. Aşağıdaki adımları gerçekleştirin:

    ![Karşıya yükleme](./media/airwatch-tutorial/ic791924.png "karşıya yükleme")   

    a. Olarak **dizin türü**seçin **hiçbiri**.

    b. Seçin **SAML kimlik doğrulaması için kullanmak**.

    c. İndirilen sertifika karşıya yüklemek için tıklayın **karşıya**.

7. İçinde **istek** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![İstek](./media/airwatch-tutorial/ic791925.png "iste")  

    a. Olarak **isteği bağlama türü**seçin **POST**.

    b. Azure portalında, üzerinde **çoklu oturum açma sırasında Airwatch yapılandırma** iletişim sayfası, kopyalama **oturum açma URL'si** değeri ve ardından yapıştırın **kimlik sağlayıcısı tek işareti bulunan URL'si** metin kutusu.

    c. Olarak **Nameıd biçimi**seçin **e-posta adresi**.

    d. **Kaydet**’e tıklayın.

8. Tıklayın **kullanıcı** yeniden sekmesi.

    ![Kullanıcı](./media/airwatch-tutorial/ic791926.png "kullanıcı")

9. İçinde **özniteliği** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Öznitelik](./media/airwatch-tutorial/ic791927.png "özniteliği")

    a. İçinde **nesne tanımlayıcısı** metin kutusuna `http://schemas.microsoft.com/identity/claims/objectidentifier`.

    b. İçinde **kullanıcıadı** metin kutusuna `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    c. İçinde **görünen ad** metin kutusuna `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.

    d. İçinde **ad** metin kutusuna `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.

    e. İçinde **Soyadı** metin kutusuna `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.

    f. İçinde **e-posta** metin kutusuna `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    g. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon\@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için AirWatch erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **AirWatch**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **AirWatch**.

    ![Uygulamalar listesinde AirWatch bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-airwatch-test-user"></a>AirWatch test kullanıcısı oluşturma

Azure AD kullanıcıları için AirWatch oturum açmak etkinleştirmek için bunlar içinde AirWatch için sağlanması gerekir. AirWatch söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **AirWatch** şirketinizin sitesi yöneticisi olarak.

2. Sol taraftaki gezinti bölmesinde **hesapları**ve ardından **kullanıcılar**.
  
   ![Kullanıcılar](./media/airwatch-tutorial/ic791929.png "kullanıcılar")

3. İçinde **kullanıcılar** menüsünde tıklatın **liste görünümü**ve ardından **Ekle \> Kullanıcı Ekle**.
  
   ![Kullanıcı ekleme](./media/airwatch-tutorial/ic791930.png "kullanıcı ekleme")

4. Üzerinde **Ekle / Düzenle kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

   ![Kullanıcı ekleme](./media/airwatch-tutorial/ic791931.png "kullanıcı ekleme")

   a. Tür **kullanıcıadı**, **parola**, **parolayı onaylayın**, **ad**, **Soyadı**,  **E-posta adresi** istediğiniz ilgili metin kutularına zbilgisayarlar geçerli bir Azure Active Directory hesabı.

   b. **Kaydet**’e tıklayın.

> [!NOTE]
> Herhangi diğer AirWatch kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak AirWatch tarafından sağlanan.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli AirWatch kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama AirWatch için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
