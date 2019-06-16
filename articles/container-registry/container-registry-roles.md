---
title: Azure Container Registry - roller ve izinler
description: Azure rol tabanlı erişim denetimi (RBAC) ve kimlik ve erişim yönetimi (IAM), Azure container registry kaynakları için ayrıntılı izinler sağlamak için kullanın.
services: container-registry
author: dlepow
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 03/20/2019
ms.author: danlep
ms.openlocfilehash: d62dd6c65975d63a0127bb5dd1c62cd741b59ac6
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67067994"
---
# <a name="azure-container-registry-roles-and-permissions"></a>Azure Container Registry rolleri ve izinleri

Azure Container Registry hizmeti farklı bir Azure container registry'ye izin düzeyleri sağlayan Azure rolleri, bir kümesini destekler. Azure kullanan [rol tabanlı erişim denetimi](../role-based-access-control/index.yml) kullanıcılara özel izinler atamak veya bir kayıt defteriyle etkileşimli çalışmak için gereken sorumluları hizmetini (RBAC).

| Rol/izni       | [Access Resource Manager](#access-resource-manager) | [Kayıt defteri oluşturma/silme](#create-and-delete-registry) | [Görüntü gönderme](#push-image) | [Görüntü çekme](#pull-image) | [Görüntü verilerini sil](#delete-image-data) | [İlkeleri değiştirme](#change-policies) |   [Oturum görüntüleri](#sign-images)  |
| ---------| --------- | --------- | --------- | --------- | --------- | --------- | --------- |
| Sahip | X | X | X | X | X | X |  |  
| Katılımcı | X | X | X |  X | X | X |  |  
| Okuyucu | X |  |  |  |  |  |  |
| AcrPush |  |  | X | X | |  |  |  
| AcrPull |  |  |  | X |  |  |  |  
| AcrDelete |  |  |  |  | X |  |  |
| AcrImageSigner |  |  |  |  |  |  | X |

## <a name="differentiate-users-and-services"></a>Kullanıcıların ve hizmetlerin ayırt

Bir kişi ya da hizmet, bir görevi gerçekleştirmek izinlerini en sınırlı sayıda sağlamak için en iyi uygulama, hiçbir zaman izin uygulanır. Aşağıdaki izin kümeleri bir dizi insanlar ve gözetimsiz Hizmetleri tarafından kullanılan bir özelliği temsil eder.

### <a name="cicd-solutions"></a>CI/CD çözümleri

Otomatikleştirirken `docker build` komutları CI/CD çözümlerinden ihtiyacınız `docker push` özellikleri. Bu gözetimsiz hizmet senaryolar için atama olan öneririz **AcrPush** rol. Bu rol, daha geniş aksine **katkıda bulunan** rolü, başka bir kayıt defteri işlemlerini gerçekleştirme veya Azure Resource Manager erişim hesabı engeller.

### <a name="container-host-nodes"></a>Kapsayıcı konak düğümleri

Benzer şekilde, kapsayıcılarınızı çalışan düğümleri gereken **AcrPull** rolü gerekmez ancak **okuyucu** özellikleri.

### <a name="visual-studio-code-docker-extension"></a>Visual Studio kod Docker uzantısı

Visual Studio Code gibi araçları için [Docker uzantısını](https://code.visualstudio.com/docs/azure/docker), kullanılabilir Azure kapsayıcısı kayıt defterleri listelemek için ek kaynak sağlayıcısı erişimi gereklidir. Bu durumda, kullanıcıların erişimini sağlamak **okuyucu** veya **katkıda bulunan** rol. Bu roller izin `docker pull`, `docker push`, `az acr list`, `az acr build`ve diğer özellikleri. 

## <a name="access-resource-manager"></a>Resource Manager'a erişme

Azure Resource Manager erişim Azure portalı ve kayıt defteri yönetimi için gerekli [Azure CLI](/cli/azure/). Örneğin, kayıt defterleri listesini almak için `az acr list` komutu, bu izne ihtiyacınız ayarlayın. 

## <a name="create-and-delete-registry"></a>Oluşturma ve kayıt silme

Oluşturma ve Azure kapsayıcısı kayıt defterleri silme yeteneği.

## <a name="push-image"></a>Görüntü gönderme

Yeteneği `docker push` bir görüntü veya başka bir anında iletme [yapıt desteklenen](container-registry-image-formats.md) gibi bir kayıt defterine bir Helm grafiği. Gerektirir [kimlik doğrulaması](container-registry-authentication.md) yetkili kimlik kullanarak kayıt defteriyle. 

## <a name="pull-image"></a>Görüntü çekme

Yeteneği `docker pull` bir olmayan-karantinaya görüntü veya başka bir çekme [yapıt desteklenen](container-registry-image-formats.md) gibi bir kayıt defterinden bir Helm grafiği. Gerektirir [kimlik doğrulaması](container-registry-authentication.md) yetkili kimlik kullanarak kayıt defteriyle.

## <a name="delete-image-data"></a>Görüntü verilerini sil

Yeteneği [kapsayıcı görüntülerini silmek](container-registry-delete.md), veya diğer silme [yapıtları desteklenen](container-registry-image-formats.md) Helm grafikleri, bir kayıt defterinden gibi.

## <a name="change-policies"></a>İlkeleri değiştirme

Bir kayıt defterini ilkeleri yapılandırma olanağı. Karantina ve görüntü imzalama etkinleştirme görüntü temizleme ilkeleri içerir.

## <a name="sign-images"></a>Oturum görüntüleri

Bir hizmet sorumlusu kullanacağınız bir otomatik işlem genellikle atanan görüntü oturum yeteneği. Bu izni genellikle ile birlikte [anında iletme görüntü](#push-image) bir kayıt defterine güvenilen bir görüntüsü göndermeye izin vermek için. Ayrıntılar için bkz [içerik Azure Container Registry güvende](container-registry-content-trust.md).

## <a name="next-steps"></a>Sonraki adımlar

* Kullanarak bir Azure kimlik için RBAC rollerini atama hakkında daha fazla bilgi [Azure portalında](../role-based-access-control/role-assignments-portal.md), [Azure CLI](../role-based-access-control/role-assignments-cli.md), ya da diğer Azure Araçları.

* Hakkında bilgi edinin [kimlik doğrulama seçenekleri](container-registry-authentication.md) Azure Container Registry için.
