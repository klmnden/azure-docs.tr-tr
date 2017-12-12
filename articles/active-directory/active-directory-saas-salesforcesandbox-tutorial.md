---
title: "Öğretici: Salesforce korumalı alan Azure Active Directory Tümleştirme | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve Salesforce korumalı alan arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 801088bd63f025ca7cb1f9e4fe66b9c6f7f93453
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Öğretici: Azure Active Directory Tümleştirme ile Salesforce korumalı alan

Bu öğreticide, Azure Active Directory (Azure AD) ile Salesforce korumalı alan tümleştirmek öğrenin.

Salesforce korumalı alan Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Salesforce korumalı alan erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak Salesforce korumalı alanda (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Salesforce Sandbox ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Salesforce korumalı alan çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Salesforce korumalı alan ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-salesforce-sandbox-from-the-gallery"></a>Galeriden Salesforce korumalı alan ekleme
Azure AD Salesforce korumalı alan tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Salesforce korumalı alan eklemeniz gerekir.

**Galeriden Salesforce korumalı alan eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Tıklatın **yeni uygulama** iletişim kutusunun üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Salesforce korumalı alan**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_search.png)

5. Sonuçlar panelinde seçin **Salesforce korumalı alan**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırmanız ve Salesforce korumalı alan Azure AD çoklu oturum açma testiyle "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Salesforce korumalı alanı içinde bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Salesforce korumalı alan ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** Salesforce korumalı alanı içinde.

Yapılandırma ve Azure AD çoklu oturum açma Salesforce korumalı alan ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Salesforce korumalı alan test kullanıcısı oluşturma](#creating-a-salesforce-sandbox-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Salesforce Sandbox sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Salesforce korumalı alan uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Salesforce Sandbox ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Salesforce korumalı alan** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_samlbase.png)

3. Üzerinde **Salesforce korumalı alan etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_url.png)

     İçinde **oturum açma URL'si** metin kutusuna, şu biçimi kullanarak değeri yazın:`https://<subdomain>.my.salesforce.com`

    > [!NOTE] 
    > Bu değer gerçek değil. Bu değer gerçek oturum açma URL'si ile güncelleştirin. Kişi [Salesforce korumalı alan istemci destek ekibi](https://help.salesforce.com/support) bu değeri alınamıyor.


4. Zaten başka bir Salesforce korumalı alan örneği için çoklu oturum açmayı dizininizde yapılandırdıktan sonra yapılandırmanız da gerekir **tanımlayıcısı** aynı değere sahip **URL üzerinde oturum**. 
    
    >[!Note]
    >**Tanımlayıcısı** alan denetleyerek bulunabilir **Göster Gelişmiş ayarları** onay kutusuna bağlı **uygulama URL'sini Yapılandır** iletişim sayfası 


5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_400.png)

7. Üzerinde **Salesforce korumalı alan yapılandırma** 'yi tıklatın **yapılandırma Salesforce korumalı alan** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_configure.png) 
<CS>
8. Tarayıcınızda yeni bir sekme açın ve Salesforce korumalı alan yönetici hesabınızda oturum açın.

9. Üstteki menüde tıklatın **Kurulum**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-salesforcesandbox-tutorial/IC781024.png)
10. Sol gezinti bölmesinde tıklayın **güvenlik denetimleri**ve ardından **çoklu oturum açma ayarları**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-salesforcesandbox-tutorial/IC781025.png)
11. Çoklu oturum açma ayarları bölümüne aşağıdaki adımları gerçekleştirin: ![yapılandırma çoklu oturum açma](./media/active-directory-saas-salesforcesandbox-tutorial/IC781026.png)
     
     a.  Seçin **SAML etkin**. 

     b.  **Yeni**’ye tıklayın.

12. SAML çoklu oturum açma ayarları bölümüne aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-salesforcesandbox-tutorial/IC781027.png)

    a.In adı metin kutusuna yapılandırma adını yazın (örneğin: *SPSSOWAAD\_Test*). 

    b. Yapıştır **SMAL varlık kimliği** içine değer **veren** metin kutusu.

    c. İçinde **varlık kimliği** metin kutusuna, türü **https://test.salesforce.com** dizininize eklediğiniz ilk Salesforce korumalı alan örnek ise. Salesforce korumalı alan örneği sonra için eklediyseniz **varlık kimliği** yazın **oturum üzerinde URL'si**, şu biçimde olmalıdır:`http://company.my.salesforce.com`  
 
    d. Tıklatın **Gözat** indirilen sertifikayı karşıya yüklemek için.  

    e. Olarak **SAML kimlik türü**seçin **onaylamayı içeren kullanıcı nesnesinden Federasyon kimliği**.
 
    f. Olarak **SAML kimlik konumu**seçin **kimliktir konu deyimi NameIdentifier öğesinde**.

    g. Yapıştır **çoklu oturum açma hizmet URL'si** içine **kimlik sağlayıcısı oturum açma URL'si** metin kutusu. 

    h. SAML oturum kapatma SFDC desteklemez.  'Https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0' geçici bir çözüm olarak Yapıştır içine **kimlik sağlayıcısı oturum kapatma URL'si** metin kutusu.

    ı. Olarak **hizmet sağlayıcısı tarafından başlatılan bağlama isteği**seçin **HTTP POST**. 

    j. **Kaydet** düğmesine tıklayın.

### <a name="enable-your-domain"></a>Etki alanınızı etkinleştir
Bu bölümde, bir etki alanı zaten oluşturduğunuzu varsayar.  Daha fazla bilgi için bkz: [etki alanı adınız tanımlama](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).

**Etki alanınızı etkinleştirmek için aşağıdaki adımları gerçekleştirin:**

1. Sol gezinti bölmesinde **etki alanı yönetimi**ve ardından **My etki alanı.**
   
     ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-salesforcesandbox-tutorial/IC781029.png)
   
   >[!NOTE]
   >Lütfen etki alanınızı doğru yapılandırılmış olduğundan emin olun. 

2. İçinde **oturum açma sayfası ayarları** 'yi tıklatın **Düzenle**, daha sonra olarak **kimlik doğrulama hizmeti**, önceki bölümden oturum açma SAML tek ayar adını seçin ve son olarak tıklatın **kaydetmek**.
   
   ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-salesforcesandbox-tutorial/IC781030.png)

Yapılandırılmış bir etki alanınız hemen kullanıcılarınızın Salesforce korumalı alan oturum açma etki alanı URL'si kullanmanız gerekir.  

URL değerini almak için önceki bölümde oluşturduğunuz SSO profili tıklatın.    

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_02.png) 

3. İletişim kutusunun üstündeki **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-salesforce-sandbox-test-user"></a>Salesforce korumalı alan test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı Salesforce korumalı alanda oluşturulur. Salesforce korumalı alan yeni saat sağlama, varsayılan olarak etkin olduğu destekler.
Bu bölümde, eylem öğe yok. Salesforce korumalı alan erişmeyi denediğinde Salesforce korumalı alanda bir kullanıcı zaten mevcut değilse yeni bir tane oluşturulur.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Salesforce korumalı alan için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Salesforce korumalı alan Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Salesforce korumalı alan**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

SSO ayarlarınızı test etmek isterseniz, erişim paneli açın. Erişim paneli hakkında daha fazla ayrıntı için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)
* [Kullanıcı sağlamayı Yapılandır](active-directory-saas-salesforce-sandbox-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_203.png

