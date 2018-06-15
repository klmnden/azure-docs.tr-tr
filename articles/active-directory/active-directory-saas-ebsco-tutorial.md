---
title: 'Öğretici: Azure Active Directory Tümleştirme ile EBSCO | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile EBSCO arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 144f7f65-69e9-4016-a151-fe1104fd6ba8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: jeedes
ms.openlocfilehash: 9da10e134db019162abdff35a8eb742ccdc30626
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34342938"
---
# <a name="tutorial-azure-active-directory-integration-with-ebsco"></a>Öğretici: Azure Active Directory Tümleştirme EBSCO ile

Bu öğreticide, Azure Active Directory (Azure AD) ile EBSCO tümleştirmek öğrenin.

EBSCO Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- EBSCO erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için EBSCO (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme EBSCO ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir EBSCO çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden EBSCO ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-ebsco-from-the-gallery"></a>Galeriden EBSCO ekleme
Azure AD EBSCO tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden EBSCO eklemeniz gerekir.

**Galeriden EBSCO eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **EBSCO**seçin **EBSCO** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde EBSCO](./media/active-directory-saas-ebsco-tutorial/tutorial_ebsco_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı EBSCO sınayın.

Tekli çalışmaya oturum için Azure AD EBSCO karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının EBSCO ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma EBSCO ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir EBSCO test kullanıcısı oluşturma](#create-an-ebsco-test-user)**  -sağlama EBSCOhost kullanıcı/kişiselleştirme otomatik hale getirebilirsiniz. EBSCO destekleyen zaman içinde sadece kullanıcı hazırlama.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma EBSCO uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile EBSCO yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **EBSCO** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-ebsco-tutorial/tutorial_ebsco_samlbase.png)

3. Üzerinde **EBSCO etki alanı ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modu tarafından başlatılan:

    ![EBSCO etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-ebsco-tutorial/tutorial_ebsco_url.png)

    İçinde **tanımlayıcısı** metin kutusuna, bir URL yazın: `pingsso.ebscohost.com`

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![EBSCO etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-ebsco-tutorial/tutorial_ebsco_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `http://search.ebscohost.com/login.aspx?authtype=sso&custid=<unique EBSCO customer ID>&profile=<profile ID>`
     
    > [!NOTE] 
    > Oturum açma URL'si değeri gerçek değil. Değerin gerçek oturum açma URL'si ile güncelleştirin. Kişi [EBSCO istemci destek ekibi](mailto:sso@ebsco.com) değeri alınamıyor. 

    o **benzersiz öğeleri:**  

    o **CustId** Enter benzersiz EBSCO Müşteri Kimliği = 

    o **profil** = istemcileri (ne EBSCO satın alma göre) belirli bir profil kullanıcıları yönlendirmek için bağlantıyı uyarlama. Bir özel profil kimliği girebilirsiniz. Ana eds (EBSCO bulma hizmeti) ve ehost (EBSOCOhost veritabanları) Kimlikleridir. Yönergeler için aynı anda [burada](https://help.ebsco.com/interfaces/EBSCOhost/EBSCOhost_FAQs/How_do_I_set_up_direct_links_to_EBSCOhost_profiles_and_or_databases#profile).

5. EBSCO uygulaması SAML onaylar belirli bir biçimde bekliyor. Bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirmesi sayfasında bölüm. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir.
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ebsco-tutorial/tutorial_ebsco_attribute.png)

    > [!Note]
    > **Adı** özniteliği zorunludur ve ile eşlenmiş **kullanıcı tanımlayıcısı** EBSCO uygulamada. Bu el ile eklemeniz gerekmez bu varsayılan olarak eklenir.
    
6. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, yukarıdaki resimde gösterildiği gibi SAML belirteci özniteliği yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | ---------------| --------------- |    
    | FirstName   | User.givenName |
    | Soyadı   | User.surname |
    | Email   | User.Mail |

    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ebsco-tutorial/tutorial_officespace_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ebsco-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. Tıklatın **Tamam**

7. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-ebsco-tutorial/tutorial_ebsco_certificate.png) 

8. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-ebsco-tutorial/tutorial_general_400.png)
    
9. Çoklu oturum açma yapılandırmak için **EBSCO** yan, indirilen göndermek için ihtiyacınız **meta veri XML** için [EBSCO destek ekibi](mailto:sso@ebsco.com). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-ebsco-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-ebsco-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-ebsco-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-ebsco-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-ebsco-test-user"></a>Bir EBSCO test kullanıcısı oluşturma

EBSCO söz konusu olduğunda, kullanıcı sağlama otomatik olarak yapılır.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

Azure AD gerekli verileri EBSCO uygulamaya geçirir. EBSCO'ın kullanıcı hazırlama, otomatik olarak veya bir kerelik form gerektirir. Bu, istemcinin çok sayıda EBSCOhost hesapları kaydedilen kişisel ayarlar ile önceden varolan olup bağlıdır. Aynı ile açıklanan [EBSCO destek ekibi](mailto:sso@ebsco.com) uygulanması sırasında. Her iki durumda da, sınama önce herhangi bir EBSCOhost hesabı oluşturmak istemci sahip değil.

   >[!Note]
   >Sağlama EBSCOhost kullanıcı/kişiselleştirme otomatik hale getirebilirsiniz. Kişi [EBSCO destek ekibi](mailto:sso@ebsco.com) zaman içinde sadece hakkında kullanıcı hazırlama. 
 
### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta EBSCO için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**EBSCO için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **EBSCO**.

    ![Uygulamalar listesinde EBSCO bağlantı](./media/active-directory-saas-ebsco-tutorial/tutorial_ebsco_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

1. Erişim paneli EBSCO parçasında tıklattığınızda, otomatik olarak EBSCO uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

2. Uygulamaya oturum sonra tıklayın **oturum** sağ üst köşedeki düğmesini.

    ![Uygulamalar listesinde EBSCO oturum açma](./media/active-directory-saas-ebsco-tutorial/tutorial_ebsco_signin.png)
 
3. Kurumsal/SAML oturum açma ile eşleştirin için tek seferlik bir ileti alacaksınız bir **varolan MyEBSCOhost hesabınızı şimdi kurum hesabınıza bağlamak** veya **yeni MyEBSCOhost hesabı oluşturun ve ona bağlayın, Kurum hesap**. Hesap EBSCOhost uygulama kişiselleştirmesini için kullanılır. Seçeneğini **yeni bir hesap oluşturma** ve aşağıdaki ekran görüntüsünde gösterildiği gibi form kişiselleştirme için saml yanıtını değerlerle önceden tamamlanmış olduğunu göreceksiniz. Tıklatın **'Devam'** bu seçimi kaydetmek için.
    
     ![Uygulamalar listesinde EBSCO kullanıcı](./media/active-directory-saas-ebsco-tutorial/tutorial_ebsco_user.png)

4. Yukarıdaki Kurulum tamamlandıktan sonra tanımlama bilgisi/önbellek ve oturum açma yeniden temizleyin. Oturum açma el ile yeniden olmaz ve kişiselleştirme ayarlarını hatırlanan

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ebsco-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ebsco-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ebsco-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ebsco-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ebsco-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ebsco-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ebsco-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ebsco-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ebsco-tutorial/tutorial_general_203.png

