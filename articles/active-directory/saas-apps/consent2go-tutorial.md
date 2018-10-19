---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Consent2Go | Microsoft Docs'
description: Azure Active Directory ve Consent2Go arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ea93bc02-58ca-4468-84ff-359888fc6183
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/09/2018
ms.author: jeedes
ms.openlocfilehash: 89fc0b69a3a7ca3f795b4ae0e79b11a4bcd9c9fb
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49431866"
---
# <a name="tutorial-azure-active-directory-integration-with-consent2go"></a>Öğretici: Azure Active Directory Consent2Go ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Consent2Go tümleştirme konusunda bilgi edinin.

Azure AD ile Consent2Go tümleştirme ile aşağıdaki avantajları sağlar:

- Consent2Go erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan Consent2Go için açma, kullanıcılarınızın etkinleştirebilirsiniz (çoklu oturum açma) ile Azure AD hesaplarına.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Consent2Go yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Consent2Go çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Consent2Go ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-consent2go-from-the-gallery"></a>Galeriden Consent2Go ekleme
Azure AD'de Consent2Go tümleştirmesini yapılandırmak için Consent2Go Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Consent2Go eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![image](./media/consent2go-tutorial/selectazuread.png)

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![image](./media/consent2go-tutorial/a_select_app.png)
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![image](./media/consent2go-tutorial/a_new_app.png)

4. Arama kutusuna **Consent2Go**seçin **Consent2Go** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![image](./media/consent2go-tutorial/tutorial_consent2go_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Consent2Go sınayın.

Tek iş için oturum açma için Azure AD ne Consent2Go karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Consent2Go ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Consent2Go ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Consent2Go test kullanıcısı oluşturma](#create-a-consent2go-test-user)**  - kullanıcı Azure AD gösterimini bağlı Consent2Go Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Consent2Go uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Consent2Go yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde [Azure portalında](https://portal.azure.com/), **Consent2Go** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![image](./media/consent2go-tutorial/b1_b2_select_sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunu tıklatın **seçin** için **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![image](./media/consent2go-tutorial/b1_b2_saml_sso.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **temel SAML yapılandırma** iletişim.

    ![image](./media/consent2go-tutorial/b1-domains_and_urlsedit.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    İçinde **oturum açma URL'si** metin kutusuna bir URL yazın:  `   https://www.mcbschools.com/Login`

    ![image](./media/consent2go-tutorial/tutorial_consent2go_url.png)

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala simgesine tıklayarak **uygulama Federasyon meta verileri URL'sini** ve bilgisayarınıza kaydedin.

    ![image](./media/consent2go-tutorial/tutorial_consent2go_certificate.png) 

6. Çoklu oturum açmayı yapılandırma **Consent2Go** tarafı, kopyalanan göndermek için ihtiyacınız **uygulama Federasyon meta verileri URL'sini** için [Consent2Go Destek ekibine](mailto:support@consent2go.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![image](./media/consent2go-tutorial/d_users_and_groups.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![image](./media/consent2go-tutorial/d_adduser.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![image](./media/consent2go-tutorial/d_userproperties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **özellikleri**seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’u seçin.
 
### <a name="create-a-consent2go-test-user"></a>Consent2Go test kullanıcısı oluşturma

Bu bölümde, Britta Simon Consent2Go içinde adlı bir kullanıcı oluşturun. Çalışmak [Consent2Go Destek ekibine](mailto:support@consent2go.com) Consent2Go platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Consent2Go erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![image](./media/consent2go-tutorial/d_all_applications.png)

2. Uygulamalar listesinde **Consent2Go**.

    ![image](./media/consent2go-tutorial/tutorial_consent2go_app.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![image](./media/consent2go-tutorial/d_leftpaneusers.png)

4. Seçin **Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![image](./media/consent2go-tutorial/d_assign_user.png)

4. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

5. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Consent2Go kutucuğa tıkladığınızda, otomatik olarak Consent2Go uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)


