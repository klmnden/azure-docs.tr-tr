---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Zscaler üç | Microsoft Docs'
description: Azure Active Directory ve Zscaler üç arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f352e00d-68d3-4a77-bb92-717d055da56f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/12/2018
ms.author: jeedes
ms.openlocfilehash: 0ef8fc2ea8b006d49dd54d638183a58bf78a5797
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2018
ms.locfileid: "49312995"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-three"></a>Öğretici: Azure Active Directory Tümleştirme ile Zscaler üç

Bu öğreticide, Azure Active Directory (Azure AD) ile Zscaler üç tümleştirme konusunda bilgi edinin.

Zscaler üç Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Zscaler üç erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan Zscaler üç için (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi üç Zscaler ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Zscaler üç çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme burada alabilirsiniz: [deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin.
Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Zscaler üç galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-zscaler-three-from-the-gallery"></a>Zscaler üç galeri ekleme

Azure AD'ye üç Zscaler'ın tümleştirmesini yapılandırmak için Zscaler üç Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Zscaler üç Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Uygulamalar][2]

3. Tıklayın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Sonuçlar panelinde seçin **Zscaler üç**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-three-tutorial/tutorial_zscalerthree_addfromgallery.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı üç Zscaler ile test edin.

Tek iş için oturum açma için Azure AD ne Zscaler üç karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı Zscaler üç arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Zscaler üç ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Ara sunucu ayarlarını yapılandırma](#configuring-proxy-settings)**  - Internet Explorer'da proxy ayarlarını yapılandırmak için
3. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Zscaler üç bir test kullanıcısı oluşturma](#creating-a-zscaler-three-test-user)**  - Zscaler kullanıcı Azure AD gösterimini bağlı üç Britta simon'un bir karşılığı vardır.
5. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
6. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Zscaler üç uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma üç Zscaler ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Zscaler üç** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-three-tutorial/tutorial_general_301.png)

3. Değiştirmeniz gerekiyorsa **SAML** herhangi bir başka bir modda modundan tıklayın **oturum açma tek Mod Değiştir** ekranın en üstünde.

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-three-tutorial/tutorial_general_300.png)

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-three-tutorial/tutorial_general_302.png)

5. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-three-tutorial/tutorial_zscalerthree_url.png)

    Yanıt URL'si metin kutusuna URL'yi girin: `https://login.zscalerthree.net/sfc_sso`

    > [!NOTE]
    > Bu değer gerçek oturum açma URL'si ile güncelleştirmeniz gerekiyor. İlgili kişi [Zscaler üç istemci Destek ekibine](https://www.zscaler.com/company/contact) bu değerleri almak için.

6. Üzerinde **SAML imzalama sertifikası** bölümü tıklatın üzerinde **indirme** indirmek için **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-three-tutorial/tutorial_zscalerthree_certificate.png)

8. Üzerinde **Zscaler üç kümesi** bölümünde, kopya **oturum açma URL'si**.

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-three-tutorial/tutorial_zscalerthree_configure.png)

9. Farklı bir web tarayıcı penceresinde Zscaler üç şirketinizin sitesi için bir yönetici olarak oturum açın.

10. Üstteki menüden **Yönetim**.

    ![Yönetim](./media/zscaler-three-tutorial/ic800206.png "Yönetim")

9. Altında **yönetmesine & rolleri**, tıklayın **Kullanıcıları Yönet & kimlik doğrulaması**.

    ![Kullanıcı ve kimlik doğrulaması yönetmek](./media/zscaler-three-tutorial/ic800207.png "kullanıcı ve kimlik doğrulaması'nı yönetme")

10. İçinde **seçin, kuruluşunuz için kimlik doğrulama seçenekleri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Kimlik doğrulaması](./media/zscaler-three-tutorial/ic800208.png "kimlik doğrulaması")

    a. Seçin **SAML çoklu oturum açma kullanarak kimlik doğrulaması**.

    b. Tıklayın **oturum açma SAML tek parametrelerini yapılandırma**.

11. Üzerinde **SAML çoklu oturum açma parametrelerini yapılandırma** iletişim sayfasında, aşağıdaki adımları uygulayın ve ardından **bitti**

    ![Çoklu oturum açma](./media/zscaler-three-tutorial/ic800209.png "çoklu oturum açma")

    a. Yapıştırma **oturum açma URL'si** Azure portaldan kopyaladığınız değeri **olduğu kullanıcılar için kimlik doğrulaması gönderilir SAML portalın URL'sini** metin.

    b. İçinde **öznitelik oturum açma adını içeren** metin kutusuna **Nameıd**.

    c. İndirilen sertifikanızı karşıya yüklemek için tıklayın **Zscaler pem**.

    d. Seçin **SAML otomatik sağlamayı etkinleştirme**.

12. Üzerinde **kullanıcı kimlik doğrulamasını yapılandırma** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Yönetim](./media/zscaler-three-tutorial/ic800210.png "Yönetim")

    a. **Kaydet**’e tıklayın.

    b. Tıklayın **etkinleştirelim**.

## <a name="configuring-proxy-settings"></a>Ara sunucu ayarlarını yapılandırma

### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Internet Explorer'ın proxy ayarlarını yapılandırmak için

1. Başlangıç **Internet Explorer**.

2. Seçin **Internet Seçenekleri** gelen **Araçları** menüsünü aç **Internet Seçenekleri** iletişim.

     ![Internet Seçenekleri](./media/zscaler-three-tutorial/ic769492.png "Internet Seçenekleri")

3. Tıklayın **bağlantıları** sekmesi.
  
     ![Bağlantıları](./media/zscaler-three-tutorial/ic769493.png "bağlantıları")

4. Tıklayın **LAN Ayarları** açmak için **LAN Ayarları** iletişim.

5. Proxy sunucusu bölümünde aşağıdaki adımları gerçekleştirin:

    ![Proxy sunucusu](./media/zscaler-three-tutorial/ic769494.png "Proxy sunucusu")

    a. Seçin **AĞINIZ için bir proxy sunucusu kullan**.

    b. Adresi metin kutusuna yazın **gateway.zscalerthree.net**.

    c. Bağlantı noktası metin kutusuna yazın **80**.

    d. Seçin **yerel adresler için proxy sunucuyu atla**.

    e. Tıklayın **Tamam** kapatmak için **yerel alan ağı (LAN) ayarları** iletişim.

6. Tıklayın **Tamam** kapatmak için **Internet Seçenekleri** iletişim.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![Azure AD kullanıcısı oluşturun][100]

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-three-tutorial/create_aaduser_01.png) 

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscaler-three-tutorial/create_aaduser_02.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **özellikleri**seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="creating-a-zscaler-three-test-user"></a>Zscaler üç bir test kullanıcısı oluşturma

Zscaler üç için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar için üç Zscaler sağlanması gerekir. Zscaler üç söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Oturum açın, **Zscaler üç** Kiracı.

2. Tıklayın **Yönetim**.

    ![Yönetim](./media/zscaler-three-tutorial/ic781035.png "Yönetim")

3. Tıklayın **kullanıcı yönetimi**.

     ![Ekleme](./media/zscaler-three-tutorial/ic781036.png "Ekle")

4. İçinde **kullanıcılar** sekmesinde **Ekle**.

    ![Ekleme](./media/zscaler-three-tutorial/ic781037.png "Ekle")

5. Kullanıcı Ekle bölümünde aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı ekleme](./media/zscaler-three-tutorial/ic781038.png "kullanıcı ekleme")

    a. Tür **UserID**, **kullanıcı görünen adı**, **parola**, **parolayı onayla**ve ardından **grupları**ve **departmanı** geçerli bir Azure AD hesabı sağlamak istediğiniz.

    b. **Kaydet**’e tıklayın.

> [!NOTE]
> Azure AD kullanıcı hesapları sağlamak için herhangi bir Zscaler üç kullanıcı hesabı oluşturma araçları veya üç Zscaler tarafından sağlanan API'leri kullanabilirsiniz.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, erişim için üç Zscaler vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **Zscaler üç**.

    ![Çoklu oturum açmayı yapılandırın](./media/zscaler-three-tutorial/tutorial_zscalerthree_app.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202]

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Zscaler üç kutucuğa tıkladığınızda, otomatik olarak Zscaler üç uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/zscaler-three-tutorial/tutorial_general_01.png
[2]: ./media/zscaler-three-tutorial/tutorial_general_02.png
[3]: ./media/zscaler-three-tutorial/tutorial_general_03.png
[4]: ./media/zscaler-three-tutorial/tutorial_general_04.png

[100]: ./media/zscaler-three-tutorial/tutorial_general_100.png

[200]: ./media/zscaler-three-tutorial/tutorial_general_200.png
[201]: ./media/zscaler-three-tutorial/tutorial_general_201.png
[202]: ./media/zscaler-three-tutorial/tutorial_general_202.png
[203]: ./media/zscaler-three-tutorial/tutorial_general_203.png