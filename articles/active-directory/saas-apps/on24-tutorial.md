---
title: 'Öğretici: Sanal ortam SAML bağlantı ON24 ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ile ON24 sanal ortam SAML bağlantısı arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d4028fb5-b2ad-4c5d-b123-7b675c509d64
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0827895d58b0b7633ee4543495014c62b5394312
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56209498"
---
# <a name="tutorial-azure-active-directory-integration-with-on24-virtual-environment-saml-connection"></a>Öğretici: Sanal ortam SAML bağlantı ON24 ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile ON24 sanal ortam SAML bağlantı tümleştirme konusunda bilgi edinin.

Sanal ortam SAML bağlantı ON24 Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Sanal ortam SAML bağlantıya ON24 erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan ON24 sanal ortam SAML bağlantı (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi ON24 sanal ortam SAML bağlantısı ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir sanal ortam SAML bağlantı ON24 çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden ON24 sanal ortam SAML bağlantı ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-on24-virtual-environment-saml-connection-from-the-gallery"></a>Galeriden ON24 sanal ortam SAML bağlantı ekleme
Azure AD'de ON24 sanal ortam SAML bağlantı tümleştirmesini yapılandırmak için ON24 sanal ortam SAML bağlantı Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden ON24 sanal ortam SAML bağlantı eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![image](./media/on24-tutorial/selectazuread.png)

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![image](./media/on24-tutorial/a_select_app.png)
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![image](./media/on24-tutorial/a_new_app.png)

4. Arama kutusuna **ON24 sanal ortam SAML bağlantı**seçin **ON24 sanal ortam SAML bağlantı** sonucu panelinden ardından **Ekle** düğme eklemek için uygulama.

     ![image](./media/on24-tutorial/tutorial_on24_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma ON24 sanal ortam SAML "Britta Simon" adlı bir test kullanıcı tabanlı bağlantıyı sınayın.

Tek iş için oturum açma için Azure AD ne ON24 sanal ortam SAML bağlantı karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının ON24 sanal ortam SAML bağlantı ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma ON24 sanal ortam SAML bağlantısı ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir sanal ortam SAML bağlantı ON24 test kullanıcısı oluşturma](#create-an-on24-virtual-environment-saml-connection-test-user)**  - bir karşılığı Britta simon'un ON24 sanal ortam SAML kullanıcı Azure AD gösterimini bağlı bağlantı sağlamak için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve ON24 sanal ortam SAML bağlantı uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ON24 sanal ortam SAML bağlantı ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde [Azure portalında](https://portal.azure.com/), **ON24 sanal ortam SAML bağlantı** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![image](./media/on24-tutorial/B1_B2_Select_SSO.png)

2. Tıklayın **oturum açma tek Mod Değiştir** seçmek için ekranın en üstünde **SAML** modu.

      ![image](./media/on24-tutorial/b1_b2_saml_ssso.png)

3. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![image](./media/on24-tutorial/b1_b2_saml_sso.png)

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **temel SAML yapılandırma** iletişim.

    ![image](./media/on24-tutorial/b1-domains_and_urlsedit.png)

5. Üzerinde **temel SAML yapılandırma** bölümü, uygulamada yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin **IDP** intiated modu:

    ![image](./media/on24-tutorial/tutorial_on24_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL yazın:

     **Üretim ortamı URL'si**
    
    `SAML-VSHOW.on24.com`

    `SAML-Gateway.on24.com` 

    `SAP PROD SAML-EliteAudience.on24.com` 
                
     **QA ortamı URL'si**
    
    `SAMLQA-VSHOW.on24.com` 

    `SAMLQA-Gateway.on24.com` 

    `SAMLQA-EliteAudience.on24.com`
 
    b. İçinde **yanıt URL'si** metin kutusuna bir URL yazın:
    
     **Üretim ortamı URL'si**
    
    `https://federation.on24.com/sp/ACS.saml2`

    `https://federation.on24.com/sp/eyJ2c2lkIjoiU0FNTC1WU2hvdy5vbjI0LmNvbSJ9/ACS.saml2`

    `https://federation.on24.com/sp/eyJ2c2lkIjoiU0FNTC1HYXRld2F5Lm9uMjQuY29tIn0/ACS.saml2`

    `https://federation.on24.com/sp/eyJ2c2lkIjoiU0FNTC1FbGl0ZUF1ZGllbmNlLm9uMjQuY29tIn0/ACS.saml2`

     **QA ortamı URL'si**
    
    `https://qafederation.on24.com/sp/ACS.saml2`

    `https://qafederation.on24.com/sp/eyJ2c2lkIjoiU0FNTFFBLVZzaG93Lm9uMjQuY29tIn0/ACS.saml2`

    `https://qafederation.on24.com/sp/eyJ2c2lkIjoiU0FNTFFBLUdhdGV3YXkub24yNC5jb20ifQ/ACS.saml2`
     
    `https://qafederation.on24.com/sp/eyJ2c2lkIjoiU0FNTFFBLUVsaXRlQXVkaWVuY2Uub24yNC5jb20ifQ/ACS.saml2` 

    c. Tıklayın **ek URL'lerini ayarlayın**. 

    d. İçinde **geçiş durumu** metin kutusuna bir URL yazın: `https://vshow.on24.com/vshow/ms_azure_saml_test?r=<ID>`

    e. Uygulamada yapılandırmak istiyorsanız **SP** intiated modu, **oturum açma URL'si** metin kutusuna bir URL yazın: `https://vshow.on24.com/vshow/<INSTANCENAME>`

6. Üzerinde **yukarı çoklu oturum açma SAML ile Ayarla** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** olarak başına uygun sertifikayı indirmek için gereksinim ve bilgisayarınıza kaydedin.

    ![image](./media/on24-tutorial/tutorial_on24_certificate.png) 

7. Çoklu oturum açmayı yapılandırma **ON24 sanal ortam SAML bağlantı** tarafı, gereken göndermek için Azure portalından indirilen sertifika/metaveri [ON24 sanal ortam SAML bağlantı Destek](https://www.on24.com/about-us/support/). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![image](./media/on24-tutorial/d_users_and_groups.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![image](./media/on24-tutorial/d_adduser.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![image](./media/on24-tutorial/d_userproperties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **özellikleri**seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’u seçin.
 
### <a name="create-an-on24-virtual-environment-saml-connection-test-user"></a>Bir sanal ortam SAML bağlantı ON24 test kullanıcısı oluşturma

Bu bölümde, sanal ortamı SAML bağlantı ON24 Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [ON24 sanal ortam SAML bağlantı Destek ekibine](https://www.on24.com/about-us/support/) ON24 sanal ortam SAML bağlantı platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, sanal ortamı SAML bağlantı ON24 erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![image](./media/on24-tutorial/d_all_applications.png)

2. Uygulamalar listesinde **ON24 sanal ortam SAML bağlantı**.

    ![image](./media/on24-tutorial/tutorial_on24_app.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![image](./media/on24-tutorial/d_leftpaneusers.png)

4. Seçin **Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![image](./media/on24-tutorial/d_assign_user.png)

4. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

5. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli ON24 sanal ortam SAML bağlantı kutucuğa tıkladığınızda, otomatik olarak sanal ortam SAML bağlantı ON24 uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

