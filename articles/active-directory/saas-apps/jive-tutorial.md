---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Jive | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Jive arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 9fc5659a-c116-4a1b-a601-333325a26b46
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: jeedes
ms.openlocfilehash: 00f255a33e9bfbec97d43bbede6116b14a096553
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36209876"
---
# <a name="tutorial-azure-active-directory-integration-with-jive"></a>Öğretici: Azure Active Directory Tümleştirme Jive ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Jive tümleştirmek öğrenin.

Jive Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Jive erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Jive (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Jive ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Jive çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin.
Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Jive ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-jive-from-the-gallery"></a>Galeriden Jive ekleme
Azure AD'ye Jive tümleştirmesini yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Jive eklemeniz gerekir.

**Galeriden Jive eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Jive**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/jive-tutorial/tutorial_jive_search.png)

5. Sonuçlar panelinde seçin **Jive**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/jive-tutorial/tutorial_jive_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı Jive ile test etme

Tekli çalışmaya oturum için Azure AD Jive karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Jive ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** Jive içinde.

Yapılandırma ve Azure AD çoklu oturum açma Jive ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Jive test kullanıcısı oluşturma](#creating-a-jive-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Jive sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Jive uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Jive yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Jive** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/jive-tutorial/tutorial_jive_samlbase.png)

3. Üzerinde **Jive etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/jive-tutorial/tutorial_jive_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<instance name>.jivecustom.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<instance name>.jiveon.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri tanımlayıcısı ve gerçek oturum açma URL'si ile güncelleştirin. Kişi [Jive istemci destek ekibi](https://www.jivesoftware.com/services-support/) bu değerleri almak için.

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve XML dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/jive-tutorial/tutorial_jive_certificate.png)

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/jive-tutorial/tutorial_general_400.png)

6. Çoklu oturum açma yapılandırmak için **Jive** tarafında, oturum açma Jive Kiracı yönetici olarak.

7. Üstteki menüde tıklayın "**Saml**."

    ![Çoklu oturum açma uygulama tarafında yapılandırma](./media/jive-tutorial/tutorial_jive_002.png)

    a. Seçin **etkin** altında **genel** sekmesini b. Tıklayın "**tüm saml Ayarları Kaydet**" düğmesi.

8. Gidin "**IDP meta veri**" sekmesi.

    ![Çoklu oturum açma uygulama tarafında yapılandırma](./media/jive-tutorial/tutorial_jive_003.png)

    a. İndirilen meta veri XML dosyasının içeriğini kopyalayın ve ardından yapıştırın **kimlik sağlayıcısı (IDP) meta veri** metin kutusu.

    b. Tıklayın "**tüm saml Ayarları Kaydet**" düğmesi.

9. Git "**kullanıcı özniteliği eşleme**" sekmesi.

    ![Çoklu oturum açma uygulama tarafında yapılandırma](./media/jive-tutorial/tutorial_jive_004.png)

    a. İçinde **e-posta** metin kutusuna, öznitelik adı kopyalayıp **posta** değeri.

    b. İçinde **ad** metin kutusuna, öznitelik adı kopyalayıp **givenname** değeri.

    c. İçinde **Soyadı** metin kutusuna, öznitelik adı kopyalayıp **Soyadı** değeri.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/jive-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/jive-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/jive-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Bir Azure AD test kullanıcısı oluşturma](./media/jive-tutorial/create_aaduser_04.png)

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.

### <a name="creating-a-jive-test-user"></a>Jive test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon içinde Jive adlı bir kullanıcı oluşturmaktır. Jive otomatik kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler. Daha fazla ayrıntı bulabilirsiniz [burada](jive-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

Kullanıcı el ile oluşturmanız gerekiyorsa, çalışmak [Jive istemci destek ekibi](https://www.jivesoftware.com/services-support/) Jive platform kullanıcıları eklemek için.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Jive için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Jive için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Jive**.

    ![Çoklu oturum açmayı yapılandırın](./media/jive-tutorial/tutorial_jive_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Jive parçasında tıklattığınızda, otomatik olarak Jive uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)
* [Kullanıcı sağlamayı Yapılandır](jive-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/jive-tutorial/tutorial_general_01.png
[2]: ./media/jive-tutorial/tutorial_general_02.png
[3]: ./media/jive-tutorial/tutorial_general_03.png
[4]: ./media/jive-tutorial/tutorial_general_04.png

[100]: ./media/jive-tutorial/tutorial_general_100.png

[200]: ./media/jive-tutorial/tutorial_general_200.png
[201]: ./media/jive-tutorial/tutorial_general_201.png
[202]: ./media/jive-tutorial/tutorial_general_202.png
[203]: ./media/jive-tutorial/tutorial_general_203.png
