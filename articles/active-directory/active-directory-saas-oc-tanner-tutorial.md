---
title: "Öğretici: Azure Active Directory Tümleştirme O.C. ile Etikan - AppreciateHub | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile O.C. arasında yapılandırmayı öğrenin Etikan - AppreciateHub."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: dee8fbca-0b60-4a21-8917-1fb6919de5a0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: jeedes
ms.openlocfilehash: ae98e6fce3507e023a72cab35894c7c2f7a87656
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-oc-tanner---appreciatehub"></a>Öğretici: Azure Active Directory Tümleştirme O.C. ile Etikan - AppreciateHub

Bu öğreticide, O.C. tümleştirmek öğrenin Etikan - AppreciateHub Azure ile Active Directory (Azure AD).

O.C. tümleştirme Etikan - AppreciateHub Azure AD ile aşağıdaki faydaları sağlar:

- O.C. erişimi, Azure AD'de kontrol edebilirsiniz Etikan - AppreciateHub
- Otomatik olarak O.C. için açan kullanıcılarınıza etkinleştirebilirsiniz Etikan - Azure AD hesaplarına sahip AppreciateHub (çoklu oturum açma)
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme O.C. ile yapılandırmak için Etikan - AppreciateHub, aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- BİR O.C. Etikan - abonelik etkin AppreciateHub çoklu oturum açma

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. O.C. ekleme Etikan - galerisinden AppreciateHub
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-oc-tanner---appreciatehub-from-the-gallery"></a>O.C. ekleme Etikan - galerisinden AppreciateHub
O.C. tümleştirmesini yapılandırmak için Etikan - AppreciateHub Azure AD'ye ihtiyacınız O.C. eklemek Etikan - yönetilen SaaS uygulamaları listenize galerisinden AppreciateHub.

**O.C. eklemek için Etikan - AppreciateHub galerisinden, aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **O.C. Etikan - AppreciateHub**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_search.png)

5. Sonuçlar panelinde seçin **O.C. Etikan - AppreciateHub**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma O.C. ile test etme Etikan - AppreciateHub "Britta Simon" adlı bir test kullanıcı tabanlı.

Tekli çalışmaya oturum için Azure AD O.C. ne karşılık gelen kullanıcı bilmesi gerekir Etikan - AppreciateHub bir kullanıcı için Azure AD içinde değil. Diğer bir deyişle, bir Azure AD kullanıcısının O.C. ilgili kullanıcı arasında bir bağlantı ilişkisi Etikan - AppreciateHub kurulması gerekir.

İçinde O.C. Etikan - AppreciateHub, değeri atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma O.C. ile test etmek için Etikan - AppreciateHub, aşağıdaki yapı taşları tamamlamanız gereken:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir O.C. oluşturma Etikan - AppreciateHub test kullanıcısı](#creating-a-oc-tanner---appreciatehub-test-user)**  - Britta Simon, karşılık gelen içinde O.C. sağlamak için Etikan - kullanıcı Azure AD gösterimini bağlı AppreciateHub.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirme ve çoklu oturum açma, O.C. yapılandırma Etikan - AppreciateHub uygulama.

**Azure AD çoklu oturum açma ile O.C. yapılandırmak için Etikan - AppreciateHub, aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **O.C. Etikan - AppreciateHub** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_samlbase.png)

3. Üzerinde **O.C. Etikan - AppreciateHub etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_url.png)

    a. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<companyname>.appreciatehub.com/fed/sp/authnResponse20`

    > [!NOTE] 
    > Bu değer gerçek değil. Bu değer ile gerçek yanıt URL'si güncelleştirin. Kişi [O.C. Etikan - AppreciateHub destek ekibi](mailto:sso@octanner.com) bu değeri alınamıyor.

    b. Aşağıdaki bağlantıyı kullanarak meta veri dosyası açın: [https://fed.appreciatehub.com/fed/sp/metadata](https://fed.appreciatehub.com/fed/sp/metadata).
   
    c. Bulun **md:AssertionConsumerService** düğümü. 
   
    d. Değerini kopyalayın **konumu** özniteliği. 
   
    ![Uygulama ayarlarını yapılandırın][12]
   
    e. İçinde **oturum üzerinde URL'si** textbox, önceki adımda elde değer geçti.

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_400.png)

6. Çoklu oturum açma yapılandırmak için **O.C. Etikan - AppreciateHub** yan, indirilen göndermek için ihtiyacınız **meta veri XML** için [O.C. Etikan - AppreciateHub destek ekibi](mailto:sso@octanner.com).

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-oc-tanner---appreciatehub-test-user"></a>Bir O.C. oluşturma Etikan - AppreciateHub test kullanıcısı

Bu bölümün amacı Britta Simon içinde O.C. adlı bir kullanıcı oluşturmaktır. Etikan - AppreciateHub.

**İçinde O.C. Britta Simon adlı bir kullanıcı oluşturmak için Etikan - AppreciateHub, aşağıdaki adımları gerçekleştirin:**

Sorun, [O.C. Etikan - AppreciateHub destek ekibi](mailto:sso@octanner.com) nameID özniteliği olarak Azure AD içinde Britta Simon kullanıcı adı ile aynı değere sahip bir kullanıcı oluşturmak için.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta O.C. için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştir Etikan - AppreciateHub.

![Kullanıcı atama][200] 

**O.C. için Britta Simon atamak için Etikan - AppreciateHub, aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **O.C. Etikan - AppreciateHub**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.  
O.C. tıkladığınızda Etikan - AppreciateHub döşeme erişim panelinde, otomatik olarak, O.C. açan Etikan - AppreciateHub uygulama.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_04.png

[12]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_08.png

[100]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_203.png

