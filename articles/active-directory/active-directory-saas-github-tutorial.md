---
title: "Öğretici: Azure Active Directory Tümleştirme github | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve GitHub arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 4395bd95-05de-4deb-87a5-dc3bc8ac4d95
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 2a0e1df5244ef977bdcccc5bcfea615a05efa3bd
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-github"></a>Öğretici: Azure Active Directory Tümleştirme github

Bu öğreticide, Azure Active Directory (Azure AD) ile GitHub tümleştirmek öğrenin.

GitHub Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- GitHub erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için GitHub (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure Yönetim Portalı'nı yönetme

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme GitHub ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir GitHub çoklu oturum açma etkin abonelik


> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.


Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Bu gerekli olmadığı sürece, üretim ortamınızın kullanmamanız gerekir.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden GitHub ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama


## <a name="adding-github-from-the-gallery"></a>Galeriden GitHub ekleme
Azure AD GitHub tümleştirilmesi yapılandırmak için GitHub Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden GitHub eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure Yönetim Portalı](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Tıklatın **Ekle** iletişim kutusunun üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Github.com'u**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-github-tutorial/tutorial_github_search02.png)

5. Sonuçlar panelinde seçin **GitHub**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-github-tutorial/tutorial_github_search_result02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı GitHub ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen github bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı github'da arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** github'da.

Yapılandırma ve Azure AD çoklu oturum açma GitHub ile test etmek için aşağıdaki yapı taşları tamamlanması gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[GitHub test kullanıcısı oluşturma](#creating-a-GitHub-test-user)**  - Britta Simon, karşılık gelen her, Azure AD gösterimine bağlı GitHub sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure Yönetim Portalı'nda etkinleştirin ve çoklu oturum açma GitHub uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma GitHub ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda üzerinde **GitHub** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda, olarak **modu** seçin **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-github-tutorial/tutorial_github_01.png)

3. Üzerinde **GitHub etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-github-tutorial/tutorial_github_saml011.png)

    a. İçinde **oturum açma URL'si** metin değeri olarak yazın:`https://github.com/orgs/<entity-id>/sso`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://github.com/orgs/<entity-id>`

    > [!NOTE] 
    > Lütfen bu gerçek değerlerin olmadığına dikkat edin. Bu değerleri tanımlayıcısı ve gerçek oturum açma URL'si ile güncelleştirmeniz gerekir. Burada dizesinin benzersiz değeri tanımlayıcıda kullanmanızı öneririz. Bu değerleri almak için GitHub yönetici bölümüne gidin. 

4. Üzerinde **kullanıcı öznitelikleri** bölümünde, select **kullanıcı tanımlayıcısı** user.mail olarak.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-github-tutorial/tutorial_github_attribute_new01.png)
    
5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **yeni sertifika oluştur**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-github-tutorial/tutorial_github_03.png)

6. Üzerinde **yeni sertifika oluştur** iletişim kutusunda, Takvim simgesine tıklayın ve bir **sona erme tarihi**. Ardından **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-github-tutorial/tutorial_general_300.png)

7. Üzerinde **SAML imzalama sertifikası** bölümünde, select **yeni sertifika etkin hale getirin** tıklatıp **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-github-tutorial/tutorial_github_04.png)

8. Açılır pencere üzerinde **geçiş sertifikası** penceresinde tıklatın **Tamam**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-github-tutorial/tutorial_general_400.png)

9. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-github-tutorial/tutorial_github_05.png) 

10. Üzerinde **GitHub yapılandırma** 'yi tıklatın **yapılandırma GitHub** açmak için **yapılandırma oturum açma** penceresi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-github-tutorial/tutorial_github_06.png) 

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-github-tutorial/tutorial_github_07.png)

11. Farklı web tarayıcısı penceresinde GitHub kuruluş sitenize yönetici olarak oturum açın.

12. Gidin **ayarları** tıklatıp **güvenlik**

    ![Ayarlar](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_03.png)

13. Denetleme **etkinleştirmek SAML kimlik doğrulaması** kutusu, çoklu oturum açma yapılandırma alanları ortaya. Ardından, çoklu oturum açma URL'si Azure AD yapılandırmasını güncelleştirmek için tek oturum açma URL değeri kullanın.

    ![Ayarlar](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_13.png)

14. Aşağıdaki alanları yapılandırın:

    a. **URL üzerinde oturum**: girin **SAML çoklu oturum açma hizmet URL'si** gelen **yapılandırma GitHub** Azure AD bölüm

    b. **Veren**: girin **SAML varlık kimliği** gelen **yapılandırma GitHub** Azure AD bölüm

    c. **Ortak sertifika**: indirilen sertifika Azure AD'den bir Not Defteri'nde açın ve "Sertifika başlayın" ve "Son SERTİFİKAYI" dahil olmak üzere içerik kopyalama

    ![Ayarlar](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_051.png)

15. Tıklayın **Test SAML Yapılandırması** onaylamak için hiçbir doğrulama hataları veya SSO sırasında hatalar.

    ![Ayarlar](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_06.png)

16. Tıklatın **Kaydet**

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure Yönetim Portalı'nda bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure Yönetim Portalı**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-github-tutorial/create_aaduser_01.png) 

2. Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar** kullanıcıların listesini görüntülemek için.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-github-tutorial/create_aaduser_02.png) 

3. İletişim kutusunun üstündeki **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-github-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-github-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın. 


### <a name="creating-a-github-test-user"></a>GitHub test kullanıcısı oluşturma

Azure AD kullanıcıların GitHub oturum etkinleştirmek için bunların GitHub sağlanmalıdır.  
GitHub söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Kullanıcı hesaplarını sağlamak için aşağıdaki adımları gerçekleştirin:**

1. GitHub şirket sitenize yönetici olarak oturum açın.

2. Tıklatın **kişiler**.

    ![Kişiler](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_08.png "kişiler")

3. Tıklatın **davet üye**.

    ![Kullanıcıları davet](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_09.png "kullanıcıları davet et")

4. Üzerinde **davet üye** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    a. İçinde **e-posta** metin kutusuna, Britta Simon hesabı e-posta adresini yazın.

    ![Kişileri davet edin](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_10.png "kişileri davet edin")
    
    b. Tıklatın **Gönder davet**.

    ![Kişileri davet edin](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_11.png "kişileri davet edin")

    > [!NOTE]
    > Azure Active Directory hesap sahibi bir e-posta alır ve onu etkinleştirilmeden önce kendi hesabı onaylamak için bağlantıyı izleyin.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta GitHub için kendi erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**GitHub için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Github.com'u**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-github-tutorial/tutorial_github_search_result021.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    


### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli GitHub parçasında tıklattığınızda, GitHub uygulamanıza açan. Hesap ancak sonra gerekirse, kişisel hesabıyla oturum kuruluş olarak günlüğü.


## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-github-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-github-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-github-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-github-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-github-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-github-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-github-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-github-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-github-tutorial/tutorial_general_203.png
