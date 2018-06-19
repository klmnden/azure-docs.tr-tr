---
title: 'Öğretici: Azure Active Directory Tümleştirme Qumu bulut ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory Qumu bulut arasındaki yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d8c4a97b-4de6-49d4-b64e-42222c2ec6c9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2018
ms.author: jeedes
ms.openlocfilehash: b4a015a1fe101b32e30940789c9768685e52b21b
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35982204"
---
# <a name="tutorial-azure-active-directory-integration-with-qumu-cloud"></a>Öğretici: Azure Active Directory Tümleştirme Qumu bulut ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Qumu bulut tümleştirme öğrenin.

Qumu bulut Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Qumu bulut erişimi olan Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak Qumu buluta (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Qumu bulut ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Qumu bulut çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Qumu bulut ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-qumu-cloud-from-the-gallery"></a>Galeriden Qumu bulut ekleme
Azure AD Qumu bulut tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Qumu bulut eklemeniz gerekir.

**Galeriden Qumu bulut eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Qumu bulut**seçin **Qumu bulut** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Qumu bulut](./media/qumucloud-tutorial/tutorial_qumucloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve Qumu bulut ile Azure AD çoklu oturum açmayı test "Britta Simon" adlı bir test kullanıcı tabanlı.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Qumu bulutta bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı Qumu bulutta arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Qumu bulut ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Qumu bulut test kullanıcısı oluşturma](#create-a-qumu-cloud-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Qumu bulut sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Qumu bulut uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Qumu bulut ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Qumu bulut** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/qumucloud-tutorial/tutorial_qumucloud_samlbase.png)

3. Üzerinde **Qumu bulut etki alanı ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modu tarafından başlatılan:

    ![Qumu bulut etki alanı ve URL'leri tek oturum açma bilgileri](./media/qumucloud-tutorial/tutorial_qumucloud_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.qumucloud.com/saml/SSO`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.qumucloud.com/saml/SSO`

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![Qumu bulut etki alanı ve URL'leri tek oturum açma bilgileri](./media/qumucloud-tutorial/tutorial_qumucloud_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.qumucloud.com`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler, gerçek tanımlayıcı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Kişi [Qumu bulut istemci destek ekibi](mailto:support@qumu.com) bu değerleri almak için.

4. Qumu bulut uygulaması SAML onaylar belirli bir biçimde bekliyor. Lütfen bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirmesi sayfasında bölüm. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir.
    
    ![Çoklu oturum açmayı yapılandırın](./media/qumucloud-tutorial/attribute.png)
    
5. Tıklatın **Görünüm ve diğer tüm kullanıcı özniteliklerini düzenleme** onay kutusu **kullanıcı öznitelikleri** öznitelikleri genişletmek için bölüm. Her biri görüntülenen öznitelikleri - üzerinde aşağıdaki adımları uygulayın

    | Öznitelik Adı | Öznitelik Değeri |
    | ---------------| --------------- |    
    | urn:oid:2.5.4.42 | User.givenName |
    | urn:oid:2.5.4.4 | User.surname |
    | urn:oid:0.9.2342.19200300.100.1.3 | User.Mail |
    | urn:oid:0.9.2342.19200300.100.1.1 | User.userPrincipalName |

    a. Özniteliği açmak için tıklatın **öznitelik Düzenle** penceresi.

    ![Çoklu oturum açmayı yapılandırın](./media/qumucloud-tutorial/tutorial_attribute_04.png)

    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.

    ![Çoklu oturum açmayı yapılandırın](./media/qumucloud-tutorial/tutorial_attribute_05.png)

    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. Tutmak **Namespace** metin kutusu boş.
    
    e. **Tamam**’a tıklayın.

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/qumucloud-tutorial/tutorial_qumucloud_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/qumucloud-tutorial/tutorial_general_400.png)
    
7. Çoklu oturum açma yapılandırmak için **Qumu bulut** yan, indirilen göndermek için ihtiyacınız **meta veri XML** için [Qumu bulut destek ekibi](mailto:support@qumu.com). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/qumucloud-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/qumucloud-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/qumucloud-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/qumucloud-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-qumu-cloud-test-user"></a>Qumu bulut test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon Qumu bulutta adlı bir kullanıcı oluşturmaktır. Qumu bulut tam zamanı sağlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, eylem öğe yok. Yeni bir kullanıcı henüz yoksa Qumu bulut erişme denemesi sırasında oluşturulur.
>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Qumu bulut istemci destek ekibi](mailto:support@qumu.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Qumu buluta erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Qumu buluta atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Qumu bulut**.

    ![Uygulamalar listesinde Qumu bulut bağlantı](./media/qumucloud-tutorial/tutorial_qumucloud_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Qumu bulut parçasında tıklattığınızda, otomatik olarak Qumu bulut uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/qumucloud-tutorial/tutorial_general_01.png
[2]: ./media/qumucloud-tutorial/tutorial_general_02.png
[3]: ./media/qumucloud-tutorial/tutorial_general_03.png
[4]: ./media/qumucloud-tutorial/tutorial_general_04.png

[100]: ./media/qumucloud-tutorial/tutorial_general_100.png

[200]: ./media/qumucloud-tutorial/tutorial_general_200.png
[201]: ./media/qumucloud-tutorial/tutorial_general_201.png
[202]: ./media/qumucloud-tutorial/tutorial_general_202.png
[203]: ./media/qumucloud-tutorial/tutorial_general_203.png

