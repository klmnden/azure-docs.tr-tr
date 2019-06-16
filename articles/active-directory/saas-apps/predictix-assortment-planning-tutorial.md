---
title: 'Öğretici: Azure Active Directory Tümleştirmesi Predictix Sınıflama planlama | Microsoft Docs'
description: Bu öğreticide, Azure Active Directory ve Sınıflama Predictix planlama arasında çoklu oturum açmayı yapılandırma öğreneceksiniz.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 37e686ff-f8e5-40b1-9d7e-f64b076917b7
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: jeedes
ms.openlocfilehash: bc3ea2f6fddc233a69d96c0c885ab310ed1e77c2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67094162"
---
# <a name="tutorial-azure-active-directory-integration-with-predictix-assortment-planning"></a>Öğretici: Azure Active Directory Tümleştirmesi Predictix Sınıflama planlama

Bu öğreticide, Azure Active Directory (Azure AD) ile tümleştirme Predictix Sınıflama planlama öğreneceksiniz.
Bu tümleştirme aşağıdaki avantajları sağlar:

* Azure AD Predictix Sınıflama planlama erişimi denetlemek için kullanabilirsiniz.
* Kullanıcılarınızın Predictix Sınıflama planlama (çoklu oturum açma) ile Azure AD hesaplarına otomatik olarak imzalanmasını etkinleştirebilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilir: Azure portalı.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [Azure Active Directory'de uygulamalar için çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

Azure aboneliğiniz yoksa, [ücretsiz bir hesap oluşturma](https://azure.microsoft.com/free/) başlamadan önce.

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Predictix Sınıflama planlama ile yapılandırmak için sahip olmanız gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/).
* Tekli etkin oturum sahip Sınıflama Predictix planlama abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Predictix Sınıflama planlama SP tarafından başlatılan SSO'yu destekler.

## <a name="add-predictix-assortment-planning-from-the-gallery"></a>Galeriden Ekle Predictix Sınıflama planlama

Azure AD'de Sınıflama Predictix planlama tümleştirmeyi ayarlamak için Sınıflama Predictix planlama Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory**:

    ![Azure Active Directory'yi seçin](common/select-azuread.png)

2. Git **kurumsal uygulamalar** > **tüm uygulamaları**:

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Bir uygulama eklemek için seçin **yeni uygulama** pencerenin üst kısmındaki:

    ![Yeni uygulama seçme](common/add-new-app.png)

4. Arama kutusuna **Sınıflama Predictix planlama**. Seçin **Sınıflama Predictix planlama** seçin ve arama sonuçlarını **Ekle**.

     ![Arama sonuçları](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Sınıflama Predictix planlamayla Britta Simon adlı bir test kullanıcısı'ni kullanarak test.
Çoklu oturum açmayı etkinleştirmek için bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir ilişki Sınıflama Predictix planlaması yapmanız gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Sınıflama Predictix planlama ile test etmek için bu adımları tamamlamak gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on)**  kullanıcılarınız için özelliği etkinleştirmek için.
2. **[Predictix Sınıflama planlama çoklu oturum açmayı yapılandırma](#configure-predictix-assortment-planning-single-sign-on)**  uygulama tarafında.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açmayı test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  Azure AD çoklu oturum açma için kullanıcı etkinleştirmek için.
5. **[Predictix Sınıflama planlama test kullanıcısı oluşturma](#create-a-predictix-assortment-planning-test-user)**  kullanıcı Azure AD gösterimini bağlı.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  yapılandırma çalıştığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalındaki etkinleştirmeniz.

Azure AD çoklu oturum açma Predictix Sınıflama planlama ile yapılandırmak için şu adımları uygulayın:

1. İçinde [Azure portalında](https://portal.azure.com/), **Sınıflama Predictix planlama** uygulama tümleştirme sayfasında **çoklu oturum açma**:

    ![Çoklu oturum açma seçin](common/select-sso.png)

2. İçinde **tek bir oturum açma yönteminizi seçmeniz** Seç iletişim kutusunda **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için:

    ![Tek bir oturum açma yöntemi seçin](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim kutusunda:

    ![Düzenle simgesi](common/edit-urls.png)

4. İçinde **temel SAML yapılandırma** iletişim kutusunda, aşağıdaki adımları tamamlayın.

    ![Temel SAML yapılandırma iletişim kutusu](common/sp-identifier.png)

    1. İçinde **oturum açma URL'si** kutusuna, bu düzende bir URL girin:

       | |
        |--|
        | `https://<sub-domain>.ap.predictix.com/sso/request`|
        | `https://<sub-domain>.dev.ap.predictix.com/`|
        | |

    1. İçinde **tanımlayıcı (varlık kimliği)** kutusuna, bu düzende bir URL girin:

        | |
        |--|
        | `https://<sub-domain>.ap.predictix.com`|
        | `https://<sub-domain>.dev.ap.predictix.com`|
        | |

    > [!NOTE]
    > Bu değerler yer tutuculardır. Gerçek oturum açma URL'si ve tanımlayıcı kullanmanız gerekir. Kişi [Sınıflama Predictix planlama Destek ekibine](https://www.infor.com/support) değerleri alabilirsiniz. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** Azure portalında iletişim kutusu.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünden **indirme** yanındaki bağlantı **sertifika (Base64)** , gereksinimlerinize göre ve bilgisayarınızdaki sertifika Kaydet:

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. İçinde **Sınıflama Predictix planlama ayarlamak** bölümünde, gereksinimlerinize göre uygun URL'ler kopyalayın:

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    1. **Oturum açma URL'si**.

    1. **Azure AD tanımlayıcısı**.

    1. **Oturum kapatma URL'si**.

### <a name="configure-predictix-assortment-planning-single-sign-on"></a>Predictix Sınıflama planlama çoklu oturum açmayı yapılandırın

Çoklu oturum açma Sınıflama Predictix planlama tarafında yapılandırmak için indirdiğiniz sertifika ve Azure için portaldan kopyaladığınız URL'leri Gönder gerekir [Sınıflama Predictix planlama Destek ekibine](https://www.infor.com/support). SAML SSO bağlantının her iki kenarı da düzgün ayarlandığından bu takım sağlar.

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

    1. Seçin **Show parola**ve ardından içinde bir değer yazın **parola** kutusu.

    1. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Sınıflama Predictix planlama erişim vererek, Azure AD çoklu oturum açma kullanılacak Britta Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**ve ardından **Sınıflama Predictix planlama**.

    ![Kurumsal uygulamalar](common/enterprise-applications.png)

2. Uygulamalar listesinde seçin **Sınıflama Predictix planlama**.

    ![Uygulamaların listesi](common/all-applications.png)

3. Sol bölmede seçin **kullanıcılar ve gruplar**:

    ![Kullanıcıları ve grupları seçin](common/users-groups-blade.png)

4. Seçin **Kullanıcı Ekle**ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim kutusu.

    ![Kullanıcı Ekle seçeneğini belirleme](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıların listesini ve ardından **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması rol değeri de beklediğiniz **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin. Tıklayın **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim kutusunda **atama**.

### <a name="create-a-predictix-assortment-planning-test-user"></a>Predictix Sınıflama planlama test kullanıcısı oluşturma

Sonra Britta Simon Sınıflama Predictix planlama adlı bir kullanıcı oluşturmanız gerekir. Çalışmak [Sınıflama Predictix planlama Destek ekibine](https://www.infor.com/support) kullanıcıları eklemek için. Kullanıcıların oluşturulabilir ve çoklu oturum açma kullanan önce etkinleştirmeniz gerekir.

> [!NOTE]
> Azure AD hesap sahibinin e-posta alır ve etkin hale gelir önce hesabı onaylamak için bir bağlantı seçer.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Şimdi Azure AD çoklu oturum açma yapılandırmanızı erişim panelini kullanarak test gerekir.

Erişim Paneli'nde Predictix Sınıflama planlama kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama Predictix Sınıflama planlama örneğine oturum açmanız. Daha fazla bilgi için [erişim ve kullanım uygulamaları uygulamalarım portalında](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)