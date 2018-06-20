---
title: 'Öğretici: Azure Active Directory Tümleştirme ile DocuSign | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile DocuSign arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: a691288b-84c1-40fb-84bd-5b06878865f0
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: 027bf154fb57e8f324757fd6b32ea6c421bbc705
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36229521"
---
# <a name="tutorial-azure-active-directory-integration-with-docusign"></a>Öğretici: Azure Active Directory Tümleştirme DocuSign ile

Bu öğreticide, Azure Active Directory (Azure AD) ile DocuSign tümleştirmek öğrenin.

DocuSign Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- DocuSign erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için DocuSign (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme DocuSign ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir DocuSign çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden DocuSign ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-docusign-from-the-gallery"></a>Galeriden DocuSign ekleme
Azure AD DocuSign tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden DocuSign eklemeniz gerekir.

**Galeriden DocuSign eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Tıklatın **yeni uygulama** iletişim kutusunun üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **DocuSign**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/docusign-tutorial/tutorial_docusign_search.png)

5. Sonuçlar panelinde seçin **DocuSign**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/docusign-tutorial/tutorial_docusign_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı DocuSign ile test etme

Tekli çalışmaya oturum için Azure AD DocuSign karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının DocuSign ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** DocuSign içinde.

Yapılandırma ve Azure AD çoklu oturum açma DocuSign ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[DocuSign test kullanıcısı oluşturma](#creating-a-docusign-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı DocuSign sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma DocuSign uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile DocuSign yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **DocuSign** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/docusign-tutorial/tutorial_docusign_samlbase.png)

3. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base 64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/docusign-tutorial/tutorial_docusign_certificate.png) 

4. Üzerinde **DocuSign yapılandırma** Azure portal, tıklatın bölümünü **yapılandırma DocuSign** yapılandırma oturum açma penceresini açmak için. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**
    
    ![Çoklu oturum açmayı yapılandırın](./media/docusign-tutorial/tutorial_docusign_configure.png)

5. Farklı bir web tarayıcı penceresinde, oturum açma için sizin **DocuSign Yönetici portalı** yönetici olarak.

6. Sol gezinti menüsünde **etki alanları**.
   
    ![Çoklu oturum açma yapılandırılıyor][51]

7. Sağ bölmede, tıklatın **talep etki alanı**.
   
    ![Çoklu oturum açma yapılandırılıyor][52]

8. Üzerinde **bir etki alanı talep** iletişim, **etki alanı adı** metin kutusuna, şirket etki alanınızı yazın ve ardından **talep**. Etki alanını doğrulayın ve durumu etkin olduğundan emin olun.
   
    ![Çoklu oturum açma yapılandırılıyor][53]

9. Sol tarafındaki menüde tıklatın **kimlik sağlayıcıları**  
   
    ![Çoklu oturum açma yapılandırılıyor][54]
10. Sağ bölmede **kimlik sağlayıcı Ekle**. 
   
    ![Çoklu oturum açma yapılandırılıyor][55]

11. Üzerinde **kimlik sağlayıcı ayarları** sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma yapılandırılıyor][56]

    a. İçinde **adı** metin kutusuna, yapılandırmanızı için benzersiz bir ad yazın. Boşluk kullanmayın.

    b. Yapıştır **SAML varlık kimliği** içine **kimlik sağlayıcısı veren** metin kutusu.

    c. Yapıştır **SAML çoklu oturum açma hizmet URL'si** içine **kimlik sağlayıcısı oturum açma URL'si** metin kutusu.

    d. Yapıştır **Sign-Out URL** içine **kimlik sağlayıcısı oturum kapatma URL'si** metin kutusu.

    e. Seçin **AuthN isteği oturum**.

    f. Olarak **AuthN gönderme isteği tarafından**seçin **POST**.

    g. Olarak **gönderme oturum kapatma isteği tarafından**seçin **almak**.

12. İçinde **özel eşleme özniteliği** bölümünde, Azure AD taleple eşlemek istediğiniz alan seçin. Bu örnekte, **emailaddress** talep değeriyle eşleşen **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**. Bu e-posta talebi için Azure AD'den varsayılan talep adıdır. 
   
    > [!NOTE]
    > Uygun kullanın **kullanıcı tanımlayıcısı** DocuSign kullanıcı eşlemesi Azure AD'den kullanıcıya eşlemek için. Uygun alanını seçin ve kuruluş ayarlarınıza göre uygun değeri girin.
          
    ![Çoklu oturum açma yapılandırılıyor][57]

13. İçinde **kimlik sağlayıcısı sertifikası** 'yi tıklatın **sertifika Ekle**ve Azure AD Portalı'ndan indirilen sertifikayı yükleyin.   
   
    ![Çoklu oturum açma yapılandırılıyor][58]

14. **Kaydet**’e tıklayın.

15. İçinde **kimlik sağlayıcıları** 'yi tıklatın **Eylemler**ve ardından **uç noktaları**.   
   
    ![Çoklu oturum açma yapılandırılıyor][59]
 
16. İçinde **uç noktalarını görüntüle SAML 2.0** bölümünde **DocuSign Yönetici portalı**, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma yapılandırılıyor][60]
   
    a. Kopya **servis sağlayıcı veren URL'sini**, kopyalayıp yapıştırabilir **tanımlayıcısı** textbox üzerinde **DocuSign etki alanı ve URL'leri** bölüm düzeni aşağıdaki Azure portal'ın: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/login/sp/<uniqueID>`.
   
    b. Kopya **hizmet sağlayıcı oturum açma URL'si**, kopyalayıp yapıştırabilir **oturum üzerinde URL'si** textbox üzerinde **DocuSign etki alanı ve URL'leri** bölüm düzeni aşağıdaki Azure portal'ın: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/`.

    ![Çoklu oturum açmayı yapılandırın](./media/docusign-tutorial/tutorial_docusign_url.png)
      
    c.  Tıklatın **Kapat**
    
17. Azure Portal'da tıklatın **kaydetmek**.
    
    ![Çoklu oturum açmayı yapılandırın](./media/docusign-tutorial/tutorial_general_400.png)

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/docusign-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/docusign-tutorial/create_aaduser_02.png) 

3. İletişim kutusunun üstündeki **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/docusign-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/docusign-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-docusign-test-user"></a>DocuSign test kullanıcısı oluşturma

Uygulamanızın desteklediği **zaman kullanıcı hazırlama, sadece** ve kimlik doğrulama kullanıcılar uygulamada otomatik olarak oluşturulduktan sonra.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta DocuSign için kendi erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**DocuSign için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **DocuSign**.

    ![Çoklu oturum açmayı yapılandırın](./media/docusign-tutorial/tutorial_docusign_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli DocuSign parçasında tıklattığınızda, otomatik olarak DocuSign uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)
* [Kullanıcı sağlamayı Yapılandır](docusign-provisioning-tutorial.md)


<!--Image references-->

[1]: ./media/docusign-tutorial/tutorial_general_01.png
[2]: ./media/docusign-tutorial/tutorial_general_02.png
[3]: ./media/docusign-tutorial/tutorial_general_03.png
[4]: ./media/docusign-tutorial/tutorial_general_04.png
[51]: ./media/docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/docusign-tutorial/tutorial_docusign_29.png
[100]: ./media/docusign-tutorial/tutorial_general_100.png

[200]: ./media/docusign-tutorial/tutorial_general_200.png
[201]: ./media/docusign-tutorial/tutorial_general_201.png
[202]: ./media/docusign-tutorial/tutorial_general_202.png
[203]: ./media/docusign-tutorial/tutorial_general_203.png

