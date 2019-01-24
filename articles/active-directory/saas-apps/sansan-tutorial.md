---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Sansan | Microsoft Docs'
description: Azure Active Directory ve Sansan arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: f653a0f2-c44a-4670-b936-68c136b578ea
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: 6bd84e42bf46885a9dced363724538cbd4d2066a
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54815013"
---
# <a name="tutorial-azure-active-directory-integration-with-sansan"></a>Öğretici: Sansan ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Sansan tümleştirme konusunda bilgi edinin.

Azure AD ile Sansan tümleştirme ile aşağıdaki avantajları sağlar:

- Sansan erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Sansan (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Sansan yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Sansan çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Sansan ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-sansan-from-the-gallery"></a>Galeriden Sansan ekleme
Azure AD'de Sansan tümleştirmesini yapılandırmak için Sansan Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Sansan eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Sansan**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/sansan-tutorial/tutorial_sansan_search.png)

5. Sonuçlar panelinde seçin **Sansan**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/sansan-tutorial/tutorial_sansan_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Sansan sınayın.

Tek iş için oturum açma için Azure AD ne Sansan karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Sansan ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Sansan içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Sansan ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Sansan test kullanıcısı oluşturma](#creating-a-sansan-test-user)**  - kullanıcı Azure AD gösterimini bağlı Sansan Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Sansan uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Sansan yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Sansan** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/sansan-tutorial/tutorial_sansan_samlbase.png)

3. Üzerinde **Sansan etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/sansan-tutorial/tutorial_sansan_url.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL kullanarak aşağıdaki düzenler: 
    
    | Ortam | URL'si |
    |:--- |:--- |
    | PC web |`https://ap.sansan.com/v/saml2/<company name>/acs` |
    | Yerel mobil uygulama |`https://internal.api.sansan.com/saml2/<company name>/acs` |
    | Mobil tarayıcı ayarları |`https://ap.sansan.com/s/saml2/<company name>/acs` |  

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Sansan istemci Destek ekibine](https://www.sansan.com/form/contact) bu değerleri almak için. 
     
4. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/sansan-tutorial/tutorial_sansan_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/sansan-tutorial/tutorial_general_400.png)

6. Birden çok Sansan uygulama bekliyor **tanımlayıcıları** ve **yanıt URL'leri** PowerShell kullanarak yapılandırılabilecek birden çok ortama (PC web, yerel mobil uygulaması, mobil tarayıcı ayarlarını) desteklemek için komut dosyası. Ayrıntılı adımlar aşağıda açıklanmıştır.

7. Birden çok yapılandırma **tanımlayıcıları** ve **yanıt URL'leri** PowerShell betiğini kullanarak Sansan uygulama için aşağıdaki adımları uygulayın:

    ![Obj çoklu oturum açmayı yapılandırın](./media/sansan-tutorial/tutorial_sansan_objid.png)  

    a. Git **özellikleri** sayfasının **Sansan** uygulama ve kopyalama **nesne kimliği** kullanarak **kopyalama** düğmesi ve Not Defteri'ne yapıştırın.

    b. **Nesne kimliği**, hangi Azure Portalı'ndan kopyaladığınız olarak kullanılacak **ServicePrincipalObjectId** PowerShell betiğinde öğreticinin ilerleyen bölümlerinde kullanılır. 

    c. Artık yükseltilmiş bir Windows PowerShell komut istemi açın.
    
    >[!NOTE] 
    > AzureAD modülüne yüklemeniz gerekir (komutunu `Install-Module -Name AzureAD`). Bir NuGet modül veya yeni Azure Active Directory V2 PowerShell modülünü yüklemeniz istenirse, Y yazın ve ENTER tuşuna basın.

    d. Çalıştırma `Connect-AzureAD` ve bir genel yönetici kullanıcı hesabıyla oturum açın.

    e. Birden fazla URL bir uygulamayı güncelleştirmek için aşağıdaki betiği kullanın:

    ```poweshell
     Param(
    [Parameter(Mandatory=$true)][guid]$ServicePrincipalObjectId,
    [Parameter(Mandatory=$false)][string[]]$ReplyUrls,
    [Parameter(Mandatory=$false)][string[]]$IdentifierUrls
    )

    $servicePrincipal = Get-AzureADServicePrincipal -ObjectId $ServicePrincipalObjectId

    if($ReplyUrls.Length)
    {
    echo "Updating Reply urls"
    Set-AzureADServicePrincipal -ObjectId $ServicePrincipalObjectId -ReplyUrls $ReplyUrls
    echo "updated"
    }
    if($IdentifierUrls.Length)
    {
    echo "Updating Identifier urls"
    $applications = Get-AzureADApplication -SearchString $servicePrincipal.AppDisplayName 
    echo "Found Applications =" $applications.Length
    $i = 0;
    do
    {  
    $application = $applications[$i];
    if($application.AppId -eq $servicePrincipal.AppId){
    Set-AzureADApplication -ObjectId $application.ObjectId -IdentifierUris $IdentifierUrls
    $servicePrincipal = Get-AzureADServicePrincipal -ObjectId $ServicePrincipalObjectId
    echo "Updated"
    return;
    }
    $i++;
    }while($i -lt $applications.Length);
    echo "Not able to find the matched application with this service principal"
    }
    ```

8. Başarılı tamamlama PowerShell betiğinin sonra aşağıda gösterildiği gibi komut dosyası sonucu şöyle olacaktır ve URL değerlerin güncelleştirilmesi ancak bunlar Azure Portalı'nda yansıtılmış olmaz. 

    ![Çoklu oturum açma betiğini Yapılandır](./media/sansan-tutorial/tutorial_sansan_powershell.png)


9. Üzerinde **Sansan yapılandırma** bölümünde **yapılandırma Sansan** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/sansan-tutorial/tutorial_sansan_configure.png) 

10. Çoklu oturum açmayı yapılandırma **Sansan** tarafı, indirilen göndermek için ihtiyacınız **sertifika**, **oturum kapatma URL'si**, **SAML varlık kimliği**, ve **SAML çoklu oturum açma hizmeti URL'si** için [Sansan Destek ekibine](https://www.sansan.com/form/contact). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

>[!NOTE]
>Bilgisayar Tarayıcı ayarlarınızı da iş için mobil uygulama ve PC web yanı sıra mobil tarayıcı. 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/sansan-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/sansan-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/sansan-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/sansan-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-sansan-test-user"></a>Sansan test kullanıcısı oluşturma

Bu bölümde, Britta Simon Sansan içinde adlı bir kullanıcı oluşturun. Sansan uygulamanın kullanıcının uygulamada SSO yapmadan önce sağlanması gerekir. 

>[!NOTE]
>Bir kullanıcı el ile oluşturabilir veya toplu gerekiyorsa kullanıcılar, iletişime geçmeniz [Sansan Destek ekibine](https://www.sansan.com/form/contact). 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Sansan erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Sansan için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Sansan**.

    ![Çoklu oturum açmayı yapılandırın](./media/sansan-tutorial/tutorial_sansan_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Sansan kutucuğa tıkladığınızda, otomatik olarak Sansan uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/sansan-tutorial/tutorial_general_01.png
[2]: ./media/sansan-tutorial/tutorial_general_02.png
[3]: ./media/sansan-tutorial/tutorial_general_03.png
[4]: ./media/sansan-tutorial/tutorial_general_04.png

[100]: ./media/sansan-tutorial/tutorial_general_100.png

[200]: ./media/sansan-tutorial/tutorial_general_200.png
[201]: ./media/sansan-tutorial/tutorial_general_201.png
[202]: ./media/sansan-tutorial/tutorial_general_202.png
[203]: ./media/sansan-tutorial/tutorial_general_203.png

