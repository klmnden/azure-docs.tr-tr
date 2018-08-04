---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle LogicMonitor | Microsoft Docs'
description: Azure Active Directory ve LogicMonitor arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 496156c3-0e22-4492-b36f-2c29c055e087
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2018
ms.author: jeedes
ms.openlocfilehash: a6bc220d15e720662eaa9605421e21ccb99892ab
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39502354"
---
# <a name="tutorial-azure-active-directory-integration-with-logicmonitor"></a>Öğretici: Azure Active Directory LogicMonitor ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile LogicMonitor tümleştirme konusunda bilgi edinin.

Azure AD ile LogicMonitor tümleştirme ile aşağıdaki avantajları sağlar:

- LogicMonitor erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için LogicMonitor (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile LogicMonitor yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik LogicMonitor çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden LogicMonitor ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-logicmonitor-from-the-gallery"></a>Galeriden LogicMonitor ekleme
Azure AD'de LogicMonitor tümleştirmesini yapılandırmak için LogicMonitor Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden LogicMonitor eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **LogicMonitor**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/logicmonitor-tutorial/tutorial_logicmonitor_search.png)

1. Sonuçlar panelinde seçin **LogicMonitor**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/logicmonitor-tutorial/tutorial_logicmonitor_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı LogicMonitor sınayın.

Tek iş için oturum açma için Azure AD ne LogicMonitor karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının LogicMonitor ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

LogicMonitor içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma LogicMonitor ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[LogicMonitor test kullanıcısı oluşturma](#creating-a-logicmonitor-test-user)**  - kullanıcı Azure AD gösterimini bağlı LogicMonitor Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve LogicMonitor uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile LogicMonitor yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **LogicMonitor** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/logicmonitor-tutorial/tutorial_logicmonitor_samlbase.png)

1. Üzerinde **LogicMonitor etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/logicmonitor-tutorial/tutorial_logicmonitor_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.logicmonitor.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.logicmonitor.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [LogicMonitor istemci Destek ekibine](https://www.logicmonitor.com/contact/) bu değerleri almak için. 
 


1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/logicmonitor-tutorial/tutorial_logicmonitor_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/logicmonitor-tutorial/tutorial_general_400.png)

1. Oturum açın, **LogicMonitor** yönetici olarak şirketin site.

1. Üstteki menüden **ayarları**.
   
    ![Ayarları](./media/logicmonitor-tutorial/ic790052.png "ayarları")

1. Sol taraftaki gezinti uygulamalarımızın tıklayın **çoklu oturum açma**
   
    ![Çoklu oturum açma](./media/logicmonitor-tutorial/ic790053.png "çoklu oturum açma")

1. İçinde **çoklu oturum açma (SSO) ayarlarını** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma ayarları](./media/logicmonitor-tutorial/ic790054.png "çoklu oturum açma ayarları")
   
    a. Seçin **çoklu oturum açmayı etkinleştirme**.

    b. Olarak **varsayılan rol ataması**seçin **salt okunur**.
   
    c. İndirilen meta veri dosyasını Not Defteri'nde açın ve ardından içeriği dosyaya yapıştırın **kimlik sağlayıcısı meta verileri** metin.
   
    d. Tıklayın **değişiklikleri kaydetmek**.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/logicmonitor-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/logicmonitor-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/logicmonitor-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/logicmonitor-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-logicmonitor-test-user"></a>LogicMonitor test kullanıcısı oluşturma

Azure AD kullanıcılarının oturum açabilmesi, Azure Active Directory kullanıcı adlarını kullanarak LogicMonitor uygulamaya sağlanmalıdır.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. LogicMonitor şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Üstteki menüden **ayarları**ve ardından **rolleri ve kullanıcıları**.
   
    ![Rolleri ve kullanıcıları](./media/logicmonitor-tutorial/ic790056.png "roller ve kullanıcılar")

1. **Ekle**'ye tıklayın.

1. İçinde **Hesap Ekle** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Hesap Ekle](./media/logicmonitor-tutorial/ic790057.png "Hesap Ekle")
   
    a. Tür **kullanıcı adı**, **e-posta**, **parola**, ve **parolayı yeniden yazın parola** sağlamak istediğiniz Azure Active Directory kullanıcı değerleri ilgili metin kutularına.
   
    b. Seçin **rolleri**, **görüntüleme izinleri**ve **durumu**.
   
    c. Tıklayın **gönderme**.

>[!NOTE]
>Herhangi diğer LogicMonitor kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri tarafından LogicMonitor sağlamak için Azure Active Directory kullanıcı hesaplarını sağlanan. 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için LogicMonitor erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon LogicMonitor için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **LogicMonitor**.

    ![Çoklu oturum açmayı yapılandırın](./media/logicmonitor-tutorial/tutorial_logicmonitor_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.
 
Erişim panelinde LogicMonitor kutucuğa tıkladığınızda, otomatik olarak LogicMonitor uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/logicmonitor-tutorial/tutorial_general_01.png
[2]: ./media/logicmonitor-tutorial/tutorial_general_02.png
[3]: ./media/logicmonitor-tutorial/tutorial_general_03.png
[4]: ./media/logicmonitor-tutorial/tutorial_general_04.png

[100]: ./media/logicmonitor-tutorial/tutorial_general_100.png

[200]: ./media/logicmonitor-tutorial/tutorial_general_200.png
[201]: ./media/logicmonitor-tutorial/tutorial_general_201.png
[202]: ./media/logicmonitor-tutorial/tutorial_general_202.png
[203]: ./media/logicmonitor-tutorial/tutorial_general_203.png

