---
title: 'Öğretici: Azure Active Directory Tümleştirme ile MobileIron | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile MobileIron arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 3e4bbd5b-290e-4951-971b-ec0c1c11aaa2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/9/2017
ms.author: jeedes
ms.openlocfilehash: 5d98e57a2af71345b404f02b81762d5e0bc0961b
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-mobileiron"></a>Öğretici: Azure Active Directory Tümleştirme MobileIron ile

Bu öğreticide, Azure Active Directory (Azure AD) ile MobileIron tümleştirmek öğrenin.

MobileIron Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- MobileIron erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için MobileIron (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme MobileIron ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir MobileIron çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden MobileIron ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-mobileiron-from-the-gallery"></a>Galeriden MobileIron ekleme
Azure AD MobileIron tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden MobileIron eklemeniz gerekir.

**Galeriden MobileIron eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **MobileIron**seçin **MobileIron** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde MobileIron](./media/active-directory-saas-mobileiron-tutorial/tutorial_mobileiron_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı MobileIron ile test etme

Tekli çalışmaya oturum için Azure AD MobileIron karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının MobileIron ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

MobileIron içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma MobileIron ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[MobileIron test kullanıcısı oluşturma](#create-a-mobileiron-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı MobileIron sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma MobileIron uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile MobileIron yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **MobileIron** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-mobileiron-tutorial/tutorial_mobileiron_samlbase.png)

3. Üzerinde **MobileIron etki alanı ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modu tarafından başlatılan:

    ![MobileIron etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-mobileiron-tutorial/tutorial_mobileiron_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://www.mobileiron.com/<key>`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<host>.mobileiron.com/saml/SSO/alias/<key>`

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![MobileIron etki alanı ve URL'ler çoklu oturum açma](./media/active-directory-saas-mobileiron-tutorial/tutorial_mobileiron_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<host>.mobileiron.com/user/login.html`
    
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler, gerçek tanımlayıcı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Öğreticide daha sonra açıklanan MobileIron Yönetim Portalı'ndan anahtarı ve ana bilgisayar değerlerini alırsınız.

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-mobileiron-tutorial/tutorial_mobileiron_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-mobileiron-tutorial/tutorial_general_400.png)

7. Farklı web tarayıcısı penceresinde MobileIron şirket sitenize yönetici olarak oturum açın.

8. Git **yönetici** > **kimlik**.

   * Seçin **AAD** seçeneğini **bulut IDP Kurulum ilgili bilgileri** alan.

    ![Oturum açma tek yönetici düğmesi yapılandırın](./media/active-directory-saas-mobileiron-tutorial/tutorial_mobileiron_admin.png)

9. Değerlerini kopyalamayı **anahtar** ve **konak** ve bunları URL'lerinde tamamlamak için Yapıştır **MobileIron etki alanı ve URL'leri** Azure portalı bölümünde.

    ![Oturum açma tek yönetici düğmesi yapılandırın](./media/active-directory-saas-mobileiron-tutorial/key.png)

10. İçinde **verme meta veri dosyasından AAD ve içeri aktarma MobileIron bulut alanına** tıklatın **Dosya Seç** Azure Portalı'ndan indirilen meta veriler karşıya yüklemek için. Tıklatın **Bitti** bir kez yüklenir.
 
    ![Çoklu oturum açma yönetici meta verileri düğmesi yapılandırın](./media/active-directory-saas-mobileiron-tutorial/tutorial_mobileiron_adminmetadata.png)

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-mobileiron-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-mobileiron-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-mobileiron-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-mobileiron-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-a-mobileiron-test-user"></a>MobileIron test kullanıcısı oluşturma

Azure AD kullanıcıları için MobileIron oturum açmak etkinleştirmek için bunların MobileIron sağlanmalıdır.  
MobileIron söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. MobileIron şirket sitenize yönetici olarak oturum açın.

2. Git **kullanıcılar** ve tıklayın **ekleme** > **tek kullanıcı**.

    ![Çoklu oturum açma kullanıcı düğmesi yapılandırın](./media/active-directory-saas-mobileiron-tutorial/tutorial_mobileiron_user.png)

3. Üzerinde **"Tek kullanıcı"** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma yapılandırma Kullanıcı Ekle düğmesi](./media/active-directory-saas-mobileiron-tutorial/tutorial_mobileiron_useradd.png)

    a. İçinde **e-posta adresi** metin kutusunda, bir kullanıcı gibi e-posta girin brittasimon@contoso.com.

    b. İçinde **ad** metin kutusuna, Britta gibi kullanıcının ilk adını girin.

    c. İçinde **Soyadı** metin kutusuna, Simon gibi kullanıcının soyadını girin.
    
    d. **Bitti**’ye tıklayın.  

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta MobileIron için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**MobileIron için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **MobileIron**.

    ![Uygulamalar listesinde MobileIron bağlantı](./media/active-directory-saas-mobileiron-tutorial/tutorial_mobileiron_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli MobileIron parçasında tıklattığınızda, otomatik olarak MobileIron uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)


<!--Image references-->

[1]: ./media/active-directory-saas-mobileiron-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mobileiron-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mobileiron-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mobileiron-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mobileiron-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mobileiron-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mobileiron-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mobileiron-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mobileiron-tutorial/tutorial_general_203.png

