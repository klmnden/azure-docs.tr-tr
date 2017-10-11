---
title: "Öğretici: Azure Active Directory Tümleştirme ile TeamSeer | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile TeamSeer arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6ec4806f-fe0f-4ed7-8cfa-32d1c840433f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 2a5e8f6d1443681c43db95da5cef0b7f2ef92291
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-teamseer"></a>Öğretici: Azure Active Directory Tümleştirme TeamSeer ile

Bu öğreticide, Azure Active Directory (Azure AD) ile TeamSeer tümleştirmek öğrenin.

TeamSeer Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- TeamSeer erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için TeamSeer (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme TeamSeer ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir TeamSeer çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden TeamSeer ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-teamseer-from-the-gallery"></a>Galeriden TeamSeer ekleme
Azure ad içinde TeamSeer tümleştirmesini yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden TeamSeer eklemeniz gerekir.

**Galeriden TeamSeer eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **TeamSeer**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_search.png)

5. Sonuçlar panelinde seçin **TeamSeer**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı TeamSeer ile test etme

Tekli çalışmaya oturum için Azure AD TeamSeer karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının TeamSeer ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

TeamSeer içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma TeamSeer ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[TeamSeer test kullanıcısı oluşturma](#creating-a-teamseer-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı TeamSeer sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma TeamSeer uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile TeamSeer yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **TeamSeer** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_samlbase.png)

3. Üzerinde **TeamSeer etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_url.png)

     İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://www.teamseer.com/<companyid>`

    > [!NOTE] 
    > Değer gerçek değil. Değerin gerçek oturum açma URL'si ile güncelleştirin. Kişi [TeamSeer istemci destek ekibi](http://pages.theaccessgroup.com/solutions_business-suite_absence-management_contact.html) değeri alınamıyor. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-teamseer-tutorial/tutorial_general_400.png)

6. Üzerinde **TeamSeer yapılandırma** 'yi tıklatın **yapılandırma TeamSeer** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_configure.png)

7. Farklı web tarayıcısı penceresinde TeamSeer şirket sitenize yönetici olarak oturum açın.

8. Git **ik yönetici**.
   
    ![HR yönetici](./media/active-directory-saas-teamseer-tutorial/ic789634.png "ik yönetici")

9. Tıklatın **Kurulum**.
   
    ![Kurulum](./media/active-directory-saas-teamseer-tutorial/ic789635.png "Kurulumu")

10. Tıklatın **SAML sağlayıcı ayrıntılarını ayarlama**.
   
    ![SAML ayarları](./media/active-directory-saas-teamseer-tutorial/ic789636.png "SAML ayarları")

11. SAML sağlayıcısı Ayrıntılar bölümünde aşağıdaki adımları gerçekleştirin:
   
    ![SAML ayarları](./media/active-directory-saas-teamseer-tutorial/ic789637.png "SAML ayarları")   

    a. Yapıştır **çoklu oturum açma hizmet URL'si** için değer **URL** metin kutusu.
          
    b. Base-64 kodlanmış sertifikanızı Not Defteri'nde açın, içinde içeriğini panonuza kopyalayın ve yapıştırın kendisine **IDP ortak sertifika** metin kutusu.

12. SAML sağlayıcısı yapılandırmasını tamamlamak için aşağıdaki adımları gerçekleştirin:
    
    ![SAML ayarları](./media/active-directory-saas-teamseer-tutorial/ic789638.png "SAML ayarları") 

    a. İçinde **Test e-posta adresleri**, test kullanıcının e-posta adresini yazın. 
  
    b. İçinde **veren** metin kutusuna, hizmet sağlayıcısı veren URL'sini yazın. 
  
    c. **Kaydet** düğmesine tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-teamseer-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-teamseer-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-teamseer-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-teamseer-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-teamseer-test-user"></a>TeamSeer test kullanıcısı oluşturma

TeamSeer için oturum açmak Azure AD kullanıcıları etkinleştirmek için bunlar içinde ShiftPlanning için hazırlanması gerekir. TeamSeer söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **TeamSeer** yönetici olarak şirket site.

2. Aşağıdaki adımları gerçekleştirin:
   
    ![HR yönetici](./media/active-directory-saas-teamseer-tutorial/ic789640.png "ik yönetici")  
 
    a. Git **ik yönetici \> kullanıcılar**.
  
    b. Tıklatın **yeni kullanıcı Sihirbazı'nı çalıştırın**.

3. İçinde **kullanıcı ayrıntıları** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcı ayrıntılarını](./media/active-directory-saas-teamseer-tutorial/ic789641.png "kullanıcı ayrıntıları")

    a. Tür **ad**, **Soyadı**, **kullanıcı adı (e-posta adresi)** sağlamak için ilgili metin kutularına, geçerli bir AAD hesabının.
  
    b. **İleri**’ye tıklayın.

4. İzleyin ekran yeni bir kullanıcı eklemek için yönergeler tıklatıp **son**.

>[!NOTE]
>API Azure AD kullanıcı hesaplarını sağlamak için TeamSeer tarafından sağlanan veya herhangi diğer TeamSeer kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz. 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta TeamSeer için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**TeamSeer için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **TeamSeer**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Çoklu oturum açma ayarlarınızı test etmek isterseniz, erişim paneli açın. Erişim paneli hakkında daha fazla ayrıntı için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_203.png

