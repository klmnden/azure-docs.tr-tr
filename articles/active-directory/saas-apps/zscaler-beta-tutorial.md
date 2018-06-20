---
title: 'Öğretici: Azure Active Directory Tümleştirme Zscaler Beta ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Zscaler Beta arasında yapılandırmayı öğrenin.
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
ms.openlocfilehash: 6e40c75319581a238d22d4ac17952ec706d17fcb
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36226664"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-beta"></a>Öğretici: Azure Active Directory Tümleştirme Zscaler Beta ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Zscaler Beta tümleştirmek öğrenin.

Zscaler Beta Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Zscaler Beta erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak Zscaler Beta (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Zscaler Beta ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Zscaler Beta çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, burada bir aylık deneme elde edebilirsiniz: [deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Zscaler Beta ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-zscaler-beta-from-the-gallery"></a>Galeriden Zscaler Beta ekleme
Azure AD Zscaler Beta tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Zscaler Beta eklemeniz gerekir.

**Galeriden Zscaler Beta eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Zscaler Beta**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-beta-tutorial/tutorial_zscalerbeta_search.png)

5. Sonuçlar panelinde seçin **Zscaler Beta**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-beta-tutorial/tutorial_zscalerbeta_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırmanız ve Zscaler Beta ile Azure AD çoklu oturum açmayı test "Britta Simon" adlı bir test kullanıcı tabanlı.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Zscaler Beta bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı Zscaler Beta arasında bir bağlantı ilişkisi kurulması gerekir.

Değeri Zscaler Beta'da atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Zscaler Beta ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Proxy ayarlarını yapılandırma](#configuring-proxy-settings)**  - Internet Explorer proxy ayarlarını yapılandırmak için
3. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Zscaler Beta test kullanıcısı oluşturma](#creating-a-zscaler-beta-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Zscaler Beta sağlamak için.
5. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
6. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Zscaler Beta uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Zscaler Beta ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Zscaler Beta** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-beta-tutorial/tutorial_zscalerbeta_samlbase.png)

3. Üzerinde **Zscaler Beta etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-beta-tutorial/tutorial_zscalerbeta_url.png)

    Oturum açma URL'si metin kutusuna, kullanıcılarınıza oturum açma Zscaler Beta uygulamanız tarafından kullanılan URL'yi yazın.

    > [!NOTE] 
    > Bu değer gerçek oturum açma URL'si ile güncelleştirmeniz gerekir. Kişi [Zscaler Beta istemci destek ekibi](https://www.zscaler.com/company/contact) bu değeri alınamıyor. 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-beta-tutorial/tutorial_zscalerbeta_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-beta-tutorial/tutorial_general_400.png)

6. Üzerinde **Zscaler Beta yapılandırma** 'yi tıklatın **yapılandırma Zscaler Beta** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-beta-tutorial/tutorial_zscalerbeta_configure.png) 

7. Farklı web tarayıcısı penceresinde Zscaler Beta şirket sitenize yönetici olarak oturum açın.

8. Üstteki menüde tıklatın **Yönetim**.
   
    ![Yönetim](./media/zscaler-beta-tutorial/ic800206.png "Yönetim")

9. Altında **yönetmesine & rolleri**, tıklatın **kullanıcıları yönetme & kimlik doğrulama**.   
            
    ![Kullanıcıların & kimlik doğrulaması Yönet](./media/zscaler-beta-tutorial/ic800207.png "kullanıcılar & kimlik doğrulaması Yönet")

10. İçinde **, kuruluşunuz için kimlik doğrulama seçeneklerini seçin** bölümünde, aşağıdaki adımları gerçekleştirin:   
                
    ![Kimlik doğrulama](./media/zscaler-beta-tutorial/ic800208.png "kimlik doğrulaması")
   
    a. Seçin **SAML çoklu oturum açma kullanarak kimlik doğrulaması**.

    b. Tıklatın **SAML çoklu oturum açma parametreleri**.

11. Üzerinde **yapılandırma SAML çoklu oturum açma parametreleri** iletişim sayfasında, aşağıdaki adımları uygulayın ve ardından **bitti**

    ![Çoklu oturum açma](./media/zscaler-beta-tutorial/ic800209.png "çoklu oturum açma")
    
    a. Yapıştır **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyaladığınız değeri **için kullanıcıların kimlik doğrulaması için gönderilir SAML portalın URL'sini** metin kutusu.
    
    b. İçinde **özniteliği oturum açma adını içeren** metin kutusuna, türü **NameID**.
    
    c. İndirilen sertifikanızı karşıya yüklemek için tıklayın **Zscaler pem**.
    
    d. Seçin **SAML otomatik sağlamayı etkinleştir**.

12. Üzerinde **kullanıcı kimlik doğrulaması yapılandırma** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Yönetim](./media/zscaler-beta-tutorial/ic800210.png "Yönetim")
    
    a. **Kaydet**’e tıklayın.

    b. Tıklatın **şimdi etkinleştirmek**.

## <a name="configuring-proxy-settings"></a>Proxy ayarlarını yapılandırma
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Internet Explorer proxy ayarlarını yapılandırmak için

1. Başlat **Internet Explorer**.

2. Seçin **Internet Seçenekleri** gelen **Araçları** açılan menü **Internet Seçenekleri** iletişim.   
    
     ![Internet Seçenekleri](./media/zscaler-beta-tutorial/ic769492.png "Internet Seçenekleri")

3. Tıklatın **bağlantıları** sekmesi.   
  
     ![Bağlantıları](./media/zscaler-beta-tutorial/ic769493.png "bağlantıları")

4. Tıklatın **LAN Ayarları** açmak için **LAN Ayarları** iletişim.

5. Proxy sunucu bölümünde, aşağıdaki adımları gerçekleştirin:   
   
    ![Proxy sunucusu](./media/zscaler-beta-tutorial/ic769494.png "Proxy sunucusu")

    a. Seçin **AĞINIZ için bir proxy sunucusu kullan**.

    b. Adresi metin kutusuna yazın **gateway.zscalerbeta.net**.

    c. Bağlantı noktası metin kutusuna yazın **80**.

    d. Seçin **yerel adresler için proxy sunucuyu atla**.

    e. Tıklatın **Tamam** kapatmak için **yerel ağ (LAN) ayarları** iletişim.

6. Tıklatın **Tamam** kapatmak için **Internet Seçenekleri** iletişim.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-beta-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-beta-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-beta-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-beta-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-zscaler-beta-test-user"></a>Zscaler Beta test kullanıcısı oluşturma

Azure AD kullanıcılarının Zscaler Beta oturum açmayı etkinleştirmek için bunlar Zscaler Beta sağlanmalıdır. Zscaler Beta söz konusu olduğunda, sağlama bir el ile bir görevdir.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Oturum, **Zscaler Beta** Kiracı.

2. Tıklatın **Yönetim**.   
   
    ![Yönetim](./media/zscaler-beta-tutorial/ic781035.png "Yönetim")

3. Tıklatın **kullanıcı yönetimi**.   
        
     ![Ekleme](./media/zscaler-beta-tutorial/ic781036.png "ekleme")

4. İçinde **kullanıcılar** sekmesini tıklatın, **Ekle**.
      
    ![Ekleme](./media/zscaler-beta-tutorial/ic781037.png "ekleme")

5. Kullanıcı Ekle bölümünde, aşağıdaki adımları gerçekleştirin:
        
    ![Kullanıcı ekleme](./media/zscaler-beta-tutorial/ic781038.png "kullanıcı ekleme")
   
    a. Türü **UserID**, **kullanıcı görünen adı**, **parola**, **parolayı onayla**ve ardından **grupları** ve **departmanı** geçerli bir Azure AD hesabının sağlamak istediğiniz.

    b. **Kaydet**’e tıklayın.

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için herhangi bir Zscaler Beta kullanıcı hesabı oluşturma araçlarını veya Zscaler Beta tarafından sağlanan API'leri kullanabilirsiniz.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Zscaler Beta erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Zscaler Beta Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Zscaler Beta**.

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-beta-tutorial/tutorial_zscalerbeta_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Zscaler Beta parçasında tıklattığınızda, otomatik olarak Zscaler Beta uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

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

