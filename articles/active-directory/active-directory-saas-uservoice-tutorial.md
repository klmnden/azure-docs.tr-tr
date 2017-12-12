---
title: "Öğretici: Azure Active Directory Tümleştirme ile UserVoice | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile UserVoice arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 684a405b-8932-46f6-b43a-4d97a42b6b87
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.openlocfilehash: 8ec3fd40b04dc6590b80c643a4429f97f7b874d9
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-uservoice"></a>Öğretici: Azure Active Directory Tümleştirme UserVoice ile

Bu öğreticide, Azure Active Directory (Azure AD) ile UserVoice tümleştirmek öğrenin.

UserVoice Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- UserVoice erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için UserVoice (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme UserVoice ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir UserVoice çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden UserVoice ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-uservoice-from-the-gallery"></a>Galeriden UserVoice ekleme
Azure AD UserVoice tümleştirilmesi yapılandırmak için UserVoice Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden UserVoice eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **UserVoice**seçin **UserVoice** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde UserVoice](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı UserVoice sınayın.

Tekli çalışmaya oturum için Azure AD UserVoice karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının UserVoice ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

UserVoice içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma UserVoice ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[UserVoice test kullanıcısı oluşturma](#create-a-uservoice-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı UserVoice sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma UserVoice uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile UserVoice yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **UserVoice** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_samlbase.png)

3. Üzerinde **UserVoice etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![UserVoice etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<tenantname>.UserVoice.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<tenantname>.UserVoice.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [UserVoice istemci destek ekibi](https://www.uservoice.com/) bu değerleri almak için.

4. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalama **parmak İZİ** sertifika değeri.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-uservoice-tutorial/tutorial_general_400.png)

6. Üzerinde **UserVoice yapılandırma** 'yi tıklatın **yapılandırma UserVoice** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![UserVoice yapılandırma](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_configure.png) 

7. Farklı web tarayıcısı penceresinde UserVoice şirket sitenize yönetici olarak oturum açın.

8. Üstteki araç çubuğunda tıklatın **ayarları**ve ardından **Web portalı** menüsünde.
   
    ![Uygulama tarafında ayarları bölümünde](./media/active-directory-saas-uservoice-tutorial/ic777519.png "ayarları")

9. Üzerinde **Web portalı** sekmesinde **kullanıcı kimlik doğrulaması** 'yi tıklatın **Düzenle** açmak için **kullanıcı kimlik doğrulamasını Düzenle** iletişim sayfası.
   
    ![Web portalı sekmesini](./media/active-directory-saas-uservoice-tutorial/ic777520.png "Web portalı")

10. Üzerinde **kullanıcı kimlik doğrulamasını Düzenle** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcı kimlik doğrulamasını Düzenle](./media/active-directory-saas-uservoice-tutorial/ic777521.png "düzenleme kullanıcı kimlik doğrulaması")
   
    a. Tıklatın **çoklu oturum açma (SSO)**.
 
    b. Yapıştır **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyaladığınız değeri **SSO uzaktan oturum açma** metin kutusu.

    c. Yapıştır **Sign-Out URL** Azure portalından kopyaladığınız değeri **SSO uzak Sign-Out textbox**.
 
    d. Yapıştır **parmak izi** Azure portalından kopyaladığınız değeri **geçerli SHA1 sertifika parmak izi** metin kutusu.
    
    e. Tıklatın **kimlik doğrulama ayarlarını Kaydet**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-uservoice-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-uservoice-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-uservoice-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-uservoice-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**'a tıklayın.
 
### <a name="create-a-uservoice-test-user"></a>UserVoice test kullanıcısı oluşturma

Azure AD kullanıcıları için UserVoice oturum açmayı etkinleştirmek için bunların UserVoice sağlanmalıdır. UserVoice söz konusu olduğunda, sağlama bir el ile bir görevdir.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:
1. Oturum, **UserVoice** Kiracı.

2. Git **ayarları**.
   
    ![Ayarları](./media/active-directory-saas-uservoice-tutorial/ic777811.png "ayarları")

3. Tıklatın **genel**.

4. Tıklatın **aracıları ve izinleri**.
   
    ![Aracılar ve izinleri](./media/active-directory-saas-uservoice-tutorial/ic777812.png "aracıları ve izinleri")

5. Tıklatın **yöneticileri ekleyin**.
   
    ![Yöneticileri ekleyin](./media/active-directory-saas-uservoice-tutorial/ic777813.png "yöneticileri ekleyin")

6. Üzerinde **davet admins** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Yöneticileri davet](./media/active-directory-saas-uservoice-tutorial/ic777814.png "davet yöneticileri")
   
    a. E-postaları metin kutusuna sağlamak istediğiniz ve ardından hesabın e-posta adresini yazın **Ekle**.
   
    b. Tıklatın **davet**.

> [!NOTE]
> API sağlama AAD kullanıcı hesaplarına UserVoice tarafından sağlanan veya herhangi diğer UserVoice kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta UserVoice için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**UserVoice için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **UserVoice**.

    ![Uygulamalar listesinde UserVoice bağlantı](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli UserVoice parçasında tıklattığınızda, otomatik olarak UserVoice uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_203.png

