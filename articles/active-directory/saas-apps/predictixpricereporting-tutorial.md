---
title: 'Öğretici: Fiyat Predictix Raporlama ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Bu öğreticide, Azure Active Directory ve fiyat Predictix raporlama arasında çoklu oturum açmayı yapılandırma öğreneceksiniz.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 691d0c43-3aa1-4220-9e46-e7a88db234ad
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/26/2019
ms.author: jeedes
ms.openlocfilehash: 808b2d964bb39af6b410a84563717102ebece454
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67094102"
---
# <a name="tutorial-azure-active-directory-integration-with-predictix-price-reporting"></a>Öğretici: Fiyat Predictix Raporlama ile Azure Active Directory Tümleştirmesi

Bu öğreticide, Azure Active Directory (Azure AD) ile tümleştirme Predictix Fiyat Raporlama öğreneceksiniz.

Bu tümleştirme aşağıdaki avantajları sağlar:

* Fiyat Predictix raporlama erişimi denetlemek için Azure AD kullanabilirsiniz.
* Kullanıcılarınızın Predictix fiyat raporlama için (çoklu oturum açma) ile Azure AD hesaplarına otomatik olarak imzalanmasını etkinleştirebilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilir: Azure portalı.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [Azure Active Directory'de uygulamalar için çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

Azure aboneliğiniz yoksa, [ücretsiz bir hesap oluşturma](https://azure.microsoft.com/free/) başlamadan önce.

## <a name="prerequisites"></a>Önkoşullar

Fiyat Predictix Raporlama ile Azure AD tümleştirmesini yapılandırmak için ihtiyacınız vardır:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, oturum açabileceğiniz bir [bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/) abonelik.
* Tekli etkin oturum sahip bir fiyat Predictix raporlama bir abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Fiyat Predictix raporlama SP tarafından başlatılan SSO'yu destekler.

## <a name="adding-predictix-price-reporting-from-the-gallery"></a>Fiyat Predictix raporlama galeri ekleme

Azure AD'de Predictix Fiyat Raporlama tümleştirmeyi ayarlamak için fiyat Predictix raporlama Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory**:

    ![Azure Active Directory'yi seçin](common/select-azuread.png)

2. Git **kurumsal uygulamalar** > **tüm uygulamaları**:

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Bir uygulama eklemek için seçin **yeni uygulama** pencerenin üst kısmındaki:

    ![Yeni uygulama seçme](common/add-new-app.png)

4. Arama kutusuna **fiyat Predictix raporlama**. Seçin **fiyat Predictix raporlama** seçin ve arama sonuçlarını **Ekle**.

     ![Arama sonuçları](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma fiyat Predictix Raporlama ile Britta Simon adlı bir test kullanıcısı kullanarak test edin.
Çoklu oturum açmayı etkinleştirmek için bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir ilişki fiyat Predictix raporlamada yapmanız gerekir.

Yapılandırma ve Azure AD çoklu oturum açma fiyat Predictix Raporlama ile test etmek için bu adımları tamamlamak gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on)**  kullanıcılarınız için özelliği etkinleştirmek için.
2. **[Fiyat Predictix raporlama çoklu oturum açmayı yapılandırma](#configure-predictix-price-reporting-single-sign-on)**  uygulama tarafında.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açmayı test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  Azure AD çoklu oturum açma için kullanıcı etkinleştirmek için.
5. **[Fiyat Predictix raporlama test kullanıcısı oluşturma](#create-a-predictix-price-reporting-test-user)**  kullanıcı Azure AD gösterimini bağlı.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  yapılandırma çalıştığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalındaki etkinleştirmeniz.

Fiyat Predictix Raporlama ile Azure AD çoklu oturum açmayı yapılandırmak için şu adımları uygulayın:

1. İçinde [Azure portalında](https://portal.azure.com/), **fiyat Predictix raporlama** uygulama tümleştirme sayfasında **çoklu oturum açma**:

    ![Çoklu oturum açma seçin](common/select-sso.png)

2. İçinde **tek bir oturum açma yönteminizi seçmeniz** Seç iletişim kutusunda **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için:

    ![Tek bir oturum açma yöntemi seçin](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim kutusunda:

    ![Düzenle simgesi](common/edit-urls.png)

4. İçinde **temel SAML yapılandırma** iletişim kutusunda, aşağıdaki adımları tamamlayın.

    ![Temel SAML yapılandırma iletişim kutusu](common/sp-identifier.png)

    1. İçinde **oturum açma URL'si** kutusuna, bu düzende bir URL girin:

       `https://<companyname-pricing>.predictix.com/sso/request`

    1. İçinde **tanımlayıcı (varlık kimliği)** kutusuna, bu düzende bir URL girin:

        | |
        |--|
        | `https://<companyname-pricing>.predictix.com` |
        | `https://<companyname-pricing>.dev.predictix.com` |
        | |

    > [!NOTE]
    > Bu değerler yer tutuculardır. Gerçek oturum açma URL'si ve tanımlayıcı kullanmanız gerekir. Kişi [fiyat Predictix raporlama Destek ekibine](https://www.infor.com/company/customer-center/) değerleri alabilirsiniz. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** Azure portalında iletişim kutusu.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünden **indirme** yanındaki bağlantı **sertifika (Base64)** , gereksinimlerinize göre ve bilgisayarınızdaki sertifika Kaydet:

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. İçinde **Set up Predictix fiyat Reporting** bölümünde, gereksinimlerinize göre uygun URL'ler kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    1. **Oturum açma URL'si**.

    1. **Azure AD tanımlayıcısı**.

    1. **Oturum kapatma URL'si**.

### <a name="configure-predictix-price-reporting-single-sign-on"></a>Fiyat Predictix raporlama çoklu oturum açmayı yapılandırın

Çoklu oturum açma fiyat Predictix raporlama tarafta yapılandırmak için indirdiğiniz sertifika ve Azure için portaldan kopyaladığınız URL'leri Gönder gereken [fiyat Predictix raporlama Destek ekibine](https://www.infor.com/company/customer-center/). SAML SSO bağlantının her iki kenarı da düzgün ayarlandığından bu takım sağlar.

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

Bu bölümde, fiyat Predictix raporlama için erişim vererek, Azure AD çoklu oturum açma kullanılacak Britta Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**ve ardından **fiyat Predictix raporlama**.

    ![Kurumsal uygulamalar](common/enterprise-applications.png)

2. Uygulamalar listesinde seçin **fiyat Predictix raporlama**.

    ![Uygulamaların listesi](common/all-applications.png)

3. Sol bölmede seçin **kullanıcılar ve gruplar**:

    ![Kullanıcıları ve grupları seçin](common/users-groups-blade.png)

4. Seçin **Kullanıcı Ekle**ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim kutusu.

    ![Kullanıcı Ekle seçeneğini belirleme](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıların listesini ve ardından **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması rol değeri de beklediğiniz **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin. Tıklayın **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim kutusunda **atama**.

### <a name="create-a-predictix-price-reporting-test-user"></a>Fiyat Predictix raporlama test kullanıcısı oluşturma

Sonra Britta Simon fiyat Predictix Raporlama'da adlı bir kullanıcı oluşturmanız gerekir. Çalışmak [fiyat Predictix raporlama Destek ekibine](https://www.infor.com/company/customer-center/) kullanıcıları eklemek için. Kullanıcıların oluşturulabilir ve çoklu oturum açma kullanan önce etkinleştirmeniz gerekir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Şimdi Azure AD çoklu oturum açma yapılandırmanızı erişim panelini kullanarak test gerekir.

Erişim Paneli'nde Predictix Fiyat Raporlama kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama Predictix Fiyat Raporlama örneği için oturum açmanız. Daha fazla bilgi için [erişim ve kullanım uygulamaları uygulamalarım portalında](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)