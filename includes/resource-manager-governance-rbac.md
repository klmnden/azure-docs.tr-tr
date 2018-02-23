---
title: "include dosyası"
description: "include dosyası"
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/16/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 24863e239620f84a57c631b3ecf5b08997de31c5
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
Kuruluşunuzdaki kullanıcıların bu kaynaklara erişim doğru düzeyde sahip olduğunuzdan emin olmak istersiniz. Sınırsız erişimi kullanıcılara vermek istediğiniz yoktur, ancak işlerini yapmak için emin olmanız gerekir. Rol tabanlı erişim denetimi (RBAC), hangi kullanıcıların belirli eylemleri bir kapsamda tamamlamak için izni yönetmenizi sağlar. Bir rolü izin verilen eylemleri kümesini tanımlar. Bir kapsama rolü atayın ve bu rol kapsam için hangi kullanıcıların ait belirtin.

Erişim denetimi stratejinizi planlarken, kullanıcıların işlerini yapmak için en düşük öncelik verin. Aşağıdaki resimde RBAC atamak için önerilen bir düzeni gösterilir.

![Kapsam](./media/resource-manager-governance-rbac/role-examples.png)

Tüm kaynaklar için - sahibi, katkıda bulunan ve okuyucu geçerli üç rol vardır. Sahip rolü atanmış herhangi bir hesabı sıkı denetlenen ve nadiren kullanılır. Yalnızca çözüm durumunu izlemek için olması gereken kullanıcılar okuyucu rolüne verilmelidir.

Çoğu kullanıcılara verilen [kaynağa özel rollere](../articles/active-directory/role-based-access-built-in-roles.md) veya [özel roller](../articles/active-directory/role-based-access-control-custom-roles.md) abonelik veya kaynak grubu düzeyinde. Bu rolleri sıkı bir şekilde izin verilen eylemleri tanımlar. Bu rollere kullanıcıları atayarak çok fazla denetim sorgulamasına olmayan kullanıcılar için gerekli erişim verin. Birden fazla rol için bir hesap atayabilirsiniz ve bu kullanıcı rollerinin birleşik izinleri alır. Kaynak düzeyinde erişim verilmesi kullanıcılar için çok kısıtlayıcı görülür, ancak belirli bir görev için tasarlanmış otomatik bir işlem için çalışabilir.

### <a name="who-can-assign-roles"></a>Kimin roller atayabilirsiniz

Oluşturma ve rol atamalarını kaldırmak için kullanıcıların olmalıdır `Microsoft.Authorization/roleAssignments/*` erişim. Bu erişim sahibi veya kullanıcı erişimi yöneticisi rolleri aracılığıyla verilir.