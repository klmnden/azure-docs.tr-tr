---
title: 'Öğretici: Azure Active Directory Tümleştirme ile BetterWorks | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile BetterWorks arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 5bb9505a-be02-46ae-9979-5308715d2b47
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 35293869b066c593c18c01f08fa7f13c48e5f84a
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36212215"
---
# <a name="tutorial-azure-active-directory-integration-with-betterworks"></a>Öğretici: Azure Active Directory Tümleştirme BetterWorks ile

Bu öğreticide, Azure Active Directory (Azure AD) ile BetterWorks tümleştirmek öğrenin.

BetterWorks Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- BetterWorks erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için BetterWorks (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme BetterWorks ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir BetterWorks çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden BetterWorks ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-betterworks-from-the-gallery"></a>Galeriden BetterWorks ekleme
Azure AD BetterWorks tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden BetterWorks eklemeniz gerekir.

**Galeriden BetterWorks eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **BetterWorks**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/betterworks-tutorial/tutorial_betterworks_search.png)

5. Sonuçlar panelinde seçin **BetterWorks**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/betterworks-tutorial/tutorial_betterworks_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı BetterWorks ile test etme

Tekli çalışmaya oturum için Azure AD BetterWorks karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının BetterWorks ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

BetterWorks içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma BetterWorks ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[BetterWorks test kullanıcısı oluşturma](#creating-a-betterworks-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı BetterWorks sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma BetterWorks uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile BetterWorks yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **BetterWorks** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/betterworks-tutorial/tutorial_betterworks_samlbase.png)

3. Üzerinde **BetterWorks etki alanı ve URL'leri** uygulamada yapılandırmak istiyorsanız, bölüm **IDP başlatılan modu**:

    ![Çoklu oturum açmayı yapılandırın](./media/betterworks-tutorial/tutorial_betterworks_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://app.betterworks.com/saml2/metadata/`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://app.betterworks.com/saml2/acs/`

4. Üzerinde **BetterWorks etki alanı ve URL'leri** uygulamada yapılandırmak istiyorsanız, bölüm **SP tarafından başlatılan modu**, aşağıdaki adımları gerçekleştirin:
    
    ![Çoklu oturum açmayı yapılandırın](./media/betterworks-tutorial/tutorial_betterworks_url1.png)

    a. Tıklayın **Göster Gelişmiş URL ayarları**.

    b. İçinde **oturum üzerinde URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://app.betterworks.com`

    > [!NOTE] 
    > Bunlar gerçek değerleri değildir. Bu değerleri yanıt URL'si, tanımlayıcı ve üzerinde gerçek oturum URL'si ile güncelleştirin. Kişi [BetterWorks destek ekibi](mailto:support@betterworks.com) bu değerleri almak için.
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/betterworks-tutorial/tutorial_betterworks_certificate.png)  

5. BetterWorks uygulaması SAML onaylar belirli bir biçimde bekliyor. Bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz "**özniteliği**" Uygulama sekmesinde. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir. 

    ![Çoklu oturum açmayı yapılandırın](./media/betterworks-tutorial/tutorial_betterworks_attribute.png)

6. Üzerinde **SAML belirteci öznitelikleri** iletişim kutusunda, aşağıdaki tabloda gösterilen her satır için aşağıdaki adımları gerçekleştirin:
 
   | Öznitelik Adı | Öznitelik Değeri |
   | -------------- |  ------------ |
   | saml_token     | bd189cf6-1701-11e6-8f90-d26992eca2a5 |

   a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/betterworks-tutorial/tutorial_officespace_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/betterworks-tutorial/tutorial_officespace_05.png)

   b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın. 

   c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
   d. **Tamam**’a tıklayın.

7. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/betterworks-tutorial/tutorial_general_400.png)

8. Çoklu oturum açma yapılandırmak için **BetterWorks** yan, indirilen göndermek için ihtiyacınız **meta veri XML** için [BetterWorks destek ekibi](mailto:support@betterworks.com).


> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/betterworks-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/betterworks-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/betterworks-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/betterworks-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-betterworks-test-user"></a>BetterWorks test kullanıcısı oluşturma

Bu bölümde, BetterWorks içinde Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [BetterWorks destek ekibi](mailto:support@betterworks.com) BetterWorks platform kullanıcıları eklemek için.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta BetterWorks için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**BetterWorks için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **BetterWorks**.

    ![Çoklu oturum açmayı yapılandırın](./media/betterworks-tutorial/tutorial_betterworks_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.

Erişim paneli BetterWorks parçasında tıklattığınızda, otomatik olarak BetterWorks uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)


<!--Image references-->

[1]: ./media/betterworks-tutorial/tutorial_general_01.png
[2]: ./media/betterworks-tutorial/tutorial_general_02.png
[3]: ./media/betterworks-tutorial/tutorial_general_03.png
[4]: ./media/betterworks-tutorial/tutorial_general_04.png

[100]: ./media/betterworks-tutorial/tutorial_general_100.png

[200]: ./media/betterworks-tutorial/tutorial_general_200.png
[201]: ./media/betterworks-tutorial/tutorial_general_201.png
[202]: ./media/betterworks-tutorial/tutorial_general_202.png
[203]: ./media/betterworks-tutorial/tutorial_general_203.png

