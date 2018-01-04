---
title: "Öğretici: Azure Active Directory Tümleştirme kayma ile | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Slack'e arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: ffc5e73f-6c38-4bbb-876a-a7dd269d4e1c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 67d95b48520cd990428447dc775d9af22528c0f0
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-slack"></a>Öğretici: Azure Active Directory Tümleştirme kayma ile

Bu öğreticide, Azure Active Directory (Azure AD) ile kayma tümleştirmek öğrenin.

Kayma Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Kayma erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak Slack'e (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme kayma ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Slack çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden kayma ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-slack-from-the-gallery"></a>Galeriden kayma ekleme
Azure AD'ye kayma tümleştirmesini yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden kayma eklemeniz gerekir.

**Galeriden kayma eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Slack'e**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-slack-tutorial/tutorial_slack_search.png)

5. Sonuçlar panelinde seçin **Slack'e**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-slack-tutorial/tutorial_slack_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırmak ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı kayma sınayın.

Tekli çalışmaya oturum için Azure AD boşluk karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının kayma ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Kayma içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma kayma ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Slack test kullanıcısı oluşturma](#creating-a-slack-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı kayma sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Slack uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile kayma yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **kayma** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-slack-tutorial/tutorial_slack_samlbase.png)

3. Üzerinde **kayma etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-slack-tutorial/tutorial_slack_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<companyname>.slack.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL'yi yazın:`https://slack.com`

    > [!NOTE] 
    > Değer gerçek değil. Değerin gerçek oturum üzerinde URL ile güncelleştirmeniz gerekir. Kişi [Slack destek ekibi](https://slack.com/help/contact) değeri alınamıyor
     
4. Slack uygulaması SAML onaylar belirli bir biçimde bekliyor. Bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirmesi sayfasında bölüm. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir.
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-slack-tutorial/tutorial_slack_attribute.png)

5. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda **user.mail** olarak **kullanıcı tanımlayıcısı** ve aşağıdaki tabloda gösterilen her satır için aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | --- | --- |
    | ilk_ad | User.givenName |
    | Soyadı | User.surname |
    | User.Email | User.Mail |  
    | User.Username | User.userPrincipalName |

    a. Tıklayın **özniteliği** açmak için **öznitelik Düzenle** iletişim kutusuna ve aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-slack-tutorial/tutorial_slack_attribute1.png)

    a. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.
    
    b. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri seçin.
    
    c. **Tamam**’a tıklayın.

6. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-slack-tutorial/tutorial_slack_certificate.png)

7. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-slack-tutorial/tutorial_general_400.png)

8. Üzerinde **kayma yapılandırma** 'yi tıklatın **yapılandırma kayma** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-slack-tutorial/tutorial_slack_configure.png) 

9.  Farklı web tarayıcısı penceresinde Slack şirket sitenizin bir yönetici olarak oturum açın.

10.  Gidin **Microsoft Azure AD** ardından **takım ayarları**.

     ![Çoklu oturum açma uygulama tarafında yapılandırma](./media/active-directory-saas-slack-tutorial/tutorial_slack_001.png)

11.  İçinde **takım ayarları** 'yi tıklatın **kimlik doğrulaması** sekmesini ve ardından **ayarlarını değiştir**.

     ![Çoklu oturum açma uygulama tarafında yapılandırma](./media/active-directory-saas-slack-tutorial/tutorial_slack_002.png)

12. Üzerinde **SAML kimlik doğrulaması ayarlarını** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma uygulama tarafında yapılandırma](./media/active-directory-saas-slack-tutorial/tutorial_slack_003.png)

    a.  İçinde **SAML 2.0 uç noktası (HTTP)** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan.

    b.  İçinde **kimlik sağlayıcısı veren** metin değerini yapıştırın **SAML varlık kimliği**, Azure portalından kopyalanan.

    c.  İndirilen sertifika dosyasını Not Defteri'nde açın, içeriğini, panoya kopyalayın ve yapıştırın kendisine **ortak sertifika** metin kutusu.

    d. Yukarıdaki üç ayarlarını Slack ekibiniz için uygun şekilde yapılandırın. Ayarlar hakkında daha fazla bilgi için lütfen bulma **Slack'e'nın SSO Yapılandırma Kılavuzu'nda** burada. `https://get.slack.help/hc/articles/220403548-Guide-to-single-sign-on-with-Slack%60`

    e.  Tıklatın **yapılandırmasını kaydetmek**.
     
    <!-- Deselect **Allow users to change their email address**.

    e.  Select **Allow users to choose their own username**.

    f.  As **Authentication for your team must be used by**, select **It’s optional**. -->

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-slack-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-slack-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-slack-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-slack-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-slack-test-user"></a>Slack test kullanıcısı oluşturma

Bu bölümün amacı kayma içinde Britta Simon adlı bir kullanıcı oluşturmaktır. Kayma yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler.

Bu bölümde, eylem öğe yok. Yeni bir kullanıcı henüz yoksa kayma erişme denemesi sırasında oluşturulur.

> [!NOTE]
> Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişiye gereksinim [Slack destek ekibi](https://slack.com/help/contact).

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Slack'e erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Slack'e Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Slack'e**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-slack-tutorial/tutorial_slack_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Slack kutucuğa tıkladığınızda erişim panelinde, otomatik olarak Slack uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-slack-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-slack-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-slack-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-slack-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-slack-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-slack-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-slack-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-slack-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-slack-tutorial/tutorial_general_203.png

