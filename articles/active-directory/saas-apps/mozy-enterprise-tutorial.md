---
title: 'Öğretici: Mozy Enterprise ile Azure Active Directory Tümleştirmesi | Microsoft Docs'
description: Azure Active Directory ve Mozy kuruluş arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 489b5e62-85c2-45c9-8766-326632d48114
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: jeedes
ms.openlocfilehash: 5bbaa90554e09d27a3c521d4a13eda44021721c8
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55176905"
---
# <a name="tutorial-azure-active-directory-integration-with-mozy-enterprise"></a>Öğretici: Mozy Enterprise ile Azure Active Directory Tümleştirmesi

Bu öğreticide, Azure Active Directory (Azure AD) ile Mozy Kurumsal tümleştirme konusunda bilgi edinin.

Azure AD ile Mozy Kurumsal tümleştirme ile aşağıdaki avantajları sağlar:

- Mozy Kurumsal erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan Mozy kuruluş (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Kurumsal Mozy yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Mozy Kurumsal çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Mozy Kurumsal ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-mozy-enterprise-from-the-gallery"></a>Galeriden Mozy Kurumsal ekleme
Azure AD'de Mozy Kurumsal tümleştirmesini yapılandırmak için Mozy Kurumsal Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Mozy kuruluş eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Mozy Kurumsal**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/mozy-enterprise-tutorial/tutorial_mozyenterprise_search.png)

1. Sonuçlar panelinde seçin **Mozy Kurumsal**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/mozy-enterprise-tutorial/tutorial_mozyenterprise_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırmanız ve Mozy Enterprise ile Azure AD çoklu oturum açmayı test "Britta Simon" adlı bir test kullanıcı tabanlı.

Tek çalışmak için oturum açma için Azure AD ne karşılık gelen kullanıcı Mozy kurumsal bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı Mozy kuruluştaki arasında bir bağlantı ilişki kurulması gerekir.

Mozy kuruluşta değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Mozy Enterprise ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Mozy Kurumsal test kullanıcısı oluşturma](#creating-a-mozy-enterprise-test-user)**  - Mozy kuruluştaki kullanıcı Azure AD gösterimini bağlı Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Mozy Kurumsal uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Mozy Enterprise ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Mozy Kurumsal** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/mozy-enterprise-tutorial/tutorial_mozyenterprise_samlbase.png)

1. Üzerinde **Mozy Kurumsal etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/mozy-enterprise-tutorial/tutorial_mozyenterprise_url.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<tenantname>.Mozyenterprise.com`

    > [!NOTE] 
    > Bu değer, gerçek değil. Bu değer, gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Mozy Kurumsal İstemci Destek ekibine](http://support.mozy.com/) bu değeri alınamıyor.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/mozy-enterprise-tutorial/tutorial_mozyenterprise_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/mozy-enterprise-tutorial/tutorial_general_400.png)

1. Üzerinde **Mozy Kurumsal yapılandırma** bölümünde **Mozy Kurumsal yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/mozy-enterprise-tutorial/tutorial_mozyenterprise_configure.png) 

1. Farklı bir web tarayıcı penceresinde Mozy Kuruluş şirket sitenize yönetici olarak oturum.

1. İçinde **yapılandırma** bölümünde **kimlik doğrulama İlkesi**.
   
   ![Kimlik doğrulama İlkesi](./media/mozy-enterprise-tutorial/ic777314.png "kimlik doğrulama İlkesi")

1. Üzerinde **kimlik doğrulama İlkesi** bölümünde, aşağıdaki adımları gerçekleştirin:
   
   ![Kimlik doğrulama İlkesi](./media/mozy-enterprise-tutorial/ic777315.png "kimlik doğrulama İlkesi")
   
   a. Seçin **dizin hizmeti** olarak **sağlayıcısı**.
   
   b. Seçin **LDAP göndermeyi kullanmak**.
   
   c. Tıklayın **SAML kimlik doğrulaması** sekmesi.
   
   d. Yapıştırma **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure portaldan kopyaladığınız **kimlik doğrulaması URL'sini** metin.
   
   e. Yapıştırma **SAML varlık kimliği**, hangi Azure portaldan kopyaladığınız **SAML uç noktası** metin.
   
   f. İndirilen base-64 kodlanmış sertifika Not Defteri'nde açın, içeriğini, panoya kopyalayın ve tüm sertifika içine yapıştırın **SAML sertifikası** metin.
   
   g. Seçin **yöneticilerin, ağ kimlik bilgileriyle oturum SSO etkinleştirme**.
   
   h. Tıklayın **değişiklikleri kaydetmek**.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/mozy-enterprise-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/mozy-enterprise-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/mozy-enterprise-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/mozy-enterprise-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-mozy-enterprise-test-user"></a>Mozy Kurumsal test kullanıcısı oluşturma

Azure AD kullanıcılarının Mozy kuruma oturum etkinleştirmek için bunlar Mozy kuruma sağlanması gerekir. Mozy Enterprise söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

>[!NOTE]
>Herhangi diğer Mozy Kurumsal kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak Mozy kuruluş tarafından sağlanan.

**Bir kullanıcı hesapları sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **Mozy Kurumsal** Kiracı.

1. Tıklayın **kullanıcılar**ve ardından **yeni kullanıcı Ekle**.
   
   ![Kullanıcılar](./media/mozy-enterprise-tutorial/ic777317.png "kullanıcılar")
   
   >[!NOTE]
   >**Yeni kullanıcı Ekle** seçeneği yalnızca, yalnızca görüntülenen **Mozy** sağlayıcı altında olarak seçilen **kimlik doğrulama İlkesi**. SAML kimlik doğrulaması yapılandırılmışsa, ardından kullanıcıları otomatik olarak, ilk oturum açma aracılığıyla çoklu oturum açma üzerinde üzerinde eklenir.
    
1. Yeni kullanıcı iletişim kutusunda aşağıdaki adımları gerçekleştirin:
   
   ![Kullanıcı ekleme](./media/mozy-enterprise-tutorial/ic777318.png "kullanıcı ekleme")
   
   a. Gelen **bir grubu seçin** listesinde, bir grubu seçin.
   
   b. Gelen **kullanıcı türüne** listesinde, bir tür seçin.
   
   c. İçinde **kullanıcıadı** metin kutusuna, Azure AD kullanıcı adını yazın.
   
   d. İçinde **e-posta** metin kutusuna, Azure AD kullanıcısının e-posta adresini yazın.
   
   e. Seçin **kullanıcı yönerge e-posta Gönder**.
   
   f. Tıklayın **kullanıcıları ekleme**.

     >[!NOTE]
     > Kullanıcı oluşturduktan sonra bunu etkinleştirilmeden önce hesabı onaylamak için bir bağlantı içeren Azure AD kullanıcı bir e-posta gönderilir.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma için Mozy Kurumsal erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Mozy kuruluş atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Mozy Kurumsal**.

    ![Çoklu oturum açmayı yapılandırın](./media/mozy-enterprise-tutorial/tutorial_mozyenterprise_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Mozy Kurumsal kutucuğa tıkladığınızda, oturum açma sayfasını Mozy Kurumsal uygulama almanız gerekir.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/mozy-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/mozy-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/mozy-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/mozy-enterprise-tutorial/tutorial_general_04.png

[100]: ./media/mozy-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/mozy-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/mozy-enterprise-tutorial/tutorial_general_201.png
[202]: ./media/mozy-enterprise-tutorial/tutorial_general_202.png
[203]: ./media/mozy-enterprise-tutorial/tutorial_general_203.png

