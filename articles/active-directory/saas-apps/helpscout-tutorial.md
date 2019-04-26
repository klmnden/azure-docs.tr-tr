---
title: 'Öğretici: Scout yardımcı olan Azure Active Directory Tümleştirmesi | Microsoft Docs'
description: Azure Active Directory ve Yardım Scout arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 0aad9910-0bc1-4394-9f73-267cf39973ab
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/15/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 35f9a8949f5b51f88b9297890fc5562e7b8dd591
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60276313"
---
# <a name="tutorial-azure-active-directory-integration-with-help-scout"></a>Öğretici: Scout yardımcı olan Azure Active Directory Tümleştirmesi

Bu öğreticide, Azure Active Directory (Azure AD) ile Scout yardımcı tümleştirme konusunda bilgi edinin.
Scout yardımcı Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Scout yardımcı erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) Yardım Scout için oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Scout yardımcı Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Yardım Scout çoklu oturum açmayı abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Yardım Scout destekler **SP ve IDP** tarafından başlatılan
* Yardım Scout destekler **zamanında** kullanıcı sağlama

## <a name="adding-help-scout-from-the-gallery"></a>Scout yardımcı galeri ekleme

Azure AD'ye yardımcı Scout tümleştirmesini yapılandırmak için Yardım Scout Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden yardımcı Scout eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **yardımcı Scout**seçin **yardımcı Scout** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuçlar listesinde Scout Yardım](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Yardımı adlı bir test kullanıcı tabanlı Scout test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ile ilgili Yardım Scout kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Yardımı Scout ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Yardım Scout çoklu oturum açmayı yapılandırma](#configure-help-scout-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Scout yardımcı test kullanıcısı oluşturma](#create-help-scout-test-user)**  - yardımcı kullanıcı Azure AD gösterimini bağlı Scout Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Yardımı Scout ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **yardımcı Scout** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    ![Scout etki alanı ve URL'ler tek oturum açma bilgileri Yardım](common/idp-intiated.png)

    a. **Tanımlayıcı** olduğu **hedef kitle URİ'si (hizmet sağlayıcı varlık kimliği)** yardımcı Scout ' başlar `urn:`

    b. **Yanıt URL'si** olduğu **sonrası arka URL (onay belgesi tüketici hizmeti URL'si)** yardımcı Scout ' başlar `https://` 

    > [!NOTE]
    > Bu URL'ler gösterimi için değerler. Bu değerler gerçek yanıt URL'si ve tanımlayıcı güncelleştirmeniz gerekir. Bu değerleri almak **çoklu oturum açma** sekmesi altında kimlik doğrulaması bölümünde, öğreticinin ilerleyen bölümlerinde açıklanmıştır.

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Scout etki alanı ve URL'ler tek oturum açma bilgileri Yardım](common/metadata-upload-additional-signon.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL: `https://secure.helpscout.net/members/login/`

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. Üzerinde **yardımcı Scout kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-help-scout-single-sign-on"></a>Yardım Scout çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde Yardım Scout şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Tıklayarak **Yönet** seçin ve üstteki menüden **şirket** açılan menüden.

    ![Çoklu oturum açmayı yapılandırın](./media/helpscout-tutorial/settings1.png)

3. Seçin **kimlik doğrulaması** sol gezinti bölmesinden.

    ![Çoklu oturum açmayı yapılandırın](./media/helpscout-tutorial/settings2.png)

4. Bu SAML ayarları bölümüne alır ve aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/helpscout-tutorial/settings3.png)

    a. Kopyalama **sonrası arka URL (onay belgesi tüketici hizmeti URL'si)** değeri ve değer yapıştırın **yanıt URL'si** metin kutusu **temel SAML yapılandırma** Azure bölümünde Portalı.

    b. Kopyalama **hedef kitle URİ'si (hizmet sağlayıcı varlık kimliği)** değeri ve değer yapıştırın **tanımlayıcı** metin kutusu **temel SAML yapılandırma** bölümünde Azure portalında.

5. İki durumlu **etkinleştirme SAML** üzerinde ve aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/helpscout-tutorial/settings4.png)

    a. İçinde **çoklu oturum açma URL'si** metin değerini yapıştırın **oturum açma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    b. Tıklayın **sertifikasını karşıya yükle** yüklenecek **Certificate(Base64)** Azure portalından indirdiğiniz.

    c. Kuruluşunuzun e-posta etki alanları ör - enter `contoso.com` içinde **e-posta etki alanları** metin. Birden çok etki alanı virgülle ayırabilirsiniz. Dilediğiniz zaman yardımcı Scout kullanıcı veya belirli bir etki alanı girer üzerinde yönetici [yardımcı Scout oturum açma sayfası](https://secure.helpscout.net/members/login/) kendi kimlik bilgileriyle kimlik doğrulaması için kimlik sağlayıcısına yeniden yönlendirilir.

    d. Son olarak, geçiş **zorla SAML oturum açma** yalnızca Scout yardımcı olmak için bu yöntemle oturum açmasını istiyorsanız. Yine de bunları kendi Yardım Scout kimlik bilgileriyle oturum seçeneğini bırakın istiyorsanız, açılıp devre dışı bırakılabilir. Bu etkin olsa bile, bir hesap sahibi her zaman Scout yardımcı olmak için hesap parolalarını oturum açamaz olur.

    e. **Kaydet**’e tıklayın.

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

Bu bölümde, Scout yardımcı olmak için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **yardımcı Scout**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **yardımcı Scout**.

    ![Uygulamalar listesinde Scout Yardım bağlantısı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-help-scout-test-user"></a>Scout yardımcı test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı Yardımı Scout içinde oluşturulur. Yardım Scout just-ın-time kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Kullanıcı Yardım Scout içinde zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli yardımcı Scout kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Scout yardımcı olmak için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)