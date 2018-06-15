---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Zscaler bir | Microsoft Docs'
description: Çoklu oturum açma arasındaki Azure Active Directory Zscaler bir yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f352e00d-68d3-4a77-bb92-717d055da56f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 1dcf9dfac84dcc4231660de9efba5af249e7f4db
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34354206"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-one"></a>Öğretici: Azure Active Directory Tümleştirme ile Zscaler bir

Bu öğreticide, Zscaler bir Azure Active Directory (Azure AD) ile tümleştirme öğrenin.

Zscaler bir Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Zscaler bir erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak Zscaler bir için (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Zscaler bir ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Zscaler bir çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, burada bir aylık deneme elde edebilirsiniz: [deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Zscaler bir ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-zscaler-one-from-the-gallery"></a>Galeriden Zscaler bir ekleme
Tümleştirmesini Zscaler bir Azure AD'ye yapılandırmak için Zscaler bir Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Zscaler bir eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Zscaler bir**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_search.png)

5. Sonuçlar panelinde seçin **Zscaler bir**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Zscaler "Britta Simon" adlı bir test kullanıcı tabanlı bir test.

Tekli çalışmaya oturum için Azure AD Zscaler bir karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı Zscaler bir arasında bir bağlantı ilişkisi kurulması gerekir.

Bir Zscaler içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma ile Zscaler bir test etmek için aşağıdaki yapı taşları tamamlanması gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Proxy ayarlarını yapılandırma](#configuring-proxy-settings)**  - Internet Explorer proxy ayarlarını yapılandırmak için
3. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Zscaler bir test kullanıcısı oluşturma](#creating-a-zscaler-one-test-user)**  - Zscaler kullanıcı Azure AD gösterimini bağlantılı bir Britta Simon, karşılık gelen sağlamak için.
5. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
6. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Zscaler bir uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Zscaler bir ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Zscaler bir** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_samlbase.png)

3. Üzerinde **Zscaler bir etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_url.png)

    Oturum açma URL'si metin kutusuna, kullanıcılarınıza oturum açma Zscaler bir uygulamanız tarafından kullanılan URL'yi yazın.

    > [!NOTE] 
    > Bu değer gerçek oturum açma URL'si ile güncelleştirmeniz gerekir. Kişi [Zscaler bir istemci destek ekibi](https://www.zscaler.com/company/contact) bu değerleri almak için.

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_400.png)

6. Üzerinde **Zscaler bir yapılandırma** 'yi tıklatın **Zscaler bir yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_configure.png) 

7. Farklı web tarayıcısı penceresinde Zscaler bir şirket sitenize yönetici olarak oturum açın.

8. Üstteki menüde tıklatın **Yönetim**.
   
    ![Yönetim](./media/active-directory-saas-zscaler-one-tutorial/ic800206.png "Yönetim")

9. Altında **yönetmesine & rolleri**, tıklatın **kullanıcıları yönetme & kimlik doğrulama**.   
            
    ![Kullanıcıların & kimlik doğrulaması Yönet](./media/active-directory-saas-zscaler-one-tutorial/ic800207.png "kullanıcılar & kimlik doğrulaması Yönet")

10. İçinde **, kuruluşunuz için kimlik doğrulama seçeneklerini seçin** bölümünde, aşağıdaki adımları gerçekleştirin:   
                
    ![Kimlik doğrulama](./media/active-directory-saas-zscaler-one-tutorial/ic800208.png "kimlik doğrulaması")
   
    a. Seçin **SAML çoklu oturum açma kullanarak kimlik doğrulaması**.

    b. Tıklatın **SAML çoklu oturum açma parametreleri**.

11. Üzerinde **yapılandırma SAML çoklu oturum açma parametreleri** iletişim sayfasında, aşağıdaki adımları uygulayın ve ardından **bitti**

    ![Çoklu oturum açma](./media/active-directory-saas-zscaler-one-tutorial/ic800209.png "çoklu oturum açma")
    
    a. Yapıştır **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyaladığınız değeri **için kullanıcıların kimlik doğrulaması için gönderilir SAML portalın URL'sini** metin kutusu.
    
    b. İçinde **özniteliği oturum açma adını içeren** metin kutusuna, türü **NameID**.
    
    c. İndirilen sertifikanızı karşıya yüklemek için tıklayın **Zscaler pem**.
    
    d. Seçin **SAML otomatik sağlamayı etkinleştir**.

12. Üzerinde **kullanıcı kimlik doğrulaması yapılandırma** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Yönetim](./media/active-directory-saas-zscaler-one-tutorial/ic800210.png "Yönetim")
    
    a. **Kaydet**’e tıklayın.

    b. Tıklatın **şimdi etkinleştirmek**.

## <a name="configuring-proxy-settings"></a>Proxy ayarlarını yapılandırma
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Internet Explorer proxy ayarlarını yapılandırmak için

1. Başlat **Internet Explorer**.

2. Seçin **Internet Seçenekleri** gelen **Araçları** açılan menü **Internet Seçenekleri** iletişim.   
    
     ![Internet Seçenekleri](./media/active-directory-saas-zscaler-one-tutorial/ic769492.png "Internet Seçenekleri")

3. Tıklatın **bağlantıları** sekmesi.   
  
     ![Bağlantıları](./media/active-directory-saas-zscaler-one-tutorial/ic769493.png "bağlantıları")

4. Tıklatın **LAN Ayarları** açmak için **LAN Ayarları** iletişim.

5. Proxy sunucu bölümünde, aşağıdaki adımları gerçekleştirin:   
   
    ![Proxy sunucusu](./media/active-directory-saas-zscaler-one-tutorial/ic769494.png "Proxy sunucusu")

    a. Seçin **AĞINIZ için bir proxy sunucusu kullan**.

    b. Adresi metin kutusuna yazın **gateway.zscalerone.net**.

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

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-zscaler-one-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-zscaler-one-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-zscaler-one-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-zscaler-one-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-zscaler-one-test-user"></a>Zscaler bir test kullanıcısı oluşturma

Azure AD kullanıcılarının Zscaler bir oturum açmayı etkinleştirmek için bunlar Zscaler bir sağlanmalıdır. Bir Zscaler söz konusu olduğunda sağlama bir el ile bir görevdir.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Oturum, **Zscaler bir** Kiracı.

2. Tıklatın **Yönetim**.   
   
    ![Yönetim](./media/active-directory-saas-zscaler-one-tutorial/ic781035.png "Yönetim")

3. Tıklatın **kullanıcı yönetimi**.   
        
     ![Ekleme](./media/active-directory-saas-zscaler-one-tutorial/ic781036.png "ekleme")

4. İçinde **kullanıcılar** sekmesini tıklatın, **Ekle**.
      
    ![Ekleme](./media/active-directory-saas-zscaler-one-tutorial/ic781037.png "ekleme")

5. Kullanıcı Ekle bölümünde, aşağıdaki adımları gerçekleştirin:
        
    ![Kullanıcı ekleme](./media/active-directory-saas-zscaler-one-tutorial/ic781038.png "kullanıcı ekleme")
   
    a. Türü **UserID**, **kullanıcı görünen adı**, **parola**, **parolayı onayla**ve ardından **grupları** ve **departmanı** geçerli bir Azure AD hesabının sağlamak istediğiniz.

    b. **Kaydet**’e tıklayın.

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için herhangi bir Zscaler bir kullanıcı hesabı oluşturma araçlarını veya Zscaler bir tarafından sağlanan API'leri kullanabilirsiniz.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Zscaler bir erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Britta Simon Zscaler bir atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Zscaler bir**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Zscaler bir kutucuğa tıkladığınızda, otomatik olarak Zscaler bir uygulamanız açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_203.png

