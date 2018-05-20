---
title: 'Öğretici: Azure Active Directory Tümleştirme ile RFPIO | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile RFPIO arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 87187076-7b50-4247-814f-f217b052703f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 915b6f9c96bc8dbab54e770340acf795df92a3fb
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-rfpio"></a>Öğretici: Azure Active Directory Tümleştirme RFPIO ile

Bu öğreticide, Azure Active Directory (Azure AD) ile RFPIO tümleştirmek öğrenin.

RFPIO Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Denetleyebilirsiniz kimin RFPIO erişimi, Azure AD'de.
- Otomatik olarak için RFPIO (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda--Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme RFPIO ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD abonelik.
- RFPIO tek bir oturum üzerinde etkin olmayan abonelik.

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamında kullanmanızı öneririz yok.

Bu öğreticide adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, alabileceğiniz bir [bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. RFPIO Galeriden ekleniyor.
2. Yapılandırma ve Azure AD sınama çoklu oturum açmayı.

## <a name="add-rfpio-from-the-gallery"></a>Galeriden RFPIO Ekle
Azure AD RFPIO tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden RFPIO eklemeniz gerekir.

### <a name="to-add-rfpio-from-the-gallery"></a>Galeriden RFPIO eklemek için

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti bölmesinde seçin **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Seçin **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **RFPIO**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_search.png)

5. Sonuçlar panelinde seçin **RFPIO**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı RFPIO sınayın.

Tekli çalışmaya oturum için Azure AD RFPIO karşılık gelen kullanıcı Azure AD'de kullanıcı arasındaki ilişki nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının RFPIO ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

RFPIO içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma RFPIO ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**--bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**--Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[RFPIO test kullanıcısı oluşturma](#creating-a-rfpio-test-user)**  --Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı RFPIO sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assigning-the-azure-ad-test-user)**--Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#testing-single-sign-on)**  --yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma RFPIO uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile RFPIO yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **RFPIO** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_samlbase.png)

3. Üzerinde **RFPIO etki alanı ve URL'leri** uygulamada yapılandırmak istiyorsanız, bölüm **IDP** modu tarafından başlatılan:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL'yi yazın: `https://www.rfpio.com`

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url1.png)

    b. Denetleme **Göster Gelişmiş URL ayarları**.

    c. İçinde **geçiş durumunu** metin kutusuna bir dize değeri girin. Kişi [RFPIO destek ekibi](https://www.rfpio.com/contact/) bu değeri alınamıyor. 

4. Denetleme **Göster Gelişmiş URL ayarları**. Uygulamada yapılandırmak istiyorsanız **SP** modu tarafından başlatılan: 

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url2.png)

    İçinde **URL üzerinde oturum** metin kutusuna, URL'yi yazın: `https://www.app.rfpio.com`

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-rfpio-tutorial/tutorial_general_400.png)

7. Farklı bir web tarayıcı penceresinde, oturum açma **RFPIO** yönetici olarak Web sitesi.

8. Alt Sol Köşe açılan'ı tıklatın.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-rfpio-tutorial/app1.png)

9. Tıklayın **kuruluş ayarları**. 

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-rfpio-tutorial/app2.png)

10. Tıklayın **özellikler ve tümleştirme**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-rfpio-tutorial/app4.png)

11. İçinde **SAML SSO yapılandırma** tıklatın **Düzenle**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-rfpio-tutorial/app3.png)

12. Bu bölümde aşağıdaki işlemleri gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-rfpio-tutorial/app5.png)
    
    a. İçeriği Kopyala **indirilen meta veri XML** ve yapıştırın **kimlik Yapılandırması** alan.

    > [!NOTE]
    >İndirilen içeriği kopyalamak için **meta veri XML** kullanım **not defteri ++** veya uygun **XML Düzenleyicisi**. 

    b. Tıklatın **doğrulamak**.

    c. ' I tıklattıktan sonra **doğrulamak**Çevir, **SAML(Enabled)** için açık.

    d. Tıklatın **gönderme**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-rfpio-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-rfpio-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-rfpio-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-rfpio-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-rfpio-test-user"></a>RFPIO test kullanıcısı oluşturma

Azure AD kullanıcıları için RFPIO oturum açmak etkinleştirmek için bunların RFPIO sağlanmalıdır.  
RFPIO söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. RFPIO şirket sitenize yönetici olarak oturum açın.

2. Alt Sol Köşe açılan'ı tıklatın.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-rfpio-tutorial/app1.png)

3. Tıklayın **kuruluş ayarları**. 

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-rfpio-tutorial/app2.png)

4. Tıklatın **ekip ÜYELERİNİN**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-rfpio-tutorial/app6.png)

5. Tıklayın **ÜYELER Ekle**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-rfpio-tutorial/app7.png)

6. İçinde **eklediğiniz yeni üyeler** bölümü. Aşağıdaki işlemleri gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-rfpio-tutorial/app8.png)

    a. ENTER **e-posta adresi** içinde **satır başına bir e-posta girin** alan.

    b. Lütfen seçin **rol** gereksinimlerinize göre.

    c. Tıklatın **ÜYELER Ekle**.
        
    > [!NOTE]
    > Azure Active Directory hesap sahibi bir e-posta alır ve bunu etkinleştirilmeden önce kendi hesabı onaylamak için bir bağlantı izler.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta RFPIO için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**RFPIO için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **RFPIO**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Erişim paneli RFPIO parçasında tıklattığınızda, otomatik olarak RFPIO uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile ilgili öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_203.png

