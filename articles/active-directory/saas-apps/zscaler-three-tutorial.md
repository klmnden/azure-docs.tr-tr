---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Zscaler üç | Microsoft Docs'
description: Azure Active Directory ve Zscaler üç arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: f352e00d-68d3-4a77-bb92-717d055da56f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5739c878812c09a8f2646320df4b1069dfa8810e
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56189064"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-three"></a>Öğretici: Zscaler üç ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Zscaler üç tümleştirme konusunda bilgi edinin.

Zscaler üç Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Zscaler üç erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan Zscaler üç için (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi üç Zscaler ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Zscaler üç çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Zscaler üç galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-zscaler-three-from-the-gallery"></a>Zscaler üç galeri ekleme

Azure AD'ye üç Zscaler'ın tümleştirmesini yapılandırmak için Zscaler üç Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Zscaler üç Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Zscaler üç**seçin **Zscaler üç** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde üç Zscaler](./media/zscaler-three-tutorial/tutorial_zscalerthree_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı üç Zscaler ile test edin.

Tek iş için oturum açma için Azure AD ne Zscaler üç karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı Zscaler üç arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Zscaler üç ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Zscaler üç çoklu oturum açmayı yapılandırma](#configure-zscaler-three-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Zscaler üç test kullanıcısı oluşturma](#create-zscaler-three-test-user)**  - kullanıcı Azure AD gösterimini bağlı Cisco terimdir Britta simon'un bir karşılığı vardır.
5. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Zscaler üç uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma üç Zscaler ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Zscaler üç** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunu tıklatın **seçin** için **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](common/tutorial_general_301.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Çoklu oturum açmayı yapılandırın](common/editconfigure.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Zscaler üç etki alanı ve URL'ler tek oturum açma bilgileri](./media/zscaler-three-tutorial/tutorial_zscalerthree_url.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL: `https://login.zscalerthree.net/sfc_sso`

5. Zscaler üç uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri ve talepler** uygulama tümleştirme sayfasında bölümü. Üzerinde **SAML sayfası ile çoklu oturum açmayı ayarlayın**, tıklayın **Düzenle** açmak için düğmeyi **kullanıcı öznitelikleri ve talepler** iletişim.

    ![Öznitelik bağlantısı](./media/zscaler-three-tutorial/tutorial_zscalerthree_attribute.png)

6. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:

    | Ad  | Kaynak özniteliği  |
    | ---------| ------------ |
    | İz     | User.assignedroles |

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](./common/new_save_attribute.png)
    
    ![image](./common/new_attribute_details.png)

    b. Gelen **kaynak özniteliği** selelct öznitelik değeri listesi.

    c. **Tamam**’a tıklayın.

    d. **Kaydet**’e tıklayın.

    > [!NOTE]
    > Lütfen tıklayın [burada](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-app-role-management) Azure AD'de rol yapılandırma bilmek

7. Üzerinde **SAML imzalama sertifikası** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/zscaler-three-tutorial/tutorial_zscalerthree_certificate.png) 

8. Üzerinde **Zscaler üç kümesi** bölümünde, ihtiyacınıza göre uygun URL'yi kopyalayın.

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

    ![Zscaler üç yapılandırma](common/configuresection.png)

### <a name="configure-zscaler-three-single-sign-on"></a>Zscaler üç çoklu oturum açmayı yapılandırın

9. Farklı bir web tarayıcı penceresinde Zscaler üç şirketinizin sitesi için bir yönetici olarak oturum açın.

10. Git **Yönetim > kimlik doğrulama > kimlik doğrulama ayarları** ve aşağıdaki adımları gerçekleştirin:
   
    ![Yönetim](./media/zscaler-three-tutorial/ic800206.png "Yönetim")

    a. Kimlik doğrulaması türü'nün altında seçin **SAML**.

    b. Tıklayın **SAML'yi yapılandırmak**.

11. Üzerinde **Düzenle SAML** penceresinde aşağıdaki adımları gerçekleştirin: ve Kaydet'e tıklayın.  
            
    ![Kullanıcı ve kimlik doğrulaması yönetmek](./media/zscaler-three-tutorial/ic800208.png "kullanıcı ve kimlik doğrulaması'nı yönetme")
    
    a. İçinde **SAML portalı URL'si** metin kutusu, yapıştırma **oturum açma URL'si** , Azure Portalı'ndan kopyaladığınız.

    b. İçinde **oturum açma adı özniteliği** metin girin **Nameıd**.

    c. Tıklayın **karşıya**, içinde Azure portalından indirilen Azure SAML imzalama sertifikasını karşıya yüklemek için **ortak SSL sertifikası**.

    d. İki durumlu **SAML otomatik sağlamayı etkinleştirme**.

    e. İçinde **kullanıcı görünen adı özniteliği** metin girin **displayName** SAML otomatik sağlama displayName öznitelikler için etkinleştirmek istiyorsanız.

    f. İçinde **grubu adı özniteliği** metin girin **memberOf** SAML otomatik sağlama memberOf öznitelikler için etkinleştirmek istiyorsanız.

    g. İçinde **departmanı Name özniteliği** Enter **departmanı** SAML otomatik sağlama için bölüm özniteliklerini etkinleştirmek istiyorsanız.

    i. **Kaydet**’e tıklayın.

12. Üzerinde **kullanıcı kimlik doğrulamasını yapılandırma** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Yönetim](./media/zscaler-three-tutorial/ic800207.png)

    a. Üzerine **etkinleştirme** ekranın sol menü.

    b. Tıklayın **etkinleştirme**.

## <a name="configuring-proxy-settings"></a>Ara sunucu ayarlarını yapılandırma
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Internet Explorer'ın proxy ayarlarını yapılandırmak için

1. Başlangıç **Internet Explorer**.

1. Seçin **Internet Seçenekleri** gelen **Araçları** menüsünü aç **Internet Seçenekleri** iletişim.   
    
     ![Internet Seçenekleri](./media/zscaler-three-tutorial/ic769492.png "Internet Seçenekleri")

1. Tıklayın **bağlantıları** sekmesi.   
  
     ![Bağlantıları](./media/zscaler-three-tutorial/ic769493.png "bağlantıları")

1. Tıklayın **LAN Ayarları** açmak için **LAN Ayarları** iletişim.

1. Proxy sunucusu bölümünde aşağıdaki adımları gerçekleştirin:   
   
    ![Proxy sunucusu](./media/zscaler-three-tutorial/ic769494.png "Proxy sunucusu")

    a. Seçin **AĞINIZ için bir proxy sunucusu kullan**.

    b. Adresi metin kutusuna yazın **ağ geçidi. Zscaler Three.net**.

    c. Bağlantı noktası metin kutusuna yazın **80**.

    d. Seçin **yerel adresler için proxy sunucuyu atla**.

    e. Tıklayın **Tamam** kapatmak için **yerel alan ağı (LAN) ayarları** iletişim.

1. Tıklayın **Tamam** kapatmak için **Internet Seçenekleri** iletişim.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![Azure AD kullanıcısı oluşturun][100]

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Bir Azure AD test kullanıcısı oluşturma](common/create_aaduser_01.png) 

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Bir Azure AD test kullanıcısı oluşturma](common/create_aaduser_02.png)

    a. İçinde **adı** alanına **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alanına **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **özellikleri**seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’u seçin.

### <a name="create-zscaler-three-test-user"></a>Zscaler üç test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon Zscaler üç adlı bir kullanıcı oluşturmaktır. Zscaler üç tam zamanında sağlama, varsayılan olarak etkin olan destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı, henüz yoksa Zscaler üç erişme denemesi sırasında oluşturulur.
>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Zscaler üç Destek ekibine](https://www.zscaler.com/company/contact).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, erişim için üç Zscaler vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **Zscaler üç**.

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-three-tutorial/tutorial_zscalerthree_app.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202]

4. Tıklayın **Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. İçinde **kullanıcılar ve gruplar** kullanıcı gibi iletişim kutusunda **Britta Simon** listeden ardından **seçin** ekranın alt kısmındaki düğmesi.

    ![image](./media/zscaler-three-tutorial/tutorial_zscalerthree_users.png)

6. Gelen **rolü Seç** iletişim listede uygun kullanıcı rolünü seçin ve ardından tıklayın **seçin** ekranın alt kısmındaki düğmesi.

    ![image](./media/zscaler-three-tutorial/tutorial_zscalerthree_roles.png)

7. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.

    ![image](./media/zscaler-three-tutorial/tutorial_zscalerthree_assign.png)

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Zscaler üç kutucuğa tıkladığınızda, otomatik olarak Zscaler üç uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
