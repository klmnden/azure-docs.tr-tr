---
title: 'Öğretici: Marketo ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Marketo arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: b88c45f5-d288-4717-835c-ca965add8735
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: ac6d7c23c6bb107ce6920cce600a57143dafc62d
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54816441"
---
# <a name="tutorial-azure-active-directory-integration-with-marketo"></a>Öğretici: Marketo ile Azure Active Directory Tümleştirme

Bu öğreticide, Marketo Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

Marketo Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Marketo erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Marketo (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi, Marketo ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir Marketo çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Marketo galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-marketo-from-the-gallery"></a>Marketo galeri ekleme
Marketo tümleştirmesi Azure AD'de yapılandırmak için Marketo Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Marketo eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Marketo**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/marketo-tutorial/tutorial_marketo_search.png)

1. Sonuçlar panelinde seçin **Marketo**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/marketo-tutorial/tutorial_marketo_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı Marketo ile test etme

Tek çalışmak için oturum açma için Azure AD ne Marketo karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve Marketo ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Marketo içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Marketo ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Marketo test kullanıcısı oluşturma](#creating-a-marketo-test-user)**  - kullanıcı Azure AD gösterimini bağlı Marketo Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Marketo uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Marketo'ya yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Marketo** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_samlbase.png)

1. Üzerinde **Marketo etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://saml.marketo.com/sp`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://login.marketo.com/saml/assertion/\<munchkinid\>`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. İlgili kişi [Marketo Destek ekibine](http://investors.marketo.com/contactus.cfm) bu değerleri almak için.
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_general_400.png)

1. Üzerinde **Marketo yapılandırma** bölümünde **yapılandırma Marketo** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_configure.png) 

1. Uygulamanızın Munchkin Kimliği almak için Marketo yönetici kimlik bilgilerini kullanarak oturum açın ve aşağıdaki işlemleri gerçekleştirin:
   
    a. Marketo uygulamasına yönetici kimlik bilgilerini kullanarak oturum açın.
   
    b. Tıklayın **yönetici** üst gezinti bölmesindeki düğmesi.
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Tümleştirme menüsüne gidin ve tıklayın **Munchkin bağlantı**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_11.png)
   
    d. Munchkin ekranda gösterilen kodu kopyalayın ve kendi yanıt URL'si Azure AD Yapılandırma Sihirbazı tamamlayın.
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_12.png) 

1. İzleyin uygulamada SSO yapılandırmak için aşağıdaki adımları:
   
    a. Marketo uygulamasına yönetici kimlik bilgilerini kullanarak oturum açın.
   
    b. Tıklayın **yönetici** üst gezinti bölmesindeki düğmesi.
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Tümleştirme menüsüne gidin ve tıklayın **çoklu oturum açma**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_07.png) 
   
    d. SAML ayarlarını etkinleştirmek için tıklayın **Düzenle** düğmesi.
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_08.png) 
   
    e. **Etkin** çoklu oturum açma ayarları.
   
    f. Yapıştırma **SAML varlık kimliği**, **verenin kimliği** metin.
   
    g. İçinde **varlık kimliği** metin olarak URL'sini `http://saml.marketo.com/sp`.
   
    h. Kullanıcı Kimliği konum olarak seçin **ad tanımlayıcısı öğesi**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_09.png)
   
    > [!NOTE]
    > Kullanıcı tanımlayıcınızı değil UPN değeri öznitelik sekme değerinde değişiklik ise.
   
    i. Azure AD Yapılandırma Sihirbazı'ndan yüklediğiniz sertifikayı yükleyin. **Kaydet** ayarlar.
   
    j. Sayfaları yeniden yönlendirme ayarlarını düzenleyin.
   
    k. Yapıştırma **SAML çoklu oturum açma hizmeti URL'si** içinde **oturum açma URL'si** metin.
   
    m. Yapıştırma **oturum kapatma URL'si** içinde **oturum kapatma URL'si** metin.
   
    m. İçinde **hata URL**, kopyalama, **Marketo örnek URL'si** tıklatıp **Kaydet** düğmesini kullanarak ayarları kaydedin.
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_10.png)

1. Kullanıcılar için SSO'yu etkinleştirmek üzere, aşağıdaki işlemleri tamamlayın:
   
    a. Marketo uygulamasına yönetici kimlik bilgilerini kullanarak oturum açın.
   
    b. Tıklayın **yönetici** üst gezinti bölmesindeki düğmesi.
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Gidin **güvenlik** menüsüne ve ardından **oturum açma ayarları**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_13.png)
   
    d. Denetleme **gerektiren SSO** seçeneği ve **Kaydet** ayarlar.
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_14.png)

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/marketo-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/marketo-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/marketo-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/marketo-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-marketo-test-user"></a>Marketo test kullanıcısı oluşturma

Bu bölümde, Britta Simon Marketo adlı bir kullanıcı oluşturun. Marketo platformunda bir kullanıcı oluşturmak için aşağıdaki adımları izleyin.

1. Marketo uygulamasına yönetici kimlik bilgilerini kullanarak oturum açın.

1. Tıklayın **yönetici** üst gezinti bölmesindeki düğmesi.
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_06.png) 

1. Gidin **güvenlik** menüsüne ve ardından **kullanıcıları ve rolleri**
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_19.png)  

1. Tıklayın **yeni kullanıcı davet** kullanıcılar sekmesinde bağlantı
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_15.png) 

1. Yeni kullanıcı davet Sihirbazı'nda aşağıdaki bilgileri doldurun.
   
    a. Kullanıcının girmesi **e-posta** metin kutusuna adresi
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_16.png)
   
    b. Girin **ad** metin kutusuna
   
    c. Girin **Soyadı** metin kutusuna
   
    d. **İleri**’ye tıklayın

1. İçinde **izinleri** sekmesinde **userRoles** tıklatıp **İleri**
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_17.png)
1. Tıklayın **Gönder** düğmesini Kullanıcı Davet Gönder
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_18.png)

1. Kullanıcı e-posta bildirimi alır ve bağlantısına tıklayın ve hesabı etkinleştirmek için parolayı değiştirmesi gerekir. 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Marketo erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon için Marketo atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Marketo**.

    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Marketo kutucuğa tıkladığınızda, otomatik olarak Marketo uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/marketo-tutorial/tutorial_general_01.png
[2]: ./media/marketo-tutorial/tutorial_general_02.png
[3]: ./media/marketo-tutorial/tutorial_general_03.png
[4]: ./media/marketo-tutorial/tutorial_general_04.png

[100]: ./media/marketo-tutorial/tutorial_general_100.png

[200]: ./media/marketo-tutorial/tutorial_general_200.png
[201]: ./media/marketo-tutorial/tutorial_general_201.png
[202]: ./media/marketo-tutorial/tutorial_general_202.png
[203]: ./media/marketo-tutorial/tutorial_general_203.png

