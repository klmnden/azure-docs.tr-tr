---
title: 'Öğretici: Azure Active Directory Tümleştirme Pasifik zaman çizelgesi ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve Pasifik zaman çizelgesi arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: e546e8ba-821a-4942-9545-c84b0670beab
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 2ffac62a4248b8c3ec8b6a9ab24a4abb07f24bc9
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35972058"
---
# <a name="tutorial-azure-active-directory-integration-with-pacific-timesheet"></a>Öğretici: Azure Active Directory Tümleştirme ile Pasifik zaman çizelgesi

Bu öğreticide, Azure Active Directory (Azure AD) ile Pasifik zaman çizelgesi tümleştirmek öğrenin.

Pasifik zaman çizelgesi Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Pasifik zaman çizelgesi erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) Pasifik çizelgesine açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Pasifik zaman çizelgesi ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Pasifik zaman çizelgesi çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Pasifik zaman çizelgesi ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-pacific-timesheet-from-the-gallery"></a>Galeriden Pasifik zaman çizelgesi ekleme
Azure AD Pasifik zaman çizelgesi tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Pasifik zaman çizelgesi eklemeniz gerekir.

**Galeriden Pasifik zaman çizelgesi eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Pasifik zaman çizelgesi**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/pacific-timesheet-tutorial/tutorial_pacifictimesheet_search.png)

5. Sonuçlar panelinde seçin **Pasifik zaman çizelgesi**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/pacific-timesheet-tutorial/tutorial_pacifictimesheet_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Pasifik "Britta Simon" adlı bir test kullanıcıyı temel alarak zaman çizelgesi ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Pasifik çizelgesinde bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Pasifik zaman çizelgesi ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değeri Pasifik çizelgesinde atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Pasifik zaman çizelgesi ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Pasifik zaman çizelgesi test kullanıcısı oluşturma](#creating-a-pacific-timesheet-test-user)**  - Britta Simon, karşılık gelen Pasifik kullanıcı Azure AD gösterimini bağlantılı zaman çizelgesi sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Pasifik zaman çizelgesi uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Pasifik zaman çizelgesi ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Pasifik zaman çizelgesi** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/pacific-timesheet-tutorial/tutorial_pacifictimesheet_samlbase.png)

3. Üzerinde **Pasifik zaman çizelgesi etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/pacific-timesheet-tutorial/tutorial_pacifictimesheet_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<InstanceID>.pacifictimesheet.com/timesheet/home.do`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<InstanceID>.pacifictimesheet.com/timesheet/home.do`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. Kişi [Pasifik zaman çizelgesi destek ekibi](http://www.pacifictimesheet.com/support) bu değerleri almak için.
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/pacific-timesheet-tutorial/tutorial_pacifictimesheet_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/pacific-timesheet-tutorial/tutorial_general_400.png)

6. Üzerinde **Pasifik zaman çizelgesi yapılandırma** 'yi tıklatın **Pasifik zaman çizelgesi yapılandırmak** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/pacific-timesheet-tutorial/tutorial_pacifictimesheet_configure.png) 

7. Çoklu oturum açma yapılandırmak için **Pasifik zaman çizelgesi** yan, indirilen göndermek için ihtiyacınız **sertifika (Base64)**, **SAML çoklu oturum açma hizmet URL'si**ve **SAML varlık kimliği** için [Pasifik zaman çizelgesi destek ekibi](http://www.pacifictimesheet.com/support). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/pacific-timesheet-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/pacific-timesheet-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/pacific-timesheet-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/pacific-timesheet-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-pacific-timesheet-test-user"></a>Pasifik zaman çizelgesi test kullanıcısı oluşturma

Bu bölümde, Pasifik zaman çizelgesinde Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [Pasifik zaman çizelgesi destek ekibi](http://www.pacifictimesheet.com/support) uygulamada bir kullanıcı oluşturmak için.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Pasifik zaman çizelgesine erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Pasifik zaman çizelgesine Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Pasifik zaman çizelgesi**.

    ![Çoklu oturum açmayı yapılandırın](./media/pacific-timesheet-tutorial/tutorial_pacifictimesheet_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.

Erişim paneli Pasifik zaman çizelgesi parçasında tıklattığınızda, otomatik olarak Pasifik zaman çizelgesi uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/pacific-timesheet-tutorial/tutorial_general_01.png
[2]: ./media/pacific-timesheet-tutorial/tutorial_general_02.png
[3]: ./media/pacific-timesheet-tutorial/tutorial_general_03.png
[4]: ./media/pacific-timesheet-tutorial/tutorial_general_04.png

[100]: ./media/pacific-timesheet-tutorial/tutorial_general_100.png

[200]: ./media/pacific-timesheet-tutorial/tutorial_general_200.png
[201]: ./media/pacific-timesheet-tutorial/tutorial_general_201.png
[202]: ./media/pacific-timesheet-tutorial/tutorial_general_202.png
[203]: ./media/pacific-timesheet-tutorial/tutorial_general_203.png

