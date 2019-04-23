---
title: Erişim için Azure BLOB'ları ve kuyrukları Azure Active Directory'yi kullanarak kimlik doğrulaması | Microsoft Docs
description: Azure BLOB'ları ve kuyrukları Azure Active Directory'yi kullanarak kimlik doğrulaması yapmak.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/21/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 12598f3866cd1041cdf3cb89dac985b8d2caafce
ms.sourcegitcommit: c884e2b3746d4d5f0c5c1090e51d2056456a1317
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60148815"
---
# <a name="authenticate-access-to-azure-blobs-and-queues-using-azure-active-directory"></a>Erişim için Azure BLOB'ları ve kuyrukları Azure Active Directory'yi kullanarak kimlik doğrulaması

Azure depolama, Blob ve kuyruk hizmetlerine kimlik doğrulaması ve yetkilendirme ile Azure Active Directory (AD) destekler. Azure AD ile erişim vermek için kullanıcıların, grupların veya uygulama hizmet sorumluları için rol tabanlı erişim denetimi (RBAC) kullanabilirsiniz. 

Kullanıcılar veya uygulamalar Azure AD kimlik bilgilerini kullanarak kimlik doğrulaması yetkilendirme başka bir yolla üstün güvenlik ve kullanım kolaylığı sağlar. Paylaşılan anahtar yetkilendirme uygulamalarınızı kullanmaya devam ederken, Azure AD kullanarak kodunuzu ile hesap erişim anahtarını depolamak için gereken bozar. Depolama hesabınızdaki kaynaklara ayrıntılı erişim vermek için paylaşılan erişim imzaları (SAS) kullanmaya devam edebilirsiniz, ancak Azure AD'ye SAS belirteçlerini yönetin veya güvenliği aşılmış bir SAS iptal etme hakkında endişelenmenize gerek kalmadan benzer özellikleri sunar. Microsoft Azure depolama uygulamalarınız için mümkün olduğunda Azure AD kimlik doğrulaması kullanmanızı önerir.

Kimlik doğrulaması ve Azure AD kimlik bilgileriyle yetkilendirme tüm mevcut genel amaçlı ve Blob Depolama hesapları tüm ortak bölgelerde. Depolama hesapları yalnızca Azure Resource Manager dağıtım modeli desteği ile Azure AD yetkilendirme oluşturuldu.

## <a name="overview-of-azure-ad-for-blobs-and-queues"></a>BLOB'lar ve Kuyruklar için Azure AD genel bakış

Bir güvenlik sorumlusu (kullanıcı, Grup veya uygulama) blob veya kuyruğa bir kaynağa erişmeyi denediğinde bir blob için anonim erişimi kullanılabilir olmadığı sürece istek yetkilendirilmelidir. Azure AD ile bir kaynağa erişim iki adımlı bir işlemdir. İlk olarak, güvenlik sorumlusunun kimliği doğrulanır ve bir OAuth 2.0 belirteç döndürdü. Ardından, belirteci, bir isteğin bir parçası Blob veya kuyruk hizmetine geçirilen ve belirtilen kaynağa erişim yetkisi vermek için hizmet tarafından kullanılan.

Kimlik doğrulaması adımı için güvenlik ilkesi olan bir veya daha fazla RBAC rolleri için atanması gerektirir. Azure depolama, blob ve kuyruk veriler için ortak izin kümelerinin kapsayabilir ve RBAC rollerini sağlar. Bir güvenlik Sorumlusu'na atanan roller bu sorumlusunun sahip olacağı erişim belirleyin. Azure depolama için RBAC rollerini atama hakkında daha fazla bilgi edinmek için [RBAC ile depolama verilere erişim haklarını yönetme](storage-auth-aad-rbac.md).

Bir uygulama çalışma zamanında bir OAuth 2.0 erişim belirteci isteklerini yetkilendirme adım gerektirir. Bir uygulamayı bir Azure VM, sanal makine ölçek kümesi veya bir Azure işlev uygulaması gibi Azure varlık içinde çalıştıran, kullanabilirsiniz bir [yönetilen kimliği](../../active-directory/managed-identities-azure-resources/overview.md) blob'lara erişim veya sıralara. Tarafından yönetilen bir kimlik Azure Blob veya kuyruk hizmetine yapılan isteklerin yetkilendirme konusunda bilgi almak için bkz: [erişim BLOB'lar ve Kuyruklar ile Azure Active Directory ve yönetilen kimlikleri için Azure kaynakları için kimlik doğrulaması](storage-auth-aad-msi.md).

De yerel uygulamalar ve Azure Blob veya sıra hizmete isteklerde web uygulamaları Azure AD ile kimlik doğrulaması yapabilir. Bir erişim belirteci isteği ve blob veya kuyruğa veri istekleri yetkilendirmek için kullanma hakkında bilgi edinmek için [Azure AD'den bir Azure depolama uygulaması ile kimlik doğrulama](storage-auth-aad-app.md).

## <a name="assigning-rbac-roles-for-access-rights"></a>RBAC rolleri için erişim hakları atama

Azure Active Directory (Azure AD) ile güvenli kaynaklara erişim hakları yetkilendirir [rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/overview.md). Azure depolama, bir dizi ortak blob ve kuyruk verilere erişmek için kullanılan izin kümelerini kapsayacak yerleşik RBAC rolleri tanımlar. Ayrıca, blob ve kuyruk verilere erişim için özel roller tanımlayabilirsiniz.

Bir RBAC rolü için bir Azure AD güvenlik sorumlusu atandığında, Azure verir, bir güvenlik sorumlusu için bu kaynaklara erişin. Abonelik, kaynak grubu, depolama hesabı veya bir kapsayıcının veya kuyruk düzeyi için erişimi sınırlayabilirsiniz. Bir Azure AD güvenlik sorumlusu olabilir bir kullanıcı, Grup, bir uygulama hizmet sorumlusu veya [yönetilen Azure kaynakları için kimliği](../../active-directory/managed-identities-azure-resources/overview.md).

### <a name="built-in-rbac-roles-for-blobs-and-queues"></a>BLOB'lar ve Kuyruklar için yerleşik RBAC rolleri

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

Yerleşik bir RBAC rolü bir güvenlik sorumlusu atama hakkında bilgi edinmek için aşağıdaki makalelerden birine bakın:

- [Azure blob ve kuyruk verilere RBAC ile Azure portalında erişim izni ver](storage-auth-aad-rbac-portal.md)
- [Azure CLI kullanarak RBAC ile Azure blob ve kuyruk verilere erişim izni ver](storage-auth-aad-rbac-cli.md)
- [PowerShell ile RBAC ile Azure blob ve kuyruk verilere erişim izni ver](storage-auth-aad-rbac-powershell.md)

Hakkında daha fazla bilgi için Azure depolama için yerleşik roller tanımlanır, bkz: [rol tanımlarını anlamak](../../role-based-access-control/role-definitions.md#management-and-data-operations-preview). Özel bir RBAC rollerini oluşturma hakkında daha fazla bilgi için bkz: [Azure rol tabanlı erişim denetimi için özel roller oluşturma](../../role-based-access-control/custom-roles.md).

### <a name="access-permissions-for-data-operations"></a>Veri işlemleri için erişim izinleri

Belirli Blob veya kuyruk hizmet işlemlerini aramak üzere gereken izinler hakkında daha fazla bilgi için bkz: [blob ve kuyruk veri işlemleri çağırmak için izinleri](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations).

## <a name="resource-scope"></a>Kaynak kapsamı

[!INCLUDE [storage-auth-resource-scope-include](../../../includes/storage-auth-resource-scope-include.md)]

## <a name="access-data-with-an-azure-ad-account"></a>Bir Azure AD hesabı ile verilere erişme

Azure portalı, PowerShell aracılığıyla blob veya kuyruğa verilere erişimi veya Azure CLI yetkilendirilmiş kullanıcının Azure AD hesabı kullanarak ya da hesap erişim anahtarlarını (paylaşılan anahtar kimlik doğrulaması) kullanarak.

### <a name="data-access-from-the-azure-portal"></a>Azure Portalı'ndan veri erişimi

Azure portalında bir Azure depolama hesabındaki blob ve kuyruk verilere erişmek için Azure AD hesabı ya da hesap erişim anahtarlarını kullanabilirsiniz. Azure portalını kullanır hangi Yetkilendirme düzeni, size atanan RBAC rolleri bağlıdır.

Blob veya kuyruğa veri erişmeyi denediğinde, Azure portalında ilk, ile bir RBAC rolü atanmış olup olmadığını denetler **Microsoft.Storage/storageAccounts/listkeys/action**. Bu eylem sahip bir role atanmış, Azure Portalı aracılığıyla paylaşılan anahtar yetkilendirme blob ve kuyruk verilerine erişmek için hesap anahtarı kullanır. Bu eylemle bir rolü atanmamış, Azure portalı, Azure AD hesabı kullanarak verilere erişmeye çalışır.

Azure portalından Azure AD hesabınızla BLOB veya kuyruğa verilere erişmek için aşağıdaki deyimleri hem de sizin için doğru olması gerekir:

- Azure Resource Manager size atandı [okuyucu](../../role-based-access-control/built-in-roles.md#reader) rol, en azından, depolama hesabının düzeyine kapsamlı ya da daha yüksek. **Okuyucu** rolü en kısıtlı izinler verir, ancak depolama hesabı yönetim kaynaklarına erişim veren başka bir Azure Resource Manager rol de kullanılabilir.
- Ya da blob veya kuyruğa verilere erişim sağlayan yerleşik veya özel RBAC rolü atandı.

Azure portalı, bir kapsayıcı veya kuyruğa gittiğinizde hangi düzeni kullanımda gösterir. Portalı'nda veri erişimi hakkında daha fazla bilgi için bkz. [blob veya sıra verilerinize erişmek için Azure portalını](storage-access-blobs-queues-portal.md).

Azure portalında bir Azure AD hesabı ile veri erişimi için kullanıcılara izinler atama hakkında daha fazla bilgi için bkz. [verilere Azure blob ve kuyruk RBAC ile Azure portalında erişim ver](storage-auth-aad-rbac-portal.md).

### <a name="data-access-from-powershell-or-azure-cli"></a>Veri erişimi PowerShell veya Azure CLI

Azure CLI ve PowerShell, Azure AD kimlik bilgileriyle oturum destekler. Oturum açtıktan sonra oturumunuz bu kimlik bilgileri altında çalışır. Daha fazla bilgi için bkz. [blob veya sıra verilerinize erişmek için Azure CLI'yı çalıştırmak veya PowerShell komutları Azure AD kimlik bilgileriyle](storage-auth-aad-script.md).

## <a name="azure-ad-authentication-over-smb-for-azure-files"></a>Azure dosyaları için SMB üzerinden Azure AD authentication

Azure dosyaları SMB üzerinden Azure AD ile kimlik doğrulaması etki alanına katılmış sanal makineleri için yalnızca (Önizleme) destekler. Azure dosyaları için SMB üzerinden Azure AD kullanma hakkında bilgi edinmek için [genel bakış, Azure Active Directory kimlik doğrulaması SMB üzerinden Azure dosyaları (Önizleme)](../files/storage-files-active-directory-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynakları için erişim BLOB'lar ve Kuyruklar ile Azure Active Directory ve yönetilen kimlikleri için kimlik doğrulaması](storage-auth-aad-msi.md)
- [Bloblara ve kuyruklara erişmek için bir uygulamadan Azure Active Directory ile kimlik doğrulaması yapma](storage-auth-aad-app.md)
- [Azure depolama desteği için Azure Active Directory tabanlı erişim denetimi genel kullanıma sunuldu](https://azure.microsoft.com/blog/azure-storage-support-for-azure-ad-based-access-control-now-generally-available/)
