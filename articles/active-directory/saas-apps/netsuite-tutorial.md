---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle muhasebesi | Microsoft Docs'
description: Azure Active Directory ve muhasebesi arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: dafa0864-aef2-4f5e-9eac-770504688ef4
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2018
ms.author: jeedes
ms.openlocfilehash: 511fdcf587d16a59ff2bb11dfc55504b2218a569
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39431425"
---
# <a name="tutorial-azure-active-directory-integration-with-netsuite"></a>Öğretici: Azure Active Directory muhasebesi ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile muhasebesi tümleştirme konusunda bilgi edinin.

Azure AD ile muhasebesi tümleştirme ile aşağıdaki avantajları sağlar:

- Muhasebesi erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için muhasebesi (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile muhasebesi yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik muhasebesi çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden muhasebesi ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-netsuite-from-the-gallery"></a>Galeriden muhasebesi ekleme
Azure AD'de muhasebesi tümleştirmesini yapılandırmak için muhasebesi Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden muhasebesi eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]

1. Tıklayın **yeni uygulama** iletişim kutusunun üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **muhasebesi**seçin **muhasebesi** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde muhasebesi](./media/netsuite-tutorial/tutorial_netsuite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı muhasebesi ile test etme

Tek çalışmak için oturum açma için Azure AD ne muhasebesi karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının muhasebesi ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değerini atayarak bu bağlantı ilişki kurulduktan **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** muhasebesi içinde.

Yapılandırma ve Azure AD çoklu oturum açma muhasebesi ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Muhasebesi test kullanıcısı oluşturma](#creating-a-netsuite-test-user)**  - kullanıcı Azure AD gösterimini bağlı muhasebesi Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve muhasebesi uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile muhasebesi yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **muhasebesi** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/tutorial_NetSuite_samlbase.png)

1. Üzerinde **muhasebesi etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/tutorial_NetSuite_url.png)

    İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak:

    `https://<tenant-name>.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na1.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na2.NetSuite.com/saml2/acs`

    `https://<tenant-name>.sandbox.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na1.sandbox.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na2.sandbox.NetSuite.com/saml2/acs`
    
    > [!NOTE]
    > Bunlar gerçek değerler değildir. Bu değerler gerçek yanıt URL'si ile güncelleştirin. İlgili kişi [muhasebesi Destek ekibine](http://www.NetSuite.com/portal/services/support.shtml) bu değerleri almak için.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda XML dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/tutorial_NetSuite_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/tutorial_general_400.png)

1. Üzerinde **muhasebesi yapılandırma** bölümünde **yapılandırma muhasebesi** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/tutorial_NetSuite_configure.png)

1. Tarayıcınızda yeni bir sekme açın ve muhasebesi şirketinizin sitesi yönetici olarak oturum açın.

1. Sayfanın üst kısmındaki araç çubuğunda **Kurulum**, ardından gidin **şirket** tıklatıp **özellikleri etkinleştirmek**.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-setupsaml.png)

1. Sayfanın ortasında, araç çubuğunda **SuiteCloud**.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-suitecloud.png)

1. Altında **yönetme kimlik doğrulaması** bölümünden **SAML çoklu oturum açma** muhasebesi SAML çoklu oturum açma seçeneği etkinleştirme.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-ticksaml.png)

1. Sayfanın üst kısmındaki araç çubuğunda **Kurulum**.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-setup.png)

1. Gelen **Kurulum görevleri** listesinde **tümleştirme**.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-integration.png)

1. İçinde **yönetme kimlik doğrulaması** bölümünde **SAML çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-saml.png)

1. Üzerinde **SAML Kurulumu** sayfasındaki **muhasebesi yapılandırma** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-saml-setup.png)
  
    a. Seçin **birincil kimlik doğrulama YÖNTEMİNİN**.

    b. Etiketli alanın **SAMLV2 kimlik SAĞLAYICISI meta verileri**seçin **IDP meta veri dosyasını karşıya yükle**. Ardından **Gözat** Azure portalından indirdiğiniz meta veri dosyasını karşıya yükleyin.

    c. Tıklayın **gönderme**.

1. Azure AD'de tıklayarak **görünümü ve diğer tüm kullanıcı özniteliklerini düzenleyin** onay kutusu ve öznitelik ekleyin.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-attributes.png)

1. İçin **öznitelik adı** yazın, alan `account`. İçin **öznitelik değeri** alanında, muhasebesi hesabı kimliğinizi yazın Bu, sabit ve değişiklik hesabıyla değerdir. Hesap Kimliğinizi bulmak yönergeler aşağıda verilmiştir:

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-add-attribute.png)

    a. Muhasebesi tıklayın **Kurulum** gidin **şirket** tıklatıp **şirket bilgilerini** üst gezinti menüsünde.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-com.png)

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-account-id.png)

    b. İçinde **şirket bilgilerini** sağ sütunda Kopyala sayfasında **hesap kimliği**.

    c. Yapıştırma **hesap kimliği** içine muhasebesi hesabından kopyalanan **öznitelik değeri** Azure ad alanı. 

1. Kullanıcılara çoklu oturum açma muhasebesi gerçekleştirmeden önce muhasebesi için uygun izinler önce atanmalıdır. Bu izinleri atamak için aşağıdaki yönergeleri izleyin.

    a. Üst gezinti menüsünde **Kurulum**.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-setup.png)

    b. Sol gezinti menüsünde **kullanıcıları/rolleri**, ardından **Rolleri Yönet**.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-manage-roles.png)

    c. Tıklayın **yeni rol**.

    d. Yazın bir **adı** yeni rolünüz için.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-new-role.png)

    e. **Kaydet**’e tıklayın.

    f. Üstteki menüden **izinleri**. Ardından **Kurulum**.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-sso.png)

    g. Seçin **SAML çoklu oturum açma**ve ardından **Ekle**.

    h. **Kaydet**’e tıklayın.

    i. Üst gezinti menüsünde **Kurulum**, ardından **Yöneticisi kurulumunu**.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-setup.png)

    j. Sol gezinti menüsünde **kullanıcıları/rolleri**, ardından **Kullanıcıları Yönet**.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-manage-users.png)

    k. Bir test kullanıcısı seçin. Ardından **Düzenle** gidin **erişim** sekmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-edit-user.png)

    m. Rol iletişim kutusunda, oluşturduğunuz uygun rolü atayın.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-add-role.png)

    m. **Kaydet**’e tıklayın.
 
### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/NetSuite-tutorial/create_aaduser_01.png) 

1.  Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/NetSuite-tutorial/create_aaduser_02.png) 

1. İletişim kutusunun en üstünde tıklayın **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/NetSuite-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/NetSuite-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın. 

### <a name="creating-a-netsuite-test-user"></a>Muhasebesi test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı muhasebesi oluşturulur. Muhasebesi tam zamanında sağlama, varsayılan olarak etkin olduğu destekler.
Bu bölümde, hiçbir eylem öğesini yoktur. Bir kullanıcı muhasebesi içinde zaten mevcut değilse muhasebesi erişmeye çalıştığında yeni bir tane oluşturulur.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için muhasebesi erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon için muhasebesi atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **muhasebesi**.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/tutorial_NetSuite_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Çoklu oturum açma ayarları test etmek için adresinden erişim Paneli'nde açın [ https://myapps.microsoft.com ](https://myapps.microsoft.com/), test hesabında oturum açın ve tıklayın **muhasebesi**.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Kullanıcı sağlamayı yapılandırma](NetSuite-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/NetSuite-tutorial/tutorial_general_01.png
[2]: ./media/NetSuite-tutorial/tutorial_general_02.png
[3]: ./media/NetSuite-tutorial/tutorial_general_03.png
[4]: ./media/NetSuite-tutorial/tutorial_general_04.png

[100]: ./media/NetSuite-tutorial/tutorial_general_100.png

[200]: ./media/NetSuite-tutorial/tutorial_general_200.png
[201]: ./media/NetSuite-tutorial/tutorial_general_201.png
[202]: ./media/NetSuite-tutorial/tutorial_general_202.png
[203]: ./media/NetSuite-tutorial/tutorial_general_203.png

