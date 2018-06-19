---
title: 'Öğretici: Azure Active Directory Tümleştirme ile UNIFI | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile UNIFI arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: e1f49ee4-d2d4-4a82-9baf-0587ca1f20f6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: bef3da47eed09ad24d43a8fca8cd1db90f7a80a8
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35982191"
---
# <a name="tutorial-azure-active-directory-integration-with-unifi"></a>Öğretici: Azure Active Directory Tümleştirme UNIFI ile

Bu öğreticide, Azure Active Directory (Azure AD) ile UNIFI tümleştirmek öğrenin.

UNIFI Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- UNIFI erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için UNIFI (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme UNIFI ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir UNIFI çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden UNIFI ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-unifi-from-the-gallery"></a>Galeriden UNIFI ekleme
Azure AD UNIFI tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden UNIFI eklemeniz gerekir.

**Galeriden UNIFI eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **UNIFI**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/unifi-tutorial/tutorial_unifi_search.png)

5. Sonuçlar panelinde seçin **UNIFI**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/unifi-tutorial/tutorial_unifi_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı UNIFI sınayın.

Tekli çalışmaya oturum için Azure AD UNIFI karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının UNIFI ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

UNIFI içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma UNIFI ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Test kullanıcı bir UNIFI oluşturma](#creating-a-unifi-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı UNIFI sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma UNIFI uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile UNIFI yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **UNIFI** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/unifi-tutorial/tutorial_unifi_samlbase.png)

3. Üzerinde **UNIFI etki alanı ve URL'leri** uygulamada yapılandırmak istiyorsanız, bölüm **IDP** modu tarafından başlatılan:

    ![Çoklu oturum açmayı yapılandırın](./media/unifi-tutorial/tutorial_unifi_url1.png)

    İçinde **tanımlayıcısı** metin değeri yazın: `INVIEWlabs` 

4. Denetleme **Göster Gelişmiş URL ayarları**, uygulamada yapılandırmak istiyorsanız **SP** modunda başlatılan:

    ![Çoklu oturum açmayı yapılandırın](./media/unifi-tutorial/tutorial_unifi_url2.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL'yi yazın: `https://app.discoverunifi.com/login`

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/unifi-tutorial/tutorial_unifi_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/unifi-tutorial/tutorial_general_400.png)
    
7. Üzerinde **UNIFI yapılandırma** 'yi tıklatın **yapılandırma UNIFI** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/unifi-tutorial/tutorial_unifi_configure.png)

8. Farklı web tarayıcısı penceresinde oturum açın, **UNIFI** yönetici olarak şirket site.

9. Tıklayın **kullanıcılar**.

    ![Çoklu oturum açmayı yapılandırın](./media/unifi-tutorial/app1.png) 

10. Tıklayın **yeni kimlik sağlayıcısı ekleyin**.

    ![Çoklu oturum açmayı yapılandırın](./media/unifi-tutorial/app2.png)

11. İçinde **kimlik sağlayıcı Ekle** bölümünde, aşağıdaki adımları gerçekleştirin:   

    ![Çoklu oturum açmayı yapılandırın](./media/unifi-tutorial/app3.png) 

    a. İçinde **sağlayıcı adı** metin kutusuna, kimlik sağlayıcısı adını yazın...

    b. İçinde **sağlayıcısı URL** textbox Yapıştır **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyaladığınız değeri.

    c. Açık Not Defteri'nde, Azure portalından indirmiş sertifikayı kaldırmak **---başlangıç sertifika---** ve **---son SERTİFİKAYI---** etiketi ve kalan içeriği yapıştırmak **sertifika** metin kutusu.

    d. Seçin **varsayılan bir sağlayıcı** onay kutusu.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/unifi-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/unifi-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/unifi-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/unifi-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-unifi-test-user"></a>UNIFI test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı oluşturun. **UNIFI** hiçbir el ile yapılacak adımlar gerekli; bu nedenle otomatik kullanıcı sağlamayı destekler. Kullanıcılar, Azure AD'den başarılı kimlik doğrulamasından sonra otomatik olarak oluşturulur.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta UNIFI için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**UNIFI için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **UNIFI**.

    ![Çoklu oturum açmayı yapılandırın](./media/unifi-tutorial/tutorial_unifi_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli UNIFI parçasında tıklattığınızda, otomatik olarak UNIFI uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/unifi-tutorial/tutorial_general_01.png
[2]: ./media/unifi-tutorial/tutorial_general_02.png
[3]: ./media/unifi-tutorial/tutorial_general_03.png
[4]: ./media/unifi-tutorial/tutorial_general_04.png

[100]: ./media/unifi-tutorial/tutorial_general_100.png

[200]: ./media/unifi-tutorial/tutorial_general_200.png
[201]: ./media/unifi-tutorial/tutorial_general_201.png
[202]: ./media/unifi-tutorial/tutorial_general_202.png
[203]: ./media/unifi-tutorial/tutorial_general_203.png

