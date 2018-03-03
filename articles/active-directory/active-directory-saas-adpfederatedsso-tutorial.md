---
title: "Öğretici: Azure Active Directory Tümleştirme ADP ile | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile ADP arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7be5331b-0481-48f7-9d6b-619dfec657e1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2018
ms.author: jeedes
ms.openlocfilehash: 1e0a35fd76f9eb6335685f05b8936b0b5105f6b2
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="tutorial-azure-active-directory-integration-with-adp"></a>Öğretici: Azure Active Directory Tümleştirme ADP ile

Bu öğreticide, Azure Active Directory (Azure AD) ile ADP tümleştirmek öğrenin.

ADP Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- ADP erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak ADP (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme ADP ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir etkin ADP aboneliği

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden ADP ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-adp-from-the-gallery"></a>Galeriden ADP ekleme
Azure AD ADP tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden ADP eklemeniz gerekir.

**Galeriden ADP eklemek için aşağıdaki adımları gerçekleştirin:**

1.  Microsoft Azure kimlik sağlayıcısı ortamınız için bir yönetici olarak oturum açın.

2. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

3. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
4. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

5. Arama kutusuna **ADP**seçin **ADP** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde ADP](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adp_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı ADP sınayın.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen ADP'de bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı ADP'de arasında bir bağlantı ilişkisi kurulması gerekir.

ADP içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma ADP ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[ADP test kullanıcısı oluşturma](#create-an-adp-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı ADP sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma ADP uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile ADP yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **ADP** uygulama tümleştirme sayfası, tıklatıldığında **özellikler sekmesi** ve aşağıdaki adımları gerçekleştirin: 

    ![Çoklu oturum açma özellikleri](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adp_prop.png)

    a. Ayarlama **kullanıcıların oturum açma için etkinleştirilen** alan için değer **Evet**.

    b. Kopyalama **kullanıcı erişim URL'si** ve yapıştırmak zorunda **yapılandırma oturum açma URL'si bölüm**, daha sonra öğreticide açıklanmıştır.

    c. Ayarlama **gerekli kullanıcı ataması** alan için değer **Evet**.

    d. Ayarlama **kullanıcılara görünür** alan için değer **Hayır**.

2. Tıklatın **çoklu oturum açma** üzerinde **ADP** uygulama tümleştirme sayfası.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

3. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adp_samlbase.png)

4. Üzerinde **ADP etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![ADP etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adp_url.png)

    İçinde **tanımlayıcısı** metin kutusuna, bir URL yazın: `https://fed.adp.com/` 
    
5. ADP uygulaması SAML onaylar SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemelerini ekleyin gerektiren belirli bir biçimde bekliyor. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir. Talep adı her zaman olacaktır **"PersonImmutableID"** ve değeri biz için eşledikten **EmployeeID**. 

    Üzerinde Azure AD kullanıcı eşlemesinden ADP için burada yapılır **EmployeeID** ancak bu uygulama ayarlarınıza göre farklı bir değer eşleyebilirsiniz. İş, bu nedenle Lütfen ile [ADP destek ekibi](https://www.adp.com/contact-us/overview.aspx) önce bir kullanıcının doğru tanıtıcısı kullanın ve bu değeri ile eşlemek için **"PersonImmutableID"** talep.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adp_attribute.png)

6. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği görüntüde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | ------------------- | -------------------- |    
    | PersonImmutableID | user.employeeid |
    
    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.

    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. **Tamam**’a tıklayın.

    > [!NOTE] 
    > SAML onayı yapılandırmadan önce başvurmanız gerekir, [ADP destek ekibi](https://www.adp.com/contact-us/overview.aspx) ve kiracınız için benzersiz tanımlayıcı özniteliği değeri isteyin. Uygulamanız için özel talep yapılandırmak için bu değeri gerekir. 

7. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adp_certificate.png) 

8. Çoklu oturum açma yapılandırmak için **ADP** yan, indirilen karşıya yüklemek için ihtiyacınız **meta veri XML** üzerinde [ADP Web sitesi](https://adpfedsso.adp.com/public/login/index.fcc).

> [!NOTE]  
> Bu işlem birkaç gün sürebilir. 

### <a name="configure-your-adp-services-for-federated-access"></a>ADP hizmetlere federe erişim için yapılandırma

>[!Important]
> ADP hizmetlere federe erişim isteyen çalışanlarınızın ADP service uygulaması ve daha sonra atanmalıdır, kullanıcıların belirli ADP hizmette atanmasını.
ADP temsilcinizle onayı alındığında ADP hizmete kullanıcı erişimi denetlemek için ADP hizmetlere ve atama ve yönetme kullanıcılarınızın yapılandırın.

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **ADP**seçin **ADP** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde ADP](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adp_addservicegallery.png)

5. Azure portalında üzerinde **ADP** uygulama tümleştirme sayfası, tıklatıldığında **özellikler sekmesi** ve aşağıdaki adımları gerçekleştirin:  

    ![Çoklu oturum açma linkedproperties](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adp_linkedproperties.png)

    a.  Ayarlama **kullanıcıların oturum açma için etkinleştirilen** alan için değer **Evet**.

    b.  Ayarlama **gerekli kullanıcı ataması** alan için değer **Evet**.

    c.  Ayarlama **kullanıcılara görünür** alan için değer **Evet**.

6. Tıklatın **çoklu oturum açma** üzerinde **ADP** uygulama tümleştirme sayfası.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

7. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **bağlantılı oturum açma**. uygulamanıza bağlamak için **ADP**.

    ![Tekli bağlantılı oturum](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adp_linked.png)

8. Gidin **yapılandırma oturum açma URL'si** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma prop](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adp_linkedsignon.png)
                                                              
    a. Yapıştır **kullanıcı erişim URL'si**, yukarıda kopyalanan **özellikler sekmesi** (uygulamasından ana ADP).
                                                             
    b. Şunlardır 5 farklı destekleyen uygulamalar **geçiş durumu URL'leri**. Uygun eklemek zorunda **geçiş durumu URL** belirli uygulama için el ile çok değer **kullanıcı erişim URL'si**.
    
    * **Şimdi ADP iş gücü**
        
        `<User access URL>?Relay State=https://fed.adp.com/saml/fedlanding.html?WFN`

    * **ADP iş gücüne artık zaman Gelişmiş**
        
        `<User access URL>?Relay State=https://fed.adp.com/saml/fedlanding.html?EETDC2`
    
    * **ADP Vantage HCM**
        
        `<User access URL>?Relay State=https://fed.adp.com/saml/fedlanding.html?ADPVANTAGE`

    * **ADP Enterprise HR**

        `<User access URL>?Relay State=https://fed.adp.com/saml/fedlanding.html?PORTAL`

    * **MyADP**

        `<User access URL>?Relay State=https://fed.adp.com/saml/fedlanding.html?REDBOX`

9. **Kaydet** değişikliklerinizi.

10. ADP temsilcinizle onayı alındığında test bir veya iki kullanıcı ile başlar.

    a. Birkaç kullanıcı ADP hizmet için Federasyon erişimi test etmek için uygulama atayın.

    b. Test kullanıcıları galeride ADP hizmet uygulamasına erişmesi ve kendi ADP hizmet erişebilirsiniz başarılı olur.
 
11. Bireysel kullanıcıları veya daha sonra öğreticide açıklandığı ve çalışanlarınıza dağıtmadan kullanıcı grupları hakkında daha fazla onay başarılı bir test federe ADP hizmet atayın. 

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-adpfederatedsso-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-adpfederatedsso-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-adpfederatedsso-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-adpfederatedsso-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-adp-test-user"></a>ADP test kullanıcısı oluşturma

Bu bölümün amacı ADP içinde Britta Simon adlı bir kullanıcı oluşturmaktır. Çalışmak [ADP destek ekibi](https://www.adp.com/contact-us/overview.aspx) ADP hesap kullanıcılar eklemek için.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta ADP erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**ADP Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **ADP**.

    ![Uygulamalar listesinde ADP bağlantı](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adp_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli ADP parçasında tıklattığınızda, otomatik olarak ADP uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_203.png

