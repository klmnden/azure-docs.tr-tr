---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Menlo güvenlik | Microsoft Docs'
description: Azure Active Directory Menlo güvenlik arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 9e63fe6b-0ad0-405d-9e41-6a1a40a41df8
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: jeedes
ms.openlocfilehash: 70b6693afe1a57e8acd62500d74f860dffc7c692
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54808706"
---
# <a name="tutorial-azure-active-directory-integration-with-menlo-security"></a>Öğretici: Menlo güvenlik ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Menlo güvenlik tümleştirme konusunda bilgi edinin.

Menlo güvenliği Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Menlo güvenlik erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan Menlo güvenlik (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Menlo güvenliği yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir Menlo güvenlik çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Menlo güvenlik ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-menlo-security-from-the-gallery"></a>Galeriden Menlo güvenlik ekleme
Azure AD'de Menlo güvenlik tümleştirmesini yapılandırmak için Menlo güvenlik Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Menlo güvenlik eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Menlo güvenlik**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/menlosecurity-tutorial/tutorial_menlosecurity_search.png)

1. Sonuçlar panelinde seçin **Menlo güvenlik**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/menlosecurity-tutorial/tutorial_menlosecurity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Menlo "Britta Simon." adlı bir test kullanıcı tabanlı güvenlik ile test etme

Tek iş için oturum açma için Azure AD ne Menlo güvenlik karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı Menlo güvenlik arasında bir bağlantı ilişki kurulması gerekir.

Değerini atayarak bu bağlantı ilişki kurulduktan **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** Menlo güvenlik.

Yapılandırma ve Azure AD çoklu oturum açma Menlo güvenlik ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Menlo güvenlik test kullanıcısı oluşturma](#creating-a-menlo-security-test-user)**  - bir karşılığı Britta simon'un kullanıcı Azure AD gösterimini bağlı Menlo güvenlik sağlamak için.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Menlo güvenlik uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Menlo güvenliği yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Menlo güvenlik** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/menlosecurity-tutorial/tutorial_menlosecurity_samlbase.png)

1. Üzerinde **Menlo güvenlik etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/menlosecurity-tutorial/tutorial_menlosecurity_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.menlosecurity.com/account/login`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.menlosecurity.com/safeview-auth-server/saml/metadata`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [Menlo güvenlik istemcisi Destek ekibine](https://www.menlosecurity.com/menlo-contact) bu değerleri almak için. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/menlosecurity-tutorial/tutorial_menlosecurity_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/menlosecurity-tutorial/tutorial_general_400.png)

1. Üzerinde **Menlo Güvenlik Yapılandırması** bölümünde **Menlo Güvenlik Yapılandırması** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği**, ve **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/menlosecurity-tutorial/tutorial_menlosecurity_configure.png) 

1. Çoklu oturum açmayı yapılandırma **Menlo güvenlik** tarafı, oturum açma **Menlo güvenlik** yönetici olarak Web sitesi.

1. Altında **ayarları** Git **kimlik doğrulaması** ve aşağıdaki işlemleri gerçekleştirin:
    
    ![Çoklu oturum açmayı yapılandırın](./media/menlosecurity-tutorial/menlo_user_setup.png)

    a. Onay kutusunu işaretleyerek **SAML kullanarak kullanıcı kimlik doğrulamasını etkinleştirme**.

    b. Seçin **dış erişim izni** için **Evet**.

    c. Altında **SAML sağlayıcısı**seçin **Azure Active Directory**.

    d. **SAML 2.0 uç nokta** : Yapıştırma **SAML çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız.

    e. **Hizmet tanımlayıcısı (veren)** : Yapıştırma **SAML varlık kimliği** , Azure Portalı'ndan kopyaladığınız.

    f. **X.509 sertifikası** : Açık **sertifika (Base64)** Defteri'nde Azure portalından indirilen ve bu kutuya yapıştırın.

    g. Ayarları kaydetmek için **Kaydet**’e tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/menlosecurity-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/menlosecurity-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/menlosecurity-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/menlosecurity-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-menlo-security-test-user"></a>Menlo güvenlik test kullanıcısı oluşturma
 
Bu bölümde, Britta Simon Menlo güvenlik adlı bir kullanıcı oluşturun. Çalışmak [Menlo güvenlik istemcisi Destek ekibine](https://www.menlosecurity.com/menlo-contact) Menlo güvenlik platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi. 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma Menlo Security'ye erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Menlo güvenlik atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Menlo güvenlik**.

    ![Çoklu oturum açmayı yapılandırın](./media/menlosecurity-tutorial/tutorial_menlosecurity_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı test edin.

Yeni bir kimlik doğrulaması tetiklemek için bir "InPrivate" veya "Gizli" modunda bir tarayıcı penceresi açın.  Internet Explorer'da, Ctrl + Shift + P kullanın.  Chrome'da, Ctrl + Shift + N kullanın.  Özel tarayıcı penceresinde, korunan bir kaynağa gidin ve bir Azure AD oturum açma işlemi gerçekleştirin.  Başarıyla oturum açtıktan sonra istenen site için bir yalıtım oturumda alınır.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/menlosecurity-tutorial/tutorial_general_01.png
[2]: ./media/menlosecurity-tutorial/tutorial_general_02.png
[3]: ./media/menlosecurity-tutorial/tutorial_general_03.png
[4]: ./media/menlosecurity-tutorial/tutorial_general_04.png

[100]: ./media/menlosecurity-tutorial/tutorial_general_100.png

[200]: ./media/menlosecurity-tutorial/tutorial_general_200.png
[201]: ./media/menlosecurity-tutorial/tutorial_general_201.png
[202]: ./media/menlosecurity-tutorial/tutorial_general_202.png
[203]: ./media/menlosecurity-tutorial/tutorial_general_203.png

