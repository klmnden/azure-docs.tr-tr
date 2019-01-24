---
title: 'Öğretici: Görüntü geçişi ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve görüntü geçiş arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 65bb5990-07ef-4244-9f41-cd28fc2cb5a2
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 0671d2f5ad4be34926c8e3d2b22711c4af5fd435
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54812106"
---
# <a name="tutorial-azure-active-directory-integration-with-image-relay"></a>Öğretici: Görüntü geçişi ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile görüntü geçiş tümleştirme konusunda bilgi edinin.

Görüntü geçişi, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Görüntü geçişi erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan görüntü geçişi (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile görüntü geçişi yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir görüntü geçiş çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden görüntü geçiş ekleniyor
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-image-relay-from-the-gallery"></a>Galeriden görüntü geçiş ekleniyor
Azure AD'de görüntü geçiş tümleştirmesini yapılandırmak için yönetilen SaaS uygulamalar listesine Galeriden görüntü geçiş eklemeniz gerekir.

**Galeriden görüntü geçiş eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **görüntü geçiş**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/imagerelay-tutorial/tutorial_imagerelay_search.png)

1. Sonuçlar panelinde seçin **görüntü geçiş**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/imagerelay-tutorial/tutorial_imagerelay_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve görüntü "Britta Simon" adlı bir test kullanıcı tabanlı geçiş Azure AD çoklu oturum açmayı sınayın.

Tek çalışmak için oturum açma için Azure AD ne görüntü geçiş karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve görüntü geçiş ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Görüntü geçiş değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma görüntü geçişi ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bir görüntü geçiş test kullanıcısı oluşturma](#creating-an-image-relay-test-user)**  - kullanıcı Azure AD gösterimini bağlantılı görüntü geçiş Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve görüntü geçiş uygulamanızda çoklu oturum açmayı yapılandırma.

**Azure AD çoklu oturum açma ile görüntü geçişi yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **görüntü geçiş** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/imagerelay-tutorial/tutorial_imagerelay_samlbase.png)

1. Üzerinde **görüntü geçiş etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/imagerelay-tutorial/tutorial_imagerelay_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.imagerelay.com/`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.imagerelay.com/sso/metadata`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [görüntü geçiş istemci Destek ekibine](http://support.imagerelay.com/) bu değerleri almak için. 
 


1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/imagerelay-tutorial/tutorial_imagerelay_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/imagerelay-tutorial/tutorial_general_400.png)

1. Üzerinde **görüntü aktarma Yapılandırması** bölümünde **yapılandırma görüntü geçiş** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma hizmeti URL'si ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/imagerelay-tutorial/tutorial_imagerelay_configure.png) 

1. Başka bir tarayıcı penceresinde görüntü geçiş şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Üst araç çubuğunda tıklatın **kullanıcıları ve izinleri** iş yükü.
   
    ![Çoklu oturum açmayı yapılandırın](./media/imagerelay-tutorial/tutorial_imagerelay_06.png) 

1. Tıklayın **yeni bir izin oluşturmak**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/imagerelay-tutorial/tutorial_imagerelay_08.png)

1. İçinde **tek oturum açma ayarları** iş yükü, select **bu grubu için yalnızca oturum açma aracılığıyla çoklu oturum açma** onay kutusunu işaretleyin ve ardından **Kaydet**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/imagerelay-tutorial/tutorial_imagerelay_09.png) 

1. Git **hesap ayarları**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/imagerelay-tutorial/tutorial_imagerelay_10.png) 

1. Git **tek oturum açma ayarları** iş yükü.
    
     ![Çoklu oturum açmayı yapılandırın](./media/imagerelay-tutorial/tutorial_imagerelay_11.png)

1. Üzerinde **SAML ayarlarını** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
    
    ![Çoklu oturum açmayı yapılandırın](./media/imagerelay-tutorial/tutorial_imagerelay_12.png)
    
    a. İçinde **oturum açma URL'si** metin değerini yapıştırın **çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız.

    b. İçinde **oturum kapatma URL'si** metin değerini yapıştırın **çoklu oturum kapatma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız.

    c. Olarak **ad kimliği biçimi**seçin **urn: OASIS: adları: tc: SAML:1.1:nameid-biçimi: emailAddress**.

    d. Olarak **hizmet sağlayıcısı (görüntü geçiş) gelen istekleri için bağlama seçeneklerini**seçin **POST bağlama**.

    e. Altında **x.509 sertifikası**, tıklayın **güncelleştirme sertifika**.

    ![Çoklu oturum açmayı yapılandırın](./media/imagerelay-tutorial/tutorial_imagerelay_17.png)

    f. İndirilen sertifikanın Not Defteri'nde açın, içeriğini kopyalayın ve x.509 sertifikası metin kutusuna yapıştırın.

    ![Çoklu oturum açmayı yapılandırın](./media/imagerelay-tutorial/tutorial_imagerelay_18.png)

    g. İçinde **kullanıcı tam zamanında sağlama** bölümünden **tam zamanında kullanıcı sağlamayı etkinleştirin**.

    ![Çoklu oturum açmayı yapılandırın](./media/imagerelay-tutorial/tutorial_imagerelay_19.png)

    h. İzin grubu seçin (örneğin, **SSO temel**) yalnızca çoklu oturum açma oturum açmak için izin.

    ![Çoklu oturum açmayı yapılandırın](./media/imagerelay-tutorial/tutorial_imagerelay_20.png)

    i. **Kaydet**’e tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/imagerelay-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/imagerelay-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/imagerelay-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/imagerelay-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-an-image-relay-test-user"></a>Bir görüntü geçiş test kullanıcısı oluşturma

Bu bölümün amacı, görüntü geçiş Britta Simon adlı bir kullanıcı oluşturmaktır.

**Britta Simon görüntü geçiş adlı bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Görüntü geçiş şirketinizin sitesi için bir yönetici olarak oturum.

1. Git **kullanıcıları ve izinleri** seçip **SSO kullanıcı oluştur**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/imagerelay-tutorial/tutorial_imagerelay_21.png) 

1. Girin **e-posta**, **ad**, **Soyadı**, ve **şirket** sağlama ve (için izin grubu seçmek istediğiniz kullanıcının Örneğin, temel SSO) yalnızca çoklu oturum açma oturum grup olduğu.
   
    ![Çoklu oturum açmayı yapılandırın](./media/imagerelay-tutorial/tutorial_imagerelay_22.png) 

1. **Oluştur**’a tıklayın.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, görüntü geçiş için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon görüntü geçişi atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **görüntü geçiş**.

    ![Çoklu oturum açmayı yapılandırın](./media/imagerelay-tutorial/tutorial_imagerelay_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test sağlamaktır.    

Erişim panelinde görüntü geçiş kutucuğa tıkladığınızda, otomatik olarak görüntü geçiş uygulamanıza açan.


## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/imagerelay-tutorial/tutorial_general_01.png
[2]: ./media/imagerelay-tutorial/tutorial_general_02.png
[3]: ./media/imagerelay-tutorial/tutorial_general_03.png
[4]: ./media/imagerelay-tutorial/tutorial_general_04.png


[100]: ./media/imagerelay-tutorial/tutorial_general_100.png

[200]: ./media/imagerelay-tutorial/tutorial_general_200.png
[201]: ./media/imagerelay-tutorial/tutorial_general_201.png
[202]: ./media/imagerelay-tutorial/tutorial_general_202.png
[203]: ./media/imagerelay-tutorial/tutorial_general_203.png

