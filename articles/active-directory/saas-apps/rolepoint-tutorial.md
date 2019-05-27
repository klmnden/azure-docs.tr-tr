---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile RolePoint | Microsoft Docs'
description: Bu öğreticide, Azure Active Directory ve RolePoint arasında çoklu oturum açmayı yapılandırma öğreneceksiniz.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 68d37f40-15da-45f5-a9e1-d53f78e786d1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/15/2019
ms.author: jeedes
ms.openlocfilehash: d2f854c869ead8016a5437a4e40339d9455c9ad7
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66226423"
---
# <a name="tutorial-azure-active-directory-integration-with-rolepoint"></a>Öğretici: RolePoint ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile RolePoint tümleştirme öğreneceksiniz.
Bu tümleştirme aşağıdaki avantajları sağlar:

* Azure AD RolePoint erişimi denetlemek için kullanabilirsiniz.
* Kullanıcılarınız için RolePoint (çoklu oturum açma) ile Azure AD hesaplarına otomatik olarak oturum açmanız etkinleştirebilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilir: Azure portalı.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [Azure Active Directory'de uygulamalar için çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile RolePoint yapılandırmak için sahip olmanız gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).
* Çoklu oturum etkin açma RolePoint abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* RolePoint SP tarafından başlatılan SSO'yu destekler.

## <a name="add-rolepoint-from-the-gallery"></a>Galeriden RolePoint Ekle

Azure AD'de RolePoint tümleştirmesini ayarlamak için yönetilen SaaS uygulamaları listenize Galeriden RolePoint eklemeniz gerekir.

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory**:

    ![Azure Active Directory'yi seçin](common/select-azuread.png)

2. Git **kurumsal uygulamalar** > **tüm uygulamaları**:

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Bir uygulama eklemek için seçin **yeni uygulama** pencerenin üst kısmındaki:

    ![Yeni uygulama seçme](common/add-new-app.png)

4. Arama kutusuna **RolePoint**. Seçin **RolePoint** seçin ve arama sonuçlarını **Ekle**.

     ![Arama sonuçları](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma ile RolePoint Britta Simon adlı bir test kullanıcısı kullanarak test edin.
Çoklu oturum açmayı etkinleştirmek için RolePoint içinde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir ilişki yapmanız gerekir.

Yapılandırma ve Azure AD çoklu oturum açma RolePoint ile test etmek için bu adımları tamamlamak gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on)**  kullanıcılarınız için özelliği etkinleştirmek için.
2. **[RolePoint çoklu oturum açmayı yapılandırma](#configure-rolepoint-single-sign-on)**  uygulama tarafında.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açmayı test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  Azure AD çoklu oturum açma için kullanıcı etkinleştirmek için.
5. **[RolePoint test kullanıcısı oluşturma](#create-a-rolepoint-test-user)**  kullanıcı Azure AD gösterimini bağlı.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  yapılandırma çalıştığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalındaki etkinleştirmeniz.

Azure AD çoklu oturum açma ile RolePoint yapılandırmak için şu adımları uygulayın:

1. İçinde [Azure portalında](https://portal.azure.com/), RolePoint uygulama tümleştirme sayfasında **çoklu oturum açma**:

    ![Çoklu oturum açma seçin](common/select-sso.png)

2. İçinde **tek bir oturum açma yönteminizi seçmeniz** Seç iletişim kutusunda **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için:

    ![Çoklu oturum açma yöntemi seçin](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim kutusunda:

    ![Düzenle simgesi](common/edit-urls.png)

4. İçinde **temel SAML yapılandırma** iletişim kutusunda, aşağıdaki adımları uygulayın.

    ![Temel SAML yapılandırma iletişim kutusu](common/sp-identifier.png)

    1. İçinde **oturum açma URL'si** kutusuna, bu düzende bir URL girin:

       `https://<subdomain>.rolepoint.com/login`

    1. İçinde **tanımlayıcı (varlık kimliği)** kutusuna, bu düzende bir URL girin:

       `https://app.rolepoint.com/<instancename>`

    > [!NOTE]
    > Bu değerler yer tutuculardır. Gerçek oturum açma URL'si ve tanımlayıcı kullanmanız gerekir. Benzersiz bir dize değeri tanımlayıcıda kullanmanızı öneririz. İlgili kişi [RolePoint Destek ekibine](mailto:info@rolepoint.com) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** Azure portalında iletişim kutusu.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünden **indirme** yanındaki bağlantı **Federasyon meta veri XML** , gereksinimlerinize göre ve bilgisayarınızda dosyayı kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. İçinde **RolePoint kümesi** bölümünde, gereksinimlerinize göre uygun URL'ler kopyalayın:

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    1. **Oturum açma URL'si**.

    1. **Azure AD tanımlayıcısı**.

    1. **Oturum kapatma URL'si**.


### <a name="configure-rolepoint-single-sign-on"></a>RolePoint çoklu oturum açmayı yapılandırın

Çalışmak istediğiniz RolePoint tarafında çoklu oturum açmayı ayarlamak için [RolePoint Destek ekibine](mailto:info@rolepoint.com). Bu ekip, Federasyon meta veri XML dosyasında ve Azure portalından aldığınız URL'leri gönderin. Bunlar, SAML SSO bağlantının her iki kenarı da düzgün ayarlandığından emin olmak için RolePoint yapılandıracaksınız.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında Britta Simon adlı bir test kullanıcısı oluşturacaksınız.

1. Azure portalında **Azure Active Directory** seçin sol bölmede **kullanıcılar**ve ardından **tüm kullanıcılar**:

    ![Tüm kullanıcıları seçin](common/users.png)

2. Seçin **yeni kullanıcı** pencerenin üst kısmındaki:

    ![Yeni bir kullanıcı seçin](common/new-user.png)

3. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları uygulayın.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    1. İçinde **adı** kutusuna **BrittaSimon**.
  
    1. İçinde **kullanıcı adı** kutusuna **@ BrittaSimon\<yourcompanydomain >.\< Uzantı >** . (Örneğin, BrittaSimon@contoso.com.)

    1. Seçin **Göster parola**ve ardından içinde bir değer yazın **parola** kutusu.

    1. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için RolePoint erişim vererek Britta Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**ve ardından **RolePoint**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde seçin **RolePoint**.

    ![Uygulama listesi](common/all-applications.png)

3. Sol bölmede seçin **kullanıcılar ve gruplar**:

    ![Kullanıcıları ve grupları seçin](common/users-groups-blade.png)

4. Seçin **Kullanıcı Ekle**ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim kutusu.

    ![Kullanıcı Ekle seçeneğini belirleme](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıların listesini ve ardından **seçin** pencerenin alt kısmındaki düğmesi.

6. SAML onaylaması rol değeri de beklediğiniz **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin. Tıklayın **seçin** pencerenin alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim kutusunda **atama**.

### <a name="create-a-rolepoint-test-user"></a>RolePoint test kullanıcısı oluşturma

Sonra Britta Simon RolePoint içinde adlı bir kullanıcı oluşturmanız gerekir. Çalışmak [RolePoint Destek ekibine](mailto:info@rolepoint.com) RolePoint için kullanıcıları eklemek için. Kullanıcıların oluşturulabilir ve çoklu oturum açma kullanabilmek için önce etkinleştirmeniz gerekir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Şimdi Azure AD çoklu oturum açma yapılandırmanızı erişim panelini kullanarak test gerekir.

Erişim Paneli'nde RolePoint kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama RolePoint örneği için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim ve kullanım uygulamaları uygulamalarım portalında](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
