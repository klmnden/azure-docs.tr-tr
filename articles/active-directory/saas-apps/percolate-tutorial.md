---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Percolate | Microsoft Docs'
description: Bu öğreticide, Azure Active Directory ve Percolate arasında çoklu oturum açmayı yapılandırma öğreneceksiniz.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 355f9659-b378-44c9-aa88-236e9b529a53
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/01/2019
ms.author: jeedes
ms.openlocfilehash: a6c1f893757baf1e6c85420b31997a5073cff684
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67094608"
---
# <a name="tutorial-azure-active-directory-integration-with-percolate"></a>Öğretici: Percolate ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Percolate tümleştirme öğreneceksiniz.

Bu tümleştirme aşağıdaki avantajları sağlar:

* Azure AD Percolate erişimi denetlemek için kullanabilirsiniz.
* Otomatik olarak (çoklu oturum açma) ile Azure AD hesaplarına Percolate için oturum açmanız, kullanıcılarınızın etkinleştirebilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilir: Azure portalı.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [Azure Active Directory'de uygulamalar için çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

Azure aboneliğiniz yoksa, [ücretsiz bir hesap oluşturma](https://azure.microsoft.com/free/) başlamadan önce.

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Percolate yapılandırmak için sahip olmanız gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).
* Tekli etkin oturum sahip Percolate abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Percolate SP tarafından başlatılan ve IDP tarafından başlatılan SSO'yu destekler.

## <a name="add-percolate-from-the-gallery"></a>Galeriden Percolate Ekle

Azure AD'de Percolate tümleştirmesini yapılandırmak için Percolate Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory**:

    ![Azure Active Directory'yi seçin](common/select-azuread.png)

2. Git **kurumsal uygulamalar** > **tüm uygulamaları**:

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Bir uygulama eklemek için seçin **yeni uygulama** pencerenin üst kısmındaki:

    ![Yeni uygulama seçme](common/add-new-app.png)

4. Arama kutusuna **Percolate**. Seçin **Percolate** seçin ve arama sonuçlarını **Ekle**.

     ![Arama sonuçları](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma ile Percolate Britta Simon adlı bir test kullanıcısı kullanarak test edin.
Çoklu oturum açmayı etkinleştirmek için Percolate içinde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir ilişki yapmanız gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Percolate ile test etmek için bu adımları tamamlamak gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on)**  kullanıcılarınız için özelliği etkinleştirmek için.
2. **[Percolate çoklu oturum açmayı yapılandırma](#configure-percolate-single-sign-on)**  uygulama tarafında.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açmayı test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  Azure AD çoklu oturum açma için kullanıcı etkinleştirmek için.
5. **[Percolate test kullanıcısı oluşturma](#create-a-percolate-test-user)**  kullanıcı Azure AD gösterimini bağlı.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  yapılandırma çalıştığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalındaki etkinleştirmeniz.

Azure AD çoklu oturum açma ile Percolate yapılandırmak için şu adımları uygulayın:

1. İçinde [Azure portalında](https://portal.azure.com/), **Percolate** uygulama tümleştirme sayfasında **çoklu oturum açma**:

    ![Çoklu oturum açma seçin](common/select-sso.png)

2. İçinde **tek bir oturum açma yönteminizi seçmeniz** Seç iletişim kutusunda **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için:

    ![Tek bir oturum açma yöntemi seçin](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim kutusunda:

    ![Düzenle simgesi](common/edit-urls.png)

4. İçinde **temel SAML yapılandırma** iletişim kutusu, IDP tarafından başlatılan modunda uygulamayı yapılandırmak için herhangi bir eylemde bulunmanız gerekmez. Uygulama zaten Azure ile tümleşiktir.

    ![Etki alanı ve URL'ler tek oturum açma bilgileri percolate](common/preintegrated.png)

5. SP tarafından başlatılan modunda uygulama yapılandırmak isteyip istemediğinizi seçin **ek URL'lerini ayarlayın** hem de **oturum açma URL'si** kutusuna **https://percolate.com/app/login** :

   ![Etki alanı ve URL'ler tek oturum açma bilgileri percolate](common/metadata-upload-additional-signon.png)
6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünden **kopyalama** kopyalamak için simge **uygulama Federasyon meta veri URL'si** . Bu URL'yi kaydedin.

    ![Uygulama Federasyon meta verileri URL'sini Kopyala](common/copy-metadataurl.png)

7. İçinde **Percolate kümesi** bölümünde, gereksinimlerinize göre uygun URL'ler kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    1. **Oturum açma URL'si**.

    1. **Azure AD tanımlayıcısı**.

    1. **Oturum kapatma URL'si**.

### <a name="configure-percolate-single-sign-on"></a>Percolate çoklu oturum açmayı yapılandırın

1. Yeni bir web tarayıcısı penceresinde Percolate için bir yönetici olarak oturum açın

2. Giriş sayfasının sol taraftan **ayarları**:
    
    ![Ayarları seçin](./media/percolate-tutorial/configure01.png)

3. Sol bölmede seçin **SSO** altında **kuruluş**:

    ![Kuruluş altında SSO seçin](./media/percolate-tutorial/configure02.png)

    1. İçinde **oturum açma URL'si** kutusu, yapıştırma **oturum açma URL'si** Azure portaldan kopyaladığınız değeri.

    1. İçinde **varlık kimliği** kutusu, yapıştırma **Azure AD tanımlayıcısı** Azure portaldan kopyaladığınız değeri.

    1. Azure portalından indirdiğiniz base-64 kodlanmış sertifika Not Defteri'nde açın. İçeriği kopyalayın ve yapıştırın **x509 sertifikaları** kutusu.

    1. İçinde **e-posta özniteliği** kutusuna **emailaddress**.

    1. **Kimlik sağlayıcısı meta veri URL'si** kutusuna isteğe bağlı bir alandır. Kopyaladığınız varsa bir **uygulama Federasyon meta verileri URL'sini** Azure portalından, onları bu kutusuna yapıştırabilirsiniz.

    1. İçinde **gereken AuthNRequests imzalanmasını?** listesinden **Hayır**.

    1. İçinde **SSO etkinleştirme otomatik sağlama** listesinden **Hayır**.

    1. **Kaydet**’i seçin.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında Britta Simon adlı bir test kullanıcısı oluşturacaksınız.

1. Azure portalında **Azure Active Directory** seçin sol bölmede **kullanıcılar**ve ardından **tüm kullanıcılar**:

    ![Tüm kullanıcıları seçin](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üst kısmındaki:

    ![Yeni bir kullanıcı seçin](common/new-user.png)

3. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları uygulayın.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    1. İçinde **adı** kutusuna **BrittaSimon**.
  
    1. İçinde **kullanıcı adı** kutusuna **@ BrittaSimon\<yourcompanydomain >.\< Uzantı >** . (Örneğin, BrittaSimon@contoso.com.)

    1. Seçin **Göster parola**ve ardından içinde bir değer yazın **parola** kutusu.

    1. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure AD çoklu oturum açma kullanmak için Percolate erişim vererek Britta Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**ve ardından **Percolate**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde seçin **Percolate**.

    ![Uygulamaların listesi](common/all-applications.png)

3. Sol bölmede seçin **kullanıcılar ve gruplar**:

    ![Kullanıcıları ve grupları seçin](common/users-groups-blade.png)

4. Seçin **Kullanıcı Ekle**ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim kutusu.

    ![Kullanıcıları ve grupları seçin](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıların listesini ve ardından **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması rol değeri de beklediğiniz **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin. Tıklayın **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim kutusunda **atama**.

### <a name="create-a-percolate-test-user"></a>Percolate test kullanıcısı oluşturma

Percolate için oturum açmak Azure AD kullanıcılarının sağlamak için Percolate eklemeniz gerekir. Bunları el ile eklemeniz gerekir.

Bir kullanıcı hesabı oluşturmak için şu adımları uygulayın:

1. Percolate bir yönetici olarak oturum açın

2. Sol bölmede seçin **kullanıcılar** altında **kuruluş**. Seçin **yeni kullanıcılar**:

    ![Yeni kullanıcıları seçin](./media/percolate-tutorial/configure03.png)

3. Üzerinde **kullanıcılar oluşturma** sayfasında, aşağıdaki adımları uygulayın.

    ![Kullanıcılar sayfası oluşturma](./media/percolate-tutorial/configure04.png)

    1. İçinde **e-posta** kutusuna, kullanıcının e-posta adresi girin. Örneğin, brittasimon@contoso.com.

    1. İçinde **tam adı** kutusuna, kullanıcının adını girin. Örneğin, **Brittasimon**.

    1. Seçin **kullanıcılar oluşturma**.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Şimdi Azure AD çoklu oturum açma yapılandırmanızı erişim panelini kullanarak test gerekir.

Erişim Paneli'nde Percolate kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama Percolate örneği için oturum açmanız. Daha fazla bilgi için [erişim ve kullanım uygulamaları uygulamalarım portalında](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)