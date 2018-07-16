---
title: "Öğretici: Azure Active Directory Tümleştirme IQNavigator vm'lerle | Microsoft Docs"
description: Azure Active Directory ve IQNavigator VM'ler arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a8a09b25-dfa5-4c31-aea2-53bf1853b365
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: jeedes
ms.openlocfilehash: 9b264a7ba1479e485ff528ee242c78c4b39010dc
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39052525"
---
# <a name="tutorial-azure-active-directory-integration-with-iqnavigator-vms"></a>Öğretici: Azure Active Directory IQNavigator VM'ler ile tümleştirme

Bu öğreticide, IQNavigator VM'leri Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.

IQNavigator VM'leri Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- IQNavigator VM erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan IQNavigator VM'lere (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile IQNavigator VM'leri yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik IQNavigator VM'leri çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme burada alabilirsiniz [deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden IQNavigator VM'leri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-iqnavigator-vms-from-the-gallery"></a>Galeriden IQNavigator VM'leri ekleme
Azure AD'de IQNavigator VM'leri tümleştirmesini yapılandırmak için IQNavigator VM'leri Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden IQNavigator VM'leri eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **IQNavigator VM'leri**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_search.png)

5. Sonuçlar panelinde seçin **IQNavigator VM'leri**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırmanız ve IQNavigator VM'ler ile Azure AD çoklu oturum açmayı test "Britta Simon" adlı bir test kullanıcı tabanlı.

Tek çalışmak için oturum açma için Azure AD ne IQNavigator vm'lerde karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı IQNavigator vm'lerde arasında bir bağlantı ilişki kurulması gerekir.

Değerini IQNavigator VM'LERDE atama **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma IQNavigator vm'lerle sınamak için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[IQNavigator VM'leri test kullanıcısı oluşturma](#creating-a-iqnavigator-vms-test-user)**  - kullanıcı Azure AD gösterimini bağlı IQNavigator vm'lerde Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve IQNavigator VM'leri uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile IQNavigator VM'leri yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **IQNavigator VM'leri** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_samlbase.png)

3. Üzerinde **IQNavigator VM'leri etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna URL'yi yazın:`iqn.com`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.iqnavigator.com/security/login?client_name=https://sts.window.net/<instance name>`

4. Denetleme **Gelişmiş URL ayarlarını göster**, aşağıdaki adımı uygulayın:

    ![Çoklu oturum açmayı yapılandırın](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_url1.png)

    İçinde **geçiş durumu** metin kutusuna bir URL şu biçimi kullanarak:`https://<subdomain>.iqnavigator.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek yanıt URL'si ve geçiş durumu ile güncelleştirin. İlgili kişi [IQNavigator VM'leri istemci Destek ekibine](https://www.beeline.com/iqn-product-support/) bu değerleri almak için.

5. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** kopyalayıp Not Defteri'ne yapıştırın.
    
    ![Çoklu oturum açmayı yapılandırın](./media/iqnavigatorvms-tutorial/tutorial_metadataurl.png)

6. IQNavigator uygulama adı tanımlayıcısı talebi benzersiz kullanıcı tanıtıcı değeri bekler. Müşteri adı tanımlayıcısı talebi için doğru değeri eşleyebilirsiniz. Bu durumda biz kullanıcı eşlenmiş. UserPrincipalName için Tanıtım amaçlı. Ancak kuruluş ayarlarınıza göre doğru değeri için eşlemeniz.

    ![Çoklu oturum açmayı yapılandırın](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_attribute.png)

7. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/iqnavigatorvms-tutorial/tutorial_general_400.png)

8. Üzerinde **IQNavigator VM'leri yapılandırma** bölümünde **yapılandırma IQNavigator VM'leri** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_configure.png)

9. Çoklu oturum açmayı yapılandırma **IQNavigator VM'leri** tarafını göndermek için ihtiyacınız **uygulama Federasyon meta verileri URL'sini**, **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si**için [IQNavigator VM'leri Destek ekibine](https://www.beeline.com/iqn-product-support/). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/iqnavigatorvms-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/iqnavigatorvms-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/iqnavigatorvms-tutorial/create_aaduser_03.png)

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/iqnavigatorvms-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.

### <a name="creating-a-iqnavigator-vms-test-user"></a>IQNavigator VM'leri test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon IQNavigator VM'LERDE adlı bir kullanıcı oluşturmaktır. Çalışmak [IQNavigator VM'leri Destek ekibine](https://www.beeline.com/iqn-product-support/) IQNavigator VM'leri hesabında kullanıcıları eklemek için.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma IQNavigator VM'lere erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı Ata][200]

**Britta Simon IQNavigator VM'lere atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **IQNavigator VM'leri**.

    ![Çoklu oturum açmayı yapılandırın](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_app.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde IQNavigator VM'leri kutucuğa tıkladığınızda, otomatik olarak IQNavigator VM'leri uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)

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

