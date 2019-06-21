---
title: include dosyası
description: include dosyası
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/16/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: f77a5d482c3f8632a3d86bd8e027fbb4418168c3
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67205322"
---
Kuruluşunuzdaki kullanıcıların bu kaynaklara erişmek için doğru düzeyde erişime sahip olduğundan emin olmak istersiniz. Kullanıcılara sınırsız erişim vermek istemezsiniz ancak işlerini halledebildiklerinden de emin olmanız gerekir. Rol tabanlı erişim denetimi (RBAC), hangi kullanıcıların belirli eylemleri bir kapsamda tamamlamak için izni yönetmenizi sağlar. Bir rol, bir dizi izin verilen eylemleri tanımlar. Rolü için bir kapsam atayın ve hangi kullanıcıların söz konusu rolü için kapsamı ait belirtin.

Erişim denetimi stratejinizi planlarken kullanıcılara yalnızca işlerini yapmak için gerekli olan ayrıcalığı tanıyın. Aşağıdaki resimde RBAC ataması için önerilen model gösterilmiştir.

![`Scope`](./media/resource-manager-governance-rbac/role-examples.png)

Tüm kaynaklar için - sahibi, katkıda bulunan ve okuyucu geçerli üç rol vardır. Sahip rolüne atanan tüm hesapları sıkı bir şekilde denetlenen ve nadiren kullanılır. Yalnızca çözüm durumunu izlemek için gereken kullanıcılar okuyucu rolü verilmiş olmalıdır.

Çoğu kullanıcı izni [kaynağa özel rollere](../articles/role-based-access-control/built-in-roles.md) veya [özel roller](../articles/role-based-access-control/custom-roles.md) abonelik veya kaynak grubu düzeyinde. Bu roller, sıkı bir şekilde izin verilen eylemleri tanımlayın. Bu roller kullanıcılara atayarak, çok fazla denetim erişimine izin verme kullanıcılar için gerekli erişim verin. Bir hesap için birden fazla rol atayabilir ve bu kullanıcı rollerinin birleşik izinleri alır. Kaynak düzeyinde erişim verme genellikle kullanıcılar için çok kısıtlayıcı ancak belirli bir görev için tasarlanmış otomatik bir işlem için çalışabilir.

### <a name="who-can-assign-roles"></a>Kimin rolleri atayabilirim miyim

Rol atamaları oluşturmak ve kaldırmak için kullanıcıların `Microsoft.Authorization/roleAssignments/*` erişimi olması gerekmektedir. Bu erişim, Sahip veya Kullanıcı Erişimi Yöneticisi rolleriyle verilir.