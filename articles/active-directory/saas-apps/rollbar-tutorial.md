---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Rollbar | Microsoft Docs'
description: Azure Active Directory ve Rollbar arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 57537e54-9388-4272-a610-805ce45a451f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/04/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4ebedddf5e62e41bd0ef2d4c426df448a81a1e2d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60349798"
---
# <a name="tutorial-azure-active-directory-integration-with-rollbar"></a>Öğretici: Rollbar ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Rollbar tümleştirme konusunda bilgi edinin.

Azure AD ile Rollbar tümleştirme ile aşağıdaki avantajları sağlar:

- Rollbar erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için Rollbar (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Rollbar yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Rollbar çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Rollbar ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-rollbar-from-the-gallery"></a>Galeriden Rollbar ekleme
Azure AD'de Rollbar tümleştirmesini yapılandırmak için Rollbar Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Rollbar eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Rollbar**seçin **Rollbar** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Rollbar](./media/rollbar-tutorial/tutorial_rollbar_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Rollbar sınayın.

Tek iş için oturum açma için Azure AD ne Rollbar karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Rollbar ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Rollbar içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Rollbar ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Rollbar test kullanıcısı oluşturma](#create-a-rollbar-test-user)**  - kullanıcı Azure AD gösterimini bağlı Rollbar Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Rollbar uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Rollbar yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Rollbar** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/rollbar-tutorial/tutorial_rollbar_samlbase.png)

1. Üzerinde **Rollbar etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![Rollbar etki alanı ve URL'ler tek oturum açma bilgileri](./media/rollbar-tutorial/tutorial_rollbar_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna URL'yi yazın: `https://saml.rollbar.com`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://rollbar.com/<accountname>/saml/sso/azure/`

1. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Rollbar etki alanı ve URL'ler tek oturum açma bilgileri](./media/rollbar-tutorial/tutorial_rollbar_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://rollbar.com/<accountname>/saml/login/azure/`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [Rollbar istemci Destek ekibine](mailto:support@rollbar.com) bu değerleri almak için. 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/rollbar-tutorial/tutorial_rollbar_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/rollbar-tutorial/tutorial_general_400.png)
    
1. Farklı bir web tarayıcı penceresinde Rollbar şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Tıklayarak **profil ayarları** 'a tıklayın ve sağ üst köşedeki **hesap adı ayarları**.
    
    ![Yapılandırma](./media/rollbar-tutorial/general.png)

1. Tıklayın **kimlik sağlayıcısı** güvenlik altında.

    ![Yapılandırma](./media/rollbar-tutorial/configure1.png)

1. İçinde **SAML kimlik sağlayıcısı** bölümünde, aşağıdaki adımları gerçekleştirin:
    
    ![Yapılandırma](./media/rollbar-tutorial/configure2.png)

    a. Seçin **AZURE** gelen **SAML kimlik sağlayıcısı** açılır.

    b. Meta veri dosyasını Not Defteri'nde açın, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **SAML meta veri** metin.

    c. **Kaydet**’e tıklayın.

1. Kaydet'e tıkladığınızda sonra düğme, ekranın olacaktır şöyle:
    
    ![Yapılandırma](./media/rollbar-tutorial/configure3.png)
    > [!NOTE] 
    > Aşağıdaki adımı tamamlamak için önce kendiniz bir kullanıcı olarak azure'da Rollbar uygulaması eklemeniz gerekir.
    a. Azure ile kimlik doğrulaması'a tıklayın, tüm kullanıcıların istiyorsanız **kimlik sağlayıcınız oturum** Azure aracılığıyla yeniden kimlik doğrulaması.  

    b.  Ekranına dönersiniz sonra seçin **SAML kimlik sağlayıcısı ile oturum açmasını gerektiren** onay kutusu.

    b. **Kaydet**’e tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/rollbar-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/rollbar-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/rollbar-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/rollbar-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-rollbar-test-user"></a>Rollbar test kullanıcısı oluşturma

Rollbar için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların Rollbar sağlanması gerekir. Rollbar söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Rollbar şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Tıklayarak **profil ayarları** 'a tıklayın ve sağ üst köşedeki **hesap adı ayarları**.

    ![Kullanıcı](./media/rollbar-tutorial/general.png)

1. **Kullanıcılar**’a tıklayın.
    
    ![Çalışan Ekle](./media/rollbar-tutorial/user1.png)

1. Tıklayın **takım üyelerini davet**.

    ![Kişileri davet edin](./media/rollbar-tutorial/user2.png)

1. Metin kutusunda gibi kullanıcı adını girin **brittasimon\@contoso.com** tıklayarak **Ekle/davet**.

    ![Kişileri davet edin](./media/rollbar-tutorial/user3.png)

1. Kullanıcı Davet alır ve kabul ettikten sonra derse sistemde oluşturdunuz.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Rollbar erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Rollbar için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Rollbar**.

    ![Uygulamalar listesinde Rollbar bağlantı](./media/rollbar-tutorial/tutorial_rollbar_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Rollbar kutucuğa tıkladığınızda, otomatik olarak Rollbar uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/rollbar-tutorial/tutorial_general_01.png
[2]: ./media/rollbar-tutorial/tutorial_general_02.png
[3]: ./media/rollbar-tutorial/tutorial_general_03.png
[4]: ./media/rollbar-tutorial/tutorial_general_04.png

[100]: ./media/rollbar-tutorial/tutorial_general_100.png

[200]: ./media/rollbar-tutorial/tutorial_general_200.png
[201]: ./media/rollbar-tutorial/tutorial_general_201.png
[202]: ./media/rollbar-tutorial/tutorial_general_202.png
[203]: ./media/rollbar-tutorial/tutorial_general_203.png

