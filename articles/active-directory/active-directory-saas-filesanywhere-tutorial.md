---
title: "Öğretici: Azure Active Directory Tümleştirme ile FilesAnywhere | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile FilesAnywhere arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: jeedes
ms.openlocfilehash: 3ca017dbfe35d291083c66f41868fd425e74a602
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-filesanywhere"></a>Öğretici: Azure Active Directory Tümleştirme FilesAnywhere ile

Bu öğreticide, Azure Active Directory (Azure AD) ile FilesAnywhere tümleştirmek öğrenin.

FilesAnywhere Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- FilesAnywhere erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için FilesAnywhere (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure Yönetim Portalı'nı yönetme

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme FilesAnywhere ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir FilesAnywhere çoklu oturum açma etkin abonelik


> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.


Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Bu gerekli olmadığı sürece, üretim ortamınızın kullanmamanız gerekir.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden FilesAnywhere ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama


## <a name="adding-filesanywhere-from-the-gallery"></a>Galeriden FilesAnywhere ekleme
Azure AD FilesAnywhere tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden FilesAnywhere eklemeniz gerekir.

**Galeriden FilesAnywhere eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure Yönetim Portalı](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Tıklatın **Ekle** iletişim kutusunun üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **FilesAnywhere**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_search.png)

5. Sonuçlar panelinde seçin **FilesAnywhere**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_addfromgallery.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı FilesAnywhere sınayın.

Tekli çalışmaya oturum için Azure AD FilesAnywhere karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının FilesAnywhere ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** FilesAnywhere içinde.

Yapılandırma ve Azure AD çoklu oturum açma FilesAnywhere ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[FilesAnywhere test kullanıcısı oluşturma](#creating-a-filesanywhere-test-user)**  - Britta Simon, karşılık gelen her, Azure AD gösterimine bağlı FilesAnywhere sağlamak için.
3. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
4. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure Yönetim Portalı'nda etkinleştirin ve çoklu oturum açma FilesAnywhere uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile FilesAnywhere yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda üzerinde **FilesAnywhere** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda, olarak **modu** seçin **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_samlbase.png)

3. Üzerinde **FilesAnywhere etki alanı ve URL'leri** uygulamada yapılandırmak istiyorsanız, bölüm **IDP başlatılan modu**:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_filesanywhere_url.png)
    
    a. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<company name>.filesanywhere.com/saml20.aspx?c=215`
> [!NOTE]
> Lütfen unutmayın değeri **215** olan bir **ClientID** ve yalnızca bir örnektir. Gerçek ClientID değeri ile değiştirmeniz gerekir.

4. Üzerinde **FilesAnywhere etki alanı ve URL'leri** uygulamada yapılandırmak istiyorsanız, bölüm **SP tarafından başlatılan modu**, aşağıdaki adımları gerçekleştirin:
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_filesanywhere_url1.png)

    a. Tıklayın **Göster Gelişmiş URL ayarları** seçeneği

    b. İçinde **oturum üzerinde URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<sub domain>.filesanywhere.com/`

    > [!NOTE] 
    > Lütfen bu gerçek değerlerin olmadığına dikkat edin. Gerçek oturum üzerinde URL'si ve yanıt URL'si bu değerleri güncelleştirmeniz gerekir. Kişi [FilesAnywhere destek ekibi](mailto:support@FilesAnywhere.com) bu değerleri almak için. 

5. FilesAnywhere yazılım uygulaması SAML onaylar belirli bir biçimde bekliyor. Lütfen bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirmesi sayfasında bölüm. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir.
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_filesanywhere_attribute.png)
    
    Kullanıcılar oturum açtığında ile FilesAnywhere değeri elde **ClientID** özniteliğini [FilesAnywhere takım](mailto:support@FilesAnywhere.com). FilesAnywhere tarafından sağlanan benzersiz değeri olan "İstemci kimliği" özniteliğini eklemeniz gerekir. Yukarıda gösterilen bu öznitelikler gereklidir.
    > [!NOTE] 
    > Lütfen unutmayın değeri **2331** , **ClientID** yalnızca bir örnektir. Gerçek değer sağlamanız gerekir.


6. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, yukarıdaki resimde gösterildiği gibi SAML belirteci özniteliği yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | ---------------| --------------- |    
    | istemci kimliği | *"uniquevalue"* |

    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_05.png)
    
    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. Tıklatın **Tamam**

7. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_400.png)

8. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_certificate.png) 

9. Üzerinde **FilesAnywhere yapılandırma** 'yi tıklatın **yapılandırma FilesAnywhere** açmak için **yapılandırma oturum açma** penceresi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_configure.png) 

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_configuresignon.png)

10. SSO yapılandırma FilesAnywhere sonunda, uygulamanız için tam almak için başvurun [FilesAnywhere destek ekibi](mailto:support@FilesAnywhere.com) ve indirilen SAML belirteç imzalama sertifikası ve çoklu oturum açma (SSO) URL verin.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure Yönetim Portalı'nda bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure Yönetim Portalı**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-FilesAnywhere-tutorial/create_aaduser_01.png) 

2. Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar** kullanıcıların listesini görüntülemek için.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-FilesAnywhere-tutorial/create_aaduser_02.png) 

3. İletişim kutusunun üstündeki **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-FilesAnywhere-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-FilesAnywhere-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın. 



### <a name="creating-a-filesanywhere-test-user"></a>FilesAnywhere test kullanıcısı oluşturma

Uygulama, süresi kullanıcı sağlama ve kimlik doğrulama kullanıcılar uygulamada otomatik olarak oluşturulacak sonra hemen destekler. 


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta FilesAnywhere için kendi erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**FilesAnywhere için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **FilesAnywhere**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    


### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli FilesAnywhere parçasında tıklattığınızda, otomatik olarak FilesAnywhere uygulamanıza açan.


## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_203.png
