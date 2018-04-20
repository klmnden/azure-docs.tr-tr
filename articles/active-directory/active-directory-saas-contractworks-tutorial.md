---
title: 'Öğretici: Azure Active Directory Tümleştirme ile ContractWorks | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile ContractWorks arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: e7b269d6-3c4e-4bc4-a55f-5071d1f52591
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2018
ms.author: jeedes
ms.openlocfilehash: 3c986810f3d994b956d4c81fd80b38d9dfee4f50
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-contractworks"></a>Öğretici: Azure Active Directory Tümleştirme ContractWorks ile

Bu öğreticide, Azure Active Directory (Azure AD) ile ContractWorks tümleştirmek öğrenin.

ContractWorks Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- ContractWorks erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için ContractWorks (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme ContractWorks ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir ContractWorks çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden ContractWorks ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-contractworks-from-the-gallery"></a>Galeriden ContractWorks ekleme
Azure AD ContractWorks tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden ContractWorks eklemeniz gerekir.

**Galeriden ContractWorks eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **ContractWorks**seçin **ContractWorks** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde ContractWorks](./media/active-directory-saas-contractworks-tutorial/tutorial_contractworks_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı ContractWorks sınayın.

Tekli çalışmaya oturum için Azure AD ContractWorks karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ContractWorks ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma ContractWorks ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[ContractWorks test kullanıcısı oluşturma](#create-a-contractworks-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı ContractWorks sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma ContractWorks uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile ContractWorks yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **ContractWorks** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-contractworks-tutorial/tutorial_contractworks_samlbase.png)

3. Üzerinde **ContractWorks etki alanı ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modu tarafından başlatılan:

    ![ContractWorks etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-contractworks-tutorial/tutorial_contractworks_url.png)

    İçinde **tanımlayıcısı** metin kutusuna, bir URL yazın: `https://login.securedocs.com/saml/metadata`

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![ContractWorks etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-contractworks-tutorial/tutorial_contractworks_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna, bir URL yazın: `https://login.securedocs.com/saml/hint`
     
5. ContractWorks uygulaması SAML onaylar belirli bir biçimde bekliyor. Bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirmesi sayfasında bölüm. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir.
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-contractworks-tutorial/tutorial_ContractWorks_attribute.png)

6. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, yukarıdaki resimde gösterildiği gibi SAML belirteci özniteliği yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | ---------------| --------------- |
    | Posta | User.Mail |
    | Görünen adı | user.displayname |

    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-contractworks-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-contractworks-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. Bırakın **Namespace** boş.
    
    d. Tıklatın **Tamam**

7. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesini tıklatın **uygulama Federasyon meta veri URL'sini** ve Not Defteri'ne yapıştırın.
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-contractworks-tutorial/tutorial_metadataurl.png)
     
8. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-contractworks-tutorial/tutorial_general_400.png)

9. Çoklu oturum açma yapılandırmak için **ContractWorks** yan, oluşturulan göndermek için ihtiyacınız **uygulama Federasyon meta veri URL'sini** için [ContractWorks destek ekibi](mailto:support@contractworks.com). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-contractworks-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-contractworks-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-contractworks-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-contractworks-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-contractworks-test-user"></a>ContractWorks test kullanıcısı oluşturma

Bu bölümde, ContractWorks içinde Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [ContractWorks destek ekibi](mailto:support@contractworks.com) ContractWorks platform kullanıcıları eklemek için. Kullanıcıların oluşturulan ve çoklu oturum açma kullanmadan önce etkinleştirilmelidir. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta ContractWorks için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**ContractWorks için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **ContractWorks**.

    ![Uygulamalar listesinde ContractWorks bağlantı](./media/active-directory-saas-contractworks-tutorial/tutorial_contractworks_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli ContractWorks parçasında tıklattığınızda, otomatik olarak ContractWorks uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-contractworks-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-contractworks-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-contractworks-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-contractworks-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-contractworks-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-contractworks-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-contractworks-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-contractworks-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-contractworks-tutorial/tutorial_general_203.png

