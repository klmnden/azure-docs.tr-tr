---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle DocuSign | Microsoft Docs'
description: Azure Active Directory ve DocuSign arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: a691288b-84c1-40fb-84bd-5b06878865f0
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: 5d8aef1edf4d7a02686db48d3e788e4f9493c398
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39448494"
---
# <a name="tutorial-azure-active-directory-integration-with-docusign"></a>Öğretici: Azure Active Directory tümleştirmesiyle DocuSign

Bu öğreticide, DocuSign, Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.

DocuSign'ı Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- DocuSign erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan DocuSign'ın (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

DocuSign ve Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik bir DocuSign çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. DocuSign galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-docusign-from-the-gallery"></a>DocuSign galeri ekleme
Azure AD'ye DocuSign tümleştirmesini yapılandırmak için DocuSign galerideki yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden DocuSign eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Tıklayın **yeni uygulama** iletişim kutusunun üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **DocuSign**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/docusign-tutorial/tutorial_docusign_search.png)

1. Sonuçlar panelinde seçin **DocuSign**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/docusign-tutorial/tutorial_docusign_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı DocuSign ile test etme

Tek çalışmak için oturum açma için Azure AD ne DocuSign karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının DocuSign ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değerini atayarak bu bağlantı ilişki kurulduktan **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** DocuSign içinde.

Yapılandırma ve Azure AD çoklu oturum açma DocuSign ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[DocuSign test kullanıcısı oluşturma](#creating-a-docusign-test-user)**  - kullanıcı Azure AD gösterimini bağlı DocuSign Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve DocuSign uygulamanızda çoklu oturum açmayı yapılandırın.

**DocuSign ve Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **DocuSign** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/docusign-tutorial/tutorial_docusign_samlbase.png)

1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base 64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/docusign-tutorial/tutorial_docusign_certificate.png) 

1. Üzerinde **DocuSign yapılandırma** bölümü Azure portalının tıklatın **yapılandırma DocuSign** yapılandırma oturum açmak için. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**
    
    ![Çoklu oturum açmayı yapılandırın](./media/docusign-tutorial/tutorial_docusign_configure.png)

1. Bir oturum açma için farklı bir web tarayıcı penceresinde, **DocuSign Yönetici portalı** yönetici olarak.

1. Sol gezinti menüsünde **etki alanları**.
   
    ![Çoklu oturum açma yapılandırılıyor][51]

1. Sağ bölmede, tıklayın **talep etki alanı**.
   
    ![Çoklu oturum açma yapılandırılıyor][52]

1. Üzerinde **bir etki alanı talep** iletişim, **etki alanı adı** metin şirket etki alanınızı girin ve ardından **talep**. Etki alanını doğrulayın ve durumun etkin olduğundan emin olun.
   
    ![Çoklu oturum açma yapılandırılıyor][53]

1. İşlecin sol tarafındaki menüde **kimlik sağlayıcıları**  
   
    ![Çoklu oturum açma yapılandırılıyor][54]
1. Sağ bölmede **kimlik sağlayıcısı Ekle**. 
   
    ![Çoklu oturum açma yapılandırılıyor][55]

1. Üzerinde **kimlik sağlayıcı ayarları** sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma yapılandırılıyor][56]

    a. İçinde **adı** metin yapılandırmanız için benzersiz bir ad yazın. Boşluk kullanmayın.

    b. Yapıştırma **SAML varlık kimliği** içine **kimlik sağlayıcısını veren** metin.

    c. Yapıştırma **SAML çoklu oturum açma hizmeti URL'si** içine **kimlik sağlayıcısı oturum açma URL'si** metin.

    d. Yapıştırma **oturum kapatma URL'si** içine **kimlik sağlayıcısı oturum kapatma URL'si** metin.

    e. Seçin **AuthN isteğini imzalamak**.

    f. Olarak **AuthN gönderme isteği tarafından**seçin **POST**.

    g. Olarak **gönderme oturum kapatma isteği tarafından**seçin **alma**.

1. İçinde **özel öznitelik eşlemesi** bölümünde, Azure AD talep ile eşlemek istediğiniz alanı seçin. Bu örnekte, **emailaddress** talep değeriyle eşleşen **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**. Bu e-posta talebi için Azure AD'den varsayılan talep adıdır. 
   
    > [!NOTE]
    > Uygun kullanın **kullanıcı tanımlayıcısı** kullanıcının Azure AD'den DocuSign kullanıcı eşleme eşlemek için. Uygun alanı seçmek ve kuruluş ayarlarınıza göre uygun değeri girin.
          
    ![Çoklu oturum açma yapılandırılıyor][57]

1. İçinde **kimlik sağlayıcısı sertifikası** bölümünde **sertifika Ekle**ve ardından Azure AD Portalı'ndan indirilen sertifikayı karşıya yükleyin.   
   
    ![Çoklu oturum açma yapılandırılıyor][58]

1. **Kaydet**’e tıklayın.

1. İçinde **kimlik sağlayıcıları** bölümünde **eylemleri**ve ardından **uç noktaları**.   
   
    ![Çoklu oturum açma yapılandırılıyor][59]
 
1. İçinde **uç noktalarını görüntüle SAML 2.0** bölümünde **DocuSign Yönetici portalı**, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma yapılandırılıyor][60]
   
    a. Kopyalama **hizmet sağlayıcısı veren URL'si**ve ardından yapıştırın **tanımlayıcı** metin üzerinde **DocuSign etki alanı ve URL'ler** bölüm düzenini izleyerek Azure portal'ın: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/login/sp/<uniqueID>`.
   
    b. Kopyalama **hizmet sağlayıcısı oturum açma URL'si**ve ardından yapıştırın **işareti bulunan URL'si** metin üzerinde **DocuSign etki alanı ve URL'ler** bölüm düzenini izleyerek Azure portal'ın: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/` .

    ![Çoklu oturum açmayı yapılandırın](./media/docusign-tutorial/tutorial_docusign_url.png)
      
    c.  Tıklayın **Kapat**
    
1. Azure Portal'da tıklayın **Kaydet**.
    
    ![Çoklu oturum açmayı yapılandırın](./media/docusign-tutorial/tutorial_general_400.png)

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/docusign-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/docusign-tutorial/create_aaduser_02.png) 

1. İletişim kutusunun en üstünde tıklayın **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/docusign-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/docusign-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-docusign-test-user"></a>DocuSign test kullanıcısı oluşturma

Uygulamanızın desteklediği **zaman kullanıcı hazırlama, sadece** ve sonra kimlik doğrulama kullanıcıları uygulama içinde otomatik olarak oluşturulur.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için DocuSign erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon için DocuSign atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **DocuSign**.

    ![Çoklu oturum açmayı yapılandırın](./media/docusign-tutorial/tutorial_docusign_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde DocuSign kutucuğa tıkladığınızda, otomatik olarak DocuSign uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Kullanıcı sağlamayı yapılandırma](docusign-provisioning-tutorial.md)


<!--Image references-->

[1]: ./media/docusign-tutorial/tutorial_general_01.png
[2]: ./media/docusign-tutorial/tutorial_general_02.png
[3]: ./media/docusign-tutorial/tutorial_general_03.png
[4]: ./media/docusign-tutorial/tutorial_general_04.png
[51]: ./media/docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/docusign-tutorial/tutorial_docusign_29.png
[100]: ./media/docusign-tutorial/tutorial_general_100.png

[200]: ./media/docusign-tutorial/tutorial_general_200.png
[201]: ./media/docusign-tutorial/tutorial_general_201.png
[202]: ./media/docusign-tutorial/tutorial_general_202.png
[203]: ./media/docusign-tutorial/tutorial_general_203.png

