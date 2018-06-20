---
title: 'Öğretici: Azure Active Directory Tümleştirme ile AppBlade | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile AppBlade arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 3360d7aa-6518-4f99-88bd-b7f7258183e8
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 66c893a89138d7daf7d8118d8e2b1d8389d40ea4
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36229307"
---
# <a name="tutorial-azure-active-directory-integration-with-appblade"></a>Öğretici: Azure Active Directory Tümleştirme AppBlade ile

Bu öğreticide, Azure Active Directory (Azure AD) ile AppBlade tümleştirmek öğrenin.

AppBlade Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- AppBlade erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için AppBlade (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme AppBlade ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir AppBlade çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden AppBlade ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-appblade-from-the-gallery"></a>Galeriden AppBlade ekleme
Azure AD AppBlade tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden AppBlade eklemeniz gerekir.

**Galeriden AppBlade eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **AppBlade**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/appblade-tutorial/tutorial_appblade_search.png)

5. Sonuçlar panelinde seçin **AppBlade**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/appblade-tutorial/tutorial_appblade_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı AppBlade ile test etme

Tekli çalışmaya oturum için Azure AD AppBlade karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının AppBlade ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

AppBlade içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma AppBlade ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir AppBlade test kullanıcısı oluşturma](#creating-an-appblade-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı AppBlade sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma AppBlade uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile AppBlade yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **AppBlade** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/appblade-tutorial/tutorial_appblade_samlbase.png)

3. Üzerinde **AppBlade etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/appblade-tutorial/tutorial_appblade_url.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<companyname>.appblade.com/saml/<tenantid>`

    > [!NOTE] 
    > Bu değer gerçek değil. Değerin gerçek oturum açma URL'si ile güncelleştirin. Kişi [AppBlade istemci destek ekibi](mailto:support@appblade.com) değeri alınamıyor. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/appblade-tutorial/tutorial_appblade_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/appblade-tutorial/tutorial_general_400.png)

6. Çoklu oturum açma yapılandırmak için **AppBlade** yan, indirilen göndermek için ihtiyacınız **meta veri XML** için [AppBlade destek ekibi](mailto:support@appblade.com). Ayrıca, lütfen yapılandırma isteyin **SSO veren URL'si** olarak `https://appblade.com/saml`. Bu ayar tek çalışmaya oturum için gereklidir.


> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
 
### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/appblade-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/appblade-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/appblade-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/appblade-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-an-appblade-test-user"></a>Bir AppBlade test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon içinde AppBlade adlı bir kullanıcı oluşturmaktır. AppBlade yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler. **Kullanıcı sağlama için etki alanı adınızı AppBlade ile yapılandırıldığından emin olun. Yalnızca tam zamanı kullanıcı sonra works sağlama.**

Kullanıcının kullanıcı otomatik olarak belirttiğiniz izin düzeyine sahip bir üye olarak hesaba katılacağı sonra AppBlade tarafından hesabınız için yapılandırılan etki alanı ile biten bir e-posta adresi varsa, "Temel" (yalnızca uygulamaları yükleyebilir temel bir kullanıcı), "Ekip üyesine" (yeni uygulama sürümleri karşıya ve projeleri yönetme bir kullanıcı) veya "Yönetici" (hesap tam yönetim ayrıcalıklarını) biridir. Normalde bir temel seçin ve sonra kullanıcıların bir yönetici oturum açma (ya da, bir yönetici e-posta tabanlı oturum açma önceden yapılandırmak veya bir kullanıcı oturum açtıktan sonra müşteri adına yükseltmek için AppBlade gereksinimlerini) kullanarak el ile yükseltmeniz.

Bu bölümde, eylem öğe yok. Yeni bir kullanıcı henüz yoksa AppBlade erişme denemesi sırasında oluşturulur. 

> [!NOTE]
> Bir kullanıcı el ile oluşturmanız gerekiyorsa, başvurmanız gerekir [AppBlade destek ekibi](mailto:support@appblade.com).

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta AppBlade için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**AppBlade için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **AppBlade**.

    ![Çoklu oturum açmayı yapılandırın](./media/appblade-tutorial/tutorial_appblade_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.  
Erişim paneli AppBlade parçasında tıklattığınızda, otomatik olarak AppBlade uygulamanıza açan. 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/appblade-tutorial/tutorial_general_01.png
[2]: ./media/appblade-tutorial/tutorial_general_02.png
[3]: ./media/appblade-tutorial/tutorial_general_03.png
[4]: ./media/appblade-tutorial/tutorial_general_04.png

[100]: ./media/appblade-tutorial/tutorial_general_100.png

[200]: ./media/appblade-tutorial/tutorial_general_200.png
[201]: ./media/appblade-tutorial/tutorial_general_201.png
[202]: ./media/appblade-tutorial/tutorial_general_202.png
[203]: ./media/appblade-tutorial/tutorial_general_203.png

