---
title: "Öğretici: Azure Active Directory Tümleştirme ile ön | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve ön arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 88270b6d-2571-434a-b139-b6ccc3a2b19f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: jeedes
ms.openlocfilehash: 944149746b05108040cd0cc340e882b39713f06e
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-front"></a>Öğretici: Azure Active Directory Tümleştirme ön ile

Bu öğreticide, Azure Active Directory (Azure AD) ile ön tümleştirmek öğrenin.

Ön Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Ön erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak öne (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme ön ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir ön çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden ön ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-front-from-the-gallery"></a>Galeriden ön ekleme
Azure AD Önden tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden ön eklemeniz gerekir.

**Galeriden ön eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **ön**seçin **ön** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde ön](./media/active-directory-saas-front-tutorial/tutorial_front_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmak ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı ön sınayın.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen önde bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve önde ilgili kullanıcı arasındaki bağlantıyı ilişki kurulması gerekir.

Önde, değeri atamak **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma ile ön test etmek için aşağıdaki yapı taşları tamamlanması gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Ön test kullanıcısı oluşturma](#create-a-front-test-user)**  - kullanıcı Azure AD gösterimini bağlantılı bir karşılık gelen Britta Simon, önde sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma ön uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile ön yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **ön** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-front-tutorial/tutorial_front_samlbase.png)

3. Üzerinde **ön etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-front-tutorial/tutorial_front_url1.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<companyname>.frontapp.com`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<companyname>.frontapp.com/sso/saml/callback`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Gerçek tanımlayıcı ve yanıt URL'si daha sonra öğreticide veya kişi açıklanacak bu değerleri güncelleştirmek [ön istemci destek ekibi](mailto:support@frontapp.com) bu değerleri almak için. 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-front-tutorial/tutorial_front_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-front-tutorial/tutorial_general_400.png)
    
6. Üzerinde **ön yapılandırma** 'yi tıklatın **yapılandırma ön** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-front-tutorial/tutorial_front_configure.png) 

7. Ön kiracınız yönetici olarak oturum.

8. Git **ayarları (dişli simgesine sol kenar) > Tercihler**.
   
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/active-directory-saas-front-tutorial/tutorial_front_000.png)

9. Tıklatın **çoklu oturum açma** bağlantı.
   
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/active-directory-saas-front-tutorial/tutorial_front_001.png)

10. Seçin **SAML** aşağı açılan listesinde **çoklu oturum açma**.
   
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/active-directory-saas-front-tutorial/tutorial_front_002.png)

11. İçinde **giriş noktası** textbox değeri put **çoklu oturum açma hizmet URL'si** Azure AD Uygulama Yapılandırma Sihirbazı'ndan.
    
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/active-directory-saas-front-tutorial/tutorial_front_003.png)

12. İndirilen açmak **Certificate(Base64)** dosyasını Not Defteri'nde, içeriğini, panoya kopyalayın ve yapıştırın kendisine **imzalama sertifikası** metin kutusu.
    
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/active-directory-saas-front-tutorial/tutorial_front_004.png)

13. Üzerinde **hizmet sağlayıcı ayarları** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/active-directory-saas-front-tutorial/tutorial_front_005.png)

    a. Değerini kopyalayın **varlık kimliği** ve yapıştırın **tanımlayıcısı** metin kutusuna **ön etki alanı ve URL'leri** Azure portalı bölümünde.

    b. Değerini kopyalayın **ACS URL** ve yapıştırın **yanıt URL'si** metin kutusuna **ön etki alanı ve URL'leri** Azure portalı bölümünde.
    
14. Tıklatın **kaydetmek** düğmesi.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-front-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-front-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-front-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-front-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**'a tıklayın.
 
### <a name="create-a-front-test-user"></a>Ön test kullanıcısı oluşturma

Bu bölümde, Britta Simon önde adlı bir kullanıcı oluşturun. Çalışmak [ön istemci destek ekibi](mailto:support@frontapp.com) ön platform kullanıcıları eklemek için. Kullanıcıların oluşturulan ve çoklu oturum açma kullanmadan önce etkinleştirilmelidir.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta öne erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Öne Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **ön**.

    ![Uygulamalar listesinde ön bağlantı](./media/active-directory-saas-front-tutorial/tutorial_front_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümün amacı, Azure AD erişim paneli kullanarak SSOconfiguration test etmektir.

Erişim paneli ön parçasında tıklattığınızda, otomatik olarak ön uygulamanıza açan. 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-front-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-front-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-front-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-front-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-front-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-front-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-front-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-front-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-front-tutorial/tutorial_general_203.png

