---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Cisco Webex | Microsoft Docs'
description: Azure Active Directory ve Cisco Webex arasında çoklu oturum açmayı yapılandırmayı öğrenin.
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
ms.openlocfilehash: accafab55c6c1aa34ccf7aa9cfc4adb2a902f5e3
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39043447"
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>Öğretici: Azure Active Directory Cisco Webex ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Cisco Webex tümleştirme konusunda bilgi edinin.

Cisco Webex Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Cisco Webex erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak Cisco Webex için kendi Azure AD hesapları ile oturum, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda--Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Cisco Webex yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Cisco Webex tek oturum üzerinde etkin olmayan abonelik

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamında kullanımı önerilmemektedir.

Bu öğreticideki adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık ücretsiz deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Cisco Webex galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="add-cisco-webex-from-the-gallery"></a>Cisco Webex Galeriden Ekle
Azure AD'de Cisco Webex tümleştirmesini yapılandırmak için Cisco Webex Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Cisco Webex Galeriden eklemek için aşağıdaki adımları uygulayın:**

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Git **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Cisco Webex**. 

5. Seçin **Cisco Webex** sonuçları panelinden. Ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Cisco Webex](./media/cisco-webex-tutorial/tutorial_ciscowebex_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Cisco "Britta Simon." adlı bir test kullanıcı tabanlı Webex ile test etme

Tek iş için oturum açma için Azure AD Cisco Webex karşılığı kullanıcının Azure AD'de bir kullanıcı için olan bilmesi gerekir. Diğer bir deyişle, Cisco Webex içinde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı kurmak gerekir.

Cisco Webex içinde değeri vermek **kullanıcıadı** aynı değer olarak **kullanıcı adı** Azure AD'de. Şimdi iki kullanıcı arasındaki bağlantı kurduğunuz. 

Yapılandırma ve Azure AD çoklu oturum açma Cisco Webex ile test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. [Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on) kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
2. [Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user) Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. [Cisco Webex test kullanıcısı oluşturma](#create-a-cisco-webex-test-user) bir karşılığı Britta simon'un kullanıcı Azure AD gösterimini bağlı Cisco Webex sağlamak için.
4. [Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user) Britta Simon, Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
5. [Çoklu oturum açmayı test](#test-single-sign-on) yapılandırma çalıştığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Cisco Webex uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Cisco Webex yapılandırmak için aşağıdaki adımları uygulayın:**

1. Azure portalında, üzerinde **Cisco Webex** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Çoklu oturum açma, etkinleştirmek için **çoklu oturum açma** iletişim kutusundaki **modu** aşağı açılan listesinden **SAML tabanlı oturum açma**.
 
    ![Çoklu oturum açma iletişim kutusu](./media/cisco-webex-tutorial/tutorial_ciscowebex_samlbase.png)

3. İçinde **Cisco Webex etki alanı ve URL'ler** bölümünde, aşağıdaki adımları uygulayın:

    ![Cisco Webex etki alanı ve URL'ler tek oturum açma bilgileri](./media/cisco-webex-tutorial/tutorial_ciscowebex_url.png)

    a. İçinde **oturum açma URL'si** kutusuna aşağıdaki desene sahip bir URL yazın: `https://<subdomain>.webex.com`

    b. İçinde **tanımlayıcı** kutusuna URL'yi yazın `http://www.webex.com`.

    c. İçinde **yanıt URL'si** kutusuna aşağıdaki desene sahip bir URL yazın: `https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek yanıt URL'si ile güncelleştirin ve oturum açma URL'si. İlgili kişi [Cisco Webex istemci Destek ekibine](https://www.webex.co.in/support/support-overview.html) bu değerleri almak için. 

5. Üzerinde **SAML imzalama sertifikası** bölümünden **meta veri XML**ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/cisco-webex-tutorial/tutorial_ciscowebex_certificate.png) 

6. **Kaydet**’i seçin.

    ![Çoklu oturum açma düğmesi kaydetme yapılandırın](./media/cisco-webex-tutorial/tutorial_general_400.png)
    
6. İçinde **Cisco Webex yapılandırma** bölümünden **yapılandırma Cisco Webex** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si**, **SAML varlık kimliği**, ve **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru** bölümü.

    ![Çoklu oturum açmayı yapılandırın](./media/cisco-webex-tutorial/tutorial_ciscowebex_configure.png) 

7. Farklı bir web tarayıcı penceresinde Cisco Webex şirketinizin sitesi için bir yönetici olarak oturum açın.

8. Üstteki menüden **Site Yönetimi**.

    ![Site Yönetimi](./media/cisco-webex-tutorial/ic777621.png "Site Yönetimi")

9. İçinde **yönetme Site** bölümünden **SSO Yapılandırması**.
   
    ![SSO yapılandırma](./media/cisco-webex-tutorial/ic777622.png "SSO yapılandırma")

10. İçinde **Federe Web SSO Yapılandırması** bölümünde, aşağıdaki adımları uygulayın:
   
    ![Federasyon SSO yapılandırma](./media/cisco-webex-tutorial/ic777623.png "Federasyon SSO yapılandırma")  

    a. İçinde **Federation Protokolü** listesinden **SAML 2.0**.

    b. İçin **SSO profili**seçin **SP tarafından başlatılan**.

    c. İndirilen sertifikanızı Not Defteri'nde açın ve ardından içeriği kopyalayın.

    d. Seçin **SAML meta verileri içeri aktarma**ve ardından sertifikayı kopyalanan içeriği yapıştırın.

    e. İçinde **SAML (IDP kimliği) için veren** kutusunda, değerini yapıştırın **SAML varlık kimliği** Azure portaldan kopyaladığınız.

    f. İçinde **müşteri SSO hizmeti oturum açma URL'si** kutusu, yapıştırma **SAML çoklu oturum açma hizmeti URL'si**, Azure Portalı'ndan kopyaladığınız.

    g. Gelen **Nameıd biçimi** listesinden **e-posta adresi**.

    h. İçinde **AuthnContextClassRef** kutusuna **urn: OASIS: adları: tc: SAML:2.0:ac:classes:Password**.

    i. İçinde **müşteri SSO hizmet oturum kapatma URL'si** kutusu, yapıştırma **oturum kapatma URL'si**, Azure Portalı'ndan kopyaladığınız.
   
    j. Seçin **güncelleştirme**.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com) uygulamasını ayarladığınız sırada. Bu uygulamadan ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölümünden **çoklu oturum açma** sekmesini ve sonra katıştırılmış erişin belgelerin **yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında [Azure AD belgeleri katıştırılmış](https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede seçin **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/cisco-webex-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/cisco-webex-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusunda **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/cisco-webex-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları uygulayın:

    ![Kullanıcı iletişim kutusu](./media/cisco-webex-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’u seçin.
 
### <a name="create-a-cisco-webex-test-user"></a>Cisco Webex test kullanıcısı oluşturma

Cisco Webex için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar Cisco Webex sağlanması gerekir. Cisco Webex söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları uygulayın:**

1. Oturum açın, **Cisco Webex** Kiracı.

2. Git **kullanıcıları yönetme** > **kullanıcı ekleme**.
   
    ![Kullanıcı ekleme](./media/cisco-webex-tutorial/ic777625.png "kullanıcı ekleme")

3. İçinde **Kullanıcı Ekle** bölümünde, aşağıdaki adımları uygulayın:
   
    ![Kullanıcı Ekle](./media/cisco-webex-tutorial/ic777626.png "Kullanıcı Ekle")   

    a. İçin **hesap türü**seçin **konak**.

    b. İçinde **ad** kullanıcının ilk adını yazın (Bu durumda, **Britta**).

    c. İçinde **Soyadı** son kullanıcının adını yazın (Bu durumda, **Simon**).

    d. İçinde **kullanıcıadı** kullanıcının e-posta yazın (Bu durumda, **Brittasimon@contoso.com**).

    e. İçinde **e-posta** kullanıcının e-posta adresi yazın (Bu durumda, **Brittasimon@contoso.com**).

    f. İçinde **parola** kullanıcının parolasını yazın.

    g. İçinde **Onayla** parola kutusu, kullanıcının parolasını yeniden girin.

    h. **Add (Ekle)** seçeneğini belirleyin.

>[!NOTE]
>Herhangi bir Cisco Webex kullanıcı hesabı oluşturma araçları veya Azure AD kullanıcı hesapları sağlamak için Cisco Webex tarafından sağlanan API'leri kullanabilirsiniz. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, kullanıcının Britta Simon, Cisco Webex erişim vererek Azure çoklu oturum açmayı kullanmak için etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon için Cisco Webex atamak için aşağıdaki adımları uygulayın:**

1. Azure portalında uygulama görünümünü açın. Sonra dizin görünümü ve ardından Git **kurumsal uygulamalar**.  

2. Seçin **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

3. Uygulamalar listesinde **Cisco Webex**.

    ![Uygulamalar listesini Cisco Webex bağlantıdaki](./media/cisco-webex-tutorial/tutorial_ciscowebex_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Seçin **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim kutusu.

    ![Atama Ekle bölmesi][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** içinde **kullanıcılar** listesi.

6. İçinde **kullanıcılar ve gruplar** iletişim kutusu, tıklayın **seçin** düğmesi.

7. Seçin **atama** düğmesine **atama Ekle** iletişim kutusu.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Ne zaman Cisco Webex uygulamanızı otomatik olarak imzalanan erişim panelinde Cisco Webex kutucuğu seçin.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)

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

