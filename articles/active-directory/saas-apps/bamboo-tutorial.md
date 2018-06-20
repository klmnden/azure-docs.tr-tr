---
title: 'Öğretici: Azure Active Directory ile tümleştirme Bambu SAML SSO GmbH çözünürlüğün | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile SAML SSO Bambu için arasında GmbH çözünürlüğün yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: f00160c7-f4cc-43bf-af18-f04168d3767c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2017
ms.author: jeedes
ms.openlocfilehash: c92d8167b46b185a55ed915b097ea6238e4bde69
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36216200"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-bamboo-by-resolution-gmbh"></a>Öğretici: Azure Active Directory ile tümleştirme Bambu SAML SSO GmbH çözünürlüğün

Bu öğreticide, Azure Active Directory (Azure AD) ile GmbH çözünürlüğün SAML SSO Bambu için tümleştirme öğrenin.

Azure AD ile GmbH çözünürlüğün SAML SSO Bambu için tümleştirme ile aşağıdaki avantajları sağlar:

- SAML SSO Bambu için GmbH çözünürlüğün erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak Bambu SAML SSO için Azure AD hesaplarına sahip (çoklu oturum açma) GmbH çözünürlüğün açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Çözümleme GmbH tarafından SAML SSO Bambu için Azure AD tümleştirme yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- SAML SSO etkin abonelik GmbH çoklu oturum çözünürlüğün Bambu için

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. SAML SSO Bambu için çözüm GmbH tarafından Galeriden ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-saml-sso-for-bamboo-by-resolution-gmbh-from-the-gallery"></a>SAML SSO Bambu için çözüm GmbH tarafından Galeriden ekleme
SAML SSO Bambu için Azure AD'ye tümleştirme GmbH çözünürlüğün yapılandırmak için SAML SSO Bambu için çözüm GmbH tarafından Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**SAML SSO Bambu için çözüm GmbH tarafından Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna yazın **SAML SSO GmbH çözünürlüğün Bambu için**seçin **SAML SSO GmbH çözünürlüğün Bambu için** sonuç panelinden ardından **Ekle** eklemek için düğmeyi uygulama.

    ![Sonuçlar listesinde GmbH çözünürlüğün Bambu SAML SSO](./media/bamboo-tutorial/tutorial_bamboo_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Bambu SAML SSO GmbH "Britta Simon" adlı bir test kullanıcı tabanlı çözünürlüğün test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen kullanıcı Bambu SAML SSO tarafından çözümleme GmbH Azure AD'de bir kullanıcı için olduğunu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının SAML SSO Bambu için ilgili kullanıcı çözünürlüğün arasında bir bağlantı ilişkisi GmbH kurulması gerekir.

Değeri GmbH çözünürlüğün Bambu için SAML SSO içinde atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırmak ve Azure AD çoklu oturum açma Bambu SAML SSO GmbH çözünürlüğün sınamak için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[SAML SSO çözümleme GmbH test kullanıcısı tarafından Bambu için oluşturma](#create-a-saml-sso-for-bamboo-by-resolution-gmbh-test-user)**  - Britta Simon, karşılık gelen SAML SSO Bambu için kullanıcı Azure AD gösterimini bağlı GmbH çözünürlüğün sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, SAML SSO Bambu için çözüm GmbH uygulama tarafından yapılandırın.

**Azure AD çoklu oturum açma Bambu SAML SSO GmbH çözümleme tarafından yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **SAML SSO GmbH çözünürlüğün Bambu için** uygulama tümleştirmesi sayfasında, tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/bamboo-tutorial/tutorial_bamboo_samlbase.png)

3. Üzerinde **GmbH etki alanı çözünürlüğün Bambu ve URL'ler için SAML SSO** bölümünde, uygulama tarafından başlatılan IDP modunda yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin:

    ![SAML SSO Bambu çözünürlüğün GmbH etki alanı için ve oturum açma URL'leri tek bilgileri](./media/bamboo-tutorial/tutorial_bamboo_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<server-base-url>/plugins/servlet/samlsso`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<server-base-url>/plugins/servlet/samlsso`

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![SAML SSO Bambu çözünürlüğün GmbH etki alanı için ve oturum açma URL'leri tek bilgileri](./media/bamboo-tutorial/tutorial_bamboo_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<server-base-url>/plugins/servlet/samlsso`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler, gerçek tanımlayıcı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Kişi [SAML SSO GmbH istemci çözünürlüğün Bambu için destek ekibi](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bamboo/server/support) bu değerleri almak için. 

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/bamboo-tutorial/tutorial_bamboo_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/bamboo-tutorial/tutorial_general_400.png)

7. SAML SSO çözümleme GmbH şirket site tarafından Bambu için yönetici olarak oturum.

8. Ana araç çubuğunun sağ tarafta tıklatın **ayarları** > **eklentileri**.

    ![Ayarları](./media/bamboo-tutorial/tutorial_bamboo_setings.png)

9. Güvenlik bölümüne gidin, tıklayın **SAML SingleSignOn** Menubar üzerinde.

    ![Samlsingle](./media/bamboo-tutorial/tutorial_bamboo_samlsingle.png)

10. Üzerinde **SAML SIngleSignOn eklentisi yapılandırma sayfası**, tıklatın **IDP eklemek**. 

    ![IDP ekleme](./media/bamboo-tutorial/tutorial_bamboo_addidp.png)

11. Üzerinde **SAML kimlik sağlayıcısı seçin** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Kimlik sağlayıcısı](./media/bamboo-tutorial/tutorial_bamboo_identityprovider.png)

    a. Seçin **IDP türü** olarak **AZURE AD**.

    b. İçinde **adı** metin kutusuna, bir ad yazın.

    c. İçinde **açıklama** metin kutusuna, bir açıklama yazın.

    d. **İleri**’ye tıklayın.

12. Üzerinde **kimlik sağlayıcı Yapılandırması** sayfasında **sonraki**.

    ![Kimlik yapılandırma](./media/bamboo-tutorial/tutorial_bamboo_identityconfig.png)

13.  Üzerinde **meta verileri içeri aktarma SAML IDP** sayfasında, tıklatın **yükleme dosyası** karşıya yüklemek için **meta veri XML** Azure Portalı'ndan indirilen dosya.

    ![İdpmetadata](./media/bamboo-tutorial/tutorial_bamboo_idpmetadata.png)

14. **İleri**’ye tıklayın.

15. Tıklatın **ayarlarını kaydetmek**.

    ![Kaydetme](./media/bamboo-tutorial/tutorial_bamboo_save.png)
    
> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/bamboo-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/bamboo-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/bamboo-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/bamboo-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-saml-sso-for-bamboo-by-resolution-gmbh-test-user"></a>SAML SSO çözümleme GmbH test kullanıcısı tarafından Bambu için oluşturma

Bu bölümün amacı Britta Simon SAML SSO için Bambu GmbH çözümleme tarafından adlı bir kullanıcı oluşturmaktır. SAML SSO GmbH çözünürlüğün Bambu yalnızca zaman kaynak sağlamayı destekler ve ayrıca kullanıcıların el ile oluşturulabilir başvurun [SAML SSO GmbH istemci çözünürlüğün Bambu için destek ekibi](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bamboo/server/support) , gereksinim göredir.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta tarafından çözüldü GmbH Bambu SAML SSO için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Çözümleme GmbH tarafından Bambu SAML SSO Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **SAML SSO GmbH çözünürlüğün Bambu için**.

    ![Uygulamalar listesinde çözümleme GmbH bağlantısıyla Bambu SAML SSO](./media/bamboo-tutorial/tutorial_bamboo_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Çözümleme GmbH kutucuğu erişim Paneli'nde tarafından Bambu SAML SSO tıklattığınızda, otomatik olarak Bambu, SAML SSO için çözümleme GmbH uygulama tarafından açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/bamboo-tutorial/tutorial_general_01.png
[2]: ./media/bamboo-tutorial/tutorial_general_02.png
[3]: ./media/bamboo-tutorial/tutorial_general_03.png
[4]: ./media/bamboo-tutorial/tutorial_general_04.png

[100]: ./media/bamboo-tutorial/tutorial_general_100.png

[200]: ./media/bamboo-tutorial/tutorial_general_200.png
[201]: ./media/bamboo-tutorial/tutorial_general_201.png
[202]: ./media/bamboo-tutorial/tutorial_general_202.png
[203]: ./media/bamboo-tutorial/tutorial_general_203.png

