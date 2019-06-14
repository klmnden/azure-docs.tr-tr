---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Jobscience | Microsoft Docs'
description: Azure Active Directory ve Jobscience arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 77282dcc-bbe2-4728-953d-adb4ab6a713b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8199f106c234e216a0982dc9e51413ccf30ae93a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60268705"
---
# <a name="tutorial-azure-active-directory-integration-with-jobscience"></a>Öğretici: Jobscience ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Jobscience tümleştirme konusunda bilgi edinin.

Azure AD ile Jobscience tümleştirme ile aşağıdaki avantajları sağlar:

- Jobscience erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Jobscience (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Jobscience yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Jobscience çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme burada alabilirsiniz: [Deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Jobscience ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-jobscience-from-the-gallery"></a>Galeriden Jobscience ekleme
Azure AD'de Jobscience tümleştirmesini yapılandırmak için Jobscience Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Jobscience eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Jobscience**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/jobscience-tutorial/tutorial_jobscience_search.png)

1. Sonuçlar panelinde seçin **Jobscience**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/jobscience-tutorial/tutorial_jobscience_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı Jobscience ile test etme

Tek iş için oturum açma için Azure AD ne Jobscience karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Jobscience ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Jobscience içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Jobscience ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Jobscience test kullanıcısı oluşturma](#creating-a-jobscience-test-user)**  - kullanıcı Azure AD gösterimini bağlı Jobscience Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Jobscience uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Jobscience yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Jobscience** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/jobscience-tutorial/tutorial_jobscience_samlbase.png)

1. Üzerinde **Jobscience etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/jobscience-tutorial/tutorial_jobscience_url.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak:  `http://<company name>.my.salesforce.com`
    
    > [!NOTE] 
    > Bu değer, gerçek değil. Bu değer, gerçek oturum açma URL'si ile güncelleştirin. Bu değer elde [Jobscience istemci Destek ekibine](https://www.jobscience.com/support) veya SSO profilinden, öğreticinin ilerleyen bölümlerinde açıklanan oluşturacaksınız. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/jobscience-tutorial/tutorial_jobscience_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/jobscience-tutorial/tutorial_general_400.png)

1. Üzerinde **Jobscience yapılandırma** bölümünde **yapılandırma Jobscience** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/jobscience-tutorial/tutorial_jobscience_configure.png) 

1. Jobscience şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Git **Kurulum**.
   
   ![Kurulum](./media/jobscience-tutorial/IC784358.png "Kurulumu")

1. Sol gezinti bölmesinde, içinde **Yönet** bölümünde **etki alanı yönetimi** ilgili bölümü genişletin ve ardından **My Domain** açmakiçin**My Domain** sayfası. 
   
   ![Etki alanım](./media/jobscience-tutorial/ic767825.png "etki alanım")

1. Etki alanınızı doğru şekilde ayarlandığını gösterdiğinde olduğunu doğrulamak için içinde olduğundan emin olun "**4 adım dağıtılan kullanıcılara**" gözden geçirin, "**etki alanı ayarlarım**".

    ![Etki alanı kullanıcıya dağıtılan](./media/jobscience-tutorial/ic784377.png "kullanıcıya dağıtılan etki alanı")

1. Jobscience şirket sitesinde tıklatın **güvenlik denetimleri**ve ardından **çoklu oturum açma ayarları**.
    
    ![Güvenlik denetimleri](./media/jobscience-tutorial/ic784364.png "güvenlik denetimleri")

1. İçinde **çoklu oturum açma ayarları** bölümünde, aşağıdaki adımları gerçekleştirin:
    
    ![Çoklu oturum açma ayarları](./media/jobscience-tutorial/ic781026.png "çoklu oturum açma ayarları")
    
    a. Seçin **SAML etkin**.

    b. **Yeni**’ye tıklayın.

1. Üzerinde **SAML çoklu oturum açma ayarı Düzenle** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
    
    ![Oturum açma SAML tek ayar](./media/jobscience-tutorial/ic784365.png "oturum açma SAML tek ayar")
    
    a. İçinde **adı** metin yapılandırmanız için bir ad yazın.

    b. İçinde **veren** metin değerini yapıştırın **SAML varlık kimliği**, hangi Azure Portalı'ndan kopyaladığınız.

    c. İçinde **varlık kimliği** metin kutusuna `https://salesforce-jobscience.com`

    d. Tıklayın **Gözat** Azure AD'ye sertifikanızı karşıya yüklemek için.

    e. Olarak **SAML kimlik türü**seçin **onaylamayı içeren kullanıcı nesnesinden Federasyon kimliği**.

    f. Olarak **SAML kimlik konumu**seçin **kimliğidir konu deyiminin NameIdentfier öğesinde**.

    g. İçinde **kimlik sağlayıcısı oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    h. İçinde **kimlik sağlayıcısı oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    i. **Kaydet**’e tıklayın.

1. Sol gezinti bölmesinde, içinde **Yönet** bölümünde **etki alanı yönetimi** ilgili bölümü genişletin ve ardından **My Domain** açmakiçin**My Domain** sayfası. 
    
    ![Etki alanım](./media/jobscience-tutorial/ic767825.png "etki alanım")

1. Üzerinde **My Domain** sayfasında **oturum açma sayfasında bulunan marka** bölümünde **Düzenle**.
    
    ![Oturum açma sayfası markalama](./media/jobscience-tutorial/ic767826.png "oturum açma sayfası markalama")

1. Üzerinde **oturum açma sayfasında bulunan marka** sayfasında **kimlik doğrulama hizmeti** bölümü adı, **SAML SSO ayarlarını** görüntülenir. Seçin ve ardından **Kaydet**.
    
    ![Oturum açma sayfası markalama](./media/jobscience-tutorial/ic784366.png "oturum açma sayfası markalama")

1. SP almak için çoklu oturum açma oturum açma URL'si tıklayarak başlatıldı: **çoklu oturum açma ayarları** içinde **güvenlik denetimleri** menü bölümü.

    ![Güvenlik denetimleri](./media/jobscience-tutorial/ic784368.png "güvenlik denetimleri")
    
    Yukarıdaki adımda oluşturduğunuz SSO profiline tıklayın. Bu sayfa çoklu oturum açma URL'SİNDE şirketiniz için gösterir (örneğin, [ https://companyname.my.salesforce.com?so=companyid ](https://companyname.my.salesforce.com?so=companyid).    

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/jobscience-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/jobscience-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/jobscience-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/jobscience-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-jobscience-test-user"></a>Jobscience test kullanıcısı oluşturma

Jobscience için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların Jobscience sağlanması gerekir. Jobscience söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

>[!NOTE]
>Herhangi diğer Jobscience kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri tarafından Jobscience sağlamak için Azure Active Directory kullanıcı hesaplarını sağlanan.
>  
        
**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **Jobscience** şirketinizin sitesi yöneticisi olarak.

1. Kurulum gidin.
   
   ![Kurulum](./media/jobscience-tutorial/ic784358.png "Kurulumu")
1. Git **kullanıcıları yönetme \> kullanıcılar**.
   
   ![Kullanıcılar](./media/jobscience-tutorial/ic784369.png "kullanıcılar")
1. Tıklayın **yeni kullanıcı**.
   
   ![Tüm kullanıcılar](./media/jobscience-tutorial/ic784370.png "tüm kullanıcılar")
1. Üzerinde **kullanıcı düzenleme** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
   ![Kullanıcı düzenleme](./media/jobscience-tutorial/ic784371.png "kullanıcı düzenleme")
   
   a. İçinde **ad** metin Britta gibi kullanıcının ilk adını yazın.
   
   b. İçinde **Soyadı** metin kutusu, kullanıcının Simon gibi bir soyadı yazın.
   
   c. İçinde **diğer** metin kutusu, kullanıcının brittas gibi bir ad yazın.

   d. İçinde **e-posta** metin kutusuna kullanıcı e-posta adresi türünü ister Brittasimon@contoso.com.

   e. İçinde **kullanıcı adı** metin kutusu, türü, bir kullanıcının kullanıcı adını ister Brittasimon@contoso.com.

   f. İçinde **takma ad** metin Simon gibi kullanıcı takma adını yazın.

   g. **Kaydet**’e tıklayın.

    
> [!NOTE]
> Azure Active Directory hesap sahibinin e-posta alır ve etkin hale gelir önce hesabını onaylamak için bir bağlantı izler.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Jobscience erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Jobscience için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Jobscience**.

    ![Çoklu oturum açmayı yapılandırın](./media/jobscience-tutorial/tutorial_jobscience_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Jobscience kutucuğa tıkladığınızda, otomatik olarak Jobscience uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/jobscience-tutorial/tutorial_general_01.png
[2]: ./media/jobscience-tutorial/tutorial_general_02.png
[3]: ./media/jobscience-tutorial/tutorial_general_03.png
[4]: ./media/jobscience-tutorial/tutorial_general_04.png

[100]: ./media/jobscience-tutorial/tutorial_general_100.png

[200]: ./media/jobscience-tutorial/tutorial_general_200.png
[201]: ./media/jobscience-tutorial/tutorial_general_201.png
[202]: ./media/jobscience-tutorial/tutorial_general_202.png
[203]: ./media/jobscience-tutorial/tutorial_general_203.png

