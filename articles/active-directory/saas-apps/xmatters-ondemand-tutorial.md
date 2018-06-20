---
title: 'Öğretici: Azure Active Directory Tümleştirme ile xMatters OnDemand | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile xMatters OnDemand arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: ca0633db-4f95-432e-b3db-0168193b5ce9
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: 9a3555f9efa34a5da2e6fa624da0f80d52dab077
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36211644"
---
# <a name="tutorial-azure-active-directory-integration-with-xmatters-ondemand"></a>Öğretici: Azure Active Directory Tümleştirme xMatters OnDemand ile

Bu öğreticide, Azure Active Directory (Azure AD) ile xMatters OnDemand tümleştirmek öğrenin.

XMatters OnDemand Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- XMatters OnDemand erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak Azure AD hesaplarına sahip xMatters OnDemand (çoklu oturum açma) için açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme OnDemand xMatters ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Abonelik xMatters OnDemand çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden xMatters OnDemand ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-xmatters-ondemand-from-the-gallery"></a>Galeriden xMatters OnDemand ekleme
Azure AD xMatters OnDemand tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden xMatters OnDemand eklemeniz gerekir.

**Galeriden xMatters OnDemand eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **xMatters OnDemand**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/xmatters-ondemand-tutorial/tutorial_xmattersondemand_search.png)

5. Sonuçlar panelinde seçin **xMatters OnDemand**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/xmatters-ondemand-tutorial/tutorial_xmattersondemand_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı OnDemand tabanlı xMatters ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen kullanıcı xMatters OnDemand Azure AD'de bir kullanıcı için olduğunu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının xMatters ilgili kullanıcı arasında bir bağlantı ilişkisi OnDemand kurulması gerekir.

Değeri xMatters OnDemand'da, Ata **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma xMatters OnDemand ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[XMatters OnDemand test kullanıcısı oluşturma](#creating-a-xmatters-ondemand-test-user)**  - bir Britta Simon karşılık gelen xMatters kullanıcı Azure AD gösterimini bağlı OnDemand içinde olması.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma xMatters OnDemand uygulama yapılandırın.

**Azure AD çoklu oturum açma OnDemand xMatters ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **xMatters OnDemand** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/xmatters-ondemand-tutorial/tutorial_xmattersondemand_samlbase.png)

3. Üzerinde **xMatters OnDemand etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/xmatters-ondemand-tutorial/tutorial_xmattersondemand_url.png)
    
    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:
    | |
    |--|
    | `https://<companyname>.au1.xmatters.com.au/`|
    | `https://<companyname>.cs1.xmatters.com/`|
    | `https://<companyname>.xmatters.com/`|
    | `https://www.xmatters.com`|
    | `https://<companyname>.xmatters.com.au/`|

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:
    | |
    |--|
    | `https://<companyname>.au1.xmatters.com.au`|
    | `https://<companyname>.xmatters.com/sp/<instancename>`|
    | `https://<companyname>.cs1.xmatters.com/sp/<instancename>`|
    | `https://<companyname>.au1.xmatters.com.au/<instancename>`|

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. Kişi [xMatters OnDemand destek ekibi](https://www.xmatters.com/company/contact-us/) bu değerleri almak için.

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyasını yerel olarak kaydedin **c:\\XMatters OnDemand.cer**.

    ![Çoklu oturum açmayı yapılandırın](./media/xmatters-ondemand-tutorial/tutorial_xmattersondemand_certificate.png)

    > [!IMPORTANT]
    > Sertifikayı iletmek gereken [xMatters OnDemand destek ekibi](https://www.xmatters.com/company/contact-us/). Tek oturum açma yapılandırmasını son haline getirmek önce xMatters destek ekibi tarafından karşıya yüklenecek sertifika gerekir. 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/xmatters-ondemand-tutorial/tutorial_general_400.png)

6. Üzerinde **xMatters OnDemand yapılandırma** 'yi tıklatın **xMatters OnDemand yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/xmatters-ondemand-tutorial/tutorial_xmattersondemand_configure.png) 

7. Farklı web tarayıcısı penceresinde XMatters OnDemand şirket sitenize yönetici olarak oturum açın.

8. Üstteki araç çubuğunda tıklatın **yönetici**ve ardından **şirket ayrıntıları** sol taraftaki gezinti çubuğunda.

    ![Yönetici](./media/xmatters-ondemand-tutorial/IC776795.png "yönetici")

9. Üzerinde **SAML Yapılandırması** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![SAML Yapılandırması](./media/xmatters-ondemand-tutorial/IC776796.png "SAML yapılandırması")

    a. Seçin **etkinleştirmek SAML**.

    b. İçinde **kimlik sağlayıcı kimliği** metin kutusuna, Yapıştır **SAML varlık kimliği** Azure portalından kopyaladığınız değeri.

    c. İçinde **üzerinde tek oturum URL'si** metin kutusuna, Yapıştır **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyaladığınız değeri.

    d. İçinde **çoklu oturum kapatma URL'si** metin kutusuna, Yapıştır **Sign-Out URL**, Azure portalından kopyalanan.

    e. Şirket Ayrıntıları sayfası, en üstte tıklayın **Değişiklikleri Kaydet**.

    ![Şirket ayrıntıları](./media/xmatters-ondemand-tutorial/IC776797.png "şirket ayrıntıları")

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/xmatters-ondemand-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/xmatters-ondemand-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/xmatters-ondemand-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Bir Azure AD test kullanıcısı oluşturma](./media/xmatters-ondemand-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.

### <a name="creating-a-xmatters-ondemand-test-user"></a>XMatters OnDemand test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon xMatters OnDemand adlı bir kullanıcı oluşturmaktır. xMatters OnDemand otomatik kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler. Daha fazla ayrıntı bulabilirsiniz [burada](xmatters-ondemand-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

**Kullanıcı el ile oluşturmanız gerekiyorsa, şu adımları gerçekleştirin:**

1. Oturum, **XMatters OnDemand** Kiracı.

2.  Tıklatın **kullanıcılar** sekmesini ve ardından **Kullanıcı Ekle**.

    ![Kullanıcıların](./media/xmatters-ondemand-tutorial/IC781048.png "kullanıcılar")

3. İçinde **kullanıcı ekleme** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı ekleme](./media/xmatters-ondemand-tutorial/IC781049.png "kullanıcı ekleme")

    a. Seçin **etkin**.

    b. İçinde **kullanıcı kimliği** metin kutusuna, kullanıcının kullanıcı kimliği türü ister Brittasimon@contoso.com.

    c. İçinde **ad** metin kutusuna, adı Britta gibi kullanıcı türü.

    d. İçinde **Soyadı** metin kutusuna, türü kullanıcının soyadını Simon gibi.

    e. İçinde **Site** metin kutusuna, Enter geçerli Azure geçerli site için sağlamak istediğiniz AD hesabı.

    f. **Kaydet**’e tıklayın.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta xMatters OnDemand erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**XMatters OnDemand Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **xMatters OnDemand**.

    ![Çoklu oturum açmayı yapılandırın](./media/xmatters-ondemand-tutorial/tutorial_xmattersondemand_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli OnDemand parçasında xMatters tıklattığınızda, otomatik olarak, xMatters OnDemand uygulama açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)
* [Kullanıcı sağlamayı Yapılandır](xmatters-ondemand-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/xmatters-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/xmatters-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/xmatters-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/xmatters-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/xmatters-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/xmatters-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/xmatters-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/xmatters-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/xmatters-ondemand-tutorial/tutorial_general_203.png
