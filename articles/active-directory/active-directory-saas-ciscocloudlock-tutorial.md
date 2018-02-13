---
title: "Öğretici: Azure Active Directory Tümleştirme ile Cisco Cloudlock | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve Cisco Cloudlock arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 549e8810-1b3b-4351-bf4b-f07de98980d1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2018
ms.author: jeedes
ms.openlocfilehash: d6961eb760544ac078fdd6ed0d7c0c4d750a8b9f
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2018
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-cloudlock"></a>Öğretici: Cisco Cloudlock Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Cisco Cloudlock tümleştirmek öğrenin.

Cisco Cloudlock Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Cisco Cloudlock erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için Cisco Cloudlock açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Önkoşullar

Cisco Cloudlock ile Azure AD tümleştirme yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Cisco Cloudlock çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Cisco Cloudlock Galeriden ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-cisco-cloudlock-from-the-gallery"></a>Cisco Cloudlock Galeriden ekleme
Azure AD Cisco Cloudlock tümleştirilmesi yapılandırmak için Cisco Cloudlock Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Cisco Cloudlock Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Cisco Cloudlock**seçin **Cisco Cloudlock** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Cisco Cloudlock sonuçlar listesinde](./media/active-directory-saas-ciscocloudlock-tutorial/tutorial_ciscocloudlock_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Cisco "Britta Simon" adlı bir test kullanıcı tabanlı Cloudlock ile test etme.

Tekli çalışmaya oturum için Azure AD Cisco Cloudlock karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Cisco Cloudlock ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırmak ve Azure AD çoklu oturum açma ile Cisco Cloudlock sınamak için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Cisco Cloudlock test kullanıcısı oluşturma](#create-a-cisco-cloudlock-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Cisco Cloudlock sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Cisco Cloudlock uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Cisco Cloudlock yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Cisco Cloudlock** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-ciscocloudlock-tutorial/tutorial_ciscocloudlock_samlbase.png)

3. Üzerinde **Cisco Cloudlock etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Cisco Cloudlock etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-ciscocloudlock-tutorial/tutorial_ciscocloudlock_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, bir URL yazın:
    | |
    |--|
    | `https://platform.cloudlock.com` |
    | `https://app.cloudlock.com` |

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: 
    | |
    |--|
    | `https://platform.cloudlock.com/gate/saml/sso/<subdomain>` |
    | `https://app.cloudlock.com/gate/saml/sso/<subdomain>` |

    > [!NOTE] 
    > Tanımlayıcı değeri gerçek değil. Değerin gerçek tanımlayıcısı ile güncelleştirin. Kişi [Cisco Cloudlock istemci destek ekibi](mailto:support@cloudlock.com) değeri alınamıyor. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-ciscocloudlock-tutorial/tutorial_ciscocloudlock_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-ciscocloudlock-tutorial/tutorial_general_400.png)

6. Çoklu oturum açma yapılandırmak için **Cisco Cloudlock** yan, indirilen göndermek için ihtiyacınız **meta veri XML** için [Cisco Cloudlock destek ekibi](mailto:support@cloudlock.com). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-ciscocloudlock-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-ciscocloudlock-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-ciscocloudlock-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-ciscocloudlock-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-cisco-cloudlock-test-user"></a>Cisco Cloudlock test kullanıcısı oluşturma

Bu bölümde, Cisco Cloudlock Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [Cisco Cloudlock destek ekibi](mailto:support@cloudlock.com) Cisco Cloudlock platform kullanıcıları eklemek için. Kullanıcıların oluşturulan ve çoklu oturum açma kullanmadan önce etkinleştirilmelidir. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Cisco Cloudlock erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Cisco Cloudlock Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Cisco Cloudlock**.

    ![Uygulamalar listesinde Cisco Cloudlock bağlantı](./media/active-directory-saas-ciscocloudlock-tutorial/tutorial_ciscocloudlock_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Cisco Cloudlock parçasında tıklattığınızda, otomatik olarak Cisco Cloudlock uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ciscocloudlock-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ciscocloudlock-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ciscocloudlock-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ciscocloudlock-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ciscocloudlock-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ciscocloudlock-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ciscocloudlock-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ciscocloudlock-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ciscocloudlock-tutorial/tutorial_general_203.png

