---
title: 'Öğretici: Azure Active Directory Tümleştirme ile yakınlaştırma | Microsoft Docs'
description: Azure Active Directory ve yakınlaştırma arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0ebdab6c-83a8-4737-a86a-974f37269c31
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/28/2017
ms.author: jeedes
ms.openlocfilehash: 57ae31245a356a4cd5769fe71ef471922bf6faf9
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39440143"
---
# <a name="tutorial-azure-active-directory-integration-with-zoom"></a>Öğretici: Azure Active Directory Tümleştirme ile yakınlaştırma

Bu öğreticide, Azure Active Directory (Azure AD) ile yakınlaştırma tümleştirme konusunda bilgi edinin.

Yakınlaştırma Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Yakınlaştırma erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan (çoklu oturum açma) yakınlaştırmak için Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile yakınlaştırma yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik bir yakınlaştırma çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Yakınlaştırma galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-zoom-from-the-gallery"></a>Yakınlaştırma galeri ekleme
Azure AD yakınlaşın tümleştirmesini yapılandırmak için yakınlaştırma Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden yakınlaştırma eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **yakınlaştırma**seçin **yakınlaştırma** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuçlar listesinde Yakınlaştır](./media/zoom-tutorial/tutorial_zoom_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı yakınlaştırma sınayın.

Tek çalışmak için oturum açma için Azure AD ne yakınlaştırma karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcının yakınlaştırma arasında bir bağlantı ilişki kurulması gerekir.

Yakınlaştırma, değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma yakınlaştırma ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Yakınlaştırma test kullanıcısı oluşturma](#create-a-zoom-test-user)**  - kullanıcı Azure AD gösterimini bağlı yakınlaştırma Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve yakınlaştırma uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile yakınlaştırma yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **yakınlaştırma** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/zoom-tutorial/tutorial_zoom_samlbase.png)

1. Üzerinde **yakınlaştırma etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Etki alanı ve URL'ler tek oturum açma bilgileri Yakınlaştır](./media/zoom-tutorial/tutorial_zoom_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.zoom.us`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `<companyname>.zoom.us`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [yakınlaştırma istemci Destek ekibine](https://support.zoom.us/hc) bu değerleri almak için.

1. Yakınlaştırma uygulama, özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirme sayfasında bölümü. 

    ![Çoklu oturum açmayı yapılandırın](./media/zoom-tutorial/tutorial_attribute.png)

1. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, önceki görüntüde gösterildiği gibi SAML belirteci özniteliği yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri | Namespace değeri |
    | ------------------- | -----------|--------- |    
    | E-posta adresi | User.Mail | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/mail`|
    | Ad | User.givenName | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`|
    | Soyadı | User.surname | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname `|
    | Telefon numarası | User.telephoneNumber | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/phone`|
    | Bölüm | User.Department | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/department`|

    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/zoom-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/zoom-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. İçinde **Namespace** metin kutusuna, bu satır için gösterilen ad alanı değeri yazın.
    
    e. **Tamam**’a tıklayın. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/zoom-tutorial/tutorial_zoom_certificate.png)

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/zoom-tutorial/tutorial_general_400.png)

1. Üzerinde **yakınlaştırma yapılandırma** bölümünde **yapılandırma yakınlaştırma** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Yakınlaştırma yapılandırma](./media/zoom-tutorial/tutorial_zoom_configure.png)

1. Farklı bir web tarayıcı penceresinde yakınlaştırma şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Tıklayın **çoklu oturum açma** sekmesi.
   
    ![Çoklu oturum açma sekmesini](./media/zoom-tutorial/IC784700.png "çoklu oturum açma")

1. Tıklayın **güvenlik denetimi** sekmesine ve ardından Git **çoklu oturum açma** ayarları.

1. Çoklu oturum açma bölümünde aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma bölümüne](./media/zoom-tutorial/IC784701.png "çoklu oturum açma")
   
    a. İçinde **oturum açma sayfası URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız.
   
    b. İçinde **sayfa oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.
     
    c. Base-64 kodlanmış sertifikanızı Not Defteri'nde açın, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **kimlik sağlayıcısı sertifikası** metin.

    d. İçinde **veren** metin değerini yapıştırın **SAML varlık kimliği** , Azure Portalı'ndan kopyaladığınız. 

    e. **Kaydet**’e tıklayın.

    > [!NOTE] 
    > Daha fazla bilgi için yakınlaştırma belgeleri sayfasını ziyaret edin. [https://zoomus.zendesk.com/hc/en-us/articles/115005887566](https://zoomus.zendesk.com/hc/en-us/articles/115005887566)

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/zoom-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/zoom-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/zoom-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/zoom-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-zoom-test-user"></a>Yakınlaştırma test kullanıcısı oluşturma

Yakınlaştırmak için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar yakınlaşın sağlanması gerekir. Yakınlaştırma söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:

1. Oturum açın, **yakınlaştırma** yönetici olarak şirketin site.
 
1. Tıklayın **hesap yönetimi** sekmesine ve ardından **kullanıcı yönetimi**.

1. Kullanıcı Yönetimi bölümünde tıklayın **kullanıcı ekleme**.
   
    ![Kullanıcı Yönetimi](./media/zoom-tutorial/IC784703.png "kullanıcı yönetimi")

1. Üzerinde **kullanıcı ekleme** sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcı ekleme](./media/zoom-tutorial/IC784704.png "kullanıcı ekleme")
   
    a. Olarak **kullanıcı türü**seçin **temel**.

    b. İçinde **e-postaları** metin kutusu, türü e-posta adresi geçerli bir Azure ad hesabına sağlamak istiyor.

    c. **Ekle**'ye tıklayın.

> [!NOTE]
> Herhangi diğer yakınlaştırma kullanıcı hesabı oluşturma araçları kullanabilir veya kullanıcı hesapları yakınlaştırma sağlama Azure Active Directory tarafından sağlanan API'leri.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, yakınlaştırmak için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon yakınlaştırmak için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **yakınlaştırma**.

    ![Uygulamaları listeden yakınlaştırma bağlantıyı](./media/zoom-tutorial/tutorial_zoom_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test sağlamaktır.

Erişim panelinde yakınlaştırma kutucuğa tıkladığınızda, otomatik olarak yakınlaştırma uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/zoom-tutorial/tutorial_general_01.png
[2]: ./media/zoom-tutorial/tutorial_general_02.png
[3]: ./media/zoom-tutorial/tutorial_general_03.png
[4]: ./media/zoom-tutorial/tutorial_general_04.png

[100]: ./media/zoom-tutorial/tutorial_general_100.png

[200]: ./media/zoom-tutorial/tutorial_general_200.png
[201]: ./media/zoom-tutorial/tutorial_general_201.png
[202]: ./media/zoom-tutorial/tutorial_general_202.png
[203]: ./media/zoom-tutorial/tutorial_general_203.png

