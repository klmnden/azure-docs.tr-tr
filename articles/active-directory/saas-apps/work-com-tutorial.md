---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Work.com | Microsoft Docs'
description: Azure Active Directory ve Work.com arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 98e6739e-eb24-46bd-9dd3-20b489839076
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 878ba9b5debd4c415a033ad5d885554f08185c1e
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54815676"
---
# <a name="tutorial-azure-active-directory-integration-with-workcom"></a>Öğretici: Work.com ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Work.com tümleştirme konusunda bilgi edinin.

Azure AD ile Work.com tümleştirme ile aşağıdaki avantajları sağlar:

- Work.com erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Work.com (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Work.com yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Work.com çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Work.com Ekle
1. Yapılandırma ve Azure AD çoklu oturum açmayı test etme

## <a name="add-workcom-from-the-gallery"></a>Galeriden Work.com Ekle
Azure AD'de Work.com tümleştirmesini yapılandırmak için Work.com Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Work.com eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Work.com**seçin **Work.com** Sonuçlar panelinde ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Galeriden Ekle](./media/work-com-tutorial/tutorial_work-com_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Work.com sınayın.

Tek iş için oturum açma için Azure AD ne Work.com karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Work.com ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Work.com içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Work.com ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Work.com test kullanıcısı oluşturma](#create-a-workcom-test-user)**  - kullanıcı Azure AD gösterimini bağlı Work.com Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Work.com uygulamanızda çoklu oturum açmayı yapılandırın.

>[!NOTE]
>Çoklu oturum açmayı yapılandırmak için bir özel Work.com etki alanı adı henüz Kurulum gerekir. En az bir etki alanı adı tanımlayın, etki alanı adınızı test etmek ve tüm kuruluşunuza dağıtmanız gerekir.

**Azure AD çoklu oturum açma ile Work.com yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Work.com** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![SAML Tabanlı Oturum Açma](./media/work-com-tutorial/tutorial_work-com_samlbase.png)

1. Üzerinde **Work.com etki alanı ve URL'ler** bölümünde, aşağıdaki işlemi gerçekleştirin:

    ![Work.com etki alanı ve URL'ler bölümü](./media/work-com-tutorial/tutorial_work-com_url.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `http://<companyname>.my.salesforce.com`

    > [!NOTE] 
    > Bu değer, gerçek değil. Bu değer, gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Work.com istemci Destek ekibine](https://help.salesforce.com/articleView?id=000159855&type=3) bu değeri alınamıyor. 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![SAML imzalama sertifikası bölümü](./media/work-com-tutorial/tutorial_work-com_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Kaydet Düğmesi](./media/work-com-tutorial/tutorial_general_400.png)

1. Üzerinde **Work.com yapılandırma** bölümünde **yapılandırma Work.com** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Work.com yapılandırma bölümü](./media/work-com-tutorial/tutorial_work-com_configure.png) 
1. Work.com Kiracı yönetici olarak oturum açın.

1. Git **Kurulum**.
   
    ![Kurulum](./media/work-com-tutorial/ic794108.png "Kurulumu")

1. Sol gezinti bölmesinde, içinde **Yönet** bölümünde **etki alanı yönetimi** ilgili bölümü genişletin ve ardından **My Domain** açmakiçin**My Domain** sayfası. 
   
    ![Etki alanım](./media/work-com-tutorial/ic767825.png "etki alanım")

1. Etki alanınızı doğru şekilde ayarlandığını gösterdiğinde olduğunu doğrulamak için içinde olduğundan emin olun "**4 adım dağıtılan kullanıcılara**" gözden geçirin, "**etki alanı ayarlarım**".
   
    ![Etki alanı kullanıcıya dağıtılan](./media/work-com-tutorial/ic784377.png "kullanıcıya dağıtılan etki alanı")

1. Work.com kiracınızda oturum açın.

1. Git **Kurulum**.
    
    ![Kurulum](./media/work-com-tutorial/ic794108.png "Kurulumu")

1. Genişletin **güvenlik denetimleri** menüsüne ve ardından **çoklu oturum açma ayarları**.
    
    ![Çoklu oturum açma ayarları](./media/work-com-tutorial/ic794113.png "çoklu oturum açma ayarları")

1. Üzerinde **çoklu oturum açma ayarları** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
    
    ![SAML etkin](./media/work-com-tutorial/ic781026.png "SAML etkin")
    
    a. Seçin **SAML etkin**.
    
    b. **Yeni**’ye tıklayın.

1. İçinde **SAML çoklu oturum açma ayarları** bölümünde, aşağıdaki adımları gerçekleştirin:
    
    ![Oturum açma SAML tek ayar](./media/work-com-tutorial/ic794114.png "oturum açma SAML tek ayar")
    
    a. İçinde **adı** metin yapılandırmanız için bir ad yazın.  
       
    > [!NOTE]
    > İçin bir değer sağlanması **adı** Otomatik Doldur **API adı** metin.
    
    b. İçinde **veren** metin değerini yapıştırın **SAML varlık kimliği** , Azure Portalı'ndan kopyaladığınız.
    
    c. Azure portalından indirilen sertifikayı karşıya yüklemek için tıklayın **Gözat**.
    
    d. İçinde **varlık kimliği** metin kutusuna `https://salesforce-work.com`.
    
    e. Olarak **SAML kimlik türü**seçin **onaylamayı içeren kullanıcı nesnesinden Federasyon kimliği**.
    
    f. Olarak **SAML kimlik konumu**seçin **kimliğidir konu deyiminin NameIdentfier öğesinde**.
    
    g. İçinde **kimlik sağlayıcısı oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız.

    h. İçinde **kimlik sağlayıcısı oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.
    
    i. Olarak **hizmet sağlayıcısı tarafından başlatılan bağlama isteği**seçin **HTTP Post**.
    
    j. **Kaydet**’e tıklayın.

1. Sol gezinti bölmesinde, Work.com Klasik Portalı'nda tıklatın **etki alanı yönetimi** ilgili bölümü genişletin ve ardından **My Domain** açmak için **My Domain** Sayfa. 
    
    ![Etki alanım](./media/work-com-tutorial/ic794115.png "etki alanım")

1. Üzerinde **My Domain** sayfasında **oturum açma sayfasında bulunan marka** bölümünde **Düzenle**.
    
    ![Oturum açma sayfası markalama](./media/work-com-tutorial/ic767826.png "oturum açma sayfası markalama")

1. Üzerinde **oturum açma sayfasında bulunan marka** sayfasında **kimlik doğrulama hizmeti** bölümü adı, **SAML SSO ayarlarını** görüntülenir. Seçin ve ardından **Kaydet**.
    
    ![Oturum açma sayfası markalama](./media/work-com-tutorial/ic784366.png "oturum açma sayfası markalama")

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/work-com-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Tüm kullanıcılar, kullanıcılar ve Gruplar ->](./media/work-com-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Ekle](./media/work-com-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Kullanıcı iletişim kutusu sayfası](./media/work-com-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-workcom-test-user"></a>Work.com test kullanıcısı oluşturma
Azure Active Directory Kullanıcıları oturum açabilmesi, bunlar için Work.com sağlanmalıdır. Work.com söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:
1. Work.com şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Git **Kurulum**.
   
    ![Kurulum](./media/work-com-tutorial/IC794108.png "Kurulumu")
1. Git **kullanıcıları yönetme \> kullanıcılar**.
   
    ![Kullanıcıları Yönet](./media/work-com-tutorial/IC784369.png "kullanıcıları yönetme")

1. Tıklayın **yeni kullanıcı**.
   
    ![Tüm kullanıcılar](./media/work-com-tutorial/IC794117.png "tüm kullanıcılar")

1. Kullanıcı düzenleme bölümünde, aşağıdaki adımlarda, geçerli bir Azure özniteliklerini gerçekleştirmek istediğiniz ilgili metin kutularına zbilgisayarlar AD hesabı:
   
    ![Kullanıcı düzenleme](./media/work-com-tutorial/ic794118.png "kullanıcı düzenleme")
   
    a. İçinde **ad** metin kutusuna **ad** kullanıcının **Britta**.
    
    b. İçinde **Soyadı** metin kutusuna **Soyadı** kullanıcının **Simon**.
    
    c. İçinde **diğer** metin kutusuna **adı** kullanıcının **BrittaS**.
    
    d. İçinde **e-posta** metin kutusuna **e-posta adresi** kullanıcının **Brittasimon@contoso.com**.
    
    e. İçinde **kullanıcı adı** metin kutusu, türü, bir kullanıcının kullanıcı adını ister **Brittasimon@contoso.com**.
    
    f. İçinde **takma ad** metin bir **takma ad** kullanıcının **Simon**.
    
    g. Seçin **rol**, **kullanıcı lisansı**, ve **profili**.
    
    h. **Kaydet**’e tıklayın.  
      
    > [!NOTE]
    > Azure AD hesap sahibinin etkin hale gelir önce hesabı onaylamak için bir bağlantı içeren bir e-posta alırsınız.
    > 
    > 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Work.com erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Work.com için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Work.com**.

    ![Uygulama listesinde Work.com](./media/work-com-tutorial/tutorial_work-com_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Work.com kutucuğa tıkladığınızda, otomatik olarak Work.com uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)


<!--Image references-->

[1]: ./media/work-com-tutorial/tutorial_general_01.png
[2]: ./media/work-com-tutorial/tutorial_general_02.png
[3]: ./media/work-com-tutorial/tutorial_general_03.png
[4]: ./media/work-com-tutorial/tutorial_general_04.png

[100]: ./media/work-com-tutorial/tutorial_general_100.png

[200]: ./media/work-com-tutorial/tutorial_general_200.png
[201]: ./media/work-com-tutorial/tutorial_general_201.png
[202]: ./media/work-com-tutorial/tutorial_general_202.png
[203]: ./media/work-com-tutorial/tutorial_general_203.png

