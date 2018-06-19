---
title: 'Öğretici: Azure Active Directory Tümleştirme IQNavigator VMs | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile IQNavigator VM'ler arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a8a09b25-dfa5-4c31-aea2-53bf1853b365
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: jeedes
ms.openlocfilehash: 93e50c1804e9e28bffca0bc5eb8fc3e4c8be07fa
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35982919"
---
# <a name="tutorial-azure-active-directory-integration-with-iqnavigator-vms"></a>Öğretici: Azure Active Directory Tümleştirme IQNavigator VMs

Bu öğreticide, Azure Active Directory (Azure AD) ile IQNavigator VM'ler tümleştirmek öğrenin.

IQNavigator VM'ler Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- IQNavigator VM'ler erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak IQNavigator VM'lere (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme IQNavigator VM'ler ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir IQNavigator VM'ler çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, burada bir aylık deneme alabilirsiniz [deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden IQNavigator VM'ler ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-iqnavigator-vms-from-the-gallery"></a>Galeriden IQNavigator VM'ler ekleme
Azure AD IQNavigator VM'ler tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden IQNavigator VM'ler eklemeniz gerekir.

**Galeriden IQNavigator VM'ler eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **IQNavigator VM'ler**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_search.png)

5. Sonuçlar panelinde seçin **IQNavigator VM'ler**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırmanız ve IQNavigator VM'ler ile Azure AD çoklu oturum açmayı test "Britta Simon" adlı bir test kullanıcı tabanlı.

Tekli çalışmaya oturum için Azure AD IQNavigator VM'ler karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının IQNavigator VM'ler ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

IQNavigator VM'LERDE değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırmak ve Azure AD çoklu oturum açma IQNavigator VMs sınamak için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[IQNavigator VM'ler test kullanıcısı oluşturma](#creating-a-iqnavigator-vms-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı IQNavigator VM'ler sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma IQNavigator VM'ler uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma IQNavigator VM'ler ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **IQNavigator VM'ler** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_samlbase.png)

3. Üzerinde **IQNavigator VM'ler etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL'yi yazın:`iqn.com`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.iqnavigator.com/security/login?client_name=https://sts.window.net/<instance name>`

4. Denetleme **Göster Gelişmiş URL ayarları**, aşağıdaki adımı gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_url1.png)

    İçinde **geçiş durumu** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<subdomain>.iqnavigator.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri fiili yanıt URL'si ve geçiş durumu ile güncelleştirin. Kişi [IQNavigator VM'ler istemci destek ekibi](https://www.beeline.com/iqn-product-support/) bu değerleri almak için.

5. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesini tıklatın **uygulama Federasyon meta veri URL'sini** ve Not Defteri'ne yapıştırın.
    
    ![Çoklu oturum açmayı yapılandırın](./media/iqnavigatorvms-tutorial/tutorial_metadataurl.png)

6. IQNavigator uygulama adı tanımlayıcısı talep benzersiz kullanıcı kimliği değeri bekler. Müşteri adı tanımlayıcısı talep için doğru değerini eşleyebilirsiniz. Bu durumda biz kullanıcı eşledikten. Tanıtım amaçlı UserPrincipalName. Ancak, kuruluşunuz ayarlarınıza göre doğru değeri için eşlenmesi gerekir.

    ![Çoklu oturum açmayı yapılandırın](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_attribute.png)

7. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/iqnavigatorvms-tutorial/tutorial_general_400.png)

8. Üzerinde **IQNavigator VM'ler yapılandırma** 'yi tıklatın **yapılandırma IQNavigator VM'ler** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_configure.png)

9. Çoklu oturum açma yapılandırmak için **IQNavigator VM'ler** yan, ihtiyacınız göndermek **uygulama Federasyon meta veri URL'sini**, **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si**için [IQNavigator VM'ler destek ekibi](https://www.beeline.com/iqn-product-support/). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/iqnavigatorvms-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/iqnavigatorvms-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/iqnavigatorvms-tutorial/create_aaduser_03.png)

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/iqnavigatorvms-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.

### <a name="creating-a-iqnavigator-vms-test-user"></a>IQNavigator VM'ler test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon IQNavigator VM'LERDE adlı bir kullanıcı oluşturmaktır. Çalışmak [IQNavigator VM'ler destek ekibi](https://www.beeline.com/iqn-product-support/) IQNavigator VM'ler hesabında kullanıcıları eklemek için.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta IQNavigator VM'lere erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200]

**Britta Simon IQNavigator VM'ler atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201]

2. Uygulamalar listesinde **IQNavigator VM'ler**.

    ![Çoklu oturum açmayı yapılandırın](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_app.png)

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli IQNavigator VM'ler parçasında tıklattığınızda, otomatik olarak IQNavigator VM'ler uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/iqnavigatorvms-tutorial/tutorial_general_01.png
[2]: ./media/iqnavigatorvms-tutorial/tutorial_general_02.png
[3]: ./media/iqnavigatorvms-tutorial/tutorial_general_03.png
[4]: ./media/iqnavigatorvms-tutorial/tutorial_general_04.png

[100]: ./media/iqnavigatorvms-tutorial/tutorial_general_100.png

[200]: ./media/iqnavigatorvms-tutorial/tutorial_general_200.png
[201]: ./media/iqnavigatorvms-tutorial/tutorial_general_201.png
[202]: ./media/iqnavigatorvms-tutorial/tutorial_general_202.png
[203]: ./media/iqnavigatorvms-tutorial/tutorial_general_203.png

