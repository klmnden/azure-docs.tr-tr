---
title: "Öğretici: Cisco Spark Azure Active Directory Tümleştirme | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve Cisco Spark arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c47894b1-f5df-4755-845d-f12f4c602dc4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: a0a221622afe1c801a331e2319f3a7ace3111dad
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-spark"></a>Öğretici: Cisco Spark Azure Active Directory Tümleştirme

Bu öğreticide, Cisco Spark Azure Active Directory (Azure AD) ile tümleştirme öğrenin.

Cisco Spark Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Cisco Spark erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için Cisco Spark açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Cisco Spark ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Cisco Spark çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Cisco Spark Galeriden ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-cisco-spark-from-the-gallery"></a>Cisco Spark Galeriden ekleme
Azure AD Cisco Spark tümleştirilmesi yapılandırmak için Cisco Spark Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Cisco Spark Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Cisco Spark**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_search.png)

5. Sonuçlar panelinde seçin **Cisco Spark**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Cisco "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı Spark ile test etme

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Cisco Spark bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı Cisco Spark arasında bir bağlantı ilişkisi kurulması gerekir.

Cisco Spark değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Cisco Spark ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Cisco Spark test kullanıcısı oluşturma](#creating-a-cisco-spark-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Cisco Spark sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Cisco Spark uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Cisco Spark yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Cisco Spark** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_samlbase.png)

3. Üzerinde **Cisco Spark etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL'yi yazın:`https://web.ciscospark.com/#/signin`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://idbroker.webex.com/<companyname>`

    > [!NOTE] 
    > Bu değer gerçek değil. Bu değer gerçek tanımlayıcısı ile güncelleştirin. Kişi [Cisco Spark istemci destek ekibi](https://support.ciscospark.com/) bu değeri alınamıyor. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_certificate.png) 

5. Cisco Spark uygulama özel öznitelikler içerecek şekilde SAML onaylar bekler. Bu uygulama için aşağıdaki öznitelikleri yapılandırabilirsiniz. Bu öznitelik değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirmesi sayfasında bölüm. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir.
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_07.png) 

6. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, yukarıdaki resimde gösterildiği gibi SAML belirteci özniteliği yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik adı  | Öznitelik değeri |
    | --------------- | -------------------- |    
    |   Kullanıcı Kimliği    | User.userPrincipalName |   

    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-cisco-spark-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-cisco-spark-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. **Tamam**’a tıklayın.

7. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_400.png)

8. Oturum [Cisco bulut işbirliği Yönetimi](https://admin.ciscospark.com/) tam yönetici kimlik bilgilerinizle.

9. Seçin **ayarları** ve altında **kimlik doğrulaması** 'yi tıklatın **Değiştir**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_10.png)
    
10. Seçin **3. taraf kimlik sağlayıcısı tümleştirin. (Gelişmiş)**  ve sonraki ekranına gidin.

11. Üzerinde **IDP meta verileri içeri aktarma** sayfasında, ya da sürükle ve Azure AD meta veri dosyası sayfaya bırak veya bulun ve Azure AD meta veri dosyası karşıya yüklemek için dosya tarayıcısı seçeneğini kullanın. Ardından, seçin **meta veriler (daha güvenli) bir sertifika yetkilisi tarafından imzalanan sertifika gerekli** tıklatıp **sonraki**. 
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_11.png)

12. Seçin **SSO Bağlantıyı Sına**ve yeni bir tarayıcı sekmesinde oturum açtığında, Azure AD ile oturum açarak kimlik doğrulaması.

13. Geri dönüp **Cisco bulut işbirliği Yönetim** tarayıcı sekmesinde. Test başarılı olduysa, seçin **bu test başarılı oldu. Çoklu oturum açma seçeneği etkinleştirme** tıklatıp **sonraki**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-cisco-spark-test-user"></a>Cisco Spark test kullanıcısı oluşturma

Bu bölümde, Cisco Spark Britta Simon adlı bir kullanıcı oluşturun. Bu bölümde, Cisco Spark Britta Simon adlı bir kullanıcı oluşturun.

1. Git [Cisco bulut işbirliği Yönetimi](https://admin.ciscospark.com/) tam yönetici kimlik bilgilerinizle.

2. Tıklatın **kullanıcılar** ve ardından **kullanıcıları yönetme**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_12.png) 

3. İçinde **yönetmek kullanıcı** penceresinde, seçin **el ile ekleyin veya kullanıcıları değiştirin** tıklatıp **sonraki**.

4. Seçin **adlarını ve e-posta adresi**. Ardından, metin kutusu aşağıdaki gibi doldurun:
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_13.png) 
    
    a. İçinde **ad** metin kutusuna, türü **Britta**. 
    
    b. İçinde **Soyadı** metin kutusuna, türü **Simon**.
    
    c. İçinde **e-posta adresi** metin kutusuna, türü  **britta.simon@contoso.com** .

5. Britta Simon eklemek için artı işaretine tıklayın. Ardından **İleri**'ye tıklayın.

6. İçinde **Hizmetleri kullanıcıları eklemek** penceresinde tıklatın **kaydetmek** ve ardından **son**.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Cisco Spark erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı atama][200] 

**Cisco Spark Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Cisco Spark**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Azure AD SSO yapılandırmanızı test etmektir.

Erişim paneli Cisco Spark parçasında tıklattığınızda, otomatik olarak Cisco Spark uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_04.png
[10]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_060.png
[100]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_203.png

