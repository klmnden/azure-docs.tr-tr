---
title: 'Öğretici: Salesforce ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ile Salesforce arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: d2d7d420-dc91-41b8-a6b3-59579e043b35
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/10/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 27a61205426cbf43fd3b3b549909ffa13ff07dc7
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62106057"
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce"></a>Öğretici: Salesforce ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Salesforce tümleştirme konusunda bilgi edinin.
Azure AD ile Salesforce tümleştirme ile aşağıdaki avantajları sağlar:

* Salesforce erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak Salesforce'a (çoklu oturum açma) ile Azure AD hesaplarına oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Salesforce yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Salesforce çoklu oturum açma abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Salesforce destekler **SP** tarafından başlatılan

* Salesforce destekler **zamanında** kullanıcı sağlama

* Salesforce destekler [ **otomatik** kullanıcı sağlama](salesforce-provisioning-tutorial.md)

## <a name="adding-salesforce-from-the-gallery"></a>Salesforce galeri ekleme

Azure AD'de Salesforce tümleştirmesini yapılandırmak için Salesforce Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Salesforce Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için tıklatın **yeni uygulama** iletişim kutusunun üstündeki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Salesforce**seçin **Salesforce** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Salesforce](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Salesforce adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve salesforce'ta ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Salesforce ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Salesforce çoklu oturum açmayı yapılandırma](#configure-salesforce-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Salesforce test kullanıcısı oluşturma](#create-salesforce-test-user)**  - kullanıcı Azure AD gösterimini bağlı Salesforce Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile Salesforce yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Salesforce** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Salesforce etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak değeri yazın:

    Kuruluş hesabı: `https://<subdomain>.my.salesforce.com`

    Geliştirici hesabı: `https://<subdomain>-dev-ed.my.salesforce.com`

    b. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak değeri yazın:

    Kuruluş hesabı: `https://<subdomain>.my.salesforce.com`

    Geliştirici hesabı: `https://<subdomain>-dev-ed.my.salesforce.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri tanımlayıcısı ve gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Salesforce müşteri destek ekibi](https://help.salesforce.com/support) bu değerleri almak için.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. Üzerinde **Salesforce kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-salesforce-single-sign-on"></a>Salesforce tek oturum açmayı yapılandırın

1. Tarayıcınızda yeni bir sekme açın ve Salesforce yönetici hesabınızda oturum açın.

2. Tıklayarak **Kurulum** altında **ayarlar simgesine** sayfanın sağ üst köşesinde.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-tutorial/configure1.png)

3. Ekranı aşağı kaydırarak **ayarları** Gezinti bölmesinden **kimlik** ilgili bölümü genişletin. Ardından **çoklu oturum açma ayarları**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-tutorial/sf-admin-sso.png)

4. Üzerinde **çoklu oturum açma ayarları** sayfasında **Düzenle** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-tutorial/sf-admin-sso-edit.png)

    > [!NOTE]
    > Çoklu oturum açma ayarlarını Salesforce hesabınız için etkinleştirmek erişemiyorsanız başvurmanız gerekebilir [Salesforce müşteri destek ekibi](https://help.salesforce.com/support).

5. Seçin **SAML etkin**ve ardından **Kaydet**.

      ![Çoklu oturum açmayı yapılandırın](./media/salesforce-tutorial/sf-enable-saml.png)

6. SAML çoklu oturum açma ayarlarınızı yapılandırmak için tıklayın **yeni meta veri dosyasından**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-tutorial/sf-admin-sso-new.png)

7. Tıklayın **Dosya Seç** Azure portal'ı seçin ve indirilen meta veri XML dosyasını karşıya yüklemek için **Oluştur**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-tutorial/xmlchoose.png)

8. Üzerinde **SAML çoklu oturum açma ayarları** sayfasında alanları otomatik olarak doldurur ve Kaydet'e tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-tutorial/salesforcexml.png)

9. Sol gezinti bölmesinde üzerinde Salesforce **şirket ayarları** ilgili bölümü genişletin ve ardından **My Domain**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-tutorial/sf-my-domain.png)

10. Ekranı aşağı kaydırarak **kimlik doğrulama Yapılandırması** bölümünde ve'ı tıklatın **Düzenle** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-tutorial/sf-edit-auth-config.png)

11. İçinde **kimlik doğrulama Yapılandırması** bölümünde, onay **AzureSSO** olarak **kimlik doğrulama hizmeti** SAML SSO yapılandırmasını ve ardından  **Kaydet**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-tutorial/sf-auth-config.png)

    > [!NOTE]
    > Birden fazla kimlik doğrulama hizmeti seçili ise, kullanıcıların hangi kimlik doğrulama hizmeti ile çoklu oturum açma Salesforce ortamınıza başlatma oturum ister seçmek için istenir. Olmasını istemediğiniz sonra yapmanız gerekenler **diğer tüm kimlik doğrulama hizmetleri işaretlemeyin**.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü `brittasimon\@yourcompanydomain.extension`. Örneğin, BrittaSimon@contoso.com.

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma Salesforce'a erişim vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Salesforce**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Salesforce**.

    ![Uygulamalar listesinde Salesforce bağlantısı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-salesforce-test-user"></a>Salesforce test kullanıcısı oluşturma

Bu bölümde, Salesforce'ta Britta Simon adlı bir kullanıcı oluşturuldu. Salesforce tam zamanında sağlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Salesforce erişmeye çalıştığında, Salesforce'ta bir kullanıcı zaten mevcut değilse yeni bir tane oluşturulur. Salesforce da destekler otomatik kullanıcı hazırlama, daha fazla ayrıntı bulabilirsiniz [burada](salesforce-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Salesforce kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama salesforce'a oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Kullanıcı sağlamayı yapılandırma](salesforce-provisioning-tutorial.md)
