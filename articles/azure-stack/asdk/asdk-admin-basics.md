---
title: Azure Stack Geliştirme Seti temel bilgileri | Microsoft Docs
description: Azure Stack geliştirme Seti'ni (ASDK) için temel yönetim görevlerini gerçekleştirmek nasıl açıklar.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 66a2871e0c4b36959ccd8f08df5b6b7edd09f624
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51227833"
---
# <a name="asdk-administration-basics"></a>ASDK yönetim temel bilgileri 
Azure Stack geliştirme Seti'ni (ASDK) yönetim için yeni bilmeniz gereken birkaç şey vardır. Bu kılavuz değerlendirme ortamında Azure Stack operatörü olarak sizin rolünüze genel bir bakış sağlar ve test kullanıcılarınızın emin olmak nasıl hızlı şekilde üretken hale gelebilir.

İlk olarak, gözden geçirmeniz gereken [Azure Stack geliştirme Seti'ni nedir?](asdk-what-is.md) makalenin amacı, ASDK ve kısıtlamalarını anladığınızdan emin olun. Geliştirme ve üretim dışı ortamda uygulamalarınızı test etmek için Azure Stack burada değerlendirebilirsiniz, bir "korumalı alan," Geliştirme Seti kullanmanız gerekir. 

Biz ASDK, yeni derlemeler düzenli olarak kullanıma sunarız şekilde Azure'da olduğu gibi Azure Stack hızlı bir şekilde yeniliklerine öncülük ediyor. Ancak, Azure Stack tümleşik sistemleri dağıtımları gibi ASDK yükseltemezsiniz. En son derlemeye taşımak istiyorsanız, bu nedenle, tamamen gerekir [ASDK yeniden](asdk-redeploy.md). Güncelleştirme paketleri uygulanamıyor. Bu işlem zaman alır, ancak kullanılabilir olduklarında hemen sonra en son özellikleri deneyebilirsiniz avantajdır. 

## <a name="what-account-should-i-use"></a>Hangi hesabı kullanmalıyım?
Azure Stack yönetirken bilmeniz gereken birkaç hesabında dikkate alınacak noktalar vardır. Dağıtımlarda özellikle Windows Server Active Directory Federasyon Hizmetleri (ADFS) yerine Azure Active Directory (Azure AD) kimlik sağlayıcısı olarak kullanma. Azure Stack tümleşik sistemleri ve ASDK dağıtımları için aşağıdaki hesabı maddeler geçerlidir:

|Hesap|Azure AD|AD FS|
|-----|-----|-----|
|Yerel yönetici (. \Administrator)|ASDK konak yönetici|ASDK konak yönetici|
|AzureStack\AzureStackAdmin|ASDK konak yönetici<br><br>Azure Stack yönetim portalında oturum açmak için kullanılabilir<br><br>Service Fabric halkaları yönetmek ve görüntülemek için erişim|ASDK konak yönetici<br><br>Azure Stack yönetim portalına erişim yok<br><br>Service Fabric halkaları yönetmek ve görüntülemek için erişim<br><br>Artık sahibi varsayılan sağlayıcı aboneliği (DPS)|
|AzureStack\CloudAdmin|Erişebilir ve ayrıcalıklı uç nokta içinde izin verilen komutları çalıştırın|Erişebilir ve ayrıcalıklı uç nokta içinde izin verilen komutları çalıştırın<br><br>ASDK ana bilgisayara oturum yok<br><br>Varsayılan sağlayıcı aboneliği (yu DPS) sahibi|
|Azure AD genel Yöneticisi|Yükleme sırasında kullanılan<br><br>Varsayılan sağlayıcı aboneliği (yu DPS) sahibi|Uygulanamaz|
|

## <a name="what-tools-do-i-use-to-manage"></a>Yönetmek için hangi araçları kullanabilir?
Kullanabileceğiniz [Azure Stack Yönetici portalı](https://adminportal.local.azurestack.external) veya Azure Stack yönetmek için PowerShell'i. Temel kavramları öğrenmenin en kolay yolu portalı kullanmaktır. PowerShell kullanmak istiyorsanız, yüklemeniz gerekir [Azure Stack için PowerShell](asdk-post-deploy.md#install-azure-stack-powershell) ve [Azure Stack araçları Github'dan indirin](asdk-post-deploy.md#download-the-azure-stack-tools).

Azure Stack, Azure Resource Manager, temel alınan dağıtımı, yönetimi ve kuruluş mekanizması olarak kullanır. Azure Stack yönetmek ve kullanıcıların desteklemeye yardımcı olmak için kullanacaksanız, Azure Resource Manager hakkında bilgi edinin. Okuyarak daha fazla bilgi [Azure Resource Manager Teknik İnceleme ile çalışmaya başlama](https://download.microsoft.com/download/E/A/4/EA4017B5-F2ED-449A-897E-BD92E42479CE/Getting_Started_With_Azure_Resource_Manager_white_paper_EN_US.pdf).

## <a name="your-typical-responsibilities"></a>Tipik sizin Sorumluluklarınız
Kullanıcıların hizmetleri kullanmak istiyorsunuz. Kendi bakış açısına ana rolünüz hizmetlerin kullanılabilir olmasını sağlamaktır. ASDK kullanarak hangi hizmetlerin sunduğu bilgi edinin ve tarafından kullanılabilir hale getirmeniz nasıl Hizmetleri [planlar, teklifler ve kotaları oluşturmaya](asdk-offer-services.md). Sanal makine görüntüleri gibi Market öğeleri eklemek gerekir. En kolay yolu [Market öğelerini indirme](asdk-marketplace-item.md) Azure Stack için azure'dan.

> [!NOTE]
> Planlar, teklifler ve Hizmetleri test etmek isterseniz, kullanması gereken [kullanıcı portalı](https://portal.local.azurestack.external); değil [Yönetici portalı](https://adminportal.local.azurestack.external).

Hizmetleri sağlamaya ek olarak, bir Azure Stack operatörü, tüm normal görevlerini ASDK çalışmaya tutmak için gerçekleştirmeniz gerekir. Bu görevleri, aşağıdaki gibi işlemler şunlardır:
- Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS) dağıtımı için kullanıcı hesaplarını ekleyin.
- Rol tabanlı erişim denetimi (RBAC) rolleri (Bu yalnızca yöneticiler tarafından kısıtlı değildir) atayın.
- Altyapı sistem durumunu izleme
- Ağ ve depolama kaynaklarını yönetme
- Başarısız Geliştirme Seti ana bilgisayar donanımı değiştirme 

## <a name="where-to-get-support"></a>Nereden destek
Geliştirme Seti için destek ile ilgili sorularınızı yalnızca destek seçeneğinizi olan [Microsoft Azure Stack Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack). Yönetici portalının sağ üst köşedeki Yardım ve Destek (soru işareti) simgesine tıklayın ve ardından **yeni destek isteği**, doğrudan bu forum sitesini açar. Bu Forum düzenli olarak izlenir. 

> [!IMPORTANT]
> ASDK değerlendirme ortamı olduğundan, Microsoft Müşteri Destek Hizmetleri (CSS) aracılığıyla sunulan resmi desteği yoktur.

## <a name="next-steps"></a>Sonraki adımlar
[ASDK dağıtma](asdk-install.md)

