---
title: Azure BLOB'ları ve kuyrukları kullanarak Azure Active Directory erişim yetkisi | Microsoft Docs
description: Azure BLOB'ları ve kuyrukları kullanarak Azure Active Directory erişimi yetkisi verme.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/21/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 8033dda4059a52cea2b775fc8765a9f2a91b96dd
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67302425"
---
# <a name="authorize-access-to-azure-blobs-and-queues-using-azure-active-directory"></a>Azure BLOB'ları ve kuyrukları kullanarak Azure Active Directory erişim yetkisi verme

Blob ve kuyruk depolama isteklerine yetkilendirmek için Azure Active Directory (AD) kullanarak azure depolama destekler. Azure AD ile olabilen bir kullanıcı, Grup veya uygulama hizmet sorumlusu, rol tabanlı erişim denetimi (RBAC) bir güvenlik sorumlusu için izinleri vermek için kullanabilirsiniz. Güvenlik sorumlusunun, OAuth 2.0 belirteç döndürecek şekilde Azure AD tarafından doğrulanır. Belirteç, Blob veya kuyruk depolama alanındaki bir kaynağa erişmek için bir isteği yetkilendirmek için kullanılabilir.

Kullanıcıları veya Azure AD tarafından döndürülen bir OAuth 2.0 belirteç kullanan uygulamalar yetkilendirme paylaşılan anahtar yetkilendirme üst düzey güvenlik ve kullanım kolaylığı sağlar ve paylaşılan erişim imzaları (SAS). Azure AD ile depolama hesabı erişim anahtarı ile kod ve risk olası güvenlik açıklarını gerek yoktur. Paylaşılan anahtar yetkilendirme uygulamalarınızı kullanmaya devam ederken, Azure AD kullanarak kodunuzu ile hesap erişim anahtarını depolamak için gereken bozar. Depolama hesabınızdaki kaynaklara ayrıntılı erişim vermek için paylaşılan erişim imzaları (SAS) kullanmaya devam edebilirsiniz, ancak Azure AD'ye SAS belirteçlerini yönetin veya güvenliği aşılmış bir SAS iptal etme hakkında endişelenmenize gerek kalmadan benzer özellikleri sunar. Microsoft Azure AD yetkilendirme mümkün olduğunda, Azure depolama uygulamalarınızla kullanılmasını önerir.

Azure AD ile yetkilendirme için kullanılabilen genel amaçlı ve Blob Depolama hesapları tüm genel bölgelerde ve Ulusal bulutlarda. Depolama hesapları yalnızca Azure Resource Manager dağıtım modeli desteği ile Azure AD yetkilendirme oluşturuldu.

## <a name="overview-of-azure-ad-for-blobs-and-queues"></a>BLOB'lar ve Kuyruklar için Azure AD genel bakış

Bir güvenlik sorumlusu (kullanıcı, Grup veya uygulama) blob veya kuyruğa bir kaynağa erişmeyi denediğinde bir blob için anonim erişimi kullanılabilir olmadığı sürece istek yetkilendirilmelidir. Azure AD ile bir kaynağa erişim iki adımlı bir işlemdir. İlk olarak, güvenlik sorumlusunun kimliği doğrulanır ve bir OAuth 2.0 belirteç döndürdü. Ardından, belirteci, bir isteğin bir parçası Blob veya kuyruk hizmetine geçirilen ve belirtilen kaynağa erişim yetkisi vermek için hizmet tarafından kullanılan.

Bir uygulama çalışma zamanında bir OAuth 2.0 erişim belirteci isteği kimlik doğrulaması adımı gerektirir. Bir uygulamayı bir Azure VM, sanal makine ölçek kümesi veya bir Azure işlev uygulaması gibi Azure varlık içinde çalıştıran, kullanabilirsiniz bir [yönetilen kimliği](../../active-directory/managed-identities-azure-resources/overview.md) blob'lara erişim veya sıralara. Tarafından yönetilen bir kimlik Azure Blob veya kuyruk hizmetine yapılan isteklerin yetkilendirme konusunda bilgi almak için bkz: [Azure kaynakları için BLOB'ları ve kuyrukları ile Azure Active Directory ve yönetilen kimlikleri erişim yetkisi](storage-auth-aad-msi.md).

Yetkilendirme adım, bir veya daha fazla RBAC rolleri için güvenlik sorumlusu atanmasını gerektirir. Azure depolama, blob ve kuyruk veriler için ortak izin kümelerinin kapsayabilir ve RBAC rollerini sağlar. Bir güvenlik sorumlusu atanmış olan rolleri sorumlu sahip olduğu izinleri belirler. Azure depolama için RBAC rollerini atama hakkında daha fazla bilgi edinmek için [RBAC ile depolama verilere erişim haklarını yönetme](storage-auth-aad-rbac.md).

Yerel uygulamaları ve Azure Blob veya sıra hizmete isteklerde web uygulamaları, ayrıca Azure AD ile erişim yetki verebilir. Bir erişim belirteci isteği ve blob veya kuyruğa veri istekleri yetkilendirmek için kullanma hakkında bilgi edinmek için [bir Azure depolama uygulamasından Azure AD ile Azure depolama için erişim yetkisi](storage-auth-aad-app.md).

## <a name="assigning-rbac-roles-for-access-rights"></a>RBAC rolleri için erişim hakları atama

Azure Active Directory (Azure AD) ile güvenli kaynaklara erişim hakları yetkilendirir [rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/overview.md). Azure depolama, bir dizi ortak blob ve kuyruk verilere erişmek için kullanılan izin kümelerini kapsayacak yerleşik RBAC rolleri tanımlar. Ayrıca, blob ve kuyruk verilere erişim için özel roller tanımlayabilirsiniz.

Bir RBAC rolü için bir Azure AD güvenlik sorumlusu atandığında, Azure verir, bir güvenlik sorumlusu için bu kaynaklara erişin. Abonelik, kaynak grubu, depolama hesabı veya bir kapsayıcının veya kuyruk düzeyi için erişimi sınırlayabilirsiniz. Bir Azure AD güvenlik sorumlusu olabilir bir kullanıcı, Grup, bir uygulama hizmet sorumlusu veya [yönetilen Azure kaynakları için kimliği](../../active-directory/managed-identities-azure-resources/overview.md).

### <a name="built-in-rbac-roles-for-blobs-and-queues"></a>BLOB'lar ve Kuyruklar için yerleşik RBAC rolleri

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

Yerleşik bir RBAC rolü bir güvenlik sorumlusu atama hakkında bilgi edinmek için aşağıdaki makalelerden birine bakın:

- [Azure blob ve kuyruk verilere RBAC ile Azure portalında erişim izni ver](storage-auth-aad-rbac-portal.md)
- [Azure CLI kullanarak RBAC ile Azure blob ve kuyruk verilere erişim izni ver](storage-auth-aad-rbac-cli.md)
- [PowerShell ile RBAC ile Azure blob ve kuyruk verilere erişim izni ver](storage-auth-aad-rbac-powershell.md)

Hakkında daha fazla bilgi için Azure depolama için yerleşik roller tanımlanır, bkz: [rol tanımlarını anlamak](../../role-based-access-control/role-definitions.md#management-and-data-operations). Özel bir RBAC rollerini oluşturma hakkında daha fazla bilgi için bkz: [Azure rol tabanlı erişim denetimi için özel roller oluşturma](../../role-based-access-control/custom-roles.md).

### <a name="access-permissions-for-data-operations"></a>Veri işlemleri için erişim izinleri

Belirli Blob veya kuyruk hizmet işlemlerini aramak üzere gereken izinler hakkında daha fazla bilgi için bkz: [blob ve kuyruk veri işlemleri çağırmak için izinleri](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations).

## <a name="resource-scope"></a>Kaynak kapsamı

[!INCLUDE [storage-auth-resource-scope-include](../../../includes/storage-auth-resource-scope-include.md)]

## <a name="access-data-with-an-azure-ad-account"></a>Bir Azure AD hesabı ile verilere erişme

Azure portalı, PowerShell aracılığıyla blob veya kuyruğa verilere erişimi veya Azure CLI yetkilendirilmiş kullanıcının Azure AD hesabı kullanarak ya da hesap erişim anahtarlarını (paylaşılan anahtar yetkilendirme) kullanarak.

### <a name="data-access-from-the-azure-portal"></a>Azure Portalı'ndan veri erişimi

Azure portalında bir Azure depolama hesabındaki blob ve kuyruk verilere erişmek için Azure AD hesabı ya da hesap erişim anahtarlarını kullanabilirsiniz. Azure portalını kullanır hangi Yetkilendirme düzeni, size atanan RBAC rolleri bağlıdır.

Blob veya kuyruğa veri erişmeyi denediğinde, Azure portalında ilk, ile bir RBAC rolü atanmış olup olmadığını denetler **Microsoft.Storage/storageAccounts/listkeys/action**. Bu eylem sahip bir role atanmış, Azure Portalı aracılığıyla paylaşılan anahtar yetkilendirme blob ve kuyruk verilerine erişmek için hesap anahtarı kullanır. Bu eylemle bir rolü atanmamış, Azure portalı, Azure AD hesabı kullanarak verilere erişmeye çalışır.

BLOB veya kuyruğa erişmek için veri, Azure AD hesabı kullanılarak Azure portalından, blob ve kuyruk verilere erişmek için izinleri gerekir ve ayrıca Azure portalında depolama hesabı kaynaklarına gezinmek için izinler gerekir. Azure Depolama tarafından sağlanan yerleşik roller blob ve kuyruk kaynaklara erişim, ancak depolama hesabı kaynaklarına izinleri yok. Bu nedenle, portal ayrıca bir Azure Resource Manager rol atamasını gibi erişmesi [okuyucu](../../role-based-access-control/built-in-roles.md#reader) rolü, depolama hesabının veya daha yüksek düzeyine kapsamlı. **Okuyucu** rolü en kısıtlı izinler verir, ancak depolama hesabı yönetim kaynaklarına erişim veren başka bir Azure Resource Manager rol de kullanılabilir. Azure portalında bir Azure AD hesabı ile veri erişimi için kullanıcılara izinler atama hakkında daha fazla bilgi için bkz. [verilere Azure blob ve kuyruk RBAC ile Azure portalında erişim ver](storage-auth-aad-rbac-portal.md).

Azure portalı hangi Yetkilendirme düzeni gösterir kullanılıyor, bir kapsayıcı veya kuyruğa gittiğinizde. Portalı'nda veri erişimi hakkında daha fazla bilgi için bkz. [blob veya sıra verilerinize erişmek için Azure portalını](storage-access-blobs-queues-portal.md).

### <a name="data-access-from-powershell-or-azure-cli"></a>Veri erişimi PowerShell veya Azure CLI

Azure CLI ve PowerShell, Azure AD kimlik bilgileriyle oturum destekler. Oturum açtıktan sonra oturumunuz bu kimlik bilgileri altında çalışır. Daha fazla bilgi için bkz. [blob veya sıra verilerinize erişmek için Azure CLI'yı çalıştırmak veya PowerShell komutları Azure AD kimlik bilgileriyle](storage-auth-aad-script.md).

## <a name="azure-ad-authorization-over-smb-for-azure-files"></a>Azure dosyaları için SMB üzerinden Azure AD yetkilendirme

Azure dosyaları SMB üzerinden Azure AD ile yetkilendirme etki alanına katılmış sanal makineleri için yalnızca (Önizleme) destekler. Azure dosyaları için SMB üzerinden Azure AD kullanma hakkında bilgi edinmek için [SMB üzerinden Azure dosyaları (Önizleme) genel bakış, Azure Active Directory yetkilendirme](../files/storage-files-active-directory-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynakları için BLOB'ları ve kuyrukları ile Azure Active Directory ve yönetilen kimlikleri erişim yetkisi verme](storage-auth-aad-msi.md)
- [Bloblara ve kuyruklara erişmek için bir uygulamadan Azure Active Directory ile kimlik doğrulaması yapma](storage-auth-aad-app.md)
- [Azure depolama desteği için Azure Active Directory tabanlı erişim denetimi genel kullanıma sunuldu](https://azure.microsoft.com/blog/azure-storage-support-for-azure-ad-based-access-control-now-generally-available/)
