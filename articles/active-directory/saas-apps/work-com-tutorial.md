---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Work.com | Microsoft Docs'
description: Azure Active Directory ve Work.com arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 98e6739e-eb24-46bd-9dd3-20b489839076
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 816f9bfe022b4a00c01c3ee1bc243f87ef56817b
ms.sourcegitcommit: b8a8d29fdf199158d96736fbbb0c3773502a092d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2019
ms.locfileid: "59565946"
---
# <a name="tutorial-azure-active-directory-integration-with-workcom"></a>Öğretici: Work.com ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Work.com tümleştirme konusunda bilgi edinin.
Azure AD ile Work.com tümleştirme ile aşağıdaki avantajları sağlar:

* Work.com erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) Work.com için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Work.com yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Abonelik Work.com çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Work.com destekler **SP** tarafından başlatılan

## <a name="adding-workcom-from-the-gallery"></a>Galeriden Work.com ekleme

Azure AD'de Work.com tümleştirmesini yapılandırmak için Work.com Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Work.com eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Work.com**seçin **Work.com** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Work.com](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Work.com adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının Work.com ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Work.com ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Work.com çoklu oturum açmayı yapılandırma](#configure-workcom-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Work.com test kullanıcısı oluşturma](#create-workcom-test-user)**  - kullanıcı Azure AD gösterimini bağlı Work.com Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

>[!NOTE]
>Çoklu oturum açmayı yapılandırmak için bir özel Work.com etki alanı adı henüz Kurulum gerekir. En az bir etki alanı adı tanımlayın, etki alanı adınızı test etmek ve tüm kuruluşunuza dağıtmanız gerekir.

Azure AD çoklu oturum açma ile Work.com yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Work.com** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Work.com etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `http://<companyname>.my.salesforce.com`

    > [!NOTE]
    > Değer, gerçek değil. Değerini gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Work.com istemci Destek ekibine](https://help.salesforce.com/articleView?id=000159855&type=3) değeri alınamıyor. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **Work.com kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-workcom-single-sign-on"></a>Work.com tek oturum açmayı yapılandırın

1. Work.com Kiracı yönetici olarak oturum açın.

2. Git **Kurulum**.
   
    ![Kurulum](./media/work-com-tutorial/ic794108.png "Kurulumu")

3. Sol gezinti bölmesinde, içinde **Yönet** bölümünde **etki alanı yönetimi** ilgili bölümü genişletin ve ardından **My Domain** açmakiçin**My Domain** sayfası. 
   
    ![Etki alanım](./media/work-com-tutorial/ic767825.png "etki alanım")

4. Etki alanınızı doğru şekilde ayarlandığını gösterdiğinde olduğunu doğrulamak için içinde olduğundan emin olun "**4 adım dağıtılan kullanıcılara**" gözden geçirin, "**etki alanı ayarlarım**".
   
    ![Etki alanı kullanıcıya dağıtılan](./media/work-com-tutorial/ic784377.png "kullanıcıya dağıtılan etki alanı")

5. Work.com kiracınızda oturum açın.

6. Git **Kurulum**.
    
    ![Kurulum](./media/work-com-tutorial/ic794108.png "Kurulumu")

7. Genişletin **güvenlik denetimleri** menüsüne ve ardından **çoklu oturum açma ayarları**.
    
    ![Çoklu oturum açma ayarları](./media/work-com-tutorial/ic794113.png "çoklu oturum açma ayarları")

8. Üzerinde **çoklu oturum açma ayarları** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
    
    ![SAML etkin](./media/work-com-tutorial/ic781026.png "SAML etkin")
    
    a. Seçin **SAML etkin**.
    
    b. **Yeni**’ye tıklayın.

9. İçinde **SAML çoklu oturum açma ayarları** bölümünde, aşağıdaki adımları gerçekleştirin:
    
    ![Oturum açma SAML tek ayar](./media/work-com-tutorial/ic794114.png "oturum açma SAML tek ayar")
    
    a. İçinde **adı** metin yapılandırmanız için bir ad yazın.  
       
    > [!NOTE]
    > İçin bir değer sağlanması **adı** Otomatik Doldur **API adı** metin.
    
    b. İçinde **veren** metin değerini yapıştırın **Azure AD tanımlayıcısı** , Azure Portalı'ndan kopyaladığınız.
    
    c. Azure portalından indirilen sertifikayı karşıya yüklemek için tıklayın **Gözat**.
    
    d. İçinde **varlık kimliği** metin kutusuna `https://salesforce-work.com`.
    
    e. Olarak **SAML kimlik türü**seçin **onaylamayı içeren kullanıcı nesnesinden Federasyon kimliği**.
    
    f. Olarak **SAML kimlik konumu**seçin **kimliğidir konu deyiminin NameIdentfier öğesinde**.
    
    g. İçinde **kimlik sağlayıcısı oturum açma URL'si** metin değerini yapıştırın **oturum açma URL'si** , Azure Portalı'ndan kopyaladığınız.

    h. İçinde **kimlik sağlayıcısı oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.
    
    i. Olarak **hizmet sağlayıcısı tarafından başlatılan bağlama isteği**seçin **HTTP Post**.
    
    j. **Kaydet**’e tıklayın.

10. Sol gezinti bölmesinde, Work.com Klasik Portalı'nda tıklatın **etki alanı yönetimi** ilgili bölümü genişletin ve ardından **My Domain** açmak için **My Domain** Sayfa. 
    
    ![Etki alanım](./media/work-com-tutorial/ic794115.png "etki alanım")

11. Üzerinde **My Domain** sayfasında **oturum açma sayfasında bulunan marka** bölümünde **Düzenle**.
    
    ![Oturum açma sayfası markalama](./media/work-com-tutorial/ic767826.png "oturum açma sayfası markalama")

12. Üzerinde **oturum açma sayfasında bulunan marka** sayfasında **kimlik doğrulama hizmeti** bölümü adı, **SAML SSO ayarlarını** görüntülenir. Seçin ve ardından **Kaydet**.
    
    ![Oturum açma sayfası markalama](./media/work-com-tutorial/ic784366.png "oturum açma sayfası markalama")

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

Bu bölümde, Azure çoklu oturum açma kullanmak için Work.com erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Work.com**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Work.com**.

    ![Uygulamalar listesinde Work.com bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-workcom-test-user"></a>Work.com test kullanıcısı oluşturma

Azure Active Directory Kullanıcıları oturum açabilmesi, bunlar için Work.com sağlanmalıdır. Work.com söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Work.com şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Git **Kurulum**.
   
    ![Kurulum](./media/work-com-tutorial/IC794108.png "Kurulumu")

3. Git **kullanıcıları yönetme \> kullanıcılar**.
   
    ![Kullanıcıları Yönet](./media/work-com-tutorial/IC784369.png "kullanıcıları yönetme")

4. Tıklayın **yeni kullanıcı**.
   
    ![Tüm kullanıcılar](./media/work-com-tutorial/IC794117.png "tüm kullanıcılar")

5. Kullanıcı düzenleme bölümünde, aşağıdaki adımlarda, geçerli bir Azure özniteliklerini gerçekleştirmek istediğiniz ilgili metin kutularına zbilgisayarlar AD hesabı:
   
    ![Kullanıcı düzenleme](./media/work-com-tutorial/ic794118.png "kullanıcı düzenleme")
   
    a. İçinde **ad** metin kutusuna **ad** kullanıcının **Britta**.
    
    b. İçinde **Soyadı** metin kutusuna **Soyadı** kullanıcının **Simon**.
    
    c. İçinde **diğer** metin kutusuna **adı** kullanıcının **BrittaS**.
    
    d. İçinde **e-posta** metin kutusuna **e-posta adresi** kullanıcının Brittasimon@contoso.com.
    
    e. İçinde **kullanıcı adı** metin kutusu, türü, bir kullanıcının kullanıcı adını ister Brittasimon@contoso.com.
    
    f. İçinde **takma ad** metin bir **takma ad** kullanıcının **Simon**.
    
    g. Seçin **rol**, **kullanıcı lisansı**, ve **profili**.
    
    h. **Kaydet**’e tıklayın.  
      
    > [!NOTE]
    > Azure AD hesap sahibinin etkin hale gelir önce hesabı onaylamak için bir bağlantı içeren bir e-posta alırsınız.
    > 

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Work.com kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Work.com için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

