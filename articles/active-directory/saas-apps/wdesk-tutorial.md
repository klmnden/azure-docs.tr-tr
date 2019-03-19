---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Wdesk | Microsoft Docs'
description: Azure Active Directory ve Wdesk arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 06900a91-a326-4663-8ba6-69ae741a536e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 75d0e962169529ab8d17aeeeed8aab26e7b7e994
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "57880883"
---
# <a name="tutorial-azure-active-directory-integration-with-wdesk"></a>Öğretici: Wdesk ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Wdesk tümleştirme konusunda bilgi edinin.

Azure AD ile Wdesk tümleştirme ile aşağıdaki avantajları sağlar:

- Wdesk erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Wdesk (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Wdesk yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir Wdesk çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Wdesk ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-wdesk-from-the-gallery"></a>Galeriden Wdesk ekleme
Azure AD'de Wdesk tümleştirmesini yapılandırmak için Wdesk Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Wdesk eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Wdesk**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/wdesk-tutorial/tutorial_wdesk_search.png)

1. Sonuçlar panelinde seçin **Wdesk**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/wdesk-tutorial/tutorial_wdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı Wdesk ile test etme

Tek iş için oturum açma için Azure AD ne Wdesk karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Wdesk ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değerini atayarak bu bağlantı ilişki kurulduktan **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** Wdesk içinde.

Yapılandırma ve Azure AD çoklu oturum açma Wdesk ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Wdesk test kullanıcısı oluşturma](#creating-a-wdesk-test-user)**  - kullanıcı Azure AD gösterimini bağlı Wdesk Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Wdesk uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Wdesk yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Wdesk** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/wdesk-tutorial/tutorial_wdesk_samlbase.png)

1. Üzerinde **Wdesk etki alanı ve URL'ler** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/wdesk-tutorial/tutorial_wdesk_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.wdesk.com/auth/saml/sp/metadata/<instancename>`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.wdesk.com/auth/saml/sp/consumer/<instancename>`

1. Denetleme **Gelişmiş URL ayarlarını göster**. Uygulamada yapılandırmak istiyorsanız **SP** modunda başlatılan aşağıdaki adımı uygulayın:

    ![Çoklu oturum açmayı yapılandırın](./media/wdesk-tutorial/tutorial_wdesk_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.wdesk.com/auth/login/saml/<instancename>`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. SSO yapılandırıldığında WDesk Portalı'ndan aşağıdaki değerleri alır. 
  
1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/wdesk-tutorial/tutorial_wdesk_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/wdesk-tutorial/tutorial_general_400.png)
    
1. Bir başka web tarayıcı penceresinde Wdesk bir güvenlik yöneticisi olarak oturum açın.

1. Alt sol, tıklayın **yönetici** ve **Hesap Yöneticisi**:
 
     ![Çoklu oturum açmayı yapılandırın](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

1. Wdesk yönetici gidin **güvenlik**, ardından **SAML** > **SAML ayarlarını**:

    ![Çoklu oturum açmayı yapılandırın](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig2.png)

1. Altında **genel ayarlar**, kontrol **SAML çoklu oturum açmayı etkinleştir**:

    ![Çoklu oturum açmayı yapılandırın](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig3.png)

1. Altında **hizmet sağlayıcısı ayrıntıları**, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig4.png)

      a. Kopyalama **oturum açma URL'si** yapıştırın **oturum açma URL'si** Azure portalında metin.
   
      b. Kopyalama **meta veri URL'si** yapıştırın **tanımlayıcı** Azure portalında metin.
       
      c. Kopyalama **tüketici url** yapıştırın **yanıt URL'si** Azure portalında metin.
   
      d. Tıklayın **Kaydet** değişiklikleri kaydetmek için Azure portalında.      

1. Tıklayın **IDP ayarlarını yapılandır** açmak için **IDP ayarlarını Düzenle** iletişim. Tıklayın **Dosya Seç** bulunacak **Metadata.xml** Azure portalından kaydettiğiniz dosyayı karşıya yükleyin.
    
    ![Çoklu oturum açmayı yapılandırın](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig5.png)
  
1. Tıklayın **değişiklikleri kaydetmek**.

    ![Çoklu oturum açmayı yapılandırın](./media/wdesk-tutorial/tutorial_wdesk_ssoconfigsavebutton.png)

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/wdesk-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/wdesk-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/wdesk-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/wdesk-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-wdesk-test-user"></a>Wdesk test kullanıcısı oluşturma

Wdesk için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların Wdesk sağlanması gerekir. Wdesk içinde sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. İçin Wdesk bir güvenlik yöneticisi olarak oturum açın.
1. Gidin **yönetici** > **yönetici hesabı**.

     ![Çoklu oturum açmayı yapılandırın](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

1. Tıklayın **üyeleri** altında **kişiler**.

1. Şimdi tıklayarak **Üye Ekle** açmak için **Üye Ekle** iletişim kutusu. 
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/wdesk-tutorial/createuser1.png)  

1. İçinde **kullanıcı** metin kutusunda, gibi kullanıcının kullanıcı adı girin **brittasimon\@contoso.com** tıklatıp **devam** düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/wdesk-tutorial/createuser3.png)

1.  Aşağıda gösterildiği gibi ayrıntılarını girin:
  
    ![Bir Azure AD test kullanıcısı oluşturma](./media/wdesk-tutorial/createuser4.png)
 
    a. İçinde **e-posta** metin kutusuna, kullanıcının gibi e-posta girin **brittasimon\@contoso.com**.

    b. İçinde **ad** metin kutusunda, gibi kullanıcı adını girin **Britta**.

    c. İçinde **Soyadı** metin kutusunda, son kullanıcı gibi adını **Simon**.

1. Tıklayın **Kaydet üye** düğmesi.  

    ![Bir Azure AD test kullanıcısı oluşturma](./media/wdesk-tutorial/createuser5.png)

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Wdesk erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Wdesk için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Wdesk**.

    ![Çoklu oturum açmayı yapılandırın](./media/wdesk-tutorial/tutorial_wdesk_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Wdesk kutucuğa tıkladığınızda, otomatik olarak Wdesk uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).


## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/wdesk-tutorial/tutorial_general_01.png
[2]: ./media/wdesk-tutorial/tutorial_general_02.png
[3]: ./media/wdesk-tutorial/tutorial_general_03.png
[4]: ./media/wdesk-tutorial/tutorial_general_04.png

[100]: ./media/wdesk-tutorial/tutorial_general_100.png

[200]: ./media/wdesk-tutorial/tutorial_general_200.png
[201]: ./media/wdesk-tutorial/tutorial_general_201.png
[202]: ./media/wdesk-tutorial/tutorial_general_202.png
[203]: ./media/wdesk-tutorial/tutorial_general_203.png

