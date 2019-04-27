---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile TalentLMS | Microsoft Docs'
description: Azure Active Directory ve TalentLMS arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: c903d20d-18e3-42b0-b997-6349c5412dde
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8fa78ec2b5623dfd010a8fe5709916a47e221a9e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60732339"
---
# <a name="tutorial-azure-active-directory-integration-with-talentlms"></a>Öğretici: TalentLMS ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile TalentLMS tümleştirme konusunda bilgi edinin.

Azure AD ile TalentLMS tümleştirme ile aşağıdaki avantajları sağlar:

- TalentLMS erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için TalentLMS (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile TalentLMS yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik TalentLMS çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme burada alabilirsiniz: [Deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden TalentLMS ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-talentlms-from-the-gallery"></a>Galeriden TalentLMS ekleme
Azure AD'de TalentLMS tümleştirmesini yapılandırmak için TalentLMS Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden TalentLMS eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **TalentLMS**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/talentlms-tutorial/tutorial_talentlms_search.png)

1. Sonuçlar panelinde seçin **TalentLMS**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/talentlms-tutorial/tutorial_talentlms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı TalentLMS ile test etme

Tek iş için oturum açma için Azure AD ne TalentLMS karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının TalentLMS ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

TalentLMS içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma TalentLMS ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[TalentLMS test kullanıcısı oluşturma](#creating-a-talentlms-test-user)**  - kullanıcı Azure AD gösterimini bağlı TalentLMS Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve TalentLMS uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile TalentLMS yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **TalentLMS** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/talentlms-tutorial/tutorial_talentlms_samlbase.png)

1. Üzerinde **TalentLMS etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/talentlms-tutorial/tutorial_talentlms_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<tenant-name>.TalentLMSapp.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `http://<tenant-name>.talentlms.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [TalentLMS istemci Destek ekibine](https://www.talentlms.com/contact) bu değerleri almak için. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde, kopya **parmak İZİ** sertifikadan değeri.

    ![Çoklu oturum açmayı yapılandırın](./media/talentlms-tutorial/tutorial_talentlms_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/talentlms-tutorial/tutorial_general_400.png)

1. Üzerinde **TalentLMS yapılandırma** bölümünde **yapılandırma TalentLMS** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/talentlms-tutorial/tutorial_talentlms_configure.png)  

1. Farklı bir web tarayıcı penceresinde TalentLMS şirketinizin sitesi için bir yönetici olarak oturum açın.

1. İçinde **hesabı & ayarları** bölümünde **kullanıcılar** sekmesi.
   
    ![& Hesap ayarları](./media/talentlms-tutorial/IC777296.png "hesabı & ayarları")

1. Tıklayın **çoklu oturum açma (SSO)**,

1. Çoklu oturum açma bölümünde aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma](./media/talentlms-tutorial/IC777297.png "çoklu oturum açma")   

    a. Gelen **SSO tümleştirme türü** listesinden **SAML 2.0**.

    b. İçinde **kimlik sağlayıcıyı (IDP)** metin değerini yapıştırın **SAML varlık kimliği**, hangi Azure Portalı'ndan kopyaladığınız.
 
    c. Yapıştırma **parmak izi** Azure portalında bir değerden **sertifika parmak izi** metin.    

    d.  İçinde **uzaktan oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure Portalı'ndan kopyaladığınız.
 
    e. İçinde **uzak oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    f. Aşağıdakileri doldurun: 

    * İçinde **TargetedID** metin kutusuna `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`
     
    * İçinde **ad** metin kutusuna `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`
    
    * İçinde **Soyadı** metin kutusuna `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`
    
    * İçinde **e-posta** metin kutusuna `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`
    
1. **Kaydet**’e tıklayın.
 
> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/talentlms-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/talentlms-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/talentlms-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/talentlms-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-talentlms-test-user"></a>TalentLMS test kullanıcısı oluşturma

TalentLMS için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların TalentLMS sağlanması gerekir. TalentLMS söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **TalentLMS** Kiracı.

1. Tıklayın **kullanıcılar**ve ardından **Kullanıcı Ekle**.

1. Üzerinde **Kullanıcı Ekle** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcı ekleme](./media/talentlms-tutorial/IC777299.png "kullanıcı ekleme")  

    a. İçinde **ad** metin gibi kullanıcı adını girin **Britta**.

    b. İçinde **Soyadı** metin gibi kullanıcının soyadını girin **Simon**.
 
    c. İçinde **e-posta adresi** metin gibi kullanıcının e-posta girin **brittasimon\@contoso.com**.

    d. Tıklayın **kullanıcı ekleme**.

>[!NOTE]
>Herhangi diğer TalentLMS kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak TalentLMS tarafından sağlanan.
 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için TalentLMS erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon TalentLMS için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **TalentLMS**.

    ![Çoklu oturum açmayı yapılandırın](./media/talentlms-tutorial/tutorial_talentlms_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test sağlamaktır.

Erişim panelinde TalentLMS kutucuğa tıkladığınızda, otomatik olarak TalentLMS uygulamanıza açan

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/talentlms-tutorial/tutorial_general_01.png
[2]: ./media/talentlms-tutorial/tutorial_general_02.png
[3]: ./media/talentlms-tutorial/tutorial_general_03.png
[4]: ./media/talentlms-tutorial/tutorial_general_04.png

[100]: ./media/talentlms-tutorial/tutorial_general_100.png

[200]: ./media/talentlms-tutorial/tutorial_general_200.png
[201]: ./media/talentlms-tutorial/tutorial_general_201.png
[202]: ./media/talentlms-tutorial/tutorial_general_202.png
[203]: ./media/talentlms-tutorial/tutorial_general_203.png

