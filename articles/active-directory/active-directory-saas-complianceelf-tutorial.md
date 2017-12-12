---
title: "Öğretici: Azure Active Directory Tümleştirme ile uyumluluk ELF | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile uyumluluk ELF arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 69c6efc3-54c7-49ec-b827-33177c09aa13
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/2/2017
ms.author: jeedes
ms.openlocfilehash: f05c43e2d8f6e777ae128e90040d8d5c270c212b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-compliance-elf"></a>Öğretici: Azure Active Directory Tümleştirme ile uyumluluk ELF

Bu öğreticide, Azure Active Directory (Azure AD) ile uyumluluk ELF tümleştirmek öğrenin.

Uyumluluk ELF Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Uyumluluk ELF erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için Uyumluluk ELF açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD Tümleştirmesi ile uyumluluk ELF yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir uyumluluk ELF çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden uyumluluk ELF ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-compliance-elf-from-the-gallery"></a>Galeriden uyumluluk ELF ekleme
Azure AD uyumluluk ELF tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden uyumluluk ELF eklemeniz gerekir.

**Galeriden uyumluluk ELF eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **uyumluluk ELF**seçin **uyumluluk ELF** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde ELF uyumluluk](./media/active-directory-saas-complianceelf-tutorial/tutorial_complianceelf_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma uyumluluk "Britta Simon" adlı bir test kullanıcı tabanlı ELF ile test etme.

Tekli çalışmaya oturum için Azure AD uyumluluk ELF karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının uyumluluk ELF ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Uyumluluk ELF içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırmak ve Azure AD çoklu oturum açma ile uyumluluk ELF sınamak için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Uyumluluk ELF test kullanıcısı oluşturma](#create-a-compliance-elf-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı uyumluluk ELF sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma uyumluluk ELF uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile uyumluluk ELF yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **uyumluluk ELF** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-complianceelf-tutorial/tutorial_complianceelf_samlbase.png)

3. Üzerinde **uyumluluk ELF etki alanı ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modu tarafından başlatılan:

    ![Uyumluluk ELF etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-complianceelf-tutorial/tutorial_complianceelf_url.png)

    İçinde **tanımlayıcısı** metin kutusuna, URL'yi yazın:`https://sso.cordium.com`

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![Uyumluluk ELF etki alanı ve URL'ler çoklu oturum açma](./media/active-directory-saas-complianceelf-tutorial/tutorial_complianceelf_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<subdomain>.complianceelf.com`
    
    > [!NOTE] 
    > Bu değer gerçek değil. Bu değerleri gerçek oturum açma URL'si ile güncelleştirin. Kişi [uyumluluk ELF destek ekibi](mailto:support@complianceelf.com) bu değeri alınamıyor.

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-complianceelf-tutorial/tutorial_general_400.png)

6. Oluşturulacak **meta veri** url, aşağıdaki adımları gerçekleştirin:

    a. Tıklatın **uygulama kayıtlar**.
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-complianceelf-tutorial/tutorial_complianceelf_appregistrations.png)
   
    b. Tıklatın **uç noktaları** açmak için **uç noktaları** iletişim kutusu.  
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-complianceelf-tutorial/tutorial_complianceelf_endpointicon.png)

    c. Kopyalamak için Kopyala düğmesini tıklatın **FEDERASYON meta veri belgesi** URL'yi kopyalayıp Not Defteri'ne yapıştırın.
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-complianceelf-tutorial/tutorial_complianceelf_endpoint.png)
     
    d. Şimdi özellik sayfasına gidin **uyumluluk ELF** ve kopyalama **uygulama kimliği** kullanarak **kopyalama** düğmesine tıklayın ve Not Defteri'ne yapıştırın.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-complianceelf-tutorial/tutorial_complianceelf_appid.png)

    e. Oluştur **meta veri URL'sini** şu biçimi kullanarak:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`

7. Çoklu oturum açma yapılandırmak için **uyumluluk ELF** yan, ihtiyacınız göndermek **meta veri URL'sini** için [uyumluluk ELF destek ekibi](mailto:support@complianceelf.com). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-complianceelf-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-complianceelf-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-complianceelf-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-complianceelf-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**'a tıklayın.
  
### <a name="create-a-compliance-elf-test-user"></a>Uyumluluk ELF test kullanıcısı oluşturma

Bu bölümde, uyumluluk ELF Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [uyumluluk ELF destek ekibi](mailto:support@complianceelf.com) uyumluluk ELF platform kullanıcıları eklemek için. Kullanıcıların oluşturulan ve çoklu oturum açma kullanmadan önce etkinleştirilmelidir.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta uyumluluk ELF erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Uyumluluk ELF Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **uyumluluk ELF**.

    ![Uygulamalar listesinde uyumluluk ELF bağlantı](./media/active-directory-saas-complianceelf-tutorial/tutorial_complianceelf_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli uyumluluk ELF parçasında tıklattığınızda, otomatik olarak uyumluluk ELF uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-complianceelf-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-complianceelf-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-complianceelf-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-complianceelf-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-complianceelf-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-complianceelf-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-complianceelf-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-complianceelf-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-complianceelf-tutorial/tutorial_general_203.png

