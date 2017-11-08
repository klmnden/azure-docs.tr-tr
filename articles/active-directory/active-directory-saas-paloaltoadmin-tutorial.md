---
title: "Öğretici: Azure Active Directory Tümleştirme Palo Alto Networks - Admin kullanıcı Arabirimi ile | Microsoft Docs"
description: "Çoklu oturum açma Palo Alto ağları - Admin kullanıcı Arabirimi ve Azure Active Directory arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a826eaec-15af-4c85-8855-8a3374d1efb9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: jeedes
ms.openlocfilehash: 4e73739588f1422e1e5b724784eedd6f25d71bf9
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="tutorial-azure-active-directory-integration-with-palo-alto-networks---admin-ui"></a>Öğretici: Azure Active Directory Tümleştirme Palo Alto ağlarla - Admin kullanıcı Arabirimi

Bu öğreticide, Azure Active Directory (Azure AD) ile yönetici UI Palo Alto Networks - tümleştirmek öğrenin.

Palo Alto ağları - yönetici UI Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Palo Alto Networks - Admin kullanıcı Arabirimi erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak Palo Alto ağlara - Azure AD hesaplarına sahip yönetim kullanıcı Arabirimi (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Palo Alto ağlarla - yönetici UI yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Palo Alto ağları - yönetici UI çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Yönetici UI galerisinden Palo Alto ağlar - ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-palo-alto-networks---admin-ui-from-the-gallery"></a>Yönetici UI galerisinden Palo Alto ağlar - ekleme
Palo Alto Networks - Azure AD'ye yönetici UI tümleştirmesini yapılandırma Palo Alto Networks - galerisinden yönetici UI yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Yönetici UI Galerisi'nden Palo Alto Networks - eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Palo Alto Networks - Admin kullanıcı Arabirimi**seçin **Palo Alto Networks - Admin kullanıcı Arabirimi** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Palo Alto Networks - sonuçlar listesinde yönetim kullanıcı Arabirimi](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Palo Alto ağlar ile test etme - "Britta Simon" adlı bir test kullanıcı yönetici UI tabanlı.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Palo Alto ağlarda - Admin kullanıcı Arabirimi bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı Palo Alto ağlarda - arasında bir bağlantı ilişkisi yönetici UI kurulması gerekir.

Palo Alto ağları - yönetici UI değerini atama **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırmak ve Azure AD çoklu oturum açma Palo Alto ağlarla - sınamak için yönetim kullanıcı Arabirimi, aşağıdaki yapı taşları tamamlanması gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Palo Alto Networks - yönetici UI test kullanıcısı oluşturma](#create-a-palo-alto-networks---admin-ui-test-user)**  - Britta Simon, karşılık gelen Palo Alto Networks - kullanıcı Azure AD gösterimini bağlantılı yönetici UI sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, Palo Alto Networks - Admin kullanıcı Arabirimi uygulama yapılandırın.

**Azure AD çoklu oturum açma Palo Alto ağlarla - yönetici UI yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Palo Alto Networks - Admin kullanıcı Arabirimi** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_samlbase.png)

3. Üzerinde **Palo Alto Networks - yönetici UI etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![-Yönetici UI etki alanı ve oturum açma URL'leri tek bilgi Palo Alto ağları](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<Customer Firewall URL>/php/login.php`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<Customer Firewall URL>/SAML20/SP`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [Palo Alto Networks - yönetici UI istemci destek ekibi](https://support.paloaltonetworks.com/support) bu değerleri almak için. 
 
4. Palo Alto Networks - yönetici kullanıcı Arabirimi uygulaması SAML onaylar belirli bir biçimde bekliyor. Lütfen bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirmesi sayfasında bölüm. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir.
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_attribute.png)
    
5. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, yukarıdaki resimde gösterildiği gibi SAML belirteci özniteliği yapılandırın ve aşağıdaki adımları gerçekleştirin: öznitelik değerleri yalnızca, örnek olduğuna dikkat edin Lütfen kullanıcı adı ve adminrole için uygun değerleri eşleyin. Başka bir isteğe bağlı öznitelik "belirli sanal sistemlere Güvenlik Duvarı'nda yönetici erişimi kısıtlamak için kullanılan accessdomain" dir.
        
    | Öznitelik adı | Öznitelik değeri |
    | --- | --- |    
    | kullanıcı adı | User.userPrincipalName |
    | adminrole | customadmin |

    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. Tıklatın **Tamam**

    > [!NOTE]
    > Makaleler öznitelikleri hakkında daha fazla ayrıntı için aşağıdaki başvurabilir.
    > 1. Yönetici kullanıcı Arabirimi (adminrole) için Yönetici rolü profili: https://www.paloaltonetworks.com/documentation/80/pan-os/pan-os/firewall-administration/manage-firewall-administrators/configure-an-admin-role-profile
    > 2. Aygıt erişim etki alanı yöneticisi kullanıcı Arabirimi (accessdomain) için: https://www.paloaltonetworks.com/documentation/80/pan-os/web-interface-help/device/device-access-domain

6. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_certificate.png) 

7. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_400.png)

8. Palo Alto siteyi başka bir tarayıcı penceresinde yönetici olarak açın.

9. Tıklayın **aygıt**.

    ![Palo Alto çoklu oturum açmayı yapılandırın](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_admin1.png)

10. Seçin **SAML kimlik sağlayıcısı** gelen sol gezinti çubuğu ve "meta veri dosyası al"'i tıklatın.

    ![Palo Alto çoklu oturum açmayı yapılandırın](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_admin2.png)

11. Aşağıdaki içeri aktarma penceresini eylemleri gerçekleştirme

    ![Palo Alto çoklu oturum açmayı yapılandırın](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_admin3.png)

    a. İçinde **profil adı** metin kutusuna, ad örneğin Azure AD yönetim kullanıcı Arabirimi sağlar.
    
    b. İçinde **kimlik sağlayıcısı meta verileri**, tıklatın **Gözat** ve Azure Portalı'ndan indirilen metadata.xml dosyası seçin
    
    c. **Tamam**’a tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-paloaltoadmin-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-paloaltoadmin-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-paloaltoadmin-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-paloaltoadmin-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**'a tıklayın.
 
### <a name="create-a-palo-alto-networks---admin-ui-test-user"></a>Palo Alto Networks - yönetici UI test kullanıcısı oluşturma

Palo Alto Networks - Admin zaten seçili değilse kullanıcı otomatik olarak sistemde başarılı kimlik doğrulamasından sonra oluşturulmayacak kullanıcı sağlamayı destekler sadece zamanında mevcut kullanıcı Arabirimi. Burada herhangi bir eylem gerçekleştirmeniz gerekmez.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Palo Alto Networks - yönetici UI erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Palo Alto Networks - yönetici UI Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Palo Alto Networks - yönetici UI**.

    ![Palo Alto ağlar - Admin kullanıcı Arabirimi bağlantısı uygulamalar listesinde](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Palo Alto Networks - yönetici UI kutucuğu erişim Paneli'nde tıklattığınızda, otomatik olarak, Palo Alto ağlara - Admin kullanıcı Arabirimi uygulama açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_203.png

