---
title: Yönetim Aracı - Azure'ı dağıtma
description: Windows sanal masaüstü Önizleme kaynakları yönetmek için bir kullanıcı arabirimi aracını yükleme.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 06/04/2019
ms.author: v-chjenk
ms.openlocfilehash: 4db9e6eaf2d7f7630d3d412d5519d97f8beca3ad
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67272829"
---
# <a name="tutorial-deploy-a-management-tool"></a>Öğretici: Bir yönetim aracı dağıtma

Yönetim Aracı, Microsoft sanal masaüstü Önizleme kaynakları yönetmek için bir kullanıcı arabirimi (UI) sağlar. Bu öğreticide, dağıtım ve yönetim aracına bağlanın öğreneceksiniz.

>[!NOTE]
>Kuruluşunuzun mevcut işlemleri ile kullanılabilecek bir Windows sanal masaüstü Önizleme özgü yapılandırma için bu yönergeleri yöneliktir.

## <a name="important-considerations"></a>Önemli noktalar

Uygulama onayı gerektirdiğinden Windows sanal masaüstü ile etkileşimde bulunmak üzere bu aracı, işletmeden işletmeye (B2B) senaryolarını desteklemez. Kendi ayrı bir dağıtım yönetim aracının her bir Azure Active Directory (AAD) kiracının aboneliğine ihtiyacınız olacaktır.

Bir örnek bu yönetim aracıdır. Microsoft önemli güvenlik ve kalite güncelleştirmeleri sağlar. [Kaynak kodu github'da bulunan](https://github.com/Azure/RDS-Templates/tree/master/wvd-templates/wvd-management-ux/deploy). Müşteriler ve iş ortakları, aracı kendi iş gereksinimlerinize uygun olarak özelleştirmek için önerilir.

## <a name="what-you-need-to-run-the-azure-resource-manager-template"></a>Azure Resource Manager şablonu çalıştırmak için ihtiyacınız olanlar

Azure Resource Manager şablonu dağıtmadan önce bir Azure Active Directory kullanıcı yönetimi kullanıcı Arabirimi dağıtmak için gerekir. Bu kullanıcı gerekir:

- Azure multi-Factor Authentication (MFA) devre dışı
- Azure aboneliğinizdeki kaynakları oluşturmak için izne sahip
- Azure AD uygulaması oluşturmak için izne sahip. Kullanıcı olup olmadığını denetlemek için bu adımları [gerekli izinler](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#required-permissions).

Azure Resource Manager şablonu dağıttıktan sonra doğrulamak için kullanıcı Arabirimi yönetim başlatmak isteyebilirsiniz. Bu kullanıcı gerekir:
- Görüntülemek veya Windows sanal masaüstü kiracınız düzenlemek için bir rol atamasına sahip

## <a name="run-the-azure-resource-manager-template-to-provision-the-management-ui"></a>Kullanıcı Arabirimi yönetim sağlamak için Azure Resource Manager şablonu çalıştırın

Başlamadan önce sunucu ve istemci uygulamaları onay ziyaret ederek sahip olun [Windows sanal masaüstü onay sayfası](https://rdweb.wvd.microsoft.com) için Azure Active Directory (temsil AAD).

Azure kaynak yönetimi şablonu dağıtmak için aşağıdaki yönergeleri izleyin:

1. Git [RDS-Templates GitHub Azure sayfasını](https://github.com/Azure/RDS-Templates/tree/master/wvd-templates/wvd-management-ux/deploy).
2. Şablonu Azure'a dağıtın.
    - Bir kurumsal aboneliğinde dağıtıyorsanız, aşağıya kaydırın ve **azure'a Dağıt**. Bkz: [şablon parametreleri için yönergeler](#guidance-for-template-parameters).
    - Bir bulut çözümü sağlayıcısı abonelikte dağıtıyorsanız, Azure'a dağıtmak için bu yönergeleri izleyin:
        1. Ekranı aşağı kaydırın ve sağ **azure'a Dağıt**, ardından **kopya bağlantı konumu**.
        2. Not Defteri gibi bir metin düzenleyicisinde açın ve bağlantı var. yapıştırın.
        3. Hemen sonra <https://portal.azure.com/> ve diyez etiketi önce (#), girin bir at işareti (@) ve Kiracı etki alanı adından. İşte bir örnek biçim: <https://portal.azure.com/@Contoso.onmicrosoft.com#create/>.
        4. Azure portalında bir bulut çözümü sağlayıcısı aboneliğe yönetici/katkıda bulunan izinlerine sahip bir kullanıcı olarak oturum açın.
        5. Adres çubuğuna metin düzenleyiciye kopyaladığınız bağlantı yapıştırın.

### <a name="guidance-for-template-parameters"></a>Şablon parametreleri için yönergeler
Aracı'nı yapılandırmak için parametreleri girmek üzere nasıl aşağıda verilmiştir:

- Bu, RD Aracısı URL kullanılır:  <https://rdbroker.wvd.microsoft.com/>
- Bu kaynak URL'si.  <https://mrs-prod.ame.gbl/mrs-RDInfra-prod>
- Azure'da oturum açmanız için devre dışı MFA ile AAD kimlik bilgilerinizi kullanın. Bkz: [Azure Resource Manager şablonu çalıştırmak için ihtiyacınız olanları](#what-you-need-to-run-the-azure-resource-manager-template).
- Azure Active Directory'de yönetim aracı için kayıtlı uygulama için benzersiz bir ad kullanın. Örneğin, Apr3UX.

## <a name="provide-consent-for-the-management-tool"></a>Onay için Yönetim Aracı'nı sağlayın.

GitHub Azure kaynak şablonu tamamlandıktan Yöneticisi sonra iki uygulama hizmetleri, Azure portalında bir app service planı ile birlikte içeren bir kaynak grubu bulabilirsiniz.

Oturum ve yönetim aracını önce yönetim aracı ile ilişkili olan yeni Azure Active Directory uygulama için onay vermeniz gerekir. İzin vererek, araca oturum açan kullanıcı adına Windows sanal masaüstü yönetimi çağrıları yapmak yönetim aracı vermiş olursunuz.

Hangi kullanıcı belirlemek için kullanabileceğiniz Aracı'nın oturum açma, Git, [Azure Active Directory kullanıcı ayarları sayfası](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/UserSettings/menuId/) ve değeri Not **kullanıcılar uygulamalara kendileri adına şirket verilerine erişme izni verebilir**.

- Değer ayarlanmışsa **Evet**, Azure Active Directory'de herhangi bir kullanıcı hesabı ile oturum açma ve yalnızca o kullanıcı için onay sağlayın. Ancak, yönetim aracı için farklı bir kullanıcı ile daha sonra oturum açıldığında, aynı onayı yeniden gerçekleştirmeniz gerekir.
- Değer ayarlanmışsa **Hayır**, gerekir Azure Active Directory'de genel yönetici ile oturum açın ve dizindeki tüm kullanıcılar için yönetici onayı sağlar. Olursunuz 


Onay sağlamak için kullanacağınız kullanıcı karar verdiğinizde, onay aracı sağlamak için bu yönergeleri izleyin:

1. Azure kaynaklarınızı gidin, sağladığınız ada sahip Azure App Services kaynak şablonu (örneğin, Apr3UX) seçin ve ilişkili URL'ye gidin; Örneğin, <https://rdmimgmtweb-210520190304.azurewebsites.net>.
2. Uygun Azure Active Directory kullanıcı hesabı kullanarak oturum açın.
3. Artık bir genel yönetici ile kimlik doğrulaması gerekiyorsa, onay kutusunu seçebilirsiniz **onay kuruluşunuz adına**. Seçin **kabul** rıza sağlamanın.

Bu artık, yönetim aracı için yönlendirir.

## <a name="use-the-management-tool"></a>Yönetim aracını kullanma

Kuruluş için veya belirli bir kullanıcı için onay girdikten sonra herhangi bir zamanda yönetim aracına erişebilirsiniz.

Aracı başlatmak için bu yönergeleri izleyin:

1. Belirttiğiniz ad ile Azure App Services kaynak şablonu (örneğin, Apr3UX) seçin ve onunla ilişkili URL'ye gidin; Örneğin, <https://rdmimgmtweb-210520190304.azurewebsites.net>.
2. Windows sanal masaüstü kimlik bilgilerinizi kullanarak oturum açın.
3. Bir kiracı grubu seçin. sorulduğunda **varsayılan Kiracı grubu** aşağı açılan listeden.

> [!NOTE]
> Özel bir kiracı grubu varsa, aşağı açılan listeden seçerek yerine el ile adını girin.

## <a name="next-steps"></a>Sonraki adımlar

Dağıtın ve yönetim aracına bağlanın öğrendiniz, Azure hizmet durumu hizmeti sorunları ve sistem durumu danışmanları izlemek için nasıl kullanılacağını öğrenebilirsiniz.

> [!div class="nextstepaction"]
> [Hizmet uyarılarını öğreticiyi ayarlayın](./set-up-service-alerts.md)
