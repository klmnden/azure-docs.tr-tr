---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Kintone | Microsoft Docs'
description: Azure Active Directory ve Kintone arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: c2b947dc-e1a8-4f5f-b40e-2c5180648e4f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jeedes
ms.openlocfilehash: 76e58ab9c7df77341e74c45cee438ac947ba507e
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54822425"
---
# <a name="tutorial-azure-active-directory-integration-with-kintone"></a>Öğretici: Kintone ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Kintone tümleştirme konusunda bilgi edinin.

Azure AD ile Kintone tümleştirme ile aşağıdaki avantajları sağlar:

- Kintone erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için bir Kintone (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Kintone yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir Kintone çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Kintone ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-kintone-from-the-gallery"></a>Galeriden Kintone ekleme
Azure AD'de Kintone tümleştirmesini yapılandırmak için Kintone Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Kintone eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Kintone**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/kintone-tutorial/tutorial_kintone_search.png)

1. Sonuçlar panelinde seçin **Kintone**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/kintone-tutorial/tutorial_kintone_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Kintone sınayın.

Tek iş için oturum açma için Azure AD ne Kintone karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Kintone ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Kintone içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Kintone ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bir Kintone test kullanıcısı oluşturma](#creating-a-kintone-test-user)**  - kullanıcı Azure AD gösterimini bağlı Kintone Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Kintone uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Kintone yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Kintone** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/kintone-tutorial/tutorial_kintone_samlbase.png)

1. Üzerinde **Kintone etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/kintone-tutorial/tutorial_kintone_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.kintone.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak:
    | |
    |--|
    | `https://<companyname>.cybozu.com`|
    | `https://<companyname>.kintone.com`|

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [Kintone istemci Destek ekibine](https://www.kintone.com/contact/) bu değerleri almak için. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/kintone-tutorial/tutorial_kintone_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/kintone-tutorial/tutorial_general_400.png)

1. Üzerinde **Kintone yapılandırma** bölümünde **yapılandırma Kintone** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/kintone-tutorial/tutorial_kintone_configure.png) 

1. Oturum başka bir web tarayıcı penceresinde açın, **Kintone** yönetici olarak şirketin site.

1. Tıklayın **ayarları**.
   
    ![Ayarları](./media/kintone-tutorial/ic785879.png "ayarları")

1. Tıklayın **kullanıcıları ve Sistem Yönetimi**.
   
    ![Kullanıcı ve Sistem Yönetimi](./media/kintone-tutorial/ic785880.png "kullanıcıları ve Sistem Yönetimi")

1. Altında **Sistem Yönetimi \> güvenlik** tıklayın **oturum açma**.
   
    ![Oturum açma](./media/kintone-tutorial/ic785881.png "oturum açma")

1. Tıklayın **etkinleştirme SAML kimlik doğrulaması**.
   
    ![SAML kimlik doğrulaması](./media/kintone-tutorial/ic785882.png "SAML kimlik doğrulaması")

1. SAML kimlik doğrulaması bölümünde, aşağıdaki adımları gerçekleştirin:
    
    ![SAML kimlik doğrulaması](./media/kintone-tutorial/ic785883.png "SAML kimlik doğrulaması")
    
    a. İçinde **oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız.
   
    b. İçinde **oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.
    
    c. Tıklayın **Gözat** indirilen sertifikanızı karşıya yüklemek için.
    
    d. **Kaydet**’e tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/kintone-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/kintone-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/kintone-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/kintone-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-kintone-test-user"></a>Bir Kintone test kullanıcısı oluşturma

Kintone için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların Kintone sağlanması gerekir.  
Kintone söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:

1. Oturum açın, **Kintone** yönetici olarak şirketin site.

1. Tıklayın **ayarı**.
   
    ![Ayarları](./media/kintone-tutorial/ic785879.png "ayarları")

1. Tıklayın **kullanıcıları ve Sistem Yönetimi**.
   
    ![Kullanıcı & Sistem Yönetimi](./media/kintone-tutorial/ic785880.png "kullanıcı & Sistem Yönetimi")

1. Altında **kullanıcı yönetimi**, tıklayın **Departmanlar k & ullanıcıların**.
   
    ![K & ullanıcıların departmanı](./media/kintone-tutorial/ic785888.png "k & ullanıcıların departmanı")

1. Tıklayın **yeni kullanıcı**.
   
    ![Yeni kullanıcılar](./media/kintone-tutorial/ic785889.png "yeni kullanıcılar")

1. İçinde **yeni kullanıcı** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Yeni kullanıcılar](./media/kintone-tutorial/ic785890.png "yeni kullanıcılar")
   
    a. Tür a **görünen adı**, **oturum açma adı**, **yeni parola**, **parolayı onaylayın**, **e-posta adresi**, ve ilgili metin kutularına zbilgisayarlar istediğiniz diğer ayrıntıları geçerli bir AAD hesabı.
 
    b. **Kaydet**’e tıklayın.

> [!NOTE]
> Herhangi diğer Kintone kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak Kintone tarafından sağlanan.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Kintone erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Kintone için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Kintone**.

    ![Çoklu oturum açmayı yapılandırın](./media/kintone-tutorial/tutorial_kintone_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test sağlamaktır.

Erişim panelinde Kintone kutucuğa tıkladığınızda, otomatik olarak Kintone uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/kintone-tutorial/tutorial_general_01.png
[2]: ./media/kintone-tutorial/tutorial_general_02.png
[3]: ./media/kintone-tutorial/tutorial_general_03.png
[4]: ./media/kintone-tutorial/tutorial_general_04.png

[100]: ./media/kintone-tutorial/tutorial_general_100.png

[200]: ./media/kintone-tutorial/tutorial_general_200.png
[201]: ./media/kintone-tutorial/tutorial_general_201.png
[202]: ./media/kintone-tutorial/tutorial_general_202.png
[203]: ./media/kintone-tutorial/tutorial_general_203.png

