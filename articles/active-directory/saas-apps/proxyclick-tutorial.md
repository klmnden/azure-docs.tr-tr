---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Proxyclick | Microsoft Docs'
description: Bu öğreticide, Azure Active Directory ve Proxyclick arasında çoklu oturum açmayı yapılandırma öğreneceksiniz.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 5c58a859-71c2-4542-ae92-e5f16a8e7f18
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: fa146ad5a36cc74a65ec6d3dee98b2ef35bc65ad
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66240401"
---
# <a name="tutorial-azure-active-directory-integration-with-proxyclick"></a>Öğretici: Proxyclick ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Proxyclick tümleştirme öğreneceksiniz.
Bu tümleştirme aşağıdaki avantajları sağlar:

* Azure AD Proxyclick erişimi denetlemek için kullanabilirsiniz.
* Kullanıcılarınız için Proxyclick (çoklu oturum açma) ile Azure AD hesaplarına otomatik olarak oturum açmanız etkinleştirebilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilir: Azure portalı.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [Azure Active Directory'de uygulamalar için çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Proxyclick yapılandırmak için sahip olmanız gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, oturum açabileceğiniz bir [bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/).
* Tekli etkin oturum sahip Proxyclick abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Proxyclick SP tarafından başlatılan ve IDP tarafından başlatılan SSO'yu destekler.

## <a name="add-proxyclick-from-the-gallery"></a>Galeriden Proxyclick Ekle

Azure AD'de Proxyclick tümleştirmesini ayarlamak için yönetilen SaaS uygulamaları listenize Galeriden Proxyclick eklemeniz gerekir.

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory**:

    ![Azure Active Directory'yi seçin](common/select-azuread.png)

2. Git **kurumsal uygulamalar** > **tüm uygulamaları**:

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Bir uygulama eklemek için seçin **yeni uygulama** pencerenin üst kısmındaki:

    ![Yeni uygulama seçme](common/add-new-app.png)

4. Arama kutusuna **Proxyclick**. Seçin **Proxyclick** seçin ve arama sonuçlarını **Ekle**.

     ![Arama sonuçları](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma ile Proxyclick Britta Simon adlı bir test kullanıcısı kullanarak test edin.
Çoklu oturum açmayı etkinleştirmek için Proxyclick içinde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir ilişki yapmanız gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Proxyclick ile test etmek için bu adımları tamamlamak gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on)**  kullanıcılarınız için özelliği etkinleştirmek için.
2. **[Proxyclick çoklu oturum açmayı yapılandırma](#configure-proxyclick-single-sign-on)**  uygulama tarafında.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açmayı test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  Azure AD çoklu oturum açma için kullanıcı etkinleştirmek için.
5. **[Proxyclick test kullanıcısı oluşturma](#create-a-proxyclick-test-user)**  kullanıcı Azure AD gösterimini bağlı.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  yapılandırma çalıştığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalındaki etkinleştirmeniz.

Azure AD çoklu oturum açma ile Proxyclick yapılandırmak için şu adımları uygulayın:

1. İçinde [Azure portalında](https://portal.azure.com/), Proxyclick uygulama tümleştirme sayfasında **çoklu oturum açma**:

    ![Çoklu oturum açma seçin](common/select-sso.png)

2. İçinde **tek bir oturum açma yönteminizi seçmeniz** Seç iletişim kutusunda **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için:

    ![Çoklu oturum açma yöntemi seçin](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim kutusunda:

    ![Düzenle simgesi](common/edit-urls.png)

4. İçinde **temel SAML yapılandırma** iletişim kutusu, IDP tarafından başlatılan modunda uygulama yapılandırmak istiyorsanız aşağıdaki adımları uygulayın.

    ![Temel SAML yapılandırma iletişim kutusu](common/idp-intiated.png)

    1. İçinde **tanımlayıcı** kutusuna, bu düzende bir URL girin:
   
       `https://saml.proxyclick.com/init/<companyId>`

    1. İçinde **yanıt URL'si** kutusuna, bu düzende bir URL girin:

       `https://saml.proxyclick.com/consume/<companyId>`

5. SP tarafından başlatılan modunda uygulama yapılandırmak isteyip istemediğinizi seçin **ek URL'lerini ayarlayın**. İçinde **oturum açma URL'si** kutusuna, bu düzende bir URL girin:
   
   `https://saml.proxyclick.com/init/<companyId>`

    ![Proxyclick etki alanı ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    

    > [!NOTE]
    > Bu değerler yer tutuculardır. Gerçek tanımlayıcısı kullanın, yanıt URL'si ve oturum açma URL'si gerekiyor. Bu değerleri almak için adımları Bu öğreticinin ilerleyen bölümlerinde açıklanmıştır.

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünden **indirme** yanındaki bağlantı **sertifika (Base64)** , gereksinimlerinize göre ve bilgisayarınızdaki sertifika Kaydet:

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. İçinde **Proxyclick kümesi** bölümünde, gereksinimlerinize göre uygun URL'ler kopyalayın:

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    1. **Oturum açma URL'si**.

    1. **Azure AD tanımlayıcısı**.

    1. **Oturum kapatma URL'si**.

### <a name="configure-proxyclick-single-sign-on"></a>Proxyclick çoklu oturum açmayı yapılandırın

1. Yeni bir web tarayıcısı penceresinde Proxyclick şirketinizin sitesi için bir yönetici olarak oturum açın

2. Seçin **hesabı & ayarları**:

    ![Hesap & ayarlarını seçin](./media/proxyclick-tutorial/configure1.png)

3. Ekranı aşağı kaydırarak **tümleştirmeler** seçin ve bölüm **SAML**:

    ![SAML seçin](./media/proxyclick-tutorial/configure2.png)

4. İçinde **SAML** bölümünde, aşağıdaki adımları uygulayın.

    ![SAML bölümü](./media/proxyclick-tutorial/configure3.png)

    1. Kopyalama **SAML tüketici URL** yapıştırın ve değer **yanıt URL'si** kutusunda **temel SAML yapılandırma** Azure portalında iletişim kutusu.

    1. Kopyalama **SAML SSO tekrar yönlendirme URL'sini** yapıştırın ve değer **oturum açma URL'si** ve **tanımlayıcı** kutularındaki **temel SAML yapılandırma** Azure portalında iletişim kutusu.

    1. İçinde **SAML isteği yöntemi** listesinden **HTTP yeniden yönlendirme**.

    1. İçinde **veren** kutusu, yapıştırma **Azure AD tanımlayıcısı** Azure portaldan kopyaladığınız değeri.

    1. İçinde **SAML 2.0 uç nokta URL'si** kutusu, yapıştırma **oturum açma URL'si** Azure portaldan kopyaladığınız değeri.

    1. Azure portalından indirdiğiniz sertifika dosyasını Not Defteri'nde açın. İçinde bu dosyanın içeriğini yapıştırın **sertifika** kutusu.

    1. Seçin **değişiklikleri kaydetmek**.

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

Bu bölümde, Azure çoklu oturum açma kullanmak için Proxyclick erişim vererek Britta Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**ve ardından **Proxyclick**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde seçin **Proxyclick**.

    ![Uygulama listesi](common/all-applications.png)

3. Sol bölmede seçin **kullanıcılar ve gruplar**:

    ![Kullanıcıları ve grupları seçin](common/users-groups-blade.png)

4. Seçin **Kullanıcı Ekle**ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim kutusu.

    ![Kullanıcı Ekle seçeneğini belirleme](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıların listesini ve ardından **seçin** pencerenin alt kısmındaki düğmesi.

6. SAML onaylaması rol değeri de beklediğiniz **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin. Tıklayın **seçin** pencerenin alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim kutusunda **atama**.

### <a name="create-a-proxyclick-test-user"></a>Proxyclick test kullanıcısı oluşturma

Proxyclick için oturum açmak Azure AD kullanıcılarının sağlamak için Proxyclick eklemeniz gerekir. Bunları el ile eklemeniz gerekir.

Bir kullanıcı hesabı oluşturmak için şu adımları uygulayın:

1. Proxyclick şirketinizin sitesi için bir yönetici olarak oturum açın

1. Seçin **iş arkadaşlarınıza** pencerenin üst kısmındaki:

    ![İş arkadaşlarını seçin](./media/proxyclick-tutorial/user1.png)

1. Seçin **iş arkadaşı ekleme**:

    ![İş arkadaşı Ekle'yi seçin](./media/proxyclick-tutorial/user2.png)

1. İçinde **bir iş arkadaşınız ekleme** bölümünde, aşağıdaki adımları uygulayın.

    ![Bir iş arkadaşınız bölümü ekleyin](./media/proxyclick-tutorial/user3.png)

    1. İçinde **e-posta** kutusuna, kullanıcının e-posta adresi girin. Bu durumda, **brittasimon\@contoso.com**.

    1. İçinde **ad** kutusuna, kullanıcının ilk adını girin. Bu durumda, **Britta**.

    1. İçinde **Soyadı** kutusunda, son kullanıcının adını girin. Bu durumda, **Simon**.

    1. Seçin **kullanıcı ekleme**.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Şimdi Azure AD çoklu oturum açma yapılandırmanızı erişim panelini kullanarak test gerekir.

Erişim Paneli'nde Proxyclick kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama Proxyclick örneği için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim ve kullanım uygulamaları uygulamalarım portalında](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

