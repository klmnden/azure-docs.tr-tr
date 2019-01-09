---
title: Öğretici - hizmet olarak doğrulama için kaynakları kümesine | Microsoft Docs
description: Bu öğreticide, bir hizmet olarak doğrulama için kaynaklarını ayarlama konusunda bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/26/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 58c69840e7b8ebdc2bd367859184ab54460c9426
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54103491"
---
# <a name="tutorial-set-up-resources-for-validation-as-a-service"></a>Öğretici: Hizmet olarak doğrulama için kaynaklarını ayarlama

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Bir çözüm oluşturmanız gerekir. Bir doğrulama hizmeti (VaaS) çözümü olarak Azure Stack çözümünü belirli donanım ürün reçetesi ile temsil eder. Çözüm çalıştırma Azure Stack, donanım destekleyip desteklemediğini kontrol etmek için kullanır. Hizmet ile çözümünüzü kullanmaya hazır hale getirmek için bu öğreticiden yararlanın.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure AD'yi ayarlarken ayarlayarak VaaS kullanmaya hazırlanmanıza (Azure AD) örneği.
> * Depolama hesabı oluşturma.

## <a name="configure-an-azure-ad-tenant"></a>Azure AD kiracısı yapılandırın

Azure AD kiracısı ile VaaS kaydetme ve kimlik doğrulaması için gereklidir. Kiracı rol tabanlı erişim denetimi (RBAC) iş ortağı tarafından kimlerin ortağı kuruluşundaki VaaS kullanabileceğini yönetmek için kullanılır.

Kuruluş, Azure AD'ye kaydetmeniz Kiracı directory (Azure Stack için kullanılan Azure AD Kiracı dizinini yerine) ve kullanıcı hesaplarını yönetmek için bir ilke oluşturun. Daha fazla bilgi için [Azure AD dizininizi yönetme](https://docs.microsoft.com/azure/active-directory/active-directory-administer).

### <a name="create-a-tenant"></a>Kiracı oluşturma

Örneğin, açıklayıcı bir ad ile kullanılmak üzere özel olarak bir kiracı ile VaaS oluşturma `ContosoVaaS@onmicrosoft.com`.

1. Bir Azure AD kiracısında oluşturma [Azure portalında](https://portal.azure.com), ya da mevcut bir kiracıyı kullanın. <!-- For instructions on creating new Azure AD tenants, see [Get started with Azure AD](https://docs.microsoft.com/azure/active-directory/get-started-azure-ad). -->

2. Kuruluşunuzun üyeleri kiracıya ekleyin. Bu kullanıcılar testleri zamanlayın veya görüntülemek için hizmeti kullanmak için sorumlu olursunuz. Kayıt işlemini tamamladıktan sonra kullanıcıların erişim düzeylerini tanımlayacaksınız.
 
    Aşağıdaki rollerden biri atayarak Eylemler VaaS içinde çalıştırmak için kiracınızdaki kullanıcılar izin verirsiniz:

    | Rol Adı | Açıklama |
    |---------------------|------------------------------------------|
    | Sahip | Tüm kaynaklar için tam erişimi vardır. |
    | Okuyucu | Tüm kaynaklarını görüntüleyebilir ancak değil oluşturabilir ve bunları yönetebilirsiniz. |
    | Test katkıda bulunan | Oluşturabilir ve test kaynaklarını yönetin. |

    Rolleri atamak için **Azure Stack doğrulama hizmeti** uygulama:

    1. [Azure Portal](https://portal.azure.com) oturum açın.
    2. Seçin **tüm hizmetleri** > **Azure Active Directory** altında **kimlik** bölümü.
    3. Seçin **kurumsal uygulamalar** > **Azure Stack doğrulama hizmeti** uygulama.
    4. **Kullanıcı ve gruplar**'ı seçin. **Azure Stack doğrulama hizmeti - kullanıcı ve grup** dikey penceresinde uygulamayı kullanmak için izne sahip kullanıcılar listelenir.
    5. Seçin **+ Ekle kullanıcı** kiracınızdan bir kullanıcı ekleyin ve bir rol atayın.
   
    VaaS kaynaklara ve işlemlere bir kuruluştaki farklı gruplar arasında yalıtmak istiyorsanız, birden çok Azure AD Kiracı dizin oluşturabilirsiniz.

### <a name="register-your-tenant"></a>Kiracınızı kaydetme

Bu işlem, kiracınız ile yetkilendirir **Azure Stack doğrulama hizmeti** Azure AD uygulaması.

1. Microsoft'ta Kiracı hakkında aşağıdaki bilgileri gönderin [ vaashelp@microsoft.com ](mailto:vaashelp@microsoft.com).

    | Veriler | Açıklama |
    |--------------------------------|---------------------------------------------------------------------------------------------|
    | Kuruluş Adı | Resmi kuruluş adı. |
    | Azure AD Kiracısı dizin adı | Kayıtlı Azure AD Kiracısı dizin adı. |
    | Azure AD Kiracı dizin kimliği | Azure AD Kiracısı Directory dizini ile ilişkili GUID. Azure AD Kiracı dizin Kimliğinizi bulma konusunda daha fazla bilgi için bkz: [Kiracı kimliği alma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#get-tenant-id). |

2. Kiracınızı VaaS portalı kullanabilirsiniz denetlemek için Azure Stack doğrulama Ekibi'nden gelen onay için bekleyin.

### <a name="consent-to-the-vaas-application"></a>VaaS uygulama onayı

Azure AD Yöneticisi VaaS Azure AD uygulama kiracınızın adına gerekli izinleri verin:

1. Oturum açmak için bir kiracı için genel yönetici kimlik bilgilerini kullanmak [VaaS portalı](https://azurestackvalidation.com/). 

2. Seçin **Hesabımı**.

3 VaaS listelenen Azure AD izinleri vermek için istendiğinde devam etmek için koşullarını kabul edin.

## <a name="create-an-azure-storage-account"></a>Azure Depolama hesabı oluşturma

Test yürütmesi sırasında bir Azure depolama hesabına tanılama günlükleri VaaS çıkarır. Test günlüklerine ek olarak, depolama hesabı da karşıya yükleme OEM uzantı paketleri için çözüm doğrulama iş akışı için kullanılabilir.

Azure depolama hesabı, Azure Stack ortamınıza değil, Azure genel bulutunda barındırılır.

1. Azure portalında **tüm hizmetleri** > **depolama** > **depolama hesapları**. Üzerinde **depolama hesapları** dikey penceresinde **Ekle**.

2. Depolama hesabının oluşturulacağı aboneliği seçin.

3. Altında **kaynak grubu**seçin **Yeni Oluştur**. Yeni kaynak grubunuz için bir ad girin.

4. Depolama hesabınız için bir ad girin. Seçtiğiniz bir adı olması gerekir:
    - Azure genelinde benzersiz
    - 3 ila 24 karakter uzunluğunda
    - Yalnızca sayılar ve küçük harfler içerebilir

5. Seçin **ABD Batı** depolama hesabınız için bölge.

    Günlükleri depolamak için ağ hizmeti ücretleri karşılanmaz emin olmak için Azure depolama hesabını sadece kullanmak üzere yapılandırılabilir **ABD Batı** bölge. Veri çoğaltma ve sık erişimli depolama katmanı özelliği bu veri için gerekli değildir. Ya da özelliğin etkinleştirilmesi, maliyetlerinizi önemli ölçüde artırır.

6. Ayarları varsayılan değerlerine dışında bırakın **hesap türü**:

    - **Dağıtım modeli** ayarlanmış **Resource Manager** varsayılan olarak.
    - **Performans** alanı varsayılan olarak **Standart**’a ayarlanır.
    - Seçin **hesap türü** olarak alan **Blob Depolama**.
    - **Çoğaltma alanı** ayarlanır **yerel olarak yedekli depolama (LRS)** varsayılan olarak.
    - **Erişim katmanı** varsayılan olarak **Sık erişim**’e ayarlanır.

7. Depolama hesabı ayarlarınızı gözden geçirmek ve hesabı oluşturmak için **Gözden Geçir + Oluştur**’a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Ortamınıza bağlı olarak bağlantılara izin vermez, donanımınıza bir testi çalıştırmak için yerel aracı dağıtma hakkında öğreticiyi izleyebilirsiniz.

> [!div class="nextstepaction"]
> [Yerel aracı dağıtma](azure-stack-vaas-local-agent.md)
