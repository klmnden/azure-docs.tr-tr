---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle CompetencyIQ | Microsoft Docs'
description: Azure Active Directory ve CompetencyIQ arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: e262bf7e-cc7d-4d0e-aea7-861f00d8837d
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: 8cb474c56e1802ccfe828f0040ae231f0748551e
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39437014"
---
# <a name="tutorial-azure-active-directory-integration-with-competencyiq"></a>Öğretici: Azure Active Directory CompetencyIQ ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile CompetencyIQ tümleştirme konusunda bilgi edinin.

Azure AD ile CompetencyIQ tümleştirme ile aşağıdaki avantajları sağlar:

- CompetencyIQ erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için CompetencyIQ (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile CompetencyIQ yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik CompetencyIQ çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden CompetencyIQ ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-competencyiq-from-the-gallery"></a>Galeriden CompetencyIQ ekleme
Azure AD'de CompetencyIQ tümleştirmesini yapılandırmak için CompetencyIQ Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden CompetencyIQ eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **CompetencyIQ**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/competencyiq-tutorial/tutorial_competencyiq_search.png)

1. Sonuçlar panelinde seçin **CompetencyIQ**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/competencyiq-tutorial/tutorial_competencyiq_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı CompetencyIQ ile test etme

Tek iş için oturum açma için Azure AD ne CompetencyIQ karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının CompetencyIQ ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

CompetencyIQ içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma CompetencyIQ ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[CompetencyIQ test kullanıcısı oluşturma](#creating-a-competencyiq-test-user)**  - kullanıcı Azure AD gösterimini bağlı CompetencyIQ Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve CompetencyIQ uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile CompetencyIQ yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **CompetencyIQ** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/competencyiq-tutorial/tutorial_competencyiq_samlbase.png)

1. Üzerinde **CompetencyIQ etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/competencyiq-tutorial/tutorial_competencyiq_url1.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<customer>.competencyiq.com/`
    
    b. İçinde **tanımlayıcı** metin kutusuna URL'yi yazın: `https://www.competencyiq.com/`

    > [!NOTE] 
    > Oturum açma URL değeri olduğu değil gerçek bir nedenle güncelleştirme bu ile gerçek oturum açma URL'si. İlgili kişi [CompetencyIQ istemci Destek ekibine](https://www.competencyiq.com/) bu alınacağı. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/competencyiq-tutorial/tutorial_competencyiq_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/competencyiq-tutorial/tutorial_general_400.png)

1. Üzerinde **CompetencyIQ yapılandırma** bölümünde **yapılandırma CompetencyIQ** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği**, ve **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/competencyiq-tutorial/tutorial_competencyiq_configure.png) 

1. Çoklu oturum açmayı yapılandırma **CompetencyIQ** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML**, **SAML varlık kimliği** ve **SAML çoklu oturum açma hizmeti URL** için [CompetencyIQ Destek ekibine](https://www.competencyiq.com/). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/competencyiq-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/competencyiq-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/competencyiq-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/competencyiq-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-competencyiq-test-user"></a>CompetencyIQ test kullanıcısı oluşturma

CompetencyIQ için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların CompetencyIQ sağlanması gerekir. İlgili kişi [CompetencyIQ Destek ekibine](https://www.competencyiq.com/) uygulamada kullanıcıları oluşturmak için.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için CompetencyIQ erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon CompetencyIQ için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **CompetencyIQ**.

    ![Çoklu oturum açmayı yapılandırın](./media/competencyiq-tutorial/tutorial_competencyiq_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli CompetencyIQ kutucuğa tıkladığınızda, otomatik olarak uygulamaya günlüğe.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/competencyiq-tutorial/tutorial_general_01.png
[2]: ./media/competencyiq-tutorial/tutorial_general_02.png
[3]: ./media/competencyiq-tutorial/tutorial_general_03.png
[4]: ./media/competencyiq-tutorial/tutorial_general_04.png

[100]: ./media/competencyiq-tutorial/tutorial_general_100.png

[200]: ./media/competencyiq-tutorial/tutorial_general_200.png
[201]: ./media/competencyiq-tutorial/tutorial_general_201.png
[202]: ./media/competencyiq-tutorial/tutorial_general_202.png
[203]: ./media/competencyiq-tutorial/tutorial_general_203.png

