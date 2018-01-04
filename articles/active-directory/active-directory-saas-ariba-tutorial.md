---
title: "Öğretici: Azure Active Directory Tümleştirme ile aralarında Ariba | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile aralarında Ariba arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 45a8364c-55d1-4dc7-b079-9eb2a701842d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2017
ms.author: jeedes
ms.openlocfilehash: 167bef10b696866a9034314c383468744ebf637b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ariba"></a>Öğretici: Azure Active Directory Tümleştirme biçimi Ariba ile

Bu öğreticide, Azure Active Directory (Azure AD) ile aralarında Ariba tümleştirmek öğrenin.

Aralarında Ariba Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Aralarında Ariba erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için biçimi Ariba (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme biçimi Ariba ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir biçimi Ariba çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden biçimi Ariba ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-ariba-from-the-gallery"></a>Galeriden biçimi Ariba ekleme
Azure AD biçimi Ariba tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden biçimi Ariba eklemeniz gerekir.

**Galeriden biçimi Ariba eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **biçimi Ariba**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-ariba-tutorial/tutorial_ariba_search.png)

5. Sonuçlar panelinde seçin **biçimi Ariba**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-ariba-tutorial/tutorial_ariba_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı biçimi Ariba ile test etme

Tekli çalışmaya oturum için Azure AD biçimi Ariba karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının biçimi Ariba ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değeri aralarında Ariba, Ata **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma biçimi Ariba ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Aralarında Ariba test kullanıcısı oluşturma](#creating-an-ariba-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı biçimi Ariba sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma biçimi Ariba uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma biçimi Ariba ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **biçimi Ariba** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ariba-tutorial/tutorial_ariba_samlbase.png)

3. Üzerinde **biçimi Ariba etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ariba-tutorial/tutorial_ariba_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.sourcing.ariba.com` veya`https://<subdomain>.supplier.ariba.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:`http://<subdomain>.procurement-2.ariba.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Burada dizesinin benzersiz değeri tanımlayıcıda kullanmanızı öneririz. Aralarında Ariba istemci destek ekibi ile iletişime geçin **1-866-218-2155** bu değerleri almak için. 
 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ariba-tutorial/tutorial_ariba_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ariba-tutorial/tutorial_general_400.png)

6. Uygulamanız için yapılandırılmış SSO almak için aralarında Ariba destek ekibi çağırmak **1-866-218-2155** ve, daha fazla indirilen sağlayacağını nasıl yardımcı **sertifika (Base64)** dosyası.  
 
> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-ariba-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-ariba-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-ariba-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-ariba-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-an-ariba-test-user"></a>Aralarında Ariba test kullanıcısı oluşturma

Bu bölümün amacı aralarında Ariba Britta Simon adlı bir kullanıcı oluşturmaktır. Çalışma biçimi Ariba destek ekibi ile **1-866-218-2155** biçimi Ariba sistemde kullanıcıları eklemek için. 

 >[!NOTE]
 >Kullanıcı el ile oluşturmanız gerekiyorsa, biçimi Ariba destek ekibi ile iletişime geçmeniz **1-866-218-2155**.
 >  

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, aralarında Ariba için erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı atama][200] 

**Aralarında Ariba için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **biçimi Ariba**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ariba-tutorial/tutorial_ariba_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme
Bu bölümün amacı erişim paneli kullanılarak Azure AD SSO yapılandırmanızı test etmektir.

Erişim paneli biçimi Ariba parçasında tıklattığınızda, otomatik olarak biçimi Ariba uygulamanıza açan. 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_203.png

