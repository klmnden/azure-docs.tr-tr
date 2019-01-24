---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile TimeOffManager | Microsoft Docs'
description: Azure Active Directory ve TimeOffManager arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 3685912f-d5aa-4730-ab58-35a088fc1cc3
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 4ca13640f64c88164aa98c5434c252d844cebd72
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54825196"
---
# <a name="tutorial-azure-active-directory-integration-with-timeoffmanager"></a>Öğretici: TimeOffManager ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile TimeOffManager tümleştirme konusunda bilgi edinin.

Azure AD ile TimeOffManager tümleştirme ile aşağıdaki avantajları sağlar:

- TimeOffManager erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için TimeOffManager (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile TimeOffManager yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik TimeOffManager çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden TimeOffManager Ekle
1. Yapılandırma ve Azure AD çoklu oturum açmayı test etme

## <a name="add-timeoffmanager-from-the-gallery"></a>Galeriden TimeOffManager Ekle
Azure AD'de TimeOffManager tümleştirmesini yapılandırmak için TimeOffManager Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden TimeOffManager eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **TimeOffManager**seçin **TimeOffManager** sonuç paneli ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Galeriden Ekle](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı TimeOffManager sınayın.

Tek iş için oturum açma için Azure AD ne TimeOffManager karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının TimeOffManager ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

TimeOffManager içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma TimeOffManager ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[TimeOffManager test kullanıcısı oluşturma](#create-a-timeoffmanager-test-user)**  - kullanıcı Azure AD gösterimini bağlı TimeOffManager Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve TimeOffManager uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile TimeOffManager yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **TimeOffManager** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Oturum açma SAML tabanlı](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_samlbase.png)

1. Üzerinde **TimeOffManager etki alanı ve URL'ler** bölümünde, aşağıdaki işlemi gerçekleştirin:

     ![TimeOffManager etki alanı ve URL'ler bölümü](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_url.png)

    İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://www.timeoffmanager.com/cpanel/sso/consume.aspx?company_id=<companyid>`

    > [!NOTE] 
    > Bu değer, gerçek değil. Bu değer, gerçek yanıt URL'si ile güncelleştirin. Bu değeri alabilirsiniz **çoklu oturum açma ayarları sayfasına** daha sonra öğreticide veya kişi açıklanan [TimeOffManager Destek ekibine](https://www.purelyhr.com/contact-us).
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![SAML imzalama sertifikası bölümü](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_certificate.png) 

1. Bu bölümün amacı, kullanıcıların Azure AD'de SAML protokolü temel alarak Federasyon kullanarak TimeOffManger için kendi hesabıyla kimlik doğrulaması sağlamak nasıl anahat sağlamaktır.
    
    TimeOffManger uygulamanız SAML onaylamalarını özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde bekliyor. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir.

    ![SAML belirteci öznitelikleri](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_attrb.png "saml belirteci öznitelikleri")
    
    | Öznitelik Adı | Öznitelik Değeri |
    | --- | --- |
    | firstName |User.givenName |
    | Soyadı |User.surname |
    | Email |User.Mail |
    
    a.  Yukarıdaki tablodaki her veri satırı için tıklatın **kullanıcı özniteliğini eklemek**.
    
    ![SAML belirteci öznitelikleri](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb.png "saml belirteci öznitelikleri")
    
    ![SAML belirteci öznitelikleri](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb1.png "saml belirteci öznitelikleri")
    
    b.  İçinde **öznitelik adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.
    
    c.  İçinde **öznitelik değeri** metin kutusuna, bu satır için gösterilen öznitelik değerini seçin.
    
    d.  **Tamam**’a tıklayın.
    
1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/timeoffmanager-tutorial/tutorial_general_400.png)

1. Üzerinde **TimeOffManager yapılandırma** bölümünde **yapılandırma TimeOffManager** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![TimeOffManager yapılandırma bölümü](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_configure.png) 

1. Farklı bir web tarayıcı penceresinde TimeOffManager şirket sitenize yönetici olarak oturum.

1. Git **hesabı \> hesap seçeneklerini \> çoklu oturum açma ayarları**.
   
   ![Çoklu oturum açma ayarları](./media/timeoffmanager-tutorial/ic795917.png "çoklu oturum açma ayarları")
1. İçinde **çoklu oturum açma ayarları** bölümünde, aşağıdaki adımları gerçekleştirin:
   
   ![Çoklu oturum açma ayarları](./media/timeoffmanager-tutorial/ic795918.png "çoklu oturum açma ayarları")
   
   a. Tüm sertifika içine yapıştırın, base-64 kodlanmış sertifika Not Defteri'nde açın ve içeriğini, panoya kopyalanmak **X.509 sertifikası** metin.
   
   b. İçinde **IDP veren** metin değerini yapıştırın **SAML varlık kimliği** , Azure Portalı'ndan kopyaladığınız.
   
   c. İçinde **IDP uç nokta URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız.
   
   d. Olarak **zorunlu SAML**seçin **Hayır**.
   
   e. Olarak **kullanıcılar otomatik olarak oluşturma**seçin **Evet**.
   
   f. İçinde **oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.
   
   g. Tıklayın **değişiklikleri kaydetmek**.

1. İçinde **çoklu oturum açma ayarları** sayfasında, değerini kopyalayın **onay belgesi tüketici hizmeti URL'si** yapıştırın **yanıt URL'si** metin kutusu altında **TimeOffManager Etki alanı ve URL'ler** bölümü Azure Portalı'nda. 

      ![Çoklu oturum açma ayarları](./media/timeoffmanager-tutorial/ic795915.png "çoklu oturum açma ayarları")

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/timeoffmanager-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Kullanıcıların ve grupların tüm kullanıcılar-->](./media/timeoffmanager-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Ekle düğmesi](./media/timeoffmanager-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Kullanıcı iletişim kutusu sayfası](./media/timeoffmanager-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-timeoffmanager-test-user"></a>TimeOffManager test kullanıcısı oluşturma

Azure AD kullanıcılarının TimeOffManager oturum etkinleştirmek için bunlar için TimeOffManager sağlanması gerekir.  

Yalnızca zaman sağlama kullanıcı TimeOffManager destekler. Sizin için hiçbir eylem öğesini yoktur.  

Kullanıcılar, çoklu oturum açma kullanarak ilk oturum açma sırasında otomatik olarak eklenir.

>[!NOTE]
>Herhangi diğer TimeOffManager kullanıcı hesabı oluşturma araçları kullanabilir veya API Azure AD'ye kullanıcı hesapları sağlamak için TimeOffManager tarafından sağlanan.
> 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için TimeOffManager erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon TimeOffManager için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **TimeOffManager**.

    ![Uygulama listesinde TimeOffManager](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde TimeOffManager kutucuğa tıkladığınızda, otomatik olarak TimeOffManager uygulamanıza açan. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/timeoffmanager-tutorial/tutorial_general_01.png
[2]: ./media/timeoffmanager-tutorial/tutorial_general_02.png
[3]: ./media/timeoffmanager-tutorial/tutorial_general_03.png
[4]: ./media/timeoffmanager-tutorial/tutorial_general_04.png

[100]: ./media/timeoffmanager-tutorial/tutorial_general_100.png

[200]: ./media/timeoffmanager-tutorial/tutorial_general_200.png
[201]: ./media/timeoffmanager-tutorial/tutorial_general_201.png
[202]: ./media/timeoffmanager-tutorial/tutorial_general_202.png
[203]: ./media/timeoffmanager-tutorial/tutorial_general_203.png

