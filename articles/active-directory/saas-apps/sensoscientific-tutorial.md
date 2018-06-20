---
title: 'Öğretici: Azure Active Directory Tümleştirme SensoScientific kablosuz sıcaklık izleme sistemi | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile SensoScientific kablosuz sıcaklık izleme sistemi arasındaki yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: ee9a924d-ccde-45b0-ab40-877f82f5dfa2
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: jeedes
ms.openlocfilehash: bb0952fe5b78b0a9933cf7e9f20787150bdbcc3b
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36231517"
---
# <a name="tutorial-azure-active-directory-integration-with-sensoscientific-wireless-temperature-monitoring-system"></a>Öğretici: Azure Active Directory Tümleştirme ile SensoScientific kablosuz sıcaklık sistem izleme

Bu öğreticide, Azure Active Directory (Azure AD) ile SensoScientific kablosuz sıcaklık sistem izleme tümleştirmek öğrenin.

SensoScientific kablosuz sıcaklık sistem izleme Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- SensoScientific kablosuz sıcaklık sistem izleme erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak SensoScientific kablosuz sıcaklık izleme sistemine (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme SensoScientific kablosuz sıcaklık izleme sistemi ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir SensoScientific kablosuz sıcaklık sistem izleme çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden SensoScientific kablosuz sıcaklık sistem izleme ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-sensoscientific-wireless-temperature-monitoring-system-from-the-gallery"></a>Galeriden SensoScientific kablosuz sıcaklık sistem izleme ekleme
Azure AD SensoScientific kablosuz sıcaklık sistem izleme tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden SensoScientific kablosuz sıcaklık sistem izleme eklemeniz gerekir.

**Galeriden SensoScientific kablosuz sıcaklık izleme sistemine eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **SensoScientific kablosuz sıcaklık sistem izleme**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_search.png)

5. Sonuçlar panelinde seçin **SensoScientific kablosuz sıcaklık sistem izleme**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma SensoScientific kablosuz Sıcaklık "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı sistem izleme ile test etme

Tekli çalışmaya oturum için Azure AD ne karşılık gelen SensoScientific kablosuz Sıcaklık İzleme sistem bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının SensoScientific kablosuz sıcaklık sistem izleme ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** SensoScientific kablosuz Sıcaklık İzleme sistem.

Yapılandırmak ve Azure AD çoklu oturum açma SensoScientific kablosuz sıcaklık izleme sistemi sınamak için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[SensoScientific kablosuz sıcaklık sistem izleme test kullanıcısı oluşturma](#creating-a-sensoscientific-wireless-temperature-monitoring-system-test-user)**  - Britta Simon, karşılık gelen SensoScientific kablosuz sıcaklık kullanıcı Azure AD gösterimini bağlantılı sistem izleme sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma SensoScientific kablosuz sıcaklık sistem izleme uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma SensoScientific kablosuz sıcaklık izleme sistemi ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **SensoScientific kablosuz sıcaklık sistem izleme** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_samlbase.png)

3. Üzerinde **SensoScientific kablosuz sıcaklık sistem etki alanı izleme ve URL'leri** bölümü, uygulama zaten Azure ile önceden tümleştirilmiş gibi herhangi bir adım gerçekleştirmeniz gerekir:

    ![Çoklu oturum açmayı yapılandırın](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_url.png)

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/sensoscientific-tutorial/tutorial_general_400.png)

6. Üzerinde **SensoScientific kablosuz Sıcaklık İzleme Sistem Yapılandırması** 'yi tıklatın **SensoScientific kablosuz Sıcaklık İzleme sistem yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği** ve **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_configure.png) 

7. SensoScientific kablosuz sıcaklık sistem izleme uygulamanızı yönetici olarak oturum açma.

8. Üst gezinti menüsünde **yapılandırma** ve Git **yapılandırma** altında **çoklu oturum açma** üzerinde tek oturum ayarlarını açın.

    ![Çoklu oturum açmayı yapılandırın](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_admin.png) 

9. İçinde **tek oturum açma ayarları** form aşağıdaki adımları gerçekleştirin:
 
    a. Seçin **verenin adı** Azure AD olarak.
    
    b. Yapıştır **SAML varlık kimliği** veren URL'si metin kutusuna Azure portalından kopyalanan.
    
    c. Yapıştır **SAML çoklu oturum açma hizmet URL'si** çoklu oturum açma hizmet URL'si metin kutusuna Azure portalından kopyalanan.

    d. Yapıştır **Sign-Out URL** tek Sign-Out hizmeti URL'si metin kutusuna Azure portalından kopyalanan.

    e. Azure portalından indirdiğiniz ve burada karşıya sertifika göz atın.
    
    f. **Kaydet**’e tıklayın.
  
> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler](https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/sensoscientific-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/sensoscientific-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/sensoscientific-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/sensoscientific-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-sensoscientific-wireless-temperature-monitoring-system-test-user"></a>SensoScientific kablosuz sıcaklık sistem izleme test kullanıcısı oluşturma

Azure AD kullanıcılarının SensoScientific kablosuz Sıcaklık İzleme sisteme oturum açmayı etkinleştirmek için bunlar SensoScientific kablosuz sıcaklık izleme sistemine sağlanmalıdır. Çalışmak [SensoScientific kablosuz sıcaklık sistem izleme destek ekibi](https://www.sensoscientific.com/contact-us/) SensoScientific kablosuz sıcaklık sistem izleme platform kullanıcıları eklemek için. Kullanıcıların oluşturulan ve çoklu oturum açma kullanmadan önce etkinleştirilmelidir. 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta SensoScientific kablosuz sıcaklık izleme sistemine erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Britta Simon SensoScientific kablosuz sıcaklık izleme sistemine atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **SensoScientific kablosuz sıcaklık sistem izleme**.

    ![Çoklu oturum açmayı yapılandırın](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin. Erişim panelinde SensoScientific kablosuz sıcaklık sistem izleme kutucuğuna tıklayın, otomatik olarak imzalanmış SensoScientific kablosuz sıcaklık sistem izleme uygulamanıza açma. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://msdn.microsoft.com/library/dn308586).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/sensoscientific-tutorial/tutorial_general_01.png
[2]: ./media/sensoscientific-tutorial/tutorial_general_02.png
[3]: ./media/sensoscientific-tutorial/tutorial_general_03.png
[4]: ./media/sensoscientific-tutorial/tutorial_general_04.png

[100]: ./media/sensoscientific-tutorial/tutorial_general_100.png

[200]: ./media/sensoscientific-tutorial/tutorial_general_200.png
[201]: ./media/sensoscientific-tutorial/tutorial_general_201.png
[202]: ./media/sensoscientific-tutorial/tutorial_general_202.png
[203]: ./media/sensoscientific-tutorial/tutorial_general_203.png

