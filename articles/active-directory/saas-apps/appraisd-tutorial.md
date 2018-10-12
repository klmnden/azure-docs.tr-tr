---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Appraisd | Microsoft Docs'
description: Azure Active Directory ve Appraisd arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: db063306-4d0d-43ca-aae0-09f0426e7429
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2018
ms.author: jeedes
ms.openlocfilehash: dae9a59b89f03a50b1adaed55cd8f97c06906526
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2018
ms.locfileid: "49118765"
---
# <a name="tutorial-azure-active-directory-integration-with-appraisd"></a>Öğretici: Azure Active Directory Appraisd ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Appraisd tümleştirme konusunda bilgi edinin.

Azure AD ile Appraisd tümleştirme ile aşağıdaki avantajları sağlar:

- Appraisd erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için Appraisd (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Appraisd yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir Appraisd çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Appraisd ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-appraisd-from-the-gallery"></a>Galeriden Appraisd ekleme
Azure AD'de Appraisd tümleştirmesini yapılandırmak için Appraisd Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Appraisd eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![image](./media/appraisd-tutorial/selectazuread.png)

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![image](./media/appraisd-tutorial/a_select_app.png)
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![image](./media/appraisd-tutorial/a_new_app.png)

4. Arama kutusuna **Appraisd**seçin **Appraisd** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![image](./media/appraisd-tutorial/tutorial_appraisd_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Appraisd sınayın.

Tek iş için oturum açma için Azure AD ne Appraisd karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Appraisd ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Appraisd ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir Appraisd test kullanıcısı oluşturma](#create-an-appraisd-test-user)**  - kullanıcı Azure AD gösterimini bağlı Appraisd Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Appraisd uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Appraisd yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde [Azure portalında](https://portal.azure.com/), **Appraisd** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![image](./media/appraisd-tutorial/B1_B2_Select_SSO.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![image](./media/appraisd-tutorial/b1_b2_saml_sso.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **temel SAML yapılandırma** iletişim.

    ![image](./media/appraisd-tutorial/b1-domains_and_urlsedit.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirmek **IDP** başlatılan modu:

    ![image](./media/appraisd-tutorial/tutorial_appraisd_url.png)

    a. Tıklayın **ek URL'lerini ayarlayın**. 

    b. İçinde **geçiş durumu** metin kutusuna bir URL yazın: `<TENANTCODE>`

    c. Uygulamada yapılandırmak istiyorsanız **SP** modunda başlatılan **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://app.appraisd.com/saml/<TENANTCODE>`

    > [!NOTE]
    > Bu öğreticinin ilerleyen bölümlerinde açıklanan Appraisd SSO yapılandırma sayfasında gerçek oturum açma URL'si ve geçiş durumu değer elde edin.
    
5. Appraisd uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Tıklayın **Düzenle** açmak için düğmeyi **kullanıcı öznitelikleri** iletişim.

    ![image](./media/appraisd-tutorial/i3-attribute.png)

6. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    a. Tıklayın **Düzenle** açmak için düğmeyi **yönetmek, kullanıcı talepleri** iletişim.

    ![image](./media/appraisd-tutorial/i2-attribute.png)

    ![image](./media/appraisd-tutorial/i4-attribute.png)

    b. Gelen **kaynak özniteliği** listesinde, öznitelik değeri seçin.

    c. **Kaydet**’e tıklayın. 

7. İçinde **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** ve bilgisayarınıza kaydedin.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_certficate.png) 

8. Üzerinde **Appraisd kümesi** bölümünde, ihtiyacınıza göre uygun URL'yi kopyalayın.

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

    ![image](./media/appraisd-tutorial/d1_samlsonfigure.png)

9. Farklı bir web tarayıcı penceresinde Appraisd için bir güvenlik yöneticisi olarak oturum açın.

10. Üst sayfanın sağ tıklayın **ayarları** simgesine ve ardından gidin **yapılandırma**.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_sett.png)

11. Menü Sol taraftan tıklayarak **SAML çoklu oturum açma**.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_single.png)

12. Üzerinde **SAML 2.0 çoklu oturum açmayı yapılandırma** sayfasında, aşağıdaki adımları gerçekleştirin:
    
    ![image](./media/appraisd-tutorial/tutorial_appraisd_saml.png)

    a. Kopyalama **varsayılan geçiş durumu** yapıştırın ve değer **geçiş durumu** metin kutusunda **temel SAML yapılandırma** Azure portalında.

    b. Kopyalama **hizmet tarafından başlatılan oturum açma URL'si** yapıştırın ve değer **oturum açma URL'si** metin kutusunda **temel SAML yapılandırma** Azure portalında.

13. Altında aynı sayfayı aşağı kaydırın **kullanıcıları tanımlama**, aşağıdaki adımları gerçekleştirin:

    ![image](./media/appraisd-tutorial/tutorial_appraisd_identifying.png)

    a. İçinde **kimlik sağlayıcısının çoklu oturum açma URL'si** metin değerini yapıştırın **oturum açma URL'si**, Azure portal'ı seçin ve kopyalanan **Kaydet**.

    b. İçinde **kimlik sağlayıcısını veren URL'si** metin değerini yapıştırın **Azure Ad tanımlayıcısı**, Azure portal'ı seçin ve kopyalanan **Kaydet**.

    c. Not Defteri'nde, Azure portalından indirdiğiniz base-64 kodlanmış sertifika açın, içeriğini kopyalayın ve ardından yapıştırın **X.509 sertifikası** kutusuna ve tıklatın **Kaydet**.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![image](./media/appraisd-tutorial/d_users_and_groups.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![image](./media/appraisd-tutorial/d_adduser.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![image](./media/appraisd-tutorial/d_userproperties.png)

    a. İçinde **adı** alanına **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **özellikleri**seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’u seçin.
 
### <a name="create-an-appraisd-test-user"></a>Bir Appraisd test kullanıcısı oluşturma

Azure AD etkinleştirmek için Appraisd için kullanıcıların oturum bunların Appraisd sağlanması gerekir. Appraisd içinde sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. İçin Appraisd bir güvenlik yöneticisi olarak oturum açın.

2. Üst sayfanın sağ tıklayın **ayarları** simgesine ve ardından gidin **Yönetim Merkezi**.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_admin.png)

3. Sayfanın üst kısmındaki araç çubuğunda **kişiler**, ardından gidin **yeni kullanıcı ekleme**.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_user.png)

4. Üzerinde **yeni kullanıcı ekleme** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![image](./media/appraisd-tutorial/tutorial_appraisd_newuser.png)
    
    a. İçinde **ad** metin kutusunda, gibi kullanıcı adını girin **Britta**.

    b. İçinde **Soyadı** metin kutusunda, son kullanıcı gibi adını **simon**.

    c. İçinde **e-posta** metin kutusuna, kullanıcının gibi e-posta girin **Brittasimon@contoso.com**.

    d. **Kullanıcı ekle**'ye tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Appraisd erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![image](./media/appraisd-tutorial/d_all_applications.png)

2. Uygulamalar listesinde **Appraisd**.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_app.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![image](./media/appraisd-tutorial/d_leftpaneusers.png)

4. Seçin **Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![image](./media/appraisd-tutorial/d_assign_user.png)

4. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

5. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Appraisd kutucuğa tıkladığınızda, otomatik olarak Appraisd uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
