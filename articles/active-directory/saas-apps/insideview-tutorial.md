---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile InsideView | Microsoft Docs'
description: Bu öğreticide, Azure Active Directory ve InsideView arasında çoklu oturum açmayı yapılandırma öğreneceksiniz.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: c489a7ab-6b1f-4efb-8a66-8bc13bca78c3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: jeedes
ms.openlocfilehash: 2149b8410104b39652b176895a31b42e094265f5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67100098"
---
# <a name="tutorial-azure-active-directory-integration-with-insideview"></a>Öğretici: InsideView ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile InsideView tümleştirme öğreneceksiniz.
Bu tümleştirme aşağıdaki avantajları sağlar:

* Azure AD InsideView erişimi denetlemek için kullanabilirsiniz.
* Kullanıcılarınız için InsideView (çoklu oturum açma) ile Azure AD hesaplarına otomatik olarak oturum açmanız etkinleştirebilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilir: Azure portalı.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [Azure Active Directory'de uygulamalar için çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile InsideView yapılandırmak için sahip olmanız gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).
* Tekli etkin oturum sahip bir InsideView aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* InsideView IDP tarafından başlatılan SSO'yu destekler.

## <a name="add-insideview-from-the-gallery"></a>Galeriden InsideView Ekle

Azure AD'de InsideView tümleştirmesini ayarlamak için yönetilen SaaS uygulamaları listenize Galeriden InsideView eklemeniz gerekir.

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory**:

    ![Azure Active Directory'yi seçin](common/select-azuread.png)

2. Git **kurumsal uygulamalar** > **tüm uygulamaları**:

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Bir uygulama eklemek için seçin **yeni uygulama** pencerenin üst kısmındaki:

    ![Yeni uygulama seçme](common/add-new-app.png)

4. Arama kutusuna **InsideView**. Seçin **InsideView** seçin ve arama sonuçlarını **Ekle**.

    ![Arama sonuçları](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma ile InsideView Britta Simon adlı bir test kullanıcısı kullanarak test edin.
Çoklu oturum açmayı etkinleştirmek için InsideView içinde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir ilişki yapmanız gerekir.

Yapılandırma ve Azure AD çoklu oturum açma InsideView ile test etmek için bu adımları tamamlamak gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on)**  kullanıcılarınız için özelliği etkinleştirmek için.
2. **[InsideView çoklu oturum açmayı yapılandırma](#configure-insideview-single-sign-on)**  uygulama tarafında.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açmayı test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  Azure AD çoklu oturum açma için kullanıcı etkinleştirmek için.
5. **[Bir InsideView test kullanıcısı oluşturma](#create-an-insideview-test-user)**  kullanıcı Azure AD gösterimini bağlı.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  yapılandırma çalıştığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalındaki etkinleştirmeniz.

Azure AD çoklu oturum açma ile InsideView yapılandırmak için şu adımları uygulayın:

1. İçinde [Azure portalında](https://portal.azure.com/), InsideView uygulama tümleştirme sayfasında **çoklu oturum açma**:

    ![Çoklu oturum açma seçin](common/select-sso.png)

2. İçinde **tek bir oturum açma yönteminizi seçmeniz** Seç iletişim kutusunda **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için:

    ![Tek bir oturum açma yöntemi seçin](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim kutusunda:

    ![Düzenle simgesi](common/edit-urls.png)

4. İçinde **temel SAML yapılandırma** iletişim kutusunda, aşağıdaki adımları uygulayın.

    ![Temel SAML yapılandırma iletişim kutusu](common/idp-reply.png)

    İçinde **yanıt URL'si** kutusuna, bu düzende bir URL girin:

    `https://my.insideview.com/iv/<STS Name>/login.iv`

    > [!NOTE]
    > Bu değer bir yer tutucudur. Fiili yanıt URL'si kullanmanız gerekir. İlgili kişi [InsideView Destek ekibine](mailto:support@insideview.com) değeri alınamıyor. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** Azure portalında iletişim kutusu.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünden **indirme** yanındaki bağlantı **sertifika (ham)** , gereksinimlerinize göre ve bilgisayarınızdaki sertifika Kaydet:

    ![Sertifika indirme bağlantısı](common/certificateraw.png)

6. İçinde **InsideView kümesi** bölümünde, gereksinimlerinize göre uygun URL'ler kopyalayın:

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    1. **Oturum açma URL'si**.

    1. **Azure AD tanımlayıcısı**.

    1. **Oturum kapatma URL'si**.

### <a name="configure-insideview-single-sign-on"></a>InsideView çoklu oturum açmayı yapılandırın

1. Yeni bir web tarayıcısı penceresinde InsideView şirketinizin sitesi için bir yönetici olarak oturum açın

1. Pencerenin üst kısmında seçin **yönetici**, **SingleSignOn ayarları**ve ardından **ekleme SAML**.
   
   ![SAML çoklu oturum açma ayarları](./media/insideview-tutorial/ic794135.png "SAML çoklu oturum açma ayarları")

1. İçinde **yeni bir SAML ekleme** bölümünde, aşağıdaki adımları uygulayın.

    ![Yeni SAML bölüm ekleme](./media/insideview-tutorial/ic794136.png "yeni SAML bölüm ekleme")

    1. İçinde **STS adını** kutusuna, yapılandırmanız için bir ad girin.

    1. İçinde **SamlP/WS-Federasyon istenmeyen uç nokta** kutusu, yapıştırma **oturum açma URL'si** Azure portaldan kopyaladığınız değeri.

    1. Azure portalından indirdiğiniz ham sertifikayı açın. Sertifika içeriğini panoya kopyalayın ve ardından içeriği yapıştırın **STS sertifikasını** kutusu.

    1. İçinde **Crm kullanıcı eşleme kimliği** kutusuna **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress** .

    1. İçinde **e-posta Crm eşleme** kutusuna **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress** .

    1. İçinde **Crm ad eşleme** kutusuna **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname** .

    1. İçinde **Crm lastName eşleme** kutusuna **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname** .  

    1. **Kaydet**’i seçin.

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

Bu bölümde, Azure çoklu oturum açma kullanmak için InsideView erişim vererek Britta Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**ve ardından **InsideView**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde seçin **InsideView**.

    ![Uygulamaların listesi](common/all-applications.png)

3. Sol bölmede seçin **kullanıcılar ve gruplar**:

    ![Kullanıcıları ve grupları seçin](common/users-groups-blade.png)

4. Seçin **Kullanıcı Ekle**ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim kutusu.

    ![Kullanıcı Ekle seçeneğini belirleme](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıların listesini ve ardından **seçin** pencerenin alt kısmındaki düğmesi.

6. SAML onaylaması rol değeri de beklediğiniz **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin. Tıklayın **seçin** pencerenin alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim kutusunda **atama**.

### <a name="create-an-insideview-test-user"></a>Bir InsideView test kullanıcısı oluşturma

InsideView için oturum açmak Azure AD kullanıcılarının sağlamak için InsideView eklemeniz gerekir. Bunları el ile eklemeniz gerekir.

Kullanıcılar ve kişiler de InsideView içinde oluşturulacak kişi [InsideView Destek ekibine](mailto:support@insideview.com).

> [!NOTE]
> Herhangi bir kullanıcı hesabı oluşturma aracı kullanabilir veya API Azure AD'ye kullanıcı hesapları sağlamak için InsideView tarafından sağlanan.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Şimdi Azure AD çoklu oturum açma yapılandırmanızı erişim panelini kullanarak test gerekir.

Erişim Paneli'nde InsideView kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama InsideView örneği için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim ve kullanım uygulamaları uygulamalarım portalında](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
