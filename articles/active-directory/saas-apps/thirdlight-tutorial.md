---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile ThirdLight | Microsoft Docs'
description: Bu öğreticide, Azure Active Directory ve ThirdLight arasında çoklu oturum açmayı yapılandırma öğreneceksiniz.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 168aae9a-54ee-4c2b-ab12-650a2c62b901
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: jeedes
ms.openlocfilehash: 448d46cd21a63488c4f567d5555fe6406fc0fa73
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67089098"
---
# <a name="tutorial-azure-active-directory-integration-with-thirdlight"></a>Öğretici: ThirdLight ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile ThirdLight tümleştirme öğreneceksiniz. Bu tümleştirme aşağıdaki avantajları sağlar:

* Azure AD ThirdLight erişimi denetlemek için kullanabilirsiniz.
* Kullanıcılarınız için ThirdLight (çoklu oturum açma) ile Azure AD hesaplarına otomatik olarak oturum açmanız etkinleştirebilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilir: Azure portalı.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız bkz [Azure Active Directory'de uygulamalar için çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile ThirdLight yapılandırmak için sahip olmanız gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).
* Tekli etkin oturum sahip ThirdLight abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* ThirdLight SP tarafından başlatılan SSO'yu destekler.

## <a name="add-thirdlight-from-the-gallery"></a>Galeriden ThirdLight Ekle

Azure AD'de ThirdLight tümleştirmesini ayarlamak için yönetilen SaaS uygulamaları listenize Galeriden ThirdLight eklemeniz gerekir.

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory**:

    ![Azure Active Directory'yi seçin](common/select-azuread.png)

2. Git **kurumsal uygulamalar** > **tüm uygulamaları**:

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Bir uygulama eklemek için seçin **yeni uygulama** pencerenin üst kısmındaki:

    ![Yeni uygulama seçme](common/add-new-app.png)

4. Arama kutusuna **ThirdLight**. Seçin **ThirdLight** seçin ve arama sonuçlarını **Ekle**.

     ![Arama sonuçları](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma ile ThirdLight Britta Simon adlı bir test kullanıcısı kullanarak test edin.
Çoklu oturum açmayı etkinleştirmek için ThirdLight içinde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir ilişki yapmanız gerekir.

Yapılandırma ve Azure AD çoklu oturum açma ThirdLight ile test etmek için bu adımları tamamlamak gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on)**  kullanıcılarınız için özelliği etkinleştirmek için.
2. **[ThirdLight çoklu oturum açmayı yapılandırma](#configure-thirdlight-single-sign-on)**  uygulama tarafında.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açmayı test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  Azure AD çoklu oturum açma için kullanıcı etkinleştirmek için.
5. **[ThirdLight test kullanıcısı oluşturma](#create-a-thirdlight-test-user)**  kullanıcı Azure AD gösterimini bağlı.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  yapılandırma çalıştığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalındaki etkinleştirmeniz.

Azure AD çoklu oturum açma ile ThirdLight yapılandırmak için şu adımları uygulayın:

1. İçinde [Azure portalında](https://portal.azure.com/), ThirdLight uygulama tümleştirme sayfasında **çoklu oturum açma**:

    ![Çoklu oturum açma seçin](common/select-sso.png)

2. İçinde **tek bir oturum açma yönteminizi seçmeniz** Seç iletişim kutusunda **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için:

    ![Tek bir oturum açma yöntemi seçin](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim kutusunda:

    ![Düzenle simgesi](common/edit-urls.png)

4. İçinde **temel SAML yapılandırma** iletişim kutusunda, aşağıdaki adımları tamamlayın.

    ![Temel SAML yapılandırma iletişim kutusu](common/sp-identifier.png)

    1. İçinde **oturum açma URL'si** kutusuna, bu düzende bir URL girin:
    
          `https://<subdomain>.thirdlight.com/`

    1. İçinde **tanımlayıcı (varlık kimliği)** kutusuna, bu düzende bir URL girin:

       `https://<subdomain>.thirdlight.com/saml/sp`

       > [!NOTE]
       > Bu değerler yer tutuculardır. Gerçek oturum açma URL'si ve tanımlayıcı kullanmanız gerekir. İlgili kişi [ThirdLight Destek ekibine](https://www.thirdlight.com/support) değerleri alabilirsiniz. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** Azure portalında iletişim kutusu.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünden **indirme** yanındaki bağlantı **Federasyon meta veri XML** , gereksinimlerinize göre ve bilgisayarınızda dosyayı kaydedin:

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. İçinde **ThirdLight kümesi** bölümünde, gereksinimlerinize göre uygun URL'ler kopyalayın:

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    1. **Oturum açma URL'si**.

    1. **Azure AD tanımlayıcısı**.

    1. **Oturum kapatma URL'si**.

### <a name="configure-thirdlight-single-sign-on"></a>ThirdLight çoklu oturum açmayı yapılandırın

1. Yeni bir web tarayıcısı penceresinde ThirdLight şirketinizin sitesi için bir yönetici olarak oturum açın

1. Git **yapılandırma** > **Sistem Yönetimi** > **SAML2**:

    ![Sistem Yönetimi](./media/thirdlight-tutorial/ic805843.png "Sistem Yönetimi")

1. Olduğu saml2 tabanlı yapılandırma bölümünde aşağıdaki adımları uygulayın.
  
    ![SAML2 yapılandırma bölümü](./media/thirdlight-tutorial/ic805844.png "olduğu saml2 tabanlı yapılandırma bölümü")

    1. Seçin **olduğu saml2 tabanlı çoklu oturum açmayı etkinleştirme**.

    1. Altında **IDP meta veri kaynağı**seçin **yük IDP meta verileri XML**.

    1. Azure portalından önceki bölümde indirdiğiniz meta veri dosyasını açın. Dosyanın içeriğini kopyalayın ve yapıştırın **IDP meta veri XML** kutusu.

    1. Seçin **Kaydet SAML2 ayarları**.

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

Bu bölümde, Azure çoklu oturum açma kullanmak için ThirdLight erişim vererek Britta Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**ve ardından **ThirdLight**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde seçin **ThirdLight**.

    ![Uygulamaların listesi](common/all-applications.png)

3. Sol bölmede seçin **kullanıcılar ve gruplar**:

    ![Kullanıcıları ve grupları seçin](common/users-groups-blade.png)

4. Seçin **Kullanıcı Ekle**ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim kutusu.

    ![Kullanıcı Ekle seçeneğini belirleme](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıların listesini ve ardından **seçin** pencerenin alt kısmındaki düğmesi.

6. SAML onaylaması rol değeri de beklediğiniz **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin. Tıklayın **seçin** pencerenin alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim kutusunda **atama**.

### <a name="create-a-thirdlight-test-user"></a>ThirdLight test kullanıcısı oluşturma

ThirdLight için oturum açmak Azure AD kullanıcılarının sağlamak için ThirdLight eklemeniz gerekir. Bunları el ile eklemeniz gerekir.

Bir kullanıcı hesabı oluşturmak için şu adımları uygulayın:

1. ThirdLight şirketinizin sitesi için bir yönetici olarak oturum açın

1. Git **kullanıcılar** sekmesi.

1. Seçin **kullanıcılar ve gruplar**.

1. Seçin **yeni kullanıcı ekleme**.

1. Kullanıcı adını girin, sağlamak istediğiniz hesap adını veya açıklamasını ve e-posta adresi geçerli bir Azure ad. Hazır ya da yeni üyeleri grubu seçin.

1. **Oluştur**’u seçin.

> [!NOTE]
> Herhangi bir kullanıcı hesabı oluşturma aracı kullanabilir veya API Azure AD'ye kullanıcı hesapları sağlamak için ThirdLight tarafından sağlanan.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Şimdi Azure AD çoklu oturum açma yapılandırmanızı erişim panelini kullanarak test gerekir.

Erişim Paneli'nde ThirdLight kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama ThirdLight örneği için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim ve kullanım uygulamaları uygulamalarım portalında](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
