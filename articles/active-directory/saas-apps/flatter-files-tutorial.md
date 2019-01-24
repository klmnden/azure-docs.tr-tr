---
title: 'Öğretici: Azure Active Directory Tümleştirmesi düzleştiren dosyalarla | Microsoft Docs'
description: Azure Active Directory ve düzleştiren dosyaları arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: f86fe5e3-0e91-40d6-869c-3df6912d27ea
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: aaf6c6ba17e41c4d32aafa98dbd2c1dc2532e197
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54808315"
---
# <a name="tutorial-azure-active-directory-integration-with-flatter-files"></a>Öğretici: Azure Active Directory tümleştirmesiyle düzleştiren dosyaları

Bu öğreticide, Azure Active Directory (Azure AD) ile tümleştirme düzleştiren dosyalarını öğrenin.

Düzleştiren dosyaları Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Düzleştiren dosyalara erişimi olan Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) düzleştiren dosyaları açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi düzleştiren dosyalarıyla yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik düzleştiren dosyaları çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden düzleştiren dosya ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-flatter-files-from-the-gallery"></a>Galeriden düzleştiren dosya ekleme
Azure AD'de düzleştiren dosyalarının tümleştirmesini yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden düzleştiren dosyaları eklemeniz gerekir.

**Galeriden düzleştiren dosyaları eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **düzleştiren dosyaları**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/flatter-files-tutorial/tutorial_flatterfiles_search.png)

1. Sonuçlar panelinde seçin **düzleştiren dosyaları**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/flatter-files-tutorial/tutorial_flatterfiles_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma düzleştiren "Britta Simon" adlı bir test kullanıcı tabanlı dosyaları test.

Tek iş için oturum açma için Azure AD ne düzleştiren dosyalarındaki karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı düzleştiren dosyalarında arasında bir bağlantı ilişki kurulması gerekir.

Düzleştiren dosyalarında değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma ile düzleştiren dosyaları test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Düzleştiren dosyaları test kullanıcısı oluşturma](#creating-a-flatter-files-test-user)**  - kullanıcı Azure AD gösterimini bağlı düzleştiren dosyalarındaki Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve düzleştiren dosyaları uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma düzleştiren dosyalarıyla yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **düzleştiren dosyaları** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/flatter-files-tutorial/tutorial_flatterfiles_samlbase.png)

1. Üzerinde **düzleştiren dosyaları etki alanı ve URL'ler** bölümü, kullanıcı gerekmez uygulama zaten Azure ile önceden tümleştirilmiştir gibi tüm adımları gerçekleştirin.

    ![Çoklu oturum açmayı yapılandırın](./media/flatter-files-tutorial/tutorial_flatterfiles_url.png)
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/flatter-files-tutorial/tutorial_flatterfiles_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/flatter-files-tutorial/tutorial_general_400.png)

1. Üzerinde **düzleştiren dosyaları yapılandırma** bölümünde **yapılandırma düzleştiren dosyaları** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/flatter-files-tutorial/tutorial_flatterfiles_configure.png) 

1. Düzleştiren dosyaları uygulamanıza yönetici olarak oturum.

1. Tıklayın **PANO**. 
   
    ![Çoklu oturum açmayı yapılandırın](./media/flatter-files-tutorial/tutorial_flatter_files_05.png)  

1. Tıklayın **ayarları**ve ardından üzerinde aşağıdaki adımları gerçekleştirin **şirket** sekmesinde: 
   
    ![Çoklu oturum açmayı yapılandırın](./media/flatter-files-tutorial/tutorial_flatter_files_06.png)  
    
    a. Seçin **SAML 2.0 kimlik doğrulaması için kullanmak**.
    
    b. Tıklayın **SAML'yi yapılandırmak**.

1. Üzerinde **SAML yapılandırma** iletişim kutusunda, aşağıdaki adımları gerçekleştirin: 
   
    ![Çoklu oturum açmayı yapılandırın](./media/flatter-files-tutorial/tutorial_flatter_files_08.png)  
   
    a. İçinde **etki alanı** metin kayıtlı etki alanınızı girin.
   
    >[!NOTE]
    >Bir kaydedilmiş bir etki alanı henüz kişi yoksa düzleştiren dosyalarınızı aracılığıyla destek [ support@flatterfiles.com ](mailto:support@flatterfiles.com). 
    
    b. İçinde **kimlik sağlayıcısı URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si** kopyaladığınız Azure portalı form.
   
    c.  Base-64 kodlanmış sertifikanızı Not Defteri'nde açın, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **kimlik sağlayıcısı sertifikası** metin.

    d. Tıklayın **güncelleştirme**.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/flatter-files-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/flatter-files-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/flatter-files-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/flatter-files-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-flatter-files-test-user"></a>Düzleştiren dosyaları test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon düzleştiren dosyalarında adlı bir kullanıcı oluşturmaktır.

**Britta Simon düzleştiren dosyalarında adlı bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **düzleştiren dosyaları** şirketinizin sitesi yöneticisi olarak.

1. Sol taraftaki gezinti bölmesinde **ayarları**ve ardından **kullanıcılar** sekmesi.
   
    ![Düzleştiren dosyaları kullanıcı oluşturma](./media/flatter-files-tutorial/tutorial_flatter_files_09.png)

1. Tıklayın **kullanıcı ekleme**. 

1. Üzerinde **Kullanıcı Ekle** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Düzleştiren dosyaları kullanıcı oluşturma](./media/flatter-files-tutorial/tutorial_flatter_files_10.png)

    a. İçinde **ad** metin kutusuna **Britta**.
   
    b. İçinde **Soyadı** metin kutusuna **Simon**. 
   
    c. İçinde **e-posta adresi** metin kutusuna, Azure portalında Britta'nın e-posta adresini yazın.
   
    d. **Gönder**'e tıklayın.   


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, düzleştiren dosyalara erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon düzleştiren dosyaları atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **düzleştiren dosyaları**.

    ![Çoklu oturum açmayı yapılandırın](./media/flatter-files-tutorial/tutorial_flatterfiles_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde düzleştiren dosyaları kutucuğa tıkladığınızda, otomatik olarak düzleştiren dosyaları uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/flatter-files-tutorial/tutorial_general_01.png
[2]: ./media/flatter-files-tutorial/tutorial_general_02.png
[3]: ./media/flatter-files-tutorial/tutorial_general_03.png
[4]: ./media/flatter-files-tutorial/tutorial_general_04.png

[100]: ./media/flatter-files-tutorial/tutorial_general_100.png

[200]: ./media/flatter-files-tutorial/tutorial_general_200.png
[201]: ./media/flatter-files-tutorial/tutorial_general_201.png
[202]: ./media/flatter-files-tutorial/tutorial_general_202.png
[203]: ./media/flatter-files-tutorial/tutorial_general_203.png

