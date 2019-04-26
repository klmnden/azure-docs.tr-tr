---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle E Sales Manager Remix | Microsoft Docs'
description: Azure Active Directory arasında E Sales Manager Remix çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 89b5022c-0d5b-4103-9877-ddd32b6e1c02
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 895fb0d83e383618818325263ac80c5919a0ee7b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60429248"
---
# <a name="integrate-azure-active-directory-with-e-sales-manager-remix"></a>Azure Active Directory E satış yöneticisi Remix ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile satış yöneticisi Remix E tümleştirme konusunda bilgi edinin.

Azure AD, E Sales Manager Remix ile tümleştirerek, aşağıdaki avantajlardan yararlanabilirsiniz:

- E Sales Manager Remix erişimi, Azure AD'de kontrol edebilirsiniz.
- Oturumunuz otomatik olarak E Sales Manager Remix için (çoklu oturum açma veya SSO) ile Azure AD hesaplarına kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda, Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile satış yöneticisi Remix E yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir E Sales Manager Remix SSO etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test ettiğinizde, bunu yapmanızı öneririz *değil* bir üretim ortamı kullanın.

Bu öğreticideki adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. 

Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

* Galeriden E Sales Manager Remix ekleme
* Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="add-e-sales-manager-remix-from-the-gallery"></a>E Sales Manager Remix Galeriden Ekle
Satış Yöneticisi E Remix ile Azure AD'nin tümleştirmesini yapılandırmak için E Sales Manager Remix Galeriden yönetilen SaaS uygulamaları listenize aşağıdakileri yaparak ekleyin:

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory**. 

    ![Azure Active Directory düğmesi][1]

1. Seçin **kurumsal uygulamalar** > **tüm uygulamaları**.

    !["Kurumsal uygulamalar" penceresi][2]
    
1. Yeni bir uygulama eklemek için seçin **yeni uygulama** pencerenin üst kısmındaki.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **E Sales Manager Remix**seçin **E Sales Manager Remix** sonuç listesini ve ardından **Ekle**.

    ![Sonuç listesinde Sales Manager Remix E](./media/esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma E, satış yöneticisi "Britta Simon." adlı bir test kullanıcı tabanlı Remix ile test etme

Tek iş için oturum açma için Azure AD E Sales Manager Remix kullanıcı ve kendisine karşılık gelen Azure AD'de tanımlaması gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının E Sales Manager Remix de aynı kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma E Sales Manager Remix ile test, yapı taşlarını sonraki beş bölümlerde tamamlamak için:

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma E Sales Manager Remix uygulamanızda aşağıdakileri yaparak yapılandırın:

1. Azure portalında, üzerinde **E Sales Manager Remix** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    !["Çoklu oturum açma" bağlantısı][4]

1. İçinde **çoklu oturum açma** penceresi içinde **çoklu oturum açma modunu** kutusunda **SAML tabanlı oturum açma**.
 
    !["Çoklu oturum açma" penceresi](./media/esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_samlbase.png)

1. Altında **E Sales Manager Remix etki ve URL'leri**, aşağıdakileri yapın:

    ![E Sales Manager Remix etki ve URL'leri tek oturum açma bilgileri](./media/esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_url.png)

    a. İçinde **oturum açma URL'si** kutusuna bir URL aşağıdaki biçimde yazın: *https://\<sunucu temel URL'si > /\<alt etki alanı > / esales-pc*.

    b. İçinde **tanımlayıcı** kutusuna bir URL aşağıdaki biçimde yazın: *https://\<sunucu temel URL'si > /\<alt etki alanı > /*.

    c. Not **tanımlayıcı** Bu öğreticide daha sonra kullanmak için değer.
    
    > [!NOTE] 
    > Yukarıdaki değerleri gerçek değildir. Bunları tanımlayıcısı ve gerçek oturum açma URL'si ile güncelleştirin. Değerlerini almak için iletişime geçin [E Sales Manager Remix istemci Destek ekibine](mailto:esupport@softbrain.co.jp).

1. Altında **SAML imzalama sertifikası**seçin **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika (Base64) indirme bağlantısı](./media/esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_certificate.png) 

1. Seçin **görünümü ve diğer tüm kullanıcı özniteliklerini düzenleyin** onay kutusunu işaretleyin ve ardından **emailaddress** özniteliği.
    
    ![Kullanıcı öznitelikleri penceresi](./media/esalesmanagerremix-tutorial/configure1.png)

    **Özniteliğini Düzenle** penceresi açılır.

1. Kopyalama **Namespace** ve **adı** değerleri. Desen değeri üretmek  *\<Namespace > /\<adı >* ve Bu öğreticide daha sonra kullanmak için kaydedin.

    ![Özniteliği düzenleme penceresi](./media/esalesmanagerremix-tutorial/configure2.png)

1. Altında **E Sales Manager yapılandırması Remix**seçin **yapılandırma E Sales Manager Remix**.

    ![E satış yöneticisi Remix yapılandırma](./media/esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_configure.png) 

    **Yapılandırma oturum açma** penceresi açılır.

1. İçinde **hızlı başvuru** bölümünde, oturum kapatma URL'si ve SAML çoklu oturum açma hizmeti URL'sini kopyalayın.

1. **Kaydet**’i seçin.

    ![Kaydet düğmesi](./media/esalesmanagerremix-tutorial/tutorial_general_400.png)

1. E Sales Manager Remix uygulamanıza yönetici olarak oturum açın.

1. Sağ üst kısımdaki seçin **yönetici menüsüne**.

    !["Yönetici menüsü" komutu](./media/esalesmanagerremix-tutorial/configure4.png)

1. Sol bölmede seçin **sistem ayarlarını** > **dış sistem ile işbirliği yaparak**.

    !["Sistem" ve "Dış Sistem işbirliği" Bağlantılarım](./media/esalesmanagerremix-tutorial/configure5.png)
    
1. İçinde **dış sistem ile işbirliği yaparak** penceresinde **SAML**.

    !["Dış Sistem işbirliği" penceresi](./media/esalesmanagerremix-tutorial/configure6.png)

1. Altında **SAML kimlik doğrulaması ayarı**, aşağıdakileri yapın:

    !["SAML kimlik doğrulaması ayarlama" bölümüne](./media/esalesmanagerremix-tutorial/configure3.png)
    
    a. Seçin **PC Sürüm** onay kutusu.
    
    b. İçinde **işbirliği öğesi** bölümünde aşağı açılan listesinde seçin **e-posta**.

    c. İçinde **işbirliği öğesi** kutusunda, daha önce Azure portaldan kopyaladığınız talep değerini yapıştırın (diğer bir deyişle, **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**).

    d. İçinde **veren (varlık kimliği)** kutusunda, daha önce kopyaladığınız öğesinden tanımlayıcı değerini yapıştırın **E Sales Manager Remix etki ve URL'leri** Azure portal'ın bölümü.

    e. Azure portalından indirilen sertifikanızı karşıya yüklemek için seçin **dosya seçimi**.

    f. İçinde **kimlik sağlayıcısı oturum açma URL'si** kutusuna, Azure portalda daha önce kopyaladığınız SAML çoklu oturum açma hizmeti URL'sini yapıştırın.

    g. İçinde **kimlik sağlayıcısı oturum kapatma URL'si** kutusuna, Azure portalda daha önce kopyaladığınız oturum kapatma URL'si değerini yapıştırın.

    h. Seçin **tam ayarını**.

> [!TIP]
> Uygulamayı hazırlama tutunun gibi önceki yönergeleri kısa bir sürümünü edinebilirsiniz [Azure portalında](https://portal.azure.com). Uygulamayı ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölümünden **çoklu oturum açma** sekmesine ve ardından erişim belgelerinde katıştırılmış **yapılandırma** alttaki bölümü. Ekli belge özelliği hakkında daha fazla bilgi için bkz. [Azure AD belgeleri katıştırılmış]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, aşağıdakileri yaparak Azure portalında, Britta Simon test kullanıcısı oluşturun:

![Bir Azure AD test kullanıcısı oluşturma][100]

1. Azure portalında, sol bölmede seçin **Azure Active Directory**.

    ![Azure Active Directory bağlantısı](./media/paloaltoadmin-tutorial/create_aaduser_01.png)

1. Geçerli kullanıcıların bir listesini görüntülemek için seçin **kullanıcılar ve gruplar** > **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/paloaltoadmin-tutorial/create_aaduser_02.png)

1. Üst kısmındaki **tüm kullanıcılar** penceresinde **Ekle**.

    ![Ekle düğmesi](./media/paloaltoadmin-tutorial/create_aaduser_03.png)
    
    **Kullanıcı** penceresi açılır.

1. İçinde **kullanıcı** penceresinde aşağıdakileri yapın:

    ![Kullanıcı penceresi](./media/paloaltoadmin-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri Not **parola** kutusu.

    d. **Oluştur**’u seçin.
 
### <a name="create-an-e-sales-manager-remix-test-user"></a>Bir E Sales Manager Remix test kullanıcısı oluşturma

1. E Sales Manager Remix uygulamanıza yönetici olarak oturum açın.

1. Seçin **yönetici menüsüne** sağ üst köşedeki menüden.

    ![E satış yöneticisi Remix yapılandırma](./media/esalesmanagerremix-tutorial/configure4.png)

1. Seçin **şirketinizin ayarları** > **Departmanlar ve çalışan Bakımı**ve ardından **kaydedilmiş çalışanlar**.

    !["Çalışanlar registered" sekmesi](./media/esalesmanagerremix-tutorial/user1.png)

1. İçinde **yeni çalışan kaydı** bölümünde, aşağıdakileri yapın:
    
    !["Yeni çalışan kaydı" bölümü](./media/esalesmanagerremix-tutorial/user2.png)

    a. İçinde **çalışan adı** kullanıcının adını yazın (örneğin, **Britta**).

    b. Kalan gerekli alanları doldurun.
    
    c. SAML etkinleştirirseniz, yönetici oturum açma sayfasında oturum açamazsınız. Oturum açma izni yönetici ayrıcalıkları seçerek kullanıcı **yönetici oturum açma** onay kutusu.

    d. Seçin **kayıt**.

1. Gelecekte, yönetici olarak oturum açmak için sonra sağ üst kısımdaki'i seçin ve yönetici izinlerine sahip kullanıcı olarak oturum açma **yönetici menüsüne**.

    !["Yönetici menüsü" komutu](./media/esalesmanagerremix-tutorial/configure4.png)

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, kullanıcı için satış yöneticisi Remix E erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin. Bunu yapmak için aşağıdakileri yapın: 

![Kullanıcı rolü atayın][200] 

1. Azure portalında açın **uygulamaları** görünümü, Git **dizin** görüntüleyin ve ardından **kurumsal uygulamalar** > **tüm uygulamaları**.

    !["Kurumsal uygulamalar" ve "Tüm uygulamalar" bağlantıları][201] 

1. İçinde **uygulamaları** listesinden **E Sales Manager Remix**.

    ![E Sales Manager Remix bağlantı](./media/esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_app.png)  

1. Sol bölmede seçin **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Seçin **Ekle** ve daha sonra **atama Ekle** bölmesinde **kullanıcılar ve gruplar**.

    ![Atama Ekle bölmesi][203]

1. İçinde **kullanıcılar ve gruplar** penceresi içinde **kullanıcılar** listesinden **Britta Simon**.

1. Seçin **seçin** düğmesi.

1. İçinde **atama Ekle** penceresinde **atama**.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Erişim Paneli'nde E Sales Manager Remix kutucuğu seçtiğinizde, otomatik olarak E Sales Manager Remix uygulamanıza oturum açmanız.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/esalesmanagerremix-tutorial/tutorial_general_01.png
[2]: ./media/esalesmanagerremix-tutorial/tutorial_general_02.png
[3]: ./media/esalesmanagerremix-tutorial/tutorial_general_03.png
[4]: ./media/esalesmanagerremix-tutorial/tutorial_general_04.png

[100]: ./media/esalesmanagerremix-tutorial/tutorial_general_100.png

[200]: ./media/esalesmanagerremix-tutorial/tutorial_general_200.png
[201]: ./media/esalesmanagerremix-tutorial/tutorial_general_201.png
[202]: ./media/esalesmanagerremix-tutorial/tutorial_general_202.png
[203]: ./media/esalesmanagerremix-tutorial/tutorial_general_203.png

