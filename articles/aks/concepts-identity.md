---
title: Kavramlar - erişim ve kimlik Azure Kubernetes Hizmetleri (AKS)
description: Erişim ve kimlik Azure Kubernetes hizmeti (Azure Active Directory Tümleştirmesi, Kubernetes rol tabanlı erişim denetimi (RBAC) ve rolleri ve bağlamalar gibi AKS), hakkında bilgi edinin.
services: container-service
author: rockboyfor
ms.service: container-service
ms.topic: conceptual
origin.date: 02/28/2019
ms.date: 04/08/2019
ms.author: v-yeche
ms.openlocfilehash: 3432ba671431c25b7cd9ee58decc638861e884c3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60467056"
---
# <a name="access-and-identity-options-for-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) için erişim ve kimlik seçenekleri

Kimlik doğrulaması ve güvenli Kubernetes kümeleri için farklı yolu vardır. Rol tabanlı erişim denetimlerine (RBAC) kullanarak, kullanıcılar veya gruplar yalnızca ihtiyaç duydukları kaynakları erişimi verebilirsiniz. Azure Kubernetes Service (AKS) ile Azure Active Directory kullanarak daha fazla güvenlik ve izinler yapısı geliştirebilirsiniz. Bu yaklaşım, uygulama iş yüklerini ve Müşteri verilerinin güvenliğini sağlamanıza yardımcı olur.

Bu makalede yardımcı kavramları kimliğini doğrulamak ve AKS izinleri atama çekirdek sunar:

- [Kubernetes hizmet hesapları](#kubernetes-service-accounts)
- [Azure Active Directory Tümleştirmesi](#azure-active-directory-integration)
- [Rol tabanlı erişim denetimlerine (RBAC)](#role-based-access-controls-rbac)
- [Rolleri ve ClusterRoles](#roles-and-clusterroles)
- [RoleBindings ve ClusterRoleBindings](#rolebindings-and-clusterrolebindings)

## <a name="kubernetes-service-accounts"></a>Kubernetes hizmet hesapları

Kubernetes birincil kullanıcı türlerinde biridir bir *hizmet hesabı*. Bir hizmet hesabı var ve Kubernetes API tarafından yönetilir. Hizmet hesapları için kimlik bilgileri, API sunucusu ile iletişim kurmak için yetkili pod'ları tarafından kullanılacak veren, Kubernetes gizli diziler olarak depolanır. Çoğu API isteklerinin bir hizmet hesabı veya normal bir kullanıcı hesabı için bir kimlik doğrulama belirteci sağlayın.

Normal kullanıcı hesapları, İnsan yöneticilerin veya geliştiricilerin, yalnızca hizmetler ve işlemler için daha geleneksel erişim sağlar. Kubernetes kendi normal kullanıcı hesapları ve parolaları depolandığı bir kimlik yönetimi çözümü sağlamaz. Bunun yerine, dış kimlik çözümleri Kubernetes ile tümleştirilebilir. AKS kümeleri için Azure Active Directory bu tümleşik kimlik çözümüdür.

Kubernetes kimlik seçenekleri hakkında daha fazla bilgi için bkz. [Kubernetes kimlik doğrulaması][kubernetes-authentication].

## <a name="azure-active-directory-integration"></a>Azure Active Directory tümleştirmesi

İle tümleştirme, Azure Active Directory (AD) güvenlik AKS küme geliştirilebilir. Kurumsal kimlik yönetimi yıllardır üzerinde oluşturulan Azure AD, bir çok kiracılı, bulut tabanlı dizin ve temel Dizin Hizmetleri, uygulama erişim yönetimi ve kimlik koruması bir araya getiren Kimlik Yönetimi hizmetidir. Azure AD ile şirket içi kimlikleri, hesap yönetimi ve güvenlik için tek bir kaynak sağlamak için AKS kümeler halinde tümleştirebilirsiniz.

![AKS kümeleri ile Azure Active Directory Tümleştirmesi](media/concepts-identity/aad-integration.png)

Azure AD ile tümleşik AKS kümeleriyle kullanıcılar veya Kubernetes kaynakları bir ad alanındaki veya kümedeki grupları erişim izni verebilirsiniz. Elde etmek için bir `kubectl` yapılandırma kapsamındaki bir kullanıcı çalıştırabilirsiniz [az aks get-credentials] [ az-aks-get-credentials] komutu. Ne zaman bir kullanıcı ardından etkileşim kullanarak AKS kümesiyle `kubectl`, kendi Azure AD kimlik bilgilerinizle oturum açmanız istenir. Bu yaklaşım, kullanıcı hesabı yönetimi ve parola kimlik bilgileri için tek bir kaynak sağlar. Kullanıcı, yalnızca Küme Yöneticisi tarafından tanımlandığı gibi kaynaklara erişebilir.

Openıd Connect, OAuth 2.0 protokolünü üzerinde yerleşik bir kimlik katmanı AKS kümelerinde Azure AD kimlik doğrulaması kullanır. OAuth 2.0 elde edilir ve korunan kaynaklara erişim belirteçleri kullanma mekanizmasını tanımlar ve Openıd Connect kimlik doğrulaması için OAuth 2.0 Yetkilendirme işlemi bir genişletme olarak uygular. Openıd Connect hakkında daha fazla bilgi için bkz: [Open ID Connect belgeleri][openid-connect]. Openıd Connect ile Azure AD'den elde edilen kimlik doğrulama belirteçleri doğrulamak için AKS kümeleri Kubernetes Web kancası belirteci kimlik doğrulaması kullanın. Daha fazla bilgi için [Web kancası belirteci kimlik doğrulaması belgeleri][webhook-token-docs].

## <a name="role-based-access-controls-rbac"></a>Rol tabanlı erişim denetimlerine (RBAC)

Kullanıcıların gerçekleştirebileceği eylemleri ayrıntılı filtreleme sağlamak için rol tabanlı erişim denetimlerine (RBAC) Kubernetes kullanır. Bu denetim mekanizması kullanıcılara atamanıza olanak tanır veya yapma izni olan kullanıcı gruplarını oluşturmak veya kaynakları değiştirmek veya uygulama iş yüklerini çalışmasını günlüklerini görüntüleyin. Bu izinleri tek bir ad alanına kapsamlı veya tüm AKS kümeye verildi. Kubernetes RBAC ile oluşturduğunuz *rolleri* izinlerini tanımlamak ve kullanıcılara roller atama *rol bağlamaları*.

Daha fazla bilgi için [kullanarak RBAC yetkilendirme][kubernetes-rbac].

### <a name="azure-role-based-access-controls-rbac"></a>Azure rol tabanlı erişim denetimleri (RBAC)
Kaynaklara erişimi denetlemek için bir ek Azure rol tabanlı erişim denetimleri (RBAC) mekanizmadır. Kubernetes RBAC AKS kümenizi içindeki kaynaklara çalışacak şekilde tasarlanmıştır ve Azure RBAC, Azure aboneliğiniz kapsamındaki kaynaklar üzerinde çalışacak şekilde tasarlanmıştır. Azure RBAC ile oluşturduğunuz bir *rol tanımı* uygulanacak izinleri özetler. Bu özel bir rol tanımı sonra atanan bir kullanıcı veya grup *kapsam*, bir kaynak grubu veya abonelik arasında ayrı bir kaynak olabilir.

Daha fazla bilgi için [Azure RBAC nedir?][azure-rbac]

## <a name="roles-and-clusterroles"></a>Rolleri ve ClusterRoles

Kubernetes RBAC ile kullanıcılara izinler atamadan önce ilk olarak bu izinleri tanımladığınız bir *rol*. Kubernetes rolleri *vermek* izinleri. Kavramı yoktur. bir *Reddet* izni.

Rolleri, bir ad alanındaki izinleri vermek için kullanılır. Tüm küme arasında ya da belirtilen bir ad alanı dışında küme kaynaklarında izinleri vermek gerekiyorsa, bunun yerine kullanabileceğiniz *ClusterRoles*.

Bir ClusterRole kaynaklarıyla ilgili izinleri vermek için aynı şekilde çalışır, ancak tüm kümeye, belirli bir ad kaynaklarına uygulanabilir.

## <a name="rolebindings-and-clusterrolebindings"></a>RoleBindings ve ClusterRoleBindings

İle bu Kubernetes RBAC izinlerinin atadığınız kaynaklarıyla ilgili izinleri vermek için rol tanımlandıktan sonra bir *RoleBinding*. AKS kümenizi Azure Active Directory ile tümleştiriliyorsa, bu Azure AD kullanıcılarının kümedeki eylemleri gerçekleştirmek için izinleri nasıl verilir bağlamaları değil.

Rol bağlamaları, belirtilen bir ad alanı için rolleri atamak için kullanılır. Bu yaklaşım, kullanıcıları sadece kendi atanan ad alanında uygulama kaynaklarına erişmek mümkün olan tek bir AKS kümesi mantıksal olarak ayırmak sağlar. Rolleri arasında tüm küme veya küme kaynaklarında belirtilen bir ad alanı dışında bağlamak gerekiyorsa, bunun yerine kullanabileceğiniz *ClusterRoleBindings*.

Bir ClusterRoleBinding roller kullanıcılara bağlamak için aynı şekilde çalışır, ancak tüm kümeye, belirli bir ad kaynaklarına uygulanabilir. Bu yaklaşım Yöneticiler erişim izni veya tüm kaynaklara erişim mühendisleri AKS kümesinde desteği sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD ile kullanmaya başlamak için ve Kubernetes RBAC [Azure Active Directory Tümleştirme ile AKS][aks-aad].

İlişkili en iyi yöntemler için bkz: [en iyi uygulamalar için kimlik doğrulama ve yetkilendirme aks'deki][operator-best-practices-identity].

Çekirdek Kubernetes hakkında daha fazla bilgi ve AKS kavramlar için aşağıdaki makalelere bakın:

- [Kubernetes / AKS kümesi ve iş yükleri][aks-concepts-clusters-workloads]
- [Kubernetes / AKS güvenlik][aks-concepts-security]
- [Kubernetes / AKS sanal ağlar][aks-concepts-network]
- [Kubernetes / AKS depolama][aks-concepts-storage]
- [Kubernetes / AKS ölçeklendirin][aks-concepts-scale]

<!-- LINKS - External -->
[kubernetes-authentication]: https://kubernetes.io/docs/reference/access-authn-authz/authentication
[webhook-token-docs]: https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication
[kubernetes-rbac]: https://kubernetes.io/docs/reference/access-authn-authz/rbac/

<!-- LINKS - Internal -->
[openid-connect]: ../active-directory/develop/v1-protocols-openid-connect-code.md
[az-aks-get-credentials]: https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[azure-rbac]: ../role-based-access-control/overview.md
[aks-aad]: aad-integration.md
[aks-concepts-clusters-workloads]: concepts-clusters-workloads.md
[aks-concepts-security]: concepts-security.md
[aks-concepts-scale]: concepts-scale.md
[aks-concepts-storage]: concepts-storage.md
[aks-concepts-network]: concepts-network.md
[operator-best-practices-identity]: operator-best-practices-identity.md