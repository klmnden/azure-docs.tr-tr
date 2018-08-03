---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Samanage | Microsoft Docs'
description: Azure Active Directory ve Samanage arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f0db4fb0-7eec-48c2-9c7a-beab1ab49bc2
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 118ab72c9afc13c5792f229f9c7bc61d226553d5
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39420583"
---
# <a name="tutorial-azure-active-directory-integration-with-samanage"></a>Öğretici: Azure Active Directory Samanage ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Samanage tümleştirme konusunda bilgi edinin.

Azure AD ile Samanage tümleştirme ile aşağıdaki avantajları sağlar:

- Samanage erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Samanage (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Samanage yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik Samanage çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Samanage ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-samanage-from-the-gallery"></a>Galeriden Samanage ekleme
Azure AD'de Samanage tümleştirmesini yapılandırmak için Samanage Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Samanage eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Samanage**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/samanage-tutorial/tutorial_samanage_search.png)

1. Sonuçlar panelinde seçin **Samanage**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/samanage-tutorial/tutorial_samanage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Samanage sınayın.

Tek iş için oturum açma için Azure AD ne Samanage karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Samanage ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Samanage içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Samanage ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Samanage test kullanıcısı oluşturma](#creating-a-samanage-test-user)**  - kullanıcı Azure AD gösterimini bağlı Samanage Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Samanage uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Samanage yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Samanage** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/samanage-tutorial/tutorial_samanage_samlbase.png)

1. Üzerinde **Samanage etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/samanage-tutorial/tutorial_samanage_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<Company Name>.samanage.com/saml_login/<Company Name>`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<Company Name>.samanage.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler, öğreticinin ilerleyen bölümlerinde açıklanan tanımlayıcısı ve gerçek oturum açma URL'si ile güncelleştirin. Daha fazla ayrıntı başvurun [Samanage istemci Destek ekibine](https://www.samanage.com/support).    
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/samanage-tutorial/tutorial_samanage_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/samanage-tutorial/tutorial_general_400.png)

1. Üzerinde **Samanage yapılandırma** bölümünde **yapılandırma Samanage** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si ve SAML varlık kimliği** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/samanage-tutorial/tutorial_samanage_configure.png) 

1. Farklı bir web tarayıcı penceresinde Samanage şirket sitenize yönetici olarak oturum.

1. Tıklayın **Pano** seçip **Kurulum** sol gezinti bölmesinde.
   
    ![Pano](./media/samanage-tutorial/tutorial_samanage_001.png "Panosu")

1. Tıklayın **çoklu oturum açma**.
   
    ![Çoklu oturum açma](./media/samanage-tutorial/tutorial_samanage_002.png "çoklu oturum açma")

1. Gidin **SAML kullanarak bir oturum açma** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![SAML kullanarak bir oturum açma](./media/samanage-tutorial/tutorial_samanage_003.png "SAML ile oturum açın")
 
    a. Tıklayın **SAML ile çoklu oturum açmayı etkinleştirme**.  
 
    b. İçinde **kimlik sağlayıcısı URL'si** metin değerini yapıştırın **SAML varlık kimliği** , Azure Portalı'ndan kopyaladığınız.    
 
    c. Onayla **oturum açma URL'si** eşleşen **işareti bulunan URL'si** , **Samanage etki alanı ve URL'ler** bölümü Azure Portalı'nda.
 
    d. İçinde **oturum kapatma URL'si** metin değeri girin **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.
 
    e. İçinde **SAML veren** metin kutusu, türü kimlik sağlayıcınız uygulama kimliği URI'si ayarlayın.
 
    f. Not Defteri'nde Azure portalından indirilen, base-64 kodlanmış sertifika açın, içeriğini, panoya kopyalayın ve yapıştırın kendisine **, kimlik sağlayıcısı x.509 sertifikası aşağıya yapıştırın** metin.
 
    g. Tıklayın **Samanage içinde yoksa, kullanıcılar oluşturma**.
 
    h. Tıklayın **güncelleştirme**.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
 
### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/samanage-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/samanage-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/samanage-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/samanage-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-samanage-test-user"></a>Samanage test kullanıcısı oluşturma

Samanage için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların Samanage sağlanması gerekir.  
Samanage söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Samanage şirket sitenize yönetici olarak oturum açın.

1. Tıklayın **Pano** seçip **Kurulum** sol gezinti bölmesinde.
   
    ![Kurulum](./media/samanage-tutorial/tutorial_samanage_001.png "Kurulumu")

1. Tıklayın **kullanıcılar** sekmesi
   
    ![Kullanıcılar](./media/samanage-tutorial/tutorial_samanage_006.png "kullanıcılar")

1. Tıklayın **yeni kullanıcı**.
   
    ![Yeni kullanıcı](./media/samanage-tutorial/tutorial_samanage_007.png "yeni kullanıcı")

1. Tür **adı** ve **e-posta adresi** , önce sağlamak istediğiniz bir Azure Active Directory hesabının **kullanıcı oluşturma**.
   
    ![Kullanıcı oluşturma](./media/samanage-tutorial/tutorial_samanage_008.png "kullanıcı oluşturma")
   
   >[!NOTE]
   >Azure Active Directory hesap sahibinin e-posta alır ve bunu etkinleştirilmeden önce hesabını onaylamak için bağlantıyı izleyin. Herhangi diğer Samanage kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri tarafından Samanage sağlamak için Azure Active Directory kullanıcı hesaplarını sağlanan.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Samanage erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Samanage için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Samanage**.

    ![Çoklu oturum açmayı yapılandırın](./media/samanage-tutorial/tutorial_samanage_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Samanage kutucuğa tıkladığınızda, otomatik olarak Samanage uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/samanage-tutorial/tutorial_general_01.png
[2]: ./media/samanage-tutorial/tutorial_general_02.png
[3]: ./media/samanage-tutorial/tutorial_general_03.png
[4]: ./media/samanage-tutorial/tutorial_general_04.png

[100]: ./media/samanage-tutorial/tutorial_general_100.png

[200]: ./media/samanage-tutorial/tutorial_general_200.png
[201]: ./media/samanage-tutorial/tutorial_general_201.png
[202]: ./media/samanage-tutorial/tutorial_general_202.png
[203]: ./media/samanage-tutorial/tutorial_general_203.png

