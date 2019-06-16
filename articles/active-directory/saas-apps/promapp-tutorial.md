---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Promapp | Microsoft Docs'
description: Bu öğreticide, Azure Active Directory ve Promapp arasında çoklu oturum açmayı yapılandırma öğreneceksiniz.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 418d0601-6e7a-4997-a683-73fa30a2cfb5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: 2ddb8777a6470c0e739545e71867a694022d1723
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67093593"
---
# <a name="tutorial-azure-active-directory-integration-with-promapp"></a>Öğretici: Promapp ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Promapp tümleştirme öğreneceksiniz.
Bu tümleştirme aşağıdaki avantajları sağlar:

* Azure AD Promapp erişimi denetlemek için kullanabilirsiniz.
* Kullanıcılarınız için Promapp (çoklu oturum açma) ile Azure AD hesaplarına otomatik olarak oturum açmanız etkinleştirebilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilir: Azure portalı.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [Azure Active Directory'de uygulamalar için çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Promapp yapılandırmak için sahip olmanız gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, oturum açabileceğiniz bir [bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/).
* Tekli etkin oturum sahip Promapp abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Promapp SP tarafından başlatılan ve IDP tarafından başlatılan SSO'yu destekler.

* Promapp, just-ın-time kullanıcı sağlamayı destekler.

## <a name="add-promapp-from-the-gallery"></a>Galeriden Promapp Ekle

Azure AD'de Promapp tümleştirmesini ayarlamak için yönetilen SaaS uygulamaları listenize Galeriden Promapp eklemeniz gerekir.

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory**:

    ![Azure Active Directory'yi seçin](common/select-azuread.png)

2. Git **kurumsal uygulamalar** > **tüm uygulamaları**:

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Bir uygulama eklemek için seçin **yeni uygulama** pencerenin üst kısmındaki:

    ![Yeni uygulama seçme](common/add-new-app.png)

4. Arama kutusuna **Promapp**. Seçin **Promapp** seçin ve arama sonuçlarını **Ekle**.

     ![Arama sonuçları](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma ile Promapp Britta Simon adlı bir test kullanıcısı kullanarak test edin.
Çoklu oturum açmayı etkinleştirmek için Promapp içinde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir ilişki yapmanız gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Promapp ile test etmek için bu adımları tamamlamak gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on)**  kullanıcılarınız için özelliği etkinleştirmek için.
2. **[Promapp çoklu oturum açmayı yapılandırma](#configure-promapp-single-sign-on)**  uygulama tarafında.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açmayı test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  Azure AD çoklu oturum açma için kullanıcı etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  yapılandırma çalıştığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalındaki etkinleştirmeniz.

Azure AD çoklu oturum açma ile Promapp yapılandırmak için şu adımları uygulayın:

1. İçinde [Azure portalında](https://portal.azure.com/), Promapp uygulama tümleştirme sayfasında **çoklu oturum açma**:

    ![Çoklu oturum açma seçin](common/select-sso.png)

2. İçinde **tek bir oturum açma yönteminizi seçmeniz** Seç iletişim kutusunda **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için:

    ![Tek bir oturum açma yöntemi seçin](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim kutusunda:

    ![Düzenle simgesi](common/edit-urls.png)

4. İçinde **temel SAML yapılandırma** iletişim kutusu, IDP tarafından başlatılan modunda uygulama yapılandırmak istiyorsanız aşağıdaki adımları tamamlayın.

    ![Temel SAML yapılandırma iletişim kutusu](common/idp-intiated.png)

    1. İçinde **tanımlayıcı** kutusuna, bu düzende bir URL girin:

       | |
        |--|
        | `https://go.promapp.com/TENANTNAME/`|
        | `https://au.promapp.com/TENANTNAME/`|
        | `https://us.promapp.com/TENANTNAME/`|
        | `https://eu.promapp.com/TENANTNAME/`|
        | `https://ca.promapp.com/TENANTNAME/`|
        |   |

       > [!NOTE]
       > Promapp ile Azure AD tümleştirmesi şu anda yalnızca hizmet tarafından başlatılan kimlik doğrulaması için yapılandırılır. (Diğer bir deyişle, kimlik doğrulama işlemi Promapp URL'ye giderek başlatır.) Ancak **yanıt URL'si** alan gerekli bir alandır.

    1. İçinde **yanıt URL'si** kutusuna, bu düzende bir URL girin:

       `https://<DOMAINNAME>.promapp.com/TENANTNAME/saml/authenticate.aspx`

5. SP tarafından başlatılan modunda uygulama yapılandırmak isteyip istemediğinizi seçin **ek URL'lerini ayarlayın**. İçinde **oturum açma URL'si** kutusuna, bu düzende bir URL girin:

      `https://<DOMAINNAME>.promapp.com/TENANTNAME/saml/authenticate`

    ![Promapp etki alanı ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

   

    > [!NOTE]
    > Bu değerler yer tutuculardır. Gerçek tanımlayıcısı kullanın, yanıt URL'si ve oturum açma URL'si gerekiyor. İlgili kişi [Promapp Destek ekibine](https://www.promapp.com/about-us/contact-us/) değerleri alabilirsiniz. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** Azure portalında iletişim kutusu.

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünden **indirme** yanındaki bağlantı **sertifika (Base64)** , gereksinimlerinize göre ve bilgisayarınızdaki sertifika Kaydet:

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. İçinde **Promapp kümesi** bölümünde, gereksinimlerinize göre uygun URL'ler kopyalayın:

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    1. **Oturum açma URL'si**.

    1. **Azure AD tanımlayıcısı**.

    1. **Oturum kapatma URL'si**.

### <a name="configure-promapp-single-sign-on"></a>Promapp çoklu oturum açmayı yapılandırın

1. Promapp şirketinizin sitesi için bir yönetici olarak oturum açın

2. Pencerenin üst kısmındaki menüde **yönetici**:
   
    ![Yönetici Seç][12]

3. Seçin **yapılandırma**:
   
    ![Yapılandırma seçin][13]

4. İçinde **güvenlik** iletişim kutusunda, aşağıdaki adımları uygulayın.
   
    ![Güvenlik iletişim kutusu][14]
    
    1. Yapıştırma **oturum açma URL'si** Azure portaldan kopyaladığınız **SSO oturum açma URL'si** kutusu.
    
    1. İçinde **SSO - çoklu oturum açma modu** listesinden **isteğe bağlı**. **Kaydet**’i seçin.

       > [!NOTE]
       > İsteğe bağlı modu, yalnızca test amaçlıdır. Yapılandırmasıyla memnun kaldıysanız sonra seçin **gerekli** içinde **SSO - çoklu oturum açma modu** tüm kullanıcıları, Azure AD kimlik doğrulaması için liste.

    1. Önceki bölümde indirdiğiniz sertifika Not Defteri'nde açın. İlk satırı olmadan sertifikayı içeriğini kopyalayın ( **---BEGIN CERTIFICATE---** ) veya son satırı ( **---END CERTIFICATE---** ). Sertifika içerik içine yapıştırın **SSO x.509 sertifikası** kutusuna ve ardından **Kaydet**.

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

Bu bölümde, Azure çoklu oturum açma kullanmak için Promapp erişim vererek Britta Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**ve ardından **Promapp**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde seçin **Promapp**.

    ![Uygulamaların listesi](common/all-applications.png)

3. Sol bölmede seçin **kullanıcılar ve gruplar**:

    ![Kullanıcıları ve grupları seçin](common/users-groups-blade.png)

4. Seçin **Kullanıcı Ekle**ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim kutusu.

    ![Kullanıcı Ekle seçeneğini belirleme](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıların listesini ve ardından **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması rol değeri de beklediğiniz **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin. Tıklayın **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim kutusunda **atama**.

### <a name="just-in-time-user-provisioning"></a>Just-ın-time kullanıcı sağlama

Promapp, just-ın-time kullanıcı sağlamayı destekler. Bu özellik varsayılan olarak etkindir. Bir kullanıcı Promapp içinde zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Şimdi Azure AD çoklu oturum açma yapılandırmanızı erişim panelini kullanarak test gerekir.

Erişim Paneli'nde Promapp kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama Promapp örneği için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim ve kullanım uygulamaları uygulamalarım portalında](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

<!--Image references-->

[12]: ./media/promapp-tutorial/tutorial_promapp_05.png
[13]: ./media/promapp-tutorial/tutorial_promapp_06.png
[14]: ./media/promapp-tutorial/tutorial_promapp_07.png
