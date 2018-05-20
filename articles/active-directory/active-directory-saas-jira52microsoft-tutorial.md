---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Microsoft Azure Active Directory çoklu oturum açma JIRA 5.2 için | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Microsoft Azure Active Directory tek oturum açma için JIRA 5.2 arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d0c00408-f9b8-4a79-bccc-c346a7331845
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: jeedes
ms.openlocfilehash: 785a75d3d9a748e3ddf01099fc844b5c1099d0b9
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-microsoft-azure-active-directory-single-sign-on-for-jira-52"></a>Öğretici: Azure Active Directory Tümleştirme ile Microsoft Azure Active Directory çoklu oturum açma JIRA 5.2 için

Bu öğreticide, Microsoft Azure Active Directory çoklu oturum açma JIRA 5.2 için Azure Active Directory (Azure AD) ile tümleştirme öğrenin.

Microsoft Azure Active Directory çoklu oturum açma JIRA 5.2 için Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Microsoft Azure Active Directory çoklu oturum açma JIRA 5.2 için erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak Microsoft Azure Active Directory çoklu oturum açma (çoklu oturum açma) JIRA 5.2 için Azure AD hesaplarına sahip için açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="description"></a>Açıklama

Microsoft Azure Active Directory hesabınıza Atlassian JIRA sunucusuyla çoklu oturum açmayı etkinleştirmek için kullanın. Bu şekilde tüm kuruluş kullanıcılarınızın Azure AD kimlik JIRA uygulamasına oturum açmak için kullanabilirsiniz. Bu eklenti, Federasyon için SAML 2.0 kullanır.

## <a name="prerequisites"></a>Önkoşullar

Microsoft Azure Active Directory tek oturum açma ile JIRA 5.2 için Azure AD tümleştirme yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- JIRA çekirdek ve yazılım 5.2 yüklü ve yapılandırılmış Windows 64-bit sürümü
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
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

**Desteklenen sürümler:**

*   JIRA çekirdek ve yazılım: 5.2
*   JIRA 6.0 ve 7.2.0 de destekler. Daha fazla ayrıntı için tıklatın [Microsoft Azure Active Directory çoklu oturum açma JIRA için](./active-directory-saas-jiramicrosoft-tutorial.md)

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin.
Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Microsoft Azure Active Directory tek oturum açma için JIRA 5.2 Galeriden ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-microsoft-azure-active-directory-single-sign-on-for-jira-52-from-the-gallery"></a>Microsoft Azure Active Directory tek oturum açma için JIRA 5.2 Galeriden ekleme
Azure AD, Microsoft Azure Active Directory tek oturum açma için JIRA 5.2 tümleştirilmesi yapılandırmak için Microsoft Azure Active Directory tek oturum açma için JIRA 5.2 Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Microsoft Azure Active Directory tek oturum açma için JIRA 5.2 Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Microsoft Azure Active Directory çoklu oturum açma JIRA 5.2 için**seçin **Microsoft Azure Active Directory çoklu oturum açma JIRA 5.2 için** sonuç panelinden ardından  **Ekleme** uygulama eklemek için düğmesi.

    ![Microsoft Azure Active Directory çoklu oturum açma JIRA 5.2 için sonuçlar listesinde](.\media\active-directory-saas-msaadssojira5.2-tutorial\tutorial_singlesign-onforjira5.2_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ile Microsoft Azure Active Directory çoklu oturum açma JIRA "Britta Simon" adlı bir test kullanıcı tabanlı 5.2 için test etme.

Tekli çalışmaya oturum için Azure AD Microsoft Azure Active Directory çoklu oturum açma JIRA 5.2 için karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve Microsoft Azure Active Directory çoklu oturum açma JIRA 5.2 için ilgili kullanıcı arasındaki bağlantıyı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma ile Microsoft Azure Active Directory çoklu oturum açma JIRA 5.2 için test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir Microsoft Azure Active Directory tek oturum açma için JIRA 5.2 test kullanıcısı oluşturma](#create-a-singlesign-onforjira5.2-test-user)**  - bir Britta Simon karşılık gelen Microsoft Azure Active Directory tek oturum açma için kullanıcı Azure AD gösterimini bağlı JIRA 5.2 içinde sağlamak için .
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, Microsoft Azure Active Directory çoklu oturum açma JIRA 5.2 uygulama için yapılandırın.

**Microsoft Azure Active Directory tek oturum açma ile JIRA 5.2 için Azure AD çoklu oturum açma yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Microsoft Azure Active Directory çoklu oturum açma JIRA 5.2 için** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](.\media\active-directory-saas-msaadssojira5.2-tutorial\tutorial_singlesign-onforjira5.2_samlbase.png)

3. Üzerinde **Microsoft Azure Active Directory çoklu oturum açma JIRA 5.2 etki alanı ve URL'ler için** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Microsoft Azure Active Directory çoklu oturum açma JIRA 5.2 etki alanı ve URL'leri tek oturum açma bilgileri](.\media\active-directory-saas-msaadssojira5.2-tutorial\tutorial_singlesign-onforjira5.2_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<domain:port>/plugins/servlet/saml/auth`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<domain:port>/`

    c. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<domain:port>/plugins/servlet/saml/auth`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler, gerçek tanımlayıcı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Adlandırılmış bir URL olması durumunda bağlantı noktası isteğe bağlıdır. Öğreticide daha sonra açıklanan Jira eklentisi yapılandırma sırasında bu değerleri alma.

4. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesini tıklatın **uygulama Federasyon meta veri URL'sini** ve Not Defteri'ne yapıştırın.
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-msaadssojira5.2-tutorial/tutorial_metadataurl.png)
     
5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](.\media\active-directory-saas-msaadssojira5.2-tutorial\tutorial_general_400.png)

6. Farklı web tarayıcısı penceresinde JIRA Örneğiniz için yönetici olarak oturum açın.

7. Dişlisine üzerine gelin ve tıklatın **eklentileri**.
    
    ![Çoklu oturum açmayı yapılandırın](.\media\active-directory-saas-msaadssojira5.2-tutorial\addon1.png)

8. Eklentiler sekmesi bölümü altında tıklatın **eklentileri yönetme**.

    ![Çoklu oturum açmayı yapılandırın](.\media\active-directory-saas-msaadssojira5.2-tutorial\addon7.png)

9. Gelen eklentiyi karşıdan [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=56521). Microsoft tarafından sağlanan eklentisini el ile karşıya **karşıya eklenti** menüsü. Eklenti yükleme altında ele [Microsoft hizmet sözleşmesi](https://www.microsoft.com/en-us/servicesagreement/).

    ![Çoklu oturum açmayı yapılandırın](.\media\active-directory-saas-msaadssojira5.2-tutorial\addon12.png)

10. Eklenti yüklendikten sonra görünür **kullanıcı yüklü** eklentileri bölümü. Tıklatın **yapılandırma** yeni eklenti yapılandırmak için.

    ![Çoklu oturum açmayı yapılandırın](.\media\active-directory-saas-msaadssojira5.2-tutorial\addon13.png)

11. Yapılandırma sayfasında şu adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](.\media\active-directory-saas-msaadssojira5.2-tutorial\addon52.png)

    > [!TIP]
    > Yani hata meta verileri çözümlenmesinde uygulamayı karşı eşlenen yalnızca bir sertifika olduğundan emin olun. Meta Veri Çözümleme sırasında birden fazla sertifika varsa yönetici bir hata alır.

    a. İçinde **meta veri URL'sini** metin kutusuna, Yapıştır **uygulama Federasyon meta veri URL'sini** Azure portal ve tıklatın kopyaladığınız değeri **gidermek** düğmesi. IDP meta veri URL'sini okur ve tüm alanları bilgileri doldurur.

    b. Kopya **tanımlayıcı, yanıt URL'si ve oturum açma URL'si** değerleri ve bunları yapıştırma **tanımlayıcı, yanıt URL'si ve oturum açma URL'si** kutularındaki sırasıyla söz **Microsoft Azure Active Directory çoklu oturum açma JIRA 5.2 etki alanı ve URL'ler için** Azure Portal'daki bölümü.

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

    ![Azure Active Directory düğmesi](.\media\active-directory-saas-msaadssojira5.2-tutorial\create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](.\media\active-directory-saas-msaadssojira5.2-tutorial\create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](.\media\active-directory-saas-msaadssojira5.2-tutorial\create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](.\media\active-directory-saas-msaadssojira5.2-tutorial\create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-microsoft-azure-active-directory-single-sign-on-for-jira-52-test-user"></a>Bir Microsoft Azure Active Directory tek oturum açma için JIRA 5.2 test kullanıcısı oluşturma

JIRA şirket içi sunucuya oturum açmak Azure AD kullanıcıları etkinleştirmek için bunların içine Microsoft Azure Active Directory çoklu oturum açma JIRA 5.2 için hazırlanması gerekir. Microsoft Azure Active Directory çoklu oturum açma için JIRA 5.2 için sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. JIRA şirket içi sunucunuza yönetici olarak oturum açın.

2. Dişlisine üzerine gelin ve tıklatın **kullanıcı yönetimi**.

    ![Çalışanı ekleyin](.\media\active-directory-saas-msaadssojira5.2-tutorial\user1.png) 

3. Girmek için yönetici erişimi sayfasına yönlendirilirsiniz **parola** tıklatıp **Onayla** düğmesi.

    ![Çalışanı ekleyin](.\media\active-directory-saas-msaadssojira5.2-tutorial\user2.png) 

4. Altında **kullanıcı yönetimi** bölüm sekmesini tıklatın, **kullanıcı oluşturma**.

    ![Çalışanı ekleyin](.\media\active-directory-saas-msaadssojira5.2-tutorial\user3.png) 

5. Üzerinde **"Yeni kullanıcı oluştur"** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çalışanı ekleyin](.\media\active-directory-saas-msaadssojira5.2-tutorial\user4.png) 

    a. İçinde **e-posta adresi** metin kutusuna, kullanıcının e-posta adresi türü ister Brittasimon@contoso.com.

    b. İçinde **tam adı** metin kutusuna, Britta Simon gibi kullanıcının tam adını yazın.

    c. İçinde **kullanıcıadı** metin kutusu, kullanıcı e-posta türünü ister Brittasimon@contoso.com.

    d. İçinde **parola** metin kutusuna, kullanıcının parolasını yazın.

    e. Tıklatın **kullanıcı oluşturma**.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Microsoft Azure Active Directory çoklu oturum açma JIRA 5.2 için erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Microsoft Azure Active Directory tek oturum açma için JIRA 5.2 için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Microsoft Azure Active Directory çoklu oturum açma JIRA 5.2 için**.

    ![Microsoft Azure Active Directory çoklu oturum açma uygulamalar listesini JIRA 5.2 bağlantısı için](.\media\active-directory-saas-msaadssojira5.2-tutorial\tutorial_singlesign-onforjira5.2_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Microsoft Azure Active Directory tek oturum açma için erişim paneli JIRA 5.2 parçasında tıklattığınızda, otomatik olarak, Microsoft Azure Active Directory çoklu oturum açma için JIRA 5.2 uygulama için açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: .\media\active-directory-saas-msaadssojira5.2-tutorial\tutorial_general_01.png
[2]: .\media\active-directory-saas-msaadssojira5.2-tutorial\tutorial_general_02.png
[3]: .\media\active-directory-saas-msaadssojira5.2-tutorial\tutorial_general_03.png
[4]: .\media\active-directory-saas-msaadssojira5.2-tutorial\tutorial_general_04.png

[100]: .\media\active-directory-saas-msaadssojira5.2-tutorial\tutorial_general_100.png

[200]: .\media\active-directory-saas-msaadssojira5.2-tutorial\tutorial_general_200.png
[201]: .\media\active-directory-saas-msaadssojira5.2-tutorial\tutorial_general_201.png
[202]: .\media\active-directory-saas-msaadssojira5.2-tutorial\tutorial_general_202.png
[203]: .\media\active-directory-saas-msaadssojira5.2-tutorial\tutorial_general_203.png
