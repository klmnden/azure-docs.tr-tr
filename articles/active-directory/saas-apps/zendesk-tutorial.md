---
title: 'Öğretici: Azure Active Directory Zendesk ile tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Zendesk arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 9d7c91e5-78f5-4016-862f-0f3242b00680
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2018
ms.author: jeedes
ms.openlocfilehash: 37d20eabfc8fb883cda346dc8b55a17b8b959218
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48268183"
---
# <a name="tutorial-azure-active-directory-integration-with-zendesk"></a>Öğretici: Azure Active Directory Zendesk ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Zendesk tümleştirme konusunda bilgi edinin.

Zendesk Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Zendesk erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için bir Zendesk (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Zendesk ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir Zendesk çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin.
Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Zendesk galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-zendesk-from-the-gallery"></a>Zendesk galeri ekleme

Azure AD'de Zendesk tümleştirmesini yapılandırmak için Zendesk Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Zendesk eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![image](./media/zendesk-tutorial/selectazuread.png)

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![image](./media/zendesk-tutorial/a_select_app.png)
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![image](./media/zendesk-tutorial/a_new_app.png)

4. Arama kutusuna **Zendesk**seçin **Zendesk** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![image](./media/zendesk-tutorial/a_add_app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Zendesk ile test edin.

Tek çalışmak için oturum açma için Azure AD ne Zendesk karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve Zendesk ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Zendesk'te, değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Zendesk ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Zendesk test kullanıcısı oluşturma](#create-a-zendesk-test-user)**  - kullanıcı Azure AD gösterimini bağlı Zendesk Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Zendesk uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Zendesk ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde [Azure portalında](https://portal.azure.com/), **Zendesk** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![image](./media/zendesk-tutorial/b1_b2_select_sso.png)

2. Tıklayın **değişiklik çoklu oturum açma modunu** seçmek için ekranın en üstünde **SAML** modu.

      ![image](./media/zendesk-tutorial/b1_b2_saml_ssso.png)

3. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunu tıklatın **seçin** için **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![image](./media/zendesk-tutorial/b1_b2_saml_sso.png)

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **temel SAML yapılandırma** iletişim.

    ![image](./media/zendesk-tutorial/b1-domains_and_urlsedit.png)

5. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<subdomain>.zendesk.com`.

    b. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `<subdomain>.zendesk.com`.

    ![image](./media/zendesk-tutorial/b1-domains_and_urls.png)

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [Zendesk istemci Destek ekibine](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise) bu değerleri almak için.

6. Zendesk, belirli bir biçimde SAML onaylamalarını bekliyor. Zorunlu SAML öznitelikleri vardır, ancak isteğe bağlı olarak bir öznitelik alma ekleyebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **kullanıcı öznitelikleri** iletişim.

    ![image](./media/zendesk-tutorial/i4-attribute.png)

7. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](./media/zendesk-tutorial/i2-attribute.png)

    ![image](./media/zendesk-tutorial/i3-attribute.png)
    
    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Bırakın **Namespace** boş.

    d. Kaynağı olarak **özniteliği**.
    
    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

    > [!NOTE]
    > Uzantı öznitelikleri, varsayılan olarak Azure AD'de olmayan öznitelikler eklemek için kullanın. Tıklayın [SAML ayarlanabilir kullanıcı öznitelikleri](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-) tam listesini almak için SAML öznitelikleri **Zendesk** kabul eder.

8. SAML imzalama sertifikası bölümünde, içinde **SAML imzalama sertifikası** bölümünde, kopya **parmak izi**ve bilgisayarınıza kaydedin.

    ![image](./media/zendesk-tutorial/C3_certificate.png)

    a. Uygun seçeneğini **imzalama seçeneği** gerekirse.

    b. Uygun seçeneğini **imzalama algoritması** gerekirse.

    c. **Kaydet**’e tıklayın

9. Üzerinde **Zendesk kümesi** bölümünde **görüntülemek hakkında adım adım yönergeler** açmak için **yapılandırma oturum açma** penceresi. Kopyalama URL'ler, aşağıda gelen **hızlı başvuru bölümü.**

    Not: url aşağıdaki bildirebilir

    a. SAML çoklu oturum açma hizmeti URL'si

    b. Varlık Kimliği

    c. Oturum kapatma URL'si

    ![image](./media/zendesk-tutorial/d1_saml.png) 

10. İçinde Zendesk yapılandırılabilir - iki yolu otomatik ve el ile vardır.
  
11. Zendesk içinde yapılandırmasını otomatik hale getirmenizi yüklemeniz gerekir **My Apps güvenli oturum açma tarayıcı uzantısı** tıklayarak **uzantıyı yükleme**.

    ![image](./media/zendesk-tutorial/install_extension.png)

12. Uzantı tarayıcıya ekledikten sonra tıklayarak **Kurulum Zendesk** Zendesk uygulamaya yönlendirir. Burada, Zendesk oturum açmak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı otomatik olarak sizin için uygulamayı yapılandırma ve 13. adım otomatikleştirin.

     ![image](./media/zendesk-tutorial/d2_saml.png) 

13. Zendesk el ile ayarlamak istiyorsanız, yeni bir web tarayıcı penceresi ve günlük Zendesk şirket sitenizin yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

    * Tıklayın **yönetici**.

    * Sol gezinti bölmesinden **ayarları**ve ardından **güvenlik**.

    * Üzerinde **güvenlik** sayfasında, aşağıdaki adımları gerçekleştirin:

      ![Güvenlik](././media/zendesk-tutorial/ic773089.png "güvenlik")

      ![Çoklu oturum açma](././media/zendesk-tutorial/ic773090.png "çoklu oturum açma")

      a. Tıklayın **yönetici ve aracılar** sekmesi.

      b. Seçin **çoklu oturum açma (SSO) ve SAML**ve ardından **SAML**.

      c. İçinde **SAML SSO URL** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız.

      d. İçinde **uzak oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.

      e. İçinde **sertifika parmak izi** metin kutusu, yapıştırma **parmak izi** Azure Portalı'ndan kopyaladığınız sertifika değeri.

      f. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![image](./media/zendesk-tutorial/d_users_and_groups.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![image](./media/zendesk-tutorial/d_adduser.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![image](./media/zendesk-tutorial/d_userproperties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **özellikleri**seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’u seçin.

### <a name="create-a-zendesk-test-user"></a>Zendesk test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon Zendesk'te adlı bir kullanıcı oluşturmaktır. Zendesk otomatik kullanıcı hazırlama, varsayılan olarak etkin olan destekler. Daha fazla ayrıntı bulabilirsiniz [burada](Zendesk-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

**Kullanıcı el ile oluşturmanız gerekiyorsa, lütfen aşağıdaki adımları uygulayın:**

> [!NOTE]
> **Son kullanıcı** hesapları otomatik olarak oturum açarken sağlandı. **Aracı** ve **yönetici** hesapları içinde el ile sağlanması gerekir **Zendesk** oturum açmadan önce.

1. Oturum açın, **Zendesk** Kiracı.

2. Seçin **müşteri listesi** sekmesi.

3. Seçin **kullanıcı** sekmesine ve tıklayın **Ekle**.

    ![Kullanıcı Ekle](././media/zendesk-tutorial/ic773632.png "Kullanıcı Ekle")
4. Tür **adı** ve **e-posta** sağlamak istediğiniz ve ardından var olan bir Azure AD hesabının **Kaydet**.

    ![Yeni kullanıcı](././media/zendesk-tutorial/ic773633.png "yeni kullanıcı")

> [!NOTE]
> Herhangi diğer Zendesk kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak Zendesk tarafından sağlanan.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Zendesk erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![image](./media/zendesk-tutorial/d_all_applications.png)

2. Uygulamalar listesinde **Zendesk**.

    ![image](./media/zendesk-tutorial/d_all_zendeskapplications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![image](./media/zendesk-tutorial/d_leftpaneusers.png)

4. Seçin **Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![image](./media/zendesk-tutorial/d_assign_user.png)

4. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

5. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Zendesk kutucuğa tıkladığınızda, otomatik olarak Zendesk uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Kullanıcı sağlamayı yapılandırma](zendesk-provisioning-tutorial.md)
