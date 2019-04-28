---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle TOPdesk - Public | Microsoft Docs'
description: Azure Active Directory ve TOPdesk - genel arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 0873299f-ce70-457b-addc-e57c5801275f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 89412040fdea32746574d8ae5bada9c017617b80
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62129305"
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---public"></a>Öğretici: Azure Active Directory Tümleştirmesi ile TOPdesk - genel

Bu öğreticide, Azure Active Directory (Azure AD) ile genel TOPdesk - tümleştirme konusunda bilgi edinin.

TOPdesk - genel Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- TOPdesk - genel erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için TOPdesk - Azure AD hesaplarına (çoklu oturum açma) genel açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile TOPdesk - yapılandırmak için genel, aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- TOPdesk - genel çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Genel Galeriden TOPdesk - ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-topdesk---public-from-the-gallery"></a>Genel Galeriden TOPdesk - ekleme
TOPdesk - genel Azure AD'ye tümleştirmesini yapılandırmak için genel Galeriden listenize yönetilen SaaS uygulamaları - TOPdesk eklemeniz gerekir.

**TOPdesk - genel Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **TOPdesk - genel**seçin **TOPdesk - genel** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![TOPdesk - genel sonuçları listesinde](./media/topdesk-public-tutorial/tutorial_topdesk-public_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma TOPdesk ile test etme - Public "Britta Simon" adlı bir test kullanıcı tabanlı.

Tek çalışmak, oturum için Azure AD'ye TOPdesk içinde karşılık gelen kullanıcının bilmesi gerekir - genel bir kullanıcının Azure AD'de. Diğer bir deyişle, bir Azure AD kullanıcısının TOPdesk - ilgili kullanıcı arasında bir bağlantı ilişki genel kurulması gerekir.

TOPdesk içinde-ortak değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma TOPdesk ile-test etmek için genel, aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[TOPdesk - genel bir test kullanıcısı oluşturma](#create-a-topdesk---public-test-user)**  - TOPdesk - kullanıcı Azure AD gösterimini bağlı olduğu ortak bir karşılığı Britta simon'un sağlamak için.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, TOPdesk - genel uygulama yapılandırın.

**Azure AD çoklu oturum açma - TOPdesk ile yapılandırmak için genel, aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **TOPdesk - genel** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/topdesk-public-tutorial/tutorial_topdesk-public_samlbase.png)

1. Üzerinde **TOPdesk - ortak etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma bilgileri TOPdesk - ortak etki alanı ve URL'ler](./media/topdesk-public-tutorial/tutorial_topdesk-public_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.topdesk.net`
    
    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.topdesk.net/tas/public/login/verify`

    c. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.topdesk.net/tas/public/login/saml`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Yanıt URL'si açıklaması öğreticinin sonraki bölümlerinde ' dir. İlgili kişi [TOPdesk - genel istemci Destek ekibine](https://help.topdesk.com/saas/enterprise/user/) bu değerleri almak için.  

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/topdesk-public-tutorial/tutorial_topdesk-public_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/topdesk-public-tutorial/tutorial_general_400.png)
    
1. Üzerinde **TOPdesk - genel yapılandırması** bölümünde **TOPdesk - ortak yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![TOPdesk - genel yapılandırma](./media/topdesk-public-tutorial/tutorial_topdesk-public_configure.png) 

1. Oturum açın, **TOPdesk - genel** yönetici olarak şirketin site.

1. İçinde **TOPdesk** menüsünü tıklatın **ayarları**.
   
    ![Ayarları](./media/topdesk-public-tutorial/ic790598.png "ayarları")

1. Tıklayın **oturum açma ayarları**.
   
    ![Oturum açma ayarları](./media/topdesk-public-tutorial/ic790599.png "oturum açma ayarları")

1. Genişletin **oturum açma ayarları** menüsüne ve ardından **genel**.
   
    ![Genel](./media/topdesk-public-tutorial/ic790600.png "genel")

1. İçinde **genel** bölümünü **SAML oturum açma** yapılandırma bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Teknik ayarlarla](./media/topdesk-public-tutorial/ic790601.png "teknik ayarları")
   
    a. Tıklayın **indirme** ortak meta veri dosyası indirin ve bilgisayarınıza yerel olarak kaydedin.
   
    b. İndirilen meta veri dosyası açın ve ardından bulun **AssertionConsumerService** düğümü.

    ![AssertionConsumerService](./media/topdesk-public-tutorial/ic790619.png "AssertionConsumerService")
   
    c. Kopyalama **AssertionConsumerService** değeri, bu değeri yapıştırın **yanıt URL'si** metin kutusunda **TOPdesk - ortak etki alanı ve URL'ler** bölümü.      
   
1. Bir sertifika dosyası oluşturmak için aşağıdaki adımları gerçekleştirin:
    
    ![Sertifika](./media/topdesk-public-tutorial/ic790606.png "sertifika")
    
    a. Azure Portalı'ndan indirilen meta veri dosyası açın.
    
    b. Genişletin **verilerde Securitytokenservicetype** sahip düğüm bir **xsi: type** , **beslenir: ApplicationServiceType**.
    
    c. Değerini kopyalayın **X509Certificate** düğümü.
    
    d. Kopyalanan Kaydet **X509Certificate** yerel olarak bilgisayarınızda bir dosyadaki değeri.

1. İçinde **genel** bölümünde **Ekle**.
    
    ![SAML oturum açma](./media/topdesk-public-tutorial/ic790625.png "SAML oturum açma")

1. Üzerinde **SAML yapılandırma Yardımcısı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
    
    ![SAML yapılandırma Yardımcısı](./media/topdesk-public-tutorial/ic790608.png "SAML yapılandırma Yardımcısı")
    
    a. Azure portalından indirilen meta verileri dosyanızı altında karşıya yüklemek için **Federasyon meta verileri**, tıklayın **Gözat**.

    b. Altında sertifika dosyası karşıya **sertifika (RSA)**, tıklayın **Gözat**.

    c. Aldığınız TOPdesk destek ekibinden altında logosu dosyayı karşıya yüklemeyi **logosu simgesi**, tıklayın **Gözat**.

    d. İçinde **kullanıcı adı özniteliği** metin kutusuna `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    e. İçinde **görünen adı** metin yapılandırmanız için bir ad yazın.

    f. **Kaydet**’e tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/topdesk-public-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/topdesk-public-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/topdesk-public-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/topdesk-public-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-topdesk---public-test-user"></a>TOPdesk - genel bir test kullanıcısı oluşturma

Azure AD TOPdesk - oturum açmasına olanak tanımak ortak, bunlar sağlanmalıdır TOPdesk - genel.  
TOPdesk - söz konusu olduğunda genel, el ile bir görev olduğundan sağlama.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:
1. Oturum açın, **TOPdesk - genel** şirketinizin sitesi yöneticisi olarak.

1. Üstteki menüden **TOPdesk \> yeni \> destek dosyalarını \> kişi**.
   
    ![Kişi](./media/topdesk-public-tutorial/ic790628.png "kişi")

1. Yeni bir kişiye iletişim kutusunda aşağıdaki adımları gerçekleştirin:
   
    ![Yeni bir kişiye](./media/topdesk-public-tutorial/ic790629.png "yeni kişi")
   
    a. Genel sekmesini tıklatın.

    b. İçinde **Soyadı** metin Simon gibi kullanıcının soyadı yazın
 
    c. Seçin bir **Site** hesabı.
 
    d. **Kaydet**’e tıklayın.

> [!NOTE]
> Herhangi diğer TOPdesk - genel bir kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri TOPdesk - Azure AD kullanıcı hesapları sağlamak için ortak tarafından sağlanan.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma TOPdesk - genel erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon - TOPdesk için atanacak genel, aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **TOPdesk - genel**.

    ![Genel bağlantı TOPdesk - uygulamalar listesinde](./media/topdesk-public-tutorial/tutorial_topdesk-public_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

TOPdesk tıklayın - genel erişim Paneli'nde döşeme sonra otomatik olarak, TOPdesk - genel uygulama açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/topdesk-public-tutorial/tutorial_general_01.png
[2]: ./media/topdesk-public-tutorial/tutorial_general_02.png
[3]: ./media/topdesk-public-tutorial/tutorial_general_03.png
[4]: ./media/topdesk-public-tutorial/tutorial_general_04.png

[100]: ./media/topdesk-public-tutorial/tutorial_general_100.png

[200]: ./media/topdesk-public-tutorial/tutorial_general_200.png
[201]: ./media/topdesk-public-tutorial/tutorial_general_201.png
[202]: ./media/topdesk-public-tutorial/tutorial_general_202.png
[203]: ./media/topdesk-public-tutorial/tutorial_general_203.png

