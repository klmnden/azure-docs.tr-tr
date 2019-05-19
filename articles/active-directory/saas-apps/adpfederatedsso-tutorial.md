---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile ADP | Microsoft Docs'
description: Azure Active Directory ve ADP arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 7be5331b-0481-48f7-9d6b-619dfec657e1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/04/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: eba63f8295fb5bebffdc8480f763c852521e331b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "65859247"
---
# <a name="tutorial-azure-active-directory-integration-with-adp"></a>Öğretici: ADP ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile ADP tümleştirme konusunda bilgi edinin.
Azure AD ile ADP tümleştirme ile aşağıdaki avantajları sağlar:

* ADP erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak ADP (çoklu oturum açma) ile Azure AD hesaplarına oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile ADP yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik ADP çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* ADP destekler **IDP** tarafından başlatılan

## <a name="adding-adp-from-the-gallery"></a>Galeriden ADP ekleme

Azure AD'de ADP tümleştirmesini yapılandırmak için ADP Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden ADP eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **ADP**seçin **ADP** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde ADP](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ADP adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ile ilgili kullanıcı ADP'de arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma ADP ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[ADP çoklu oturum açmayı yapılandırma](#configure-adp-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[ADP test kullanıcısı oluşturma](#create-adp-test-user)**  - kullanıcı Azure AD gösterimini bağlı ADP'de Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile ADP yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure portalında, üzerinde **ADP** uygulama tümleştirme sayfası, tıklayarak **Özellikleri sekmesi** ve aşağıdaki adımları gerçekleştirin: 

    ![Çoklu oturum açma özellikleri](./media/adpfederatedsso-tutorial/tutorial_adp_prop.png)

    a. Ayarlama **kullanıcıların oturum açması etkinleştirildi** alan için değer **Evet**.

    b. Kopyalama **kullanıcı erişim URL'SİNDEN** ve yapıştırmak zorunda **yapılandırma oturum açma URL'si bölüm**, öğreticinin ilerleyen bölümlerinde açıklanmıştır.

    c. Ayarlama **kullanıcı ataması gerekli** alan için değer **Evet**.

    d. Ayarlama **kullanıcılara görünür** alan için değer **Hayır**.

2. İçinde [Azure portalında](https://portal.azure.com/), **ADP** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

3. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

5. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![ADP etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-identifier.png)

    İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna bir URL yazın:  `https://fed.adp.com`

6. ADP uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **kullanıcı öznitelikleri** iletişim. Talep adı her zaman olacaktır **"PersonImmutableID"** ve değeri hangisinin size, ile eşlemek için gösterilen **EmployeeID**. 

    Azure AD kullanıcı eşlemeyi ADP üzerinde yapılan **EmployeeID** ancak bu uygulama ayarlarınıza göre farklı bir değer eşleyebilirsiniz. Bu nedenle iş Lütfen ile [ADP Destek ekibine](https://www.adp.com/contact-us/overview.aspx) önce bir kullanıcının doğru tanımlayıcı kullanın ve bu değerle eşleştirmek için **"PersonImmutableID"** talep.

    ![image](common/edit-attribute.png)

7. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda kullanarak talep Düzenle **düzenleme simgesi** veya talep kullanarak **Ekle yeni talep**SAML belirteci özniteliği yukarıdaki görüntüde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Ad | Kaynak özniteliği | 
    | ---------------| --------------- |
    | PersonImmutableID  | User.employeeid |

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Bırakın **Namespace** boş.

    d. Kaynağı olarak **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

    > [!NOTE] 
    > SAML onaylaması yapılandırmadan önce iletişime geçmeniz, [ADP Destek ekibine](https://www.adp.com/contact-us/overview.aspx) ve kiracınız için benzersiz kullanıcı tanımlayıcısı özniteliğinin değeri isteyin. Uygulamanız için özel talep yapılandırmak için bu değere ihtiyacınız. 

8. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

### <a name="configure-adp-single-sign-on"></a>ADP tek oturum açmayı yapılandırın

Çoklu oturum açmayı yapılandırma **ADP** tarafını ihtiyacınız indirilen yüklenecek **meta veri XML** üzerinde [ADP Web sitesi](https://adpfedsso.adp.com/public/login/index.fcc).

> [!NOTE]  
> Bu işlem, birkaç gün sürebilir.

### <a name="configure-your-adp-services-for-federated-access"></a>ADP hizmetlere federe erişim için yapılandırma

>[!Important]
> Çalışanlarınızın ADP hizmetlerinizi Federasyon erişmesi ADP service uygulamasına ve daha sonra atanması gerekir, kullanıcılar ilgili ADP hizmete atanmaları gerekir.
ADP temsilcinize onayı alındığında ADP hizmete kullanıcı erişimi denetlemek için ADP hizmetlere ve atama ve yönetme kullanıcılarınızın yapılandırın.

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **ADP**seçin **ADP** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde ADP](common/search-new-app.png)

5. Azure portalında, üzerinde **ADP** uygulama tümleştirme sayfası, tıklayarak **Özellikleri sekmesi** ve aşağıdaki adımları gerçekleştirin:  

    ![Çoklu oturum açma bağlı özellikleri](./media/adpfederatedsso-tutorial/tutorial_adp_linkedproperties.png)

    a.  Ayarlama **kullanıcıların oturum açması etkinleştirildi** alan için değer **Evet**.

    b.  Ayarlama **kullanıcı ataması gerekli** alan için değer **Evet**.

    c.  Ayarlama **kullanıcılara görünür** alan için değer **Evet**.

6. İçinde [Azure portalında](https://portal.azure.com/), **ADP** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

7. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **modu** olarak **bağlı**. uygulamanıza bağlamak için **ADP**.

    ![Çoklu oturum bağlı açma](./media/adpfederatedsso-tutorial/tutorial_adp_linked.png)

8. Gidin **yapılandırma oturum açma URL'si** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma prop](./media/adpfederatedsso-tutorial/tutorial_adp_linkedsignon.png)

    a. Yapıştırma **kullanıcı erişim URL'SİNDEN**, yukarıdaki kopyalanan **Özellikleri sekmesi** (uygulamadan ana ADP).
                                                             
    b. Aşağıda, 5 farklı destekleyen uygulamalar **geçiş durumu URL'leri**. Uygun eklemek zorunda **geçiş durumu URL** el ile geçirmek için belirli uygulama değer **kullanıcı erişim URL'SİNDEN**.
    
    * **Artık ADP iş gücü**
        
        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?WFN`

    * **ADP iş gücü artık süresi geliştirildi**
        
        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?EETDC2`
    
    * **HCM ADP görüş**
        
        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?ADPVANTAGE`

    * **İK ADP Enterprise**

        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?PORTAL`

    * **MyADP**

        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?REDBOX`

9. **Kaydet** yaptığınız değişiklikleri.

10. Onay ekranında ADP temsilcinize alındığında, bir veya iki kullanıcı ile test başlayın.

    a. Birkaç kullanıcı ADP hizmete Federasyon erişimi test etmek için uygulama atayın.

    b. Kullanıcılar ADP hizmet app Galerisi'nde erişmek ve bunların ADP hizmete erişebilir, test başarılı olur.
 
11. Bireysel kullanıcılar ya da kullanıcı grupları, öğreticinin ilerleyen bölümlerinde açıklanmıştır ve çalışanlarınıza kullanıma konusunda daha fazla onay başarılı test federe ADP hizmet atayın.
 
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

Bu bölümde, Azure çoklu oturum açma ADP erişim vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **ADP**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde yazın ve **ADP**.

    ![Uygulamalar listesinde ADP bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-adp-test-user"></a>ADP test kullanıcısı oluşturma

Bu bölümün amacı ADP Britta Simon adlı bir kullanıcı oluşturmaktır. Çalışmak [ADP Destek ekibine](https://www.adp.com/contact-us/overview.aspx) ADP hesabında kullanıcıları eklemek için. 

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli ADP kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama ADP için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

