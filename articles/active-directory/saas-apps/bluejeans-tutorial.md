---
title: 'Öğretici: Azure Active Directory Tümleştirme ile BlueJeans | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile BlueJeans arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: dfc634fd-1b55-4ba8-94a8-b8288429b6a9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: jeedes
ms.openlocfilehash: e6addaeda5f7873a95386baf40ff4cd0c9726f1a
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35982175"
---
# <a name="tutorial-azure-active-directory-integration-with-bluejeans"></a>Öğretici: Azure Active Directory Tümleştirme BlueJeans ile

Bu öğreticide, Azure Active Directory (Azure AD) ile BlueJeans tümleştirmek öğrenin.

BlueJeans Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- BlueJeans erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için BlueJeans (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme BlueJeans ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir BlueJeans çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden BlueJeans ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-bluejeans-from-the-gallery"></a>Galeriden BlueJeans ekleme
Azure AD BlueJeans tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden BlueJeans eklemeniz gerekir.

**Galeriden BlueJeans eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi.

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **BlueJeans**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/bluejeans-tutorial/tutorial_bluejeans_search.png)

5. Sonuçlar panelinde seçin **BlueJeans**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/bluejeans-tutorial/tutorial_bluejeans_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı BlueJeans ile test etme

Tekli çalışmaya oturum için Azure AD BlueJeans karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının BlueJeans ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

BlueJeans içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma BlueJeans ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[BlueJeans test kullanıcısı oluşturma](#creating-a-bluejeans-test-user)**  - Britta Simon BlueJeans içinde kullanıcı Azure AD gösterimini bağlı, karşılık gelen sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma BlueJeans uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile BlueJeans yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **BlueJeans** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/bluejeans-tutorial/tutorial_bluejeans_samlbase.png)

3. Üzerinde **BlueJeans etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/bluejeans-tutorial/tutorial_bluejeans_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<companyname>.BlueJeans.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<companyname>.BlueJeans.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [BlueJeans istemci destek ekibi](https://support.bluejeans.com/contact) bu değerleri almak için.

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/bluejeans-tutorial/tutorial_bluejeans_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/bluejeans-tutorial/tutorial_general_400.png)

6. Üzerinde **BlueJeans yapılandırma** 'yi tıklatın **yapılandırma BlueJeans** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, değişiklik parola URL'si ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/bluejeans-tutorial/tutorial_bluejeans_configure.png) 

7. Farklı web tarayıcısı penceresinde oturum açın, **BlueJeans** yönetici olarak şirket site.

8. Git **yönetici \> grup ayarları \> güvenlik**.

   ![Yönetici](./media/bluejeans-tutorial/IC785868.png "yönetici")

9. İçinde **güvenlik** bölümünde, aşağıdaki adımları gerçekleştirin:

   ![SAML çoklu oturum açma](./media/bluejeans-tutorial/IC785869.png "SAML çoklu oturum açma")

   a. Seçin **SAML çoklu oturum açma**.

   b. Seçin **otomatik sağlamayı etkinleştirmek**.

10. Aşağıdaki adımlarla geçin:

    ![Sertifika yolu](./media/bluejeans-tutorial/IC785870.png "sertifika yolu")

    a. Tıklatın **Dosya Seç**ve indirilen sertifika yükleyin.

    b. Yapıştır **SAML çoklu oturum açma hizmet URL'si** içine **oturum açma URL'si** metin kutusu.

    c. Yapıştır **değişiklik parola URL'si** içine **parola değişikliği URL'si** metin kutusu.

    d. Yapıştır **Sign-Out URL** içine **oturum kapatma URL'si** metin kutusu.

11. Aşağıdaki adımlarla geçin:

    ![Değişiklikleri kaydetmek](./media/bluejeans-tutorial/IC785874.png "Değişiklikleri Kaydet")

    a. İçinde **kullanıcı kimliği** metin kutusuna, türü `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    b. İçinde **e-posta** metin kutusuna, türü `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    c. Tıklatın **değişiklikleri kaydetmek**.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/bluejeans-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/bluejeans-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/bluejeans-tutorial/create_aaduser_03.png)

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Bir Azure AD test kullanıcısı oluşturma](./media/bluejeans-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.

### <a name="creating-a-bluejeans-test-user"></a>BlueJeans test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon içinde BlueJeans adlı bir kullanıcı oluşturmaktır. BlueJeans destekler otomatik kullanıcı hazırlama, varsayılan olarak etkin olduğu. Daha fazla ayrıntı bulabilirsiniz [burada](bluejeans-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

**Kullanıcı el ile oluşturmanız gerekiyorsa, şu adımları gerçekleştirin:**

1. Oturum, **BlueJeans** yönetici olarak şirket site.

2. Git **yönetici \> kullanıcıları yönetme \> kullanıcı ekleme**.

   ![Yönetici](./media/bluejeans-tutorial/IC785877.png "yönetici")

   >[!IMPORTANT]
   >**Kullanıcı Ekle** sekmesi kullanılabilir yalnızca IF, içinde **Güvenlik sekmesinde**, **otomatik sağlamayı etkinleştirmek** işaretli değildir. 

3. İçinde **Kullanıcı Ekle** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı ekleme](./media/bluejeans-tutorial/IC785886.png "kullanıcı ekleme")

    a. Tür a **BlueJeans kullanıcı adı**, bir **e-posta adresi**, **BlueJeans toplantı kimliği**, **yönetici parolası**, **tam adı** , **şirket** istediğiniz ilgili metin kutularına sağlamayı geçerli bir AAD hesabının.

    b. Tıklatın **kullanıcı ekleme**.

>[!NOTE]
>API sağlama AAD kullanıcı hesaplarına BlueJeans tarafından sağlanan veya herhangi diğer BlueJeans kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta BlueJeans için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200]

**BlueJeans için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201]

2. Uygulamalar listesinde **BlueJeans**.

    ![Çoklu oturum açmayı yapılandırın](./media/bluejeans-tutorial/tutorial_bluejeans_app.png)

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.

### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli BlueJeans parçasında tıkladığınızda, oturum açma sayfasına BlueJeans uygulamasının almanız gerekir.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)
* [Kullanıcı sağlamayı Yapılandır](bluejeans-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/bluejeans-tutorial/tutorial_general_01.png
[2]: ./media/bluejeans-tutorial/tutorial_general_02.png
[3]: ./media/bluejeans-tutorial/tutorial_general_03.png
[4]: ./media/bluejeans-tutorial/tutorial_general_04.png

[100]: ./media/bluejeans-tutorial/tutorial_general_100.png

[200]: ./media/bluejeans-tutorial/tutorial_general_200.png
[201]: ./media/bluejeans-tutorial/tutorial_general_201.png
[202]: ./media/bluejeans-tutorial/tutorial_general_202.png
[203]: ./media/bluejeans-tutorial/tutorial_general_203.png
