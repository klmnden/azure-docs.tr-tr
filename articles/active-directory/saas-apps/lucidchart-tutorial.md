---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Lucidchart | Microsoft Docs'
description: Azure Active Directory ve Lucidchart arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 1068d364-11f3-43b5-bd6d-26f00ecd5baa
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: eccb6efc95b274545a4d74bbd1ace7ab5629214f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55169051"
---
# <a name="tutorial-azure-active-directory-integration-with-lucidchart"></a>Öğretici: Lucidchart ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Lucidchart tümleştirme konusunda bilgi edinin.

Azure AD ile Lucidchart tümleştirme ile aşağıdaki avantajları sağlar:

- Lucidchart erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Lucidchart (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Lucidchart yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Lucidchart çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Lucidchart ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-lucidchart-from-the-gallery"></a>Galeriden Lucidchart ekleme
Azure AD'de Lucidchart tümleştirmesini yapılandırmak için Lucidchart Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Lucidchart eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Lucidchart**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/lucidchart-tutorial/tutorial_lucidchart_search.png)

1. Sonuçlar panelinde seçin **Lucidchart**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/lucidchart-tutorial/tutorial_lucidchart_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Lucidchart sınayın.

Tek iş için oturum açma için Azure AD ne Lucidchart karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Lucidchart ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Lucidchart içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Lucidchart ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Lucidchart test kullanıcısı oluşturma](#creating-a-lucidchart-test-user)**  - kullanıcı Azure AD gösterimini bağlı Lucidchart Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Lucidchart uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Lucidchart yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Lucidchart** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/lucidchart-tutorial/tutorial_lucidchart_samlbase.png)

1. Üzerinde **Lucidchart etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/lucidchart-tutorial/tutorial_lucidchart_url.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL: `https://chart2.office.lucidchart.com/saml/sso/azure`

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/lucidchart-tutorial/tutorial_lucidchart_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/lucidchart-tutorial/tutorial_general_400.png)

1. Farklı bir web tarayıcı penceresinde Lucidchart şirket sitenize yönetici olarak oturum.

1. Üstteki menüden **takım**.
   
    ![Takım](./media/lucidchart-tutorial/ic791190.png "takım")

1. Tıklayın **uygulamaları \> SAML yönetme**.
   
    ![SAML yönetme](./media/lucidchart-tutorial/ic791191.png "SAML yönetme")

1. Üzerinde **SAML kimlik doğrulaması ayarlarını** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    a. Seçin **SAML kimlik doğrulamasını etkinleştir**ve ardından **isteğe bağlı**.

    ![SAML kimlik doğrulaması ayarlarını](./media/lucidchart-tutorial/ic791192.png "SAML kimlik doğrulama ayarları")
 
    b. İçinde **etki alanı** metin etki alanınızı girin ve ardından **değişiklik sertifika**.

    ![Sertifika değiştirme](./media/lucidchart-tutorial/ic791193.png "sertifikasını değiştirme")
 
    c. İndirilen meta verileri dosyanızı açın, içeriği kopyalayın ve ardından yapıştırın **meta verilerini karşıya yükleme** metin.

    ![Meta verilerini karşıya yükleme](./media/lucidchart-tutorial/ic791194.png "meta verilerini karşıya yükleme")
 
    d. Seçin **takım için yeni kullanıcı otomatik olarak Ekle**ve ardından **değişiklikleri kaydetmek**.

    ![Değişiklikleri kaydetmek](./media/lucidchart-tutorial/ic791195.png "Değişiklikleri Kaydet")

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/lucidchart-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/lucidchart-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/lucidchart-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/lucidchart-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-lucidchart-test-user"></a>Lucidchart test kullanıcısı oluşturma

Kullanıcı için Lucidchart sağlama yapılandırmanız için hiçbir eylem öğesini yoktur.  Atanan kullanıcı Lucidchart erişim panelini kullanarak açmaya çalıştığında Lucidchart kullanıcının var olup olmadığını denetler.  

Varsa kullanıcı hesabı kullanılabilir henüz Lucidchart tarafından otomatik olarak oluşturulur.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Lucidchart erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Lucidchart için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Lucidchart**.

    ![Çoklu oturum açmayı yapılandırın](./media/lucidchart-tutorial/tutorial_lucidchart_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Lucidchart kutucuğa tıkladığınızda, otomatik olarak Lucidchart uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/lucidchart-tutorial/tutorial_general_01.png
[2]: ./media/lucidchart-tutorial/tutorial_general_02.png
[3]: ./media/lucidchart-tutorial/tutorial_general_03.png
[4]: ./media/lucidchart-tutorial/tutorial_general_04.png

[100]: ./media/lucidchart-tutorial/tutorial_general_100.png

[200]: ./media/lucidchart-tutorial/tutorial_general_200.png
[201]: ./media/lucidchart-tutorial/tutorial_general_201.png
[202]: ./media/lucidchart-tutorial/tutorial_general_202.png
[203]: ./media/lucidchart-tutorial/tutorial_general_203.png

