---
title: 'Öğretici: Gizli dizi sunucusu (şirket içi) ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Gizli dizi sunucusu (şirket) ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: be4ba84a-275d-4f71-afce-cb064edc713f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9167a5ed72e6fec2ca03cc97d1d41dd6cd4aaba6
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62104586"
---
# <a name="tutorial-azure-active-directory-integration-with-secret-server-on-premises"></a>Öğretici: Gizli dizi sunucusu (şirket içi) ile Azure Active Directory Tümleştirme

Bu öğreticide, gizli sunucusu (şirket içi) Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

Gizli dizi sunucusu (şirket içi) Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Gizli dizi sunucusu (şirket içi) erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak gizli sunucusuna (şirket içi) (çoklu oturum açma) ile Azure AD hesaplarına açan, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Gizli dizi sunucusu (şirket) ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir gizli dizi sunucusu (şirket içi) çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden gizli sunucusu (şirket içi) ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-secret-server-on-premises-from-the-gallery"></a>Galeriden gizli sunucusu (şirket içi) ekleme
Azure AD ile gizli sunucusu (şirket içi) tümleştirmesini yapılandırmak için gizli sunucusu (şirket içi) Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden gizli sunucusu (şirket içi) eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **gizli sunucusu (şirket içi)** seçin **gizli sunucusu (şirket içi)** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde gizli sunucusu (şirket içi)](./media/secretserver-on-premises-tutorial/tutorial_secretserver_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma gizli anahtarı "Britta Simon" adlı bir test kullanıcı tabanlı sunucusu (şirket) ile test edin.

Tek çalışmak için oturum açma için Azure AD ne gizli sunucusu (şirket içi) karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı gizliliği sunucusu (şirket içi) arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma gizli anahtarı sunucusu (şirket) ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Gizli dizi sunucusu (şirket içi) test kullanıcısı oluşturma](#create-a-secret-server-on-premises-test-user)**  - gizli anahtarı kullanıcı Azure AD gösterimini bağlı sunucusu (şirket içi) Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma gizli anahtarı sunucusu (şirket içi) uygulamanızı yapılandırın.

**Azure AD çoklu oturum açma gizli anahtarı sunucusuyla (şirket içi) yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **gizli sunucusu (şirket içi)** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/secretserver-on-premises-tutorial/tutorial_secretserver_samlbase.png)

1. Üzerinde **gizli sunucusu (şirket içi) etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![Çoklu oturum açma bilgileri gizli sunucusu (şirket içi) etki alanı ve URL'ler](./media/secretserver-on-premises-tutorial/tutorial_secretserver_url.png)

    a. İçinde **tanımlayıcı** metin değeri bir örnek olarak seçilen kullanıcı girin: `https://secretserveronpremises.azure`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<SecretServerURL>/SAML/AssertionConsumerService.aspx`

    > [!NOTE]
    > Yukarıda gösterilen varlık kimliği yalnızca örnek olarak verilmiştir; Azure AD'de parola Server örneğinizi tanımlayan herhangi bir benzersiz değer seçmek ücretsizdir. Bu varlık kimliği için gönderilmesini [istemci gizli anahtarı sunucusu (şirket içi) destek ekibi](https://thycotic.force.com/support/s/) ve bunlar kendi tarafında yapılandırın. Daha fazla ayrıntı için lütfen okuyun [bu makalede](https://thycotic.force.com/support/s/article/Configuring-SAML-in-Secret-Server).

1. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Çoklu oturum açma bilgileri gizli sunucusu (şirket içi) etki alanı ve URL'ler](./media/secretserver-on-premises-tutorial/tutorial_secretserver_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<SecretServerURL>/login.aspx`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [istemci gizli anahtarı sunucusu (şirket içi) destek ekibi](https://thycotic.force.com/support/s/) bu değerleri almak için.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/secretserver-on-premises-tutorial/tutorial_secretserver_certificate.png)

1. Denetleme **gelişmiş sertifika imzalama ayarlarını göster** seçip **imzalama seçeneği** olarak **oturum SAML yanıtını ve onayını**.

    ![İmzalama Seçenekleri](./media/secretserver-on-premises-tutorial/signing.png)

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/secretserver-on-premises-tutorial/tutorial_general_400.png)
    
1. Üzerinde **gizli sunucusu (şirket içi) yapılandırma** bölümünde **yapılandırma gizli sunucusu (şirket içi)** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Gizli sunucusu (şirket içi) yapılandırma](./media/secretserver-on-premises-tutorial/tutorial_secretserver_configure.png)

1. Çoklu oturum açmayı yapılandırma **gizli sunucusu (şirket içi)** tarafı, indirilen göndermek için ihtiyacınız **Certificate(Base64), oturum kapatma URL'si SAML çoklu oturum açma hizmeti URL'si**, ve **SAML varlık Kimliği** için [gizli sunucusu (şirket içi) destek ekibi](https://thycotic.force.com/support/s/). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/secretserver-on-premises-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/secretserver-on-premises-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/secretserver-on-premises-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/secretserver-on-premises-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-secret-server-on-premises-test-user"></a>Gizli dizi sunucusu (şirket içi) test kullanıcısı oluşturma

Bu bölümde, Britta Simon gizli sunucusu (şirket içi) adlı bir kullanıcı oluşturun. Çalışmak [gizli sunucusu (şirket içi) destek ekibi](https://thycotic.force.com/support/s/) gizli sunucusu (şirket içi) platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma gizli anahtarı sunucusuna (şirket içi) erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Britta Simon gizli sunucusuna (şirket içi) atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

1. Uygulamalar listesinde **gizli sunucusu (şirket içi)**.

    ![Uygulamalar listesinde gizli sunucusu (şirket içi) bağlantısı](./media/secretserver-on-premises-tutorial/tutorial_secretserver_app.png)

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli gizli sunucusu (şirket içi) kutucuğa tıkladığınızda, size otomatik olarak gizli sunucusu (şirket içi) uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/secretserver-on-premises-tutorial/tutorial_general_01.png
[2]: ./media/secretserver-on-premises-tutorial/tutorial_general_02.png
[3]: ./media/secretserver-on-premises-tutorial/tutorial_general_03.png
[4]: ./media/secretserver-on-premises-tutorial/tutorial_general_04.png

[100]: ./media/secretserver-on-premises-tutorial/tutorial_general_100.png

[200]: ./media/secretserver-on-premises-tutorial/tutorial_general_200.png
[201]: ./media/secretserver-on-premises-tutorial/tutorial_general_201.png
[202]: ./media/secretserver-on-premises-tutorial/tutorial_general_202.png
[203]: ./media/secretserver-on-premises-tutorial/tutorial_general_203.png

