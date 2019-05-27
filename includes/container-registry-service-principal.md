---
title: include dosyası
description: include dosyası
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 12/14/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 69951693f9d3bacb556453aba954620815884d43
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66152230"
---
## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Kapsayıcı kayıt defterinizde erişimi olan bir hizmet sorumlusu oluşturmak için aşağıdaki betiği çalıştırın [Azure Cloud Shell](../articles/cloud-shell/overview.md) veya yerel bir yüklemesini [Azure CLI](/cli/azure/install-azure-cli). Komut, Bash kabuğunda biçimlendirilir.

Betiği çalıştırmadan önce güncelleştirme `ACR_NAME` değişkenini kapsayıcı kayıt defterinizin adıyla. `SERVICE_PRINCIPAL_NAME` Değeri, Azure Active Directory kiracınızda benzersiz olmalıdır. Alırsanız, bir "`'http://acr-service-principal' already exists.`" hatası, hizmet sorumlusu için farklı bir ad belirtin.

İsteğe bağlı olarak değiştirebileceğiniz `--role` değerini [az ad sp create-for-rbac] [ az-ad-sp-create-for-rbac] farklı izinleri vermek istiyor komutu. Rolleri tam bir listesi için bkz. [ACR rolleri ve izinleri](https://github.com/Azure/acr/blob/master/docs/roles-and-permissions.md).

Betiği çalıştırdıktan sonra hizmet sorumlusunun Not **kimliği** ve **parola**. Kimlik bilgilerini aldıktan sonra uygulamalarınızı ve hizmetlerinizi kapsayıcı kayıt defterinizde hizmet sorumlusu olarak kimlik doğrulaması için yapılandırabilirsiniz.

<!-- https://github.com/Azure-Samples/azure-cli-samples/blob/master/container-registry/service-principal-create/service-principal-create.sh -->
[!code-azurecli-interactive[acr-sp-create](~/cli_scripts/container-registry/service-principal-create/service-principal-create.sh)]

## <a name="use-an-existing-service-principal"></a>Mevcut bir hizmet sorumlusunu kullanın

Mevcut bir hizmet sorumlusu için kayıt defteri erişim vermek için hizmet sorumlusu için yeni bir rol atamanız gerekir. Yeni hizmet sorumlusu oluşturma gibi ile çekme, gönderme ve çekme ve diğerlerinin yanı sıra sahip erişimi verebilirsiniz.

Aşağıdaki betik [az rol ataması oluşturma] [ az-role-assignment-create] vermek için komut *çekme* bir hizmet sorumlusu izinleri belirttiğiniz `SERVICE_PRINCIPAL_ID` değişkeni. Ayarlama `--role` farklı düzeyde bir erişim vermek istiyorsanız, değer.


<!-- https://github.com/Azure-Samples/azure-cli-samples/blob/master/container-registry/service-principal-assign-role/service-principal-assign-role.sh -->
[!code-azurecli-interactive[acr-sp-role-assign](~/cli_scripts/container-registry/service-principal-assign-role/service-principal-assign-role.sh)]

<!-- LINKS - Internal -->
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
