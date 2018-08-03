---
title: 'Öğretici: Azure Active Directory Tableau Server ile tümleştirme | Microsoft Docs'
description: Tableau Server ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: c1917375-08aa-445c-a444-e22e23fa19e0
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 78f58f28eb9c25e0b5f6869f7e2348b41780fb60
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39437075"
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>Öğretici: Azure Active Directory Tableau Server ile tümleştirme

Bu öğreticide, Tableau Server, Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.

Tableau Server, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Tableau Server erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) için Tableau Server açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Tableau Server ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Tableau Server Çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Tableau Server ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-tableau-server-from-the-gallery"></a>Galeriden Tableau Server ekleme
Azure AD'de Tableau Server tümleştirmesini yapılandırmak için Tableau Server Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Tableau Server eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Tableau Server**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/tableauserver-tutorial/tutorial_tableauserver_search.png)

1. Sonuçlar panelinde seçin **Tableau Server**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/tableauserver-tutorial/tutorial_tableauserver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı Tableau Server ile test etme

Tek iş için oturum açma için Azure AD ne Tableau Server karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve Tableau Server ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Tableau Server, değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Tableau Server ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Tableau Server test kullanıcısı oluşturma](#creating-a-tableau-server-test-user)**  - kullanıcı Azure AD gösterimini bağlı Tableau Server Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Tableau Server uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Tableau Server ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Tableau Server** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/tableauserver-tutorial/tutorial_tableauserver_samlbase.png)

1. Üzerinde **Tableau Server etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/tableauserver-tutorial/tutorial_tableauserver_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://azure.<domain name>.link`
    
    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://azure.<domain name>.link`

    c. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://azure.<domain name>.link/wg/saml/SSO/index.html`
     
    > [!NOTE] 
    > Yukarıdaki değerlerden gerçek değerlerin değil. Daha sonra gerçek URL ve tanımlayıcıdır Tableau Server yapılandırma sayfasından ile değerlerini güncelleştirin. 

1. Tableau Server uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **"Kullanıcı öznitelikleri"** uygulama tümleştirme sayfasında bölümü. Aşağıdaki ekran görüntüsünde bir örnek için aynı gösterir.
    
    ![Çoklu oturum açmayı yapılandırın](./media/tableauserver-tutorial/3.png)
    
1. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | ---------------| --------------- |    
    | kullanıcı adı | *user.mailnickname* |

    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/tableauserver-tutorial/tutorial_officespace_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/tableauserver-tutorial/tutorial_officespace_05.png)
    
    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. Tıklayın **Tamam**


1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/tableauserver-tutorial/tutorial_tableauserver_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/tableauserver-tutorial/tutorial_general_400.png)
<CS>
1. Uygulamanız için yapılandırılmış SSO elde etmek için Tableau Server Kiracı yönetici olarak oturum açma gerekir.
   
   a. Tableau Server yapılandırmasında tıklayın **SAML** sekmesi.
  
    ![Çoklu oturum açmayı yapılandırın](./media/tableauserver-tutorial/tutorial_tableauserver_001.png) 
  
   b. Onay kutusunu işaretleyin, **kullanım SAML çoklu oturum açma için**.
   
   c. Tableau Server dönüş URL'si — Tableau Server kullanıcıları, gibi erişecek URL http://tableau_server. Kullanarak http://localhost önerilmez. Bir URL kullanarak bir eğik çizgiyle (örneğin, http://tableau_server/) desteklenmiyor. Kopyalama **Tableau Server dönüş URL'si** ve Azure AD'ye yapıştırın **işareti bulunan URL'si** metin kutusunda **Tableau Server etki alanı ve URL'ler** bölümü.
   
   d. SAML varlık kimliği — varlık kimliği için IDP Tableau Server yüklemenizin benzersiz olarak tanımlar. Tableau Server URL'nizi yeniden burada isterseniz girebilirsiniz, ancak Tableau Server URL'nizi olması gerekmez. Kopyalama **SAML varlık kimliği** ve Azure AD'ye yapıştırın **tanımlayıcı** metin kutusunda **Tableau Server etki alanı ve URL'ler** bölümü.
     
   e. Tıklayın **meta veri dosyası dışarı** ve metin düzenleyicisi uygulamayı açın. Onay belgesi tüketici hizmeti URL'si Http Post ile bulup 0 dizini ve URL'yi kopyalayın. Artık Azure AD'ye yapıştırın **yanıt URL'si** metin kutusunda **Tableau Server etki alanı ve URL'ler** bölümü.
   
   f. Azure portalından indirdiğiniz, Federasyon meta verileri dosyasını bulun ve bunu karşıya **IDP SAML meta veri dosyası**.
   
   g. Tıklayın **Tamam** Tableau Server yapılandırma sayfasında düğme.
   
    >[!NOTE] 
    >Müşteriniz varsa Tableau Server SAML SSO yapılandırma herhangi bir sertifikayı karşıya yüklemek ve SSO akışta dikkate.
    >Tableau Server üzerinde SAML yapılandırmasına yardımcı olması sonra lütfen bu makaleyi okuyun [SAML yapılandırma](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm).
    >
<CE>

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/tableauserver-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/tableauserver-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/tableauserver-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/tableauserver-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-tableau-server-test-user"></a>Tableau Server test kullanıcısı oluşturma

Bu bölümün amacı, Tableau Server Britta Simon adlı bir kullanıcı oluşturmaktır. Tableau server içindeki tüm kullanıcılar sağlamanız gerekir. 

Bu kullanıcının kullanıcı adı Azure AD'ye özel öznitelik içinde yapılandırılmış değer ile eşleşmelidir **username**. Doğru eşleme içeren tümleştirme çalışmalıdır [yapılandırma Azure AD çoklu oturum açma](#configuring-azure-ad-single-sign-on).

>[!NOTE]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kuruluşunuzdaki Tableau Server yöneticinize başvurun gerekir.
> 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Tableau Server için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Tableau Server Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Tableau Server**.

    ![Çoklu oturum açmayı yapılandırın](./media/tableauserver-tutorial/tutorial_tableauserver_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Tableau Server kutucuğa tıkladığınızda, size otomatik olarak Tableau Server uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/tableauserver-tutorial/tutorial_general_01.png
[2]: ./media/tableauserver-tutorial/tutorial_general_02.png
[3]: ./media/tableauserver-tutorial/tutorial_general_03.png
[4]: ./media/tableauserver-tutorial/tutorial_general_04.png

[100]: ./media/tableauserver-tutorial/tutorial_general_100.png

[200]: ./media/tableauserver-tutorial/tutorial_general_200.png
[201]: ./media/tableauserver-tutorial/tutorial_general_201.png
[202]: ./media/tableauserver-tutorial/tutorial_general_202.png
[203]: ./media/tableauserver-tutorial/tutorial_general_203.png

