---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Zscaler iki | Microsoft Docs'
description: Azure Active Directory ve Zscaler iki arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 1fd8a940-7320-47e0-a176-2dd4eeca6db2
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: be41b1cc19043faf40804876d11fd43a32a1a45c
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39055805"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-two"></a>Öğretici: Azure Active Directory Tümleştirme ile Zscaler iki

Bu öğreticide, Zscaler iki Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.

Zscaler iki Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Zscaler iki erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan Zscaler iki için (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi iki Zscaler ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Zscaler iki çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme burada alabilirsiniz: [deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Zscaler iki galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-zscaler-two-from-the-gallery"></a>Zscaler iki galeri ekleme
Azure AD'de Zscaler iki'nın tümleştirmesini yapılandırmak için Zscaler iki Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Zscaler iki Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Zscaler iki**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-two-tutorial/tutorial_zscalertwo_search.png)

5. Sonuçlar panelinde seçin **Zscaler iki**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-two-tutorial/tutorial_zscalertwo_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı iki Zscaler ile test edin.

Tek iş için oturum açma için Azure AD ne Zscaler iki karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı Zscaler iki arasında bir bağlantı ilişki kurulması gerekir.

İki Zscaler içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Zscaler iki ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Ara sunucu ayarlarını yapılandırma](#configuring-proxy-settings)**  - Internet Explorer'da proxy ayarlarını yapılandırmak için
3. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Zscaler iki test kullanıcısı oluşturma](#creating-a-zscaler-two-test-user)**  - Zscaler kullanıcı Azure AD gösterimini bağlı iki Britta simon'un bir karşılığı vardır.
5. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
6. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Zscaler iki uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma iki Zscaler ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Zscaler iki** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-two-tutorial/tutorial_zscalertwo_samlbase.png)

3. Üzerinde **Zscaler iki etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-two-tutorial/tutorial_zscalertwo_url.png)

   Oturum açma URL'si metin kutusuna, kullanıcılarınız oturum açmaya ZScaler iki uygulamanızın kullandığı URL'yi yazın.

    > [!NOTE] 
    > Bu değer gerçek oturum açma URL'si ile güncelleştirmeniz gerekiyor. İlgili kişi [Zscaler iki istemci Destek ekibine](https://www.zscaler.com/company/contact) bu değerleri almak için.

4. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-two-tutorial/tutorial_zscalertwo_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-two-tutorial/tutorial_general_400.png)

6. Üzerinde **Zscaler iki yapılandırma** bölümünde **Zscaler iki yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-two-tutorial/tutorial_zscalertwo_configure.png) 

7. Farklı bir web tarayıcı penceresinde ZScaler iki şirketinizin sitesi için bir yönetici olarak oturum açın.

8. Üstteki menüden **Yönetim**.
   
    ![Yönetim](./media/zscaler-two-tutorial/ic800206.png "Yönetim")

9. Altında **yönetmesine & rolleri**, tıklayın **Kullanıcıları Yönet & kimlik doğrulaması**.   
            
    ![Kullanıcı ve kimlik doğrulaması yönetmek](./media/zscaler-two-tutorial/ic800207.png "kullanıcı ve kimlik doğrulaması'nı yönetme")

10. İçinde **seçin, kuruluşunuz için kimlik doğrulama seçenekleri** bölümünde, aşağıdaki adımları gerçekleştirin:   
                
    ![Kimlik doğrulaması](./media/zscaler-two-tutorial/ic800208.png "kimlik doğrulaması")
   
    a. Seçin **SAML çoklu oturum açma kullanarak kimlik doğrulaması**.

    b. Tıklayın **oturum açma SAML tek parametrelerini yapılandırma**.

11. Üzerinde **SAML çoklu oturum açma parametrelerini yapılandırma** iletişim sayfasında, aşağıdaki adımları uygulayın ve ardından **bitti**

    ![Çoklu oturum açma](./media/zscaler-two-tutorial/ic800209.png "çoklu oturum açma")
    
    a. Yapıştırma **SAML çoklu oturum açma hizmeti URL'si** Azure portaldan kopyaladığınız değeri **olduğu kullanıcılar için kimlik doğrulaması gönderilir SAML portalın URL'sini** metin.
    
    b. İçinde **öznitelik oturum açma adını içeren** metin kutusuna **Nameıd**.
    
    c. İndirilen sertifikanızı karşıya yüklemek için tıklayın **Zscaler pem**.
    
    d. Seçin **SAML otomatik sağlamayı etkinleştirme**.

12. Üzerinde **kullanıcı kimlik doğrulamasını yapılandırma** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Yönetim](./media/zscaler-two-tutorial/ic800210.png "Yönetim")
    
    a. **Kaydet**’e tıklayın.

    b. Tıklayın **etkinleştirelim**.

## <a name="configuring-proxy-settings"></a>Ara sunucu ayarlarını yapılandırma
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Internet Explorer'ın proxy ayarlarını yapılandırmak için

1. Başlangıç **Internet Explorer**.

2. Seçin **Internet Seçenekleri** gelen **Araçları** menüsünü aç **Internet Seçenekleri** iletişim.   
    
     ![Internet Seçenekleri](./media/zscaler-two-tutorial/ic769492.png "Internet Seçenekleri")

3. Tıklayın **bağlantıları** sekmesi.   
  
     ![Bağlantıları](./media/zscaler-two-tutorial/ic769493.png "bağlantıları")

4. Tıklayın **LAN Ayarları** açmak için **LAN Ayarları** iletişim.

5. Proxy sunucusu bölümünde aşağıdaki adımları gerçekleştirin:   
   
    ![Proxy sunucusu](./media/zscaler-two-tutorial/ic769494.png "Proxy sunucusu")

    a. Seçin **AĞINIZ için bir proxy sunucusu kullan**.

    b. Adresi metin kutusuna yazın **gateway.zscalertwo.net**.

    c. Bağlantı noktası metin kutusuna yazın **80**.

    d. Seçin **yerel adresler için proxy sunucuyu atla**.

    e. Tıklayın **Tamam** kapatmak için **yerel alan ağı (LAN) ayarları** iletişim.

6. Tıklayın **Tamam** kapatmak için **Internet Seçenekleri** iletişim.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-two-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-two-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-two-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-two-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-zscaler-two-test-user"></a>Zscaler iki test kullanıcısı oluşturma

ZScaler için iki oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar için iki ZScaler sağlanması gerekir. ZScaler iki söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Oturum açın, **Zscaler iki** Kiracı.

2. Tıklayın **Yönetim**.   
   
    ![Yönetim](./media/zscaler-two-tutorial/ic781035.png "Yönetim")

3. Tıklayın **kullanıcı yönetimi**.   
        
     ![Ekleme](./media/zscaler-two-tutorial/ic781036.png "Ekle")

4. İçinde **kullanıcılar** sekmesinde **Ekle**.
      
    ![Ekleme](./media/zscaler-two-tutorial/ic781037.png "Ekle")

5. Kullanıcı Ekle bölümünde aşağıdaki adımları gerçekleştirin:
        
    ![Kullanıcı ekleme](./media/zscaler-two-tutorial/ic781038.png "kullanıcı ekleme")
   
    a. Tür **UserID**, **kullanıcı görünen adı**, **parola**, **parolayı onayla**ve ardından **grupları**ve **departmanı** geçerli bir Azure AD hesabı sağlamak istediğiniz.

    b. **Kaydet**’e tıklayın.

> [!NOTE]
> Azure AD kullanıcı hesapları sağlamak için herhangi bir ZScaler iki kullanıcı hesabı oluşturma araçları veya iki ZScaler tarafından sağlanan API'leri kullanabilirsiniz.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, erişim için iki Zscaler vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Zscaler iki için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Zscaler iki**.

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-two-tutorial/tutorial_zscalertwo_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Zscaler iki kutucuğa tıkladığınızda, otomatik olarak Zscaler iki uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/zscaler-two-tutorial/tutorial_general_01.png
[2]: ./media/zscaler-two-tutorial/tutorial_general_02.png
[3]: ./media/zscaler-two-tutorial/tutorial_general_03.png
[4]: ./media/zscaler-two-tutorial/tutorial_general_04.png

[100]: ./media/zscaler-two-tutorial/tutorial_general_100.png

[200]: ./media/zscaler-two-tutorial/tutorial_general_200.png
[201]: ./media/zscaler-two-tutorial/tutorial_general_201.png
[202]: ./media/zscaler-two-tutorial/tutorial_general_202.png
[203]: ./media/zscaler-two-tutorial/tutorial_general_203.png

