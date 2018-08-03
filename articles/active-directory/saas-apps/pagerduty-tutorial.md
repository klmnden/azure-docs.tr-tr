---
title: 'Öğretici: Azure Active Directory PagerDuty ile tümleştirme | Microsoft Docs'
description: Azure Active Directory ve PagerDuty arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 0410456a-76f7-42a7-9bb5-f767de75a0e0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: jeedes
ms.openlocfilehash: 0e571880d9893c0027c200c6f49dc704fea09ead
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39443707"
---
# <a name="tutorial-azure-active-directory-integration-with-pagerduty"></a>Öğretici: Azure Active Directory PagerDuty ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile PagerDuty tümleştirme konusunda bilgi edinin.

PagerDuty Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- PagerDuty erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için PagerDuty (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi PagerDuty ile yapılandırma için aşağıdakiler gerekir:

- Azure AD aboneliğiniz
- Abonelik bir PagerDuty çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. PagerDuty galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-pagerduty-from-the-gallery"></a>PagerDuty galeri ekleme
Azure AD'de PagerDuty tümleştirmesini yapılandırmak için PagerDuty Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden PagerDuty eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **PagerDuty**seçin **PagerDuty** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/pagerduty-tutorial/tutorial_pagerduty_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve PagerDuty "Britta Simon" adlı bir test kullanıcı tabanlı Azure AD çoklu oturum açmayı sınayın.

Tek çalışmak için oturum açma için Azure AD ne PagerDuty karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve PagerDuty ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

PagerDuty içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma PagerDuty ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[PagerDuty test kullanıcısı oluşturma](#create-a-pagerduty-test-user)**  - kullanıcı Azure AD gösterimini bağlı PagerDuty Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve PagerDuty uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma PagerDuty ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **PagerDuty** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/pagerduty-tutorial/tutorial_pagerduty_samlbase.png)

1. Üzerinde **PagerDuty etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![PagerDuty etki alanı ve URL'ler tek oturum açma bilgileri](./media/pagerduty-tutorial/tutorial_pagerduty_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<tenant-name>.pagerduty.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<tenant-name>.pagerduty.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [PagerDuty istemci Destek ekibine](https://www.pagerduty.com/support/) bu değerleri almak için.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/pagerduty-tutorial/tutorial_pagerduty_certificate.png)

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/pagerduty-tutorial/tutorial_general_400.png)

1. Üzerinde **PagerDuty yapılandırma** bölümünde **yapılandırma PagerDuty** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![PagerDuty yapılandırma](./media/pagerduty-tutorial/tutorial_pagerduty_configure.png)

1. Farklı bir web tarayıcı penceresinde Pagerduty şirket sitenize yönetici olarak oturum.

1. Üstteki menüden **hesap ayarları**.

    ![Hesap ayarları](./media/pagerduty-tutorial/ic778535.png "hesap ayarları")

1. Tıklayın **çoklu oturum açma**.

    ![Çoklu oturum açma](./media/pagerduty-tutorial/ic778536.png "çoklu oturum açma")

1. Üzerinde **etkinleştirme çoklu oturum açma (SSO)** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı etkinleştirme](./media/pagerduty-tutorial/ic778537.png "çoklu oturum açmayı etkinleştir")

    a. Not Defteri'nde Azure portalından indirilen, base-64 kodlanmış sertifika açın, içeriğini, panoya kopyalayın ve yapıştırın kendisine **X.509 sertifikası** metin kutusu
  
    b. İçinde **oturum açma URL'si** metin kutusu, yapıştırma **SAML çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız.
  
    c. İçinde **oturum kapatma URL'si** metin kutusu, yapıştırma **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.

    d. Seçin **izin kullanıcı adı/parola ile oturum açma**.

    e. Seçin **gerektiren tam kimlik doğrulaması bağlamı karşılaştırma** onay kutusu.

    f. Tıklayın **değişiklikleri kaydetmek**.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](./media/pagerduty-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/pagerduty-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Ekle düğmesi](./media/pagerduty-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Kullanıcı iletişim kutusu](./media/pagerduty-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-pagerduty-test-user"></a>PagerDuty test kullanıcısı oluşturma

PagerDuty için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların PagerDuty sağlanması gerekir.  
PagerDuty söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

>[!NOTE]
>Herhangi diğer Pagerduty kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri tarafından Pagerduty sağlamak için Azure Active Directory kullanıcı hesaplarını sağlanan.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **Pagerduty** Kiracı.

1. Üstteki menüden **kullanıcılar**.

1. Tıklayın **kullanıcı ekleme**.
   
    ![Kullanıcı ekleme](./media/pagerduty-tutorial/ic778539.png "kullanıcı ekleme")

1.  Üzerinde **takımınızın davet** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Takımınız davet](./media/pagerduty-tutorial/ic778540.png "takımınızın davet edin")

    a. Tür **ilk ve son adı** gibi kullanıcının **Britta Simon**. 
   
    b. Girin **e-posta** istediğiniz kullanıcının adresini **brittasimon@contoso.com**.
   
    c. Tıklayın **Ekle**ve ardından **Davet Gönder**.
   
    >[!NOTE]
    >Eklenen tüm kullanıcılar, bir PagerDuty hesabı oluşturmak için bir davet alırsınız.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, PagerDuty için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Britta Simon PagerDuty için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **PagerDuty**.

    ![Uygulamalar listesinde PagerDuty bağlantı](./media/pagerduty-tutorial/tutorial_pagerduty_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

' A tıkladığınızda erişim Panelyou PagerDuty kutucukta otomatik olarak PagerDuty uygulamanıza açan.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/pagerduty-tutorial/tutorial_general_01.png
[2]: ./media/pagerduty-tutorial/tutorial_general_02.png
[3]: ./media/pagerduty-tutorial/tutorial_general_03.png
[4]: ./media/pagerduty-tutorial/tutorial_general_04.png

[100]: ./media/pagerduty-tutorial/tutorial_general_100.png

[200]: ./media/pagerduty-tutorial/tutorial_general_200.png
[201]: ./media/pagerduty-tutorial/tutorial_general_201.png
[202]: ./media/pagerduty-tutorial/tutorial_general_202.png
[203]: ./media/pagerduty-tutorial/tutorial_general_203.png
