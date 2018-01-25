---
title: "Öğretici: Azure Active Directory Tümleştirme Confluence SAML SSO Microsoft tarafından | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve Microsoft tarafından Confluence SAML SSO arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 1ad1cf90-52bc-4b71-ab2b-9a5a1280fb2d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2018
ms.author: jeedes
ms.openlocfilehash: 22189c3d2d2164ba0fa3c2d790c36361fb0f5854
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="tutorial-azure-active-directory-integration-with-confluence-saml-sso-by-microsoft"></a>Öğretici: Microsoft tarafından Confluence SAML SSO Azure Active Directory Tümleştirme

Bu öğreticide, Microsoft tarafından Confluence SAML SSO Azure Active Directory (Azure AD) ile tümleştirme öğrenin.

Microsoft tarafından Confluence SAML SSO Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Microsoft tarafından Confluence SAML SSO erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak Confluence SAML SSO (çoklu oturum açma) Microsoft tarafından açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="description"></a>Açıklama:

Microsoft Azure Active Directory hesabınıza Atlassian Confluence sunucusuyla çoklu oturum açmayı etkinleştirmek için kullanın. Bu şekilde tüm kuruluş kullanıcılarınızın Azure AD kimlik Confluence uygulamasına oturum açmak için kullanabilirsiniz. Bu eklenti, Federasyon için SAML 2.0 kullanır.

## <a name="prerequisites"></a>Önkoşullar

Microsoft tarafından Confluence SAML SSO ile Azure AD tümleştirme yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Windows 64-bit sunucuda (şirket içi veya Bulut Iaas altyapı) yüklü confluence sunucu uygulaması
- HTTPS etkinleştirilmiş confluence sunucusudur
- Aşağıdaki bölümüne Confluence eklentisi için desteklenen sürümleri bahsedilen unutmayın.
- Confluence sunucu kimlik doğrulaması için Azure AD oturum açma sayfasına özellikle Internet üzerinden erişilebildiğinden ve Azure AD'den belirteç alamaz gerekir
- Yönetici kimlik bilgileri Confluence ayarlanır
- WebSudo Confluence içinde devre dışı bırakıldı
- Confluence sunucu uygulamasından oluşturulan test kullanıcısı

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamında Confluence birini kullanarak önermiyoruz. Geliştirme veya uygulamanın hazırlama ortamında tümleştirme önce test ve üretim ortamına kullanın.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, burada bir aylık deneme elde edebilirsiniz: [deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="supported-versions-of-confluence"></a>Confluence desteklenen sürümleri 

Şimdi itibariyle Confluence aşağıdaki sürümleri desteklenir:

- Confluence: 5.0 5.10

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Microsoft tarafından Confluence SAML SSO Galeriden ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-confluence-saml-sso-by-microsoft-from-the-gallery"></a>Microsoft tarafından Confluence SAML SSO Galeriden ekleme
Azure AD Confluence SAML SSO Microsoft tarafından tümleştirilmesi yapılandırmak için Microsoft tarafından Confluence SAML SSO Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Microsoft tarafından Confluence SAML SSO Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Confluence SAML SSO Microsoft tarafından**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_search.png)

5. Sonuçlar panelinde seçin **Confluence SAML SSO Microsoft tarafından**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Confluence SAML SSO "Britta Simon" adlı bir test kullanıcıyı temel alarak Microsoft tarafından test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Confluence SAML SSO Microsoft tarafından bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve Microsoft tarafından Confluence SAML SSO ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Confluence SAML SSO Microsoft tarafından test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Microsoft test kullanıcı tarafından Confluence SAML SSO oluşturma](#creating-a-confluence-saml-sso-by-microsoft-test-user)**  - Britta Simon, karşılık gelen Confluence SAML SSO kullanıcı Azure AD gösterimini bağlantılı Microsoft tarafından sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Microsoft uygulaması tarafından Confluence SAML SSO yapılandırın.

**Microsoft tarafından Confluence SAML SSO ile Azure AD çoklu oturum açma yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Confluence SAML SSO Microsoft tarafından** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_samlbase.png)

3. Üzerinde **Confluence SAML SSO Microsoft Domain ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<domain:port>/plugins/servlet/saml/auth`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<domain:port>/`

    c. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<domain:port>/plugins/servlet/saml/auth`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler, gerçek tanımlayıcı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Adlandırılmış bir URL olması durumunda bağlantı noktası isteğe bağlıdır. Öğreticide daha sonra açıklanan Confluence eklentisi yapılandırma sırasında bu değerleri alma.
 

4. Oluşturulacak **meta veri** url, aşağıdaki adımları gerçekleştirin:

    a. Tıklatın **uygulama kayıtlar**.
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-Confluencemicrosoft-tutorial/appregistrations.png)
   
    b. Tıklatın **uç noktaları** açmak için **uç noktaları** iletişim kutusu.  
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-Confluencemicrosoft-tutorial/endpointicon.png)

    c. Kopyalamak için Kopyala düğmesini tıklatın **FEDERASYON meta veri belgesi** URL'yi kopyalayıp Not Defteri'ne yapıştırın.
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-Confluencemicrosoft-tutorial/endpoint.png)
     
    d. Şimdi özellik sayfasına gidin **Confluence SAML SSO Microsoft tarafından** ve kopyalama **uygulama kimliği** kullanarak **kopyalama** düğmesine tıklayın ve Not Defteri'ne yapıştırın.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-Confluencemicrosoft-tutorial/appid.png)

    e. Oluştur **meta veri URL'sini** şu biçimi kullanarak: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>` ve daha sonra eklentiyi yapılandırma için kullanılan bu değer Not Defteri'nde kopyalayın.

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-Confluencemicrosoft-tutorial/tutorial_general_400.png)

6. Farklı web tarayıcısı penceresinde Confluence Örneğiniz için yönetici olarak oturum açın.

7. Dişlisine üzerine gelin ve tıklatın **eklentileri**.
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon1.png)

8. Gelen eklentiyi karşıdan [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=56503). Microsoft tarafından sağlanan eklentisini el ile karşıya **karşıya eklenti** menüsü
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon12.png)

9. Eklenti yüklendikten sonra görünür **kullanıcı yüklü** eklentileri bölümünü **yönetmek eklenti** bölümü. Tıklatın **yapılandırma** yeni eklenti yapılandırmak için.
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon13.png)

10. Yapılandırma sayfasında şu adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon52.png)
 
    > [!TIP]
    > Yani hata meta verileri çözümlenmesinde uygulamayı karşı eşlenen yalnızca bir sertifika olduğundan emin olun. Birden fazla sertifika varsa, yönetim meta veri çözümleme sırasında bir hata alır.

    a. İçinde **meta veri URL'sini** Yapıştır **meta veri URL'sini** Azure AD'den oluşturulur ve tıklatın **gidermek** düğmesi. IDP meta veri URL'sini okur ve tüm alanları bilgileri doldurur.

    b. Kopya **tanımlayıcı, yanıt URL'si ve oturum açma URL'si** değerleri ve bunları yapıştırın **tanımlayıcı, yanıt URL'si ve oturum açma URL'si** kutularındaki sırasıyla söz **Confluence SAML SSO Microsoft Domain ve URL'leri**  Azure Portal'daki bölümü.

    c. İçinde **oturum açma düğmesi adı** , kuruluşunuzda kullanıcıların oturum açma ekranında düğmesinin adını yazın.

    d. İçinde **SAML kullanıcı kimliği konumları**, şunlardan birini seçin **kullanıcı kimliğidir konu deyimi NameIdentifier öğesinde** veya **kullanıcı kimliği olan bir öznitelik öğedeki**.  Bu kimliği Confluence kullanıcı kimliği olması gerekir. Kullanıcı Kimliği eşleşmiyorsa, ardından sistemi oturum açmak kullanıcılar izin vermez. 

    > [!Note]
    > Varsayılan SAML kullanıcı kimliği konumu ad tanımlayıcısıdır. Bu öznitelik seçeneği ile değiştirin ve uygun öznitelik adını girin.
    
    e. Seçerseniz **kullanıcı kimliği olan bir öznitelik öğedeki** seçeneği, ardından **öznitelik adı** metin kullanıcı kimliği beklenirken özniteliğin adını yazın. 

    f. Azure AD ile Federasyon etki alanını (örneğin, ADFS vb.) kullanıyorsanız, tıklayın **giriş bölgesi bulmayı etkinleştirmek** seçeneği ve yapılandırma **etki alanı adı**.
    
    g. İçinde **etki alanı adı** ADFS tabanlı oturum açma durumunda burada etki alanı adını yazın.

    h. Denetleme **etkinleştirmek tek oturum kapatma** bir kullanıcı oturum açtığında Confluence Azure AD'den oturumunuzu etmek istiyor. 

    i. ' I tıklatın **kaydetmek** düğmesini tıklatarak ayarları kaydedin.


> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-confluence-saml-sso-by-microsoft-test-user"></a>Microsoft test kullanıcı tarafından Confluence SAML SSO oluşturma

Confluence için şirket içi sunucuda oturum açmak Azure AD kullanıcıları etkinleştirmek için bunların Confluence SAML SSO Microsoft tarafından sağlanmalıdır. Microsoft tarafından Confluence SAML SSO için sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Şirket içi sunucuda, Confluence yönetici olarak oturum açın.

2. Dişlisine üzerine gelin ve tıklatın **kullanıcı yönetimi**.

    ![Çalışanı ekleyin](./media/active-directory-saas-confluencemicrosoft-tutorial/user1.png) 

3. Kullanıcılar bölümü altında tıklatın **kullanıcıları eklemek** sekmesi. Üzerinde **kullanıcı ekleme** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çalışanı ekleyin](./media/active-directory-saas-confluencemicrosoft-tutorial/user2.png) 

    a. İçinde **kullanıcıadı** metin kutusu, kullanıcı Britta Simon gibi e-posta yazın.

    b. İçinde **tam adı** metin kutusuna, Britta Simon gibi kullanıcının tam adını yazın.

    c. İçinde **e-posta** metin kutusuna, kullanıcının e-posta adresi türü ister Brittasimon@contoso.com.

    d. İçinde **parola** metin kutusuna, Britta Simon için parolayı yazın.

    e. Tıklatın **parolayı onayla** parolayı yeniden girin.
    
    f. Tıklatın **Ekle** düğmesi.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Microsoft tarafından Confluence SAML SSO için erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı atama][200] 

**Microsoft tarafından Confluence SAML SSO Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Confluence SAML SSO Microsoft tarafından**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Microsoft parçasında tarafından Confluence SAML SSO tıklattığınızda, otomatik olarak, Confluence SAML SSO Microsoft uygulaması tarafından açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_203.png

