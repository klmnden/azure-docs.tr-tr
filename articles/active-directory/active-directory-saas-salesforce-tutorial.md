---
title: "Öğretici: Azure Active Directory Tümleştirme Salesforce ile | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve Salesforce arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2d7d420-dc91-41b8-a6b3-59579e043b35
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 639e40ca7e406a1726033e9f5c5363c289087589
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce"></a>Öğretici: Salesforce Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Salesforce tümleştirmek öğrenin.

Salesforce Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Salesforce erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak Salesforce (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Salesforce ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Salesforce çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Salesforce Galeriden ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-salesforce-from-the-gallery"></a>Salesforce Galeriden ekleme
Azure AD Salesforce tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Salesforce eklemeniz gerekir.

**Salesforce Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Tıklatın **yeni uygulama** iletişim kutusunun üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Salesforce**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_search.png)

5. Sonuçlar panelinde seçin **Salesforce**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı Salesforce ile test etme

Tekli çalışmaya oturum için Azure AD Salesforce karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Salesforce ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** Salesforce içinde.

Yapılandırma ve Azure AD çoklu oturum açma Salesforce ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Salesforce test kullanıcısı oluşturma](#creating-a-salesforce-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Salesforce sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Salesforce uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Salesforce yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Salesforce** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_samlbase.png)

3. Üzerinde **Salesforce etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_url.png)

    İçinde **oturum açma URL'si** metin kutusuna, şu biçimi kullanarak değeri yazın: 
   * Kurumsal hesap:`https://<subdomain>.my.salesforce.com`
   * Geliştirici hesabı:`https://<subdomain>-dev-ed.my.salesforce.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerleri gerçek oturum açma URL'si ile güncelleştirin. Kişi [Salesforce istemci destek ekibi](https://help.salesforce.com/support) bu değerleri almak için. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-salesforce-tutorial/tutorial_general_400.png)

6. Üzerinde **Salesforce yapılandırma** 'yi tıklatın **yapılandırma Salesforce** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.** 

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_configure.png) 
<CS>
7.  Tarayıcı ve günlüğüne Salesforce yönetici hesabınız için yeni bir sekme açın.

8.  Altında **yönetici** Gezinti bölmesinde, tıklatın **güvenlik denetimleri** ilgili bölümü genişletin. Ardından **çoklu oturum açma ayarları**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso.png)

9.  Üzerinde **çoklu oturum açma ayarları** sayfasında, **Düzenle** düğmesi.
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-edit.png)

      > [!NOTE]
      > Salesforce hesabınız için çoklu oturum açma ayarlarını etkinleştirmek erişemiyorsanız başvurmanız gerekebilir [Salesforce istemci destek ekibi](https://help.salesforce.com/support). 

10. Seçin **SAML etkin**ve ardından **kaydetmek**.

      ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-salesforce-tutorial/sf-enable-saml.png)
11. SAML çoklu oturum açma ayarlarınızı yapılandırmak için tıklatın **yeni**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-new.png)

12. Üzerinde **SAML çoklu oturum açma ayarını Düzenle** sayfasında, aşağıdaki yapılandırmaları yapın:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-salesforce-tutorial/sf-saml-config.png)

    a. İçin **adı** alanına, bu yapılandırma için bir kolay ad yazın. İçin bir değer sağlama **adı** otomatik olarak doldurulması **API adı** metin kutusu.

    b. Yapıştır **SMAL varlık kimliği** içine değer **veren** Salesforce alanındaki.

    c. İçinde **varlık kimliği textbox**, şu biçimi kullanarak Salesforce etki alanı adınızı yazın:
      
      * Kurumsal hesap:`https://<subdomain>.my.salesforce.com`
      * Geliştirici hesabı:`https://<subdomain>-dev-ed.my.salesforce.com`
      
    d. Tıklatın **Gözat** veya **Dosya Seç** açmak için **karşıya yüklenecek dosyayı Seç** iletişim kutusunda, Salesforce sertifikanızı seçin ve ardından **açmak** sertifikayı karşıya yüklemek için.

    e. İçin **SAML kimlik türü**seçin **onaylamayı kullanıcının salesforce.com kullanıcı adını içeren**.

    f. İçin **SAML kimlik konumu**seçin **kimliktir konu deyiminin NameIdentifier öğesi**

    g. Yapıştır **çoklu oturum açma hizmet URL'si** içine **kimlik sağlayıcısı oturum açma URL'si** Salesforce alanındaki.
    
    h. İçin **hizmet sağlayıcısı tarafından başlatılan bağlama isteği**seçin **HTTP yeniden yönlendirme**.
    
    ı. Son olarak, tıklatın **kaydetmek** , SAML çoklu oturum açma ayarları uygulamak için.

13. Salesforce sol gezinti bölmesinde üzerinde tıklatın **etki alanı yönetimi** ilgili bölümü genişletin ve ardından **My etki alanı**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-salesforce-tutorial/sf-my-domain.png)

14. Ekranı aşağı kaydırarak **kimlik doğrulama Yapılandırması** bölüm ve'ı tıklatın **Düzenle** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-salesforce-tutorial/sf-edit-auth-config.png)

15. İçinde **kimlik doğrulama hizmeti** bölümünde, SAML SSO yapılandırmanızı kolay adını seçin ve ardından **kaydetmek**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-salesforce-tutorial/sf-auth-config.png)

    > [!NOTE]
    > Birden fazla kimlik doğrulama hizmeti seçili ise, kullanıcıların bunlar çoklu oturum açma Salesforce ortamınıza başlatma sırasında oturum açmak istiyor hangi kimlik doğrulama hizmeti seçmeniz istenir. Bunu olmasını istemiyorsanız sonra yapmanız gerekenler **diğer tüm kimlik doğrulama hizmetleri işaretlemeden bırakın**.
<CE>    
> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol gezinti bölmesindeki **Azure portal**, tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-salesforce-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-salesforce-tutorial/create_aaduser_02.png) 

3. İletişim kutusunun üstündeki **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-salesforce-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-salesforce-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-salesforce-test-user"></a>Salesforce test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı Salesforce'ta oluşturulur. Salesforce yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler.
Bu bölümde, eylem öğe yok. Bir kullanıcı zaten Salesforce'ta yoksa, Salesforce erişmeyi denediğinde yeni bir tane oluşturulur.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Salesforce erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Salesforce Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Salesforce**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Çoklu oturum açma ayarlarınızı sınamak için adresinden erişim Paneli'nde açın [https://myapps.microsoft.com](https://myapps.microsoft.com/), test hesaba oturum ve tıklatın **Salesforce**.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)
* [Kullanıcı sağlamayı Yapılandır](active-directory-saas-salesforce-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_203.png

