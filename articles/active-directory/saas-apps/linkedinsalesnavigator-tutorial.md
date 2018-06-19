---
title: 'Öğretici: Azure Active Directory Tümleştirme LinkedIn satış Gezgini ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile LinkedInSalesNavigator arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 7a9fa8f3-d611-4ffe-8d50-04e9586b24da
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: jeedes
ms.openlocfilehash: 15d8e14c0df4ddbf4526cda62af97a1f5b7feb01
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35980633"
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-sales-navigator"></a>Öğretici: Azure Active Directory Tümleştirme ile LinkedIn satış Gezgini

Bu öğreticide, Azure Active Directory (Azure AD) ile LinkedIn satış Gezgini tümleştirmek öğrenin.

LinkedIn satış Gezgini Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- LinkedIn satış Gezgini erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) LinkedIn satış Gezgini açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, Gözat [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme LinkedIn satış Gezgini ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir LinkedIn satış Gezgini çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmaktan kaçının.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden LinkedIn satış Gezgini ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-linkedin-sales-navigator-from-the-gallery"></a>Galeriden LinkedIn satış Gezgini ekleme
Azure AD LinkedIn satış Gezgini tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden LinkedIn satış Gezgini eklemeniz gerekir.

**Galeriden LinkedIn satış Gezgini eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Tıklatın **yeni uygulama** iletişim kutusunun üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **LinkedIn satış Gezgini**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_search.png)

5. Sonuçlar panelinde seçin **LinkedIn satış Gezgini**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma LinkedIn satış "Britta Simon" adlı bir test kullanıcı tabanlı Gezgini ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen LinkedIn satış Gezgininde bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı LinkedIn satış Gezgininde arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** LinkedIn satış Gezgininde.

Yapılandırma ve Azure AD çoklu oturum açma LinkedIn satış Gezgini ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir LinkedIn satış Gezgini test kullanıcısı oluşturma](#creating-a-linkedin-sales-navigator-test-user)**  - LinkedIn satış Gezgininde, kullanıcının Azure AD gösterimini bağlı Britta Simon, karşılık gelen sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma LinkedIn satış Gezgini uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma LinkedIn satış Gezgini ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **LinkedIn satış Gezgini** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim, **modu** seçin **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_samlbase.png)

3. Farklı web tarayıcısı penceresinde için oturum, **LinkedIn satış Gezgini** yönetici olarak Web sitesi.

4. İçinde **hesap Merkezi'nde**, tıklatın **genel ayarları** altında **ayarları**. Ayrıca, seçin **satış Gezgini** aşağı açılan listeden.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_01.png)

5. Tıklatın **veya yük ve tek tek alanların formdan kopyalamak için burayı tıklatın** kopyalayıp **varlık kimliği** ve **onaylama tüketici erişim (ACS) Url**.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_031.png)

6. Azure Portal'da altında **LinkedIn satış Gezgini etki alanı ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modunda başlatılır.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url1.png)

    a. İçinde **tanımlayıcısı** metin girin **varlık kimliği** LinkedIn portalından kopyalandığından 

    b. İçinde **yanıt URL'si** metin girin **onaylama tüketici erişim (ACS) Url** LinkedIn portalından kopyalandığından

7. Denetleyin **Göster Gelişmiş URL ayarları**, uygulamada yapılandırmak istiyorsanız **SP** modunda başlatılır.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url2.png)

    İçinde **oturum açma URL'si** metin kutusuna, şu biçimi kullanarak değeri yazın: `https://www.linkedin.com/checkpoint/enterprise/login/<account id>?application=salesNavigator`

8. **LinkedIn satış Gezgini** uygulama SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemelerini ekleyin gerektiren belirli bir biçimde SAML onaylar bekler. Aşağıdaki ekran görüntüsünde bir örneği gösterir. Varsayılan değer olan **kullanıcı tanımlayıcısı** olan **user.userprincipalname** ancak LinkedIn satış Gezgini kullanıcının e-posta adresiyle eşlenmesi için bekliyor. Kullanabileceğiniz **user.mail** özniteliği listeden veya kuruluş yapılandırmanızı temel alarak uygun öznitelik değeri kullanın. 

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinsalesnavigator-tutorial/updateusermail.png)
    
9. İçinde **kullanıcı öznitelikleri** 'yi tıklatın **Görünüm ve diğer tüm kullanıcı özniteliklerini düzenleme** ve özniteliklerini ayarlayın. Kullanıcı adında dört talep eklemesi gerekiyor **e-posta**, **departmanı**, **firstname**, ve **lastname** ve ile eşlenecek değer ise **user.mail**, **user.department**, **user.givenname**, ve **user.surname** sırasıyla

    | Öznitelik Adı | Öznitelik Değeri |
    | --- | --- |    
    | e-posta| User.Mail |
    | bölüm| User.Department |
    | FirstName| User.givenName |
    | Soyadı| User.surname |
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinsalesnavigator-tutorial/userattribute.png)
    
    a. Tıklayın **özniteliği eklemek** özniteliği iletişim kutusunu açın.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinsalesnavigator-tutorial/tutorial_attribute_04.png)
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinsalesnavigator-tutorial/tutorial_attribute_05.png)
   
    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. Tıklatın **Tamam**

10. Aşağıdaki adımları gerçekleştirin **adı** öznitelik -

    a. Özniteliği açmak için tıklatın **öznitelik Düzenle** penceresi.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinsalesnavigator-tutorial/url_update.png)

    b. URL değerinden silme **ad alanı**.
    
    c. Tıklatın **Tamam** ayarı kaydetmek için.

11. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve XML dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_certificate.png) 

12. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinsalesnavigator-tutorial/tutorial_general_400.png)

13. Git **LinkedIn yönetici ayarları** bölümü. Tıklatın **karşıya yükleme XML dosyası** Azure portalından indirdiğiniz meta veri XML dosyasını karşıya yükleyin.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_metadata_03.png)

14. Tıklatın **üzerinde** SSO'yu etkinleştirmek için. SSO durumu değişir **bağlı** için **bağlandı**

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_05.png)


> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinsalesnavigator-tutorial/create_aaduser_01.png) 

2. Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinsalesnavigator-tutorial/create_aaduser_02.png) 

3. İletişim kutusunun üstündeki **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinsalesnavigator-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinsalesnavigator-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-linkedin-sales-navigator-test-user"></a>Bir LinkedIn satış Gezgini test kullanıcısı oluşturma

Bağlantılı satış Gezgin uygulaması sadece kullanıcı zamanı (JIT) sağlama ve kimlik doğrulama kullanıcılar uygulamada otomatik olarak oluşturulduktan sonra destekler. Etkinleştirme **otomatik olarak lisansları atama** kullanıcıya bir lisans atamak için.
   
   ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinsalesnavigator-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta LinkedIn satış Gezgini'ne erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**LinkedIn satış Gezgini Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **LinkedIn satış Gezgini**.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli LinkedIn satış Gezgini parçasında tıklattığınızda, kişisel LinkedIn hesap ayrıntılarını sağlamak için sahip olduğu kuruluş sayfasına yönlendirilmeniz gerekir. Kişisel hesabınızı LinkedIn iş hesabınızla bağlar. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://msdn.microsoft.com/library/dn308586). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_01.png
[2]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_02.png
[3]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_03.png
[4]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_04.png

[100]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_100.png

[200]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_200.png
[201]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_201.png
[202]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_202.png
[203]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_203.png

