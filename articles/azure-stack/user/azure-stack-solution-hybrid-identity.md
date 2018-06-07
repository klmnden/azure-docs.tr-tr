---
title: Karma bulut kimliği ile Azure ve Azure yığın uygulamalarını yapılandırmak | Microsoft Docs
description: Azure ve Azure yığın uygulamaları ile karma bulut kimlik yapılandırmayı öğrenin.
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
ms.date: 05/22/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: a57afb4a90da5877879afddc35545e0bfef622a7
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34808171"
---
# <a name="tutorial-configure-hybrid-cloud-identity-for-azure-and-azure-stack-applications"></a>Öğretici: karma bulut kimliği Azure ve Azure yığın uygulamaları için yapılandırma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Karma bulut kimliği, Azure ve Azure yığın uygulamalarınız için yapılandırmayı öğrenin.

Genel Azure ve Azure yığını uygulamalarınızda erişim verilmesi için iki seçeneğiniz vardır.

 * Azure yığın sürekli bir Internet bağlantısına sahip olduğunda, Azure Active Directory (Azure AD) kullanabilirsiniz.
 * Azure yığın Internet bağlantısı kesildiğinde, Azure Directory Federasyon Hizmetleri (AD FS) kullanabilirsiniz.

Dağıtım veya Azure yığınında Azure Kaynak Yöneticisi'ni kullanarak yapılandırma amacıyla Azure yığın uygulamalarınıza erişim vermek için hizmet asıl adı kullanın.

Bu öğreticide, bir örnek ortamı için yapı:

> [!div class="checklist"]
> * Karma kimliğini genel Azure ve Azure yığın oluşturma
> * Azure yığını API'si erişmek için bir belirteç alır.

Bu öğreticide adımları için Azure yığın işleci izinleri olmalıdır.

## <a name="create-a-service-principal-for-azure-ad-in-the-portal"></a>Azure AD portalı için bir hizmet sorumlusu oluşturma

Azure AD kimlik deposu olarak kullanarak Azure yığın dağıttıktan sonra Azure için gibi hizmet asıl adı oluşturabilirsiniz. [Hizmet sorumluları oluşturmak](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-create-service-principals#create-service-principal-for-azure-ad) makale portal üzerinden adımların nasıl gerçekleştirileceğini gösterir. Sahip olduğunuz denetleyin [gereken Azure AD izinler](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#required-permissions) başlamadan önce.

## <a name="create-a-service-principal-for-ad-fs-using-powershell"></a>PowerShell kullanarak AD FS için hizmet sorumlusu oluşturma

AD FS ile Azure yığın dağıttıysanız, bir hizmet sorumlusu oluşturma, erişim için bir rol atayın ve Powershell'den bu kimliğini kullanarak oturum açmak için PowerShell'i kullanabilirsiniz. [AD FS için hizmet sorumlusu oluşturma](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-create-service-principals#create-service-principal-for-ad-fs) PowerShell kullanarak gerekli adımların nasıl gerçekleştirileceğini gösterir.

## <a name="using-the-azure-stack-api"></a>Azure yığını API'si kullanma

[Azure yığını API'si](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-rest-api-use) öğreticide Azure yığını API'si erişmek için bir belirteç alma işlemi açıklanmaktadır.

## <a name="connect-to-azure-stack-using-powershell"></a>Azure Powershell kullanarak yığınına Bağlan

Hızlı Başlangıç [Azure yığınında PowerShell ile başlamak ve çalıştırmak için](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-configure-quickstart) Azure PowerShell'i yüklemek ve Azure yığın yüklemenizi bağlanmak için gereken adımlarda size yol gösterir.

### <a name="prerequisites"></a>Önkoşullar

Bir Azure yığın yükleme Azure Active Directory'ye erişmek için bir aboneliğe bağlı. Bir Azure yığın yükleme yoksa, bu yönergeleri ayarlamak için kullanabileceğiniz bir [Azure yığın Geliştirme Seti](https://docs.microsoft.com/azure/azure-stack/asdk/asdk-deploy).

#### <a name="connect-to-azure-stack-using-code"></a>Kod kullanarak Azure yığın Bağlan

Kod kullanarak Azure yığın bağlanmak için kimlik doğrulama ve grafik uç noktaları Azure yığın yüklemenizin almak ve REST isteklerini kullanarak kimlik doğrulaması yapmak için Azure Resource Manager uç noktaları API kullanın. Örnek bir istemci uygulama bulabilirsiniz [GitHub](https://github.com/shriramnat/HybridARMApplication).

>[!Note]
>Seçtiğiniz dil için Azure SDK'sı Azure API profilleri desteklemedikçe SDK'sı Azure yığın ile çalışmayabilir. Azure API profilleri hakkında daha fazla bilgi için bkz: [API sürümü profillerini yönetmek](https://docs.microsoft.com/da-dk/azure/azure-stack/user/azure-stack-version-profiles) makalesi.

## <a name="next-steps"></a>Sonraki adımlar

 - Kimlik Azure yığınında nasıl işlendiğini hakkında daha fazla bilgi için bkz: [Azure yığın kimliği mimarisi](https://docs.microsoft.com/azure/azure-stack/azure-stack-identity-architecture).
 - Azure bulut desenleri hakkında daha fazla bilgi için bkz: [bulut tasarım modeli](https://docs.microsoft.com/azure/architecture/patterns).
