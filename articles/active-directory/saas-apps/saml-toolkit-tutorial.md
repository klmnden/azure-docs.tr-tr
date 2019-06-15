---
title: 'Öğretici: Azure AD SAML araç seti ile Azure Active Directory Tümleştirmesi | Microsoft Docs'
description: Azure Active Directory ve Azure AD SAML Araç Seti arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 3f4348e7-c34e-43c7-926e-f1b26ffacf6d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/06/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4baeda4fca69969fae3f866e642ad3dc8ad46509
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67091930"
---
# <a name="tutorial-integrate-azure-ad-saml-toolkit-with-azure-active-directory"></a>Öğretici: Azure AD SAML Araç Seti Azure Active Directory ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Azure AD SAML Araç Seti tümleştirmeyi öğreneceksiniz. Azure AD SAML Araç Seti Azure AD ile tümleştirdiğinizde, şunları yapabilirsiniz:

* Azure AD SAML Araç Seti erişimi, Azure AD'de denetler.
* Otomatik olarak Azure AD SAML Araç Seti Azure AD'ye hesaplarıyla oturum olmasını sağlayın.
* Bir merkezi konumda - Azure portalı hesaplarınızı yönetin.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir aboneliğiniz yoksa, bir aylık ücretsiz deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).
* Azure AD SAML Araç Seti çoklu oturum açma (SSO) abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD SSO bir test ortamında test edin. Azure AD SAML araç takımı destekler **SP** SSO başlattı.

## <a name="adding-azure-ad-saml-toolkit-from-the-gallery"></a>Azure AD SAML Araç Seti galeri ekleme

Azure AD'de Azure AD SAML Araç Seti tümleştirmesini yapılandırmak için Azure AD SAML Araç Seti Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti.
1. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları**.
1. Yeni bir uygulama eklemek için seçin **yeni uygulama**.
1. İçinde **Galeriden Ekle** bölümüne şunu yazın **Azure AD SAML Araç Seti** arama kutusuna.
1. Seçin **Azure AD SAML Araç Seti** gelen sonuçlar panelinde ve uygulama ekleyin. Uygulama, kiracınıza eklendiği sırada birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Yapılandırma ve Azure AD SSO kullanarak bir test kullanıcı olarak adlandırılır ve Azure AD SAML araç seti ile test etme **B.Simon**. Çalışmak SSO için Azure AD SAML araç setindeki bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki oluşturmanız gerekir.

Yapılandırma ve Azure AD SAML araç seti ile Azure AD SSO sınamak için aşağıdaki yapı taşlarını tamamlayın:

1. **[Azure AD SSO'yu yapılandırma](#configure-azure-ad-sso)**  kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
2. **[Azure AD SAML Araç Seti SSO yapılandırma](#configure-azure-ad-saml-toolkit-sso)**  uygulama tarafında SSO ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açma B.Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  B.Simon Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
5. **[Azure AD SAML Araç Seti test kullanıcısı oluşturma](#create-azure-ad-saml-toolkit-test-user)**  kullanıcı Azure AD gösterimini bağlı olduğu Azure AD SAML Araç Seti B.Simon bir karşılığı sağlamak için.
6. **[Test SSO](#test-sso)**  yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek üzere aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **Azure AD SAML Araç Seti** uygulama tümleştirme sayfası, bulma **Yönet** bölümünde ve seçin **çoklu oturum açma**.
1. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** sayfasında **SAML**.
1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında, düzenleme/kalem simgesine tıklayıp **temel SAML yapılandırma** ayarlarını düzenlemek için.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. Üzerinde **temel SAML yapılandırma** sayfasında, aşağıdaki alanlar için değerleri girin:

    1. İçinde **oturum açma URL'si** metin kutusuna bir URL yazın: `https://samltoolkit.azurewebsites.net/`

    1. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna bir URL yazın: `https://samltoolkit.azurewebsites.net`

    1. İçinde **yanıt URL'si** metin kutusuna bir URL yazın: `https://samltoolkit.azurewebsites.net/SAML/Consume`

1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

   ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. Üzerinde **Azure AD SAML Araç Seti kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

   ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

### <a name="configure-azure-ad-saml-toolkit-sso"></a>Azure AD SAML Araç Seti SSO'yu yapılandırma

1. Açık ve Azure AD SAML Araç Seti Web sitesinde kaydetmediyseniz yeni bir web tarayıcı penceresi, önce kaydetmeniz tıklayarak **kaydetme**. Zaten kayıtlıysanız, Azure AD SAML Araç Seti şirket sitenizi kayıtlı oturum açma kimlik bilgilerini kullanarak oturum açın.

    ![Azure AD SAML Araç Seti kayıt](./media/saml-toolkit-tutorial/register.png)

1. Tıklayarak **SAML yapılandırma**.

    ![Azure AD SAML Araç Seti SAML yapılandırma](./media/saml-toolkit-tutorial/saml-configure.png)

1. **Oluştur**’a tıklayın.

    ![Azure AD SAML Araç Seti SSO oluşturma](./media/saml-toolkit-tutorial/createsso.png)

1. Üzerinde **SAML SSO Yapılandırması** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Azure AD SAML Araç Seti SSO oluşturma](./media/saml-toolkit-tutorial/fill-details.png)

    1. İçinde **oturum açma URL'si** metin kutusu, yapıştırma **oturum açma URL'si** Azure portaldan kopyaladığınız değeri.

    1. İçinde **Azure AD tanımlayıcısı** metin kutusu, yapıştırma **Azure AD tanımlayıcısı** Azure portaldan kopyaladığınız değeri.

    1. İçinde **oturum kapatma URL'si** metin kutusu, yapıştırma **oturum kapatma URL'si** Azure portaldan kopyaladığınız değeri.

    1. Tıklayın **Dosya Seç** ve karşıya yükleme **Federasyon meta verileri XML** Azure portalından indirdiğiniz dosyası.

    1. **Oluştur**’a tıklayın.

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

Bu bölümde, Azure AD SAML Araç Seti için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak B.Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.
1. Uygulamalar listesinde **Azure AD SAML Araç Seti**.
1. Uygulamanın genel bakış sayfasında bulma **Yönet** seçin ve bölüm **kullanıcılar ve gruplar**.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Kullanıcı ekleme bağlantısı](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **B.Simon** kullanıcılar listesinden ardından **seçin** ekranın alt kısmındaki düğmesi.
1. SAML onaylama işlemi herhangi bir rolü değer de beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından **seçin** ekranın alt kısmındaki düğmesi.
1. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-azure-ad-saml-toolkit-test-user"></a>Azure AD SAML Araç Seti test kullanıcısı oluşturma

Bu bölümde, Azure AD SAML araç setindeki B.Simon adlı bir kullanıcı oluşturuldu. Azure AD SAML Araç Seti tam zamanında kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Bir kullanıcı Azure AD SAML araç setindeki zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur.

### <a name="test-sso"></a>Test SSO

Erişim Paneli'nde Azure AD SAML Araç Seti kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama Azure AD SAML Araç Seti için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
