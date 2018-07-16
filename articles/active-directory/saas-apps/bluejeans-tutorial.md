---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle BlueJeans | Microsoft Docs'
description: Azure Active Directory ve BlueJeans arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: dfc634fd-1b55-4ba8-94a8-b8288429b6a9
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: jeedes
ms.openlocfilehash: d485c16719c07062249e8d40f0feca9685851834
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39043474"
---
# <a name="tutorial-azure-active-directory-integration-with-bluejeans"></a>Öğretici: Azure Active Directory BlueJeans ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile BlueJeans tümleştirme konusunda bilgi edinin.

Azure AD ile BlueJeans tümleştirme ile aşağıdaki avantajları sağlar:

- BlueJeans erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için BlueJeans (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile BlueJeans yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir BlueJeans çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden BlueJeans ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-bluejeans-from-the-gallery"></a>Galeriden BlueJeans ekleme
Azure AD'de BlueJeans tümleştirmesini yapılandırmak için BlueJeans Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden BlueJeans eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **BlueJeans**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/bluejeans-tutorial/tutorial_bluejeans_search.png)

5. Sonuçlar panelinde seçin **BlueJeans**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/bluejeans-tutorial/tutorial_bluejeans_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı BlueJeans ile test etme

Tek iş için oturum açma için Azure AD ne BlueJeans karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının BlueJeans ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

BlueJeans içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma BlueJeans ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[BlueJeans test kullanıcısı oluşturma](#creating-a-bluejeans-test-user)**  - kullanıcı Azure AD gösterimini bağlı Britta simon'un BlueJeans içinde bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve BlueJeans uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile BlueJeans yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **BlueJeans** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/bluejeans-tutorial/tutorial_bluejeans_samlbase.png)

3. Üzerinde **BlueJeans etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/bluejeans-tutorial/tutorial_bluejeans_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.BlueJeans.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.BlueJeans.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [BlueJeans istemci Destek ekibine](https://support.bluejeans.com/contact) bu değerleri almak için.

4. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/bluejeans-tutorial/tutorial_bluejeans_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/bluejeans-tutorial/tutorial_general_400.png)

6. Üzerinde **BlueJeans yapılandırma** bölümünde **yapılandırma BlueJeans** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, parola URL'yi Değiştir ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/bluejeans-tutorial/tutorial_bluejeans_configure.png) 

7. Farklı bir web tarayıcı penceresinde oturum açın, **BlueJeans** yönetici olarak şirketin site.

8. Git **yönetici \> grup ayarları \> güvenlik**.

   ![Yönetici](./media/bluejeans-tutorial/IC785868.png "yönetici")

9. İçinde **güvenlik** bölümünde, aşağıdaki adımları gerçekleştirin:

   ![SAML çoklu oturum açma](./media/bluejeans-tutorial/IC785869.png "SAML çoklu oturum açma")

   a. Seçin **SAML çoklu oturum açma**.

   b. Seçin **otomatik sağlamayı etkinleştirme**.

10. Aşağıdaki adımlara geçin:

    ![Sertifika yolu](./media/bluejeans-tutorial/IC785870.png "sertifika yolu")

    a. Tıklayın **Dosya Seç**ve ardından indirilen sertifikayı karşıya yükleyin.

    b. Yapıştırma **SAML çoklu oturum açma hizmeti URL'si** içine **oturum açma URL'si** metin.

    c. Yapıştırma **parola URL'yi Değiştir** içine **parola değişikliği URL'si** metin.

    d. Yapıştırma **oturum kapatma URL'si** içine **oturum kapatma URL'si** metin.

11. Aşağıdaki adımlara geçin:

    ![Değişiklikleri kaydetmek](./media/bluejeans-tutorial/IC785874.png "Değişiklikleri Kaydet")

    a. İçinde **kullanıcı kimliği** metin kutusuna `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    b. İçinde **e-posta** metin kutusuna `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    c. Tıklayın **değişiklikleri kaydetmek**.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/bluejeans-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/bluejeans-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/bluejeans-tutorial/create_aaduser_03.png)

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Bir Azure AD test kullanıcısı oluşturma](./media/bluejeans-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.

### <a name="creating-a-bluejeans-test-user"></a>BlueJeans test kullanıcısı oluşturma

Bu bölümün amacı BlueJeans Britta Simon adlı bir kullanıcı oluşturmaktır. BlueJeans destekler otomatik kullanıcı hazırlama, varsayılan olarak etkin olduğu. Daha fazla ayrıntı bulabilirsiniz [burada](bluejeans-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

**Kullanıcı el ile oluşturmanız gerekiyorsa, aşağıdaki adımları uygulayın:**

1. Oturum açın, **BlueJeans** yönetici olarak şirketin site.

2. Git **yönetici \> kullanıcıları yönetme \> kullanıcı ekleme**.

   ![Yönetici](./media/bluejeans-tutorial/IC785877.png "yönetici")

   >[!IMPORTANT]
   >**Kullanıcı Ekle** sekmesi kullanılabilir yalnızca ise, **Güvenlik sekmesinde**, **otomatik sağlamayı etkinleştirme** olarak işaretli değildir. 

3. İçinde **Kullanıcı Ekle** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı ekleme](./media/bluejeans-tutorial/IC785886.png "kullanıcı ekleme")

    a. Tür a **BlueJeans kullanıcıadı**, bir **e-posta adresi**, **BlueJeans toplantı kimliği**, **Moderator geçiş kodu**, **tam adı** , **şirket** istediğiniz ilgili metin kutularına zbilgisayarlar geçerli bir AAD hesabı.

    b. Tıklayın **kullanıcı ekleme**.

>[!NOTE]
>Herhangi diğer BlueJeans kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak BlueJeans tarafından sağlanan.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için BlueJeans erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200]

**Britta Simon BlueJeans için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **BlueJeans**.

    ![Çoklu oturum açmayı yapılandırın](./media/bluejeans-tutorial/tutorial_bluejeans_app.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli BlueJeans kutucuğa tıkladığınızda, oturum açma sayfası BlueJeans uygulamanın almanız gerekir.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)
* [Kullanıcı sağlamayı yapılandırma](bluejeans-provisioning-tutorial.md)

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
