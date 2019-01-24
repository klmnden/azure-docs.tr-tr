---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile TeamSeer | Microsoft Docs'
description: Azure Active Directory ve TeamSeer arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 6ec4806f-fe0f-4ed7-8cfa-32d1c840433f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: ffe94d8d3e08bb35cf8ea20f4b1e32b1ec84aa35
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54814163"
---
# <a name="tutorial-azure-active-directory-integration-with-teamseer"></a>Öğretici: TeamSeer ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile TeamSeer tümleştirme konusunda bilgi edinin.

Azure AD ile TeamSeer tümleştirme ile aşağıdaki avantajları sağlar:

- TeamSeer erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için TeamSeer (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile TeamSeer yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir TeamSeer çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden TeamSeer ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-teamseer-from-the-gallery"></a>Galeriden TeamSeer ekleme
Azure ad içinde TeamSeer tümleştirmesini yapılandırmak için TeamSeer Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden TeamSeer eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **TeamSeer**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/teamseer-tutorial/tutorial_teamseer_search.png)

1. Sonuçlar panelinde seçin **TeamSeer**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/teamseer-tutorial/tutorial_teamseer_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı TeamSeer ile test etme

Tek iş için oturum açma için Azure AD ne TeamSeer karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının TeamSeer ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

TeamSeer içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma TeamSeer ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[TeamSeer test kullanıcısı oluşturma](#creating-a-teamseer-test-user)**  - kullanıcı Azure AD gösterimini bağlı TeamSeer Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve TeamSeer uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile TeamSeer yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **TeamSeer** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/teamseer-tutorial/tutorial_teamseer_samlbase.png)

1. Üzerinde **TeamSeer etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/teamseer-tutorial/tutorial_teamseer_url.png)

     İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://www.teamseer.com/<companyid>`

    > [!NOTE] 
    > Değer, gerçek değil. Değerini gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [TeamSeer istemci Destek ekibine](https://pages.theaccessgroup.com/solutions_business-suite_absence-management_contact.html) değeri alınamıyor. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/teamseer-tutorial/tutorial_teamseer_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/teamseer-tutorial/tutorial_general_400.png)

1. Üzerinde **TeamSeer yapılandırma** bölümünde **yapılandırma TeamSeer** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/teamseer-tutorial/tutorial_teamseer_configure.png)

1. Farklı bir web tarayıcı penceresinde TeamSeer şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Git **ik Yöneticisi**.
   
    ![İK Yöneticisi](./media/teamseer-tutorial/ic789634.png "ik Yöneticisi")

1. Tıklayın **Kurulum**.
   
    ![Kurulum](./media/teamseer-tutorial/ic789635.png "Kurulumu")

1. Tıklayın **SAML sağlayıcı ayrıntılarını ayarlama**.
   
    ![SAML ayarlarını](./media/teamseer-tutorial/ic789636.png "SAML ayarları")

1. SAML sağlayıcısı Ayrıntılar bölümünde aşağıdaki adımları gerçekleştirin:
   
    ![SAML ayarlarını](./media/teamseer-tutorial/ic789637.png "SAML ayarları")   

    a. Yapıştırma **çoklu oturum açma hizmeti URL'si** için değer **URL** metin.
          
    b. Base-64 kodlanmış sertifikanızı Not Defteri'nde açın, içinde içeriği panonuza kopyalayın ve ardından ona yapıştırın **IDP ortak sertifika** metin.

1. SAML sağlayıcısı yapılandırmasını tamamlamak için aşağıdaki adımları gerçekleştirin:
    
    ![SAML ayarlarını](./media/teamseer-tutorial/ic789638.png "SAML ayarları") 

    a. İçinde **Test e-posta adreslerini**, test kullanıcının e-posta adresini yazın. 
  
    b. İçinde **veren** metin hizmet sağlayıcısı veren URL'sini yazın. 
  
    c. **Kaydet**’e tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/teamseer-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/teamseer-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/teamseer-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/teamseer-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-teamseer-test-user"></a>TeamSeer test kullanıcısı oluşturma

Azure AD kullanıcıları için TeamSeer oturum açmak etkinleştirmek için bunlar içinde ShiftPlanning için sağlanması gerekir. TeamSeer söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **TeamSeer** yönetici olarak şirketin site.

1. Aşağıdaki adımları gerçekleştirin:
   
    ![İK Yöneticisi](./media/teamseer-tutorial/ic789640.png "ik Yöneticisi")  
 
    a. Git **ik Yöneticisi \> kullanıcılar**.
  
    b. Tıklayın **yeni kullanıcı Sihirbazı'nı çalıştırın**.

1. İçinde **kullanıcı ayrıntıları** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcı ayrıntılarını](./media/teamseer-tutorial/ic789641.png "kullanıcı ayrıntıları")

    a. Tür **ad**, **Soyadı**, **kullanıcı adı (e-posta adresi)** için ilgili metin kutularına, sağlamak istediğiniz geçerli bir AAD hesabı.
  
    b. **İleri**’ye tıklayın.

1. İzleyin ekrandaki yönergeleri yeni bir kullanıcı eklemek için tıklayın **son**.

>[!NOTE]
>Herhangi diğer TeamSeer kullanıcı hesabı oluşturma araçları kullanabilir veya API Azure AD'ye kullanıcı hesapları sağlamak için TeamSeer tarafından sağlanan. 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için TeamSeer erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon TeamSeer için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **TeamSeer**.

    ![Çoklu oturum açmayı yapılandırın](./media/teamseer-tutorial/tutorial_teamseer_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Çoklu oturum açma ayarları test etmek isterseniz, erişim Paneli'nde açın. Erişim paneli hakkında daha fazla ayrıntı için bkz. [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/teamseer-tutorial/tutorial_general_01.png
[2]: ./media/teamseer-tutorial/tutorial_general_02.png
[3]: ./media/teamseer-tutorial/tutorial_general_03.png
[4]: ./media/teamseer-tutorial/tutorial_general_04.png

[100]: ./media/teamseer-tutorial/tutorial_general_100.png

[200]: ./media/teamseer-tutorial/tutorial_general_200.png
[201]: ./media/teamseer-tutorial/tutorial_general_201.png
[202]: ./media/teamseer-tutorial/tutorial_general_202.png
[203]: ./media/teamseer-tutorial/tutorial_general_203.png

