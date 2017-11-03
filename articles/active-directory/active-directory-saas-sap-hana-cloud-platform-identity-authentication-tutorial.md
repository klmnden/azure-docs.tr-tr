---
title: "Öğretici: Azure Active Directory Tümleştirme SAP bulut platformu kimlik doğrulama ile | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve SAP bulut platformu kimlik doğrulama arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 1c1320d1-7ba4-4b5f-926f-4996b44d9b5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/20/2017
ms.author: jeedes
ms.openlocfilehash: e4e7c7273acc216ae82a52b3e6dcd530a6ad1bd7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-platform-identity-authentication"></a>Öğretici: SAP bulut platformu kimlik doğrulama Azure Active Directory Tümleştirme

Bu öğreticide, SAP bulut platformu kimlik doğrulaması Azure Active Directory (Azure AD) ile tümleştirme öğrenin. SAP bulut platformu kimlik doğrulama, bir proxy sunucu olarak IDP ana IDP Azure AD kullanarak SAP uygulamalarına erişmek için kullanılır.

SAP bulut platformu kimlik doğrulaması Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- SAP uygulama erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak uygulamaları çoklu oturum açma (SSO) SAP açan kullanıcılarınıza etkinleştirebilirsiniz) Azure AD hesaplarına sahip.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

SAP bulut platformu kimlik doğrulaması ile Azure AD tümleştirme yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir SAP bulut platformu kimlik doğrulaması çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden SAP bulut platformu kimlik doğrulama ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

Teknik ayrıntılara girmeden önce bakmak oluşturacağız kavramları anlamanız önemlidir. SAP bulut platformu kimlik doğrulaması ve Azure Active Directory Federasyon uygulamalar veya SAP uygulamaları ve Hizmetleri SAP bulut Platform kimliği tarafından korunan ile AAD (olarak bir IDP) tarafından korunan hizmetler arasında SSO uygulamak sağlar Kimlik doğrulaması.

Şu anda, SAP bulut platformu kimlik doğrulama SAP uygulamaları için bir Proxy Kimlik sağlayıcısı gibi davranır. Azure Active Directory sırayla bu kurulumunda başında kimlik sağlayıcısı gibi davranır. 

Aşağıdaki diyagram bu gösterir:

![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-sapcloudauth-tutorial/architecture-01.png)

Bu kurulum ile SAP bulut platformu kimlik doğrulama kiracınızı Azure Active Directory'de güvenilir bir uygulama olarak yapılandırılır. 

Daha sonra tüm SAP uygulamalar ve hizmetler bu şekilde korumak istediğiniz SAP bulut platformu kimlik doğrulama yönetim konsolunda yapılandırılan!

Başka bir deyişle, SAP uygulamaları ve hizmetlerine erişim izni verme için yetkilendirme (yetkilendirme Azure Active Directory'de yapılandırma) aksine Kurulum SAP bulut platformu kimlik doğrulama yerinde yapması gerekmez.

Azure Active Directory Marketi üzerinden bir uygulama olarak SAP bulut platformu kimlik doğrulama yapılandırarak, gerekli bireysel talepler yapılandırmayı dikkatli gerekmez / SAML onaylar ve dönüşümleri gerekli geçerli bir oluşturmak SAP uygulamaları için kimlik doğrulama belirteci.

>[!NOTE] 
>Şu anda Web SSO her iki taraf tarafından yalnızca test edilmiştir. Uygulama API veya API API iletişimi için gerekli akışları çalışması gerekir, ancak, henüz test edilmemiştir. Bunlar bir sonraki etkinliklere bir parçası olarak test edilir.
>

## <a name="adding-sap-cloud-platform-identity-authentication-from-the-gallery"></a>Galeriden SAP bulut platformu kimlik doğrulama ekleme
Azure AD SAP bulut platformu kimlik doğrulama tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden SAP bulut platformu kimlik doğrulama eklemeniz gerekir.

**SAP bulut platformu kimlik doğrulama Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **SAP bulut platformu kimlik doğrulama**seçin **SAP bulut platformu kimlik doğrulama** sonuç panelinden ardından **Ekle** düğmesi uygulama ekleyin.

    ![Sonuçlar listesinde SAP bulut platformu kimlik doğrulama](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma SAP bulut Platform kimliği "Britta Simon" adlı bir test kullanıcı tabanlı kimlik doğrulaması ile test etme.

Tekli çalışmaya oturum için Azure AD karşılık gelen kullanıcı SAP bulut platformu kimlik doğrulama için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının SAP bulut platformu kimlik doğrulama ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

SAP bulut platformu kimlik doğrulama içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma SAP bulut platformu kimlik doğrulama ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir SAP bulut platformu kimlik doğrulama test kullanıcısı oluşturma](#create-an-sap-cloud-platform-identity-authentication-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı SAP bulut platformu kimlik doğrulama sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma SAP bulut platformu kimlik doğrulama uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma SAP bulut platformu kimlik doğrulama ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **SAP bulut platformu kimlik doğrulama** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_samlbase.png)

3. Üzerinde **SAP bulut Platform kimliği kimlik doğrulaması etki alanı ve URL'leri** uygulamada yapılandırmak istiyorsanız, bölüm **IDP** modu tarafından başlatılan:

    ![SAP bulut Platform kimliği kimlik doğrulama etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_url.png)

    İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<entity-id>.accounts.ondemand.com`

    > [!NOTE] 
    > Bu değer gerçek değil. Bu değer gerçek tanımlayıcısı ile güncelleştirin. Kişi [SAP bulut Platform kimliği kimlik doğrulaması istemci destek ekibi](https://cloudplatform.sap.com/capabilities/security/trustcenter.html) bu değeri alınamıyor. Bu değer bilmiyorsanız, lütfen SAP bulut platformu kimlik doğrulama belgeleri izleyin [Kiracı SAML 2.0 yapılandırma](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html).

4. Denetleme **Göster Gelişmiş URL ayarları**. Uygulamada yapılandırmak istiyorsanız **SP** modu tarafından başlatılan:

    ![SAP bulut Platform kimliği kimlik doğrulama etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_url1.png)

    İçinde **oturum üzerinde URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<entity-id>.accounts.ondemand.com/admin`

    > [!NOTE] 
    > Bu değer gerçek değil. Bu değer gerçek oturum açma URL'si ile güncelleştirin. Kişi [SAP bulut Platform kimliği kimlik doğrulaması istemci destek ekibi](https://cloudplatform.sap.com/capabilities/security/trustcenter.html) bu değeri alınamıyor.

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_certificate.png)

6. SAP bulut platformu kimlik doğrulama uygulaması SAML onaylar belirli bir biçimde bekliyor. Bu öznitelik değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirmesi sayfasında bölüm. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir. 

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sapcloudauth-tutorial/attribute.png)

7. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** SAP uygulamanız bir öznitelik örneğin "firstName" görüyorsa iletişim. SAML belirteci öznitelikleri iletişim kutusunda, "ad" özniteliğini ekleyin.

    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_attribute_04.png)
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** metin kutusuna, öznitelik adı "ad" yazın.
    
    c. Gelen **değeri** listesinde, öznitelik değeri "user.givenname" seçin.
    
    d. **Tamam**’a tıklayın.

8. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_400.png)

9. Üzerinde **SAP bulut Platform kimliği kimlik doğrulama Yapılandırması** 'yi tıklatın **SAP bulut platformu kimlik doğrulama yapılandırma** açmak için **yapılandırma oturum açma** penceresini açın. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![SAP bulut Platform kimliği kimlik doğrulama yapılandırması](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_configure.png) 

10. Uygulamanız için yapılandırılmış SSO almak için SAP bulut Platform kimliği kimlik doğrulama yönetim konsoluna gidin. URL aşağıdaki deseni vardır: `https://<tenant-id>.accounts.ondemand.com/admin`. Ardından, SAP bulut platformu kimlik doğrulama için belgeleri takip [SAP bulut platformu kimlik doğrulaması sırasında Kurumsal kimlik sağlayıcısı yapılandırma Microsoft Azure AD](https://help.hana.ondemand.com/cloud_identity/frameset.htm?626b17331b4d4014b8790d3aea70b240.html). 

11. Azure portalında tıklatın **kaydetmek** düğmesi.

12. Yalnızca ekleyin ve başka bir SAP uygulama için SSO'yu etkinleştirmek isterseniz, aşağıdaki adımları devam edin. "Ekleme SAP bulut platformu kimlik doğrulama galerisinden" bölümü altındaki adımları yineleyin SAP bulut platformu kimlik doğrulama başka bir örneği eklemek için.

13. Azure portalında üzerinde **SAP bulut platformu kimlik doğrulama** uygulama tümleştirme sayfasını tıklatın **bağlantılı oturum açma**.

    ![Bağlantılı oturum açma özelliğini yapılandırın](./media/active-directory-saas-sapcloudauth-tutorial/linked_sign_on.png)

14. Yapılandırmayı kaydedin.

>[!NOTE] 
>Yeni uygulama önceki SAP uygulaması için SSO yapılandırma özelliğinden yararlanır. Lütfen, aynı Kurumsal kimlik sağlayıcıları SAP bulut Platform kimliği kimlik doğrulama yönetim konsolunda kullandığınızdan emin olun.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-sapcloudauth-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-sapcloudauth-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-sapcloudauth-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-sapcloudauth-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**'a tıklayın.
 
### <a name="create-an-sap-cloud-platform-identity-authentication-test-user"></a>Bir SAP bulut platformu kimlik doğrulama test kullanıcısı oluşturma

SAP bulut Platform kimliği kimlik doğrulamasını bir kullanıcı oluşturmanız gerekmez. Azure AD kullanıcı deposunda kullanıcılar SSO işlevini kullanabilirsiniz.

SAP bulut platformu kimlik doğrulama Kimlik Federasyonu seçeneğini destekler. Bu seçenek, Kurumsal kimlik sağlayıcısı tarafından kimliği doğrulanmış kullanıcılar kullanıcı deposu, SAP bulut platformu kimlik doğrulama içinde olup olmadığını denetlemek için uygulamanın sağlar. 

Varsayılan ayar, Kimlik Federasyonu seçenek devre dışıdır. Kimlik Federasyonu etkinleştirilirse, SAP bulut platformu kimlik doğrulama içeri aktarılan kullanıcıların uygulamaya erişmek kullanabilirsiniz. 

SAP bulut platformu kimlik doğrulaması ile Kimlik Federasyonu etkinleştirme etkinleştirmek veya SAP bulut platformu kimlik doğrulaması ile Kimlik Federasyonu devre dışı bırakma hakkında daha fazla bilgi için bkz: [ile Kimlik Federasyonu yapılandırma Kullanıcı deposu, SAP bulut platformu kimlik doğrulama](https://help.hana.ondemand.com/cloud_identity/frameset.htm?c029bbbaefbf4350af15115396ba14e2.html).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, SAP bulut platformu kimlik doğrulaması için erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**SAP bulut platformu kimlik doğrulama için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **SAP bulut platformu kimlik doğrulama**.

    ![Uygulamalar listesinde SAP bulut platformu kimlik doğrulama bağlantı](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli SAP bulut platformu kimlik doğrulama parçasında tıklattığınızda, otomatik olarak SAP bulut platformu kimlik doğrulama uygulamanız açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_203.png

