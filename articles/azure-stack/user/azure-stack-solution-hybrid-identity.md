---
title: Hibrit bulut kimliği ile Azure'da ve Azure Stack'te uygulama yapılandırma | Microsoft Docs
description: Azure ve Azure Stack uygulamaları ile karma bulut kimlik yapılandırmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: bed67c0213ed5715b8b3d8fd393d8d856e0ea15b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46964999"
---
# <a name="tutorial-configure-hybrid-cloud-identity-for-azure-and-azure-stack-applications"></a>Öğretici: Azure ve Azure Stack uygulamalar için hibrit bulut kimliği yapılandırma

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bir karma bulut kimliği, Azure ve Azure Stack uygulamalarınız için yapılandırmayı öğrenin.

Uygulamalarınızın genel Azure hem de Azure Stack için erişim izni verme için iki seçeneğiniz vardır.

 * Azure Stack sürekli bir Internet bağlantısı varsa, Azure Active Directory (Azure AD) kullanabilirsiniz.
 * Azure Stack, Internet bağlantısı kesildiğinde, Azure Directory Federasyon Hizmetleri'nde (AD FS) kullanabilirsiniz.

Hizmet sorumluları, Azure Stack uygulamalarınızın dağıtım veya Azure Stack'te Azure Resource Manager kullanarak yapılandırma amacıyla erişim vermek için kullanın.

Bu öğreticide, bir örnek ortama oluşturacaksınız:

> [!div class="checklist"]
> - Genel Azure ve Azure Stack'te karma kimlik oluşturma
> - Azure Stack API'sine erişmek için bir belirteç alır.

Bu öğreticideki adımlar için Azure Stack operatör izinleri olmalıdır.

> [!Tip]  
> ![karma pillars.png](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack, Azure'nın bir uzantısıdır. Azure Stack çevikliğini ve yenilik bulut bilgi işlem, şirket içi ortamınıza ve hibrit uygulamaları her yerde oluşturup dağıtmayı olanak tanıyan tek hibrit Bulutu sunar.  
> 
> Teknik incelemeyi [karma uygulamaları için tasarım konuları](https://aka.ms/hybrid-cloud-applications-pillars) tasarlama, dağıtma ve karma işletim (yerleştirme, ölçeklenebilirlik, kullanılabilirlik, dayanıklılık, yönetilebilirlik ve güvenlik) yazılım kalitesinin yapı taşları gözden geçirmeleri uygulamalar. Tasarım konuları, üretim ortamlarında sorunlarını en aza karma uygulama tasarımının en iyi duruma getirme yardımcı olur.


## <a name="create-a-service-principal-for-azure-ad-in-the-portal"></a>Azure AD portalında için hizmet sorumlusu oluşturma

Azure Stack kimlik deposu olarak Azure AD'yi kullanarak dağıttıysanız, Azure için gibi hizmet sorumluları oluşturabilirsiniz. [Hizmet sorumluları oluşturma](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-create-service-principals#create-service-principal-for-azure-ad) makalede portal üzerinden adımların nasıl gerçekleştirileceğini gösterir. Sahip olduğunuzu denetleyin [Azure AD izinleri gerekli](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#required-permissions) başlamadan önce.

## <a name="create-a-service-principal-for-ad-fs-using-powershell"></a>PowerShell kullanarak AD FS için hizmet sorumlusu oluşturma

AD FS ile Azure Stack dağıttıysanız, hizmet sorumlusu oluşturma, erişim için rol atama ve o kimlik kullanarak PowerShell üzerinden oturum için PowerShell kullanabilirsiniz. [AD FS için hizmet sorumlusu oluşturma](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-create-service-principals#create-service-principal-for-ad-fs) PowerShell kullanarak gerekli adımların nasıl gerçekleştirileceğini gösterir.

## <a name="using-the-azure-stack-api"></a>Azure yığını API'si kullanma

[Azure Stack API](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-rest-api-use) öğretici Azure Stack API'ye erişmek için bir belirteç alma sürecinde açıklar.

## <a name="connect-to-azure-stack-using-powershell"></a>Powershell kullanarak Azure Stack'e bağlanma

Bu hızlı başlangıçta [PowerShell'de Azure Stack ile başlamak için](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-configure-quickstart) , Azure PowerShell'i yükleme ve Azure Stack yüklemenizi bağlanmak için gereken adımlar anlatılmaktadır.

### <a name="prerequisites"></a>Önkoşullar

Azure Stack yükleme erişebileceğiniz bir abonelikle Azure Active Directory'ye bağlı. Azure Stack yükleme yoksa, bu yönergeleri ayarlamak için kullanabileceğiniz bir [Azure Stack geliştirme Seti'ni](https://docs.microsoft.com/azure/azure-stack/asdk/asdk-deploy).

#### <a name="connect-to-azure-stack-using-code"></a>Kod kullanarak Azure Stack'e bağlanma

Azure Stack'e kod kullanarak bağlanmak için graf uç noktaları ve kimlik doğrulaması için Azure Stack yüklemenizi almak ve REST istekleri kullanılarak kimlik doğrulaması yapmak için Azure Resource Manager Uç noktalara API'sini kullanın. Örnek istemci uygulaması bulabilirsiniz [GitHub](https://github.com/shriramnat/HybridARMApplication).

>[!Note]
>İstediğiniz dilde için Azure SDK'sı Azure API profillerini desteklemedikçe SDK'sı, Azure Stack ile çalışmayabilir. Azure API profilleri hakkında daha fazla bilgi için bkz: [API sürümü profillerini yönetme](https://docs.microsoft.com/da-dk/azure/azure-stack/user/azure-stack-version-profiles) makalesi.

## <a name="next-steps"></a>Sonraki adımlar

 - Azure Stack'te kimlik nasıl işlendiğini hakkında daha fazla bilgi için bkz: [Azure Stack için kimlik mimarisi](https://docs.microsoft.com/azure/azure-stack/azure-stack-identity-architecture).
 - Azure bulut desenleri hakkında daha fazla bilgi için bkz: [bulut tasarımı desenleri](https://docs.microsoft.com/azure/architecture/patterns).
