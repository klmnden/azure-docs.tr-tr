---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Druva | Microsoft Docs'
description: Azure Active Directory ve Druva arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: ab92b600-1fea-4905-b1c7-ef8e4d8c495c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2017
ms.author: jeedes
ms.openlocfilehash: 24f0a74f5abb625ba7350c2bd3a500fc2f296575
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54824703"
---
# <a name="tutorial-azure-active-directory-integration-with-druva"></a>Öğretici: Druva ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Druva tümleştirme konusunda bilgi edinin.

Azure AD ile Druva tümleştirme ile aşağıdaki avantajları sağlar:

- Druva erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için Druva (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Druva yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Druva çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Druva ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-druva-from-the-gallery"></a>Galeriden Druva ekleme
Azure AD'de Druva tümleştirmesini yapılandırmak için Druva Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Druva eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Druva**seçin **Druva** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Druva](./media/druva-tutorial/tutorial_druva_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Druva "Britta Simon" adlı bir test kullanıcı tabanlı Azure AD çoklu oturum açmayı sınayın.

Tek iş için oturum açma için Azure AD ne Druva karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve Druva ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Druva içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Druva ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Druva test kullanıcısı oluşturma](#create-a-druva-test-user)**  - kullanıcı Azure AD gösterimini bağlı Druva Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Druva uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Druva yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Druva** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/druva-tutorial/tutorial_druva_samlbase.png)

1. Üzerinde **Druva etki alanı ve URL'ler** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu:

    ![Çoklu oturum açmayı yapılandırın](./media/druva-tutorial/tutorial_druva_url.png)

    İçinde **tanımlayıcı** metin dizesi değeri yazın: `druva-cloud`
    
1. Denetleme **Gelişmiş URL ayarlarını göster**. Uygulamada yapılandırmak istiyorsanız **SP** başlatılan modu:

    ![Çoklu oturum açmayı yapılandırın](./media/druva-tutorial/tutorial_druva_url1.png)
    
    İçinde **oturum açma URL'si** metin kutusuna URL'yi yazın: `https://cloud.druva.com/home`

1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/druva-tutorial/tutorial_druva_certificate.png) 

1. Özel öznitelik eşlemeleri eklemek gerektiren belirli bir biçimde SAML onaylamalarını Druva uygulamanızı bekliyor, **SAML belirteci öznitelikleri** yapılandırma. 

    ![Çoklu oturum açmayı yapılandırın](./media/druva-tutorial/tutorial_druva_attribute.png)

1. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, önceki görüntüde gösterildiği gibi SAML belirteci özniteliği yapılandırın ve aşağıdaki adımları gerçekleştirin:

    | Öznitelik Adı      | Öznitelik Değeri      |
    | ------------------- | -------------------- |
    | ınsync\_auth\_belirteci |Oluşturulan belirteç değeri girin |
    
    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.
    
    ![Çoklu oturum açmayı yapılandırın](./media/druva-tutorial/tutorial_attribute_04.png)
    
    ![Çoklu oturum açmayı yapılandırın](./media/druva-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın. Oluşturulan belirteç değeri, bu öğreticinin sonraki bölümlerinde açıklanmıştır.
    
    d. **Tamam**’a tıklayın.    

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/druva-tutorial/tutorial_general_400.png)

1. Üzerinde **Druva yapılandırma** bölümünde **yapılandırma Druva** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/druva-tutorial/tutorial_druva_configure.png) 

1. Farklı bir web tarayıcı penceresinde Druva şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Git **yönetme \> ayarları**.

    ![Ayarları](./media/druva-tutorial/ic795091.png "ayarları")

1. Çoklu oturum açma ayarları iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma ayarları](./media/druva-tutorial/ic795092.png "çoklu oturum açma ayarları")
    
    a. İçinde **kimlik sağlayıcısı oturum açma URL'si** metin değerini yapıştırın **çoklu oturum açma hizmeti URL'si**, hangi Azure Portalı'ndan kopyaladığınız.
        
    b. İçinde **kimlik sağlayıcısı oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si**, hangi Azure Portalı'ndan kopyaladığınız
        
    c. Base-64 kodlanmış sertifikanızı Not Defteri'nde açın, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **kimlik sağlayıcısı sertifikası** metin kutusu
     
    d. Açmak için **ayarları** sayfasında **Kaydet**.

1. Üzerinde **ayarları** sayfasında **SSO belirteci üretmek**.

    ![Ayarları](./media/druva-tutorial/ic795093.png "ayarları")

1. Üzerinde **tek oturum açma kimlik doğrulaması belirteci** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![SSO belirteci](./media/druva-tutorial/ic795094.png "SSO belirteci")
    
    a. Tıklayın **kopyalama**, Yapıştır kopyalanan değer **değer** metin kutusunda **öznitelik Ekle** bölümünde Azure portalında.
    
    b. **Kapat**’a tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/druva-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/druva-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/druva-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/druva-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-druva-test-user"></a>Druva test kullanıcısı oluşturma

Azure AD kullanıcıları için Druva oturum açmayı etkinleştirmek için bunların Druva sağlanması gerekir. Druva söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **Druva** şirketinizin sitesi yöneticisi olarak.

1. Git **yönetme \> kullanıcılar**.
   
   ![Kullanıcıları Yönet](./media/druva-tutorial/ic795097.png "kullanıcıları yönetme")

1. Tıklayın **Yeni Oluştur**.
   
   ![Kullanıcıları Yönet](./media/druva-tutorial/ic795098.png "kullanıcıları yönetme")

1. Yeni kullanıcı oluşturma iletişim kutusunda aşağıdaki adımları gerçekleştirin:
   
   ![NewUser oluşturma](./media/druva-tutorial/ic795099.png "NewUser oluşturma")
   
   a. İçinde **e-posta adresi** metin gibi kullanıcının e-posta girin **brittasimon@contoso.com**.
   
   b. İçinde **adı** metin gibi kullanıcı adını girin **BrittaSimon**.
   
   c. Tıklayın **oluşturacağı**.

>[!NOTE]
>Herhangi diğer Druva kullanıcı hesabı oluşturma araçları kullanabilir veya API Azure AD'ye kullanıcı hesapları sağlamak için Druva tarafından sağlanan.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Druva erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon için Druva atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Druva**.

    ![Uygulamalar listesinde Druva bağlantı](./media/druva-tutorial/tutorial_druva_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Druva kutucuğa tıkladığınızda, otomatik olarak Druva uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/druva-tutorial/tutorial_general_01.png
[2]: ./media/druva-tutorial/tutorial_general_02.png
[3]: ./media/druva-tutorial/tutorial_general_03.png
[4]: ./media/druva-tutorial/tutorial_general_04.png

[100]: ./media/druva-tutorial/tutorial_general_100.png

[200]: ./media/druva-tutorial/tutorial_general_200.png
[201]: ./media/druva-tutorial/tutorial_general_201.png
[202]: ./media/druva-tutorial/tutorial_general_202.png
[203]: ./media/druva-tutorial/tutorial_general_203.png

