---
title: 'Öğretici: Uyarlamalı Insights ile Azure Active Directory Tümleştirmesi | Microsoft Docs'
description: Azure Active Directory ve Uyarlamalı Insights arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 13af9d00-116a-41b8-8ca0-4870b31e224c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d42c86ec262cd9d3d3db3035d252429e44c1208f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60285642"
---
# <a name="tutorial-azure-active-directory-integration-with-adaptive-insights"></a>Öğretici: Uyarlamalı Insights ile Azure Active Directory Tümleştirmesi

Bu öğreticide, Uyarlamalı Insights Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

Uyarlamalı Insights'ı Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Uyarlamalı Insights erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak Uyarlamalı Insights'a (çoklu oturum açma) açan, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Uyarlamalı Insights ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir Uyarlamalı Insights çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Uyarlamalı Insights ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-adaptive-insights-from-the-gallery"></a>Galeriden Uyarlamalı Insights ekleme
Azure AD Uyarlamalı Öngörüler tümleştirmesini yapılandırmak için Uyarlamalı Insights Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Uyarlamalı Insights eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![image](./media/adaptivesuite-tutorial/selectazuread.png)

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![image](./media/adaptivesuite-tutorial/a_select_app.png)
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![image](./media/adaptivesuite-tutorial/a_new_app.png)

4. Arama kutusuna **Uyarlamalı Insights**seçin **Uyarlamalı Insights** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![image](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Uyarlamalı "Britta Simon" adlı bir test kullanıcı tabanlı Insights ile Azure AD çoklu oturum açma testi.

Tek çalışmak için oturum açma için Azure AD ne Uyarlamalı ınsights karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı Uyarlamalı ınsights arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Uyarlamalı öngörülerle sınamak için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Uyarlamalı ınsights'ı bir test kullanıcısı oluşturma](#create-an-adaptive-insights-test-user)**  - kullanıcı Azure AD gösterimini bağlı Uyarlamalı ınsights'ta Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Uyarlamalı Insights uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Uyarlamalı Insights ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde [Azure portalında](https://portal.azure.com/), **Uyarlamalı Insights** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![image](./media/adaptivesuite-tutorial/B1_B2_Select_SSO.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![image](./media/adaptivesuite-tutorial/b1_b2_saml_sso.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **temel SAML yapılandırma** iletişim.

    ![image](./media/adaptivesuite-tutorial/b1-domains_and_urlsedit.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** intiated modu:

    ![image](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_url.png)

    a. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna bir URL şu biçimi kullanarak: `https://login.adaptiveinsights.com:443/samlsso/<unique-id>`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://login.adaptiveinsights.com:443/samlsso/<unique-id>`

    >[!NOTE]
    > Uyarlamalı Insights'tan 's tanımlayıcı (varlık kimliği) ve yanıt URL'si değerleri alabilirsiniz **SAML SSO ayarlarını** sayfası.
 
5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** ve bilgisayarınıza kaydedin.

    ![image](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_certficate.png) 

6. Üzerinde **Uyarlamalı ınsights'ı ayarlarken** bölümünde, ihtiyacınıza göre uygun URL'yi kopyalayın.

    Not: URL aşağıdaki bildirebilir

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

    ![image](./media/adaptivesuite-tutorial/d1_samlsonfigure.png) 

7. Farklı bir web tarayıcı penceresinde Uyarlamalı Insights şirketinizin sitesi için bir yönetici olarak oturum açın.

8. Git **yönetici**.

    ![Yönetici](./media/adaptivesuite-tutorial/IC805644.png "yönetici")

9. İçinde **kullanıcıları ve rolleri** bölümünde **SAML SSO ayarlarını Yönet**.

    ![SAML SSO ayarlarını yönetme](./media/adaptivesuite-tutorial/IC805645.png "SAML SSO ayarlarını yönetme")

10. Üzerinde **SAML SSO ayarlarını** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![SAML SSO ayarlarını](./media/adaptivesuite-tutorial/IC805646.png "SAML SSO ayarları")

    a. İçinde **kimlik sağlayıcı adı** metin yapılandırmanız için bir ad yazın.

    b. Yapıştırma **Azure Ad tanımlayıcısı** değeri kopyalanan Azure portalından **kimlik sağlayıcısı varlık kimliği** metin.

    c. Yapıştırma **oturum açma URL'si** değeri kopyalanan Azure portalından **kimlik sağlayıcısı SSO URL** metin.

    d. Yapıştırma **oturum kapatma URL'si** değeri kopyalanan Azure portalından **özel oturum kapatma URL'si** metin.

    e. İndirilen sertifikanızı karşıya yüklemek için tıklayın **dosya**.

    f. İçin şunu seçin:

    * **SAML kullanıcı kimliği**seçin **Uyarlamalı Insights kullanıcının adını**.

    * **SAML kullanıcı kimliği konumu**seçin **kullanıcı kimliği, Nameıd konusunda**.

    * **SAML Nameıd biçimi**seçin **e-posta adresi**.

    * **SAML'yi etkinleştir**seçin **izin SAML SSO ve doğrudan Uyarlamalı Insights oturum açma**.

    g. Kopyalama **Uyarlamalı Insights SSO URL** ve yapıştırmak **tanımlayıcı (varlık kimliği)** ve **yanıt URL'si** metin kutuları içinde **Uyarlamalı Insights etki alanı ve URL'ler** bölümünde Azure portalında.

    h. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![image](./media/adaptivesuite-tutorial/d_users_and_groups.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![image](./media/adaptivesuite-tutorial/d_adduser.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![image](./media/adaptivesuite-tutorial/d_userproperties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon\@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **özellikleri**seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’u seçin.
 
### <a name="create-an-adaptive-insights-test-user"></a>Uyarlamalı ınsights'ı bir test kullanıcısı oluşturma

Uyarlamalı Insights'a oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar Uyarlamalı Öngörüler sağlanması gerekir. Uyarlamalı Insights söz konusu olduğunda, sağlama, elle bir görevin.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:** 

1. Oturum açın, **Uyarlamalı Insights** yönetici olarak şirketin site.
2. Git **yönetici**.

   ![Yönetici](./media/adaptivesuite-tutorial/IC805644.png "yönetici")

3. İçinde **kullanıcıları ve rolleri** bölümünde **Kullanıcı Ekle**.

   ![Kullanıcı ekleme](./media/adaptivesuite-tutorial/IC805648.png "kullanıcı ekleme")
   
4. İçinde **yeni kullanıcı** bölümünde, aşağıdaki adımları gerçekleştirin:

   ![Gönderme](./media/adaptivesuite-tutorial/IC805649.png "gönderin")

   a. Tür **adı**, **oturum açma**, **e-posta**, **parola** ilgili zbilgisayarlar istediğiniz geçerli bir Azure Active Directory kullanıcısı metin kutuları.

   b. Seçin bir **rol**.

   c. **Gönder**'e tıklayın.

>[!NOTE]
>Herhangi diğer Uyarlamalı Insights kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak Uyarlamalı Insights tarafından sağlanan.
>

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Uyarlamalı Öngörülere erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![image](./media/adaptivesuite-tutorial/d_all_applications.png)

2. Uygulamalar listesinde **Uyarlamalı Insights**.

    ![image](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_app.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![image](./media/adaptivesuite-tutorial/d_leftpaneusers.png)

4. Seçin **Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![image](./media/adaptivesuite-tutorial/d_assign_user.png)

4. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

5. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Uyarlamalı Insights kutucuğa tıkladığınızda, otomatik olarak Uyarlamalı Insights uygulamanızı açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
