---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Zscaler özel erişim Yöneticisi | Microsoft Docs'
description: Azure Active Directory ve Zscaler özel erişim Yöneticisi arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: c87392a7-e7fe-4cdc-a8e6-afe1ed975172
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ce75431de24886c038cd2eb4ee7db02d2b6cde31
ms.sourcegitcommit: b8a8d29fdf199158d96736fbbb0c3773502a092d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2019
ms.locfileid: "59563362"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-private-access-administrator"></a>Öğretici: Azure Active Directory Tümleştirmesi ile Zscaler özel erişim Yöneticisi

Bu öğreticide, Azure Active Directory (Azure AD) ile Zscaler özel erişim Yöneticisi tümleştirme konusunda bilgi edinin.
Zscaler özel erişim Yöneticisi, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Zscaler için özel erişim Yöneticisi erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak Zscaler özel erişim Yöneticisi (çoklu oturum açma) için oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Zscaler özel erişim Yöneticisi ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Zscaler özel erişim Yöneticisi çoklu oturum açma abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Zscaler özel erişim Yöneticisi destekler **SP** ve **IDP** tarafından başlatılan

## <a name="adding-zscaler-private-access-administrator-from-the-gallery"></a>Galeriden Zscaler özel erişim yönetici ekleme

Azure AD'de Zscaler özel erişim Yöneticisi'nin tümleştirmesini yapılandırmak için Zscaler özel erişim Yöneticisi galerideki yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Zscaler özel erişim yönetici eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Zscaler özel erişim Yöneticisi**seçin **Zscaler özel erişim Yöneticisi** sonucu panelinden ardından **Ekle** düğme eklemek için uygulama.

    ![Sonuç listesinde Zscaler özel erişim Yöneticisi](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Zscaler özel erişim adlı bir test kullanıcı tabanlı Yöneticisi ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ile ilgili kullanıcı Zscaler özel erişim Yöneticisi'nde arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Zscaler özel erişim Yöneticisi ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Zscaler özel erişim yönetici çoklu oturum açmayı yapılandırma](#configure-zscaler-private-access-administrator-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Zscaler özel erişim yönetici test kullanıcısı oluşturma](#create-zscaler-private-access-administrator-test-user)**  - Zscaler özel erişim kullanıcı Azure AD gösterimini bağlı Yöneticisi'nde Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Zscaler özel erişim Yöneticisi ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Zscaler özel erişim Yöneticisi** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    ![Zscaler özel erişim yöneticisinin etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-relay.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<subdomain>.private.zscaler.com/auth/metadata`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<subdomain>.private.zscaler.com/auth/sso`

    c. Tıklayın **ek URL'lerini ayarlayın**.

    d. İçinde **geçiş durumu** metin kutusuna bir URL yazın: `idpadminsso`

5.  Uygulamada yapılandırmak istiyorsanız **SP** modunda başlatılan aşağıdaki adımı uygulayın:

    ![Zscaler özel erişim yöneticisinin etki alanı ve URL'ler tek oturum açma bilgileri](common/both-signonurl.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<subdomain>.private.zscaler.com/auth/sso`   

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [Zscaler özel erişim Yöneticisi istemci Destek ekibine](https://help.zscaler.com/zpa-submit-ticket) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. Üzerinde **Zscaler özel erişim Yöneticisi yedekleme kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-zscaler-private-access-administrator-single-sign-on"></a>Zscaler özel erişim Yöneticisi tek oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde Zscaler özel erişim Yöneticisi için bir yönetici olarak oturum açın.

2. En üstte tıklayın **Yönetim** gidin **kimlik doğrulaması** bölümünde **IDP yapılandırmasını**.

    ![Zscaler özel erişim Yöneticisi Yönetici](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_admin.png)

3. Sağ üst köşedeki, tıklayın **IDP Yapılandırması Ekle**. 

    ![Zscaler özel erişim Yöneticisi addidp](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_addpidp.png)

4. Üzerinde **IDP Yapılandırması Ekle** sayfasında aşağıdaki adımları gerçekleştirin:
 
    ![Zscaler özel erişim Yöneticisi idpselect](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_idpselect.png)

    a. Tıklayın **Dosya Seç** indirdiğiniz Azure AD'de meta verileri dosyadan karşıya yüklemek için **IDP meta veri dosyasını karşıya yükleyin.** alan.

    b. Okuduğu **IDP meta verileri** Azure AD'den ve aşağıda gösterildiği gibi tüm alanları bilgileri doldurur.

    ![Zscaler özel erişim Yöneticisi idpconfig](./media/zscalerprivateaccessadministrator-tutorial/idpconfig.png)

    c. Seçin **tekli oturum** olarak **yönetici**.

    d. Etki alanınızdan seçin **etki alanları** alan.
    
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
  
    b. İçinde **kullanıcı adı** alan türü `brittasimon@yourcompanydomain.extension`. Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Zscaler özel erişim Yöneticisi erişimi vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Zscaler özel erişim Yöneticisi**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Zscaler özel erişim Yöneticisi**.

    ![Uygulamalar listesinde Zscaler özel erişim Yöneticisi bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-zscaler-private-access-administrator-test-user"></a>Zscaler özel erişim yönetici test kullanıcısı oluşturma

Zscaler özel erişim Yöneticisi için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar Zscaler özel erişim Yöneticisi olarak sağlanması gerekir. Zscaler özel erişim Yöneticisi söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Zscaler özel erişim Yöneticisi şirketinizin sitesi için bir yönetici olarak oturum açın.

2. En üstte tıklayın **Yönetim** gidin **kimlik doğrulaması** bölümünde **IDP yapılandırmasını**.

    ![Zscaler özel erişim Yöneticisi Yönetici](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_admin.png)

3. Tıklayın **Yöneticiler** sol tarafındaki menüyü.

    ![Zscaler özel erişim Yöneticisi Yönetici](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_adminstrator.png)

4. Sağ üst köşedeki, tıklayın **Yöneticisi Ekle**:

    ![Zscaler özel erişim yönetici, yönetici Ekle](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_addadmin.png)

5. İçinde **Yöneticisi Ekle** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Zscaler özel erişim Yönetici Kullanıcı Yöneticisi](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_useradmin.png)

    a. İçinde **kullanıcıadı** metin gibi kullanıcının e-posta girin BrittaSimon@contoso.com.

    b. İçinde **parola** metin kutusuna parolayı yazın.

    c. İçinde **parolayı onayla** metin kutusuna parolayı yazın.

    d. Seçin **rol** olarak **Zscaler özel erişim Yöneticisi**.

    e. İçinde **e-posta** metin gibi kullanıcının e-posta girin BrittaSimon@contoso.com.

    f. İçinde **telefon** metin telefon numarasını yazın.

    g. İçinde **saat dilimi** metin saat dilimi seçin.

    h. **Kaydet**’e tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Zscaler özel erişim Yöneticisi kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Zscaler özel erişim yönetici oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

