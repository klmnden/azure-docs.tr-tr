---
title: 'Öğretici: Azure Active Directory Tümleştirme ile iWellnessNow | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile iWellnessNow arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 24ffc841-7a77-481c-9cc4-6f8bda58fe66
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: c3d2e359927d1e1e6aec825145fc84cf2a83a946
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35972061"
---
# <a name="tutorial-azure-active-directory-integration-with-iwellnessnow"></a>Öğretici: Azure Active Directory Tümleştirme iWellnessNow ile

Bu öğreticide, Azure Active Directory (Azure AD) ile iWellnessNow tümleştirmek öğrenin.

İWellnessNow Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- İWellnessNow erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak Azure AD hesaplarına sahip (çoklu oturum açma) iWellnessNow için açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme iWellnessNow ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir iWellnessNow çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden iWellnessNow ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-iwellnessnow-from-the-gallery"></a>Galeriden iWellnessNow ekleme
Azure AD iWellnessNow tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden iWellnessNow eklemeniz gerekir.

**Galeriden iWellnessNow eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **iWellnessNow**seçin **iWellnessNow** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde iWellnessNow](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı iWellnessNow sınayın.

Tekli çalışmaya oturum için Azure AD iWellnessNow karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının iWellnessNow ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma iWellnessNow ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir iWellnessNow test kullanıcısı oluşturma](#create-an-iwellnessnow-test-user)**  - bir Britta Simon karşılık gelen kullanıcı Azure AD gösterimini bağlı iWellnessNow içinde olması.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma iWellnessNow uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile iWellnessNow yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **iWellnessNow** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_samlbase.png)

3. Üzerinde **iWellnessNow etki alanı ve URL'leri** varsa, bölüm **hizmet sağlayıcısı meta veri dosyası** ve uygulamada yapılandırmak istediğiniz **IDP** modunda başlatılan gerçekleştirin Aşağıdaki adımlar:

    ![etki alanı ve URL'ler çoklu oturum açma iWellnessNow karşıya yükleme](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_upload.png)

    a. Tıklatın **karşıya yükleme meta veri dosyası**.

    ![etki alanı ve URL'ler çoklu oturum açma iWellnessNow uploadconfig](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_uploadconfig.png)

    b. Tıklayın **klasörü logosu** meta veri dosyası seçin ve **karşıya**.
    
    c. Karşıya başarıyla tamamlandıktan sonra **hizmet sağlayıcısı meta veri dosyası** **tanımlayıcısı** ve **yanıt URL'si** değerlerini alma otomatik olarak doldurulmuş  **iWellnessNow etki alanı ve URL'leri** aşağıda gösterildiği gibi metin kutusu bölümünde:

    ![iWellnessNow etki alanı ve URL'leri tek oturum açma bilgileri](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_url3.png)

4. Sahip değilseniz **hizmet sağlayıcısı meta veri dosyası** ve uygulamada yapılandırmak istediğiniz **IDP** modunda başlatılan aşağıdaki adımları gerçekleştirin:

    ![iWellnessNow etki alanı ve URL'leri tek oturum açma bilgileri](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `http://<CustomerName>.iwellnessnow.com`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<CustomerName>.iwellnessnow.com/ssologin`

5. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![iWellnessNow etki alanı ve URL'leri tek oturum açma bilgileri](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<CustomerName>.iwellnessnow.com/`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler, gerçek tanımlayıcı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Kişi [iWellnessNow istemci destek ekibi](mailto:info@iwellnessnow.com) bu değerleri almak için.

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/iwellnessnow-tutorial/tutorial_general_400.png)
    
7. Çoklu oturum açma yapılandırmak için **iWellnessNow** yan, indirilen göndermek için ihtiyacınız **meta veri XML** için [iWellnessNow destek ekibi](mailto:info@iwellnessnow.com). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/iwellnessnow-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/iwellnessnow-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/iwellnessnow-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/iwellnessnow-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-iwellnessnow-test-user"></a>Bir iWellnessNow test kullanıcısı oluşturma

Bu bölümde, iWellnessNow içinde Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [iWellnessNow destek ekibi](mailto:info@iwellnessnow.com) iWellnessNow platform kullanıcıları eklemek için. Kullanıcıların gerekir ve çoklu oturum açma kullanmadan önce etkinleştirildi

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta iWellnessNow erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**İWellnessNow için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **iWellnessNow**.

    ![Uygulamalar listesinde iWellnessNow bağlantı](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli iWellnessNow parçasında tıklattığınızda, otomatik olarak iWellnessNow uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/iwellnessnow-tutorial/tutorial_general_01.png
[2]: ./media/iwellnessnow-tutorial/tutorial_general_02.png
[3]: ./media/iwellnessnow-tutorial/tutorial_general_03.png
[4]: ./media/iwellnessnow-tutorial/tutorial_general_04.png

[100]: ./media/iwellnessnow-tutorial/tutorial_general_100.png

[200]: ./media/iwellnessnow-tutorial/tutorial_general_200.png
[201]: ./media/iwellnessnow-tutorial/tutorial_general_201.png
[202]: ./media/iwellnessnow-tutorial/tutorial_general_202.png
[203]: ./media/iwellnessnow-tutorial/tutorial_general_203.png

