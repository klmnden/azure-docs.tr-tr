---
title: 'Öğretici: Azure Active Directory Tümleştirme yakınlaştırma ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve yakınlaştırma arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0ebdab6c-83a8-4737-a86a-974f37269c31
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/28/2017
ms.author: jeedes
ms.openlocfilehash: 33b5c918fc5de301d0f6df73bda53e53c1446088
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35972338"
---
# <a name="tutorial-azure-active-directory-integration-with-zoom"></a>Öğretici: Yakınlaştırma Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile yakınlaştırma tümleştirmek öğrenin.

Yakınlaştırma Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Yakınlaştırma erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak (çoklu oturum açma) yakınlaştırmak için Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme yakınlaştırma ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir yakınlaştırma çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden yakınlaştırma ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-zoom-from-the-gallery"></a>Galeriden yakınlaştırma ekleme
Azure AD'ye yakınlaştırma tümleştirmesini yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden yakınlaştırma eklemeniz gerekir.

**Yakınlaştırma Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **yakınlaştırma**seçin **yakınlaştırma** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Yakınlaştır](./media/zoom-tutorial/tutorial_zoom_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı yakınlaştırma ile test etme.

Tekli çalışmaya oturum için Azure AD yakınlaştırma karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve yakınlaştırma ilgili kullanıcı arasındaki bağlantıyı ilişki kurulması gerekir.

Yakınlaştırma, değeri atamak **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma yakınlaştırma ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Yakınlaştırma test kullanıcısı oluşturma](#create-a-zoom-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı yakınlaştırma sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma yakınlaştırma uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile yakınlaştırma yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **yakınlaştırma** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/zoom-tutorial/tutorial_zoom_samlbase.png)

3. Üzerinde **yakınlaştırma etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Etki alanı ve URL'leri tek oturum açma bilgilerini Yakınlaştır](./media/zoom-tutorial/tutorial_zoom_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<companyname>.zoom.us`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `<companyname>.zoom.us`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [yakınlaştırma istemci destek ekibi](https://support.zoom.us/hc) bu değerleri almak için.

4. Yakınlaştırma uygulaması SAML onaylar SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemelerini ekleyin gerektiren belirli bir biçimde bekliyor. Bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirmesi sayfasında bölüm. 

    ![Çoklu oturum açmayı yapılandırın](./media/zoom-tutorial/tutorial_attribute.png)

5. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, önceki görüntüde gösterildiği gibi SAML belirteci özniteliği yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri | Namespace değeri |
    | ------------------- | -----------|--------- |    
    | E-posta adresi | User.Mail | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/mail`|
    | Ad | User.givenName | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`|
    | Soyadı | User.surname | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname `|
    | Telefon numarası | User.telephoneNumber | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/phone`|
    | Bölüm | User.Department | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/department`|

    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/zoom-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/zoom-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.

    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. İçinde **Namespace** metin kutusuna, ilgili satır için gösterilen ad alanı değeri yazın.
    
    e. **Tamam**’a tıklayın. 
 
6. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/zoom-tutorial/tutorial_zoom_certificate.png)

7. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/zoom-tutorial/tutorial_general_400.png)

8. Üzerinde **yakınlaştırma yapılandırma** 'yi tıklatın **yapılandırma yakınlaştırma** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Yakınlaştırma yapılandırma](./media/zoom-tutorial/tutorial_zoom_configure.png)

9. Farklı web tarayıcısı penceresinde yakınlaştırma şirket sitenize yönetici olarak oturum açın.

10. Tıklatın **çoklu oturum açma** sekmesi.
   
    ![Oturum açma tek sekme](./media/zoom-tutorial/IC784700.png "çoklu oturum açma")

11. Tıklatın **güvenlik denetimi** sekmesini tıklatın ve ardından Git **çoklu oturum açma** ayarlar.

12. Çoklu oturum açma bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma bölüm](./media/zoom-tutorial/IC784701.png "çoklu oturum açma")
   
    a. İçinde **oturum açma sayfası URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan.
   
    b. İçinde **oturum kapatma sayfası URL'si** metin değerini yapıştırın **Sign-Out URL** Azure portalından kopyalanan.
     
    c. Base-64 kodlanmış sertifikanızı Not Defteri'nde açın, içeriğini, panoya kopyalayın ve yapıştırın kendisine **kimlik sağlayıcısı sertifikası** metin kutusu.

    d. İçinde **veren** metin değerini yapıştırın **SAML varlık kimliği** Azure portalından kopyalanan. 

    e. **Kaydet**’e tıklayın.

    > [!NOTE] 
    > Daha fazla bilgi için yakınlaştırma belgeleri ziyaret edin [https://zoomus.zendesk.com/hc/en-us/articles/115005887566](https://zoomus.zendesk.com/hc/en-us/articles/115005887566)

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/zoom-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/zoom-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/zoom-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/zoom-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-zoom-test-user"></a>Yakınlaştırma test kullanıcısı oluşturma

Yakınlaştırma için oturum açmak Azure AD kullanıcıları etkinleştirmek için bunların yakınlaştırma sağlanmalıdır. Yakınlaştırma söz konusu olduğunda, sağlama bir el ile bir görevdir.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:

1. Oturum, **yakınlaştırma** yönetici olarak şirket site.
 
2. Tıklatın **hesap yönetimi** sekmesini ve ardından **kullanıcı yönetimi**.

3. Kullanıcı Yönetimi bölümünde tıklayın **kullanıcıları eklemek**.
   
    ![Kullanıcı Yönetimi](./media/zoom-tutorial/IC784703.png "kullanıcı yönetimi")

4. Üzerinde **kullanıcıları eklemek** sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcıları ekleme](./media/zoom-tutorial/IC784704.png "kullanıcı ekleme")
   
    a. Olarak **kullanıcı türü**seçin **temel**.

    b. İçinde **e-postaları** metin kutusuna, türü e-posta adresi geçerli bir Azure ad hesabına sağlamak istiyor.

    c. **Ekle**'ye tıklayın.

> [!NOTE]
> Kullanıcı hesapları sağlama Azure Active Directory yakınlaştırma API'leri sağladığı veya herhangi diğer yakınlaştırma kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta yakınlaştırmak için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Yakınlaştırma için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **yakınlaştırma**.

    ![Uygulamalar listesinde yakınlaştırma bağlantı](./media/zoom-tutorial/tutorial_zoom_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.

Erişim paneli yakınlaştırma parçasında tıklattığınızda, otomatik olarak yakınlaştırma uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/zoom-tutorial/tutorial_general_01.png
[2]: ./media/zoom-tutorial/tutorial_general_02.png
[3]: ./media/zoom-tutorial/tutorial_general_03.png
[4]: ./media/zoom-tutorial/tutorial_general_04.png

[100]: ./media/zoom-tutorial/tutorial_general_100.png

[200]: ./media/zoom-tutorial/tutorial_general_200.png
[201]: ./media/zoom-tutorial/tutorial_general_201.png
[202]: ./media/zoom-tutorial/tutorial_general_202.png
[203]: ./media/zoom-tutorial/tutorial_general_203.png

