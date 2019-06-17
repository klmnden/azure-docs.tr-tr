---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile ExpenseIn | Microsoft Docs'
description: Azure Active Directory ve ExpenseIn arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 6ac8053b-a216-45d8-bf5e-ecd37d808e57
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/11/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7c09542013dff3a18965d1070216a938c26a144e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67102853"
---
# <a name="tutorial-integrate-expensein-with-azure-active-directory"></a>Öğretici: ExpenseIn Azure Active Directory ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile ExpenseIn tümleştirme öğreneceksiniz. ExpenseIn Azure AD ile tümleştirdiğinizde, şunları yapabilirsiniz:

* ExpenseIn erişimi, Azure AD'de denetler.
* Otomatik olarak ExpenseIn için kendi Azure AD hesapları ile oturum açmış olmasını sağlayın.
* Bir merkezi konumda - Azure portalı hesaplarınızı yönetin.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir aboneliğiniz yoksa, alabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).
* ExpenseIn çoklu oturum açma (SSO) abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD SSO bir test ortamında test edin. ExpenseIn destekler **SP ve IDP** SSO başlattı.

## <a name="adding-expensein-from-the-gallery"></a>Galeriden ExpenseIn ekleme

Azure AD'de ExpenseIn tümleştirmesini yapılandırmak için ExpenseIn Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti.
1. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları**.
1. Yeni bir uygulama eklemek için seçin **yeni uygulama**.
1. İçinde **Galeriden Ekle** bölümüne şunu yazın **ExpenseIn** arama kutusuna.
1. Seçin **ExpenseIn** gelen sonuçlar panelinde ve uygulama ekleyin. Uygulama, kiracınıza eklendiği sırada birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Yapılandırma ve Azure AD SSO kullanarak adlı bir test kullanıcı ExpenseIn ile test etme **B.Simon**. Çalışmak SSO için ExpenseIn içinde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki oluşturmanız gerekir.

Yapılandırma ve Azure AD SSO ile ExpenseIn sınamak için aşağıdaki yapı taşlarını tamamlayın:

1. **[Azure AD SSO'yu yapılandırma](#configure-azure-ad-sso)**  kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
2. **[ExpenseIn yapılandırma](#configure-expensein)**  uygulama tarafında SSO ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açma B.Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  B.Simon Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
5. **[ExpenseIn test kullanıcısı oluşturma](#create-expensein-test-user)**  kullanıcı Azure AD gösterimini bağlı ExpenseIn B.Simon bir karşılığı sağlamak için.
6. **[Test SSO](#test-sso)**  yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek üzere aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **ExpenseIn** uygulama tümleştirme sayfası, bulma **Yönet** bölümünde ve seçin **çoklu oturum açma**.
1. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** sayfasında **SAML**.
1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında, düzenleme/kalem simgesine tıklayıp **temel SAML yapılandırma** ayarlarını düzenlemek için.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** modunda başlatılan aşağıdaki adımı uygulayın:

    İçinde **yanıt URL'si** metin kutusunda, URL herhangi birini yazın:

    | |
    |--|
    | `https://app.expensein.com/samlcallback` |
    | `https://mobileapi.expensein.com/identity/samlcallback` |

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    İçinde **oturum açma URL'si** metin kutusuna bir URL yazın:  `https://app.expensein.com/saml`

1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta veri URL'si** tıklatıp**İndirme** indirmek için **sertifika (Base64)** ve bilgisayarınıza kaydedin.

   ![Sertifika indirme bağlantısı](./media/expensein-tutorial/copy-metdataurl-certificate.png)

1. Üzerinde **ExpenseIn kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

   ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

### <a name="configure-expensein"></a>ExpenseIn yapılandırın

1. Yüklemeniz gerekiyor ExpenseIn içinde yapılandırmasını otomatik hale getirmenizi **My Apps güvenli oturum açma tarayıcı uzantısı** tıklayarak **uzantıyı yükleme**.

    ![Uygulamaları uzantım](common/install-myappssecure-extension.png)

2. Uzantı tarayıcıya ekledikten sonra tıklayarak **Kurulum ExpenseIn** ExpenseIn uygulamaya yönlendirir. Burada, ExpenseIn oturum açmak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı otomatik olarak sizin için uygulamayı yapılandırma ve 3-5 adımlarını otomatik hale getirin.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. ExpenseIn el ile ayarlamak istiyorsanız, yeni bir web tarayıcı penceresi ve oturum ExpenseIn şirketinizin sitesi yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Tıklayın **yönetici** gidin sonra sayfanın üst kısmındaki **çoklu oturum açma** tıklatıp **Ekle sağlayıcısı**.

     ![ExpenseIn yapılandırma](./media/expenseIn-tutorial/config01.png)

5. Üzerinde **yeni kimlik sağlayıcısı** açılır penceresinde, aşağıdaki adımları gerçekleştirin:

    ![ExpenseIn yapılandırma](./media/expenseIn-tutorial/config02.png)

    a. İçinde **sağlayıcı adı** metin kutusuna, örn: Azure gibi bir ad yazın.

    b. Seçin **Evet** olarak **sağlayıcısı Intitated oturum açma izin**.

    c. İçinde **hedef Url** metin kutusunda, değerini yapıştırın **oturum açma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    d. İçinde **veren** metin kutusunda, değerini yapıştırın **Azure AD tanımlayıcısı**, hangi Azure Portalı'ndan kopyaladığınız.

    e. Sertifika (Base64) Not Defteri'nde açın, içeriğini kopyalayın ve yapıştırın **sertifika** metin kutusu.

    f. **Oluştur**’a tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, bir test kullanıcısı B.Simon adlı Azure portalında oluşturacaksınız.

1. Azure Portalı'ndaki sol bölmeden seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.
1. Seçin **yeni kullanıcı** ekranın üstünde.
1. İçinde **kullanıcı** özellikleri, aşağıdaki adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. İçinde **kullanıcı adı** alanına username@companydomain.extension. Örneğin, `B.Simon@contoso.com`.
   1. Seçin **Show parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için ExpenseIn erişim vererek B.Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.
1. Uygulamalar listesinde **ExpenseIn**.
1. Uygulamanın genel bakış sayfasında bulma **Yönet** seçin ve bölüm **kullanıcılar ve gruplar**.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Kullanıcı ekleme bağlantısı](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **B.Simon** kullanıcılar listesinden ardından **seçin** ekranın alt kısmındaki düğmesi.
1. SAML onaylama işlemi herhangi bir rolü değer de beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından **seçin** ekranın alt kısmındaki düğmesi.
1. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-expensein-test-user"></a>ExpenseIn test kullanıcısı oluşturma

ExpenseIn için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların ExpenseIn sağlanması gerekir. ExpenseIn içinde sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Yönetici olarak ExpenseIn için oturum açın.

2. Tıklayarak **yönetici** gidin sonra sayfanın üst kısmındaki **kullanıcılar** tıklatıp **yeni kullanıcı**.

     ![ExpenseIn yapılandırma](./media/expenseIn-tutorial/config03.png)

3. Üzerinde **ayrıntıları** açılır penceresinde, aşağıdaki adımları gerçekleştirin:

    ![ExpenseIn yapılandırma](./media/expenseIn-tutorial/config04.png)

    a. İçinde **ad** metin kutusunda, gibi kullanıcı adını girin **B**.

    b. İçinde **Soyadı** metin kutusunda, son kullanıcı gibi adını **Simon**.

    c. İçinde **e-posta** metin kutusuna, kullanıcının gibi e-posta girin `B.Simon@contoso.com`.

    d. **Oluştur**’a tıklayın.

### <a name="test-sso"></a>Test SSO

Erişim Paneli'nde ExpenseIn kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama ExpenseIn için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
