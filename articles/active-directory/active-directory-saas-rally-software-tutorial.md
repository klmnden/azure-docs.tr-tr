---
title: "Öğretici: Azure Active Directory Tümleştirme Rally yazılımı ile | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve Rally yazılımı arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ba25fade-e152-42dd-8377-a30bbc48c3ed
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: jeedes
ms.openlocfilehash: 6481c9ef0ca71419ccfa6f7956f4702985743df3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rally-software"></a>Öğretici: Azure Active Directory Tümleştirme Rally yazılımı

Bu öğreticide, Azure Active Directory (Azure AD) ile Rally yazılımı tamamlayan öğrenin.

RALLY yazılımı Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Rally yazılımı erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak Rally yazılımı (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Rally yazılımı ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Rally yazılımı çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. RALLY yazılımı Galeriden ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-rally-software-from-the-gallery"></a>RALLY yazılımı Galeriden ekleme
Azure AD Rally yazılımı tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Rally yazılımı eklemeniz gerekir.

**RALLY yazılımı Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Rally yazılımı**seçin **Rally yazılımı** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde rally yazılımı](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmak ve yazılım Rally "Britta Simon" adlı bir test kullanıcı tabanlı Azure AD çoklu oturum açmayı sınayın.

Tekli çalışmaya oturum için Azure AD Rally yazılımı karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Rally yazılımı ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

RALLY yazılımı değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Rally yazılımı ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Rally yazılımı test kullanıcısı oluşturma](#create-a-rally-software-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Rally yazılımı sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Rally yazılımı uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Rally yazılımı ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Rally yazılımı** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_samlbase.png)

3. Üzerinde **Rally yazılımı etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Rally yazılımı etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<tenant-name>.rally.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<tenant-name>.rally.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [Rally yazılımı istemci destek ekibi](https://help.rallydev.com/) bu değerleri almak için. 
 


4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-rally-software-tutorial/tutorial_general_400.png)

6. Üzerinde **Rally yazılımı Yapılandırması** 'yi tıklatın **Rally yazılımı Yapılandır** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL ve SAML varlık kimliği** gelen **hızlı başvuru bölümü.**

    ![Rally yazılımı yapılandırması](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_configure.png) 

7. Oturum, **Rally yazılımı** Kiracı.

8. Üstteki araç çubuğunda tıklatın **Kurulum**ve ardından **abonelik**.
   
    ![Abonelik](./media/active-directory-saas-rally-software-tutorial/ic769531.png "abonelik")

9. Tıklatın **eylem** düğmesi. Seçin **Düzenle abonelik** araç üst sağ tarafındaki.

10. Üzerinde **abonelik** iletişim sayfasında, aşağıdaki adımları uygulayın ve ardından **Kaydet ve Kapat**:
   
    ![Kimlik doğrulama](./media/active-directory-saas-rally-software-tutorial/ic769542.png "kimlik doğrulaması")
   
    a. Seçin **yarışı veya SSO kimlik doğrulaması** gelen kimlik doğrulama açılır.

    b. İçinde **kimlik sağlayıcısı URL'si** metin değerini yapıştırın **SAML varlık kimliği**, Azure portalından kopyalanan. 

    c. İçinde **SSO oturum kapatma** metin değerini yapıştırın **Sign-Out URL**, Azure portalından kopyalanan.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-rally-software-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-rally-software-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-rally-software-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-rally-software-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**'a tıklayın.
 
### <a name="create-a-rally-software-test-user"></a>Rally yazılımı test kullanıcısı oluşturma

Azure AD kullanıcıların oturum açabilmesi için bunlar Azure Active Directory kullanıcı adlarını kullanarak Rally yazılımı uygulamaya sağlanmalıdır.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Rally yazılımı kiracınız için oturum açın.

2. Git **Kurulum \> kullanıcılar**ve ardından **+ yeni Ekle**.
   
    ![Kullanıcıların](./media/active-directory-saas-rally-software-tutorial/ic781039.png "kullanıcılar")

3. Yeni kullanıcı metin kutusunda adı yazın ve ardından **ayrıntılarla Ekle**.

4. İçinde **kullanıcı oluştur** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcı oluşturma](./media/active-directory-saas-rally-software-tutorial/ic781040.png "kullanıcı oluştur")

    a. İçinde **kullanıcı adı** metin kutusu, kullanıcı adını yazın ister **Brittsimon**.
   
    b. İçinde **e-posta adresi** metin kutusuna, bir kullanıcı gibi e-posta girin  **brittasimon@contoso.com** .

    c. İçinde **ad** metin kutusuna, gibi kullanıcının ilk adını girin **Britta**.

    d. İçinde **Soyadı** metin kutusuna, gibi kullanıcının soyadını girin **Simon**.

    e. Tıklatın **Kaydet ve Kapat**.

   >[!NOTE]
   >Azure AD kullanıcı hesaplarını sağlamak için herhangi bir Rally yazılımı kullanıcı hesabı oluşturma araçlarını veya Rally yazılımı tarafından sağlanan API'leri kullanabilirsiniz.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Rally yazılımı erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Rally yazılımı Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Rally yazılımı**.

    ![Uygulamalar listesinde Rally yazılımı bağlantı](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.

Erişim paneli Rally yazılımı parçasında tıklattığınızda, otomatik olarak Rally yazılımı uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_203.png

