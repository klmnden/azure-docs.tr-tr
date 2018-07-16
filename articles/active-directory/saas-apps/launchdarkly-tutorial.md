---
title: 'Öğretici: Azure Active Directory LaunchDarkly ile tümleştirme | Microsoft Docs'
description: Azure Active Directory ve LaunchDarkly arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0e9cb37e-16bf-493b-bfc8-8d9840545a1e
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2018
ms.author: jeedes
ms.openlocfilehash: 6d0024adfde82480d46f876021a25bb8f42ba6ff
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39041305"
---
# <a name="tutorial-azure-active-directory-integration-with-launchdarkly"></a>Öğretici: Azure Active Directory LaunchDarkly ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile LaunchDarkly tümleştirme konusunda bilgi edinin.

LaunchDarkly Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- LaunchDarkly erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için LaunchDarkly (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi LaunchDarkly ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- LaunchDarkly çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. LaunchDarkly galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-launchdarkly-from-the-gallery"></a>LaunchDarkly galeri ekleme
LaunchDarkly tümleştirmesi Azure AD'de yapılandırmak için LaunchDarkly Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden LaunchDarkly eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **LaunchDarkly**seçin **LaunchDarkly** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde LaunchDarkly](./media/launchdarkly-tutorial/tutorial_launchdarkly_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı LaunchDarkly ile test edin.

Tek iş için oturum açma için Azure AD ne LaunchDarkly karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve LaunchDarkly ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma LaunchDarkly ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[LaunchDarkly test kullanıcısı oluşturma](#create-a-launchdarkly-test-user)**  - kullanıcı Azure AD gösterimini bağlı LaunchDarkly Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve LaunchDarkly uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma LaunchDarkly ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **LaunchDarkly** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/launchdarkly-tutorial/tutorial_launchdarkly_samlbase.png)

3. Üzerinde **LaunchDarkly etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![LaunchDarkly etki alanı ve URL'ler tek oturum açma bilgileri](./media/launchdarkly-tutorial/tutorial_launchdarkly_url.png)

    a. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna URL'yi yazın: `app.launchdarkly.com`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://app.launchdarkly.com/trust/saml2/acs/<customers-unique-id>`
    
    > [!NOTE]
    > Yanıt URL'si değeri gerçek değil. Bu öğreticinin ilerleyen bölümlerinde açıklanan gerçek yanıt URL'si ile değeri güncelleştirir.

4. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![LaunchDarkly etki alanı ve URL'ler tek oturum açma bilgileri](./media/launchdarkly-tutorial/tutorial_launchdarkly_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna URL'yi yazın: `https://app.launchdarkly.com`

5. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/launchdarkly-tutorial/tutorial_launchdarkly_certificate.png) 

6. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/launchdarkly-tutorial/tutorial_general_400.png)
    
7. Üzerinde **LaunchDarkly yapılandırma** bölümünde **yapılandırma LaunchDarkly** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![LaunchDarkly yapılandırma](./media/launchdarkly-tutorial/tutorial_launchdarkly_configure.png)

8. Farklı bir web tarayıcı penceresinde LaunchDarkly şirket sitenize yönetici olarak oturum.

9. Seçin **hesap ayarları** sol gezinti panelinde.

    ![LaunchDarkly yapılandırma](./media/launchdarkly-tutorial/configure1.png)

10. Tıklayın **güvenlik** sekmesi.

    ![LaunchDarkly yapılandırma](./media/launchdarkly-tutorial/configure2.png)

11. Tıklayın **etkinleştir SSO** ardından **düzenleme SAML yapılandırma**.

    ![LaunchDarkly yapılandırma](./media/launchdarkly-tutorial/configure3.png)

12. Üzerinde **SAML yapılandırmanızı Düzenle** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![LaunchDarkly yapılandırma](./media/launchdarkly-tutorial/configure4.png)

    a. Kopyalama **SAML tüketici hizmeti URL'si** örneğinizin ve yanıt URL'si metin kutusuna yapıştırın **LaunchDarkly etki alanı ve URL'ler** bölümü Azure portalı.

    b. İçinde **oturum açma URL'si** metin kutusu, yapıştırma **çoklu oturum açma hizmeti URL'si** Azure portaldan kopyaladığınız değeri.

    c. İndirilen sertifikanın Not Defteri'ne Azure portalından açın, içeriği kopyalayın ve ardından yapıştırın **X.509 sertifikası** kutusu veya doğrudan yükleyebileceği sertifika tıklayarak **karşıya**.

    d. **Kaydet**’e tıklayın

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/launchdarkly-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/launchdarkly-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/launchdarkly-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/launchdarkly-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-a-launchdarkly-test-user"></a>LaunchDarkly test kullanıcısı oluşturma

Bu bölümün amacı LaunchDarkly Britta Simon adlı bir kullanıcı oluşturmaktır. LaunchDarkly tam zamanında sağlama, varsayılan olarak etkin olan destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı, henüz yoksa LaunchDarkly erişme denemesi sırasında oluşturulur.
>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [LaunchDarkly istemci Destek ekibine](mailto:support@launchdarkly.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için LaunchDarkly erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon LaunchDarkly için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **LaunchDarkly**.

    ![Uygulamalar listesinde LaunchDarkly bağlantı](./media/launchdarkly-tutorial/tutorial_launchdarkly_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde LaunchDarkly kutucuğa tıkladığınızda, otomatik olarak LaunchDarkly uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/launchdarkly-tutorial/tutorial_general_01.png
[2]: ./media/launchdarkly-tutorial/tutorial_general_02.png
[3]: ./media/launchdarkly-tutorial/tutorial_general_03.png
[4]: ./media/launchdarkly-tutorial/tutorial_general_04.png

[100]: ./media/launchdarkly-tutorial/tutorial_general_100.png

[200]: ./media/launchdarkly-tutorial/tutorial_general_200.png
[201]: ./media/launchdarkly-tutorial/tutorial_general_201.png
[202]: ./media/launchdarkly-tutorial/tutorial_general_202.png
[203]: ./media/launchdarkly-tutorial/tutorial_general_203.png

