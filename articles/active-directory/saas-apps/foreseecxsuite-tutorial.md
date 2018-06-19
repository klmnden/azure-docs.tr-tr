---
title: 'Öğretici: Azure Active Directory Tümleştirme Öngörüyor CX Suite ile | Microsoft Docs'
description: Çoklu oturum açma Öngörüyor CX Suite ile Azure Active Directory arasındaki yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 5f4b7830-6186-4d17-b77b-504d4192bfde
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2018
ms.author: jeedes
ms.openlocfilehash: 5cf217de4973fd06935d88381de86520df604c29
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35980660"
---
# <a name="tutorial-azure-active-directory-integration-with-foresee-cx-suite"></a>Öğretici: Azure Active Directory Tümleştirme Öngörüyor CX Suite ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Öngörüyor CX Suite tümleştirmek öğrenin.

Öngörüyor CX Suite Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Öngörüyor CX Suite erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak Öngörüyor CX paketine (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Öngörüyor CX paketiyle yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Öngörüyor CX Suite çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin.
Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Öngörüyor CX paketi ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-foresee-cx-suite-from-the-gallery"></a>Galeriden Öngörüyor CX paketi ekleme
Azure AD Öngörüyor CX Suite tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Öngörüyor CX paketi eklemeniz gerekir.

**Galeriden Öngörüyor CX paketi eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna yazın **Öngörüyor CX Suite**seçin **Öngörüyor CX Suite** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde CX Suite Öngörüyor](./media/foreseecxsuite-tutorial/tutorial_foreseecxsuite_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Öngörüyor CX "Britta Simon" adlı bir test kullanıcı tabanlı Suite ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Öngörüyor CX grubundaki bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Öngörüyor CX paketindeki ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Öngörüyor CX Suite ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Öngörüyor CX Suite test kullanıcısı oluşturma](#create-a-foresee-cx-suite-test-user)**  - Britta Simon, karşılık gelen Öngörüyor CX kullanıcı Azure AD gösterimini bağlantılı Suite sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Öngörüyor CX Suite uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Öngörüyor CX paketiyle yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Öngörüyor CX Suite** uygulama tümleştirmesi sayfasında, tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/foreseecxsuite-tutorial/tutorial_foreseecxsuite_samlbase.png)

3. Üzerinde **Öngörüyor CX Suite etki alanı ve URL'leri** varsa, bölüm **hizmet sağlayıcısı meta veri dosyası**, aşağıdaki adımları gerçekleştirin:

    ![CX Suite etki alanı ve URL'leri tek oturum açma bilgilerini Öngörüyor](./media/foreseecxsuite-tutorial/upload.png)

    a. Tıklatın **karşıya yükleme meta veri dosyası**.

    ![CX Suite etki alanı ve URL'leri tek oturum açma bilgilerini Öngörüyor](./media/foreseecxsuite-tutorial/tutorial_foreseen_uploadconfig.png)

    b. Tıklayın **klasörü logosu** meta veri dosyası seçin ve **karşıya**.

    c. Karşıya başarıyla tamamlandıktan sonra **hizmet sağlayıcısı meta veri dosyası** **tanımlayıcısı** değeri get otomatik doldurulmuş **Öngörüyor CX Suite etki alanı ve URL'leri** bölümü Aşağıda gösterildiği gibi metin:

    ![CX Suite etki alanı ve URL'leri tek oturum açma bilgilerini Öngörüyor](./media/foreseecxsuite-tutorial/urlupload.png)

4. Sahip değilseniz **hizmet sağlayıcısı meta veri dosyası**, aşağıdaki adımları gerçekleştirin:

    ![CX Suite etki alanı ve URL'leri tek oturum açma bilgilerini Öngörüyor](./media/foreseecxsuite-tutorial/tutorial_foreseecxsuite_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL'yi yazın: `https://cxsuite.foresee.com/`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://www.okta.com/saml2/service-provider/<UniqueID>`

    > [!NOTE]
    > Tanımlayıcı değeri gerçek değil. Bu değer gerçek tanımlayıcısı ile güncelleştirin. Kişi [Öngörüyor CX Suite istemci destek ekibi](mailto:support@foresee.com) bu değeri alınamıyor.

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/foreseecxsuite-tutorial/tutorial_foreseecxsuite_certificate.png)

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/foreseecxsuite-tutorial/tutorial_general_400.png)

7. Çoklu oturum açma yapılandırmak için **Öngörüyor CX Suite** yan, indirilen göndermek için ihtiyacınız **meta veri XML** için [Öngörüyor CX Suite Destek Ekibi](mailto:support@foresee.com). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/foreseecxsuite-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/foreseecxsuite-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/foreseecxsuite-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/foreseecxsuite-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-a-foresee-cx-suite-test-user"></a>Öngörüyor CX Suite test kullanıcısı oluşturma

Bu bölümde, Britta Simon Öngörüyor CX paketindeki adlı bir kullanıcı oluşturun. Çalışmak [Öngörüyor CX Suite Destek Ekibi](mailto:support@foresee.com) kullanıcıları veya Öngörüyor CX Suite Platform güvenilir listesinde olması için gerekli olan etki alanı eklemek için. Etki alanı ekibi tarafından eklediyseniz, kullanıcıların otomatik olarak Öngörüyor CX Suite platform için sağlanan. Kullanıcıların oluşturulan ve çoklu oturum açma kullanmadan önce etkinleştirilmelidir.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Öngörüyor CX paketine erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Britta Simon Öngörüyor CX paketine atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201]

2. Uygulamalar listesinde **Öngörüyor CX Suite**.

    ![Uygulamalar listesinde Öngörüyor CX Suite bağlantısı](./media/foreseecxsuite-tutorial/tutorial_foreseecxsuite_app.png)

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Öngörüyor CX Suite parçasında tıklattığınızda, otomatik olarak Öngörüyor CX Suite uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/foreseecxsuite-tutorial/tutorial_general_01.png
[2]: ./media/foreseecxsuite-tutorial/tutorial_general_02.png
[3]: ./media/foreseecxsuite-tutorial/tutorial_general_03.png
[4]: ./media/foreseecxsuite-tutorial/tutorial_general_04.png

[100]: ./media/foreseecxsuite-tutorial/tutorial_general_100.png

[200]: ./media/foreseecxsuite-tutorial/tutorial_general_200.png
[201]: ./media/foreseecxsuite-tutorial/tutorial_general_201.png
[202]: ./media/foreseecxsuite-tutorial/tutorial_general_202.png
[203]: ./media/foreseecxsuite-tutorial/tutorial_general_203.png

