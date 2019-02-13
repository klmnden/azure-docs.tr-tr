---
title: 'Öğretici: SkyDesk e-posta ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve SkyDesk e-posta arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: a9d0bbcb-ddb5-473f-a4aa-028ae88ced1a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8caad0a3c75ab269aa2be93e3b338695dd29caa6
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56209379"
---
# <a name="tutorial-azure-active-directory-integration-with-skydesk-email"></a>Öğretici: SkyDesk e-posta ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile SkyDesk e-posta tümleştirme konusunda bilgi edinin.

Azure AD ile SkyDesk e-posta tümleştirme ile aşağıdaki avantajları sağlar:

- SkyDesk e-posta erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan SkyDesk e-posta (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi SkyDesk e-posta ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik SkyDesk e-posta çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme burada alabilirsiniz [deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden SkyDesk e-posta ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-skydesk-email-from-the-gallery"></a>Galeriden SkyDesk e-posta ekleme
Azure AD'de SkyDesk e-posta tümleştirmesini yapılandırmak için SkyDesk e-posta Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden SkyDesk e-posta eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **SkyDesk e-posta**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/skydeskemail-tutorial/tutorial_skydeskemail_search.png)

1. Sonuçlar panelinde seçin **SkyDesk e-posta**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/skydeskemail-tutorial/tutorial_skydeskemail_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırmanız ve SkyDesk e-posta ile Azure AD çoklu oturum açmayı test "Britta Simon" adlı bir test kullanıcı tabanlı.

Tek iş için oturum açma için Azure AD ne SkyDesk e-posta karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının SkyDesk e-posta ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değerini SkyDesk e-postada atama **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma SkyDesk e-posta ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[SkyDesk e-posta test kullanıcısı oluşturma](#creating-a-skydesk-email-test-user)**  - SkyDesk postada kullanıcının Azure AD gösterimini bağlı Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma SkyDesk e-posta uygulamanızı yapılandırın.

**Azure AD çoklu oturum açma SkyDesk e-posta ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **SkyDesk e-posta** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/skydeskemail-tutorial/tutorial_skydeskemail_samlbase.png)

1. Üzerinde **SkyDesk e-posta etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/skydeskemail-tutorial/tutorial_skydeskemail_url.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://mail.skydesk.jp/portal/<companyname>`

    > [!NOTE] 
    > Değer, gerçek değil. Değerini gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [SkyDesk e-posta istemcisi Destek ekibine](https://www.skydesk.jp/apps/support/) değeri alınamıyor. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/skydeskemail-tutorial/tutorial_skydeskemail_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/skydeskemail-tutorial/tutorial_general_400.png)

1. Üzerinde **SkyDesk e-posta yapılandırmasını** bölümünde **SkyDesk e-posta Yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/skydeskemail-tutorial/tutorial_skydeskemail_configure.png) 

1. İçinde SSO'yu etkinleştirmek üzere **SkyDesk e-posta**, aşağıdaki adımları gerçekleştirin:

    a. SkyDesk e-posta hesabınıza yönetici olarak oturum.

    b. Üstteki menüden **Kurulum**seçip **kuruluş**. 
    
      ![Çoklu oturum açmayı yapılandırın](./media/skydeskemail-tutorial/tutorial_skydeskemail_51.png)
  
    c. Tıklayarak **etki alanları** sol panelden.
    
      ![Çoklu oturum açmayı yapılandırın](./media/skydeskemail-tutorial/tutorial_skydeskemail_53.png)

    d. Tıklayarak **etki alanı ekleme**.
    
      ![Çoklu oturum açmayı yapılandırın](./media/skydeskemail-tutorial/tutorial_skydeskemail_54.png)

    e. Etki alanı adınızı girin ve sonra etki alanını doğrulayın.
    
      ![Çoklu oturum açmayı yapılandırın](./media/skydeskemail-tutorial/tutorial_skydeskemail_55.png)

    f. Tıklayarak **SAML kimlik doğrulaması** sol panelden.
    
      ![Çoklu oturum açmayı yapılandırın](./media/skydeskemail-tutorial/tutorial_skydeskemail_52.png)

1. Üzerinde **SAML kimlik doğrulaması** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
      ![Çoklu oturum açmayı yapılandırın](./media/skydeskemail-tutorial/tutorial_skydeskemail_56.png)
   
    >[!NOTE]
    >SAML tabanlı kimlik doğrulaması kullanmak için ya da olması **etki alanını doğruladıysanız** veya **portal URL** kurulumu. Portal ayarlayabilirsiniz URL'si ile benzersiz adı.
    > 
    > 
   
    ![Çoklu oturum açmayı yapılandırın](./media/skydeskemail-tutorial/tutorial_skydeskemail_57.png)

    a. İçinde **oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure Portalı'ndan kopyaladığınız.
   
    b. İçinde **oturum kapatma** URL metin değerini yapıştırın **oturum kapatma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    c. **Parola URL'sini değiştirmek** isteğe bağlı olduğundan bu nedenle boş bırakın.

    d. Tıklayarak **anahtarı dosyadan al** Azure portalından indirilen sertifikanızı seçin ve ardından **açık** sertifikayı karşıya yüklemek için.

    e. Olarak **algoritması**seçin **RSA**.

    f. Tıklayın **Tamam** değişiklikleri kaydedin.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/skydeskemail-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/skydeskemail-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/skydeskemail-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/skydeskemail-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-skydesk-email-test-user"></a>SkyDesk e-posta test kullanıcısı oluşturma

Bu bölümde, Britta Simon SkyDesk e-posta adlı bir kullanıcı oluşturun.

1. Tıklayarak **kullanıcı erişimini** Sol paneli SkyDesk e-postada ve kullanıcı adınızı girin. 

    ![Çoklu oturum açmayı yapılandırın](./media/skydeskemail-tutorial/tutorial_skydeskemail_58.png)

>[!NOTE] 
>Toplu kullanıcı oluşturmak ihtiyacınız varsa, iletişime geçmeniz [SkyDesk e-posta istemcisi Destek ekibine](https://www.skydesk.jp/apps/support/).


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma SkyDesk e-postasına erişim verme tarafından kullanmak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon SkyDesk e-posta atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **SkyDesk e-posta**.

    ![Çoklu oturum açmayı yapılandırın](./media/skydeskemail-tutorial/tutorial_skydeskemail_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD SSO yapılandırmanızı sınamanızı sağlamaktır.

Erişim panelinde SkyDesk e-posta kutucuğa tıkladığınızda, otomatik olarak SkyDesk e-posta uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/skydeskemail-tutorial/tutorial_general_01.png
[2]: ./media/skydeskemail-tutorial/tutorial_general_02.png
[3]: ./media/skydeskemail-tutorial/tutorial_general_03.png
[4]: ./media/skydeskemail-tutorial/tutorial_general_04.png

[100]: ./media/skydeskemail-tutorial/tutorial_general_100.png

[200]: ./media/skydeskemail-tutorial/tutorial_general_200.png
[201]: ./media/skydeskemail-tutorial/tutorial_general_201.png
[202]: ./media/skydeskemail-tutorial/tutorial_general_202.png
[203]: ./media/skydeskemail-tutorial/tutorial_general_203.png

