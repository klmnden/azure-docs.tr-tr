---
title: 'Öğretici: Azure Active Directory Tümleştirme LinkedIn yükseltmesine ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory arasındaki LinkedIn yükseltmesine yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 2ad9941b-c574-42c3-bd0f-5d6ec68537ef
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: 5b940ac896f95ec796179f44e0dd11dcf6184464
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34589398"
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-elevate"></a>Öğretici: Azure Active Directory Tümleştirme LinkedIn yükseltmesine ile

Bu öğreticide, Azure Active Directory (Azure AD) ile tümleştirme LinkedIn yükseltmesine öğrenin.

Azure AD ile tümleştirme LinkedIn yükseltmesine ile aşağıdaki avantajları sağlar:

- LinkedIn yükseltmesine erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) LinkedIn yükseltmek için açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure Yönetim Portalı'nı yönetme

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme LinkedIn yükseltmesine ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir LinkedIn yükseltmesine çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Bu gerekli olmadığı sürece, üretim ortamınızın kullanmamanız gerekir.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin.
Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden LinkedIn yükseltmesine ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-linkedin-elevate-from-the-gallery"></a>Galeriden LinkedIn yükseltmesine ekleme
LinkedIn yükseltmesine tümleştirilmesi Azure AD'ye yapılandırmak için LinkedIn yükseltmesine Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**LinkedIn yükseltmesine Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure Yönetim Portalı](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi.

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]

3. Tıklatın **Ekle** iletişim kutusunun üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **LinkedIn yükseltmesine**. Sonuçları panelinden tıklatın **LinkedIn yükseltmesine** uygulama eklemek için.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedinElevate_000.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı LinkedIn yükseltmesine ile test etme.

Tekli çalışmaya oturum için Azure AD LinkedIn yükseltmesine karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ilgili LinkedIn yükseltmesine kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** LinkedIn yükseltmesine içinde.

Yapılandırma ve Azure AD çoklu oturum açma LinkedIn yükseltmesine ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir LinkedIn yükseltmesine test kullanıcısı oluşturma](#creating-a-linkedin-elevate-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure Yönetim Portalı'nda etkinleştirin ve çoklu oturum açma LinkedIn yükseltmesine uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma LinkedIn yükseltmesine ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda üzerinde **LinkedIn yükseltmesine** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda, olarak **modu** seçin **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedin_01.png)

3. Farklı web tarayıcısı penceresinde LinkedIn yükseltmesine kiracınız yönetici olarak oturum.

4. İçinde **hesap Merkezi'nde**, tıklatın **genel ayarları** altında **ayarları**. Ayrıca, seçin **yükseltme - AAD Test yükseltmesine** aşağı açılan listeden.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_admin_01.png)

5. Tıklayın **veya yük ve tek tek alanların formdan kopyalamak için burayı tıklatın** kopyalayıp **varlık kimliği** ve **onaylama tüketici erişim (ACS) URL'si**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_admin_03.png)

6. Azure Portal'da altında **LinkedIn yükseltme etki alanı ve URL'leri**, içinde SSO yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP başlatılan** modu

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_signon_01.png)

    a. İçinde **tanımlayıcısı** metin girin **varlık kimliği** LinkedIn portalından kopyalandığından 

    b. İçinde **yanıt URL'si** metin girin **onaylama tüketici erişim (ACS) Url** LinkedIn portalından kopyalandığından

7. SSO içinde yapılandırmak istiyorsanız, **SP tarafından başlatılan**, ardından yapılandırma bölümündeki Gelişmiş URL Göster ayarı seçeneğini tıklayın ve oturum açma URL'SİNDE aşağıdaki desenle yapılandırın:

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=elevate&applicationInstanceId=<InstanceId>` 

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_signon_02.png) 

8. LinkedIn yükseltmesine uygulamanızı SAML onaylar SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemelerini ekleyin gerektiren belirli bir biçimde bekliyor. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir. Varsayılan değer olan **kullanıcı tanımlayıcısı** olan **user.userprincipalname** ancak LinkedIn yükseltmesine, bekliyor bu kullanıcının e-posta adresi ile eşlendi. Bunun için kullanabileceğiniz **user.mail** özniteliği listeden veya kuruluş yapılandırmanızı temel alarak uygun öznitelik değeri kullanın.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-linkedinElevate-tutorial/updateusermail.png)

9. İçinde **kullanıcı öznitelikleri** 'yi tıklatın **Görünüm ve diğer tüm kullanıcı özniteliklerini düzenleme** ve özniteliklerini ayarlayın. Adlı başka bir talep eklemeniz **departmanı** ve değeri eşlenmesi gerekiyor **user.department**.

    | Öznitelik Adı | Öznitelik Değeri |
    | --- | --- |
    | bölüm| User.Department |

      ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-linkedinElevate-tutorial/userattribute.png)

      a. Aşağıdaki - gösterildiği gibi Ekle'ye departmanı öznitelik Ekle öznitelik'özniteliği Ayrıntılar sayfasını açmak için tıklayın

      ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-linkedinElevate-tutorial/adduserattribute.png)

      b. Tıklayın **Tamam** öznitelik kaydetmek için.

      c. Özniteliğin adını değiştirmek **emailaddress** için **e-posta**.

10. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve XML dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedinElevate_certificate.png) 

11. **Kaydet**’e tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_400.png)

12. Git **LinkedIn yönetici ayarları** bölümü. Yalnızca karşıya yükleme XML dosyası seçeneğini tıklatarak Azure portalından indirdiğiniz XML dosyasını karşıya yükleyin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_metadata_03.png)

13. Tıklatın **üzerinde** SSO'yu etkinleştirmek için. SSO durum değişecektir **bağlı** için **bağlandı**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_admin_05.png)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure Yönetim Portalı'nda bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure Yönetim Portalı**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_01.png) 

2. Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar** kullanıcıların listesini görüntülemek için.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_02.png) 

3. İletişim kutusunun üstündeki **Ekle** açmak için **kullanıcı** iletişim.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.

### <a name="creating-a-linkedin-elevate-test-user"></a>Bir LinkedIn yükseltmesine test kullanıcısı oluşturma

LinkedIn yükseltmesine uygulama zaman kullanıcı sağlama ve kimlik doğrulama kullanıcılar uygulamada otomatik olarak oluşturulacak sonra hemen destekler. Yönetici ayarları LinkedIn yükseltmesine portal Çevir ' anahtar sayfa **otomatik olarak ata lisansları** zaman içinde etkin sadece sağlama ve bu da bir lisansı kullanıcıya atar. Daha fazla ayrıntı bulabilirsiniz, LinkedIn yükseltmesine de destekler otomatik kullanıcı sağlamayı [burada](active-directory-saas-linkedinelevate-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

   ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-linkedinElevate-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, kendi erişim izni verme LinkedIn yükseltmek için Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı atama][200] 

**LinkedIn yükseltmek için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201]

2. Uygulamalar listesinde **LinkedIn yükseltmesine**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedinElevate_0001.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.

### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli LinkedIn yükseltmesine döşemeyi tıkladığınızda, Azure oturum açma sayfasına almanız gerekir ve üzerinde başarılı oturum açma sonra bunu LinkedIn yükseltmesine uygulamanıza almanız gerekir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Öğretici: LinkedIn yükseltmesine otomatik kullanıcı hazırlama Azure Active Directory ile yapılandırma](active-directory-saas-linkedinelevate-provisioning-tutorial.md)
* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)
* [Kullanıcı sağlamayı Yapılandır](active-directory-saas-linkedinelevate-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_203.png