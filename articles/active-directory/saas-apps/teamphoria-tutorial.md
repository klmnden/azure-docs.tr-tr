---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Teamphoria | Microsoft Docs'
description: Azure Active Directory ve Teamphoria arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: d569c705-6f0f-4ec1-b485-ba82526b5d32
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0f32a4ebd5a28c1054c19c578f3ba82e3b4951a9
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56184117"
---
# <a name="tutorial-azure-active-directory-integration-with-teamphoria"></a>Öğretici: Teamphoria ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Teamphoria tümleştirme konusunda bilgi edinin.

Azure AD ile Teamphoria tümleştirme ile aşağıdaki avantajları sağlar:

- Teamphoria erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Teamphoria (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Teamphoria yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir Teamphoria çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin.
Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Teamphoria ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-teamphoria-from-the-gallery"></a>Galeriden Teamphoria ekleme
Azure AD'de Teamphoria tümleştirmesini yapılandırmak için Teamphoria Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Teamphoria eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalı](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Tıklayın **Ekle** iletişim kutusunun üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Teamphoria**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/teamphoria-tutorial/tutorial_teamphoria_search.png)

1. Sonuçlar panelinde seçin **Teamphoria**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/teamphoria-tutorial/tutorial_teamphoria_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Teamphoria sınayın.

Tek iş için oturum açma için Azure AD ne Teamphoria karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Teamphoria ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Teamphoria ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Teamphoria test kullanıcısı oluşturma](#creating-a-teamphoria-test-user)**  - Azure AD gösterimini her için bağlı Teamphoria Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Teamphoria uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Teamphoria yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Teamphoria** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/teamphoria-tutorial/tutorial_teamphoria_samlbase.png)

1. Üzerinde **Teamphoria etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/teamphoria-tutorial/tutorial_teamphoria_url.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak URL'yi yazın: `https://<sub-domain>.teamphoria.com/login`   

    > [!NOTE] 
    > Oturum açma URL değeri, gerçek değil. Bu değer gerçek oturum açma URL'si ile güncelleştirmeniz gerekiyor. İlgili kişi [Teamphoria istemci Destek ekibine](https://www.teamphoria.com/) oturum açma URL'sini alabilirsiniz.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve sertifika bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/teamphoria-tutorial/tutorial_teamphoria_certificate.png)

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/teamphoria-tutorial/tutorial_general_400.png)

1. Üzerinde **Teamphoria yapılandırma** bölümünde **yapılandırma Teamphoria** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/teamphoria-tutorial/tutorial_teamphoria_configure.png)

1. Çoklu oturum açmayı yapılandırma **Teamphoria** tarafını Teamphoria uygulamanızı yönetici olarak oturum açın.

1. Git **yönetici ayarları** sol araç çubuğundaki ve yapılandırma sekmesi altındaki seçeneği tıklatın **çoklu oturum açma** SSO Yapılandırması penceresi açmak için.

    ![Çoklu oturum açmayı yapılandırın](./media/teamphoria-tutorial/admin_sso_configure.png)

1. Tıklayarak **yeni kimlik SAĞLAYICISI Ekle** SSO ayarlarını ekleme formunu açmak için sağ üst köşedeki seçeneği.

    ![Çoklu oturum açmayı yapılandırın](./media/teamphoria-tutorial/add_new_identity_provider.png)

1. Aşağıdaki - açıklandığı gibi alanlarda ayrıntıları girin

    ![Çoklu oturum açmayı yapılandırın](./media/teamphoria-tutorial/Teamphoria_sso_save.png)

    a. **GÖRÜNEN AD**: Yönetici sayfasına eklenti görünen adını girin.

    b. **DÜĞME ADI**: SSO ile oturum açma için oturum açma sayfasında görüntülenecek sekmenin adı.

    c. **SERTİFİKA**: Daha önce Not Defteri'nde Azure portalından indirdiğiniz sertifikayı açın, aynı içeriğini kopyalayıp buraya kutuya yapıştırın.

    d. **GİRİŞ NOKTASI**: Yapıştırma **SAML çoklu oturum açma hizmeti URL'si** daha önce Azure portaldan kopyaladığınız.

    e. Geçiş seçeneği **ON** tıklayın **Kaydet**.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/teamphoria-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/teamphoria-tutorial/create_aaduser_02.png) 

1. İletişim kutusunun en üstünde tıklayın **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/teamphoria-tutorial/create_aaduser_03.png)

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/teamphoria-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.

### <a name="creating-a-teamphoria-test-user"></a>Teamphoria test kullanıcısı oluşturma

Teamphoria açarken Azure AD kullanıcılarının etkinleştirmek için bunların Teamphoria sağlanması gerekir. Teamphoria söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Teamphoria şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Tıklayarak **yönetici** ayarları altında ve sol araç çubuğundaki **Yönet** sekmesine tıklatın **kullanıcılar** kullanıcılar için Yönetim sayfasını açın.

    ![Çalışan Ekle](./media/teamphoria-tutorial/admin_manage_users.png)

1. Tıklayarak **el ile davet** seçeneği.

    ![Kişileri davet edin](./media/teamphoria-tutorial/admin_manage_add_users.png)

1. Bu sayfada, eylemi gerçekleştirin.
    
    ![Kişileri davet edin](./media/teamphoria-tutorial/manual_user_invite.png)

    a. İçinde **e-posta adresi** metin **e-posta adresi** BrittaSimon biri.

    b. İçinde **ad** metin kutusuna **Britta**.

    c. İçinde **SOYADI** metin kutusuna **Simon**.

    d. Tıklayın **davet 1 kullanıcı**. Kullanıcı sistemde oluşturulmasına daveti kabul etmek gerekir.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Teamphoria erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200]

**Britta Simon Teamphoria için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

1. Uygulamalar listesinde **Teamphoria**.

    ![Çoklu oturum açmayı yapılandırın](./media/teamphoria-tutorial/tutorial_teamphoria_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Çoklu oturum açma ayarları test etmek isterseniz, erişim Paneli'nde açın. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/teamphoria-tutorial/tutorial_general_01.png
[2]: ./media/teamphoria-tutorial/tutorial_general_02.png
[3]: ./media/teamphoria-tutorial/tutorial_general_03.png
[4]: ./media/teamphoria-tutorial/tutorial_general_04.png

[100]: ./media/teamphoria-tutorial/tutorial_general_100.png

[200]: ./media/teamphoria-tutorial/tutorial_general_200.png
[201]: ./media/teamphoria-tutorial/tutorial_general_201.png
[202]: ./media/teamphoria-tutorial/tutorial_general_202.png
[203]: ./media/teamphoria-tutorial/tutorial_general_203.png
