---
title: 'Öğretici: Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 88dc82ab-be0b-4017-8335-c47d00775d7b
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: jeedes
ms.openlocfilehash: 7b31cbbf639e5835a3a46063b4b15d67915fb3d4
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54816679"
---
# <a name="tutorial-azure-active-directory-integration-with-five9-plus-adapter-cti-contact-center-agents"></a>Öğretici: Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile nasıl tümleştireceğinizi Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) öğrenin.

Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan Five9 artı bağdaştırıcısına (CTI, ilgili Center aracıları) açma, kullanıcılarınızın etkinleştirebilirsiniz (çoklu oturum açma) ile Azure AD hesaplarına
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Five9 artı bağdaştırıcısıyla (CTI, ilgili Center aracıları) yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme burada alabilirsiniz: [Deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-five9-plus-adapter-cti-contact-center-agents-from-the-gallery"></a>Galeriden Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) ekleme
Azure AD'ye Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) tümleştirmesini yapılandırmak için Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) eklemek galerideki yönetilen SaaS uygulamaları listenize gerekir.

**Galeriden Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları)**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/five9-tutorial/tutorial_five9_search.png)

1. Sonuçlar panelinde seçin **Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları)** ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/five9-tutorial/tutorial_five9_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma bağdaştırıcısıyla Five9 yanı sıra "Britta Simon" adlı bir test kullanıcıya dayanarak (CTI, ilgili Center aracıları) test edin.

Tek çalışmak için oturum açma için Azure AD ne Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı Five9 artı bağdaştırıcısında (CTI, ilgili Center aracıları) arasında bir bağlantı ilişki kurulması gerekir.

Five9 artı bağdaştırıcısında (CTI, ilgili Center aracıları) değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) test kullanıcısı oluşturma](#creating-a-five9-plus-adapter-cti-contact-center-agents-test-user)**  - bir karşılığı Britta simon'un Five9 yanı sıra kullanıcı Azure AD gösterimini bağlı bağdaştırıcı (CTI, ilgili Center aracıları) sağlamak için.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) uygulamanızı yapılandırın.

**Azure AD çoklu oturum açma Five9 artı bağdaştırıcısıyla (CTI, ilgili Center aracıları) yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları)** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/five9-tutorial/tutorial_five9_samlbase.png)

1. Üzerinde **Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/five9-tutorial/tutorial_five9_url.png)
    
    a. İçinde **tanımlayıcı** metin kutusuna bir URL kullanarak aşağıdaki düzenler:

    |    Ortam      |       URL'si      |
    | :-- | :-- |
    | "Five9 Plus için Microsoft Dynamics CRM bağdaştırıcısı" | `https://app.five9.com/appsvcs/saml/metadata/alias/msdc` |
    | "Five9 Plus bağdaştırıcısı Zendesk için" | `https://app.five9.com/appsvcs/saml/metadata/alias/zd` |
    | "Five9 Plus bağdaştırıcısı aracı Masaüstü araç seti için" | `https://app.five9.com/appsvcs/saml/metadata/alias/adt` |

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak:

    |      Ortam     |      URL'si      |
    | :--                  | :--           |
    | "Five9 Plus için Microsoft Dynamics CRM bağdaştırıcısı" | `https://app.five9.com/appsvcs/saml/SSO/alias/msdc` |
    | "Five9 Plus bağdaştırıcısı Zendesk için" | `https://app.five9.com/appsvcs/saml/SSO/alias/zd` |
    | "Five9 Plus bağdaştırıcısı aracı Masaüstü araç seti için" | `https://app.five9.com/appsvcs/saml/SSO/alias/adt` |

1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/five9-tutorial/tutorial_five9_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/five9-tutorial/tutorial_general_400.png)

1. Üzerinde **Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) yapılandırması** bölümünde **yapılandırma Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları)** açmak için **oturum açmaYapılandırma**penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/five9-tutorial/tutorial_five9_configure.png) 

1. Çoklu oturum açmayı yapılandırma **Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları)** tarafı, indirilen göndermek için ihtiyacınız **Certificate(Base64), oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si**için [Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) destek ekibi](https://www.five9.com/about/contact). Ayrıca, SSO daha fazla yapılandırmak için lütfen izleyin aşağıdaki adımları bağdaştırıcısı göre:

    a. "Five9 artı bağdaştırıcısı aracı Masaüstü araç seti için" Yönetici Kılavuzu: [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf)
    
    b. "Five9 yanı sıra Microsoft Dynamics CRM için bağdaştırıcı" Yönetici Kılavuzu: [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf)
    
    c. "Five9 artı bağdaştırıcısı Zendesk için" Yönetici Kılavuzu: [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf)


> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/five9-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/five9-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/five9-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/five9-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-five9-plus-adapter-cti-contact-center-agents-test-user"></a>Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) test kullanıcısı oluşturma

Bu bölümde, Britta Simon Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) adlı bir kullanıcı oluşturun. Çalışmak [Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) destek ekibi](https://www.five9.com/about/contact) Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Five9 artı bağdaştırıcısına (CTI, ilgili Center aracıları) erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Five9 artı bağdaştırıcısına (CTI, ilgili Center aracıları) atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları)**.

    ![Çoklu oturum açmayı yapılandırın](./media/five9-tutorial/tutorial_five9_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) kutucuğa tıkladığınızda, otomatik olarak Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/five9-tutorial/tutorial_general_01.png
[2]: ./media/five9-tutorial/tutorial_general_02.png
[3]: ./media/five9-tutorial/tutorial_general_03.png
[4]: ./media/five9-tutorial/tutorial_general_04.png

[100]: ./media/five9-tutorial/tutorial_general_100.png

[200]: ./media/five9-tutorial/tutorial_general_200.png
[201]: ./media/five9-tutorial/tutorial_general_201.png
[202]: ./media/five9-tutorial/tutorial_general_202.png
[203]: ./media/five9-tutorial/tutorial_general_203.png

