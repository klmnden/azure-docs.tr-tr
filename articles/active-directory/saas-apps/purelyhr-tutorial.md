---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile PurelyHR | Microsoft Docs'
description: Azure Active Directory ve PurelyHR arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 86a9c454-596d-4902-829a-fe126708f739
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: jeedes
ms.openlocfilehash: 0e389f43586bd88f1d03a6cb8d714908d299977f
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54826403"
---
# <a name="tutorial-azure-active-directory-integration-with-purelyhr"></a>Öğretici: PurelyHR ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile PurelyHR tümleştirme konusunda bilgi edinin.

Azure AD ile PurelyHR tümleştirme ile aşağıdaki avantajları sağlar:

- PurelyHR erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için PurelyHR (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile PurelyHR yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir PurelyHR çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden PurelyHR ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-purelyhr-from-the-gallery"></a>Galeriden PurelyHR ekleme
Azure AD'de PurelyHR tümleştirmesini yapılandırmak için PurelyHR Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden PurelyHR eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **PurelyHR**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/purelyhr-tutorial/tutorial_purelyhr_search.png)

1. Sonuçlar panelinde seçin **PurelyHR**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/purelyhr-tutorial/tutorial_purelyhr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı PurelyHR ile test etme

Tek iş için oturum açma için Azure AD ne PurelyHR karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının PurelyHR ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

PurelyHR içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma PurelyHR ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[PurelyHR test kullanıcısı oluşturma](#creating-a-purelyhr-test-user)**  - kullanıcı Azure AD gösterimini bağlı PurelyHR Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve PurelyHR uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile PurelyHR yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **PurelyHR** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/purelyhr-tutorial/tutorial_purelyhr_samlbase.png)

1. Üzerinde **PurelyHR etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![Çoklu oturum açmayı yapılandırın](./media/purelyhr-tutorial/tutorial_purelyhr_url.png)
   
    İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyID>.purelyhr.com/sso-consume`

1. Denetleme **Gelişmiş URL ayarlarını göster**, uygulamada yapılandırmak istiyorsanız **SP** başlatılan modu:

    ![Çoklu oturum açmayı yapılandırın](./media/purelyhr-tutorial/tutorial_purelyhr_url1.png)
    
    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak değeri yazın: `https://<companyID>.purelyhr.com/sso-initiate`
     
    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [PurelyHR istemci Destek ekibine](https://support.purelyhr.com/) bu değerleri almak için. 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/purelyhr-tutorial/tutorial_purelyhr_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/purelyhr-tutorial/tutorial_general_400.png)
    
1. Üzerinde **PurelyHR yapılandırma** bölümünde **yapılandırma PurelyHR** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/purelyhr-tutorial/tutorial_purelyhr_configure.png) 

1. Çoklu oturum açmayı yapılandırma **PurelyHR** tarafı, Web sitesi yönetici olarak oturum açın.

1. Açık **Pano** tıklayıp araç seçeneklerinden **SSO ayarlarını**.

1. Aşağıdaki - açıklandığı kutularında değerleri yapıştırın

    ![Çoklu oturum açmayı yapılandırın](./media/purelyhr-tutorial/purelyhr-dashboard-sso-settings.png)  

    a. Açık **Certificate(Bas64)** Defteri'nde Azure portalından indirilen ve sertifika değerini kopyalayın. Kopyalanan değer içine yapıştırın **X.509 sertifikası** kutusu.

    b. İçinde **IDP veren URL'si** kutusu, yapıştırma **SAML varlık kimliği** Azure portalından kopyalanır.

    c. İçinde **IDP uç nokta URL'si** kutusu, yapıştırma **SAML çoklu oturum açma hizmeti URL'si** Azure portalından kopyalanır. 

    d. Denetleme **kullanıcılar otomatik olarak oluşturma** içinde PurelyHR otomatik kullanıcı sağlamayı etkinleştirmek için onay kutusu.

    e. Tıklayın **Değişiklikleri Kaydet** ayarları kaydetmek için.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/purelyhr-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/purelyhr-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/purelyhr-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/purelyhr-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-purelyhr-test-user"></a>PurelyHR test kullanıcısı oluşturma

PurelyHR için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların PurelyHR sağlanması gerekir. PurelyHR, sağlama otomatik bir görevdir ve otomatik kullanıcı hazırlama etkin olduğunda herhangi bir el ile adım gerekli değildir.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için PurelyHR erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon PurelyHR için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **PurelyHR**.

    ![Çoklu oturum açmayı yapılandırın](./media/purelyhr-tutorial/tutorial_purelyhr_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde devralarak LMS kutucuğa tıklayarak, otomatik olarak imzalanmış devralarak LMS uygulamanıza açma.

Erişim paneli hakkında daha fazla bilgi için bkz. [Erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/purelyhr-tutorial/tutorial_general_01.png
[2]: ./media/purelyhr-tutorial/tutorial_general_02.png
[3]: ./media/purelyhr-tutorial/tutorial_general_03.png
[4]: ./media/purelyhr-tutorial/tutorial_general_04.png

[100]: ./media/purelyhr-tutorial/tutorial_general_100.png

[200]: ./media/purelyhr-tutorial/tutorial_general_200.png
[201]: ./media/purelyhr-tutorial/tutorial_general_201.png
[202]: ./media/purelyhr-tutorial/tutorial_general_202.png
[203]: ./media/purelyhr-tutorial/tutorial_general_203.png

