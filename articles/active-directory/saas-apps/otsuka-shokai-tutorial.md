---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Otsuka Shokai | Microsoft Docs'
description: Azure Active Directory ve Otsuka Shokai arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: c04bb636-a3c7-4940-9b36-810425b855d1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bd06eafca2c508bc73fa2b327235621797be417c
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67274203"
---
# <a name="tutorial-integrate-otsuka-shokai-with-azure-active-directory"></a>Öğretici: Otsuka Shokai Azure Active Directory ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Otsuka Shokai tümleştirme öğreneceksiniz. Otsuka Shokai Azure AD ile tümleştirdiğinizde, şunları yapabilirsiniz:

* Otsuka Shokai erişimi, Azure AD'de denetler.
* Otomatik olarak Otsuka Shokai için kendi Azure AD hesapları ile oturum açmış olmasını sağlayın.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir aboneliğiniz yoksa, alabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).
* Otsuka Shokai çoklu oturum açma (SSO) abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD SSO bir test ortamında test edin. Otsuka Shokai destekler **IDP** SSO başlattı.

## <a name="adding-otsuka-shokai-from-the-gallery"></a>Galeriden Otsuka Shokai ekleme

Azure AD'de Otsuka Shokai tümleştirmesini yapılandırmak için Otsuka Shokai Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti.
1. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları**.
1. Yeni bir uygulama eklemek için seçin **yeni uygulama**.
1. İçinde **Galeriden Ekle** bölümüne şunu yazın **Otsuka Shokai** arama kutusuna.
1. Seçin **Otsuka Shokai** gelen sonuçlar panelinde ve uygulama ekleyin. Uygulama, kiracınıza eklendiği sırada birkaç saniye bekleyin.
1. Tıklayarak **özellikleri** sekmesinde, kopya **uygulama kimliği** ve kullanmak için bilgisayarınızda kaydedin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Yapılandırma ve Azure AD SSO Otsuka adlı bir test kullanıcı kullanarak Shokai ile test etme **b Simon**. Çalışmak SSO için Otsuka Shokai içinde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki oluşturmanız gerekir.

Yapılandırma ve Azure AD SSO ile Otsuka Shokai sınamak için aşağıdaki yapı taşlarını tamamlayın:

1. **[Azure AD SSO'yu yapılandırma](#configure-azure-ad-sso)**  kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
2. **[Otsuka Shokai yapılandırma](#configure-otsuka-shokai)**  uygulama tarafında SSO ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açma b Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  Azure AD çoklu oturum açmayı kullanmak b Simon etkinleştirmek için.
5. **[Otsuka Shokai test kullanıcısı oluşturma](#create-otsuka-shokai-test-user)**  b Simon bir karşılığı kullanıcı Azure AD gösterimini bağlı Otsuka Shokai sağlamak için.
6. **[Test SSO](#test-sso)**  yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek üzere aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **Otsuka Shokai** uygulama tümleştirme sayfası, bulma **Yönet** bölümünde ve seçin **çoklu oturum açma**.
1. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** sayfasında **SAML**.
1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında, düzenleme/kalem simgesine tıklayıp **temel SAML yapılandırma** ayarlarını düzenlemek için.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında, uygulamanın önceden yapılandırılmış olduğu ve gerekli URL'ler zaten Azure ile önceden doldurulur. Tıklayarak yapılandırmayı kaydetmek kullanıcının erişmesi **Kaydet** düğmesi.

1. Otsuka Shokai uygulama, özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde SAML onaylamalarını bekler. Varsayılan öznitelikler listesinde aşağıdaki ekran görüntüsünde gösterilmektedir oysa **NameIdentifier** ile eşlenmiş **user.userprincipalname**. Otsuka Shokai uygulama bekliyor **NameIdentifier** ile eşlenecek **user.objectid**tıklayarak özellik eşlemesi düzenlemeniz gerekir böylece **Düzenle**  simgesi ve değişiklik öznitelik eşlemesi.

    ![image](common/edit-attribute.png)

1. Yukarıdaki için ayrıca Otsuka Shokai uygulama SAML yanıtta geçirilecek birkaç daha fazla öznitelik bekliyor. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda gösterildiği gibi SAML belirteci özniteliği eklemek için aşağıdaki adımları gerçekleştirin tablonun altındaki:

    | Ad | Kaynak özniteliği|
    | ---------------| --------------- |
    | Uygulama Kimliği | `<Application ID>` |

    >[!NOTE]
    >`<Application ID>` öğesinden kopyalanan değer **özellikleri** Azure portalının sekmesi.

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Bırakın **Namespace** boş.

    d. Kaynağı olarak **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

### <a name="configure-otsuka-shokai"></a>Configure Otsuka Shokai

1. Müşterinin sayfam SSO uygulamadan SSO ayarı başlatır sihirbazın bağladığınızda.

2. Otsuka kimliği kayıtlı değil, Otsuka kimliği yeni kayıt için devam edin.   İçin bağlantı ayarını Otsuka kimliği kayıtlıysanız, devam edin.

3. Sonuna kadar devam etmek ve müşterinin sayfam oturum açtıktan sonra üst ekran görüntülendiğinde, SSO ayarlarını tamamlandı.

4. Kılavuz ekranı açıldıktan sonra müşterinin sayfam SSO uygulamadan sonraki bağlanışınızda üst ekran müşterinin sayfam oturum açtıktan sonra görüntülenir.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, bir test kullanıcısı b Simon adlı Azure portalında oluşturacaksınız.

1. Azure Portalı'ndaki sol bölmeden seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.
1. Seçin **yeni kullanıcı** ekranın üstünde.
1. İçinde **kullanıcı** özellikleri, aşağıdaki adımları izleyin:
   1. **Ad** alanına `B. Simon` girin.  
   1. İçinde **kullanıcı adı** alanına username@companydomain.extension. Örneğin, `B.Simon@contoso.com`.
   1. Seçin **Show parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, B. Otsuka Shokai için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.
1. Uygulamalar listesinde **Otsuka Shokai**.
1. Uygulamanın genel bakış sayfasında bulma **Yönet** seçin ve bölüm **kullanıcılar ve gruplar**.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Kullanıcı ekleme bağlantısı](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **b Simon** kullanıcılar listesinden ardından **seçin** ekranın alt kısmındaki düğmesi.
1. SAML onaylama işlemi herhangi bir rolü değer de beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından **seçin** ekranın alt kısmındaki düğmesi.
1. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-otsuka-shokai-test-user"></a>Otsuka Shokai test kullanıcısı oluşturma

Yeni kayıt SaaS hesabının Otsuka Shokai için ilk erişim gerçekleştirilir. Ayrıca, biz de Azure AD hesabını ve SaaS hesabını yeni oluşturma sırasında ilişkilendireceksiniz.

### <a name="test-sso"></a>Test SSO

Erişim Paneli'nde Otsuka Shokai kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama Otsuka Shokai için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
