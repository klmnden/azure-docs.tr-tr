---
title: 'Öğretici: Soloinsight CloudGate SSO ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Soloinsight CloudGate SSO arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 9263c241-85a4-4724-afac-0351d6275958
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/07/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6e8b2b4d1a660fe2f1289bba6fa596d08ec824b8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60341480"
---
# <a name="tutorial-azure-active-directory-integration-with-soloinsight-cloudgate-sso"></a>Öğretici: Soloinsight CloudGate SSO ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Soloinsight CloudGate SSO tümleştirme konusunda bilgi edinin.
Soloinsight CloudGate SSO, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Soloinsight CloudGate SSO erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak Soloinsight CloudGate SSO (çoklu oturum açma) ile Azure AD hesaplarına oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Soloinsight CloudGate SSO ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik Soloinsight CloudGate SSO çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Soloinsight CloudGate SSO'yu destekler **SP** tarafından başlatılan

## <a name="adding-soloinsight-cloudgate-sso-from-the-gallery"></a>Galeriden Soloinsight CloudGate SSO ekleme

Azure AD'de Soloinsight CloudGate SSO tümleştirmesini yapılandırmak için Soloinsight CloudGate SSO Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Soloinsight CloudGate SSO eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Soloinsight CloudGate SSO**seçin **Soloinsight CloudGate SSO** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Soloinsight CloudGate SSO](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Soloinsight CloudGate adlı bir test kullanıcı tabanlı SSO ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının Soloinsight CloudGate SSO ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Soloinsight CloudGate SSO ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Soloinsight CloudGate SSO çoklu oturum açmayı yapılandırma](#configure-soloinsight-cloudgate-sso-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Soloinsight CloudGate SSO test kullanıcısı oluşturma](#create-soloinsight-cloudgate-sso-test-user)**  - kullanıcı Azure AD gösterimini bağlı Soloinsight CloudGate SSO Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Soloinsight CloudGate SSO ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Soloinsight CloudGate SSO** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Soloinsight CloudGate SSO etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<SUBDOMAIN>.sigateway.com/login`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<SUBDOMAIN>.sigateway.com/process/sso`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve daha sonra açıklanan tanımlayıcısı ile güncelleştirme **yapılandırma Soloinsight CloudGate SSO çoklu oturum açma** öğreticinin bölümünde.

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **Soloinsight CloudGate SSO'yu ayarlama** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-soloinsight-cloudgate-sso-single-sign-on"></a>Soloinsight CloudGate SSO çoklu oturum açmayı yapılandırın

1. Ardından kimlik bilgilerinizi kullanarak CloudGate Web portalı Azure portalında temel SAML yapılandırılırken yapıştırılmasına değerleri almak için şu yolda bulunabilir SSO ayarlarını erişim **Giriş > Yönetim > Sistem Ayarlar > Genel**.

    ![CloudGate SSO ayarları](./media/soloinsight-cloudgate-sso-tutorial/sso-main-settings.png)

2. **SAML tüketici URL'si**

    * Bağlantıları karşı kopyalama **Saml tüketici URL** ve **tekrar yönlendirme URL'sini** alanları ve bunları Azure Portalı'nda yapıştırın **temel SAML yapılandırma** bölümü **Tanımlayıcı (varlık kimliği)** ve **yanıt URL'si** sırasıyla alanları.

        ![SAMLIdentifier](./media/soloinsight-cloudgate-sso-tutorial/saml-identifier.png)

3. **SAML imzalama sertifikası**

    * Azure portalı SAML imzalama sertifikası listeler ve sağ indirildiğini sertifika (Base64) dosyanın kaynağına gidin. Seçin **Düzenle not defteri ++ ile** listeden seçeneği. 

        ![SAMLcertificate](./media/soloinsight-cloudgate-sso-tutorial/certificate-file.png)

    * İçeriği sertifika (Base64) not defteri ++ dosyasına kopyalayın.

        ![Sertifika kopyalama](./media/soloinsight-cloudgate-sso-tutorial/certificate-copy.png)

    * İçerik CloudGate Web portalı SSO ayarlarında yapıştırmak **sertifika** alan ve düğmesine tıklayın.

        ![Sertifika portalı](./media/soloinsight-cloudgate-sso-tutorial/certificate-portal.png)

4. **Varsayılan grubu**

    * Seçin **Business Admin** aşağı açılan listesinden **varsayılan grup** CloudGate Web portalında seçeneği

        ![Varsayılan grubu](./media/soloinsight-cloudgate-sso-tutorial/default-group.png)

5. **AD tanımlayıcısı ve oturum açma URL'si**

    * Kopyalanan **oturum açma URL'si** Azure Portal'dan **Soloinsight CloudGate SSO'yu ayarlama** yapılandırmalarıdır CloudGate Web portalı SSO ayarları bölümünde girilmelidir. 

    * Yapıştırma **oturum açma URL'si** CloudGate Web portalında Azure Portalı'ndan bağlantısı **AD oturum açma URL'si** alan.
     
    * Yapıştırma **Azure AD tanımlayıcısı** CloudGate Web portalında Azure Portalı'ndan bağlantısı **AD tanımlayıcısı** alan

        ![Ad oturum açma](./media/soloinsight-cloudgate-sso-tutorial/ad-login.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alanına **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alanına **brittasimon\@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Soloinsight CloudGate SSO erişimi vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Soloinsight CloudGate SSO**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Soloinsight CloudGate SSO**.

    ![Uygulamalar listesinde Soloinsight CloudGate SSO bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından'a tıklayın **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-soloinsight-cloudgate-sso-test-user"></a>Soloinsight CloudGate SSO test kullanıcısı oluşturma

Bir test kullanıcısı oluşturmak için Seç **çalışanlar** ana menüden CloudGate Web portalı ve yeni Ekle çalışan formu doldurun. Test kullanıcıya atanacak yetki düzeyi **Business Admin** tıklayarak **Oluştur** sonra tüm gerekli alanları doldurulur.

![Çalışan test](./media/soloinsight-cloudgate-sso-tutorial/employee-test.png)

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Soloinsight CloudGate SSO kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Soloinsight CloudGate SSO için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

