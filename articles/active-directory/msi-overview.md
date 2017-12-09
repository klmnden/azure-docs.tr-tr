---
title: "Azure Active Directory için hizmet kimliği (MSI) yönetilen"
description: "Yönetilen hizmet kimliği genel bakış Azure kaynakları için."
services: active-directory
documentationcenter: 
author: skwan
manager: mbaldwin
editor: 
ms.assetid: 0232041d-b8f5-4bd2-8d11-27999ad69370
ms.service: active-directory
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: identity
ms.date: 10/31/2017
ms.author: skwan
ms.openlocfilehash: 428b0b2397887299e691c82751f6c45673575f26
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
---
#  <a name="managed-service-identity-msi-for-azure-resources"></a>Yönetilen hizmet kimliği (MSI) için Azure kaynakları

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Bulut uygulamaları derleme kimlik bilgilerini yönetme olduğunda ortak bir challenge, bulut hizmetlerine kimlik doğrulaması için kodunuzda olması gerekir. Bu kimlik bilgileri güvenliğini sağlamanın önemli bir görevdir. İdeal olarak, bunlar hiç Geliştirici istasyonlarında görünür veya kaynak denetimine iade. Azure anahtar kasası kimlik bilgileri ve diğer anahtarları ve gizli anahtarları güvenli bir şekilde depolamak için bir yol sağlar, ancak bunları almak için anahtar Kasası'na kimlik doğrulaması kodunuzu gerekiyor. Yönetilen hizmet kimliği (MSI), Azure hizmetleri otomatik olarak yönetilen bir kimliği Azure Active Directory (Azure AD) vererek daha basit bu sorunun çözümüne yapar. Bu kimlik, anahtar kasası, kodunuzda herhangi bir kimlik bilgisi olmadan dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmeti için kimlik doğrulaması kullanabilirsiniz.

## <a name="how-does-it-work"></a>Nasıl çalışır?

Yönetilen hizmet kimliği üzerinde bir Azure hizmetini etkinleştirdiğinizde, Azure hizmet örneği için bir kimlik, Azure aboneliğinizin tarafından kullanılan Azure AD kiracısı otomatik olarak oluşturur.  Perde arkasında Azure hizmet örneği oturum kimliği için kimlik bilgilerini sağlar.  Kodunuzu Azure AD kimlik doğrulamasını destekleyen hizmetler için erişim belirteçleri almak için bir yerel isteği daha sonra yapabilirsiniz.  Azure hizmet örneği tarafından kullanılan kimlik bilgileri çalışırken mvc'deki.  Hizmet örneği silinirse, Azure otomatik olarak kimlik bilgilerini ve kimlik Azure AD'de temizler.

Yönetilen hizmet kimliği Azure sanal makineler ile nasıl çalıştığını örnek aşağıda verilmiştir.

![Sanal makine MSI örneği](./media/msi-vm-example.png)

1. Azure Kaynak Yöneticisi bir VM üzerinde MSI etkinleştirmek için bir ileti alır.
2. Azure Resource Manager VM kimliğini temsil etmek için Azure AD içinde bir hizmet sorumlusu oluşturur. Hizmet sorumlusu Bu abonelik tarafından güvenilen Azure AD kiracısı oluşturulur.
3. Azure Resource Manager hizmet sorumlusu ayrıntıları MSI VM uzantısı'nda VM yapılandırır.  Bu adım, istemci kimliği ve Azure AD erişim belirteçleri almak için uzantı tarafından kullanılan sertifika yapılandırmayı içerir.
4. VM hizmet sorumlusu kimliğini bilinen, Azure kaynaklarına erişimi verilebilir.  Kodunuzu Azure Resource Manager çağırmak gerekirse, örneğin, daha sonra VM'ın hizmet sorumlusu Azure AD'de rol tabanlı erişim denetimi (RBAC) kullanarak uygun rol atamanız gerekir.  Daha sonra kodunuzu anahtar kasası çağırmak gerekirse, belirli gizli veya anahtar kasası anahtarında, kodu erişim verin.
5. MSI VM uzantısı tarafından barındırılan bir yerel uç noktasından belirteç VM'de çalıştırılan kodunuzu istekleri: http://localhost:50342/oauth2/belirteci.  Kaynak parametresi belirteç gönderildiği hizmeti belirtir. Örneğin, Azure Resource Manager kimliğini doğrulamak için kodunuzu istiyorsanız, kaynak kullanırsınız https://management.azure.com/ =.
6. MSI VM uzantısı, Azure AD'den bir erişim belirteci istemek için yapılandırılmış istemci kimliği ve sertifika kullanır.  Azure AD bir JSON Web Token (JWT) erişim belirteci döndürür.
7. Kodunuzu Azure AD kimlik doğrulamasını destekleyen bir hizmetine yapılan bir çağrı erişim belirteci gönderir.

Yönetilen hizmet kimliği destekleyen her Azure hizmet kodunuzun bir erişim belirteci edinmek için kendi yöntemi vardır. Öğreticiler bir belirteç almak üzere belirli yöntemini bulmak her hizmet için göz atın.

## <a name="try-managed-service-identity"></a>Yönetilen hizmet kimliği deneyin

Farklı Azure kaynaklarına erişmek için uçtan uca senaryoları öğrenmek için bir yönetilen hizmet kimliği öğretici deneyin:
<br><br>
| MSI etkin kaynaktan | Aşağıdakileri nasıl yapacağınızı öğrenin: |
| ------- | -------- |
| Azure VM (Windows) | [Hizmet kimliği erişim Azure Data Lake Store bir Windows VM ile yönetilen](msi-tutorial-windows-vm-access-datalake.md) |
|                    | [Bir Windows VM ile erişim Azure Kaynak Yöneticisi yönetilen hizmet kimliği](msi-tutorial-windows-vm-access-arm.md) |
|                    | [Erişim Azure SQL bir Windows VM ile yönetilen hizmet kimliği](msi-tutorial-windows-vm-access-sql.md) |
|                    | [Erişim anahtarı aracılığıyla erişim Azure Storage ile bir Windows VM yönetilen hizmet kimliği](msi-tutorial-windows-vm-access-storage.md) |
|                    | [Erişim Azure depolama Windows VM ile SAS aracılığıyla yönetilen hizmet kimliği](msi-tutorial-windows-vm-access-storage-sas.md) |
|                    | [Bir Windows VM yönetilen hizmet kimliği ve Azure anahtar kasası ile Azure olmayan AD kaynağa erişim](msi-tutorial-windows-vm-access-nonaad.md) |
| Azure VM (Linux)   | [Hizmet kimliği erişim Azure Data Lake Store bir Linux VM ile yönetilen](msi-tutorial-linux-vm-access-datalake.md) |
|                    | [Erişim Azure Resource Manager bir Linux VM ile yönetilen hizmet kimliği](msi-tutorial-linux-vm-access-arm.md) |
|                    | [Erişim anahtarı aracılığıyla erişim Azure Storage ile bir Linux VM yönetilen hizmet kimliği](msi-tutorial-linux-vm-access-storage.md) |
|                    | [Erişim Azure depolama bir Linux VM ile SAS aracılığıyla yönetilen hizmet kimliği](msi-tutorial-linux-vm-access-storage-sas.md) |
|                    | [Bir Linux VM yönetilen hizmet kimliği ve Azure anahtar kasası ile Azure olmayan AD kaynağa erişim](msi-tutorial-linux-vm-access-nonaad.md) |
| Azure App Service  | [Azure uygulama hizmeti veya Azure işlevleri ile yönetilen hizmet kimliğini kullan](/azure/app-service/app-service-managed-service-identity) |
| Azure işlevi     | [Azure uygulama hizmeti veya Azure işlevleri ile yönetilen hizmet kimliğini kullan](/azure/app-service/app-service-managed-service-identity) |

## <a name="which-azure-services-support-managed-service-identity"></a>Hangi Azure Hizmetleri yönetilen hizmet kimliği destekliyor?

Yönetilen hizmet kimliği destekleyen azure Hizmetleri, Azure AD kimlik doğrulamasını destekleyen hizmetleri kimliğini doğrulamaya MSI kullanabilirsiniz.  MSI ve Azure AD kimlik doğrulaması Azure arasında tümleştirme sürecinde duyuyoruz.  Denetleme Güncelleştirmeleri için sık sık.

### <a name="azure-services-that-support-managed-service-identity"></a>Yönetilen hizmet kimliği destekleyen azure Hizmetleri

Aşağıdaki Azure hizmetlerini yönetilen hizmet kimliği destekler.

| Hizmet | Durum | Tarih | Yapılandırma | Belirteç alın |
| ------- | ------ | ---- | --------- | ----------- |
| Azure Sanal Makineler | Önizleme | Eylül 2017 | [Azure portal](msi-qs-configure-portal-windows-vm.md)<br>[PowerShell](msi-qs-configure-powershell-windows-vm.md)<br>[Azure CLI](msi-qs-configure-cli-windows-vm.md)<br>[Azure Resource Manager şablonları](msi-qs-configure-template-windows-vm.md) | [REST](msi-how-to-use-vm-msi-token.md#get-a-token-using-http)<br>[.NET](msi-how-to-use-vm-msi-token.md#get-a-token-using-c)<br>[Bash/Curl](msi-how-to-use-vm-msi-token.md#get-a-token-using-curl)<br>[Git](msi-how-to-use-vm-msi-token.md#get-a-token-using-go)<br>[PowerShell](msi-how-to-use-vm-msi-token.md#get-a-token-using-azure-powershell) |
| Azure App Service | Önizleme | Eylül 2017 | [Azure portal](/azure/app-service/app-service-managed-service-identity#using-the-azure-portal)<br>[Azure Resource Manager şablonu](/azure/app-service/app-service-managed-service-identity#using-an-azure-resource-manager-template) | [.NET](/azure/app-service/app-service-managed-service-identity#asal)<br>[REST](/azure/app-service/app-service-managed-service-identity#using-the-rest-protocol) |
| Azure İşlevleri | Önizleme | Eylül 2017 | [Azure portal](/azure/app-service/app-service-managed-service-identity#using-the-azure-portal)<br>[Azure Resource Manager şablonu](/azure/app-service/app-service-managed-service-identity#using-an-azure-resource-manager-template) | [.NET](/azure/app-service/app-service-managed-service-identity#asal)<br>[REST](/azure/app-service/app-service-managed-service-identity#using-the-rest-protocol) |
| Azure Data Factory V2 | Önizleme | Kasım 2017 | [Azure portal](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity)<br>[PowerShell](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-powershell)<br>[REST](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-rest-api)<br>[SDK](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-sdk) |

### <a name="azure-services-that-support-azure-ad-authentication"></a>Bu destek Azure AD kimlik doğrulaması Azure Hizmetleri

Aşağıdaki hizmetler Azure AD kimlik doğrulamayı desteklemek ve yönetilen hizmet kimliği kullanan istemci Hizmetleri ile test edilmiştir.

| Hizmet | Kaynak kimliği | Durum | Tarih | Erişimi atayın |
| ------- | ----------- | ------ | ---- | ------------- |
| Azure Resource Manager | https://Management.Azure.com/ | Kullanılabilir | Eylül 2017 | [Azure portal](msi-howto-assign-access-portal.md) <br>[PowerShell](msi-howto-assign-access-powershell.md) <br>[Azure CLI](msi-howto-assign-access-CLI.md) |
| Azure Key Vault | https://vault.Azure.NET/ | Kullanılabilir | Eylül 2017 | |
| Azure Data Lake | https://datalake.Azure.NET/ | Kullanılabilir | Eylül 2017 | |
| Azure SQL | https://Database.Windows.NET/ | Kullanılabilir | Ekim 2017 | |

## <a name="how-much-does-managed-service-identity-cost"></a>Nasıl yönetilen hizmet kimliği maliyeti nedir?

Azure abonelikleri için varsayılan değer olan yönetilen hizmet kimliği Azure Active Directory ücretsiz ile birlikte gelir.  Yönetilen hizmet kimliği için ek bir maliyet yoktur.

## <a name="support-and-feedback"></a>Destek ve geri bildirim

Görüşlerinizi almak isteriz!

* Nasıl yapılır soruları yığın taşması etiketiyle sorun [azure MSI](http://stackoverflow.com/questions/tagged/azure-msi).
* Özellik istekleri veya geri bildirim verin [geliştiriciler için Azure AD geri bildirim Forumunda](https://feedback.azure.com/forums/169401-azure-active-directory/category/164757-developer-experiences).






