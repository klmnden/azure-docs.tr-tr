---
title: 'Öğretici: Azure Active Directory Tümleştirme ile dönüm OnDemand | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory dönüm OnDemand arasındaki yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f57c5fef-49b0-4591-91ef-fc0de6d654ab
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: jeedes
ms.openlocfilehash: 79ae29e8d922112ca5d351f331b841b74cec4cdd
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35971955"
---
# <a name="tutorial-azure-active-directory-integration-with-cornerstone-ondemand"></a>Öğretici: Azure Active Directory Tümleştirme dönüm OnDemand ile

Bu öğreticide, Azure Active Directory (Azure AD) ile dönüm OnDemand tümleştirmek öğrenin.

Dönüm OnDemand Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Dönüm OnDemand erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için dönüm OnDemand açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme dönüm OnDemand ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir dönüm OnDemand çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden dönüm OnDemand ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-cornerstone-ondemand-from-the-gallery"></a>Galeriden dönüm OnDemand ekleme
Azure AD dönüm OnDemand tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden dönüm OnDemand eklemeniz gerekir.

**Galeriden dönüm OnDemand eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **dönüm OnDemand**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_search.png)

5. Sonuçlar panelinde seçin **dönüm OnDemand**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma dönüm "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı OnDemand ile test etme

Tekli çalışmaya oturum için Azure AD dönüm OnDemand karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının dönüm OnDemand ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değeri dönüm OnDemand içinde atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma dönüm OnDemand ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Dönüm OnDemand test kullanıcısı oluşturma](#creating-a-cornerstone-ondemand-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı dönüm OnDemand sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma dönüm OnDemand uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile dönüm OnDemand yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **dönüm OnDemand** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_samlbase.png)

3. Üzerinde **dönüm OnDemand etki alanı ve URL'leri** bölümünde, aşağıdaki adımı gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<company>.csod.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<company>.csod.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [dönüm OnDemand istemci destek ekibi](mailTo:moreinfo@csod.com) bu değerleri almak için.

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/cornerstone-ondemand-tutorial/tutorial_general_400.png)

6. Üzerinde **dönüm OnDemand yapılandırma** 'yi tıklatın **yapılandırma dönüm OnDemand** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_configure.png) 

7. Çoklu oturum açma yapılandırmak için **dönüm OnDemand** yan, indirilen göndermek için ihtiyacınız **sertifika**, **Sign-Out URL** ve **SAML çoklu oturum açma hizmet URL'si** için [dönüm OnDemand destek ekibi](mailTo:moreinfo@csod.com). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cornerstone-ondemand-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cornerstone-ondemand-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cornerstone-ondemand-tutorial/create_aaduser_03.png)

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cornerstone-ondemand-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.

### <a name="creating-a-cornerstone-ondemand-test-user"></a>Dönüm OnDemand test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon dönüm OnDemand adlı bir kullanıcı oluşturmaktır. Dönüm OnDemand otomatik kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler. Daha fazla ayrıntı bulabilirsiniz [burada](cornerstone-ondemand-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

**Kullanıcı el ile oluşturmanız gerekiyorsa, şu adımları gerçekleştirin:**

Kullanıcı sağlamayı yapılandırmak için bilgi gönder (örn: ad, e-posta) Azure AD kullanıcısının hakkında sağlamak istediğiniz [dönüm OnDemand destek ekibi](mailTo:moreinfo@csod.com).

>[!NOTE]
>API'leri sağlama AAD kullanıcı hesaplarına dönüm OnDemand tarafından sağlanan veya herhangi başka dönüm OnDemand kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta dönüm OnDemand erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Britta Simon dönüm OnDemand atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **dönüm OnDemand**.

    ![Çoklu oturum açmayı yapılandırın](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli dönüm OnDemand parçasında tıklattığınızda, otomatik olarak dönüm OnDemand uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)
* [Kullanıcı sağlamayı Yapılandır](cornerstone-ondemand-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/cornerstone-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/cornerstone-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/cornerstone-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/cornerstone-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/cornerstone-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/cornerstone-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/cornerstone-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/cornerstone-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/cornerstone-ondemand-tutorial/tutorial_general_203.png
