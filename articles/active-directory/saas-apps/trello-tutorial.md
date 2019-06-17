---
title: 'Öğretici: Trello ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Trello arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: cd5ae365-9ed6-43a6-920b-f7814b993949
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/02/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 730ff5ff12f18d1f85b3ca53adb42fee41e19fb4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67088288"
---
# <a name="tutorial-azure-active-directory-integration-with-trello"></a>Öğretici: Trello ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Trello tümleştirme konusunda bilgi edinin.
Trello Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Trello erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) için Trello kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilir: Azure portalı.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Trello ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/).
* Tek oturum üzerinde-etkin olmayan bir Trello abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Trello SP ve IDP-başlatılan SSO'yu destekler.

* Trello zamanında kullanıcı sağlamayı destekler

## <a name="add-trello-from-the-gallery"></a>Trello Galeriden Ekle

Azure AD'de Trello tümleştirmesini yapılandırmak için önce Trello Galeriden yönetilen SaaS uygulamaları listenize ekleyin.

Trello Galeriden eklemek için aşağıdaki adımları uygulayın:

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Seçin **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üstündeki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Trello**ve ardından **Trello** sonuçlar bölmesinden.

5. Seçin **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Trello](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Trello adlı bir test kullanıcı tabanlı test **Britta Simon**.

Tek iş için oturum açma için Trello'da bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı yapmanız gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Trello ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. [Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on) kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
2. [Trello çoklu oturum açmayı yapılandırma](#configure-trello-single-sign-on) üzerinde uygulama tarafından çoklu oturum açma ayarları yapılandırmak için.
3. [Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user) Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. [Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user) Britta Simon, Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
5. [Trello test kullanıcısı oluşturma](#create-a-trello-test-user) kullanıcı Azure AD gösterimini bağlı trello'da bir karşılığı Britta simon'un sağlamak için.
6. [Çoklu oturum açmayı test](#test-single-sign-on) yapılandırma çalıştığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

> [!NOTE]
> Alması gereken **\<Kurumsal\>** trello'daki bilgi. Bilgi değer yoksa, kişi [Trello Destek ekibine](mailto:support@trello.com) , kuruluşunuz için bilgi almak için.

Azure AD çoklu oturum açma Trello ile yapılandırmak için aşağıdaki adımları uygulayın:

1. İçinde [Azure portalında](https://portal.azure.com/), **Trello** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. İçinde **tek bir oturum açma yönteminizi seçmeniz** Seç iletişim kutusunda **SAML** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **SAML ile çoklu oturum açmayı ayarlama** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim kutusu.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. İçinde **temel SAML yapılandırma** bölümünde IDP tarafından başlatılan modunda uygulama yapılandırmak istiyorsanız aşağıdaki adımları uygulayın:

    ![Oturum açma bilgileri tek bir Trello etki alanı ve URL'ler](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** kutusunda, URL şu biçimi kullanarak girin: `https://trello.com/auth/saml/metadata`

    b. İçinde **yanıt URL'si** kutusunda, URL şu biçimi kullanarak girin: `https://trello.com/auth/saml/consume/<enterprise>`

5. Seçin **ek URL'lerini ayarlayın**ve uygulama SP tarafından başlatılan modunda yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin:

    ![Oturum açma bilgileri tek bir Trello etki alanı ve URL'ler](common/metadata-upload-additional-signon.png)

    İçinde **oturum açma URL'si** kutusunda, URL şu biçimi kullanarak girin:  `https://trello.com/auth/saml/login/<enterprise>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısıyla güncelleştirme, yanıt URL'si ve oturum açma URL'si. İlgili kişi [Trello istemci Destek ekibine](mailto:support@trello.com) bu değerleri almak için. Desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

6. Trello uygulamanın belirli bir biçimde SAML onaylamalarını bekliyor. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **kullanıcı öznitelikleri** iletişim kutusu.

    ![Kullanıcı öznitelikleri iletişim kutusu](common/edit-attribute.png)

7. İçinde **kullanıcı taleplerini** konusundaki **kullanıcı öznitelikleri** iletişim kutusunda, önceki görüntüde gösterildiği gibi SAML belirteci özniteliği yapılandırabilirsiniz. Ardından aşağıdaki adımları uygulayın:

    | Ad |  Kaynak özniteliği|
    | --- | --- |
    | User.Email | User.Mail |
    | User.FirstName | User.givenName |
    | User.LastName | User.surname |

    a. Seçin **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim kutusu.

    ![Kullanıcı talepleri iletişim kutusu](common/new-save-attribute.png)

    ![Kullanıcı taleplerini Yönet](common/new-attribute-details.png)

    b. İçinde **adı** kutusunda, ilgili satır için gösterilen öznitelik adını girin.

    c. Bırakın **Namespace** boş.

    d. İçin **kaynak**seçin **özniteliği**.

    e. İçinde **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri girin.

    f. **Tamam**’ı seçin.

    g. **Kaydet**’i seçin.

8. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünden **indirme** indirmek için **sertifika (Base64)**  gereksinimlerinizi karşılayan belirli Seçenekler. Bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

9. Üzerinde **Trello kümesi** bölümünde, gereksinimlerinize göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-trello-single-sign-on"></a>Trello çoklu oturum açmayı yapılandırın

Çoklu oturum açma Trello tarafta yapılandırmak için ilk olarak indirilen Gönder **sertifika (Base64)** ve URL'leri için Azure Portalı'ndan kopyaladığınız [Trello Destek ekibine](mailto:support@trello.com). Bunlar, SAML SSO bağlantının her iki kenarı da düzgün ayarlandığından emin olun.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları uygulayın.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alanına **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alanına, "brittasimon@yourcompanydomain.extension". Örneğin, bu durumda, girdiğiniz "BrittaSimon@contoso.com".

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri Not **parola** kutusu.

    d. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açmayı kullanmak için Trello erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**ve ardından **Trello**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Trello**.

    ![Uygulamalar listesinde Trello bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Seçin **Kullanıcı Ekle** düğmesi. Ardından **atama Ekle** iletişim kutusunda **kullanıcılar ve gruplar**.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde. Ardından **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylama işlemi ardından, içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin. Ardından **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.

### <a name="create-a-trello-test-user"></a>Trello test kullanıcısı oluşturma

Bu bölümde, Trello'da Britta Simon adlı bir kullanıcı oluşturun. Trello, varsayılan olarak etkindir, zaman kullanıcı hazırlama, sadece destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Trello'da bir kullanıcı zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur.

> [!NOTE]
> Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Trello Destek ekibine](mailto:support@trello.com).

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı MyApps portalında kullanarak test.

MyApps portalında Trello kutucuğu seçtiğinizde, otomatik olarak Trello için oturum açmanız. Uygulamalarım portal hakkında daha fazla bilgi için bkz. [MyApps portalı nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

