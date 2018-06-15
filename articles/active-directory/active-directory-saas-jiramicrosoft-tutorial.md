---
title: 'Öğretici: Azure Active Directory Tümleştirme JIRA SAML SSO Microsoft tarafından | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve Microsoft tarafından JIRA SAML SSO arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4b663047-7f88-443b-97bd-54224b232815
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2018
ms.author: jeedes
ms.openlocfilehash: 99d50e67ea7194444a4cbff1e48d370ab7f4f370
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34698705"
---
# <a name="tutorial-azure-active-directory-integration-with-jira-saml-sso-by-microsoft"></a>Öğretici: Microsoft tarafından JIRA SAML SSO Azure Active Directory Tümleştirme

Bu öğreticide, Microsoft tarafından JIRA SAML SSO Azure Active Directory (Azure AD) ile tümleştirme öğrenin.

Microsoft tarafından JIRA SAML SSO Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Microsoft tarafından JIRA SAML SSO erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak JIRA SAML SSO (çoklu oturum açma) Microsoft tarafından açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="description"></a>Açıklama

Microsoft Azure Active Directory hesabınıza Atlassian JIRA sunucusuyla çoklu oturum açmayı etkinleştirmek için kullanın. Bu şekilde tüm kuruluş kullanıcılarınızın Azure AD kimlik JIRA uygulamasına oturum açmak için kullanabilirsiniz. Bu eklenti, Federasyon için SAML 2.0 kullanır.

## <a name="prerequisites"></a>Önkoşullar

Microsoft tarafından JIRA SAML SSO ile Azure AD tümleştirme yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- JIRA çekirdek 7,8 yazılım 6.0 veya JIRA hizmet Masası 3.0 3.2 için yüklü ve yapılandırılmış Windows 64-bit sürümü
- HTTPS etkinleştirilmiş JIRA sunucusudur
- Aşağıdaki bölümüne JIRA eklentisi için desteklenen sürümleri bahsedilen unutmayın.
- JIRA sunucu kimlik doğrulaması için Azure AD oturum açma sayfasına özellikle Internet üzerinden erişilebildiğinden ve Azure AD'den belirteç alamaz gerekir
- Yönetici kimlik bilgileri JIRA ayarlanır
- WebSudo JIRA içinde devre dışı bırakıldı
- JIRA sunucu uygulamasından oluşturulan test kullanıcısı

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamında JIRA birini kullanarak önermiyoruz. Geliştirme veya uygulamanın hazırlama ortamında tümleştirme önce test ve üretim ortamına kullanın.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, burada bir aylık deneme elde edebilirsiniz: [deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="supported-versions-of-jira"></a>JIRA desteklenen sürümleri

*   JIRA çekirdek ve yazılım: 6.0 için 7,8
*   JIRA hizmet masasına 3.0 için 3.2
*   JIRA 5.2 de destekler. Daha fazla ayrıntı için tıklatın [Microsoft Azure Active Directory çoklu oturum açma JIRA 5.2 için](./active-directory-saas-jira52microsoft-tutorial.md)

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin.
Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Microsoft tarafından JIRA SAML SSO Galeriden ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-jira-saml-sso-by-microsoft-from-the-gallery"></a>Microsoft tarafından JIRA SAML SSO Galeriden ekleme
Azure AD JIRA SAML SSO Microsoft tarafından tümleştirilmesi yapılandırmak için Microsoft tarafından JIRA SAML SSO Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Microsoft tarafından JIRA SAML SSO Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **JIRA SAML SSO Microsoft tarafından**seçin **JIRA SAML SSO Microsoft tarafından** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Microsoft tarafından JIRA SAML SSO](.\media\active-directory-saas-msaadssojira-tutorial\tutorial_singlesign-onforjira_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma JIRA SAML SSO "Britta Simon" adlı bir test kullanıcıyı temel alarak Microsoft tarafından test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen JIRA SAML SSO Microsoft tarafından bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve Microsoft tarafından JIRA SAML SSO ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma JIRA SAML SSO Microsoft tarafından test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Microsoft test kullanıcı tarafından JIRA SAML SSO oluşturma](#create-a-jira-saml-sso-by-microsoft-test-user)**  - Britta Simon, karşılık gelen JIRA SAML SSO kullanıcı Azure AD gösterimini bağlantılı Microsoft tarafından sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Microsoft uygulaması tarafından JIRA SAML SSO yapılandırın.

**Microsoft tarafından JIRA SAML SSO ile Azure AD çoklu oturum açma yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **JIRA SAML SSO Microsoft tarafından** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](.\media\active-directory-saas-msaadssojira-tutorial\tutorial_singlesign-onforjira_samlbase.png)

3. Üzerinde **JIRA SAML SSO Microsoft Domain ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![JIRA SAML SSO Microsoft Domain ve URL'ler tek oturum açma bilgileri](.\media\active-directory-saas-msaadssojira-tutorial\tutorial_singlesign-onforjira_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<domain:port>/plugins/servlet/saml/auth`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<domain:port>/`

    c. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<domain:port>/plugins/servlet/saml/auth`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler, gerçek tanımlayıcı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Adlandırılmış bir URL olması durumunda bağlantı noktası isteğe bağlıdır. Öğreticide daha sonra açıklanan Jira eklentisi yapılandırma sırasında bu değerleri alma.

4. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesini tıklatın **uygulama Federasyon meta veri URL'sini** ve Not Defteri'ne yapıştırın.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-msaadssojira-tutorial/tutorial_metadataurl.png)

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](.\media\active-directory-saas-msaadssojira-tutorial\tutorial_general_400.png)

6. Farklı web tarayıcısı penceresinde JIRA Örneğiniz için yönetici olarak oturum açın.

7. Dişlisine üzerine gelin ve tıklatın **eklentileri**.

    ![Çoklu oturum açmayı yapılandırın](.\media\active-directory-saas-msaadssojira-tutorial\addon1.png)

8. Gelen eklentiyi karşıdan [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=56506). Microsoft tarafından sağlanan eklentisini el ile karşıya **karşıya eklenti** menüsü. Eklenti yükleme altında ele [Microsoft hizmet sözleşmesi](https://www.microsoft.com/en-us/servicesagreement/).

    ![Çoklu oturum açmayı yapılandırın](.\media\active-directory-saas-msaadssojira-tutorial\addon12.png)

9. Eklenti yüklendikten sonra görünür **kullanıcı yüklü** eklentileri bölümünü **yönetmek eklenti** bölümü. Tıklatın **yapılandırma** yeni eklenti yapılandırmak için.

    ![Çoklu oturum açmayı yapılandırın](.\media\active-directory-saas-msaadssojira-tutorial\addon13.png)

10. Yapılandırma sayfasında şu adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](.\media\active-directory-saas-msaadssojira-tutorial\addon52.png)

    > [!TIP]
    > Yani hata meta verileri çözümlenmesinde uygulamayı karşı eşlenen yalnızca bir sertifika olduğundan emin olun. Meta Veri Çözümleme sırasında birden fazla sertifika varsa yönetici bir hata alır.

    a. İçinde **meta veri URL'sini** metin kutusuna, Yapıştır **uygulama Federasyon meta veri URL'sini** Azure portal ve tıklatın kopyaladığınız değeri **gidermek** düğmesi. IDP meta veri URL'sini okur ve tüm alanları bilgileri doldurur.

    b. Kopya **tanımlayıcı, yanıt URL'si ve oturum açma URL'si** değerleri ve bunları yapıştırın **tanımlayıcı, yanıt URL'si ve oturum açma URL'si** kutularındaki sırasıyla söz **JIRA SAML SSO Microsoft Domain ve URL'leri** Azure Portal'daki bölümü.

    c. İçinde **oturum açma düğmesi adı** , kuruluşunuzda kullanıcıların oturum açma ekranında düğmesinin adını yazın.

    d. İçinde **SAML kullanıcı kimliği konumları** seçin **kullanıcı kimliğidir konu deyimi NameIdentifier öğesinde** veya **kullanıcı kimliği olan bir öznitelik öğedeki**.  Bu kimliği JIRA kullanıcı kimliği olması gerekir. Kullanıcı Kimliği eşleşmiyorsa, ardından sistemi oturum açmak kullanıcılar izin vermez.

    > [!Note]
    > Varsayılan SAML kullanıcı kimliği konumu ad tanımlayıcısıdır. Bu öznitelik seçeneği ile değiştirin ve uygun öznitelik adını girin.

    e. Seçerseniz **kullanıcı kimliği olan bir öznitelik öğedeki** seçeneği, ardından **öznitelik adı** metin kullanıcı kimliği beklenirken özniteliğin adını yazın.

    f. Azure AD ile Federasyon etki alanını (örneğin, ADFS vb.) kullanıyorsanız, tıklayın **giriş bölgesi bulmayı etkinleştirmek** seçeneği ve yapılandırma **etki alanı adı**.

    g. İçinde **etki alanı adı** ADFS tabanlı oturum açma durumunda burada etki alanı adını yazın.

    h. Denetleme **etkinleştirmek tek oturum kapatma** bir kullanıcı oturum açtığında JIRA Azure AD'den oturumunuzu etmek istiyor.

    i. ' I tıklatın **kaydetmek** düğmesini tıklatarak ayarları kaydedin.

    > [!NOTE]
    > Yükleme ve sorun giderme hakkında daha fazla bilgi için ziyaret [MS JIRA SSO Bağlayıcısı Yönetici Kılavuzu](ms-confluence-jira-plugin-adminguide.md) ayrıca [SSS](ms-confluence-jira-plugin-faq.md) , Yardım almak için

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](.\media\active-directory-saas-msaadssojira-tutorial\create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](.\media\active-directory-saas-msaadssojira-tutorial\create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](.\media\active-directory-saas-msaadssojira-tutorial\create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](.\media\active-directory-saas-msaadssojira-tutorial\create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-a-jira-saml-sso-by-microsoft-test-user"></a>Microsoft test kullanıcı tarafından JIRA SAML SSO oluşturma

JIRA şirket içi sunucuya oturum açmak Azure AD kullanıcıları etkinleştirmek için bunların JIRA SAML SSO Microsoft tarafından sağlanmalıdır. Microsoft tarafından JIRA SAML SSO için sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. JIRA şirket içi sunucunuza yönetici olarak oturum açın.

2. Dişlisine üzerine gelin ve tıklatın **kullanıcı yönetimi**.

    ![Çalışanı ekleyin](.\media\active-directory-saas-msaadssojira-tutorial\user1.png)

3. Girmek için yönetici erişimi sayfasına yönlendirilirsiniz **parola** tıklatıp **Onayla** düğmesi.

    ![Çalışanı ekleyin](.\media\active-directory-saas-msaadssojira-tutorial\user2.png)

4. Altında **kullanıcı yönetimi** bölüm sekmesini tıklatın, **kullanıcı oluşturma**.

    ![Çalışanı ekleyin](.\media\active-directory-saas-msaadssojira-tutorial\user3.png) 

5. Üzerinde **"Yeni kullanıcı oluştur"** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çalışanı ekleyin](.\media\active-directory-saas-msaadssojira-tutorial\user4.png) 

    a. İçinde **e-posta adresi** metin kutusuna, kullanıcının e-posta adresi türü ister Brittasimon@contoso.com.

    b. İçinde **tam adı** metin kutusuna, Britta Simon gibi kullanıcının tam adını yazın.

    c. İçinde **kullanıcıadı** metin kutusu, kullanıcı e-posta türünü ister Brittasimon@contoso.com.

    d. İçinde **parola** metin kutusuna, kullanıcının parolasını yazın.

    e. Tıklatın **kullanıcı oluşturma**.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Microsoft tarafından JIRA SAML SSO için erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Microsoft tarafından JIRA SAML SSO Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201]

2. Uygulamalar listesinde **JIRA SAML SSO Microsoft tarafından**.

    ![Uygulamalar listesinde Microsoft bağlantısıyla JIRA SAML SSO](.\media\active-directory-saas-msaadssojira-tutorial\tutorial_singlesign-onforjira_app.png)

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Microsoft parçasında tarafından JIRA SAML SSO tıklattığınızda, otomatik olarak, JIRA SAML SSO Microsoft uygulaması tarafından açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: .\media\active-directory-saas-msaadssojira-tutorial\tutorial_general_01.png
[2]: .\media\active-directory-saas-msaadssojira-tutorial\tutorial_general_02.png
[3]: .\media\active-directory-saas-msaadssojira-tutorial\tutorial_general_03.png
[4]: .\media\active-directory-saas-msaadssojira-tutorial\tutorial_general_04.png

[100]: .\media\active-directory-saas-msaadssojira-tutorial\tutorial_general_100.png

[200]: .\media\active-directory-saas-msaadssojira-tutorial\tutorial_general_200.png
[201]: .\media\active-directory-saas-msaadssojira-tutorial\tutorial_general_201.png
[202]: .\media\active-directory-saas-msaadssojira-tutorial\tutorial_general_202.png
[203]: .\media\active-directory-saas-msaadssojira-tutorial\tutorial_general_203.png