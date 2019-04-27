---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile SpaceIQ | Microsoft Docs'
description: Azure Active Directory ve SpaceIQ arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 5b55ae29-491f-401f-9299-d3a6b64a1b99
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6695fe4854c6d91d3d2ba671104d1b481721356c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60517832"
---
# <a name="tutorial-azure-active-directory-integration-with-spaceiq"></a>Öğretici: SpaceIQ ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile SpaceIQ tümleştirme konusunda bilgi edinin.

Azure AD ile SpaceIQ tümleştirme ile aşağıdaki avantajları sağlar:

- SpaceIQ erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için SpaceIQ (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile SpaceIQ yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik SpaceIQ çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden SpaceIQ ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-spaceiq-from-the-gallery"></a>Galeriden SpaceIQ ekleme
Azure AD'de SpaceIQ tümleştirmesini yapılandırmak için SpaceIQ Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden SpaceIQ eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **SpaceIQ**seçin **SpaceIQ** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde SpaceIQ](./media/spaceiq-tutorial/tutorial_spaceiq_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı SpaceIQ ile test etme

Tek iş için oturum açma için Azure AD ne SpaceIQ karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının SpaceIQ ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

SpaceIQ içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma SpaceIQ ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[SpaceIQ test kullanıcısı oluşturma](#create-a-spaceiq-test-user)**  - kullanıcı Azure AD gösterimini bağlı SpaceIQ Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve SpaceIQ uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile SpaceIQ yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **SpaceIQ** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/spaceiq-tutorial/tutorial_spaceiq_samlbase.png)

1. Üzerinde **SpaceIQ etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SpaceIQ etki alanı ve URL'ler tek oturum açma bilgileri](./media/spaceiq-tutorial/tutorial_spaceiq_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna URL'yi yazın: `https://api.spaceiq.com`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://api.spaceiq.com/saml/<instanceid>/callback`

    > [!NOTE] 
    > Fiili yanıt URL'si ve öğreticinin sonraki bölümlerinde açıklanan tanımlayıcısı ile bu değerleri güncelleştirin.
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **(sertifika Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/spaceiq-tutorial/tutorial_spaceiq_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/spaceiq-tutorial/tutorial_general_400.png)

1. Üzerinde **SpaceIQ yapılandırma** bölümünde **yapılandırma SpaceIQ** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği** gelen **hızlı başvuru bölümü.**

    ![SpaceIQ yapılandırma](./media/spaceiq-tutorial/tutorial_spaceiq_configure.png) 

1.  Yeni bir tarayıcı penceresi açın ve ardından SpaceIQ ortamınıza bir yönetici olarak oturum açın.

1. Oturum açtıktan sonra sağ üst köşedeki Bulmacanın oturum tıklayın ve ardından tıklayarak **"Tümleştirmeler"**

    ![Hesap ayarları](./media/spaceiq-tutorial/setting1.png) 

1. Altında **tüm sağlama ve SSO**, tıklayarak **Azure** Azure örneğini IDP'si olarak eklemek istediğiniz kutucuk.

    ![SAML simgesi](./media/spaceiq-tutorial/setting2.png)

1. İçinde **SSO** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![SAML kimlik doğrulama ayarları](./media/spaceiq-tutorial/setting3.png)

    a. İçinde **SAML veren URL'si** kutusu, yapıştırma **SAML varlık kimliği** değer Azure AD uygulama yapılandırması penceresinde kopyalanır.
    
    b. Kopyalama **SAML geri çağırma uç nokta URL'si (salt okunur)** değeri ve değer yapıştırın **yanıt URL'si** Azure portalında, SpaceIQ kutusunda **etki alanı ve URL'ler** bölümü.
    
    c. Kopyalama **SAML hedef kitle URİ'si (salt okunur)** değeri ve değer yapıştırın **tanımlayıcı** Azure portalında, SpaceIQ kutusunda **etki alanı ve URL'ler** bölümü.

    d. İndirilen sertifika dosyasını Not Defteri'nde açın, içeriği kopyalayın ve ardından yapıştırın **X.509 sertifikası** kutusu.
    
    e. **Kaydet**’e tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/spaceiq-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/spaceiq-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/spaceiq-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/spaceiq-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-a-spaceiq-test-user"></a>SpaceIQ test kullanıcısı oluşturma

Bu bölümde, Britta Simon SpaceIQ içinde adlı bir kullanıcı oluşturun. İş [SpaceIQ Destek ekibine](mailto:eng@spaceiq.com) SpaceIQ platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için SpaceIQ erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon SpaceIQ için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **SpaceIQ**.

    ![Uygulamalar listesinde SpaceIQ bağlantı](./media/spaceiq-tutorial/tutorial_spaceiq_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde SpaceIQ kutucuğa tıkladığınızda, otomatik olarak SpaceIQ uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/spaceiq-tutorial/tutorial_general_01.png
[2]: ./media/spaceiq-tutorial/tutorial_general_02.png
[3]: ./media/spaceiq-tutorial/tutorial_general_03.png
[4]: ./media/spaceiq-tutorial/tutorial_general_04.png

[100]: ./media/spaceiq-tutorial/tutorial_general_100.png

[200]: ./media/spaceiq-tutorial/tutorial_general_200.png
[201]: ./media/spaceiq-tutorial/tutorial_general_201.png
[202]: ./media/spaceiq-tutorial/tutorial_general_202.png
[203]: ./media/spaceiq-tutorial/tutorial_general_203.png

