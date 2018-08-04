---
title: include dosyası
description: include dosyası
services: container-registry
author: mmacy
ms.service: container-registry
ms.topic: include
ms.date: 08/03/2018
ms.author: marsma
ms.custom: include file
ms.openlocfilehash: 2174ae44f8e78763c1939aee5e6b86c95a0924ce
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39513726"
---
## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Kapsayıcı kayıt defterinizde erişimi olan bir hizmet sorumlusu oluşturmak için aşağıdaki betiği çalıştırın [Azure Cloud Shell](../articles/cloud-shell/overview.md) veya yerel bir yüklemesini [Azure CLI](/cli/azure/install-azure-cli). Komut, Bash kabuğunda biçimlendirilir.

Betiği çalıştırmadan önce güncelleştirme `ACR_NAME` değişkenini kapsayıcı kayıt defterinizin adıyla. `SERVICE_PRINCIPAL_NAME` Değeri, Azure Active Directory kiracınızda benzersiz olmalıdır. Alırsanız, bir "`'http://acr-service-principal' already exists.`" hatası, hizmet sorumlusu için farklı bir ad belirtin.

İsteğe bağlı olarak değiştirebileceğiniz `--role` değerini [az ad sp create-for-rbac] [ az-ad-sp-create-for-rbac] farklı izinleri vermek istiyor komutu.

Betiği çalıştırdıktan sonra hizmet sorumlusunun Not **kimliği** ve **parola**. Kimlik bilgilerini aldıktan sonra uygulamalarınızı ve hizmetlerinizi kapsayıcı kayıt defterinizde hizmet sorumlusu olarak kimlik doğrulaması için yapılandırabilirsiniz.

<!-- https://github.com/Azure-Samples/azure-cli-samples/blob/master/container-registry/service-principal-create/service-principal-create.sh --> [!code-azurecli-interactive[acr-sp-create](~/cli_scripts/container-registry/service-principal-create/service-principal-create.sh)]

## <a name="use-an-existing-service-principal"></a>Mevcut bir hizmet sorumlusunu kullanın

Mevcut bir hizmet sorumlusu için kayıt defteri erişim vermek için hizmet sorumlusu için yeni bir rol atamanız gerekir. Yeni hizmet sorumlusu oluşturma gibi ile çekme, gönderme ve çekme ve sahibi erişim izni verebilirsiniz.

Aşağıdaki betik [az rol ataması oluşturma] [ az-role-assignment-create] vermek için komut *çekme* bir hizmet sorumlusu izinleri belirttiğiniz `SERVICE_PRINCIPAL_ID` değişkeni. Ayarlama `--role` farklı düzeyde bir erişim vermek istiyorsanız, değer.

<!-- https://github.com/Azure-Samples/azure-cli-samples/blob/master/container-registry/service-principal-assign-role/service-principal-assign-role.sh --> [!code-azurecli-interactive[acr-sp-role-assign](~/cli_scripts/container-registry/service-principal-assign-role/service-principal-assign-role.sh)]

<!-- LINKS - Internal -->
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az_ad_sp_create_for_rbac
[az-role-assignment-create]: /cli/azure/role/assignment#az_role_assignment_create
