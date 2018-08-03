---
title: 'Öğretici: Azure Active Directory OneTrust gizlilik yönetim yazılımı ile tümleştirme | Microsoft Docs'
description: Azure Active Directory ve OneTrust gizlilik yönetim yazılımı arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 71c2b6d0-3d28-4130-a2c8-1e72ab3d5814
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: jeedes
ms.openlocfilehash: f8e06a4578d2f11331b87fdfb493e2bba4edb8cf
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39421699"
---
# <a name="tutorial-azure-active-directory-integration-with-onetrust-privacy-management-software"></a>Öğretici: Azure Active Directory tümleştirmesiyle OneTrust gizlilik yönetim yazılımı

Bu öğreticide, Azure Active Directory (Azure AD) ile OneTrust gizlilik yönetim yazılımı tümleştirme konusunda bilgi edinin.

OneTrust gizlilik yönetim yazılımı, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Erişimi OneTrust gizlilik yönetim yazılımı, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan OneTrust gizlilik yönetim yazılımı (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi OneTrust gizlilik yönetim yazılımı ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir OneTrust gizlilik yönetim yazılımı çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden OneTrust gizlilik yönetim yazılımı ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-onetrust-privacy-management-software-from-the-gallery"></a>Galeriden OneTrust gizlilik yönetim yazılımı ekleme
Azure AD'de OneTrust gizlilik yönetim yazılımı tümleştirmesini yapılandırmak için OneTrust gizlilik yönetim yazılımı Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden OneTrust gizlilik yönetim yazılımının eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **OneTrust gizlilik yönetim yazılımı**seçin **OneTrust gizlilik yönetim yazılımı** sonucu panelinden ardından **Ekle** düğme eklemek için uygulama.

    ![Sonuç listesinde OneTrust gizlilik yönetim yazılımı](./media/onetrust-tutorial/tutorial_onetrust_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı OneTrust gizlilik yönetim yazılımı ile test edin.

Tek iş için oturum açma için Azure AD ne OneTrust gizlilik yönetim yazılımı karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı OneTrust gizlilik yönetim yazılımı arasında bir bağlantı ilişki kurulması gerekir.

Değerini OneTrust gizlilik yönetimi yazılımı Ata **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma OneTrust gizlilik yönetim yazılımı ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Gizlilik Yönetimi yazılımını OneTrust test kullanıcısı oluşturma](#create-a-onetrust-privacy-management-software-test-user)**  - kullanıcı Azure AD gösterimini bağlı OneTrust gizlilik yönetim yazılımı Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve OneTrust gizlilik yönetim yazılımı uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma OneTrust gizlilik yönetim yazılımı ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **OneTrust gizlilik yönetim yazılımı** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/onetrust-tutorial/tutorial_onetrust_samlbase.png)

1. Üzerinde **OneTrust gizlilik yönetim yazılımı etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![OneTrust gizlilik yönetim yazılımı etki alanı ve URL'ler tek oturum açma bilgileri](./media/onetrust-tutorial/tutorial_onetrust_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL: `https://www.onetrust.com/saml2`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.onetrust.com/auth/consumerservice`

1. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![OneTrust gizlilik yönetim yazılımı etki alanı ve URL'ler tek oturum açma bilgileri](./media/onetrust-tutorial/tutorial_onetrust_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.onetrust.com/auth/login`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [OneTrust gizlilik Yönetimi yazılımını Destek ekibine](mailto:support@onetrust.com) bu değerleri almak için. 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/onetrust-tutorial/tutorial_onetrust_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/onetrust-tutorial/tutorial_general_400.png)

1. Çoklu oturum açmayı yapılandırma **OneTrust gizlilik yönetim yazılımı** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** için [OneTrust gizlilik yönetim yazılımı Destek ekibine](mailto:support@onetrust.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/onetrust-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/onetrust-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/onetrust-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/onetrust-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-onetrust-privacy-management-software-test-user"></a>Gizlilik Yönetimi yazılımını OneTrust test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon OneTrust gizlilik yönetim yazılımı adlı bir kullanıcı oluşturmaktır. Gizlilik yönetimi yazılımı OneTrust tam zamanında sağlama, varsayılan olarak etkin olan destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı, henüz yoksa OneTrust gizlilik yönetim yazılımı erişme denemesi sırasında oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa başvurun [OneTrust gizlilik yönetim yazılımı Destek ekibine](mailto:support@onetrust.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma OneTrust gizlilik yönetim yazılımı erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon OneTrust gizlilik yönetim yazılımı atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **OneTrust gizlilik yönetim yazılımı**.

    ![Uygulamalar listesinde OneTrust gizlilik yönetim yazılımı bağlantı](./media/onetrust-tutorial/tutorial_onetrust_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde OneTrust gizlilik yönetim yazılımı kutucuğa tıkladığınızda, otomatik olarak OneTrust gizlilik yönetim yazılımı uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/onetrust-tutorial/tutorial_general_01.png
[2]: ./media/onetrust-tutorial/tutorial_general_02.png
[3]: ./media/onetrust-tutorial/tutorial_general_03.png
[4]: ./media/onetrust-tutorial/tutorial_general_04.png

[100]: ./media/onetrust-tutorial/tutorial_general_100.png

[200]: ./media/onetrust-tutorial/tutorial_general_200.png
[201]: ./media/onetrust-tutorial/tutorial_general_201.png
[202]: ./media/onetrust-tutorial/tutorial_general_202.png
[203]: ./media/onetrust-tutorial/tutorial_general_203.png

