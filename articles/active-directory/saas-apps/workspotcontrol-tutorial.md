---
title: 'Öğretici: Azure Active Directory Tümleştirmesi Workspot denetimiyle | Microsoft Docs'
description: Azure Active Directory ile Workspot denetimi arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3ea8e4e9-f61f-4f45-b635-b0e306eda3d1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/12/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 97f375c6f48d3dc497eb59e76f19fc64cf906b56
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62098683"
---
# <a name="tutorial-azure-active-directory-integration-with-workspot-control"></a>Öğretici: Azure Active Directory Tümleştirmesi Workspot denetimi ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Workspot denetimi tümleştirme konusunda bilgi edinin.

Azure AD ile Workspot denetimi tümleştirme ile aşağıdaki avantajları sağlar:

- Workspot denetim erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) Workspot denetimine açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Workspot denetimi ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Workspot Denetim Çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Workspot denetimi ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-workspot-control-from-the-gallery"></a>Galeriden Workspot denetimi ekleme
Azure AD'de Workspot denetim tümleştirmesini yapılandırmak için Workspot denetim Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Workspot denetim eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![image](./media/workspotcontrol-tutorial/selectazuread.png)

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![image](./media/workspotcontrol-tutorial/a_select_app.png)
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![image](./media/workspotcontrol-tutorial/a_new_app.png)

4. Arama kutusuna **Workspot denetimi**seçin **Workspot denetimi** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![image](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve Azure AD çoklu oturum açmayı test Workspot denetimiyle "Britta Simon" adlı bir test kullanıcı tabanlı.

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcı Workspot denetimi bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı Workspot denetiminde arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Workspot denetimi ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Workspot denetim test kullanıcısı oluşturma](#create-a-workspot-control-test-user)**  - Workspot denetiminde, kullanıcının Azure AD gösterimini bağlı Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Workspot denetimi uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Workspot denetimi ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde [Azure portalında](https://portal.azure.com/), **Workspot denetimi** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![image](./media/workspotcontrol-tutorial/B1_B2_Select_SSO.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![image](./media/workspotcontrol-tutorial/b1_b2_saml_sso.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **temel SAML yapılandırma** iletişim.

    ![image](./media/workspotcontrol-tutorial/b1-domains_and_urlsedit.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** intiated modu:

    ![image](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<INSTANCENAME>-saml.workspot.com/saml/metadata`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<INSTANCENAME>-saml.workspot.com/saml/assertion`

    c. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

     ![image](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<INSTANCENAME>-saml.workspot.com/`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [Workspot denetimi istemci Destek ekibine](mailto:support@workspot.com) bu değerleri almak için. 

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** ve bilgisayarınıza kaydedin.

    ![image](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_certficate.png) 

6. Üzerinde **Workspot denetimini Ayarla** bölümünde, ihtiyacınıza göre uygun URL'yi kopyalayın.

    Not: URL aşağıdaki bildirebilir

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

    ![image](./media/workspotcontrol-tutorial/d1_samlsonfigure.png) 

7. Bir başka web tarayıcı penceresinde Workspot denetimi bir güvenlik yöneticisi olarak oturum açın.

8. Sayfanın üst kısmındaki araç çubuğunda **Kurulum**, ardından gidin **SAML**.

    ![image](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_setup.png)

9. Üzerinde **güvenlik onaylama işlemi biçimlendirme dili Yapılandırması** sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![image](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_saml.png)

    a. İçinde **varlık kimliği** metin değerini yapıştırın **Azure Ad tanımlayıcısı** hangi Azure portaldan kopyaladığınız.   

    b.In **oturum açma hizmeti URL'si** metin değerini yapıştırın **oturum açma URL'si** hangi Azure portaldan kopyaladığınız.

    c. İçinde **oturum kapatma hizmeti URL'si** metin değerini yapıştırın **oturum kapatma URL'si** hangi Azure portaldan kopyaladığınız. 

    d. Tıklayarak **güncelleştirme dosyası** base-64 karşıya yükleme düğmesini kodlanmış X.509 sertifikası ile Azure portalından indirdiğiniz sertifika.

    e. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![image](./media/workspotcontrol-tutorial/d_users_and_groups.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![image](./media/workspotcontrol-tutorial/d_adduser.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![image](./media/workspotcontrol-tutorial/d_userproperties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon\@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **özellikleri**seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’u seçin.
 
### <a name="create-a-workspot-control-test-user"></a>Workspot denetim test kullanıcısı oluşturma

Workspot denetimine oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar Workspot denetimine sağlanması gerekir. Workspot denetiminde sağlama el ile bir görev olur.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Workspot denetimine bir güvenlik yöneticisi olarak oturum açın.

2. Sayfanın üst kısmındaki araç çubuğunda **kullanıcılar**, ardından gidin **Kullanıcı Ekle**.

    ![image](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_adduser.png)

3. Üzerinde **yeni kullanıcı ekleme** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![image](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_addnewuser.png)

    a. İçinde **ad** metin kutusunda, gibi kullanıcı adını girin **Britta**.

    b. İçinde **Soyadı** metin kutusunda, son kullanıcı gibi adını **simon**.

    c. İçinde **e-posta** metin kutusuna, kullanıcının gibi e-posta girin **Brittasimon\@contoso.com**.

    d. Uygun kullanıcı rolünden seçin **rol** açılır.

    e. Uygun kullanıcı grubunu seçin **grubu** açılır.

    f. Tıklayın **kullanıcı ekleme**.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Workspot denetim için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![image](./media/workspotcontrol-tutorial/d_all_applications.png)

2. Uygulamalar listesinde **Workspot denetim**.

    ![image](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_app.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![image](./media/workspotcontrol-tutorial/d_leftpaneusers.png)

4. Seçin **Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![image](./media/workspotcontrol-tutorial/d_assign_user.png)

4. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

5. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Workspot denetimi kutucuğa tıkladığınızda, otomatik olarak Workspot denetim uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
