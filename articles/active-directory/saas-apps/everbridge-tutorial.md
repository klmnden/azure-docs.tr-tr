---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile EverBridge | Microsoft Docs'
description: Azure Active Directory ve EverBridge arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 58d7cd22-98c0-4606-9ce5-8bdb22ee8b3e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d1bb62c9a11971f72a6c96c4652b136c19812cb3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60279508"
---
# <a name="tutorial-azure-active-directory-integration-with-everbridge"></a>Öğretici: EverBridge ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile EverBridge tümleştirme konusunda bilgi edinin.

Azure AD ile EverBridge tümleştirme ile aşağıdaki avantajları sağlar:

- EverBridge erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için EverBridge (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile EverBridge yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir EverBridge çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden EverBridge ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-everbridge-from-the-gallery"></a>Galeriden EverBridge ekleme

Azure AD'de EverBridge tümleştirmesini yapılandırmak için EverBridge Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden EverBridge eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **EverBridge**seçin **EverBridge** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde EverBridge](./media/everbridge-tutorial/tutorial_everbridge_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı EverBridge sınayın.

Tek iş için oturum açma için Azure AD ne EverBridge karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının EverBridge ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma EverBridge ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir EverBridge test kullanıcısı oluşturma](#creating-an-everbridge-test-user)**  - kullanıcı Azure AD gösterimini bağlı EverBridge Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve EverBridge uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile EverBridge yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **EverBridge** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunu tıklatın **seçin** için **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](common/tutorial_general_301.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Çoklu oturum açmayı yapılandırın](common/editconfigure.png)

    >[!NOTE]
    >Manager portalı ya da başka bir deyişle Azure portalı ve Everbridge portalı üzerinde her iki ucunda üye portalı olarak uygulamanın yapılandırmalar yapmanız gerekir.

4. Yapılandırmak için **EverBridge** olarak uygulama **EverBridge Manager portalı**, **temel SAML yapılandırma** bölümünde aşağıdaki adımları gerçekleştirin:

    ![EverBridge etki alanı ve URL'ler tek oturum açma bilgileri](./media/everbridge-tutorial/tutorial_everbridge_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://sso.everbridge.net/<API_Name>`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://manager.everbridge.net/saml/SSO/<API_Name>/alias/defaultAlias`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. İlgili kişi [EverBridge Destek ekibine](mailto:support@everbridge.com) bu değerleri almak için.

5. Yapılandırmak için **EverBridge** olarak uygulama **EverBridge üye portalı**, **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

   * Uygulamada yapılandırmak istiyorsanız **IDP** başlatılan modu:

       ![EverBridge etki alanı ve URL'ler tek oturum açma bilgileri](./media/everbridge-tutorial/tutorial_everbridge_url1.png)

       * İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://sso.everbridge.net/<API_Name>/<Organization_ID>`

       * İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://member.everbridge.net/saml/SSO/<API_Name>/<Organization_ID>/alias/defaultAlias`

   * Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

       ![EverBridge etki alanı ve URL'ler tek oturum açma bilgileri](./media/everbridge-tutorial/tutorial_everbridge_url2.png)

       * İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://member.everbridge.net/saml/login/<API_Name>/<Organization_ID>/alias/defaultAlias?disco=true`

     > [!NOTE]
     > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [EverBridge Destek ekibine](mailto:support@everbridge.com) bu değerleri almak için.

6. Üzerinde **SAML imzalama sertifikası** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta verileri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/everbridge-tutorial/tutorial_everbridge_certificate.png) 

7. Üzerinde **EverBridge kümesi** bölümünde, ihtiyacınıza göre uygun URL'yi kopyalayın.

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

    ![EverBridge yapılandırma](common/configuresection.png)

8. SSO için yapılandırılmış almak için **EverBridge** olarak **EverBridge Manager portalı** uygulama, aşağıdaki adımları gerçekleştirin: 
 
9. Bir başka web tarayıcı penceresinde EverBridge yönetici olarak oturum açın.

9. Üstteki menüden **ayarları** sekmenize **çoklu oturum açma** altında **güvenlik**.
   
     ![Çoklu oturum açmayı yapılandırın](./media/everbridge-tutorial/tutorial_everbridge_002.png)
   
     a. İçinde **adı** metin kimlik sağlayıcısının adını yazın (örneğin: şirketinizin adı).
   
     b. İçinde **API adı** metin API adını yazın.
   
     c. Tıklayın **Dosya Seç** düğmesine Azure portalından indirilen meta veri dosyasını karşıya yükleyin.
   
     d. SAML kimlik konumda seçin **kimliğidir konu deyiminin NameIdentifier öğesinde**.
   
     e. İçinde **kimlik sağlayıcısı oturum açma URL'si** metin değerini yapıştırın **oturum açma URL'si** , Azure Portalı'ndan kopyaladığınız.
   
     f. Hizmet sağlayıcısı tarafından başlatılan istek bağlamasındaki seçin **HTTP yeniden yönlendirme**.

     g. **Kaydet**’e tıklayın

10. Çoklu oturum açmayı yapılandırma **EverBridge** olarak uygulama **EverBridge üye portalı**, indirilen göndermem gerekiyor **Federasyon meta verileri XML** için [ Everbridge Destek ekibine](mailto:support@everbridge.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![Azure AD kullanıcısı oluşturun][100]

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Bir Azure AD test kullanıcısı oluşturma](common/create_aaduser_01.png) 

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Bir Azure AD test kullanıcısı oluşturma](common/create_aaduser_02.png)

    a. İçinde **adı** alanına **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alanına **brittasimon\@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **özellikleri**seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’u seçin.
  
### <a name="creating-an-everbridge-test-user"></a>Bir EverBridge test kullanıcısı oluşturma

Bu bölümde, Britta Simon Everbridge içinde adlı bir kullanıcı oluşturun. Çalışmak [EverBridge Destek ekibine](mailto:support@everbridge.com) Everbridge platform kullanıcıları eklemek için.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için EverBridge erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **EverBridge**.

    ![Çoklu oturum açmayı yapılandırın](./media/everbridge-tutorial/tutorial_everbridge_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.

### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde EverBridge kutucuğa tıkladığınızda, otomatik olarak EverBridge uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
