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
ms.date: 05/07/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: 9a025716c2bb6266c1c1c552a2d0791b39429cac
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="tutorial-configure-hybrid-cloud-identity-with-azure-and-azure-stack-applications"></a>Öğretici: Azure ve Azure yığın uygulamaları ile karma bulut kimlik yapılandırın

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Genel Azure ve Azure yığın uygulamalarınızda erişim vermek için iki seçeneğiniz vardır. Azure yığın sürekli bir Internet bağlantısına sahip olduğunda, Azure Active Directory (Azure AD) kullanabilirsiniz. Azure yığın Internet bağlantısı kesildiğinde, Azure Directory Federasyon Hizmetleri (AD FS) kullanabilirsiniz. Azure yığınında uygulamalarınızı dağıtma veya yapılandırma Azure yığınında Azure Kaynak Yöneticisi aracılığıyla amacıyla erişim vermek için hizmet asıl adı kullanın. 

Bu öğreticide, bir örnek ortamı için yapı:

> [!div class="checklist"]
> * Karma kimliğini genel Azure ve Azure yığın oluşturma
> * Azure yığını API'si erişmek için bir belirteç alınıyor.

Bu adımları Azure yığın operatör izinleri olmasını gerektirir.

## <a name="create-a-service-principal-for-azure-ad-in-the-portal"></a>Azure AD portalı için bir hizmet sorumlusu oluşturma

Azure AD kimlik deposu olarak kullanarak Azure yığın dağıttıktan sonra Azure için gibi hizmet asıl adı oluşturabilirsiniz. [Bu belge](https://docs.microsoft.com/en-us/azure/azure-stack/user/azure-stack-create-service-principals#create-service-principal-for-azure-ad) portal üzerinden adımların nasıl gerçekleştirileceğini gösterir. Sahip olduğunuz onay [gereken Azure AD izinler](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal#required-permissions) başlamadan önce.

## <a name="create-a-service-principal-for-ad-fs-using-powershell"></a>PowerShell kullanarak AD FS için hizmet sorumlusu oluşturma

AD FS ile Azure yığın dağıttıysanız, bir hizmet sorumlusu oluşturma, erişim için bir rol atayın ve Powershell'den bu kimliğini kullanarak oturum açmak için PowerShell'i kullanabilirsiniz. [Bu belge](https://docs.microsoft.com/en-us/azure/azure-stack/user/azure-stack-create-service-principals#create-service-principal-for-ad-fs) PowerShell kullanarak adımların nasıl gerçekleştirileceğini gösterir.

## <a name="using-the-azure-stack-api"></a>Azure yığını API'si kullanma

[Bu öğretici](https://docs.microsoft.com/en-us/azure/azure-stack/user/azure-stack-rest-api-use) Azure yığını API'si erişmek için bir belirteç alma işleminde size kılavuzluk edecektir.

## <a name="connect-to-azure-stack-using-powershell"></a>Azure Powershell kullanarak yığınına Bağlan

Burada, Azure yığınına bağlanmak için bu belgedeki öğrettin kavramları kullanılarak bir örnek komut verilmiştir.

### <a name="prerequisites"></a>Önkoşullar

Bir Azure yığın yükleme Azure Active Directory'ye erişmek için bir aboneliğe bağlı. Bir Azure yığın yükleme yoksa, bu yönergeleri ayarlamak için kullanabileceğiniz bir [Azure yığın Geliştirme Seti](https://docs.microsoft.com/en-us/azure/azure-stack/asdk/asdk-deploy).

[Bu öğretici](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-powershell-configure-quickstart) Azure PowerShell'i yüklemek ve Azure yığın yüklemenizi bağlanmak için gereken adımlarda size yol gösterecek.

#### <a name="connect-to-azure-stack-using-code"></a>Kod kullanarak Azure yığın Bağlan

Kod kullanarak Azure yığın bağlanmak için kimlik doğrulama ve grafik uç noktaları Azure yığın yüklemenizin almak ve REST isteklerini kullanarak kimlik doğrulaması yapmak için Azure Resource Manager uç noktaları API kullanın. Örnek bir uygulama bulabilirsiniz [burada](https://github.com/shriramnat/HybridARMApplication).

> [!note]  
Seçtiğiniz dil için Azure SDK'sı Azure API profilleri desteklemedikçe SDK'sı Azure yığın ile çalışmayabilir. Azure API profilleri hakkında daha fazla bilgi için Git [burada](https://docs.microsoft.com/da-dk/azure/azure-stack/user/azure-stack-version-profiles).

## <a name="next-steps"></a>Sonraki adımlar

 - Kimlik Azure yığınında nasıl işlendiğini hakkında daha fazla bilgi için bkz: [Azure yığın kimliği mimarisi](https://docs.microsoft.com/azure/azure-stack/azure-stack-identity-architecture).  
 - Azure bulut desenleri hakkında daha fazla bilgi için bkz: [bulut tasarım modeli](https://docs.microsoft.com/azure/architecture/patterns).
