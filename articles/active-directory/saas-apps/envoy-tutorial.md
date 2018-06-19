---
title: 'Öğretici: Azure Active Directory Tümleştirme ile haberci | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile haberci arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 71f7afcc-1033-4098-9b7e-4f9f2b26f734
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: jeedes
ms.openlocfilehash: 07045e62b84f7746f0cc544c9631ffbe24de3c2a
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35979868"
---
# <a name="tutorial-azure-active-directory-integration-with-envoy"></a>Öğretici: Azure Active Directory Tümleştirme haberci ile

Bu öğreticide, Azure Active Directory (Azure AD) ile haberci tümleştirmek öğrenin.

Haberci Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Haberci erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için haberci (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme haberci ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir haberci çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden haberci ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-envoy-from-the-gallery"></a>Galeriden haberci ekleme
Azure AD haberci tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden haberci eklemeniz gerekir.

**Galeriden haberci eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **haberci**seçin **haberci** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde haberci](./media/envoy-tutorial/tutorial_envoy_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı haberci sınayın.

Tekli çalışmaya oturum için Azure AD haberci karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının haberci ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Haberci içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma haberci ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir haberci test kullanıcısı oluşturma](#create-an-envoy-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı haberci sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma haberci uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile haberci yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **haberci** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/envoy-tutorial/tutorial_envoy_samlbase.png)

3. Üzerinde **haberci etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Haberci etki alanı ve URL'leri tek oturum açma bilgileri](./media/envoy-tutorial/tutorial_envoy_url.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<tenant-name>.Envoy.com`
    
    > [!NOTE] 
    > Bu değer gerçek değil. Bu değer gerçek oturum açma URL'si ile güncelleştirin. Kişi [haberci istemci destek ekibi](https://envoy.com/contact/) bu değeri alınamıyor.

4. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalama **parmak İZİ** sertifika değerini...

    ![Sertifika indirme bağlantısı](./media/envoy-tutorial/tutorial_envoy_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/envoy-tutorial/tutorial_general_400.png)

6. Üzerinde **haberci yapılandırma** 'yi tıklatın **yapılandırma haberci** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Haberci yapılandırma](./media/envoy-tutorial/tutorial_envoy_configure.png)

7. Farklı web tarayıcısı penceresinde haberci şirket sitenize yönetici olarak oturum açın.

8. Üstteki araç çubuğunda tıklatın **ayarları**.

    ![Haberci](./media/envoy-tutorial/ic776782.png "haberci")

9. Tıklatın **şirket**.

    ![Şirket](./media/envoy-tutorial/ic776783.png "şirket")

10. Tıklatın **SAML**.

    ![SAML](./media/envoy-tutorial/ic776784.png "SAML")

11. İçinde **SAML kimlik doğrulaması** yapılandırma bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SAML kimlik doğrulaması](./media/envoy-tutorial/ic776785.png "SAML kimlik doğrulaması")
    
    >[!NOTE]
    >Denetim merkezini konum kimliği için uygulama tarafından üretilen otomatik bir değerdir.
    
    a. İçinde **parmak izi** metin kutusuna, Yapıştır **parmak izi** Azure portalından kopyaladığınız sertifika değeri.
    
    b. Yapıştır **SAML çoklu oturum açma hizmet URL'si** kopyaladığınız değeri form Azure portalına **kimlik SAĞLAYICISI HTTP SAML URL'si** metin kutusu.
    
    c. Tıklatın **değişiklikleri kaydetmek**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/envoy-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/envoy-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/envoy-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/envoy-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-envoy-test-user"></a>Bir haberci test kullanıcısı oluşturma

Kullanıcı için haberci hazırlama yapılandırmanız için eylem öğe yok. Atanmış bir kullanıcı erişim paneli kullanılarak haberci oturum açmaya çalıştığında haberci kullanıcının var olup olmadığını denetler. Hiçbir kullanıcı hesabı varsa kullanılabilir henüz, haberci tarafından otomatik olarak oluşturulur.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta haberci için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Haberci için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **haberci**.

    ![Uygulamalar listesinde haberci bağlantı](./media/envoy-tutorial/tutorial_envoy_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli haberci parçasında tıklattığınızda, otomatik olarak haberci uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/envoy-tutorial/tutorial_general_01.png
[2]: ./media/envoy-tutorial/tutorial_general_02.png
[3]: ./media/envoy-tutorial/tutorial_general_03.png
[4]: ./media/envoy-tutorial/tutorial_general_04.png

[100]: ./media/envoy-tutorial/tutorial_general_100.png

[200]: ./media/envoy-tutorial/tutorial_general_200.png
[201]: ./media/envoy-tutorial/tutorial_general_201.png
[202]: ./media/envoy-tutorial/tutorial_general_202.png
[203]: ./media/envoy-tutorial/tutorial_general_203.png

