---
title: "Öğretici: Azure Active Directory Tümleştirme ile XaitPorter | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile XaitPorter arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: d33c7cb7-0550-425b-882a-619a713a71b7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/28/2017
ms.author: jeedes
ms.openlocfilehash: 2012d990f7cdcb8c12da5f16db518b261b06a5b7
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-xaitporter"></a>Öğretici: Azure Active Directory Tümleştirme XaitPorter ile

Bu öğreticide, Azure Active Directory (Azure AD) ile XaitPorter tümleştirmek öğrenin.

XaitPorter Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- XaitPorter erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için XaitPorter (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme XaitPorter ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir XaitPorter çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden XaitPorter ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-xaitporter-from-the-gallery"></a>Galeriden XaitPorter ekleme
Azure AD XaitPorter tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden XaitPorter eklemeniz gerekir.

**Galeriden XaitPorter eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **XaitPorter**seçin **XaitPorter** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde XaitPorter](./media/active-directory-saas-xaitporter-tutorial/tutorial_xaitporter_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı XaitPorter sınayın.

Tekli çalışmaya oturum için Azure AD XaitPorter karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının XaitPorter ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

XaitPorter içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma XaitPorter ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[XaitPorter test kullanıcısı oluşturma](#create-a-xaitporter-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı XaitPorter sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma XaitPorter uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile XaitPorter yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **XaitPorter** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-xaitporter-tutorial/tutorial_xaitporter_samlbase.png)

3. Üzerinde **XaitPorter etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![XaitPorter etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-xaitporter-tutorial/tutorial_xaitporter_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<subdomain>.xaitporter.com/saml/login`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<subdomain>.xaitporter.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [XaitPorter istemci destek ekibi](https://www.xait.com/support/) bu değerleri almak için. 

4. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-xaitporter-tutorial/tutorial_general_400.png)

5. Oluşturulacak **meta veri** url, aşağıdaki adımları gerçekleştirin:

    a. Tıklatın **uygulama kayıtlar**.
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-xaitporter-tutorial/tutorial_xaitporter_appregistrations.png)
   
    b. Tıklatın **uç noktaları** açmak için **uç noktaları** iletişim kutusu.  
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-xaitporter-tutorial/tutorial_xaitporter_endpointicon.png)

    c. Kopyalamak için Kopyala düğmesini tıklatın **FEDERASYON meta veri belgesi** URL'yi kopyalayıp Not Defteri'ne yapıştırın.
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-xaitporter-tutorial/tutorial_xaitporter_endpoint.png)
     
    d. Şimdi özellik sayfasına gidin **XaitPorter** ve kopyalama **uygulama kimliği** kullanarak **kopyalama** düğmesine tıklayın ve Not Defteri'ne yapıştırın.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-xaitporter-tutorial/tutorial_xaitporter_appid.png)

    e. Oluştur **meta veri URL'sini** şu biçimi kullanarak:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`

6. Sağlamak **IP adresi** veya **meta veri URL'sini** için [SmartRecruiters destek ekibi](https://www.smartrecruiters.com/about-us/contact-us/), böylece XaitPorter IP adresi, XaitPorter erişilebilir olduğundan emin olun beyaz liste kendi tarafında yapılandırma örneği. 

7. Farklı web tarayıcısı penceresinde XaitPorter şirket sitenize yönetici olarak oturum açın.

8. Tıklayın **yönetici**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-xaitporter-tutorial/user1.png)

9. Seçin **yönetmek çoklu oturum açma** gelen **sistem kurulumu** açılır liste.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-xaitporter-tutorial/user2.png)

10. İçinde **yönetmek tek oturum açma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-xaitporter-tutorial/user3.png)

    a. Seçin **tek oturum açma kimlik doğrulamasını etkinleştirme**.

    b. İçinde **kimlik sağlayıcı ayarları** metin kutusuna, Yapıştır **meta veri URL'sini** Azure ve tıklatın kopyalanan **getirme**.

    c. Seçin **etkinleştirmek kullanıcıların Autocreation**.

    d. **Tamam** düğmesine tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-xaitporter-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-xaitporter-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-xaitporter-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-xaitporter-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**'a tıklayın.
 
### <a name="create-a-xaitporter-test-user"></a>XaitPorter test kullanıcısı oluşturma

Bu bölümde, XaitPorter içinde Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [XaitPorter istemci destek ekibi](https://www.xait.com/support/) XaitPorter platform kullanıcıları eklemek için. Kullanıcıların oluşturulan ve çoklu oturum açma kullanmadan önce etkinleştirilmelidir. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta XaitPorter için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**XaitPorter için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **XaitPorter**.

    ![Uygulamalar listesinde XaitPorter bağlantı](./media/active-directory-saas-xaitporter-tutorial/tutorial_xaitporter_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli XaitPorter parçasında tıklattığınızda, otomatik olarak XaitPorter uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-xaitporter-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-xaitporter-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-xaitporter-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-xaitporter-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-xaitporter-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-xaitporter-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-xaitporter-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-xaitporter-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-xaitporter-tutorial/tutorial_general_203.png

