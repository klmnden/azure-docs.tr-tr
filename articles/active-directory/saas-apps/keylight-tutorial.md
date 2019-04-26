---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile LockPath Keylight | Microsoft Docs'
description: Azure Active Directory ve LockPath Keylight arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 234a32f1-9f56-4650-9e31-7b38ad734b1a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4bc5121f6604fae9a28b52db1bfb308d7cdb968d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60263412"
---
# <a name="tutorial-azure-active-directory-integration-with-lockpath-keylight"></a>Öğretici: LockPath Keylight ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile LockPath Keylight tümleştirme konusunda bilgi edinin.

Azure AD ile LockPath Keylight tümleştirme ile aşağıdaki avantajları sağlar:

- LockPath Keylight erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) için LockPath Keylight açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile LockPath Keylight yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir LockPath Keylight çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden LockPath Keylight ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-lockpath-keylight-from-the-gallery"></a>Galeriden LockPath Keylight ekleme
Azure AD'de LockPath Keylight tümleştirmesini yapılandırmak için LockPath Keylight Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden LockPath Keylight eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **LockPath Keylight**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/keylight-tutorial/tutorial_keylight_search.png)

1. Sonuçlar panelinde seçin **LockPath Keylight**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/keylight-tutorial/tutorial_keylight_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırmanız ve LockPath Keylight ile Azure AD çoklu oturum açmayı test "Britta Simon." adlı bir test kullanıcı tabanlı

Tek iş için oturum açma için Azure AD ne LockPath Keylight karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının LockPath Keylight ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

LockPath Keylight içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma LockPath Keylight ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[LockPath Keylight test kullanıcısı oluşturma](#creating-a-lockpath-keylight-test-user)**  - kullanıcı Azure AD gösterimini bağlı LockPath Keylight Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve LockPath Keylight uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile LockPath Keylight yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **LockPath Keylight** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/keylight-tutorial/tutorial_keylight_samlbase.png)

1. Üzerinde **LockPath Keylight etki alanı ve URL'ler** bölümünde, aşağıdaki adımları uygulayın::

    ![Çoklu oturum açmayı yapılandırın](./media/keylight-tutorial/tutorial_keylight_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<company name>.keylightgrc.com/`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<company name>.keylightgrc.com`

    c. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<company name>.keylightgrc.com/Login.aspx`
    
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [LockPath Keylight istemci Destek ekibine](https://www.lockpath.com/contact/) bu değerleri almak için. 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Raw)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/keylight-tutorial/tutorial_keylight_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/keylight-tutorial/tutorial_general_400.png)
    
1. Üzerinde **LockPath Keylight yapılandırma** bölümünde **yapılandırma LockPath Keylight** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/keylight-tutorial/tutorial_keylight_configure.png) 

1. İçinde LockPath Keylight SSO'yu etkinleştirmek üzere aşağıdaki adımları gerçekleştirin:
   
    a. LockPath Keylight hesabınız yönetici olarak oturum.
    
    b. Üstteki menüden **kişi**seçip **Keylight Kurulum**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/keylight-tutorial/401.png) 

    c. Soldaki ağaç görünümünde tıklayın **SAML**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/keylight-tutorial/402.png) 

    d. Üzerinde **SAML ayarlarını** iletişim kutusunda, tıklayın **Düzenle**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/keylight-tutorial/404.png) 

1. Üzerinde **SAML ayarlarını Düzenle** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı yapılandırın](./media/keylight-tutorial/405.png) 
   
    a. Ayarlama **SAML kimlik doğrulaması** için **etkin**.

    b. Yapıştırma **SAML çoklu oturum açma hizmeti URL'si** Azure portaldan kopyaladığınız değeri **kimlik sağlayıcısı oturum açma URL'si** metin.

    c. Yapıştırma **çoklu oturum kapatma hizmeti URL'si** Azure portaldan kopyaladığınız değeri **kimlik sağlayıcısı oturum kapatma URL'si** metin.

    d. Tıklayın **Dosya Seç** indirilen LockPath Keylight sertifikanızı seçin ve ardından **açık** sertifikayı karşıya yüklemek için.

    e. Ayarlama **SAML kullanıcı kimliği konumu** için **NameIdentifier öğesi konu deyiminin**.
    
    f. Sağlamak **Keylight hizmet sağlayıcısı** aşağıdaki deseni kullanılarak: **https://&lt;CompanyName&gt;. keylightgrc.com**.
    
    g. Ayarlama **otomatik sağlama kullanıcı** için **etkin**.

    h. Ayarlama **otomatik sağlama hesap türü** için **tam kullanıcı**.

    i. Ayarlama **otomatik sağlama güvenlik rolü**seçin **SAML sahip standart kullanıcı**.
    
    j. Ayarlama **otomatik sağlama güvenlik yapılandırma**seçin **standart kullanıcı yapılandırması**.
     
    k. İçinde **e-posta özniteliği** metin kutusuna `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.
    
    m. İçinde **ad özniteliği** metin kutusuna `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.
    
    m. İçinde **son name özniteliği** metin kutusuna `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.
    
    n. **Kaydet**’e tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/keylight-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/keylight-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/keylight-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/keylight-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-lockpath-keylight-test-user"></a>LockPath Keylight test kullanıcısı oluşturma

Bu bölümde, Britta Simon LockPath Keylight içinde adlı bir kullanıcı oluşturun. LockPath Keylight tam zamanında sağlama, varsayılan olarak etkin olduğu destekler.

Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı, kullanıcı henüz mevcut değilse LockPath Keylight erişirken oluşturulur. 

>[!NOTE]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, iletişime geçmeniz [LockPath Keylight istemci Destek ekibine](https://www.lockpath.com/contact/). 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için LockPath Keylight erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon LockPath Keylight için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **LockPath Keylight**.

    ![Çoklu oturum açmayı yapılandırın](./media/keylight-tutorial/tutorial_keylight_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde LockPath Keylight kutucuğa tıkladığınızda, otomatik olarak LockPath Keylight uygulamanıza açan. 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/keylight-tutorial/tutorial_general_01.png
[2]: ./media/keylight-tutorial/tutorial_general_02.png
[3]: ./media/keylight-tutorial/tutorial_general_03.png
[4]: ./media/keylight-tutorial/tutorial_general_04.png

[100]: ./media/keylight-tutorial/tutorial_general_100.png

[200]: ./media/keylight-tutorial/tutorial_general_200.png
[201]: ./media/keylight-tutorial/tutorial_general_201.png
[202]: ./media/keylight-tutorial/tutorial_general_202.png
[203]: ./media/keylight-tutorial/tutorial_general_203.png

