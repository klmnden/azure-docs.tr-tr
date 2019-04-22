---
title: 'Öğretici: Veritas Kurumsal Vault.cloud SSO ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Veritas Kurumsal Vault.cloud SSO ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: c47894b1-f5df-4755-845d-f12f4c602dc4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: jeedes
ms.openlocfilehash: 59012bf32a4e1f0532b4d42e510d431180c35730
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59264191"
---
# <a name="tutorial-azure-active-directory-integration-with-veritas-enterprise-vaultcloud-sso"></a>Öğretici: Veritas Kurumsal Vault.cloud SSO ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Veritas Kurumsal Vault.cloud SSO tümleştirme konusunda bilgi edinin.
Veritas Kurumsal Vault.cloud SSO, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Veritas Kurumsal Vault.cloud SSO erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak Veritas Kurumsal Vault.cloud SSO (çoklu oturum açma) oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Veritas Kurumsal Vault.cloud SSO ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Veritas Kurumsal Vault.cloud SSO çoklu oturum açma abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Veritas Kurumsal Vault.cloud SSO'yu destekler **SP** tarafından başlatılan

## <a name="adding-veritas-enterprise-vaultcloud-sso-from-the-gallery"></a>Galeriden Veritas Kurumsal Vault.cloud SSO ekleme

Azure AD'de Veritas Kurumsal Vault.cloud SSO tümleştirmesini yapılandırmak için Veritas Kurumsal Vault.cloud SSO Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Veritas Kurumsal Vault.cloud SSO eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Veritas Kurumsal Vault.cloud SSO**seçin **Veritas Kurumsal Vault.cloud SSO** sonucu panelinden ardından **Ekle** düğme eklemek için uygulama.

     ![Sonuç listesinde Veritas Kurumsal Vault.cloud SSO](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Veritas Kurumsal Vault.cloud adlı bir test kullanıcı tabanlı SSO ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının ve kurumsal Vault.cloud SSO Veritas ilgili kullanıcı bağlantısına ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Veritas Kurumsal Vault.cloud SSO ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Veritas Kurumsal Vault.cloud SSO çoklu oturum açmayı yapılandırma](#configure-veritas-enterprise-vaultcloud-sso-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Veritas Kurumsal Vault.cloud SSO test kullanıcısı oluşturma](#create-veritas-enterprise-vaultcloud-sso-test-user)**  - kullanıcı Azure AD gösterimini bağlı Veritas Kurumsal Vault.cloud SSO Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Veritas Kurumsal Vault.cloud SSO ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Veritas Kurumsal Vault.cloud SSO** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Veritas Kurumsal Vault.cloud SSO etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://personal.ap.archive.veritas.com/CID=<CUSTOMERID>`

    b. İçinde **tanımlayıcı** kutusunda, veri merkezi göre URL'yi kullanın:

    | Veri merkezi| URL'si |
    |----------|----|
    | Kuzey Amerika| `https://auth.lax.archivecloud.net` |
    | Avrupa | `https://auth.ams.archivecloud.net` |
    | Asya Pasifik| `https://auth.syd.archivecloud.net`|

    c. İçinde **yanıt URL'si** metin kutusunda, veri merkezi göre URL'yi kullanın:

    | Veri merkezi| URL'si |
    |----------|----|
    | Kuzey Amerika| `https://auth.lax.archivecloud.net` |
    | Avrupa | `https://auth.ams.archivecloud.net` |
    | Asya Pasifik| `https://auth.syd.archivecloud.net`|

    > [!NOTE]
    > Bu değer, gerçek değil. Bu değer, gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Veritas Kurumsal Vault.cloud SSO istemci Destek ekibine](https://www.veritas.com/support/.html) bu değeri alınamıyor. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **Veritas Kurumsal Vault.cloud SSO'yu ayarlama** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-veritas-enterprise-vaultcloud-sso-single-sign-on"></a>Veritas Kurumsal Vault.cloud SSO çoklu oturum açmayı yapılandırın

Çoklu oturum açmayı yapılandırma **Veritas Kurumsal Vault.cloud SSO** tarafı, indirilen göndermek için ihtiyacınız **sertifika (Base64)** ve uygun Azure portalına kopyalanan URL'lerden [Veritas Kurumsal Vault.cloud SSO Destek ekibine](https://www.veritas.com/support/.html). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü brittasimon@yourcompanydomain.extension. Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Veritas Kurumsal Vault.cloud SSO için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Veritas Kurumsal Vault.cloud SSO**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Veritas Kurumsal Vault.cloud SSO**.

    ![Uygulamalar listesini Veritas Kurumsal Vault.cloud SSO bağlantıdaki](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-veritas-enterprise-vaultcloud-sso-test-user"></a>Veritas Kurumsal Vault.cloud SSO test kullanıcısı oluşturma

Bu bölümde, Veritas Kurumsal Vault.cloud SSO, Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [Veritas Kurumsal Vault.cloud SSO Destek ekibine](https://www.veritas.com/support/.html) Veritas Kurumsal Vault.cloud SSO platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Veritas Kurumsal Vault.cloud SSO kutucuğa tıkladığınızda, size otomatik olarak Veritas Kurumsal Vault.cloud SSO'yu ayarlama SSO için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

