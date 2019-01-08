---
title: 'Öğretici: Palo Alto Networks - Captive portalı ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Palo Alto Networks - Captive portalı ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 67a0b476-2305-4157-8658-2ec3625850d5
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/25/2018
ms.author: jeedes
ms.openlocfilehash: eff08cc17f475e2b6ad6406e463de27371bbe5b1
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2019
ms.locfileid: "54064738"
---
# <a name="tutorial-azure-active-directory-integration-with-palo-alto-networks---captive-portal"></a>Öğretici: Palo Alto Networks - Captive portalı ile Azure Active Directory Tümleştirme

Bu öğreticide, Palo Alto Networks - Captive portalı Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.
Palo Alto Networks tarafından sağlanan-Captive portalı Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Palo Alto Networks - Captive Portal erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak Palo Alto Networks - Captive portalıyla Azure AD hesaplarına (çoklu oturum açma) için oturum açmış, kullanıcılarınızın etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Palo Alto Networks - Captive portalı ile Azure AD tümleştirmesi yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik Palo Alto ağları - Captive portalı çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Palo Alto Networks tarafından sağlanan - Captive Portal destekler **IDP** tarafından başlatılan

* Palo Alto Networks tarafından sağlanan - Captive Portal destekler **zamanında** kullanıcı sağlama

## <a name="adding-palo-alto-networks---captive-portal-from-the-gallery"></a>Palo Alto Networks tarafından sağlanan - Captive portalından galeri ekleme

Palo Alto Networks - Azure AD'ye Captive portalı tümleştirmesini yapılandırmak için Galeri Captive portalından yönetilen SaaS uygulamaları listenize Palo Alto Networks - eklemeniz gerekir.

**Palo Alto Networks - galeri Captive portalından eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Palo Alto Networks - Captive portalı**seçin **Palo Alto Networks - Captive portalı** sonucu panelinden ardından **Ekle** uygulama eklemek için .

     ![Palo Alto Networks tarafından sağlanan-sonuç listesinde Captive portalı](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Palo Alto Networks ile test etme - Captive Portal tabanlı adlı bir test kullanıcı **Britta Simon**.
Çoklu oturum açma iş, bir Azure AD kullanıcısının Palo Alto Networks - ilgili kullanıcı arasında bir bağlantı ilişki Captive portalı kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Palo Alto Networks ile-test etmek için Captive portalı, aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Palo Alto Networks tarafından sağlanan - Captive portalı çoklu oturum açmayı yapılandırma](#configure-palo-alto-networks---captive-portal-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Palo Alto Networks tarafından sağlanan-Captive portalı test kullanıcısı oluşturma](#create-palo-alto-networks---captive-portal-test-user)**  - Palo Alto Networks - kullanıcı Azure AD gösterimini bağlı Captive portalı Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Palo Alto Networks - Captive portalı ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Palo Alto Networks - Captive portalı** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Palo Alto ağları - Captive portalı etki alanı ve URL'ler tek bilgi'oturum açma](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<Customer Firewall Hostname>/SAML20/SP`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<Customer Firewall Hostname>/SAML20/SP/ACS`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. İlgili kişi [Palo Alto Networks - Captive portalı istemcisi Destek ekibine](https://support.paloaltonetworks.com/support) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

### <a name="configure-palo-alto-networks---captive-portal-single-sign-on"></a>Palo Alto Networks tarafından sağlanan - Captive Portal çoklu oturum açmayı yapılandırın

1. Palo Alto site başka bir tarayıcı penceresinde bir yönetici olarak açın.

2. Tıklayarak **cihaz**.

    ![Palo Alto çoklu oturum açmayı yapılandırın](./media/paloaltonetworks-captiveportal-tutorial/tutorial_paloaltoadmin_admin1.png)

3. Seçin **SAML kimlik sağlayıcısı** sol gezinti bölmesinden çubuk ve meta veri dosyası içeri aktarmak için "Al" seçeneğini tıklatın.

    ![Palo Alto çoklu oturum açmayı yapılandırın](./media/paloaltonetworks-captiveportal-tutorial/tutorial_paloaltoadmin_admin2.png)

4. Aşağıdaki içeri aktarma penceresini Eylemler gerçekleştirin

    ![Palo Alto çoklu oturum açmayı yapılandırın](./media/paloaltonetworks-captiveportal-tutorial/tutorial_paloaltoadmin_admin3.png)

    a. İçinde **profil adı** metin kutusuna bir ad ör. Azure AD Yöneticisi kullanıcı Arabirimi sağlar.
    
    b. İçinde **kimlik sağlayıcısı meta verileri**, tıklayın **Gözat** ve Azure portalından indirdiğiniz metadata.xml dosyası seçin
    
    c. **Tamam**’a tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma Palo Alto Networks - Captive Portal erişim vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Palo Alto Networks - Captive portalı**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde yazın ve **Palo Alto Networks - Captive portalı**.

    ![Palo Alto ağları - uygulamalar listesinde Captive Portal bağlantısı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-palo-alto-networks---captive-portal-test-user"></a>Palo Alto Networks tarafından sağlanan-Captive portalı test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı Palo Alto Networks - Captive portalında oluşturulur. Palo Alto Networks tarafından sağlanan - Captive Portal destekler **just-ın-time kullanıcı sağlamayı**, varsayılan olarak etkindir. Bu bölümde, hiçbir eylem öğesini yoktur. Palo Alto Networks - Captive Portal, bir kullanıcı zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur.

> [!NOTE]
> Bir kullanıcı el ile oluşturmanız gerekiyorsa, iletişime geçmeniz [Palo Alto Networks - Captive portalı istemcisi Destek ekibine](https://support.paloaltonetworks.com/support).

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Windows VM Güvenlik duvarının arkasında captive portalında yapılandırılır. Captive portalında, RDP kullanarak Windows VM'de oturum açma çoklu oturum açmayı test etmek için. Gelen RDP oturumu içinde herhangi bir web sitesi için bir tarayıcı açın, otomatik olarak SSO URL'yi ve kimlik doğrulama istemi açmanız gerekir. Challenge tamamlandıktan sonra web sitelerine navgiate yapabileceksiniz.

## <a name="additional-resources"></a>Ek Kaynaklar

- [ SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

