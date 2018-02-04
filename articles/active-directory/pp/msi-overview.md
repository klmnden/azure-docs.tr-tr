---
title: "Azure Active Directory için hizmet kimliği (MSI) yönetilen"
description: "Yönetilen hizmet kimliği genel bakış Azure kaynakları için."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: identity
ms.date: 12/15/2017
ms.author: daveba
ms.reviewer: skwan
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 95980c082b09ad959ab8bbaae0250b40ac08d2c8
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
#  <a name="managed-service-identity-msi-for-azure-resources"></a>Yönetilen hizmet kimliği (MSI) için Azure kaynakları

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Bulut uygulamaları derleme kimlik bilgilerini yönetme olduğunda ortak bir challenge, bulut hizmetlerine kimlik doğrulaması için kodunuzda olması gerekir. Bu kimlik bilgileri güvenliğini sağlamanın önemli bir görevdir. İdeal olarak, bunlar hiç Geliştirici istasyonlarında görünür veya kaynak denetimine iade. Azure anahtar kasası kimlik bilgileri ve diğer anahtarları ve gizli anahtarları güvenli bir şekilde depolamak için bir yol sağlar, ancak bunları almak için anahtar Kasası'na kimlik doğrulaması kodunuzu gerekiyor. Yönetilen hizmet kimliği (MSI), Azure hizmetleri otomatik olarak yönetilen bir kimliği Azure Active Directory (Azure AD) vererek daha basit bu sorunun çözümüne yapar. Bu kimlik, anahtar kasası, kodunuzda herhangi bir kimlik bilgisi olmadan dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmeti için kimlik doğrulaması kullanabilirsiniz.

## <a name="how-does-it-work"></a>Nasıl çalışır?

Yönetilen hizmet kimliği üzerinde bir Azure hizmet örneğini kullanırken, Azure, Azure aboneliğinizin tarafından kullanılan Azure AD kiracısı bir kimlik oluşturur. Ayrıca, Azure hizmet örneği oturum kimliği için kimlik bilgilerini sağlar. Sonuç olarak, kodunuzu Azure AD kimlik doğrulamasını destekleyen hizmetler için erişim belirteçleri almak için bir yerel isteği daha sonra yapabilirsiniz. Tüm Azure mvc'deki çalışırken hizmet örneği tarafından kullanılan kimlik bilgileri.

## <a name="how-do-i-enable-my-resources-to-use-a-managed-service-identity"></a>Yönetilen hizmet kimliği kullanılacak Kaynaklarım nasıl etkinleştirebilirim?

Yönetilen hizmet kimliği iki tür kullanılabilir: *sistem atanan* ve *atanmış kullanıcı*.

- A **sistem atanan** MSI doğrudan Azure hizmet örneği üzerinde etkindir. Etkinleştirme işlemi aracılığıyla Azure Azure AD kiracısı'nda hizmet örneği için bir kimlik oluşturur ve hizmet örneği oturum kimliği için kimlik bilgilerini sağlar. Atanan MSI Azure'a doğrudan bağlı bir sistem yaşam döngüsünü hizmet örneği üzerinde etkindir. Hizmet örneği silinirse, Azure otomatik olarak kimlik bilgilerini ve kimlik Azure AD'de temizler.

- A **atanmış kullanıcı** MSI *(özel olarak incelenmektedir)* tek başına Azure kaynak olarak oluşturulur. Oluşturma işlemi yoluyla, Azure, Azure AD kiracısı'nda bir kimliği oluşturur. Kimlik oluşturduktan sonra bir veya daha fazla Azure hizmet örneklerine atanabilir. Kullanıcı tarafından atanan bir MSI birden çok Azure hizmeti örnekleri tarafından kullanılabildiğinden, yaşam döngüsü ayrı olarak yönetilir.

Sistem tarafından atanan bir MSI Azure sanal makineler ile nasıl çalıştığını örnek aşağıda verilmiştir.

![Sanal makine MSI örneği](~/articles/active-directory/media/msi-vm-example.png)

1. Azure Kaynak Yöneticisi bir VM üzerinde sistem atanan MSI etkinleştirmek için bir ileti alır.
2. Azure Resource Manager VM kimliğini temsil etmek için Azure AD içinde bir hizmet sorumlusu oluşturur. Hizmet sorumlusu Bu abonelik tarafından güvenilen Azure AD kiracısı oluşturulur.
3. Azure Resource Manager hizmet sorumlusu ayrıntıları MSI VM uzantısı'nda VM yapılandırır. Bu adım, istemci kimliği ve Azure AD erişim belirteçleri almak için uzantı tarafından kullanılan sertifika yapılandırmayı içerir.
4. VM hizmet sorumlusu kimliğini bilinen, Azure kaynaklarına erişimi verilebilir. Kodunuzu Azure Resource Manager çağırmak gerekirse, örneğin, daha sonra VM'ın hizmet sorumlusu Azure AD'de rol tabanlı erişim denetimi (RBAC) kullanarak uygun rol atamanız gerekir.  Daha sonra kodunuzu anahtar kasası çağırmak gerekirse, belirli gizli veya anahtar kasası anahtarında, kodu erişim verin.
5. MSI VM uzantısı tarafından barındırılan bir yerel uç noktasından belirteç VM'de çalıştırılan kodunuzu istekleri: http://localhost:50342/oauth2/belirteci. Kaynak parametresi belirteç gönderildiği hizmeti belirtir. Örneğin, Azure Resource Manager kimliğini doğrulamak için kodunuzu istiyorsanız, kaynak kullanırsınız https://management.azure.com/ =.
6. MSI VM uzantısı, Azure AD'den bir erişim belirteci istemek için yapılandırılmış istemci kimliği ve sertifika kullanır.  Azure AD bir JSON Web Token (JWT) erişim belirteci döndürür.
7. Kodunuzu Azure AD kimlik doğrulamasını destekleyen bir hizmetine yapılan bir çağrı erişim belirteci gönderir.

Aynı diyagramın, burada örnek bir kullanıcı tarafından atanan MSI Azure sanal makineler ile nasıl çalıştığını kullanarak.

![Sanal makine MSI örneği](~/articles/active-directory/media/msi-vm-example.png)

1. Azure Resource Manager, bir kullanıcı tarafından atanan MSI oluşturmak için bir ileti alır.
2. Azure Resource Manager MSI kimliğini temsil etmek için Azure AD içinde bir hizmet sorumlusu oluşturur. Hizmet sorumlusu Bu abonelik tarafından güvenilen Azure AD kiracısı oluşturulur.
3. Azure Kaynak Yöneticisi bir VM MSI VM uzantısında hizmet sorumlusu ayrıntıları yapılandırmak için bir ileti alır. Bu adım, istemci kimliği ve Azure AD erişim belirteçleri almak için uzantı tarafından kullanılan sertifika yapılandırmayı içerir.
4. MSI hizmet sorumlusu kimliğini bilinen, Azure kaynaklarına erişimi verilebilir. Kodunuzu Azure Resource Manager çağırmak gerekirse, örneğin, daha sonra MSI hizmet sorumlusu Azure AD'de rol tabanlı erişim denetimi (RBAC) kullanarak uygun rol atamanız gerekir. Daha sonra kodunuzu anahtar kasası çağırmak gerekirse, belirli gizli veya anahtar kasası anahtarında, kodu erişim verin. Not: 3. adım adımı tamamlamak için 4 gerekli değildir. Bir MSI var olduğunda, bir VM'de veya yapılandırılmakta bağımsız olarak kaynaklarına erişimi verilebilir.
5. MSI VM uzantısı tarafından barındırılan bir yerel uç noktasından belirteç VM'de çalıştırılan kodunuzu istekleri: http://localhost:50342/oauth2/belirteci. İstemci kimliği parametresi kullanılacak MSI kimliği adını belirtir. Ayrıca, kaynak parametresi belirteç gönderildiği hizmeti belirtir. Örneğin, Azure Resource Manager kimliğini doğrulamak için kodunuzu istiyorsanız, kaynak kullanırsınız https://management.azure.com/ =.
6. MSI VM uzantısı istenen istemci Kimliğine ilişkin sertifika yapılandırılmış ve bir erişim belirteci Azure AD'den istekleri olmadığını denetler. Azure AD bir JSON Web Token (JWT) erişim belirteci döndürür.
7. Kodunuzu Azure AD kimlik doğrulamasını destekleyen bir hizmetine yapılan bir çağrı erişim belirteci gönderir.

Yönetilen hizmet kimliği destekleyen her Azure hizmet kodunuzun bir erişim belirteci edinmek için kendi yöntemi vardır. Öğreticiler bir belirteç almak üzere belirli yöntemini bulmak her hizmet için göz atın.

## <a name="try-managed-service-identity"></a>Yönetilen hizmet kimliği deneyin

Farklı Azure kaynaklarına erişmek için uçtan uca senaryoları öğrenmek için bir yönetilen hizmet kimliği öğretici deneyin:
<br><br>
| MSI etkin kaynaktan | Aşağıdakileri nasıl yapacağınızı öğrenin: |
| ------- | -------- |
| Azure VM (Linux)   | [Erişim Azure Resource Manager bir Linux VM ile yönetilen hizmet kimliği](msi-tutorial-linux-vm-access-arm.md) |
|                    | [Erişim anahtarı aracılığıyla erişim Azure Storage ile bir Linux VM yönetilen hizmet kimliği](msi-tutorial-linux-vm-access-storage.md) |

## <a name="which-azure-services-support-managed-service-identity"></a>Hangi Azure Hizmetleri yönetilen hizmet kimliği destekliyor?

Yönetilen hizmet kimliği destekleyen azure Hizmetleri, Azure AD kimlik doğrulamasını destekleyen hizmetleri kimliğini doğrulamaya MSI kullanabilirsiniz.  MSI ve Azure AD kimlik doğrulaması Azure arasında tümleştirme sürecinde duyuyoruz.  Denetleme Güncelleştirmeleri için sık sık.

### <a name="azure-services-that-support-managed-service-identity"></a>Yönetilen hizmet kimliği destekleyen azure Hizmetleri

Aşağıdaki Azure hizmetlerini yönetilen hizmet kimliği destekler.

| Hizmet | Durum | Tarih | Yapılandırma | Belirteç alın |
| ------- | ------ | ---- | --------- | ----------- |
| Azure Sanal Makineler | Önizleme | Eylül 2017 | [Azure CLI](msi-qs-configure-cli-windows-vm.md)<br>[Azure Resource Manager şablonları](msi-qs-configure-template-windows-vm.md) | [Bash/Curl](msi-how-to-use-vm-msi-token.md#get-a-token-using-curl)<br>[HTTP/REST](msi-how-to-use-vm-msi-token.md#get-a-token-using-http) |

### <a name="azure-services-that-support-azure-ad-authentication"></a>Bu destek Azure AD kimlik doğrulaması Azure Hizmetleri

Aşağıdaki hizmetler Azure AD kimlik doğrulamayı desteklemek ve yönetilen hizmet kimliği kullanan istemci Hizmetleri ile test edilmiştir.

| Hizmet | Kaynak kimliği | Durum | Tarih | Erişimi atayın |
| ------- | ----------- | ------ | ---- | ------------- |
| Azure Resource Manager | https://management.azure.com/ | Kullanılabilir | Eylül 2017 | [Azure CLI](msi-howto-assign-access-CLI.md) |
| Azure Key Vault | https://vault.azure.net/ | Kullanılabilir | Eylül 2017 | |
| Azure Data Lake | https://datalake.azure.net/ | Kullanılabilir | Eylül 2017 | |
| Azure SQL | https://database.windows.net/ | Kullanılabilir | Ekim 2017 | |

## <a name="how-much-does-managed-service-identity-cost"></a>Nasıl yönetilen hizmet kimliği maliyeti nedir?

Azure abonelikleri için varsayılan değer olan yönetilen hizmet kimliği Azure Active Directory ücretsiz ile birlikte gelir.  Yönetilen hizmet kimliği için ek bir maliyet yoktur.

## <a name="support-and-feedback"></a>Destek ve geri bildirim

Görüşlerinizi almak isteriz!

* Nasıl yapılır soruları yığın taşması etiketiyle sorun [azure MSI](http://stackoverflow.com/questions/tagged/azure-msi).
* Özellik istekleri veya geri bildirim verin [geliştiriciler için Azure AD geri bildirim Forumunda](https://feedback.azure.com/forums/169401-azure-active-directory/category/164757-developer-experiences).






