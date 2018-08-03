---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Asana | Microsoft Docs'
description: Azure Active Directory ve Asana arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 837e38fe-8f55-475c-87f4-6394dc1fee2b
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: jeedes
ms.openlocfilehash: f5ea7a330891d4befeb6388bbe7f37b2a4aa848f
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39438217"
---
# <a name="tutorial-azure-active-directory-integration-with-asana"></a>Öğretici: Azure Active Directory Asana ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Asana tümleştirme konusunda bilgi edinin.

Azure AD ile Asana tümleştirme ile aşağıdaki avantajları sağlar:

- Asana erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Asana (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Asana yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir Asana çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin.
Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Asana ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-asana-from-the-gallery"></a>Galeriden Asana ekleme
Azure AD'de Asana tümleştirmesini yapılandırmak için Asana Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Asana eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Asana**seçin **Asana** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/asana-tutorial/tutorial_asana_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı Asana ile test etme

Tek çalışmak için oturum açma için Azure AD ne asana karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı asana arasında bir bağlantı ilişki kurulması gerekir.

Asana'da, değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Asana ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bir Asana test kullanıcısı oluşturma](#create-an-asana-test-user)**  - kullanıcı Azure AD gösterimini bağlı asana Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Asana uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Asana yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Asana** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/asana-tutorial/tutorial_asana_samlbase.png)

1. Üzerinde **Asana etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Oturum açma bilgileri tek bir Asana etki alanı ve URL'ler](./media/asana-tutorial/tutorial_asana_url.png)

    a. İçinde **oturum açma URL'si** metin türü URL'si: `https://app.asana.com/`

    b. İçinde **tanımlayıcı** metin tür değeri: `https://app.asana.com/`

1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/asana-tutorial/tutorial_asana_certificate.png)

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/asana-tutorial/tutorial_general_400.png)

1. Üzerinde **Asana yapılandırma** bölümünde **yapılandırma Asana** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Asana yapılandırma](./media/asana-tutorial/tutorial_asana_configure.png)

1. Farklı bir tarayıcı penceresinde bir Asana uygulamanıza oturum. Asana'da SSO yapılandırmak için çalışma alanı ayarlarını, ekranın sağ üst köşesinde çalışma alanının adına tıklayarak erişin. Ardından  **\<çalışma alanınızın adının\> ayarları**.

    ![Asana sso ayarları](./media/asana-tutorial/tutorial_asana_09.png)

1. Üzerinde **kuruluş ayarlarına** penceresinde tıklayın **Yönetim**. ' A tıklayarak **üyeleri gerekir oturum SAML** SSO yapılandırmasını etkinleştirmek için. Aşağıdaki gerçekleştirme adımları:

    ![Çoklu oturum açma kuruluş ayarlarını yapılandırma](./media/asana-tutorial/tutorial_asana_10.png)  

     a. İçinde **oturum açma sayfası URL'si** metin kutusu, yapıştırma **SAML çoklu oturum açma hizmeti URL'si**.

     b. Azure portalından indirdiğiniz sertifika sağ tıklayın ve ardından sertifika dosyasını Not Defteri'nde veya tercih edilen metin düzenleyiciyi kullanarak açın. Başlangıç ve bitiş sertifika başlığı arasında içeriği kopyalayın ve yapıştırın **X.509 sertifikası** metin.

1. **Kaydet**’e tıklayın. Git [SSO'yu ayarlama için Asana Kılavuzu](https://asana.com/guide/help/premium/authentication#gl-saml) daha fazla yardıma ihtiyacınız varsa.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](./media/asana-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/asana-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/asana-tutorial/create_aaduser_03.png)

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Ekle düğmesi](./media/asana-tutorial/create_aaduser_04.png)

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.

### <a name="create-an-asana-test-user"></a>Bir Asana test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon Asana'da adlı bir kullanıcı oluşturmaktır. Asana otomatik kullanıcı hazırlama, varsayılan olarak etkin olan destekler. Daha fazla ayrıntı bulabilirsiniz [burada](asana-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

**Kullanıcı el ile oluşturmanız gerekiyorsa, lütfen aşağıdaki adımları uygulayın:**

Bu bölümde, Britta Simon Asana'da adlı bir kullanıcı oluşturun.

1. Üzerinde **Asana**Git **takımlar** bölümünde Sol paneldeki. Artı işareti düğmesine tıklayın.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/asana-tutorial/tutorial_asana_12.png)

1. E-postasını yazmalı britta.simon@contoso.com metin kutusuna ve ardından **davet**.

1. Tıklayın **Davet Gönder**. Yeni kullanıcı, kendi e-posta hesabına bir e-posta alırsınız. Filiz, oluşturun ve hesabı doğrulamak gerekir.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açmayı kullanmak için Asana erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Britta Simon için Asana atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

1. Uygulamalar listesinde **Asana**.

    ![Uygulamalar listesinde Asana bağlantı](./media/asana-tutorial/tutorial_asana_app.png)

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, Azure AD çoklu oturum açmayı test etmektir.

Asana oturum açma sayfasına gidin. E-posta adresi metin kutusuna e-posta adresi eklemek britta.simon@contoso.com. Parola metin kutusunu boş bırakın ve ardından **oturum**. Azure AD oturum açma sayfasına yönlendirilirsiniz. Azure AD kimlik bilgilerinizi doldurun. Şimdi, Asana üzerinde oturum açtınız.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Kullanıcı sağlamayı yapılandırma](asana-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/asana-tutorial/tutorial_general_01.png
[2]: ./media/asana-tutorial/tutorial_general_02.png
[3]: ./media/asana-tutorial/tutorial_general_03.png
[4]: ./media/asana-tutorial/tutorial_general_04.png

[100]: ./media/asana-tutorial/tutorial_general_100.png

[200]: ./media/asana-tutorial/tutorial_general_200.png
[201]: ./media/asana-tutorial/tutorial_general_201.png
[202]: ./media/asana-tutorial/tutorial_general_202.png
[203]: ./media/asana-tutorial/tutorial_general_203.png
[10]: ./media/asana-tutorial/tutorial_general_060.png
[11]: ./media/asana-tutorial/tutorial_general_070.png