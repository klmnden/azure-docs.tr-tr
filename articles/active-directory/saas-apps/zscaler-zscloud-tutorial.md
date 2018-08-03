---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Zscaler ZSCloud | Microsoft Docs'
description: Azure Active Directory ve Zscaler ZSCloud arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 411d5684-a780-410a-9383-59f92cf569b5
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: a23d68e0b48a01cf98a5d1cc136a6af46895b0ee
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39440652"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-zscloud"></a>Öğretici: Azure Active Directory Zscaler ZSCloud ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Zscaler ZSCloud tümleştirme konusunda bilgi edinin.

Zscaler ZSCloud Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Zscaler ZSCloud erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) için Zscaler ZSCloud açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Zscaler ZSCloud yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Zscaler ZSCloud çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Zscaler ZSCloud galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-zscaler-zscloud-from-the-gallery"></a>Zscaler ZSCloud galeri ekleme
Azure AD'de Zscaler ZSCloud tümleştirmesini yapılandırmak için Zscaler ZSCloud Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Zscaler ZSCloud eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Zscaler ZSCloud**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_search.png)

1. Sonuçlar panelinde seçin **Zscaler ZSCloud**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırmanız ve Zscaler ZSCloud ile Azure AD çoklu oturum açmayı test "Britta Simon." adlı bir test kullanıcı tabanlı

Tek iş için oturum açma için Azure AD ne Zscaler ZSCloud karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Zscaler ZSCloud ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değerini atayarak bu bağlantı ilişki kurulduktan **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** Zscaler ZSCloud içinde.

Yapılandırma ve Azure AD çoklu oturum açma Zscaler ZSCloud ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Ara sunucu ayarlarını yapılandırma](#configuring-proxy-settings)**  - Internet Explorer'da proxy ayarlarını yapılandırmak için
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Zscaler ZSCloud test kullanıcısı oluşturma](#creating-a-zscaler-zscloud-test-user)**  - kullanıcı Azure AD gösterimini bağlı Zscaler ZSCloud Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Zscaler ZSCloud uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Zscaler ZSCloud yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Zscaler ZSCloud** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_samlbase.png)

1. Üzerinde **Zscaler ZSCloud etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_url.png)

     İçinde **oturum açma URL'si** metin kutusu, türü URL kullanıcılarınız oturum açmaya ZScaler ZSCloud uygulamanıza tarafından kullanılıyor.
    
    > [!NOTE] 
    > Bu değer gerçek oturum açma URL'si ile güncelleştirmeniz gerekiyor. İlgili kişi [Zscaler ZSCloud istemci Destek ekibine](https://help.zscaler.com/zia) bu değeri alınamıyor. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-zscloud-tutorial/tutorial_general_400.png)

1. Üzerinde **Zscaler ZSCloud yapılandırma** bölümünde **yapılandırma Zscaler ZSCloud** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_configure.png) 

1. Farklı bir web tarayıcı penceresinde ZScaler ZSCloud şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Üstteki menüden **Yönetim**.
   
    ![Yönetim](./media/zscaler-zscloud-tutorial/ic800206.png "Yönetim")

1. Altında **yönetmesine & rolleri**, tıklayın **Kullanıcıları Yönet & kimlik doğrulaması**.   
            
    ![Kullanıcı ve kimlik doğrulaması yönetmek](./media/zscaler-zscloud-tutorial/ic800207.png "kullanıcı ve kimlik doğrulaması'nı yönetme")

1. İçinde **seçin, kuruluşunuz için kimlik doğrulama seçenekleri** bölümünde, aşağıdaki adımları gerçekleştirin:   
                
    ![Kimlik doğrulaması](./media/zscaler-zscloud-tutorial/ic800208.png "kimlik doğrulaması")
   
    a. Seçin **SAML çoklu oturum açma kullanarak kimlik doğrulaması**.

    b. Tıklayın **oturum açma SAML tek parametrelerini yapılandırma**.

1. Üzerinde **SAML çoklu oturum açma parametrelerini yapılandırma** iletişim sayfasında, aşağıdaki adımları uygulayın ve ardından **bitti**

    ![Çoklu oturum açma](./media/zscaler-zscloud-tutorial/ic800209.png "çoklu oturum açma")
    
    a. Yapıştırma **SAML çoklu oturum açma hizmeti URL'si** içine değer **olduğu kullanıcılar için kimlik doğrulaması gönderilir SAML portalın URL'sini** metin.
    
    b. İçinde **öznitelik oturum açma adını içeren** metin kutusuna **Nameıd**.
    
    c. İndirilen sertifikanızı karşıya yüklemek için tıklayın **Zscaler pem**.
    
    d. Seçin **SAML otomatik sağlamayı etkinleştirme**.

1. Üzerinde **kullanıcı kimlik doğrulamasını yapılandırma** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Yönetim](./media/zscaler-zscloud-tutorial/ic800210.png "Yönetim")
    
    a. **Kaydet**’e tıklayın.

    b. Tıklayın **etkinleştirelim**.

## <a name="configuring-proxy-settings"></a>Ara sunucu ayarlarını yapılandırma
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Internet Explorer'ın proxy ayarlarını yapılandırmak için

1. Başlangıç **Internet Explorer**.

1. Seçin **Internet Seçenekleri** gelen **Araçları** menüsünü aç **Internet Seçenekleri** iletişim.   
    
     ![Internet Seçenekleri](./media/zscaler-zscloud-tutorial/ic769492.png "Internet Seçenekleri")

1. Tıklayın **bağlantıları** sekmesi.   
  
     ![Bağlantıları](./media/zscaler-zscloud-tutorial/ic769493.png "bağlantıları")

1. Tıklayın **LAN Ayarları** açmak için **LAN Ayarları** iletişim.

1. Proxy sunucusu bölümünde aşağıdaki adımları gerçekleştirin:   
   
    ![Proxy sunucusu](./media/zscaler-zscloud-tutorial/ic769494.png "Proxy sunucusu")

    a. Seçin **AĞINIZ için bir proxy sunucusu kullan**.

    b. Adresi metin kutusuna yazın **gateway.zscalerone.net**.

    c. Bağlantı noktası metin kutusuna yazın **80**.

    d. Seçin **yerel adresler için proxy sunucuyu atla**.

    e. Tıklayın **Tamam** kapatmak için **yerel alan ağı (LAN) ayarları** iletişim.

1. Tıklayın **Tamam** kapatmak için **Internet Seçenekleri** iletişim.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-zscloud-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-zscloud-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-zscloud-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-zscloud-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.

### <a name="creating-a-zscaler-zscloud-test-user"></a>Zscaler ZSCloud test kullanıcısı oluşturma

ZScaler ZSCloud için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar için ZScaler ZSCloud sağlanması gerekir.  
ZScaler ZSCloud söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Oturum açın, **Zscaler** Kiracı.

1. Tıklayın **Yönetim**.   
   
    ![Yönetim](./media/zscaler-zscloud-tutorial/ic781035.png "Yönetim")

1. Tıklayın **kullanıcı yönetimi**.   
        
     ![Ekleme](./media/zscaler-zscloud-tutorial/ic781037.png "Ekle")

1. İçinde **kullanıcılar** sekmesinde **Ekle**.
      
    ![Ekleme](./media/zscaler-zscloud-tutorial/ic781037.png "Ekle")

1. Kullanıcı Ekle bölümünde aşağıdaki adımları gerçekleştirin:
        
    ![Kullanıcı ekleme](./media/zscaler-zscloud-tutorial/ic781038.png "kullanıcı ekleme")
   
    a. Tür **UserID**, **kullanıcı görünen adı**, **parola**, **parolayı onayla**ve ardından **grupları**ve **departmanı** sağlamak istediğiniz geçerli bir AAD hesabı.

    b. **Kaydet**’e tıklayın.

> [!NOTE]
> Herhangi diğer ZScaler ZSCloud kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak ZScaler ZSCloud tarafından sağlanan.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Zscaler ZSCloud erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Zscaler ZSCloud için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Zscaler ZSCloud**.

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Çoklu oturum açma ayarları test etmek isterseniz, erişim Paneli'nde açın.

Erişim panelinde Zscaler ZSCloud kutucuğa tıkladığınızda, otomatik olarak Zscaler ZSCloud uygulamanıza açan.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/zscaler-zscloud-tutorial/tutorial_general_01.png
[2]: ./media/zscaler-zscloud-tutorial/tutorial_general_02.png
[3]: ./media/zscaler-zscloud-tutorial/tutorial_general_03.png
[4]: ./media/zscaler-zscloud-tutorial/tutorial_general_04.png

[100]: ./media/zscaler-zscloud-tutorial/tutorial_general_100.png

[200]: ./media/zscaler-zscloud-tutorial/tutorial_general_200.png
[201]: ./media/zscaler-zscloud-tutorial/tutorial_general_201.png
[202]: ./media/zscaler-zscloud-tutorial/tutorial_general_202.png
[203]: ./media/zscaler-zscloud-tutorial/tutorial_general_203.png

