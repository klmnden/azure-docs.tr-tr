---
title: "Öğretici: Azure Active Directory Tümleştirme ile ServiceChannel | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile ServiceChannel arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c3546eab-96b5-489b-a309-b895eb428053
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/3/2017
ms.author: jeedes
ms.openlocfilehash: 7e1dad18ff0ae9a9102b789b2cb32e7b96ed3d38
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-servicechannel"></a>Öğretici: Azure Active Directory Tümleştirme ServiceChannel ile

Bu öğreticide, Azure Active Directory (Azure AD) ile ServiceChannel tümleştirmek öğrenin.

ServiceChannel Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- ServiceChannel erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için ServiceChannel (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure Yönetim Portalı'nı yönetme

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme ServiceChannel ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir ServiceChannel çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Bu gerekli olmadığı sürece, üretim ortamınızın kullanmamanız gerekir.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden ServiceChannel ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-servicechannel-from-the-gallery"></a>Galeriden ServiceChannel ekleme
Azure AD ServiceChannel tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden ServiceChannel eklemeniz gerekir.

**Galeriden ServiceChannel eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure Yönetim Portalı](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Tıklatın **Ekle** iletişim kutusunun üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **ServiceChannel**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_000.png)

5. Sonuçlar panelinde seçin **ServiceChannel**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_2.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı ServiceChannel sınayın.

Tekli çalışmaya oturum için Azure AD ServiceChannel karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ServiceChannel ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** ServiceChannel içinde.

Yapılandırma ve Azure AD çoklu oturum açma ServiceChannel ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[ServiceChannel test kullanıcısı oluşturma](#creating-a-servicechannel-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure Yönetim Portalı'nda etkinleştirin ve çoklu oturum açma ServiceChannel uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile ServiceChannel yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda üzerinde **ServiceChannel** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda, olarak **modu** seçin **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_01.png)

3. Üzerinde **ServiceChannel etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_urls.png)

    a. İçinde **tanımlayıcısı** metin değeri olarak yazın:`http://adfs.<domain>.com/adfs/service/trust`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<customer domain>.servicechannel.com/saml/acs`

    > [!NOTE] 
    > Lütfen bu gerçek değerlerin olmadığına dikkat edin. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirmeniz gerekir. Burada dizesinin benzersiz değeri tanımlayıcıda kullanmanızı öneririz. Kişi [ServiceChannel destek ekibi](https://servicechannel.zendesk.com/hc/en-us) bu değerleri almak için.

4. SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemelerini ekleyin gerektiren belirli bir biçimde SAML onaylar ServiceChannel uygulamanızı bekliyor. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir. **NameIdentifier (kullanıcı tanımlayıcısı)** yalnızca zorunlu talebi ve varsayılan değer **user.userprincipalname** ancak ServiceChannel bekliyor bu ile eşlenecek **user.mail**. Yalnızca zaman içinde kullanıcı sağlamayı etkinleştirin planlıyorsanız, aşağıda gösterildiği gibi aşağıdaki talep eklemelisiniz. **Rol** talep eşlenmesi gerekiyor **user.assignedroles** kullanıcı rolünü içerir.  

    ServiceChannel Kılavuzu başvurabilir [burada](https://servicechannel.zendesk.com/hc/en-us/articles/217514326-Azure-AD-Configuration-Example) talepler hakkında daha fazla yönergeler için.
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-servicechannel-tutorial/tutorial_servicechannel_attribute.png)

    > [!NOTE] 
    > Lütfen tıklatın [burada](http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/) nasıl yapılandırılacağını öğrenmek için **rol** Azure AD'de

5. İçinde **kullanıcı öznitelikleri** 'yi tıklatın **Görünüm ve diğer tüm kullanıcı özniteliklerini düzenleme** ve özniteliklerini ayarlayın.

    | Öznitelik adı | Öznitelik değeri |
    | --- | --- |    
    | Rol| User.assignedroles |

    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-servicechannel-tutorial/tutorial_servicechannel_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-servicechannel-tutorial/tutorial_servicechannel_05.png)
    
    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. Tıklatın **Tamam**
    
6. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_05.png) 

7. **Kaydet** düğmesine tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-servicechannel-tutorial/tutorial_general_400.png)

8. Üzerinde **ServiceChannel yapılandırma** 'yi tıklatın **yapılandırma ServiceChannel** açmak için **yapılandırma oturum açma** penceresi. Lütfen unutmayın **SAML Enitity kimliği** gelen **hızlı başvuru** bölümü.

9. Çoklu oturum açma yapılandırmak için **ServiceChannel** yan, indirilen göndermek için ihtiyacınız **sertifika (Base64)** ve **SAML varlık kimliği** için [ServiceChannel destek ekibi](https://servicechannel.zendesk.com/hc/en-us). Bunlar bu iki tarafta da ayarlamanızı SAML SSO bağlantınız için kurar.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure Yönetim Portalı'nda bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure Yönetim Portalı**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_01.png) 

2. Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar** kullanıcıların listesini görüntülemek için.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_02.png) 

3. İletişim kutusunun üstündeki **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın. 

### <a name="creating-a-servicechannel-test-user"></a>ServiceChannel test kullanıcısı oluşturma

Uygulama, süresi kullanıcı sağlama ve kimlik doğrulama kullanıcılar uygulamada otomatik olarak oluşturulacak sonra hemen destekler. Tam kullanıcı sağlama için temasa [ServiceChannel destek ekibi](https://servicechannel.zendesk.com/hc/en-us)

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta ServiceChannel için kendi erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**ServiceChannel için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **ServiceChannel**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_app01.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli ServiceChannel parçasında tıklattığınızda, otomatik olarak ServiceChannel uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_203.png