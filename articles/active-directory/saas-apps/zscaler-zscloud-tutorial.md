---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Zscaler ZSCloud | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Zscaler ZSCloud arasında yapılandırmayı öğrenin.
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
ms.openlocfilehash: 1ccd0048eb2f1ab32e9fbf403b65f68b07ada480
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36227386"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-zscloud"></a>Öğretici: Azure Active Directory Tümleştirme Zscaler ZSCloud ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Zscaler ZSCloud tümleştirmek öğrenin.

Zscaler ZSCloud Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Zscaler ZSCloud erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için Zscaler ZSCloud açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Zscaler ZSCloud ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Zscaler ZSCloud çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Zscaler ZSCloud ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-zscaler-zscloud-from-the-gallery"></a>Galeriden Zscaler ZSCloud ekleme
Azure AD Zscaler ZSCloud tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Zscaler ZSCloud eklemeniz gerekir.

**Galeriden Zscaler ZSCloud eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Zscaler ZSCloud**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_search.png)

5. Sonuçlar panelinde seçin **Zscaler ZSCloud**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırmanız ve Zscaler ZSCloud ile Azure AD çoklu oturum açmayı test "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı

Tekli çalışmaya oturum için Azure AD Zscaler ZSCloud karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Zscaler ZSCloud ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** Zscaler ZSCloud içinde.

Yapılandırma ve Azure AD çoklu oturum açma Zscaler ZSCloud ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Proxy ayarlarını yapılandırma](#configuring-proxy-settings)**  - Internet Explorer proxy ayarlarını yapılandırmak için
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Zscaler ZSCloud test kullanıcısı oluşturma](#creating-a-zscaler-zscloud-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Zscaler ZSCloud sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Zscaler ZSCloud uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Zscaler ZSCloud ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Zscaler ZSCloud** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_samlbase.png)

3. Üzerinde **Zscaler ZSCloud etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_url.png)

     İçinde **oturum açma URL'si** metin kutusuna, türü URL kullanıcılarınıza oturum açma ZScaler ZSCloud uygulamanıza tarafından kullanılıyor.
    
    > [!NOTE] 
    > Bu değer gerçek oturum açma URL'si ile güncelleştirmeniz gerekir. Kişi [Zscaler ZSCloud istemci destek ekibi](https://support.zscaler.com/hc/articles/210172606-Zscaler-is-Expanding-Its-Zscloud-Global-Footprint) bu değeri alınamıyor. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-zscloud-tutorial/tutorial_general_400.png)

6. Üzerinde **Zscaler ZSCloud yapılandırma** 'yi tıklatın **yapılandırma Zscaler ZSCloud** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_configure.png) 

7. Farklı web tarayıcısı penceresinde ZScaler ZSCloud şirket sitenize yönetici olarak oturum açın.

8. Üstteki menüde tıklatın **Yönetim**.
   
    ![Yönetim](./media/zscaler-zscloud-tutorial/ic800206.png "Yönetim")

9. Altında **yönetmesine & rolleri**, tıklatın **kullanıcıları yönetme & kimlik doğrulama**.   
            
    ![Kullanıcıların & kimlik doğrulaması Yönet](./media/zscaler-zscloud-tutorial/ic800207.png "kullanıcılar & kimlik doğrulaması Yönet")

10. İçinde **, kuruluşunuz için kimlik doğrulama seçeneklerini seçin** bölümünde, aşağıdaki adımları gerçekleştirin:   
                
    ![Kimlik doğrulama](./media/zscaler-zscloud-tutorial/ic800208.png "kimlik doğrulaması")
   
    a. Seçin **SAML çoklu oturum açma kullanarak kimlik doğrulaması**.

    b. Tıklatın **SAML çoklu oturum açma parametreleri**.

11. Üzerinde **yapılandırma SAML çoklu oturum açma parametreleri** iletişim sayfasında, aşağıdaki adımları uygulayın ve ardından **bitti**

    ![Çoklu oturum açma](./media/zscaler-zscloud-tutorial/ic800209.png "çoklu oturum açma")
    
    a. Yapıştır **SAML çoklu oturum açma hizmet URL'si** içine değer **için kullanıcıların kimlik doğrulaması için gönderilir SAML portalın URL'sini** metin kutusu.
    
    b. İçinde **özniteliği oturum açma adını içeren** metin kutusuna, türü **NameID**.
    
    c. İndirilen sertifikanızı karşıya yüklemek için tıklayın **Zscaler pem**.
    
    d. Seçin **SAML otomatik sağlamayı etkinleştir**.

12. Üzerinde **kullanıcı kimlik doğrulaması yapılandırma** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Yönetim](./media/zscaler-zscloud-tutorial/ic800210.png "Yönetim")
    
    a. **Kaydet**’e tıklayın.

    b. Tıklatın **şimdi etkinleştirmek**.

## <a name="configuring-proxy-settings"></a>Proxy ayarlarını yapılandırma
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Internet Explorer proxy ayarlarını yapılandırmak için

1. Başlat **Internet Explorer**.

2. Seçin **Internet Seçenekleri** gelen **Araçları** açılan menü **Internet Seçenekleri** iletişim.   
    
     ![Internet Seçenekleri](./media/zscaler-zscloud-tutorial/ic769492.png "Internet Seçenekleri")

3. Tıklatın **bağlantıları** sekmesi.   
  
     ![Bağlantıları](./media/zscaler-zscloud-tutorial/ic769493.png "bağlantıları")

4. Tıklatın **LAN Ayarları** açmak için **LAN Ayarları** iletişim.

5. Proxy sunucu bölümünde, aşağıdaki adımları gerçekleştirin:   
   
    ![Proxy sunucusu](./media/zscaler-zscloud-tutorial/ic769494.png "Proxy sunucusu")

    a. Seçin **AĞINIZ için bir proxy sunucusu kullan**.

    b. Adresi metin kutusuna yazın **gateway.zscalerone.net**.

    c. Bağlantı noktası metin kutusuna yazın **80**.

    d. Seçin **yerel adresler için proxy sunucuyu atla**.

    e. Tıklatın **Tamam** kapatmak için **yerel ağ (LAN) ayarları** iletişim.

6. Tıklatın **Tamam** kapatmak için **Internet Seçenekleri** iletişim.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-zscloud-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-zscloud-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-zscloud-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-zscloud-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.

### <a name="creating-a-zscaler-zscloud-test-user"></a>Zscaler ZSCloud test kullanıcısı oluşturma

Azure AD kullanıcılarının ZScaler ZSCloud oturum açmayı etkinleştirmek için bunlar ZScaler ZSCloud sağlanmalıdır.  
ZScaler ZSCloud söz konusu olduğunda, sağlama bir el ile bir görevdir.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Oturum, **Zscaler** Kiracı.

2. Tıklatın **Yönetim**.   
   
    ![Yönetim](./media/zscaler-zscloud-tutorial/ic781035.png "Yönetim")

3. Tıklatın **kullanıcı yönetimi**.   
        
     ![Ekleme](./media/zscaler-zscloud-tutorial/ic781037.png "ekleme")

4. İçinde **kullanıcılar** sekmesini tıklatın, **Ekle**.
      
    ![Ekleme](./media/zscaler-zscloud-tutorial/ic781037.png "ekleme")

5. Kullanıcı Ekle bölümünde, aşağıdaki adımları gerçekleştirin:
        
    ![Kullanıcı ekleme](./media/zscaler-zscloud-tutorial/ic781038.png "kullanıcı ekleme")
   
    a. Tür **UserID**, **kullanıcı görünen adı**, **parola**, **parolayı onayla**ve ardından **grupları** ve **departmanı** sağlamak istediğiniz geçerli bir AAD hesabının.

    b. **Kaydet**’e tıklayın.

> [!NOTE]
> API sağlama AAD kullanıcı hesaplarına ZScaler ZSCloud tarafından sağlanan veya herhangi diğer ZScaler ZSCloud kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Zscaler ZSCloud erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Zscaler ZSCloud Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Zscaler ZSCloud**.

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Çoklu oturum açma ayarlarınızı test etmek isterseniz, erişim paneli açın.

Erişim paneli Zscaler ZSCloud parçasında tıklattığınızda, otomatik olarak Zscaler ZSCloud uygulamanıza açan.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

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

