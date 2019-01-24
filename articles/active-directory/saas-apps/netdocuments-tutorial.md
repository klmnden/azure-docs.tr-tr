---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile NetDocuments | Microsoft Docs'
description: Azure Active Directory ve NetDocuments arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 1a47dc42-1a17-48a2-965e-eca4cfb2f197
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 279a5700b7d9934afa1f772a282f276580a168ec
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54817911"
---
# <a name="tutorial-azure-active-directory-integration-with-netdocuments"></a>Öğretici: NetDocuments ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile NetDocuments tümleştirme konusunda bilgi edinin.

Azure AD ile NetDocuments tümleştirme ile aşağıdaki avantajları sağlar:

- NetDocuments erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için NetDocuments (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile NetDocuments yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik NetDocuments çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden NetDocuments ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-netdocuments-from-the-gallery"></a>Galeriden NetDocuments ekleme
Azure AD'de NetDocuments tümleştirmesini yapılandırmak için NetDocuments Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden NetDocuments eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **NetDocuments**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/netdocuments-tutorial/tutorial_netdocuments_search.png)

1. Sonuçlar panelinde seçin **NetDocuments**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/netdocuments-tutorial/tutorial_netdocuments_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı NetDocuments sınayın.

Tek iş için oturum açma için Azure AD ne NetDocuments karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının NetDocuments ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

NetDocuments içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma NetDocuments ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[NetDocuments test kullanıcısı oluşturma](#creating-a-netdocuments-test-user)**  - kullanıcı Azure AD gösterimini bağlı NetDocuments Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve NetDocuments uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile NetDocuments yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **NetDocuments** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/netdocuments-tutorial/tutorial_netdocuments_samlbase.png)

1. Üzerinde **NetDocuments etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/netdocuments-tutorial/tutorial_netdocuments_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<user identifier>`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<user identifier>`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve yanıt URL'si ile güncelleştirin. İlgili kişi [NetDocuments Destek ekibine](https://support.netdocuments.com/hc/) bu değerleri almak için.
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/netdocuments-tutorial/tutorial_netdocuments_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/netdocuments-tutorial/tutorial_general_400.png)

1. Farklı bir web tarayıcı penceresinde NetDocuments şirket sitenize yönetici olarak oturum.

1. Git **yönetici**.

1. Tıklayın **ekleme ve kaldırma kullanıcılar ve gruplar**.
   
    ![Depo](./media/netdocuments-tutorial/ic795047.png "depo")

1. Tıklayın **Gelişmiş kimlik doğrulama Seçenekleri Yapılandır**.
    
    ![Gelişmiş kimlik doğrulama Seçenekleri Yapılandır](./media/netdocuments-tutorial/ic795048.png "Gelişmiş kimlik doğrulama seçeneklerini yapılandır")

1. Üzerinde **Federal Kimlik** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Identitty Federasyon](./media/netdocuments-tutorial/ic795049.png "Identitty Federasyon")
   
    a. Olarak **federe kimlik sunucu türü**seçin **Active Directory Federasyon Hizmetleri**.
   
    b. Tıklayın **dosya**, Azure portalından indirdiğiniz indirilen meta veri dosyasını karşıya yükleyin.
   
    c. **Tamam** düğmesine tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/netdocuments-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/netdocuments-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/netdocuments-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/netdocuments-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-netdocuments-test-user"></a>NetDocuments test kullanıcısı oluşturma

NetDocuments için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların NetDocuments sağlanması gerekir.  
NetDocuments söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açma, **NetDocuments** şirketinizin sitesi yöneticisi olarak.

1. Üstteki menüden **yönetici**.
   
    ![Yönetici](./media/netdocuments-tutorial/ic795051.png "yönetici")

1. Tıklayın **ekleme ve kaldırma kullanıcılar ve gruplar**.
   
    ![Depo](./media/netdocuments-tutorial/ic795047.png "depo")

1. İçinde **e-posta adresi** metin kutusuna geçerli bir Azure Active Directory hesabı sağlamak istediğiniz ve ardından e-posta adresini yazın **Kullanıcı Ekle**.
   
    ![E-posta adresi](./media/netdocuments-tutorial/ic795053.png "e-posta adresi")
   
   >[!NOTE]
   >Azure Active Directory hesap sahibi bu etkinleştirilmeden önce hesabı onaylamak için bir bağlantı içeren bir e-posta alırsınız. Herhangi diğer NetDocuments kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri tarafından NetDocuments sağlamak için Azure Active Directory kullanıcı hesaplarını sağlanan.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için NetDocuments erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon NetDocuments için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **NetDocuments**.

    ![Çoklu oturum açmayı yapılandırın](./media/netdocuments-tutorial/tutorial_netdocuments_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde NetDocuments kutucuğa tıkladığınızda, otomatik olarak NetDocuments uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/netdocuments-tutorial/tutorial_general_01.png
[2]: ./media/netdocuments-tutorial/tutorial_general_02.png
[3]: ./media/netdocuments-tutorial/tutorial_general_03.png
[4]: ./media/netdocuments-tutorial/tutorial_general_04.png

[100]: ./media/netdocuments-tutorial/tutorial_general_100.png

[200]: ./media/netdocuments-tutorial/tutorial_general_200.png
[201]: ./media/netdocuments-tutorial/tutorial_general_201.png
[202]: ./media/netdocuments-tutorial/tutorial_general_202.png
[203]: ./media/netdocuments-tutorial/tutorial_general_203.png

