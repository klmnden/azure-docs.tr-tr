---
title: 'Öğretici: Azure Active Directory JIRA SAML SSO tarafından Microsoft (V5.2) ile tümleştirme | Microsoft Docs'
description: JIRA SAML SSO tarafından Microsoft (V5.2) ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d0c00408-f9b8-4a79-bccc-c346a7331845
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2018
ms.author: jeedes
ms.openlocfilehash: d7f53efd4b473f36aa03628da4992d1c4c2fb04b
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "42058310"
---
# <a name="tutorial-azure-active-directory-integration-with-jira-saml-sso-by-microsoft-v52"></a>Öğretici: Azure Active Directory JIRA SAML SSO tarafından Microsoft (V5.2) ile tümleştirme

Bu öğreticide, JIRA SAML SSO tarafından Microsoft (V5.2) Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

JIRA SAML SSO tarafından Microsoft (V5.2) Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- JIRA SAML SSO tarafından Microsoft (V5.2) erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan JIRA SAML SSO için Microsoft (V5.2 tarafından) (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="description"></a>Açıklama

Microsoft Azure Active Directory hesabınız Atlassian JIRA sunucusu ile çoklu oturum açmayı etkinleştirmek için kullanın. Bu şekilde tüm kuruluş kullanıcıları Azure AD kimlik JIRA uygulamasına oturum açmak için kullanabilirsiniz. Bu eklenti, Federasyon için SAML 2.0 kullanır.

## <a name="prerequisites"></a>Önkoşullar

JIRA SAML SSO tarafından Microsoft (V5.2) ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- JIRA çekirdek ve yazılım 5.2 yüklü ve yapılandırılmış Windows 64-bit sürümü
- HTTPS etkin JIRA sunucusudur
- JIRA eklentisi için desteklenen sürümler bölümü belirtilmiştir unutmayın.
- JIRA sunucu kimlik doğrulaması için Azure AD oturum açma sayfasına özellikle Internet üzerinden erişilebilir olduğundan ve Azure AD'den belirteç alamaz gerekir
- Yönetici kimlik bilgileri JIRA'da ayarlanmıştır
- JIRA'da WebSudo devre dışı bırakıldı
- JIRA sunucu uygulamasında oluşturulan bir test kullanıcısı

> [!NOTE]
> Bu öğreticideki adımları test etmek için bir üretim ortamına JIRA kullanarak önermiyoruz. Tümleşik geliştirme ya da hazırlık ortamı uygulamanın önce test ve üretim ortamına kullanın.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

**Desteklenen sürümler:**

*   JIRA çekirdek ve yazılım: 5.2
*   JIRA, 6.0 ve 7.8 de destekler. Daha fazla bilgi için tıklayın [JIRA SAML SSO Microsoft tarafından](jiramicrosoft-tutorial.md)

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin.
Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden JIRA SAML SSO tarafından Microsoft (V5.2) ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-jira-saml-sso-by-microsoft-v52-from-the-gallery"></a>Galeriden JIRA SAML SSO tarafından Microsoft (V5.2) ekleme
Azure AD'de JIRA SAML SSO tarafından Microsoft (V5.2) tümleştirmesini yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden JIRA SAML SSO tarafından Microsoft (V5.2) eklemeniz gerekir.

**Galeriden JIRA SAML SSO tarafından Microsoft (V5.2) eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **JIRA SAML SSO tarafından Microsoft (V5.2)** seçin **JIRA SAML SSO tarafından Microsoft (V5.2)** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![JIRA SAML SSO tarafından Microsoft (V5.2) sonuç listesinde](./media/jira52microsoft-tutorial/tutorial_singlesign-onforjira5.2_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma JIRA SAML SSO tarafından Microsoft ("Britta Simon" adlı bir test kullanıcı tabanlı V5.2) ile test edin.

Tek iş için oturum açma için Azure AD ne JIRA SAML SSO tarafından Microsoft (V5.2) karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı JIRA SAML SSO tarafından Microsoft (V5.2) arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma JIRA SAML SSO tarafından Microsoft (V5.2) ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Microsoft (V5.2) test kullanıcı tarafından bir JIRA SAML SSO oluşturma](#create-a-jira-saml-sso-by-microsoft-v52-test-user)**  - bir karşılığı Britta simon'un JIRA SAML SSO tarafından Microsoft (kullanıcı Azure AD gösterimini bağlı V5.2) sağlamak için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma (V5.2) Microsoft uygulaması tarafından JIRA SAML çoklu oturum AÇMAYA yapılandırın.

**JIRA SAML SSO tarafından Microsoft (V5.2) ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **JIRA SAML SSO tarafından Microsoft (V5.2)** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/jira52microsoft-tutorial/tutorial_singlesign-onforjira5.2_samlbase.png)

3. Üzerinde **JIRA SAML SSO Microsoft Domain ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![JIRA SAML SSO bilgileriyle Microsoft Domain ve URL'leri tek oturum açma](./media/jira52microsoft-tutorial/tutorial_singlesign-onforjira5.2_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<domain:port>/plugins/servlet/saml/auth`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<domain:port>/`

    c. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<domain:port>/plugins/servlet/saml/auth`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Adlandırılmış bir URL olması durumunda bağlantı noktası isteğe bağlıdır. Bu değerler, öğreticinin ilerleyen bölümlerinde açıklanan Jıra eklentisi, yapılandırma sırasında alınır.

4. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** kopyalayıp Not Defteri'ne yapıştırın.

    ![Çoklu oturum açmayı yapılandırın](./media/jira52microsoft-tutorial/tutorial_metadataurl.png)

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/jira52microsoft-tutorial/tutorial_general_400.png)

6. Farklı bir web tarayıcı penceresinde bir JIRA Örneğiniz için bir yönetici olarak oturum açın.

7. Dişlisine gelin ve tıklayın **eklentileri**.

    ![Çoklu oturum açmayı yapılandırın](./media/jira52microsoft-tutorial/addon1.png)

8. Eklentiler sekmesi bölümü altında **eklentileri yönetme**.

    ![Çoklu oturum açmayı yapılandırın](./media/jira52microsoft-tutorial/addon7.png)

9. Eklentisini indirin [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=56521). Microsoft tarafından sağlanan eklentisini el ile karşıya **karşıya yükleme, eklenti** menüsü. Eklenti indirilmesini altında ele [Microsoft hizmet sözleşmesi](https://www.microsoft.com/servicesagreement/).

    ![Çoklu oturum açmayı yapılandırın](./media/jira52microsoft-tutorial/addon12.png)

10. Eklentiyi yükledikten sonra görünür **kullanıcı yüklü** eklentileri bölümü. Tıklayın **yapılandırma** yeni eklentiyi yapılandırmak için.

    ![Çoklu oturum açmayı yapılandırın](./media/jira52microsoft-tutorial/addon13.png)

11. Yapılandırma sayfasında aşağıdaki adımları uygulayın:

    ![Çoklu oturum açmayı yapılandırın](./media/jira52microsoft-tutorial/addon52.png)

    > [!TIP]
    > Böylece hata meta veriler çözümlenmesinde uygulamayı karşı eşlenen yalnızca bir sertifika olduğundan emin olun. Yönetici, meta veri çözümleme sırasında birden çok sertifika varsa bir hata alır.

    a. İçinde **meta veri URL'si** metin kutusu, yapıştırma **uygulama Federasyon meta verileri URL'sini** Azure portal'ı seçin ve kopyaladığınız değeri **çözmek** düğmesi. IDP meta veri URL'sini okur ve tüm alanları bilgileri doldurur.

    b. Kopyalama **tanımlayıcısı, yanıt URL'si ve oturum açma URL'si** değerleri ve bunları yapıştırın **tanımlayıcısı, yanıt URL'si ve oturum açma URL'si** sırasıyla içinde metin kutuları **JIRA SAML SSO (V5.2) Microsoft etki alanı ve URL'ler**  bölümü Azure portalı.

    c. İçinde **oturum açma düğmesi adı** kuruluşunuzun oturum açma ekranında kullanıcıların istediği düğme adı yazın.

    d. İçinde **SAML kullanıcı kimliği konumları** seçin **kullanıcı kimliğidir konu deyiminin NameIdentifier öğesinde** veya **kullanıcı kimliği olup öznitelik öğe**.  Bu kimliği JIRA kullanıcı kimliği olması gerekir. Kullanıcı Kimliği eşleşmiyorsa, sonra sistem oturum açmasına izin vermez.

    > [!Note]
    > Varsayılan kullanıcı kimliği SAML ad tanımlayıcısı konumdur. Bu öznitelik seçeneği değiştirin ve uygun öznitelik adını girin.

    e. Seçerseniz **kullanıcı kimliği olup öznitelik öğe** seçeneği, ardından **öznitelik adı** metin kullanıcı kimliği beklenirken özniteliğin adını yazın. 

    f. Azure AD ile Federasyon etki alanını (örneğin, ADFS vb.) kullanıyorsanız, ardından **giriş bölgesi bulmayı etkinleştirmek** seçeneği ve yapılandırma **etki alanı adı**.

    g. İçinde **etki alanı adı** ADFS tabanlı oturum açma durumunda burada etki alanı adını yazın.

    h. Denetleme **etkinleştirme çoklu oturum kapatma** Azure AD'den bir kullanıcı oturum açtığında JIRA oturumunuzu kapatmak istiyor. 

    i. Tıklayın **Kaydet** düğmesini kullanarak ayarları kaydedin.

    > [!NOTE]
    > Yükleme ve sorun giderme hakkında daha fazla bilgi için ziyaret [MS JIRA SSO Bağlayıcısı yönetici kılavuzundaki](../ms-confluence-jira-plugin-adminguide.md) ayrıca [SSS](../ms-confluence-jira-plugin-faq.md) , Yardım almak için

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/jira52microsoft-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/jira52microsoft-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/jira52microsoft-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/jira52microsoft-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-a-jira-saml-sso-by-microsoft-v52-test-user"></a>Microsoft (V5.2) test kullanıcı tarafından bir JIRA SAML SSO oluşturma

JIRA şirket içi sunucuya oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar JIRA şirket içi sunucuya sağlanması gerekir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. JIRA şirket içi sunucunuza yönetici olarak oturum açın.

2. Dişlisine gelin ve tıklayın **kullanıcı yönetimi**.

    ![Çalışan Ekle](./media/jira52microsoft-tutorial/user1.png)

3. Girmek için yönetici erişimi sayfasına yönlendirilirsiniz **parola** tıklatıp **Onayla** düğmesi.

    ![Çalışan Ekle](./media/jira52microsoft-tutorial/user2.png)

4. Altında **kullanıcı yönetimi** sekmesinde bölüm **oluşturacağı**.

    ![Çalışan Ekle](./media/jira52microsoft-tutorial/user3.png) 

5. Üzerinde **"Yeni kullanıcı oluşturma"** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çalışan Ekle](./media/jira52microsoft-tutorial/user4.png)

    a. İçinde **e-posta adresi** metin kutusuna kullanıcı e-posta adresi türünü ister Brittasimon@contoso.com.

    b. İçinde **tam adı** metin Britta Simon gibi kullanıcının tam adını yazın.

    c. İçinde **kullanıcıadı** metin kutusuna kullanıcı e-posta türünü ister Brittasimon@contoso.com.

    d. İçinde **parola** metin kutusu, kullanıcı parolasını yazın.

    e. Tıklayın **kullanıcı oluşturma**.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, JIRA SAML SSO tarafından Microsoft (V5.2) için erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Britta Simon JIRA SAML SSO tarafından Microsoft (V5.2) atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **JIRA SAML SSO tarafından Microsoft (V5.2)**.

    ![Uygulamalar listesinde Microsoft (V5.2) bağlantısıyla JIRA SAML SSO](./media/jira52microsoft-tutorial/tutorial_singlesign-onforjira5.2_app.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

JIRA SAML SSO erişim Paneli'nde Microsoft (V5.2) kutucuğu tarafından'ye tıkladığınızda, otomatik olarak, JIRA SAML SSO için Microsoft (V5.2) uygulama tarafından açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/msaadssojira5.2-tutorial/tutorial_general_01.png
[2]: ./media/msaadssojira5.2-tutorial/tutorial_general_02.png
[3]: ./media/msaadssojira5.2-tutorial/tutorial_general_03.png
[4]: ./media/msaadssojira5.2-tutorial/tutorial_general_04.png

[100]: ./media/msaadssojira5.2-tutorial/tutorial_general_100.png

[200]: ./media/msaadssojira5.2-tutorial/tutorial_general_200.png
[201]: ./media/msaadssojira5.2-tutorial/tutorial_general_201.png
[202]: ./media/msaadssojira5.2-tutorial/tutorial_general_202.png
[203]: ./media/msaadssojira5.2-tutorial/tutorial_general_203.png