---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile muhasebesi | Microsoft Docs'
description: Azure Active Directory ve muhasebesi arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: dafa0864-aef2-4f5e-9eac-770504688ef4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/17/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ab329574ef425e8133ac746c185050efcc8bc15a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "65871040"
---
# <a name="tutorial-azure-active-directory-integration-with-netsuite"></a>Öğretici: Muhasebesi ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile muhasebesi tümleştirme konusunda bilgi edinin.
Azure AD ile muhasebesi tümleştirme ile aşağıdaki avantajları sağlar:

* Muhasebesi erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) muhasebesi için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile muhasebesi yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik muhasebesi çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Muhasebesi destekler **IDP** tarafından başlatılan
* Muhasebesi destekler **zamanında** kullanıcı sağlama
* Muhasebesi destekler [otomatik kullanıcı hazırlama](NetSuite-provisioning-tutorial.md)

## <a name="adding-netsuite-from-the-gallery"></a>Galeriden muhasebesi ekleme

Azure AD'de muhasebesi tümleştirmesini yapılandırmak için muhasebesi Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden muhasebesi eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **muhasebesi**seçin **muhasebesi** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde muhasebesi](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma muhasebesi adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının muhasebesi ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma muhasebesi ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Muhasebesi çoklu oturum açmayı yapılandırma](#configure-netsuite-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Muhasebesi test kullanıcısı oluşturma](#create-netsuite-test-user)**  - kullanıcı Azure AD gösterimini bağlı muhasebesi Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile muhasebesi yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **muhasebesi** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Muhasebesi etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-reply.png)

    İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak:

    `https://<tenant-name>.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na1.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na2.NetSuite.com/saml2/acs`

    `https://<tenant-name>.sandbox.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na1.sandbox.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na2.sandbox.NetSuite.com/saml2/acs`

    > [!NOTE]
    > Değer, gerçek değil. Değerini gerçek yanıt URL'si ile güncelleştirin. İlgili kişi [muhasebesi istemci Destek ekibine](http://www.netsuite.com/portal/services/support-services/suitesupport.shtml) değeri alınamıyor. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Muhasebesi uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **kullanıcı öznitelikleri** iletişim.

    ![image](common/edit-attribute.png)

6. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda kullanarak talep Düzenle **düzenleme simgesi** veya talep kullanarak **Ekle yeni talep**SAML belirteci özniteliği yukarıdaki görüntüde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Ad | Kaynak özniteliği | 
    | ---------------| --------------- |
    | hesap  | `account id` |

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Bırakın **Namespace** boş.

    d. Kaynağı olarak **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

    >[!NOTE]
    >Hesap özniteliğinin değerini gerçek değil. Bu değer, öğreticinin ilerleyen bölümlerinde açıklanan güncelleştirir.

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **meta veri XML**bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. Üzerinde **muhasebesi kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-netsuite-single-sign-on"></a>Muhasebesi tek oturum açmayı yapılandırın

1. Tarayıcınızda yeni bir sekme açın ve muhasebesi şirketinizin sitesi yönetici olarak oturum açın.

2. Sayfanın üst kısmındaki araç çubuğunda **Kurulum**, ardından gidin **şirket** tıklatıp **özellikleri etkinleştirmek**.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-setupsaml.png)

3. Sayfanın ortasında, araç çubuğunda **SuiteCloud**.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-suitecloud.png)

4. Altında **yönetme kimlik doğrulaması** bölümünden **SAML çoklu oturum açma** muhasebesi SAML çoklu oturum açma seçeneği etkinleştirme.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-ticksaml.png)

5. Sayfanın üst kısmındaki araç çubuğunda **Kurulum**.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-setup.png)

6. Gelen **Kurulum görevleri** listesinde **tümleştirme**.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-integration.png)

7. İçinde **yönetme kimlik doğrulaması** bölümünde **SAML çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-saml.png)

8. Üzerinde **SAML Kurulumu** sayfasındaki **muhasebesi yapılandırma** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-saml-setup.png)
  
    a. Seçin **birincil kimlik doğrulama YÖNTEMİNİN**.

    b. Etiketli alanın **SAMLV2 kimlik SAĞLAYICISI meta verileri**seçin **IDP meta veri dosyasını karşıya yükle**. Ardından **Gözat** Azure portalından indirdiğiniz meta veri dosyasını karşıya yükleyin.

    c. **Gönder**'e tıklayın.

9. Muhasebesi tıklayın **Kurulum** gidin **şirket** tıklatıp **şirket bilgilerini** üst gezinti menüsünde.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-com.png)

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-account-id.png)

    b. İçinde **şirket bilgilerini** sağ sütunda Kopyala sayfasında **hesap kimliği**.

    c. Yapıştırma **hesap kimliği** içine muhasebesi hesabından kopyalanan **öznitelik değeri** Azure ad alanı. 

10. Kullanıcılara çoklu oturum açma muhasebesi gerçekleştirmeden önce muhasebesi için uygun izinler önce atanmalıdır. Bu izinleri atamak için aşağıdaki yönergeleri izleyin.

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

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon\@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için muhasebesi erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **muhasebesi**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde yazın ve **muhasebesi**.

    ![Uygulamalar listesinde muhasebesi bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-netsuite-test-user"></a>Muhasebesi test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı muhasebesi oluşturulur. Muhasebesi just-ın-time kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Bir kullanıcı muhasebesi içinde zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli muhasebesi kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama muhasebesi için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Kullanıcı sağlamayı yapılandırma](NetSuite-provisioning-tutorial.md)

