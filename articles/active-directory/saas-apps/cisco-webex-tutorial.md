---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Cisco Webex | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve Cisco Webex arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 26704ca7-13ed-4261-bf24-fd6252e2072b
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: 76980214daf6e7dd019c56db089095ae121b853f
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36215360"
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>Öğretici: Cisco Webex Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Cisco Webex tümleştirmek öğrenin.

Cisco Webex Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Cisco Webex erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak Cisco Webex kendi Azure AD hesaplarıyla oturum, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda--Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı için bkz: [uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Cisco Webex ile Azure AD tümleştirme yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Cisco Webex tek oturum üzerindeki etkin olmayan abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamında kullanmanızı öneririz yok.

Bu öğreticide adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Cisco Webex Galeriden ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="add-cisco-webex-from-the-gallery"></a>Cisco Webex Galerisi'nden ekleme
Azure AD Cisco Webex tümleştirilmesi yapılandırmak için Cisco Webex Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Cisco Webex Galeriden eklemek için aşağıdaki adımları uygulayın:**

1. İçinde [Azure portal](https://portal.azure.com), sol bölmede seçin **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Git **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Cisco Webex**. 

5. Seçin **Cisco Webex** sonuçları panelinden. Ardından **Ekle** uygulama eklemek için düğmesi.

    ![Cisco Webex sonuçlar listesinde](./media/cisco-webex-tutorial/tutorial_ciscowebex_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Cisco "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı Webex ile test etme

Tekli çalışmaya oturum için Azure AD için bir kullanıcı Azure AD'de Cisco Webex karşılık gelen kullanıcı olan bilmek ister. Diğer bir deyişle, Cisco Webex bir Azure AD kullanıcısının ve ilgili kullanıcı arasında bir bağlantı oluşturmanız gerekir.

Cisco Webex içinde değere vermek **kullanıcıadı** aynı değer olarak **kullanıcı adı** Azure AD'de. Şimdi iki kullanıcılar arasında bağlantı kurulduktan. 

Yapılandırma ve Azure AD çoklu oturum açma Cisco Webex ile test etmek için aşağıdaki yapı taşları tamamlayın:

1. [Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on) bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. [Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user) Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. [Cisco Webex test kullanıcısı oluşturma](#create-a-cisco-webex-test-user) Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Cisco Webex sağlamak için.
4. [Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user) Britta Azure AD çoklu oturum açma kullanmak Simon etkinleştirmek için.
5. [Test çoklu oturum açma](#test-single-sign-on) yapılandırma çalıştığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Cisco Webex uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Cisco Webex yapılandırmak için aşağıdaki adımları uygulayın:**

1. Azure portalında üzerinde **Cisco Webex** uygulama tümleştirmesi sayfasında, **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Çoklu oturum açma, etkinleştirmek için **çoklu oturum açma** iletişim kutusunda **modu** aşağı açılan listesinden, **SAML tabanlı oturum açma**.
 
    ![Çoklu oturum açma iletişim kutusu](./media/cisco-webex-tutorial/tutorial_ciscowebex_samlbase.png)

3. İçinde **Cisco Webex etki alanı ve URL'leri** bölümünde, aşağıdaki adımları uygulayın:

    ![Cisco Webex etki alanı ve URL'leri tek oturum açma bilgileri](./media/cisco-webex-tutorial/tutorial_ciscowebex_url.png)

    a. İçinde **oturum açma URL'si** kutusunda, aşağıdaki desende bir URL yazın: `https://<subdomain>.webex.com`

    b. İçinde **tanımlayıcısı** kutusuna URL'yi yazın `http://www.webex.com`.

    c. İçinde **yanıt URL'si** kutusunda, aşağıdaki desende bir URL yazın: `https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Fiili yanıt URL'si ile bu değerleri güncelleştirmek ve URL oturum açma. Kişi [Cisco Webex istemci destek ekibi](https://www.webex.co.in/support/support-overview.html) bu değerleri almak için. 

5. Üzerinde **SAML imzalama sertifikası** bölümünde, select **meta veri XML**ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/cisco-webex-tutorial/tutorial_ciscowebex_certificate.png) 

6. **Kaydet**’i seçin.

    ![Çoklu oturum açma düğmesi kaydetme yapılandırın](./media/cisco-webex-tutorial/tutorial_general_400.png)
    
6. İçinde **Cisco Webex yapılandırma** bölümünde, select **yapılandırma Cisco Webex** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL**, **SAML varlık kimliği**, ve **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru** bölümü.

    ![Çoklu oturum açmayı yapılandırın](./media/cisco-webex-tutorial/tutorial_ciscowebex_configure.png) 

7. Farklı web tarayıcısı penceresinde Cisco Webex şirket sitenize yönetici olarak oturum açın.

8. Üstteki menüden seçin **Site Yönetimi**.

    ![Site Yönetimi](./media/cisco-webex-tutorial/ic777621.png "Site Yönetimi")

9. İçinde **sitesi yönetme** bölümünde, select **SSO yapılandırma**.
   
    ![SSO yapılandırma](./media/cisco-webex-tutorial/ic777622.png "SSO yapılandırma")

10. İçinde **Federe Web SSO Yapılandırması** bölümünde, aşağıdaki adımları uygulayın:
   
    ![SSO yapılandırma Federasyon](./media/cisco-webex-tutorial/ic777623.png "Federasyon SSO yapılandırma")  

    a. İçinde **Federasyon Protokolü** listesinde **SAML 2.0**.

    b. İçin **SSO profili**seçin **SP tarafından başlatılan**.

    c. İndirilen sertifikanızı Not Defteri'nde açın ve ardından içeriği kopyalayın.

    d. Seçin **SAML meta verileri içeri aktarma**ve ardından sertifika kopyalanan içeriğini yapıştırın.

    e. İçinde **veren SAML (IDP kimliği) için** kutusunda, değerini yapıştırın **SAML varlık kimliği** Azure portalından kopyaladığınız.

    f. İçinde **müşteri SSO hizmeti oturum açma URL'si** kutusunda, yapıştırma **SAML çoklu oturum açma hizmet URL'si**, hangi Azure portalından kopyalanır.

    g. Gelen **NameID biçimi** listesinde **e-posta adresi**.

    h. İçinde **AuthnContextClassRef** kutusuna **urn: OASIS: adları: tc: SAML:2.0:ac:classes:Password**.

    i. İçinde **müşteri SSO hizmet oturum kapatma URL'si** kutusunda, yapıştırma **Sign-Out URL**, hangi Azure portalından kopyalanır.
   
    j. Seçin **güncelleştirme**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com) uygulaması kuruluyor sırada. Bu uygulamadan ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölümünde, select **çoklu oturum açma** sekmesini tıklatın ve sonra katıştırılmış erişim belgeleri etraflıca **yapılandırma** alt bölüm. Daha fazla bilgiyi embedded belgeler özelliği hakkında [Azure AD embedded belgeler](https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında sol bölmede seçin **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/cisco-webex-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/cisco-webex-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusunda **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/cisco-webex-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları uygulayın:

    ![Kullanıcı iletişim kutusu](./media/cisco-webex-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’u seçin.
 
### <a name="create-a-cisco-webex-test-user"></a>Cisco Webex test kullanıcısı oluşturma

Cisco Webex oturum açmak Azure AD kullanıcıları etkinleştirmek için bunlar Cisco Webex sağlanmalıdır. Cisco Webex söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları uygulayın:**

1. Oturum açın, **Cisco Webex** Kiracı.

2. Git **kullanıcıları yönetme** > **kullanıcı ekleme**.
   
    ![Kullanıcıları ekleme](./media/cisco-webex-tutorial/ic777625.png "kullanıcı ekleme")

3. İçinde **Kullanıcı Ekle** bölümünde, aşağıdaki adımları uygulayın:
   
    ![Kullanıcı Ekle](./media/cisco-webex-tutorial/ic777626.png "Kullanıcı Ekle")   

    a. İçin **hesap türü**seçin **konak**.

    b. İçinde **ad** kutusuna, kullanıcının ilk adını yazın (Bu durumda, **Britta**).

    c. İçinde **Soyadı** kutusuna, son kullanıcının adını yazın (Bu durumda, **Simon**).

    d. İçinde **kullanıcıadı** kutusuna, kullanıcının e-posta yazın (Bu durumda, **Brittasimon@contoso.com**).

    e. İçinde **e-posta** kutusuna, kullanıcının e-posta adresi yazın (Bu durumda, **Brittasimon@contoso.com**).

    f. İçinde **parola** kullanıcının parolasını yazın.

    g. İçinde **Onayla** parola kutusu, kullanıcının parolayı yeniden girin.

    h. **Add (Ekle)** seçeneğini belirleyin.

>[!NOTE]
>Herhangi bir Cisco Webex kullanıcı hesabı oluşturma araçlarını veya Azure AD kullanıcı hesaplarını sağlamak için Cisco Webex tarafından sağlanan API'ları kullanabilirsiniz. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde kullanıcının Britta Cisco Webex erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirir.

![Kullanıcı rolü atayın][200] 

**Cisco Webex Britta Simon atamak için aşağıdaki adımları uygulayın:**

1. Azure portalında uygulamaları görünümünü açın. Ardından, dizin görünümü ve ardından için Git **kurumsal uygulamalar**.  

2. Seçin **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

3. Uygulamalar listesinde **Cisco Webex**.

    ![Uygulamalar listesinde Cisco Webex bağlantı](./media/cisco-webex-tutorial/tutorial_ciscowebex_app.png)  

3. Soldaki menüde seçin **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Seçin **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** içinde **eklemek atama** iletişim kutusu.

    ![Ekleme atama bölmesi][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** içinde **kullanıcılar** listesi.

6. İçinde **kullanıcılar ve gruplar** iletişim kutusu, tıklatın **seçin** düğmesi.

7. Seçin **atamak** düğmesini **eklemek atama** iletişim kutusu.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Cisco Webex döşeme erişim panelinde Cisco Webex uygulamanızı otomatik olarak imzalanmış seçtiğinizde.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/cisco-webex-tutorial/tutorial_general_01.png
[2]: ./media/cisco-webex-tutorial/tutorial_general_02.png
[3]: ./media/cisco-webex-tutorial/tutorial_general_03.png
[4]: ./media/cisco-webex-tutorial/tutorial_general_04.png

[100]: ./media/cisco-webex-tutorial/tutorial_general_100.png

[200]: ./media/cisco-webex-tutorial/tutorial_general_200.png
[201]: ./media/cisco-webex-tutorial/tutorial_general_201.png
[202]: ./media/cisco-webex-tutorial/tutorial_general_202.png
[203]: ./media/cisco-webex-tutorial/tutorial_general_203.png

