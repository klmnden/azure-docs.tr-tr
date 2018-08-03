---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Zscaler Beta | Microsoft Docs'
description: Azure Active Directory ve Zscaler Beta arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 56b846ae-a1e7-45ae-a79d-992a87f075ba
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 29637f8a733e9f92f37144491bef4ab4ba5aae07
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39433369"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-beta"></a>Öğretici: Azure Active Directory tümleştirmesiyle Zscaler Beta

Bu öğreticide, Azure Active Directory (Azure AD) ile Zscaler Beta tümleştirme konusunda bilgi edinin.

Zscaler Beta Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Zscaler Beta erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan Zscaler Beta (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Zscaler Beta yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Zscaler Beta çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme burada alabilirsiniz: [deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Zscaler Beta galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-zscaler-beta-from-the-gallery"></a>Zscaler Beta galeri ekleme
Azure AD'de Zscaler Beta tümleştirmesini yapılandırmak için Zscaler Beta Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Zscaler Beta eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Zscaler Beta**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-beta-tutorial/tutorial_zscalerbeta_search.png)

1. Sonuçlar panelinde seçin **Zscaler Beta**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-beta-tutorial/tutorial_zscalerbeta_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırmanız ve Zscaler Beta ile Azure AD çoklu oturum açmayı test "Britta Simon" adlı bir test kullanıcı tabanlı.

Tek iş için oturum açma için Azure AD ne Zscaler Beta karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı Zscaler Beta arasında bir bağlantı ilişki kurulması gerekir.

Zscaler Beta'da değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma ile Zscaler Beta test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Ara sunucu ayarlarını yapılandırma](#configuring-proxy-settings)**  - Internet Explorer'da proxy ayarlarını yapılandırmak için
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Zscaler Beta test kullanıcısı oluşturma](#creating-a-zscaler-beta-test-user)**  - kullanıcı Azure AD gösterimini bağlı Zscaler Beta Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Zscaler Beta uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Zscaler Beta yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Zscaler Beta** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-beta-tutorial/tutorial_zscalerbeta_samlbase.png)

1. Üzerinde **Zscaler Beta etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-beta-tutorial/tutorial_zscalerbeta_url.png)

    Oturum açma URL'si metin kutusuna, kullanıcılarınız oturum açmaya Zscaler Beta uygulamanız tarafından kullanılan URL'yi yazın.

    > [!NOTE] 
    > Bu değer gerçek oturum açma URL'si ile güncelleştirmeniz gerekiyor. İlgili kişi [Zscaler Beta istemci Destek ekibine](https://www.zscaler.com/company/contact) bu değeri alınamıyor. 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-beta-tutorial/tutorial_zscalerbeta_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-beta-tutorial/tutorial_general_400.png)

1. Üzerinde **Zscaler Beta yapılandırma** bölümünde **yapılandırma Zscaler Beta** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-beta-tutorial/tutorial_zscalerbeta_configure.png) 

1. Farklı bir web tarayıcı penceresinde Zscaler Beta şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Üstteki menüden **Yönetim**.
   
    ![Yönetim](./media/zscaler-beta-tutorial/ic800206.png "Yönetim")

1. Altında **yönetmesine & rolleri**, tıklayın **Kullanıcıları Yönet & kimlik doğrulaması**.   
            
    ![Kullanıcı ve kimlik doğrulaması yönetmek](./media/zscaler-beta-tutorial/ic800207.png "kullanıcı ve kimlik doğrulaması'nı yönetme")

1. İçinde **seçin, kuruluşunuz için kimlik doğrulama seçenekleri** bölümünde, aşağıdaki adımları gerçekleştirin:   
                
    ![Kimlik doğrulaması](./media/zscaler-beta-tutorial/ic800208.png "kimlik doğrulaması")
   
    a. Seçin **SAML çoklu oturum açma kullanarak kimlik doğrulaması**.

    b. Tıklayın **oturum açma SAML tek parametrelerini yapılandırma**.

1. Üzerinde **SAML çoklu oturum açma parametrelerini yapılandırma** iletişim sayfasında, aşağıdaki adımları uygulayın ve ardından **bitti**

    ![Çoklu oturum açma](./media/zscaler-beta-tutorial/ic800209.png "çoklu oturum açma")
    
    a. Yapıştırma **SAML çoklu oturum açma hizmeti URL'si** Azure portaldan kopyaladığınız değeri **olduğu kullanıcılar için kimlik doğrulaması gönderilir SAML portalın URL'sini** metin.
    
    b. İçinde **öznitelik oturum açma adını içeren** metin kutusuna **Nameıd**.
    
    c. İndirilen sertifikanızı karşıya yüklemek için tıklayın **Zscaler pem**.
    
    d. Seçin **SAML otomatik sağlamayı etkinleştirme**.

1. Üzerinde **kullanıcı kimlik doğrulamasını yapılandırma** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Yönetim](./media/zscaler-beta-tutorial/ic800210.png "Yönetim")
    
    a. **Kaydet**’e tıklayın.

    b. Tıklayın **etkinleştirelim**.

## <a name="configuring-proxy-settings"></a>Ara sunucu ayarlarını yapılandırma
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Internet Explorer'ın proxy ayarlarını yapılandırmak için

1. Başlangıç **Internet Explorer**.

1. Seçin **Internet Seçenekleri** gelen **Araçları** menüsünü aç **Internet Seçenekleri** iletişim.   
    
     ![Internet Seçenekleri](./media/zscaler-beta-tutorial/ic769492.png "Internet Seçenekleri")

1. Tıklayın **bağlantıları** sekmesi.   
  
     ![Bağlantıları](./media/zscaler-beta-tutorial/ic769493.png "bağlantıları")

1. Tıklayın **LAN Ayarları** açmak için **LAN Ayarları** iletişim.

1. Proxy sunucusu bölümünde aşağıdaki adımları gerçekleştirin:   
   
    ![Proxy sunucusu](./media/zscaler-beta-tutorial/ic769494.png "Proxy sunucusu")

    a. Seçin **AĞINIZ için bir proxy sunucusu kullan**.

    b. Adresi metin kutusuna yazın **gateway.zscalerbeta.net**.

    c. Bağlantı noktası metin kutusuna yazın **80**.

    d. Seçin **yerel adresler için proxy sunucuyu atla**.

    e. Tıklayın **Tamam** kapatmak için **yerel alan ağı (LAN) ayarları** iletişim.

1. Tıklayın **Tamam** kapatmak için **Internet Seçenekleri** iletişim.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-beta-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-beta-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-beta-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-beta-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-zscaler-beta-test-user"></a>Zscaler Beta test kullanıcısı oluşturma

Zscaler Beta oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar Zscaler Beta sağlanması gerekir. Zscaler Beta söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Oturum açın, **Zscaler Beta** Kiracı.

1. Tıklayın **Yönetim**.   
   
    ![Yönetim](./media/zscaler-beta-tutorial/ic781035.png "Yönetim")

1. Tıklayın **kullanıcı yönetimi**.   
        
     ![Ekleme](./media/zscaler-beta-tutorial/ic781036.png "Ekle")

1. İçinde **kullanıcılar** sekmesinde **Ekle**.
      
    ![Ekleme](./media/zscaler-beta-tutorial/ic781037.png "Ekle")

1. Kullanıcı Ekle bölümünde aşağıdaki adımları gerçekleştirin:
        
    ![Kullanıcı ekleme](./media/zscaler-beta-tutorial/ic781038.png "kullanıcı ekleme")
   
    a. Tür **UserID**, **kullanıcı görünen adı**, **parola**, **parolayı onayla**ve ardından **grupları**ve **departmanı** geçerli bir Azure AD hesabı sağlamak istediğiniz.

    b. **Kaydet**’e tıklayın.

> [!NOTE]
> Azure AD kullanıcı hesapları sağlamak için herhangi bir Zscaler Beta kullanıcı hesabı oluşturma araçları veya Zscaler Beta tarafından sağlanan API'leri kullanabilirsiniz.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Zscaler Beta için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Zscaler Beta atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Zscaler Beta**.

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-beta-tutorial/tutorial_zscalerbeta_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Zscaler Beta kutucuğa tıkladığınızda, otomatik olarak Zscaler Beta uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/zscaler-beta-tutorial/tutorial_general_01.png
[2]: ./media/zscaler-beta-tutorial/tutorial_general_02.png
[3]: ./media/zscaler-beta-tutorial/tutorial_general_03.png
[4]: ./media/zscaler-beta-tutorial/tutorial_general_04.png

[100]: ./media/zscaler-beta-tutorial/tutorial_general_100.png

[200]: ./media/zscaler-beta-tutorial/tutorial_general_200.png
[201]: ./media/zscaler-beta-tutorial/tutorial_general_201.png
[202]: ./media/zscaler-beta-tutorial/tutorial_general_202.png
[203]: ./media/zscaler-beta-tutorial/tutorial_general_203.png

