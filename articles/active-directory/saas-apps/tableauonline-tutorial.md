---
title: 'Öğretici: Tableau Online ile Azure Active Directory Tümleştirmesi | Microsoft Docs'
description: Azure Active Directory ve Tableau Online arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1d4b1149-ba3b-4f4e-8bce-9791316b730d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/05/2019
ms.author: jeedes
ms.openlocfilehash: 352ad9473a1c1a9360ddceb720ff968f4e97e012
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65889290"
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-online"></a>Öğretici: Tableau Online ile Azure Active Directory Tümleştirmesi

Bu öğreticide, Tableau çevrimiçi Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.
Tableau çevrimiçi Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Tableau Online'a erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak Tableau Online'a (çoklu oturum açma) oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Tableau Online ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Tableau çevrimiçi çoklu oturum açma abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Tableau çevrimiçi destekler **SP** tarafından başlatılan

## <a name="adding-tableau-online-from-the-gallery"></a>Tableau çevrimiçi galeriden ekleme

Azure AD'de Tableau Online'nın tümleştirmesini yapılandırmak için Tableau çevrimiçi galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Tableau çevrimiçi Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Tableau çevrimiçi**seçin **Tableau çevrimiçi** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde tableau çevrimiçi](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Tableau adlı bir test kullanıcı tabanlı Online ile Azure AD çoklu oturum açmayı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve ilgili kullanıcı Tableau Online arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Tableau Online ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Tableau çevrimiçi çoklu oturum açmayı yapılandırma](#configure-tableau-online-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Tableau çevrimiçi test kullanıcısı oluşturma](#create-tableau-online-test-user)**  - Tableau kullanıcı Azure AD gösterimini bağlı olan çevrimiçi bir karşılığı Britta simon'un sağlamak için.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Tableau Online ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Tableau çevrimiçi** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Tableau çevrimiçi etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna URL'yi yazın: `https://sso.online.tableau.com/public/sp/login?alias=<entityid>`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna URL'yi yazın: `https://sso.online.tableau.com/public/sp/metadata?alias=<entityid>`

    > [!NOTE]
    > Erişmenizi sağlayacak `<entityid>` değerini **Tableau çevrimiçi kümesi** Bu öğretici bölümünde. Varlık Kimliği değeri olacak **Azure AD tanımlayıcısı** değerini **Tableau çevrimiçi kümesi** bölümü.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. Üzerinde **Tableau çevrimiçi kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-tableau-online-single-sign-on"></a>Tableau çevrimiçi çoklu oturum açmayı yapılandırın

1. Farklı bir tarayıcı penceresinde, Tableau çevrimiçi uygulamanıza oturum. Git **ayarları** ardından **kimlik doğrulaması**.

    ![Çoklu oturum açmayı yapılandırın](./media/tableauonline-tutorial/tutorial_tableauonline_09.png)

2. SAML, altında etkinleştirmek için **kimlik doğrulama türleri** bölümü. Denetleme **bir ek kimlik doğrulama yöntemini etkinleştirin** iade edin **SAML** onay kutusu.

    ![Çoklu oturum açmayı yapılandırın](./media/tableauonline-tutorial/tutorial_tableauonline_12.png)

3. Kadar aşağı kaydırın **alma meta veri dosyası içine Tableau çevrimiçi** bölümü.  Gözat'a tıklayın ve Azure AD'den yüklediğiniz meta veri dosyası içeri aktarın. ' A tıklayarak **Uygula**.

   ![Çoklu oturum açmayı yapılandırın](./media/tableauonline-tutorial/tutorial_tableauonline_13.png)

4. İçinde **eşleşen onaylar** bölümünde, eklemek için karşılık gelen kimlik sağlayıcı onaylama adı **e-posta adresi**, **ad**, ve **Soyadı**. Azure AD'den bu bilgileri almak için: 
  
    a. Azure portalında, go **Tableau çevrimiçi** uygulama tümleştirme sayfası.

    b. İçinde **kullanıcı öznitelikleri ve talepler** bölümünde, düzenleme simgesine tıklayın.

   ![Çoklu oturum açmayı yapılandırın](./media/tableauonline-tutorial/attributesection.png)

    c. Bu öznitelikleri ad alanı değerini kopyalayın: givenname, e-posta ve aşağıdaki adımları kullanarak Soyadı:

   ![Azure AD çoklu oturum açma](./media/tableauonline-tutorial/tutorial_tableauonline_10.png)

    d. Tıklayın **user.givenname** değeri

    e. Değeri Şuradan Kopyala: **Namespace** metin.

    ![Çoklu oturum açmayı yapılandırın](./media/tableauonline-tutorial/attributesection2.png)

    f. Ad alanı kopyalamak için değerler soyadı ve e-posta için yukarıdaki adımları yineleyin.

    g. Tableau çevrimiçi uygulamaya geçiş yapın ve ardından ayarlamak **kullanıcı öznitelikleri ve talepler** gibi bölümünde:

    * E-posta: **posta** veya **userprincipalname**

    * Ad: **givenname**

    * Soyadı: **Soyadı**

    ![Çoklu oturum açmayı yapılandırın](./media/tableauonline-tutorial/tutorial_tableauonline_14.png)

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
    Örneğin, BrittaSimon\@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Tableau çevrimiçi erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Tableau çevrimiçi**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Tableau çevrimiçi**.

    ![Uygulamalar listesinde Tableau çevrimiçi bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-tableau-online-test-user"></a>Tableau çevrimiçi test kullanıcısı oluşturma

Bu bölümde, Britta Simon Tableau Online'da adlı bir kullanıcı oluşturun.

1. Üzerinde **Tableau çevrimiçi**, tıklayın **ayarları** ardından **kimlik doğrulaması** bölümü. Ekranı aşağı kaydırarak **Kullanıcıları Yönet** bölümü. Tıklayın **Add Users** ve ardından **e-posta adreslerini girin**.
  
    ![Bir Azure AD test kullanıcısı oluşturma](./media/tableauonline-tutorial/tutorial_tableauonline_15.png)

2. Seçin **(SAML) kimlik doğrulaması için kullanıcı ekleme**. İçinde **Enter e-posta adreslerini** metin kutusu ekleme britta.simon\@contoso.com
  
    ![Bir Azure AD test kullanıcısı oluşturma](./media/tableauonline-tutorial/tutorial_tableauonline_11.png)

3. Tıklayın **kullanıcı ekleme**.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Tableau çevrimiçi kutucuğa tıkladığınızda, size otomatik olarak Tableau çevrimiçi SSO'yu ayarlamak için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
