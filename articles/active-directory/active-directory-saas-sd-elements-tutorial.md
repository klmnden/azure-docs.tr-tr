---
title: "Öğretici: Azure Active Directory Tümleştirme SD öğelerle | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve SD öğeleri arasındaki yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f0386307-bb3b-4810-8d4b-d0bfebda04f4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 796f4d1b679c06be8677cd07f52ce305a7bc1ef8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sd-elements"></a>Öğretici: Azure Active Directory Tümleştirme SD öğeleri

Bu öğreticide, Azure Active Directory (Azure AD) ile SD öğeleri tümleştirmek öğrenin.

SD öğeleri Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- SD öğeleri erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak SD öğelerine (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme SD öğeleriyle yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir SD öğeleri çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden SD öğeleri ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-sd-elements-from-the-gallery"></a>Galeriden SD öğeleri ekleme
Azure AD SD öğeleri tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden SD öğeleri eklemeniz gerekir.

**Galeriden SD öğeler eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **SD öğeleri**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_search.png)

5. Sonuçlar panelinde seçin **SD öğeleri**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma SD "Britta Simon" adlı bir test kullanıcıya bağlı öğeleri ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen SD öğelerinin bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı SD öğeleri arasında bir bağlantı ilişkisi kurulması gerekir.

SD öğelerinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırmak ve Azure AD çoklu oturum açma SD öğelerle sınamak için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[SD öğeleri test kullanıcısı oluşturma](#creating-a-sd-elements-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı SD öğeleri sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma SD öğeleri uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma SD öğeleriyle yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **SD öğeleri** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_samlbase.png)

3. Üzerinde **SD öğeleri etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<tenantname>.sdelements.com/sso/saml2/metadata`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<tenantname>.sdelements.com/sso/saml2/acs/`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. Kişi [SD öğeleri destek ekibi](mailto:support@sdelements.com) bu değerleri almak için.

4. SD öğeleri uygulaması SAML onaylar belirli bir biçimde bekliyor. Bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz **"Kullanıcı özniteliği"** uygulama sekmesinde. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_attribute.png)

5. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği görüntüde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin: 

    | Öznitelik Adı | Öznitelik Değeri |
    | --- | --- |
    | e-posta |User.Mail |
    | FirstName |User.givenName |
    | Soyadı |User.surname |

    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sd-elements-tutorial/tutorial_officespace_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sd-elements-tutorial/tutorial_officespace_05.png)

    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.

    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. **Tamam**’a tıklayın.
 
6. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_certificate.png) 

7. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sd-elements-tutorial/tutorial_general_400.png)

8. Üzerinde **SD öğeleri yapılandırma** 'yi tıklatın **yapılandırma SD öğeleri** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_configure.png)

9. Tekli etkin oturum almak için başvurun, [SD öğeleri destek ekibi](mailto:support@sdelements.com) ve indirilen sertifika dosyasıyla verin. 

10. Farklı bir tarayıcı penceresinde SD öğeleri kiracınız yönetici olarak oturum.

11. Üstteki menüde tıklatın **sistem**ve ardından **çoklu oturum açma**. 
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_09.png) 

12. Üzerinde **çoklu oturum açma ayarları** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_10.png) 
   
    a. Olarak **SSO türü**seçin **SAML**.
   
    b. İçinde **kimlik sağlayıcısı varlık kimliği** metin değerini yapıştırın **SAML varlık kimliği**, Azure portalından kopyalanan. 
   
    c. İçinde **kimlik sağlayıcısı çoklu oturum açma hizmeti** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan. 
   
    d. **Kaydet** düğmesine tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-sd-elements-test-user"></a>SD öğeleri test kullanıcısı oluşturma

Bu bölümün amacı, SD öğelerinde Britta Simon adlı bir kullanıcı oluşturmaktır. SD söz konusu olduğunda, SD öğeleri kullanıcıları oluşturarak el ile bir görev öğedir.

**SD öğelerinde Britta Simon oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Bir web tarayıcısı penceresinde SD öğeleri şirket sitenize yönetici olarak oturum.

2. Üstteki menüde tıklatın **kullanıcı yönetimi**ve ardından **kullanıcılar**.
   
    ![SD öğeleri test kullanıcısı oluşturma](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_11.png) 

3. Tıklatın **yeni kullanıcı Ekle**.
   
    ![SD öğeleri test kullanıcısı oluşturma](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_12.png)
 
4. Üzerinde **yeni kullanıcı Ekle** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![SD öğeleri test kullanıcısı oluşturma](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_13.png) 
   
    a. İçinde **e-posta** metin kutusuna, bir kullanıcı gibi e-posta girin  **brittasimon@contoso.com** .
   
    b. İçinde **ad** metin gibi kullanıcının ilk adını girin **Britta**.
   
    c. İçinde **Soyadı** metin kutusuna, son kullanıcı gibi adını **Simon**.
   
    d. Olarak **rol**seçin **kullanıcı**. 
   
    e. Tıklatın **kullanıcı oluşturma**.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, SD öğelere erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı atama][200] 

**SD öğelerine Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **SD öğeleri**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.
  
Erişim paneli SD öğeleri parçasında tıklattığınızda, otomatik olarak SD öğeleri uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_203.png

