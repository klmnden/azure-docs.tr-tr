---
title: "Öğretici: Azure Active Directory Tümleştirme FirmPlay - işe alma için çalışan hakları ile | Microsoft Docs"
description: "Çoklu oturum açma FirmPlay - çalışan hakları işe alma için Azure Active Directory arasındaki yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a6799629-7546-43f8-a966-956db32864b1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2017
ms.author: jeedes
ms.openlocfilehash: 3cddd5b9508159089bf344dbb3882d462799747c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-firmplay---employee-advocacy-for-recruiting"></a>Öğretici: Azure Active Directory Tümleştirme ile FirmPlay - işe alma için çalışan hakları

Bu öğreticide, Azure Active Directory (Azure AD) ile işe alma için çalışan hakları FirmPlay - tümleştirmek öğrenin.

FirmPlay - Azure AD ile işe alma için çalışan hakları tümleştirme ile aşağıdaki avantajları sağlar:

- FirmPlay - çalışan hakları işe alma için erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak FirmPlay - işe alma (çoklu oturum açma) Azure AD hesaplarına sahip çalışan hakları için açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure Yönetim Portalı'nı yönetme

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme FirmPlay - işe alma, çalışan hakları ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- FirmPlay - çoklu oturum açma etkin abonelik işe alma için çalışan hakları


> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.


Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Bu gerekli olmadığı sürece, üretim ortamınızın kullanmamanız gerekir.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. İşe alma galerisinden çalışan hakları FirmPlay - ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama


## <a name="adding-firmplay---employee-advocacy-for-recruiting-from-the-gallery"></a>İşe alma galerisinden çalışan hakları FirmPlay - ekleme
FirmPlay - Azure AD'ye işe alma için çalışan hakları tümleştirmesini yapılandırmak için çalışan hakları galerisinden işe alma listenize yönetilen SaaS uygulamaları için FirmPlay - eklemeniz gerekir.

**İşe alma Galerisi'nden çalışan hakları FirmPlay - eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure Yönetim Portalı](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Tıklatın **Ekle** iletişim kutusunun üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **FirmPlay - işe alma için çalışan hakları**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_001.png)

5. Sonuçlar panelinde seçin **FirmPlay - işe alma için çalışan hakları**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma FirmPlay - işe alma "Britta Simon" adlı bir test kullanıcıyı temel alarak çalışan hakları ile test etme.

Çoklu oturum açma çalışmak özelliğini için Azure AD FirmPlay ne karşılık gelen kullanıcı bilmek ister - çalışan hakları işe alma için bir kullanıcı Azure AD'de. Diğer bir deyişle, bir Azure AD kullanıcısının FirmPlay - çalışan hakları işe alma için ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** FirmPlay - işe alma için çalışan hakları içinde.

Yapılandırma ve Azure AD çoklu oturum açma FirmPlay ile-test etmek için işe alma, çalışan hakları, aşağıdaki yapı taşları tamamlanması gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[İşe alma test kullanıcısı için çalışan hakları FirmPlay - oluşturma](#creating-a-firmplay---employee-advocacy-for-recruiting-test-user)**  - FirmPlay içinde karşılık gelen Britta Simon, olmasını: çalışan başka bir deyişle işe alma için hakları bağlı her, Azure AD gösterimi.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure Yönetim Portalı'nda etkinleştirin ve çoklu oturum açma FirmPlay - çalışan hakları işe alma uygulaması için yapılandırın.

**Azure AD çoklu oturum açma FirmPlay - işe alma, çalışan hakları yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda üzerinde **FirmPlay - işe alma için çalışan hakları** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda, olarak **modu** seçin **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_01.png)

3. Üzerinde **FirmPlay - işe alma etki alanı ve URL'ler için çalışan hakları** bölümünde **oturum üzerinde URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<your-subdomain>.firmplay.com/`

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_02.png)

    > [!NOTE] 
    > Lütfen bu gerçek değer olmadığını unutmayın. Bu değer gerçek oturum üzerinde URL ile güncelleştirmeniz gerekir. Kişi [FirmPlay - işe alma destek ekibi için çalışan hakları](mailto:engineering@firmplay.com) bu değeri alınamıyor. 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **yeni sertifika oluştur**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_03.png)   

5. Üzerinde **yeni sertifika oluştur** iletişim kutusunda, Takvim simgesine tıklayın ve bir **sona erme tarihi**. Ardından **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-firmplay-tutorial/tutorial_general_300.png)

6. Üzerinde **SAML imzalama sertifikası** bölümünde, select **yeni sertifika etkin hale getirin** tıklatıp **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_04.png)

7. Açılır pencere üzerinde **geçiş sertifikası** penceresinde tıklatın **Tamam**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-firmplay-tutorial/tutorial_general_400.png)

8. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (base64)** ve sertifika dosyayı bilgisayarınıza kaydedin. 

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_05.png) 

9. Üzerinde **FirmPlay - işe alma yapılandırması için çalışan hakları** 'yi tıklatın **FirmPlay - çalışan hakları işe alma için yapılandırma** açmak için **yapılandırma oturum açma** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_06.png) 

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_07.png)

10. Uygulamanız için yapılandırılmış SSO almak için başvurun [FirmPlay - işe alma destek ekibi için çalışan hakları](mailto:engineering@firmplay.com) ve ile aşağıdakileri sağlar: 

    • İndirilen **sertifika dosyası**

    • **SAML çoklu oturum açma hizmeti URL'si**

    • **SAML varlık kimliği**

    • **Oturum kapatma URL'si**
  

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure Yönetim Portalı'nda bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure Yönetim Portalı**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-firmplay-tutorial/create_aaduser_01.png) 

2. Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar** kullanıcıların listesini görüntülemek için.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-firmplay-tutorial/create_aaduser_02.png) 

3. İletişim kutusunun üstündeki **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-firmplay-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-firmplay-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın. 



### <a name="creating-a-firmplay---employee-advocacy-for-recruiting-test-user"></a>İşe alma test kullanıcısı için çalışan hakları FirmPlay - oluşturma

Bu bölümde, Britta Simon FirmPlay - işe alma için çalışan hakları adlı bir kullanıcı oluşturun. Lütfen çalışmak [FirmPlay - işe alma destek ekibi için çalışan hakları](mailto:engineering@firmplay.com) FirmPlay - çalışan hakları işe alma platform için kullanıcıları eklemek için.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta FirmPlay - işe alma için çalışan hakları için kendi erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Britta Simon FirmPlay - işe alma, çalışan hakları atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **FirmPlay - işe alma için çalışan hakları**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_50.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    


### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

FirmPlay - çalışan hakları erişim panelinde, işe alma döşemeye tıklattığınızda, otomatik olarak FirmPlay - çalışan hakları işe alma uygulaması için açan.


## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_203.png