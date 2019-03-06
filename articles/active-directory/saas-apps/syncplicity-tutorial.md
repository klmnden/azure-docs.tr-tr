---
title: 'Öğretici: Syncplicity ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Syncplicity arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 896a3211-f368-46d7-95b8-e4768c23be08
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5f6fcc4d2920841c730ef179497f9184b1f6649d
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57451969"
---
# <a name="tutorial-azure-active-directory-integration-with-syncplicity"></a>Öğretici: Syncplicity ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Syncplicity tümleştirme konusunda bilgi edinin.

Syncplicity Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Syncplicity erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Syncplicity (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Syncplicity yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Syncplicity çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Syncplicity galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-syncplicity-from-the-gallery"></a>Syncplicity galeri ekleme
Azure AD'de Syncplicity tümleştirmesini yapılandırmak için Syncplicity Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Syncplicity Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Syncplicity**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/syncplicity-tutorial/tutorial_syncplicity_search.png)

1. Sonuçlar panelinde seçin **Syncplicity**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/syncplicity-tutorial/tutorial_syncplicity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı Syncplicity ile test etme

Tek iş için oturum açma için Azure AD ne Syncplicity karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Syncplicity ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Syncplicity içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Syncplicity ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Syncplicity test kullanıcısı oluşturma](#creating-a-syncplicity-test-user)**  - kullanıcı Azure AD gösterimini bağlı Syncplicity Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Syncplicity uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Syncplicity yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Syncplicity** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/syncplicity-tutorial/tutorial_syncplicity_samlbase.png)

1. Üzerinde **Syncplicity etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/syncplicity-tutorial/tutorial_syncplicity_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.syncplicity.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.syncplicity.com/sp`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [Syncplicity istemci Destek ekibine](https://www.syncplicity.com/contact-us) bu değerleri almak için. 
 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/syncplicity-tutorial/tutorial_syncplicity_certificate.png) 

  
1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/syncplicity-tutorial/tutorial_general_400.png)

1. Üzerinde **Syncplicity yapılandırma** bölümünde **yapılandırma Syncplicity** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/syncplicity-tutorial/tutorial_syncplicity_configure.png) 

1. Oturum açın, **Syncplicity** Kiracı.

1. Üstteki menüden **yönetici**seçin **ayarları**ve ardından **özel etki alanı ve çoklu oturum açma**.
   
    ![Syncplicity](./media/syncplicity-tutorial/ic769545.png "Syncplicity")

1. Üzerinde **çoklu oturum açma (SSO)** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma \(SSO\)](./media/syncplicity-tutorial/ic769550.png "Single Sign-On \\\(SSO\\\)")   

    a. İçinde **özel etki alanı** metin etki alanınızın adını yazın.
  
    b. Seçin **etkin** olarak **çoklu oturum açma durumu**.

    c. İçinde **varlık kimliği** metin değerini yapıştırın **SAML varlık kimliği** , Azure Portalı'ndan kopyaladığınız.

    d. İçinde **oturum açma sayfası URL'si** metin kutusu, yapıştırma **SAML çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız.

    e. İçinde **oturum kapatma sayfasını URL'si** metin kutusu, yapıştırma **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.

    f. İçinde **kimlik sağlayıcısı sertifikası**, tıklayın **dosya**ve sonra Azure portalından indirdiğiniz sertifika karşıya yükleyin. 

    g. Tıklayın **değişiklikleri kaydetme**.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/syncplicity-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/syncplicity-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/syncplicity-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/syncplicity-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-syncplicity-test-user"></a>Syncplicity test kullanıcısı oluşturma
AAD kullanıcıları oturum açabilmesi bunlar Syncplicity uygulama sağlanmalıdır. Bu bölümde Syncplicity AAD kullanıcı hesapları oluşturma işlemini açıklar.

**Bir kullanıcı hesabına Syncplicity sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **Syncplicity** Kiracı (örneğin: `https://company.Syncplicity.com`).

1. Tıklayın **yönetici** seçip **kullanıcı hesaplarını**.

1. Tıklayın **kullanıcı ekleme**.
   
    ![Kullanıcıları Yönet](./media/syncplicity-tutorial/ic769764.png "kullanıcıları yönetme")

1. Tür **e-posta adreslerini** seçin, sağlamak istediğiniz bir AAD hesabı **kullanıcı** olarak **rol**ve ardından **sonraki**.
   
    ![Hesap bilgileri](./media/syncplicity-tutorial/ic769765.png "hesap bilgileri")
   
    >[!NOTE]
    >AAD hesap sahibinin onaylayın ve hesap etkinleştirme bağlantısı içeren bir e-posta alır. 
    > 

1. Bir grubu seçin, yeni kullanıcı bir üyesi olun ve ardından, şirketinizde **sonraki**.
   
    ![Grup üyeliği](./media/syncplicity-tutorial/ic769772.png "grup üyeliği")
   
    >[!NOTE]
    >Listelenen hiçbir grupları varsa, tıklayın **sonraki**. 
    > 

1. Kullanıcının bilgisayarında Syncplicity'nın denetimi altına yerleştirin ve ardından istediğiniz klasörleri seçin **sonraki**.
   
    ![Syncplicity klasörleri](./media/syncplicity-tutorial/ic769773.png "Syncplicity klasörleri")

>[!NOTE]
>Herhangi diğer Syncplicity kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak Syncplicity tarafından sağlanan. 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Syncplicity erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Syncplicity için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Syncplicity**.

    ![Çoklu oturum açmayı yapılandırın](./media/syncplicity-tutorial/tutorial_syncplicity_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test sağlamaktır.

Erişim panelinde Syncplicity kutucuğa tıkladığınızda, otomatik olarak Syncplicity uygulamanıza açan.
## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/syncplicity-tutorial/tutorial_general_01.png
[2]: ./media/syncplicity-tutorial/tutorial_general_02.png
[3]: ./media/syncplicity-tutorial/tutorial_general_03.png
[4]: ./media/syncplicity-tutorial/tutorial_general_04.png

[100]: ./media/syncplicity-tutorial/tutorial_general_100.png

[200]: ./media/syncplicity-tutorial/tutorial_general_200.png
[201]: ./media/syncplicity-tutorial/tutorial_general_201.png
[202]: ./media/syncplicity-tutorial/tutorial_general_202.png
[203]: ./media/syncplicity-tutorial/tutorial_general_203.png

