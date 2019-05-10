---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Displayr | Microsoft Docs'
description: Azure Active Directory ve Displayr arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b739b4e3-1a37-4e3c-be89-c3945487f4c1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9694790ea02bc778bf3b9db212e61fabb90a28a8
ms.sourcegitcommit: 4891f404c1816ebd247467a12d7789b9a38cee7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65441437"
---
# <a name="tutorial-azure-active-directory-integration-with-displayr"></a>Öğretici: Displayr ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Displayr tümleştirme konusunda bilgi edinin.
Azure AD ile Displayr tümleştirme ile aşağıdaki avantajları sağlar:

* Displayr erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) Displayr için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Displayr yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Abonelik Displayr çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Displayr destekler **SP** tarafından başlatılan

## <a name="adding-displayr-from-the-gallery"></a>Galeriden Displayr ekleme

Azure AD'de Displayr tümleştirmesini yapılandırmak için Displayr Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Displayr eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Displayr**seçin **Displayr** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Displayr](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Displayr adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının Displayr ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Displayr ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Displayr çoklu oturum açmayı yapılandırma](#configure-displayr-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Displayr test kullanıcısı oluşturma](#create-displayr-test-user)**  - kullanıcı Azure AD gösterimini bağlı Displayr Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile Displayr yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Displayr** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **Kurulum çoklu oturum açma SAML ile** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımı uygulayın:

    ![Displayr etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-intiated.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL yazın:  `https://app.displayr.com/Login`

5. İçinde **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (ham)** verilen seçeneklerden ihtiyacınıza göre ve üzerinde kaydedin, bilgisayar.

    ![Sertifika indirme bağlantısı](common/certificateraw.png)

6. Displayr uygulama, özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde SAML onaylamalarını bekler. Aşağıdaki ekran görüntüsünde, varsayılan öznitelikler listesinde gösterilmiştir. Tıklayın **Düzenle** kullanıcı öznitelikleri iletişim kutusunu açmak için simge.

    ![image](common/edit-attribute.png)

7. Yukarıdaki için ayrıca Displayr uygulama SAML yanıtta geçirilecek birkaç daha fazla öznitelik bekliyor. İçinde **kullanıcı öznitelikleri ve talepler** bölümünde **grup talepleri (Önizleme)** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    a. Tıklayın **kalem** yanındaki **grupları döndürülen talebi**.

    ![image](./media/displayr-tutorial/config04.png)

    ![image](./media/displayr-tutorial/config05.png)

    b. Seçin **tüm grupları** radyo listeden.

    c. Seçin **kaynak özniteliği** , **Grup Kimliği**.

    d. Denetleme **grup talebi adını özelleştirme**.

    e. Denetleme **yayma gruplar olarak rol talepleri**.

    f. **Kaydet**’e tıklayın.

8. Üzerinde **Kurulum Displayr** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-displayr-single-sign-on"></a>Displayr tek oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde Displayr için yönetici olarak oturum açın.

2. Tıklayarak **ayarları** gidin **hesabı**.

    ![Yapılandırma](./media/displayr-tutorial/config01.png)

3. Geçiş **ayarları** tıklayarak için sayfayı aşağı kaydırın ve üstteki menüden **yapılandırma tek oturum üzerinde (SAML)**.

    ![Yapılandırma](./media/displayr-tutorial/config02.png)

4. Üzerinde **tek oturum üzerinde (SAML)** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Yapılandırma](./media/displayr-tutorial/config03.png)

    a. Denetleme **etkinleştirme tek oturum üzerinde (SAML)** kutusu.

    b. İçinde **veren** metin kutusunda, değerini yapıştırın **Azure AD tanımlayıcısı**, hangi Azure Portalı'ndan kopyaladığınız.

    c. İçinde **oturum açma URL'si** metin kutusunda, değerini yapıştırın **oturum açma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    d. İçinde **oturum kapatma URL'si** metin kutusunda, değerini yapıştırın **oturum kapatma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    e. Sertifika (ham) Not Defteri'nde açın, içeriğini kopyalayın ve yapıştırın **sertifika** metin kutusu.

    f. **Grup eşlemelerini** isteğe bağlıdır.

    g. **Kaydet**’e tıklayın.  

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alanına **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü `brittasimon@yourcompanydomain.extension`. Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Displayr erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Displayr**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Displayr**.

    ![Uygulamalar listesinde Displayr bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-displayr-test-user"></a>Displayr test kullanıcısı oluşturma

Azure AD kullanıcılarının etkinleştirmek için oturum içinde Displayr için bunların Displayr sağlanması gerekir. Displayr içinde sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Yönetici olarak Displayr için oturum açın.

2. Tıklayarak **ayarları** gidin **hesabı**.

    ![Displayr yapılandırma](./media/displayr-tutorial/config01.png)

3. Geçiş **ayarları** sayfayı aşağı kaydırın ve üstteki menüden kadar **kullanıcılar** bölümünde sonra tıklayın **yeni kullanıcı**.

    ![Displayr yapılandırma](./media/displayr-tutorial/config07.png)

4. Üzerinde **yeni kullanıcı** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Displayr yapılandırma](./media/displayr-tutorial/config06.png)

    a. İçinde **adı** metin kutusunda, gibi kullanıcı adını girin **Brittasimon**.

    b. İçinde **e-posta** metin kutusuna, kullanıcının gibi e-posta girin `Brittasimon@contoso.com`.

    c. Uygun seçin **grup üyeliği**.

    d. **Kaydet**’e tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Displayr kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Displayr için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

