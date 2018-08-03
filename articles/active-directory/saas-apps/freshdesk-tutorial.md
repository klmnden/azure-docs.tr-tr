---
title: 'Öğretici: Azure Active Directory FreshDesk ile tümleştirme | Microsoft Docs'
description: Azure Active Directory ve FreshDesk arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: c2a3e5aa-7b5a-4fe4-9285-45dbe6e8efcc
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: d0fbed347805a581fb66e0218290993817277214
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39428341"
---
# <a name="tutorial-azure-active-directory-integration-with-freshdesk"></a>Öğretici: Azure Active Directory FreshDesk ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile FreshDesk tümleştirme konusunda bilgi edinin.

FreshDesk Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- FreshDesk erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan Freshdesk'e (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Bir merkezi konumda - Azure Yönetim Portalı hesaplarınızı yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi FreshDesk ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir FreshDesk çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Bu gerekli olmadığı sürece üretim ortamınızı kullanmamanız gerekir.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden FreshDesk ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-freshdesk-from-the-gallery"></a>Galeriden FreshDesk ekleme
Azure AD'de FreshDesk tümleştirmesini yapılandırmak için FreshDesk Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden FreshDesk eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure Yönetim Portalı](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Tıklayın **Ekle** iletişim kutusunun üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **FreshDesk**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/freshdesk-tutorial/tutorial_freshdesk_search.png)

1. Sonuçlar panelinde seçin **FreshDesk**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/freshdesk-tutorial/tutorial_freshdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı FreshDesk ile test edin.

Tek iş için oturum açma için Azure AD ne FreshDesk karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının FreshDesk ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değerini atayarak bu bağlantı ilişki kurulduktan **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** FreshDesk içinde.

Yapılandırma ve Azure AD çoklu oturum açma FreshDesk ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[FreshDesk test kullanıcısı oluşturma](#creating-a-freshdesk-test-user)**  - Azure AD gösterimini her için bağlı olan FreshDesk Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure yönetim portalında etkinleştirin ve FreshDesk uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma FreshDesk ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda üzerinde **FreshDesk** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda olarak **modu** seçin **SAML tabanlı oturum açma** için çoklu oturum açmayı etkinleştirme.
 
    ![Çoklu oturum açmayı yapılandırın](./media/freshdesk-tutorial/tutorial_freshdesk_samlbase.png)

1. Üzerinde **FreshDesk etki alanı ve URL'ler** bölümünde, lütfen **oturum açma URL'si** olarak: `https://<tenant-name>.freshdesk.com` veya başka bir değer Freshdesk önerdi.

    ![Çoklu oturum açmayı yapılandırın](./media/freshdesk-tutorial/tutorial_freshdesk_url.png)

    > [!NOTE] 
    > Bu gerçek değer olmadığını unutmayın. Değerini gerçek oturum açma URL'si ile güncelleştirmeniz gerekir. İlgili kişi [FreshDesk istemci Destek ekibine](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg) bu değeri alınamıyor.  

1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika** ve sertifika bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/freshdesk-tutorial/tutorial_freshdesk_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/freshdesk-tutorial/tutorial_general_400.png)

1. Üzerinde **FreshDesk yapılandırma** bölümünde **yapılandırma FreshDesk** yapılandırma oturum açma penceresini açın. SAML çoklu oturum açma hizmet URL'si ve oturum kapatma URL'si kopyalama **hızlı başvuru** bölümü.

    ![Çoklu oturum açmayı yapılandırın](./media/freshdesk-tutorial/tutorial_freshdesk_configure.png)

1. Farklı bir web tarayıcı penceresinde Freshdesk şirket sitenize yönetici olarak oturum.

1. Üstteki menüden **yönetici**.
   
   ![Yönetici](./media/freshdesk-tutorial/IC776768.png "yönetici")

1. İçinde **genel ayarlar** sekmesinde **güvenlik**.
   
   ![Güvenlik](./media/freshdesk-tutorial/IC776769.png "güvenlik")

1. İçinde **güvenlik** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum](./media/freshdesk-tutorial/IC776770.png "tekli oturum")
   
    a. İçin **çoklu oturum açma (SSO)** seçin **üzerinde**.

    b. Seçin **SAML SSO**.

    c. Tür **SAML çoklu oturum açma hizmeti URL'si** Azure portaldan kopyaladığınız **SAML oturum açma URL'si** metin.

    d. Tür **oturum kapatma URL'si** Azure portaldan kopyaladığınız **oturum kapatma URL'si** metin.

    e. Kopyalama **parmak izi** yapıştırın ve Azure portalından indirilen sertifikanın değeri **güvenlik sertifikası parmak izi** metin.  
 
    >[!TIP]
    >Daha fazla ayrıntı için [bir sertifikanın parmak izi değerini almak nasıl](http://youtu.be/YKQF266SAxI). 
    
    f. **Kaydet**’e tıklayın.


### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, bir test kullanıcısı Britta Simon adlı Azure Yönetim Portalı'nda oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure Yönetim Portalı**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/freshdesk-tutorial/create_aaduser_01.png) 

1. Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar** kullanıcılar listesini görüntüleyin.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/freshdesk-tutorial/create_aaduser_02.png) 

1. İletişim kutusunun en üstünde tıklayın **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/freshdesk-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/freshdesk-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-freshdesk-test-user"></a>FreshDesk test kullanıcısı oluşturma

FreshDesk açarken Azure AD kullanıcılarının etkinleştirmek için bunların FreshDesk sağlanması gerekir.  
FreshDesk söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesapları sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **Freshdesk** Kiracı.
1. Üstteki menüden **yönetici**.
   
   ![Yönetici](./media/freshdesk-tutorial/IC776772.png "yönetici")

1. İçinde **genel ayarlar** sekmesinde **aracıları**.
   
   ![Aracıları](./media/freshdesk-tutorial/IC776773.png "aracıları")

1. Tıklayın **yeni aracı**.
   
    ![Yeni aracı](./media/freshdesk-tutorial/IC776774.png "yeni aracı")

1. Aracı bilgilerini iletişim kutusunda aşağıdaki adımları gerçekleştirin:
   
   ![Aracı bilgilerini](./media/freshdesk-tutorial/IC776775.png "aracısı bilgileri")
   
   a. İçinde **tam adı** metin sağlamak istediğiniz Azure AD hesabı adını yazın.

   b. İçinde **e-posta** metin sağlamak istediğiniz Azure AD hesabını Azure AD türü e-posta adresi.

   c. İçinde **başlık** metin sağlamak istediğiniz Azure AD hesabı adını yazın.

   d. Seçin **aracıları rol**ve ardından **atama**.
       
   e. **Kaydet**’e tıklayın.     
   
    >[!NOTE]
    >Azure AD hesap sahibinin etkinleştirilmeden önce hesabı onaylamak için bir bağlantı içeren bir e-posta alırsınız. 
    > 
    
    >[!NOTE]
    >Herhangi diğer Freshdesk kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak Freshdesk tarafından sağlanan. Freshdesk'e.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kutusuna erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Freshdesk'e atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda uygulamaları görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **FreshDesk**.

    ![Çoklu oturum açmayı yapılandırın](./media/freshdesk-tutorial/tutorial_freshdesk_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli FreshDesk kutucuğa tıkladığınızda, oturum açma sayfası açan FreshDesk uygulamanız için almanız gerekir.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/freshdesk-tutorial/tutorial_general_01.png
[2]: ./media/freshdesk-tutorial/tutorial_general_02.png
[3]: ./media/freshdesk-tutorial/tutorial_general_03.png
[4]: ./media/freshdesk-tutorial/tutorial_general_04.png

[100]: ./media/freshdesk-tutorial/tutorial_general_100.png

[200]: ./media/freshdesk-tutorial/tutorial_general_200.png
[201]: ./media/freshdesk-tutorial/tutorial_general_201.png
[202]: ./media/freshdesk-tutorial/tutorial_general_202.png
[203]: ./media/freshdesk-tutorial/tutorial_general_203.png

