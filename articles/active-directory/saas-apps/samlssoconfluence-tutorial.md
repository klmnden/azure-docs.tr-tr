---
title: 'Öğretici: Azure Active Directory ile tümleştirme Confluence için SAML SSO GmbH çözünürlüğün | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile SAML SSO Confluence için arasında GmbH çözünürlüğün yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 6b47d483-d3a3-442d-b123-171e3f0f7486
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: jeedes
ms.openlocfilehash: 260656ed17fecb0cac30b7970e9a2b76be42d8ea
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36230169"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-confluence-by-resolution-gmbh"></a>Öğretici: Çözümleme GmbH Confluence için SAML SSO Azure Active Directory Tümleştirmesi

Bu öğreticide, Azure Active Directory (Azure AD) ile GmbH çözünürlüğün SAML SSO Confluence için tümleştirme öğrenin.

Azure AD ile GmbH çözünürlüğün SAML SSO Confluence için tümleştirme ile aşağıdaki avantajları sağlar:

- SAML SSO Confluence için GmbH çözünürlüğün erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak Confluence için SAML SSO için Azure AD hesaplarına sahip (çoklu oturum açma) GmbH çözünürlüğün açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Çözümleme GmbH tarafından Confluence için SAML SSO ile Azure AD tümleştirme yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- SAML SSO etkin abonelik GmbH çoklu oturum çözünürlüğün Confluence için

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. SAML SSO Confluence için çözüm GmbH tarafından Galeriden ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-saml-sso-for-confluence-by-resolution-gmbh-from-the-gallery"></a>SAML SSO Confluence için çözüm GmbH tarafından Galeriden ekleme

SAML SSO Confluence için Azure AD'ye tümleştirme GmbH çözünürlüğün yapılandırmak için SAML SSO Confluence için çözüm GmbH tarafından Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**SAML SSO Confluence için çözüm GmbH tarafından Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **SAML SSO GmbH çözünürlüğün Confluence için**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/samlssoconfluence-tutorial/tutorial_samlssoconfluence_search.png)

5. Sonuçlar panelinde seçin **SAML SSO GmbH çözünürlüğün Confluence için**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/samlssoconfluence-tutorial/tutorial_samlssoconfluence_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Confluence için SAML SSO GmbH "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı çözünürlüğün test etme

Tekli çalışmaya oturum için Azure AD ne karşılık gelen kullanıcı Confluence için SAML SSO tarafından çözümleme GmbH Azure AD'de bir kullanıcı için olduğunu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının SAML SSO Confluence için ilgili kullanıcı çözünürlüğün arasında bir bağlantı ilişkisi GmbH kurulması gerekir.

Değeri GmbH çözünürlüğün Confluence için SAML SSO içinde atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırmak ve Azure AD çoklu oturum açma Confluence için SAML SSO GmbH çözünürlüğün sınamak için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[SAML SSO çözümleme GmbH test kullanıcı tarafından Confluence için oluşturma](#creating-a-saml-sso-for-confluence-by-resolution-gmbh-test-user)**  - Britta Simon, karşılık gelen SAML SSO Confluence için kullanıcı Azure AD gösterimini bağlı GmbH çözünürlüğün sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, SAML SSO Confluence için çözüm GmbH uygulama tarafından yapılandırın.

**Azure AD çoklu oturum açma Confluence için SAML SSO GmbH çözümleme tarafından yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **SAML SSO GmbH çözünürlüğün Confluence için** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/tutorial_samlssoconfluence_samlbase.png)

3. Üzerinde **Confluence GmbH etki alanı çözünürlüğün ve URL'ler için SAML SSO** uygulamada yapılandırmak istiyorsanız, bölüm **IDP** modu tarafından başlatılan:

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/tutorial_samlssoconfluence_url_1.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<server-base-url>/plugins/servlet/samlsso`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<server-base-url>/plugins/servlet/samlsso`

4. Denetleme **Göster Gelişmiş URL ayarları**. Uygulamada yapılandırmak istiyorsanız **SP** modu tarafından başlatılan:

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/tutorial_samlssoconfluence_url_2.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<server-base-url>/plugins/servlet/samlsso`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler, gerçek tanımlayıcı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Kişi [SAML SSO GmbH istemci çözünürlüğün Confluence için destek ekibi](https://www.resolution.de/go/support) bu değerleri almak için. 

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/tutorial_samlssoconfluence_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/tutorial_general_400.png)    
    
7. Farklı web tarayıcısı penceresinde oturum açın, **SAML SSO çözümleme GmbH Yönetici portalı tarafından Confluence için** yönetici olarak.

8. Dişlisine üzerine gelin ve tıklatın **eklentileri**.
    
    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon1.png)

9. Yönetici erişimi sayfasına yönlendirilirsiniz. Parolayı girin ve tıklayın **Onayla** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon2.png)

10. Altında **ATLASSIAN Market** sekmesini tıklatın, **Bul yeni eklentileri**. 

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon.png)

11. Arama **SAML çoklu oturum açma (SSO) Confluence için** tıklatıp **yükleme** yeni SAML eklentisini yüklemek için düğmeyi.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon7.png)

12. Eklenti yükleme başlar. **Kapat**’a tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon8.png)

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon9.png)

13. **Yönet**'e tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon10.png)
    
14. Tıklatın **yapılandırma** yeni eklenti yapılandırmak için.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon11.png)

15. Bu yeni eklenti ayrıca altında bulunabilir **kullanıcılar ve güvenlik** sekmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon3.png)
    
16. Üzerinde **SAML SingleSignOn eklentisi yapılandırma** sayfasında, **yeni IDP eklemek** kimlik sağlayıcısı ayarlarını yapılandırmak için düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon4.png)

17. Üzerinde **SAML kimlik sağlayıcısı seçin** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon5a.png)
 
    a. Ayarlama **Azure AD** IDP türü.
    
    b. Ekleme **adı** kimlik sağlayıcısı (örneğin Azure AD).
    
    c. Ekleme **açıklama** kimlik sağlayıcısı (örneğin Azure AD).
    
    d. **İleri**’ye tıklayın.
    
18. Üzerinde **kimlik sağlayıcı Yapılandırması** sayfasında, **sonraki** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon5b.png)

19. Üzerinde **meta verileri içeri aktarma SAML IDP** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon5c.png)

    a. Tıklatın **yükleme dosyası** düğmesini tıklatın ve 5. adımda indirdiğiniz meta veri XML dosyası seçin.

    b. Tıklatın **alma** düğmesi.
    
    c. İçeri aktarma başarılı olana kadar kısa bir süre bekleyin.
    
    d. Tıklatın **sonraki** düğmesi.
    
20. Üzerinde **kullanıcı kimliği öznitelik ve dönüşüm** sayfasında, **sonraki** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon5d.png)
    
21. Üzerinde **kullanıcı oluşturma ve güncelleştirme** sayfasında, **İleri & Kaydet** ayarlarını kaydetmek için.   
    
    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon6a.png)
    
22. Üzerinde **ayarlarınızı sınama** sayfasında, **Atla test & el ile yapılandırmanız** kullanıcı test şimdilik atlamak için. Bu, sonraki bölümde gerçekleştirilir ve Azure portalındaki bazı ayarları gerektirir. 
    
    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon6b.png)
    
23. Apprearing iletişim okumada **test anlamına gelir atlanıyor...** , tıklatın **Tamam**.
    
    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon6c.png)

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/samlssoconfluence-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/samlssoconfluence-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/samlssoconfluence-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/samlssoconfluence-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-saml-sso-for-confluence-by-resolution-gmbh-test-user"></a>SAML SSO çözümleme GmbH test kullanıcı tarafından Confluence için oluşturma

SAML SSO Confluence için çözüm GmbH tarafından oturum açmak Azure AD kullanıcıları etkinleştirmek için bunlar Confluence için SAML SSO içine GmbH çözümleme tarafından sağlanmalıdır.  
Confluence GmbH çözünürlüğün için SAML SSO içinde sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. SAML SSO çözümleme GmbH şirket site tarafından Confluence için yönetici olarak oturum açın.

2. Dişlisine üzerine gelin ve tıklatın **kullanıcı yönetimi**.

    ![Çalışanı ekleyin](./media/samlssoconfluence-tutorial/user1.png) 

3. Kullanıcılar bölümü altında tıklatın **kullanıcıları eklemek** sekmesi. Üzerinde **"Bir kullanıcı Ekle"** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çalışanı ekleyin](./media/samlssoconfluence-tutorial/user2.png) 

    a. İçinde **kullanıcıadı** metin kutusu, kullanıcı Britta Simon gibi e-posta yazın.

    b. İçinde **tam adı** metin kutusuna, Britta Simon gibi kullanıcının tam adını yazın.

    c. İçinde **e-posta** metin kutusuna, kullanıcının e-posta adresi türü ister Brittasimon@contoso.com.

    d. İçinde **parola** metin kutusuna, Britta Simon için parolayı yazın.

    e. Tıklatın **parolayı onayla** parolayı yeniden girin.
    
    f. Tıklatın **Ekle** düğmesi.    

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, çözüm GmbH tarafından Confluence için SAML SSO için erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı atama][200] 

**Çözümleme GmbH tarafından Confluence için SAML SSO Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **SAML SSO GmbH çözünürlüğün Confluence için**.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/tutorial_samlssoconfluence_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Çözümleme GmbH kutucuğu erişim Paneli'nde tarafından Confluence SAML SSO tıklattığınızda, otomatik olarak Confluence, SAML SSO için çözümleme GmbH uygulama tarafından açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/samlssoconfluence-tutorial/tutorial_general_01.png
[2]: ./media/samlssoconfluence-tutorial/tutorial_general_02.png
[3]: ./media/samlssoconfluence-tutorial/tutorial_general_03.png
[4]: ./media/samlssoconfluence-tutorial/tutorial_general_04.png

[100]: ./media/samlssoconfluence-tutorial/tutorial_general_100.png

[200]: ./media/samlssoconfluence-tutorial/tutorial_general_200.png
[201]: ./media/samlssoconfluence-tutorial/tutorial_general_201.png
[202]: ./media/samlssoconfluence-tutorial/tutorial_general_202.png
[203]: ./media/samlssoconfluence-tutorial/tutorial_general_203.png

