---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Cerner Orta | Microsoft Docs'
description: Azure Active Directory ve Cerner Orta arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: d2bc549d-d286-4679-854e-bb67c62b0475
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: c097fb045db1afe65a84a2a96dc202c57e8a449e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60428738"
---
# <a name="tutorial-azure-active-directory-integration-with-cerner-central"></a>Öğretici: Azure Active Directory Tümleştirmesi ile Cerner Orta

Bu öğreticide, Azure Active Directory (Azure AD) ile Cerner Orta tümleştirme konusunda bilgi edinin.

Azure AD ile Cerner Orta tümleştirme ile aşağıdaki avantajları sağlar:

- Cerner merkezi erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) Cerner merkezi siteden açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Cerner Central ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Onaylanan bir Cerner Merkezi sistem hesabı

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Cerner Orta ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-cerner-central-from-the-gallery"></a>Galeriden Cerner Orta ekleme
Azure AD'de Cerner Orta tümleştirmesini yapılandırmak için Cerner Orta Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Cerner Orta eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]

1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim kutusunun en üstünde düğme.

    ![Uygulamalar][3]

1. Arama kutusuna **Cerner orta**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cernercentral-tutorial/tutorial_cernercentral_search.png)

1. Sonuçlar panelinde seçin **Cerner orta**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cernercentral-tutorial/tutorial_cernercentral_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırmanız ve Cerner Merkezi ile Azure AD çoklu oturum açmayı test "Britta Simon." adlı bir test kullanıcı tabanlı

Tek iş için oturum açma için Azure AD ne Cerner Orta karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı Cerner Orta arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Cerner Central ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Cerner Orta test kullanıcısı oluşturma](#creating-a-cerner-central-test-user)**  - kullanıcı Azure AD gösterimini bağlı Cerner Orta Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Cerner Orta uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Cerner Central ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Cerner orta** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/cernercentral-tutorial/tutorial_cernercentral_samlbase.png)

1. Üzerinde **Cerner merkezi etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/cernercentral-tutorial/tutorial_cernercentral_url.png)

    a. İçinde **tanımlayıcı** metin aşağıdaki desenleri kullanarak değeri yazın:

    | |
    |--|
    | `https://<instancename>.cernercentral.com/session-api/protocol/saml2/metadata` |
    | `https://<instancename>.sandboxcernercentral.com/session-api/protocol/saml2/metadata` |
    
    b. İçinde **yanıt URL'si** metin kutusuna bir URL kullanarak aşağıdaki düzenler:
    
    | |
    |--|
    | `https://<instancename>.cernercentral.com/session-api/protocol/saml2/sso` |
    | `https://<instancename>.sandboxcernercentral.com/session-api/protocol/saml2/sso` |

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. İlgili kişi [Cerner Orta Destek ekibine](https://wiki.ucern.com/display/CernerCentral/Contacting+Cloud+Operations) bu değerleri almak için.

1. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** kopyalayıp Not Defteri'ne yapıştırın.

    ![Çoklu oturum açmayı yapılandırın](./media/cernercentral-tutorial/tutorial_metadataurl.png)

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/cernercentral-tutorial/tutorial_general_400.png)

1. Çoklu oturum açmayı yapılandırma **Cerner orta** tarafını göndermek için ihtiyacınız **uygulama Federasyon meta verileri URL'sini** için [Cerner merkezi desteği](https://wiki.ucern.com/display/CernerCentral/Contacting+Cloud+Operations). Bunlar, tümleştirme tamamlamak için uygulama tarafından SSO yapılandırın.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cernercentral-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cernercentral-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cernercentral-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cernercentral-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** Britta simon'un.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.

### <a name="creating-a-cerner-central-test-user"></a>Cerner Orta test kullanıcısı oluşturma

**Cerner orta** uygulama herhangi bir Federasyon kimlik sağlayıcısından gelen kimlik doğrulamasına izin verir. Bir kullanıcı için uygulama giriş sayfası oturum açamaz, birleştirilen ve herhangi bir el ile sağlama için gerek vardır. Daha fazla ayrıntı bulabilirsiniz [burada](cernercentral-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Cerner Merkezi erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200]

**Britta Simon Cerner merkezi siteden atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

1. Uygulamalar listesinde **Cerner orta**.

    ![Çoklu oturum açmayı yapılandırın](./media/cernercentral-tutorial/tutorial_cernercentral_app.png)

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Cerner Orta kutucuğa tıkladığınızda, otomatik olarak Cerner merkezi uygulamanıza açan. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Kullanıcı sağlamayı yapılandırma](cernercentral-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/cernercentral-tutorial/tutorial_general_01.png
[2]: ./media/cernercentral-tutorial/tutorial_general_02.png
[3]: ./media/cernercentral-tutorial/tutorial_general_03.png
[4]: ./media/cernercentral-tutorial/tutorial_general_04.png

[100]: ./media/cernercentral-tutorial/tutorial_general_100.png

[200]: ./media/cernercentral-tutorial/tutorial_general_200.png
[201]: ./media/cernercentral-tutorial/tutorial_general_201.png
[202]: ./media/cernercentral-tutorial/tutorial_general_202.png
[203]: ./media/cernercentral-tutorial/tutorial_general_203.png
