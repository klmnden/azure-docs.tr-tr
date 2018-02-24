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
ms.openlocfilehash: 0cb3de7d893ccfe638468110b1b6f5fb61b2bc7c
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
Kaynak kilitleri yanlışlıkla silinmesi ya da kritik kaynaklara değiştirme kuruluşunuzdaki kullanıcıların engelleyin. Rol tabanlı erişim denetimi farklı olarak, tüm kullanıcılar ve roller bir kısıtlama kaynak kilitleri uygulayın. 

Kilit düzeyini ayarlayabilirsiniz **CanNotDelete** veya **salt okunur**. Portalı'nda kilitleri düzeyleri olarak görüntülenen **silmek** ve **salt okunur** sırasıyla.

* **CanNotDelete** yetkili kullanıcıların, hala okuyun ve kaynak değiştirebilirsiniz, ancak bir kaynağı silemezsiniz anlamına gelir. 
* **Salt okunur** yetkili kullanıcılar, bir kaynak okuyabilir, ancak silebilir veya kaynak güncelleştirme anlamına gelir. Bu kilit uygulama benzer tüm yetkili kullanıcılar tarafından verilen izinleri kısıtlamak için **okuyucu** rol. 

> [!TIP]
> Uygularken dikkatli olun bir **ReadOnly** kilitle. İşlemler gerçekten ek eylemleri gerektirir göründüğü gibi bazı işlemler okuyun. Örneğin, bir **ReadOnly** kilit bir depolama hesabında anahtarları listeleme tüm kullanıcıları engeller. Yazma işlemlerini listenin döndürülen anahtarları için kullanılabilir olmadığından anahtarları işlemi bir POST isteği gerçekleştirilir. A **ReadOnly** kilit bir uygulama hizmeti kaynağına yazma erişimi bu etkileşimi gerektirmesi nedeniyle kaynak dosyalarını görüntüleme Visual Studio Sunucu Gezgini engeller.

Bir üst kapsamda kilit uyguladığınızda, kapsamı içindeki tüm kaynakların aynı kilit devralır. Daha sonra eklediğiniz bile kaynakları kilidi üst devralır. Devralmada en kısıtlayıcı kilidi önceliklidir.

Resource Manager kilitleri uygulamak gönderilen işlemleri oluşan yönetim düzeyi gerçekleşen işlemlerine `https://management.azure.com`. Kilitler kaynakları kendi işlevleri işleminin nasıl kısıtlama yok. Kaynak kısıtlı değişir, ancak kaynak işlemlerinin sınırlı değildir. Örneğin, bir SQL veritabanı salt okunur kilit, silme veya veritabanı değiştirme engeller. Oluşturma, güncelleştirme ya da veritabanındaki verileri silme engellemez. Bu işlemler için gönderilmediği için veri hareketlerini izin verilen `https://management.azure.com`.

### <a name="who-can-create-or-delete-locks-in-your-organization"></a>Kimin oluşturmak veya kuruluşunuzdaki kilitleri silmek mi
Oluşturmak veya yönetim kilitleri silmek için erişimi olmalıdır `Microsoft.Authorization/locks/*` eylemler. Yerleşik roller, yalnızca **sahibi** ve **kullanıcı erişimi Yöneticisi** bu eylemleri verilir.
