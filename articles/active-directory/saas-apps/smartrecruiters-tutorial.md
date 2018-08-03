---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle SmartRecruiters | Microsoft Docs'
description: Azure Active Directory ve SmartRecruiters arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: e96aeecd-e113-454e-89c3-58c9f44cfd4c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: jeedes
ms.openlocfilehash: b248cd7d5d45e4f91bc97a5a29476f9bfa03089d
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39426994"
---
# <a name="tutorial-azure-active-directory-integration-with-smartrecruiters"></a>Öğretici: Azure Active Directory SmartRecruiters ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile SmartRecruiters tümleştirme konusunda bilgi edinin.

Azure AD ile SmartRecruiters tümleştirme ile aşağıdaki avantajları sağlar:

- SmartRecruiters erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için SmartRecruiters (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile SmartRecruiters yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir SmartRecruiters çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden SmartRecruiters ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-smartrecruiters-from-the-gallery"></a>Galeriden SmartRecruiters ekleme
Azure AD'de SmartRecruiters tümleştirmesini yapılandırmak için SmartRecruiters Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden SmartRecruiters eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **SmartRecruiters**seçin **SmartRecruiters** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde SmartRecruiters](./media/smartrecruiters-tutorial/tutorial_smartrecruiters_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı SmartRecruiters sınayın.

Tek iş için oturum açma için Azure AD ne SmartRecruiters karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının SmartRecruiters ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

SmartRecruiters içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma SmartRecruiters ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[SmartRecruiters test kullanıcısı oluşturma](#create-a-smartrecruiters-test-user)**  - kullanıcı Azure AD gösterimini bağlı SmartRecruiters Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve SmartRecruiters uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile SmartRecruiters yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **SmartRecruiters** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/smartrecruiters-tutorial/tutorial_smartrecruiters_samlbase.png)

1. Üzerinde **SmartRecruiters etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![SmartRecruiters etki alanı ve URL'ler tek oturum açma bilgileri](./media/smartrecruiters-tutorial/tutorial_smartrecruiters_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://www.smartrecruiters.com/web-sso/saml/<companyname>`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://www.smartrecruiters.com/web-sso/saml/<companyname>/callback`

1. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![SmartRecruiters etki alanı ve URL'ler tek oturum açma bilgileri](./media/smartrecruiters-tutorial/tutorial_smartrecruiters_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://www.smartrecruiters.com/web-sso/saml/<companyname>/login`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [SmartRecruiters istemci Destek ekibine](https://www.smartrecruiters.com/about-us/contact-us/) bu değerleri almak için. 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve sertifika bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/smartrecruiters-tutorial/tutorial_smartrecruiters_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/smartrecruiters-tutorial/tutorial_general_400.png)
    
1. Üzerinde **SmartRecruiters yapılandırma** bölümünde **yapılandırma SmartRecruiters** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![SmartRecruiters yapılandırma](./media/smartrecruiters-tutorial/tutorial_smartrecruiters_configure.png) 

1. Farklı bir web tarayıcı penceresinde SmartRecruiters şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Git **ayarları / yönetici**.

    ![SmartRecruiters yapılandırma](./media/smartrecruiters-tutorial/configure.png)

1. İçinde **yapılandırma** bölümünde **Web SSO**.

    ![SmartRecruiters yapılandırma](./media/smartrecruiters-tutorial/configure1.png)

1. İki durumlu **Web SSO etkinleştirme**.

    ![SmartRecruiters yapılandırma](./media/smartrecruiters-tutorial/configure2.png)

1. İçinde **kimlik sağlayıcı Yapılandırması**, aşağıdaki adımları gerçekleştirin:

    ![SmartRecruiters yapılandırma](./media/smartrecruiters-tutorial/configure4.png)

    a. İçinde **kimlik sağlayıcısı URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız.

    b. Açık **certificate(Base64)** Azure Portalı'ndan yüklemiş ve değeri olarak yapıştırın **kimlik sağlayıcısı sertifikası** metin.

1. Tıklayın **Web SSO kaydetme yapılandırma**.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/smartrecruiters-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/smartrecruiters-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/smartrecruiters-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/smartrecruiters-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-smartrecruiters-test-user"></a>SmartRecruiters test kullanıcısı oluşturma

Bu bölümde, Britta Simon SmartRecruiters içinde adlı bir kullanıcı oluşturun. Çalışmak [SmartRecruiters Destek ekibine](https://www.smartrecruiters.com/about-us/contact-us/) SmartRecruiters platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için SmartRecruiters erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon SmartRecruiters için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **SmartRecruiters**.

    ![Uygulamalar listesinde SmartRecruiters bağlantı](./media/smartrecruiters-tutorial/tutorial_smartrecruiters_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde SmartRecruiters kutucuğa tıkladığınızda, otomatik olarak SmartRecruiters uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/smartrecruiters-tutorial/tutorial_general_01.png
[2]: ./media/smartrecruiters-tutorial/tutorial_general_02.png
[3]: ./media/smartrecruiters-tutorial/tutorial_general_03.png
[4]: ./media/smartrecruiters-tutorial/tutorial_general_04.png

[100]: ./media/smartrecruiters-tutorial/tutorial_general_100.png

[200]: ./media/smartrecruiters-tutorial/tutorial_general_200.png
[201]: ./media/smartrecruiters-tutorial/tutorial_general_201.png
[202]: ./media/smartrecruiters-tutorial/tutorial_general_202.png
[203]: ./media/smartrecruiters-tutorial/tutorial_general_203.png

