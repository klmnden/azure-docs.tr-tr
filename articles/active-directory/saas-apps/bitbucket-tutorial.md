---
title: 'Öğretici: Azure Active Directory ile tümleştirme Bitbucket için SAML SSO GmbH çözünürlüğün | Microsoft Docs'
description: Çoklu oturum açma SAML SSO için Bitbucket ile Azure Active Directory arasında GmbH çözünürlüğün yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: fc947df1-f24e-43ae-9a34-518293583d69
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2017
ms.author: jeedes
ms.openlocfilehash: 21b6adb98fdb973b75aa1d6db519777bab730d73
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39048132"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-bitbucket-by-resolution-gmbh"></a>Öğretici: Azure Active Directory ile tümleştirme Bitbucket için SAML SSO GmbH çözünürlüğün

Bu öğreticide, Azure Active Directory (Azure AD) ile GmbH çözünürlüğün Bitbucket için SAML SSO tümleştirme konusunda bilgi edinin.

Bitbucket için SAML SSO çözünürlüğün GmbH Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- SAML SSO Bitbucket için GmbH çözünürlüğün erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanmış için Bitbucket için SAML SSO (çoklu oturum açma) GmbH çözünürlüğün Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi GmbH çözünürlüğüyle Bitbucket için SAML SSO ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- SAML SSO etkin abonelikte GmbH çoklu oturum çözünürlüğün Bitbucket için

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Bitbucket için SAML SSO GmbH çözünürlüğüyle galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-saml-sso-for-bitbucket-by-resolution-gmbh-from-the-gallery"></a>Bitbucket için SAML SSO GmbH çözünürlüğüyle galeri ekleme
Bitbucket için SAML SSO, Azure AD'de tümleştirmesini GmbH çözünürlüğün yapılandırmak için Bitbucket için SAML SSO GmbH çözünürlüğüyle Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Bitbucket için SAML SSO GmbH çözünürlüğüyle Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **GmbH çözünürlüğün Bitbucket için SAML SSO**seçin **GmbH çözünürlüğün Bitbucket için SAML SSO** sonucu panelinden ardından **Ekle** eklemek için Ekle düğmesine uygulama.

    ![Sonuç listesinde GmbH çözünürlüğün Bitbucket için SAML SSO](./media/bitbucket-tutorial/tutorial_bitbucket_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma SAML SSO için Bitbucket ile GmbH "Britta Simon" adlı bir test kullanıcı tabanlı bir çözüm olarak test edin.

Tek çalışmak için oturum açma için Azure AD hangi karşılık gelen kullanıcı Bitbucket için SAML SSO çözünürlüğün GmbH, Azure AD'de bir kullanıcı için olduğunu bilmesi gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve SAML SSO Bitbucket için ilgili kullanıcı çözünürlüğün arasında bir bağlantı ilişki GmbH kurulması gerekir.

SAML SSO GmbH çözünürlüğün Bitbucket, değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma SAML SSO için Bitbucket ile GmbH çözünürlüğün sınamak için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[SAML SSO için çözüm GmbH test kullanıcısı tarafından Bitbucket oluşturma](#create-a-saml-sso-for-bitbucket-by-resolution-gmbh-test-user)**  - kullanıcı Azure AD gösterimini bağlı GmbH çözünürlüğün Bitbucket için SAML SSO içinde bir karşılığı Britta simon'un sağlamak için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, SAML SSO Bitbucket için GmbH uygulama çözünürlüğüne göre yapılandırın.

**Azure AD çoklu oturum açma SAML SSO için Bitbucket ile GmbH çözünürlüğüyle yapılandırmak için aşağıdaki adımları uygulayın:**

1. Azure portalında üzerinde **GmbH çözünürlüğün Bitbucket için SAML SSO** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/bitbucket-tutorial/tutorial_bitbucket_samlbase.png)

3. Üzerinde **Bitbucket çözünürlüğün GmbH etki alanı ve URL'ler için SAML SSO** bölümünde, IDP tarafından başlatılan modunda uygulama yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin:

    ![Oturum açma bilgileri tek bir SAML SSO için Bitbucket çözünürlüğün GmbH etki alanı ve URL'ler](./media/bitbucket-tutorial/tutorial_bitbucket_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<server-base-url>/plugins/servlet/samlsso`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<server-base-url>/plugins/servlet/samlsso`

4. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Oturum açma bilgileri tek bir SAML SSO için Bitbucket çözünürlüğün GmbH etki alanı ve URL'ler](./media/bitbucket-tutorial/tutorial_bitbucket_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<server-base-url>/plugins/servlet/samlsso`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [SAML SSO GmbH istemci çözünürlüğün Bitbucket için Destek ekibine](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bitbucket/server/support) bu değerleri almak için. 

5. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/bitbucket-tutorial/tutorial_bitbucket_certificate.png) 

6. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/bitbucket-tutorial/tutorial_general_400.png)
    
7. SAML SSO için çözüm GmbH şirket site tarafından Bitbucket için yönetici olarak oturum.

8. Ana araç çubuğunun sağ tarafında tıklayın **ayarları**.

9. HESAPLAR bölümüne gidin, tıklayarak **SAML SingleSignOn** menü çubuğu üzerinde.

    ![Samlsingle](./media/bitbucket-tutorial/tutorial_bitbucket_samlsingle.png)

10. Üzerinde **SAML SIngleSignOn eklentisi yapılandırma sayfası**, tıklayın **IDP ekleme**. 

    ![IDP Ekle](./media/bitbucket-tutorial/tutorial_bitbucket_addidp.png)

11. Üzerinde **SAML kimlik sağlayıcınızı seçin** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Kimlik sağlayıcısı](./media/bitbucket-tutorial/tutorial_bitbucket_identityprovider.png)

    a. Seçin **IDP türü** olarak **AZURE AD'ye**.

    b. İçinde **adı** metin kutusuna bir ad yazın.

    c. İçinde **açıklama** metin kutusuna bir açıklama yazın.

    d. **İleri**’ye tıklayın.

12. Üzerinde **kimlik sağlayıcı yapılandırma sayfası**, tıklayın **sonraki**.

    ![Kimlik yapılandırma](./media/bitbucket-tutorial/tutorial_bitbucket_identityconfig.png)

13.  Üzerinde **SAML IDP meta verileri içeri aktarma** sayfasında, **yük dosyası** yüklenecek **meta veri XML** Azure portalından indirdiğiniz dosyası.

    ![İdpmetadata](./media/bitbucket-tutorial/tutorial_bitbucket_idpmetadata.png)
    
14. **İleri**’ye tıklayın.

15. Tıklayın **ayarlarını kaydetmek**.

    ![Kaydetme](./media/bitbucket-tutorial/tutorial_bitbucket_save.png)

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/bitbucket-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/bitbucket-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/bitbucket-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/bitbucket-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-saml-sso-for-bitbucket-by-resolution-gmbh-test-user"></a>SAML SSO çözümleme GmbH test kullanıcısı tarafından Bitbucket için oluşturma

Bu bölümün amacı, Britta Simon SAML SSO için Bitbucket GmbH çözünürlüğüyle adlı bir kullanıcı oluşturmaktır. SAML SSO GmbH çözünürlüğün Bitbucket, just-ın-time sağlamayı destekler ve ayrıca kullanıcılar el ile oluşturulabilir başvurun [SAML SSO GmbH istemci çözünürlüğün Bitbucket için Destek ekibine](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bitbucket/server/support) ihtiyacınıza göre.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma GmbH çözünürlüğüyle Bitbucket için SAML SSO için erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Bitbucket için SAML SSO GmbH çözünürlüğüyle atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **GmbH çözünürlüğün Bitbucket için SAML SSO**.

    ![SAML SSO uygulamaları listesinde çözümleme GmbH bağlantısıyla Bitbucket için](./media/bitbucket-tutorial/tutorial_bitbucket_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Tarafından erişim panelinde çözümleme GmbH kutucuk SAML SSO için Bitbucket'ye tıkladığınızda, otomatik olarak, SAML SSO için Bitbucket için çözüm GmbH uygulama tarafından açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/bitbucket-tutorial/tutorial_general_01.png
[2]: ./media/bitbucket-tutorial/tutorial_general_02.png
[3]: ./media/bitbucket-tutorial/tutorial_general_03.png
[4]: ./media/bitbucket-tutorial/tutorial_general_04.png

[100]: ./media/bitbucket-tutorial/tutorial_general_100.png

[200]: ./media/bitbucket-tutorial/tutorial_general_200.png
[201]: ./media/bitbucket-tutorial/tutorial_general_201.png
[202]: ./media/bitbucket-tutorial/tutorial_general_202.png
[203]: ./media/bitbucket-tutorial/tutorial_general_203.png

