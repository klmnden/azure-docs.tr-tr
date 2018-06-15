---
title: 'Öğretici: Azure Active Directory Tümleştirme SAP bulut platformu kimlik doğrulama ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve SAP bulut platformu kimlik doğrulama arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 1c1320d1-7ba4-4b5f-926f-4996b44d9b5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2018
ms.author: jeedes
ms.openlocfilehash: e7cc1df5e550dec62869c2a6f68cdc2a84167142
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34352421"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-platform-identity-authentication"></a>Öğretici: SAP bulut platformu kimlik doğrulama Azure Active Directory Tümleştirme

Bu öğreticide, SAP bulut platformu kimlik doğrulaması Azure Active Directory (Azure AD) ile tümleştirme öğrenin. SAP bulut platformu kimlik doğrulama proxy'si olarak IDP ana IDP Azure AD kullanan SAP uygulamaları erişmek için kullanılır.

SAP bulut platformu kimlik doğrulaması Azure AD ile tümleştirdiğinizde, aşağıdaki faydaları alın:

- SAP uygulamalara erişimi, Azure AD'de kontrol edebilirsiniz.
- Kullanıcılarınızın Azure AD hesaplarına uygulamalarla SAP otomatik olarak oturum açmak etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda--Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

SAP bulut platformu kimlik doğrulaması ile Azure AD tümleştirme yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD abonelik.
- Bir tek oturum üzerindeki etkin olmayan abonelik için SAP bulut platformu kimlik doğrulama.

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamında kullanmanızı öneririz yok.

Bu öğreticide adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa [bir aylık ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden SAP bulut platformu kimlik doğrulama ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

Teknik ayrıntılara hemen önce bakmak oluşturacağız kavramları anlamanız önemlidir. SAP bulut platformu kimlik doğrulama ve Active Directory Federasyon Hizmetleri, uygulamalar veya SAP uygulamalarla (bir IDP) olarak Azure AD tarafından korunan Hizmetleri ve SAP bulut tarafından korunan Hizmetleri üzerinden SSO uygulamanızı etkinleştirme Platform kimlik doğrulama.

Şu anda, SAP bulut platformu kimlik doğrulama SAP uygulamaları için bir Proxy Kimlik sağlayıcısı gibi davranır. Azure Active Directory sırayla bu kurulumunda başında kimlik sağlayıcısı gibi davranır. 

Aşağıdaki diyagram bu ilişkiyi gösterir:

![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-sapcloudauth-tutorial/architecture-01.png)

Bu kurulum ile SAP bulut platformu kimlik doğrulama kiracınızı Azure Active Directory'de güvenilir bir uygulama olarak yapılandırılır. 

Daha sonra tüm SAP uygulamaları ve bu şekilde korumak istediğiniz hizmetleri SAP bulut platformu kimlik doğrulama Yönetimi konsolunda yapılandırılır.

Bu nedenle, SAP uygulamaları ve hizmetlerine erişim izni verme için yetkilendirme SAP bulut platformu kimlik doğrulama (aksine, Azure Active Directory) içinde gerçekleşmesi gerekir.

Azure Active Directory Marketi üzerinden bir uygulama olarak SAP bulut platformu kimlik doğrulama yapılandırarak, bireysel talepler veya SAML onaylar yapılandırmanız gerekmez.

>[!NOTE] 
>Şu anda yalnızca Web SSO hem taraflar tarafından test edilmiştir. Uygulama API veya API API iletişimi için gerekli olan akışlar çalışması gereken ancak henüz test edilmemiştir. Bunlar bir sonraki etkinlik sırasında test edilir.
>

## <a name="add-sap-cloud-platform-identity-authentication-from-the-gallery"></a>Galeriden SAP bulut platformu kimlik doğrulama ekleme
Azure AD'de SAP bulut platformu kimlik doğrulaması tümleştirmesini yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden SAP bulut platformu kimlik doğrulama eklemeniz gerekir.

**SAP bulut platformu kimlik doğrulama Galeriden eklemek için aşağıdaki adımları uygulayın:**

1. İçinde [Azure portal](https://portal.azure.com), sol gezinti panelinde seçin **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Git **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üstündeki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **SAP bulut platformu kimlik doğrulama**. 

5. Seçin **SAP bulut platformu kimlik doğrulama** sonuçlar paneli ve ardından **Ekle** düğmesi.

    ![Sonuçlar listesinde SAP bulut platformu kimlik doğrulama](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma SAP bulut platformu kimlik doğrulama ile test etme. Yapılandırma ve "Britta Simon." olarak adlandırılan bir test kullanıcısı ile test etme

Tekli çalışmaya oturum için Azure AD SAP bulut platformu kimlik doğrulama karşılık gelen kullanıcının kim olduğunu bilmek ister. Diğer bir deyişle, SAP bulut platformu kimlik doğrulaması bir Azure AD kullanıcısının ve ilgili kullanıcı arasında bir bağlantı oluşturmanız gerekir.

SAP bulut platformu kimlik doğrulama içinde değere vermek **kullanıcıadı** aynı değer olarak **kullanıcı adı** Azure AD'de. Şimdi iki kullanıcılar arasında bağlantı kurulduktan.

Yapılandırma ve Azure AD çoklu oturum açma SAP bulut platformu kimlik doğrulama ile test etmek için aşağıdaki yapı taşları tamamlayın:

1. [Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on) bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. [Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user) Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. [Bir SAP bulut platformu kimlik doğrulama test kullanıcısı oluşturma](#create-an-sap-cloud-platform-identity-authentication-test-user) Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı SAP bulut platformu kimlik doğrulama sağlamak için.
4. [Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user) Britta Azure AD çoklu oturum açma kullanmak Simon etkinleştirmek için.
5. [Test çoklu oturum açma](#test-single-sign-on) yapılandırma çalıştığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma SAP bulut platformu kimlik doğrulama uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma SAP bulut platformu kimlik doğrulama ile yapılandırmak için aşağıdaki adımları uygulayın:**

1. Azure portalında üzerinde **SAP bulut platformu kimlik doğrulama** uygulama tümleştirmesi sayfasında, **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. İçinde **çoklu oturum açma** iletişim kutusunda **SAML tabanlı oturum açma**seçin **modu** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_samlbase.png)

3. Uygulamada yapılandırmak istiyorsanız, **IDP** modunda başlatılan **SAP bulut Platform kimliği kimlik doğrulaması etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:  

    ![SAP bulut Platform kimliği kimlik doğrulama etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_url.png)

    a. İçinde **tanımlayıcısı** kutusunda, aşağıdaki desende bir URL yazın: `<IAS-tenant-id>.accounts.ondemand.com`

    b. İçinde **yanıt URL'si** kutusunda, aşağıdaki desende bir URL yazın: `https://<IAS-tenant-id>.accounts.ondemand.com/saml2/idp/acs/<IAS-tenant-id>.accounts.ondemand.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. Kişi [SAP bulut Platform kimliği kimlik doğrulaması istemci destek ekibi](https://cloudplatform.sap.com/capabilities/security/trustcenter.html) bu değerleri almak için. Tanımlayıcı değeri anlamadığınız, SAP bulut platformu kimlik doğrulama belgeleri hakkında okuyun [Kiracı SAML 2.0 yapılandırma](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html).

4. Uygulamada yapılandırmak istiyorsanız, **SP** başlatılan modu, select **Göster Gelişmiş URL ayarları**.

    ![SAP bulut Platform kimliği kimlik doğrulama etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_url1.png)

    İçinde **oturum üzerinde URL'si** kutusunda, aşağıdaki desende bir URL yazın: `{YOUR BUSINESS APPLICATION URL}`.

    > [!NOTE]
    > Bu değer gerçek değil. Bu değer gerçek oturum açma URL'si ile güncelleştirin. Oturum açma, belirli iş uygulama URL'sini kullanın. Kişi [SAP bulut Platform kimliği kimlik doğrulaması istemci destek ekibi](https://cloudplatform.sap.com/capabilities/security/trustcenter.html) tüm şüpheli varsa.

5. İçinde **SAML imzalama sertifikası** bölümünde, select **meta veri XML**. Ardından meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_certificate.png)

6. SAP bulut platformu kimlik doğrulama uygulaması SAML onaylar belirli bir biçimde bekliyor. Bu öznitelik değerleri yönetme **kullanıcı öznitelikleri** bölümü uygulama tümleştirmesi sayfasında. Aşağıdaki ekran görüntüsünde biçimi örneği gösterir. 

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sapcloudauth-tutorial/attribute.png)

7. SAP uygulamanız bir öznitelik gibi görüyorsa **firstName**, ekleme **firstName** özniteliğini **kullanıcı öznitelikleri** bölümü. Bu seçenek kullanılabilir **çoklu oturum açma** iletişim kutusunun **SAML belirteci öznitelikleri** iletişim kutusu...

    a. Açmak için **özniteliği eklemek** iletişim kutusunda **Ekle özniteliği**. 
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_attribute_04.png)
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** öznitelik adı yazın **firstName**.
    
    c. Gelen **değeri** listesinde, öznitelik değeri seçin **user.givenname**.
    
    d. Seçin **Tamam**.

8. **Kaydet** düğmesini seçin.

    ![Çoklu oturum açma düğmesi kaydetme yapılandırın](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_400.png)

9. İçinde **SAP bulut Platform kimliği kimlik doğrulama Yapılandırması** bölümünde, select **SAP bulut platformu kimlik doğrulama yapılandırma** açmak için **oturum açmaYapılandırma**penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![SAP bulut Platform kimliği kimlik doğrulama yapılandırması](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_configure.png) 

10. Uygulamanız için yapılandırılmış SSO almak için SAP bulut platformu kimlik doğrulama yönetim konsoluna gidin. URL aşağıdaki deseni vardır: `https://<tenant-id>.accounts.ondemand.com/admin`. SAP bulut platformu kimlik doğrulaması sırasında ilgili belgelere okuma [Microsoft Azure AD ile tümleştirme](https://help.hana.ondemand.com/cloud_identity/frameset.htm?626b17331b4d4014b8790d3aea70b240.html). 

11. Azure portalında seçin **kaydetmek** düğmesi.

12. Yalnızca ekleyin ve başka bir SAP uygulama için SSO'yu etkinleştirmek isterseniz, aşağıdaki ile devam edin. Bölümü altındaki adımları yineleyin **galerisinden SAP bulut platformu kimlik doğrulama ekleme**.

13. Azure portalında üzerinde **SAP bulut platformu kimlik doğrulama** uygulama tümleştirmesi sayfasında, **bağlantılı oturum açma**.

    ![Bağlantılı oturum açma özelliğini yapılandırın](./media/active-directory-saas-sapcloudauth-tutorial/linked_sign_on.png)

14. Yapılandırmayı kaydedin.

>[!NOTE] 
>Yeni uygulama tek oturum açma yapılandırma önceki SAP uygulamanın yararlanır. SAP bulut platformu kimlik doğrulama yönetim konsolunda aynı Kurumsal kimlik sağlayıcıları kullandığınızdan emin olun.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com) uygulaması kuruluyor sırada!  Bu uygulamadan ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölümünde, select **çoklu oturum açma** sekmesinde ve katıştırılmış erişim belgeleri etraflıca **yapılandırma** alt bölüm. Daha fazla bilgiyi embedded belgeler özelliği hakkında [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları uygulayın:**

1. Azure portalında sol bölmede seçin **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-sapcloudauth-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-sapcloudauth-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusunda **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-sapcloudauth-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları uygulayın:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-sapcloudauth-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’u seçin.
 
### <a name="create-an-sap-cloud-platform-identity-authentication-test-user"></a>Bir SAP bulut platformu kimlik doğrulama test kullanıcısı oluşturma

SAP bulut platformu kimlik doğrulaması bir kullanıcı oluşturmanız gerekmez. Azure AD kullanıcı deposunda kullanıcılar SSO işlevini kullanabilirsiniz.

SAP bulut platformu kimlik doğrulama Kimlik Federasyonu seçeneğini destekler. Bu seçenek, Kurumsal kimlik sağlayıcısı tarafından kimliği doğrulanmış kullanıcılar kullanıcı deposu, SAP bulut platformu kimlik doğrulama içinde olup olmadığını denetlemek için uygulamanın sağlar. 

Kimlik Federasyonu seçeneği varsayılan olarak devre dışıdır. Kimlik Federasyonu etkinleştirilirse, SAP bulut platformu kimlik doğrulama alınan kullanıcı uygulamasına erişebilir. 

Etkinleştirmek veya SAP bulut platformu kimlik doğrulaması ile Kimlik Federasyonu devre dışı bırakma hakkında daha fazla bilgi için "Etkinleştirme kimlik Federasyon ile SAP bulut platformu kimlik doğrulama" bölümüne bakın. [ile Kimlik Federasyonu yapılandırma Kullanıcı deposu, SAP bulut platformu kimlik doğrulama](https://help.hana.ondemand.com/cloud_identity/frameset.htm?c029bbbaefbf4350af15115396ba14e2.html).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, SAP bulut platformu kimlik doğrulaması için erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**SAP bulut platformu kimlik doğrulama için Britta Simon atamak için aşağıdaki adımları uygulayın:**

1. Azure Portalı'ndaki uygulamaları görünümünü açın ve dizin görünümüne gidin. Ardından, Git **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **SAP bulut platformu kimlik doğrulama**.

    ![Uygulamalar listesinde SAP bulut platformu kimlik doğrulama bağlantı](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_app.png)  

3. Soldaki menüde seçin **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Seçin **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** içinde **eklemek atama** iletişim kutusu.

    ![Ekleme atama bölmesi][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim kutusu.

7. Seçin **atamak** düğmesini **eklemek atama** iletişim kutusu.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

SAP bulut platformu kimlik doğrulama döşeme erişim panelinde seçtiğinizde, SAP bulut platformu kimlik doğrulama uygulamanıza oturumunuz otomatik olarak.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)

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
