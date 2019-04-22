---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Coralogix | Microsoft Docs'
description: Azure Active Directory ve Coralogix arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: ba79bfc1-992e-4924-b76a-8eb0dfb97724
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 1/2/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d0095a825f5582dc795f5bebdcf08be07a92946e
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59678301"
---
# <a name="tutorial-azure-active-directory-integration-with-coralogix"></a>Öğretici: Coralogix ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Coralogix tümleştirme konusunda bilgi edinin.
Azure AD ile Coralogix tümleştirme ile aşağıdaki avantajları sağlar:

* Coralogix erişimi, Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınız için Coralogix (çoklu oturum açma) ile Azure AD hesaplarına otomatik olarak oturum açmanız etkinleştirebilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilir: Azure portalı.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Coralogix yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/).
- Çoklu oturum açma bir Coralogix abonelik etkin. 

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Coralogix SP tarafından başlatılan SSO'yu destekler.

## <a name="add-coralogix-from-the-gallery"></a>Galeriden Coralogix Ekle

Azure AD'de Coralogix tümleştirmesini yapılandırmak için önce Coralogix Galeriden yönetilen SaaS uygulamaları listenize ekleyin.

Galeriden Coralogix eklemek için aşağıdaki adımları uygulayın:

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Git **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üstündeki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Coralogix**. Seçin **Coralogix** seçin ve sonuçlar bölmesinde **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Coralogix](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Coralogix Britta Simon adlı bir test kullanıcı tabanlı Azure AD çoklu oturum açmayı sınayın.
Tek iş için oturum açma için Coralogix içinde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı yapmanız gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Coralogix ile test etmek için önce aşağıdaki yapı taşlarını tamamlayın:

1. [Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on) kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
2. [Coralogix çoklu oturum açmayı yapılandırma](#configure-coralogix-single-sign-on) üzerinde uygulama tarafından çoklu oturum açma ayarları yapılandırmak için.
3. [Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user) Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. [Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user) Britta Simon, Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
5. [Coralogix test kullanıcısı oluşturma](#create-a-coralogix-test-user) bir karşılığı Britta simon'un kullanıcı Azure AD gösterimini bağlı Coralogix sağlamak için.
6. [Çoklu oturum açmayı test](#test-single-sign-on) yapılandırma çalıştığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile Coralogix yapılandırmak için aşağıdaki adımları uygulayın:

1. İçinde [Azure portalında](https://portal.azure.com/), **Coralogix** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. İçinde **tek bir oturum açma yönteminizi seçmeniz** Seç iletişim kutusunda **SAML** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim kutusu.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. İçinde **temel SAML yapılandırma** iletişim kutusunda, aşağıdaki adımları uygulayın:

    ![Coralogix etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** kutusunda, aşağıdaki desene sahip bir URL girin: `https://<SUBDOMAIN>.coralogix.com`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusunda, aşağıdaki gibi bir URL girin:
    
    `https://api.coralogix.com/saml/metadata.xml`

    or

    `https://aws-client-prod.coralogix.com/saml/metadata.xml` 

    > [!NOTE]
    > Gerçek oturum açma URL değeri değil. Değerini gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Coralogix istemci Destek ekibine](mailto:info@coralogix.com) değeri alınamıyor. Desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Coralogix uygulamanın belirli bir biçimde SAML onaylamalarını bekliyor. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **kullanıcı öznitelikleri** iletişim kutusu.

    ![image](common/edit-attribute.png)

6. İçinde **kullanıcı taleplerini** konusundaki **kullanıcı öznitelikleri** iletişim kutusunda, talep kullanarak düzenleyin **Düzenle** simgesi. Kullanarak talepleri ekleyebilirsiniz **Ekle yeni talep** SAML belirteci özniteliği önceki görüntüde gösterildiği gibi yapılandırabilirsiniz. Ardından aşağıdaki adımları uygulayın:
    
    a. Seçin **düzenleme simgesi** açmak için **yönetmek, kullanıcı talepleri** iletişim kutusu.

    ![Görüntü](./media/coralogix-tutorial/tutorial_usermail.png) ![görüntüsü](./media/coralogix-tutorial/tutorial_usermailedit.png)

    b. Gelen **belirleyin adı tanımlayıcı biçimi** listesinden **e-posta adresi**.

    c. Gelen **kaynak özniteliği** listesinden **user.mail**.

    d. **Kaydet**’i seçin.

7. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünden **indirme** indirmek için **Federasyon meta veri XML**  gereksinimlerinize göre belirli Seçenekler. Bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

8. İçinde **Coralogix kümesi** bölümünde, uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-coralogix-single-sign-on"></a>Coralogix çoklu oturum açmayı yapılandırın

Çoklu oturum açmayı yapılandırma **Coralogix** yan, indirilen Gönder **Federasyon meta verileri XML** ve URL'leri için Azure Portalı'ndan kopyaladığınız [Coralogix Destek ekibine](mailto:info@coralogix.com). Bunlar, SAML SSO bağlantının her iki kenarı da düzgün ayarlandığından emin olun.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında seçin **yeni kullanıcı**.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları uygulayın.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alanına **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alanına, "brittasimon@yourcompanydomain.extension." Örneğin, bu durumda, girdiğiniz "brittasimon@contoso.com."

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri Not **parola** kutusu.

    d. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Coralogix erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**ve ardından **Coralogix**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Coralogix**.

    ![Uygulamalar listesinde Coralogix bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Seçin **Kullanıcı Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim kutusu.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde. Ardından **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması rol değeri de beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin. Ardından **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.

### <a name="create-a-coralogix-test-user"></a>Coralogix test kullanıcısı oluşturma

Bu bölümde, Britta Simon Coralogix içinde adlı bir kullanıcı oluşturun. Çalışmak [Coralogix Destek ekibine](mailto:info@coralogix.com) Coralogix platform kullanıcıları eklemek için. Oluşturma ve kullanıcılara çoklu oturum açma kullanmadan önce etkinleştirmeniz gerekir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı MyApps portalında kullanarak test.

MyApps portalında Coralogix kutucuğu seçtiğinizde, otomatik olarak Coralogix için oturum açmanız. MyApps portalında hakkında daha fazla bilgi için bkz: [MyApps portalı nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

