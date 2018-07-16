---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Palo Alto Networks - GlobalProtect | Microsoft Docs'
description: Azure Active Directory ve Palo Alto Networks - GlobalProtect arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 03bef6f2-3ea2-4eaa-a828-79c5f1346ce5
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: jeedes
ms.openlocfilehash: 6f395897687235f0956928fd0a5dccf00d4c7d12
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39041050"
---
# <a name="tutorial-azure-active-directory-integration-with-palo-alto-networks---globalprotect"></a>Öğretici: Azure Active Directory Palo Alto Networks - GlobalProtect ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile GlobalProtect Palo Alto Networks - tümleştirme konusunda bilgi edinin.

Palo Alto Networks tarafından sağlanan-GlobalProtect Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Palo Alto Networks - GlobalProtect erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için Palo Alto Networks - GlobalProtect (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Palo Alto Networks - GlobalProtect, Azure AD tümleştirmesi yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Palo Alto Networks - GlobalProtect çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Palo Alto Networks tarafından sağlanan - galerisinden GlobalProtect ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-palo-alto-networks---globalprotect-from-the-gallery"></a>Palo Alto Networks tarafından sağlanan - galerisinden GlobalProtect ekleme
Palo Alto Networks - Azure AD'ye GlobalProtect tümleştirmesini yapılandırmak için listenize yönetilen SaaS uygulamaları Galerisi'nden GlobalProtect Palo Alto Networks - eklemeniz gerekir.

**Palo Alto Networks - galerisinden GlobalProtect eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Palo Alto Networks - GlobalProtect**seçin **Palo Alto Networks - GlobalProtect** sonucu panelinden ardından **Ekle** uygulama eklemek için .

    ![Palo Alto Networks tarafından sağlanan-GlobalProtect sonuç listesinde](./media/paloaltoglobalprotect-tutorial/tutorial_paloaltoglobal_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Palo Alto Networks ile test etme - GlobalProtect "Britta Simon" adlı bir test kullanıcı tabanlı.

Tek iş için oturum açma için Azure AD ne Palo Alto Networks - GlobalProtect karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Palo Alto Networks - ilgili kullanıcı arasında bir bağlantı ilişki GlobalProtect kurulması gerekir.

Palo Alto Networks - GlobalProtect, değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma ile Palo Alto Networks - test etme GlobalProtect, aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Palo Alto Networks - GlobalProtect test kullanıcısı oluşturma](#create-a-palo-alto-networks---globalprotect-test-user)**  - Palo Alto Networks - kullanıcı Azure AD gösterimini bağlı GlobalProtect Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, Palo Alto Networks - GlobalProtect uygulama yapılandırın.

**Palo Alto Networks - GlobalProtect, Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Palo Alto Networks - GlobalProtect** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/paloaltoglobalprotect-tutorial/tutorial_paloaltoglobal_samlbase.png)

3. Üzerinde **Palo Alto Networks - GlobalProtect etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Palo Alto Networks tarafından sağlanan-GlobalProtect etki alanı ve URL'ler tek bilgi'oturum açma](./media/paloaltoglobalprotect-tutorial/tutorial_paloaltoglobal_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<Customer Firewall URL>`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<Customer Firewall URL>/SAML20/SP`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [Palo Alto Networks - GlobalProtect istemci Destek ekibine](https://support.paloaltonetworks.com/support) bu değerleri almak için. 
 
4. Palo Alto Networks tarafından sağlanan - GlobalProtect uygulama belirli bir biçimde SAML onaylamalarını bekler. Lütfen bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirme sayfasında bölümü. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir.
    
    ![Çoklu oturum açmayı yapılandırın](./media/paloaltoglobalprotect-tutorial/tutorial_paloaltoglobal_attribute.png)
    
5. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | --- | --- |    
    | kullanıcı adı | User.userPrincipalName |

    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/paloaltoglobalprotect-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/paloaltoglobalprotect-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın. Biz user.userprincipalname örnek ile değer eşlediğiniz ancak, uygun bir değer ile eşleyebilirsiniz. 
    
    d. Tıklayın **Tamam**


6. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/paloaltoglobalprotect-tutorial/tutorial_paloaltoglobal_certificate.png) 

7. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/paloaltoglobalprotect-tutorial/tutorial_general_400.png)

8. Palo Alto ağ güvenlik duvarı yönetici kullanıcı Arabirimi, başka bir tarayıcı penceresinde bir yönetici olarak açın.

9. Tıklayarak **cihaz**.

    ![Palo Alto çoklu oturum açmayı yapılandırın](./media/paloaltoglobalprotect-tutorial/tutorial_paloaltoadmin_admin1.png)

10. Seçin **SAML kimlik sağlayıcısı** sol gezinti bölmesinden çubuk ve meta veri dosyası içeri aktarmak için "Al" seçeneğini tıklatın.

    ![Palo Alto çoklu oturum açmayı yapılandırın](./media/paloaltoglobalprotect-tutorial/tutorial_paloaltoadmin_admin2.png)

11. Aşağıdaki içeri aktarma penceresini Eylemler gerçekleştirin

    ![Palo Alto çoklu oturum açmayı yapılandırın](./media/paloaltoglobalprotect-tutorial/tutorial_paloaltoadmin_admin3.png)

    a. İçinde **profil adı** metin bir adı ör. Azure AD GlobalProtect sağlayın.
    
    b. İçinde **kimlik sağlayıcısı meta verileri**, tıklayın **Gözat** ve Azure portalından indirdiğiniz metadata.xml dosyası seçin
    
    c. **Tamam**’a tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/paloaltoglobalprotect-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/paloaltoglobalprotect-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/paloaltoglobalprotect-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/paloaltoglobalprotect-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-palo-alto-networks---globalprotect-test-user"></a>Palo Alto Networks - GlobalProtect test kullanıcısı oluşturma

Palo Alto ağları - GlobalProtect zaten seçili değilse kullanıcı otomatik olarak sistemde başarılı kimlik doğrulamasından sonra oluşturulacak şekilde kullanıcı sağlamayı destekler Just-ın-time bulunmaktadır. Burada herhangi bir eylem gerçekleştirmeniz gerekmez. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma Palo Alto Networks - GlobalProtect erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Palo Alto Networks - GlobalProtect, Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Palo Alto Networks - GlobalProtect**.

    ![-GlobalProtect bağlantı uygulamalar listesinde Palo Alto ağlar](./media/paloaltoglobalprotect-tutorial/tutorial_paloaltoglobal_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Palo Alto Networks - erişim Paneli'nde GlobalProtect kutucuğa tıkladığınızda, otomatik olarak imzalanmış, Palo Alto Networks - GlobalProtect uygulama için açma.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/paloaltoglobalprotect-tutorial/tutorial_general_01.png
[2]: ./media/paloaltoglobalprotect-tutorial/tutorial_general_02.png
[3]: ./media/paloaltoglobalprotect-tutorial/tutorial_general_03.png
[4]: ./media/paloaltoglobalprotect-tutorial/tutorial_general_04.png

[100]: ./media/paloaltoglobalprotect-tutorial/tutorial_general_100.png

[200]: ./media/paloaltoglobalprotect-tutorial/tutorial_general_200.png
[201]: ./media/paloaltoglobalprotect-tutorial/tutorial_general_201.png
[202]: ./media/paloaltoglobalprotect-tutorial/tutorial_general_202.png
[203]: ./media/paloaltoglobalprotect-tutorial/tutorial_general_203.png

