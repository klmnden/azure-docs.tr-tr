---
title: 'Öğretici: Azure Active Directory Tümleştirmesi, kampüs sonsuz ile | Microsoft Docs'
description: Azure Active Directory ve sonsuz kampüs arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3995b544-e751-4e0f-ab8b-c9a3862da6ba
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: jeedes
ms.openlocfilehash: 75cc92e420b08307377ab85a2d8d121303429ce5
ms.sourcegitcommit: 41015688dc94593fd9662a7f0ba0e72f044915d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59500048"
---
# <a name="tutorial-azure-active-directory-integration-with-infinite-campus"></a>Öğretici: Sonsuz kampüs ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile sonsuz kampüs tümleştirme konusunda bilgi edinin.
Sonsuz kampüs Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Sonsuz kampüs erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için sonsuz kampüs oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile sonsuz kampüs yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Abonelik sonsuz kampüs çoklu oturum açma etkin
* En azından, Azure Active Directory yönetici olmanız ve kampüs ürün güvenlik rolü, "Öğrenci bilgi sistemi (yapılandırmasını tamamlamak için SIS)" olması gerekir.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Sonsuz kampüs destekler **SP** tarafından başlatılan

## <a name="adding-infinite-campus-from-the-gallery"></a>Galeriden sonsuz kampüs ekleme

Azure AD'de sonsuz kampüs tümleştirmesini yapılandırmak için sonsuz kampüs Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden sonsuz kampüs eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için tıklatın **yeni uygulama** iletişim kutusunun üstündeki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **sonsuz kampüs**seçin **sonsuz kampüs** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde sonsuz kampüs](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma sonsuz adlı bir test kullanıcı tabanlı kampüs test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve sonsuz kampüs ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma sonsuz kampüs ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Sonsuz kampüs çoklu oturum açmayı yapılandırma](#configure-infinite-campus-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Sonsuz kampüs test kullanıcısı oluşturma](#create-infinite-campus-test-user)**  - sonsuz kullanıcı Azure AD gösterimini bağlı olduğu kampüs Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile sonsuz kampüs yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **sonsuz kampüs** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Temel bir SAML yapılandırma bölümünde aşağıdaki adımları uygulayın (etki alanı barındırma modeli ile değişir unutmayın ancak **tamamen-tam etki alanı** değeri sonsuz kampüs yüklemenizi eşleşmesi gerekir):

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<DOMAIN>.infinitecampus.com/campus/SSO/<DISTRICTNAME>/SIS`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<DOMAIN>.infinitecampus.com/campus/<DISTRICTNAME>`

    c. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<DOMAIN>.infinitecampus.com/campus/SSO/<DISTRICTNAME>`

    ![Çoklu oturum açma bilgileri sonsuz kampüs etki alanı ve URL'ler](common/sp-identifier-reply.png)

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** ve üzerinde kaydedin, bilgisayar.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

### <a name="configure-infinite-campus-single-sign-on"></a>Sonsuz kampüs çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde sonsuz kampüs için bir güvenlik yöneticisi olarak oturum açın.

2. Menü sol tarafında tıklayın **Sistem Yönetimi**.

    ![Yönetici](./media/infinitecampus-tutorial/tutorial_infinitecampus_admin.png)

3. Gidin **kullanıcı güvenlik** > **SAML Yönetim** > **SSO Servis Sağlayıcı Yapılandırması**.

    ![Saml](./media/infinitecampus-tutorial/tutorial_infinitecampus_saml.png)

4. Üzerinde **SSO Servis Sağlayıcı Yapılandırması** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Sso](./media/infinitecampus-tutorial/tutorial_infinitecampus_sso.png)

    a. Seçin **etkinleştir SAML çoklu oturum açma**.

    b. Düzen **isteğe bağlı öznitelik adı** içerecek şekilde **adı**

    c. Üzerinde **kimlik sağlayıcısı (IDP) sunucu verilerini almak için bir seçenek belirleyin** bölümünde, seçin **meta veri URL'si**, Yapıştır **uygulama Federasyon meta verileri URL'sini** sahip olduğunuz değeri Azure portalından kutusunda kopyalanır ve ardından **eşitleme**.

    d. ' I tıklattıktan sonra **eşitleme** içinde otomatik olarak doldurulan değerleri almak **SSO Servis Sağlayıcı Yapılandırması** sayfası. Bu değerler, adım 4'te görüldüğü değerleriyle eşleşecek şekilde doğrulanabilir.

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
  
    b. İçinde **kullanıcı adı** alan türü `brittasimon@yourcompanydomain.extension`. Örneğin, BrittaSimon@contoso.com.

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

> [!NOTE]
> Tüm istiyorsanız Azure kullanıcılarınız için çoklu oturum açma erişmek için sonsuz kampüs ve erişimi denetlemek için sonsuz kampüs iç izinleri sistemine, ayarlayabileceğiniz **kullanıcı ataması gerekli** Hayır uygulama özelliği ve aşağıdaki adımları atlayın.

Bu bölümde, Azure çoklu oturum açma kullanmak için sonsuz kampüs erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **sonsuz kampüs**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **sonsuz kampüs**.

    ![Uygulamalar listesinde sonsuz kampüs bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-infinite-campus-test-user"></a>Sonsuz kampüs test kullanıcısı oluşturma

Sonsuz kampüs, demografik bilgileri ortalanmış bir mimariye sahiptir. Lütfen başvurun [sonsuz kampüs Destek ekibine](mailto:sales@infinitecampus.com) sonsuz kampüs platform kullanıcıları eklemek için.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli sonsuz kampüs kutucuğa tıkladığınızda, size otomatik olarak sonsuz SSO'yu ayarlama kampüs için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [ SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
