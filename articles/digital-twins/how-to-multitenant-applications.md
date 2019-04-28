---
title: Azure dijital çiftleri ile çok kiracılı uygulamalar'ı etkinleştirme | Microsoft Docs
description: Azure dijital çiftleri için çok kiracılı bir Azure Active Directory uygulamaları nasıl yapılandıracağınızı öğrenmek.
author: mavoge
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/03/2019
ms.author: mavoge
ms.openlocfilehash: 2b4f9bf87122f047e496dca1dbd425db8ad7c16c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60926060"
---
# <a name="enable-multitenant-applications-with-azure-digital-twins"></a>Azure dijital çiftleri ile çok kiracılı uygulamaları etkinleştirin

Azure üzerinde dijital İkizlerini oluşturan çözümleri geliştiriciler, tek bir hizmet veya çözüm birden çok müşteriyi desteklemek istediğinizi fark edebilirsiniz. Aslında, *çok kiracılı* en yaygın Azure dijital İkizlerini yapılandırmaları arasında uygulamalardır.

Bu belgede, çeşitli Azure Active Directory kiracıları ve müşterileri desteklemek için bir Azure dijital İkizlerini uygulamasını yapılandırma açıklanmaktadır.

## <a name="multitenancy"></a>Çoklu müşteri mimarisi

A *çok kiracılı* birden çok müşteriyi destekleyen tek bir sağlanan örnek bir kaynaktır. Her müşteriye kendi bağımsız veri ve ayrıcalıkları vardır. Böylece farklı olan kendi "uygulamayı görüntüle" her müşteri deneyimi birbirinden ait ayrı tutulur.

Çoklu müşteri mimarisi hakkında daha fazla bilgi edinmek için [azure'da çok Kiracılı uygulamalar](https://docs.microsoft.com/azure/dotnet-develop-multitenant-applications).

## <a name="problem-scenario"></a>Sorunu senaryosu

Bu senaryoda, bir Azure dijital İkizlerini çözümünüzü oluşturmaya bir geliştirici göz önünde bulundurun (**Geliştirici**) ve bu çözümü kullanan bir müşteri (**müşteri**):

- **Geliştirici** Azure Active Directory kiracısı ile bir Azure aboneliği de mevcuttur.
- **Geliştirici** bir Azure aboneliğine dijital İkizlerini Azure örneğine dağıtır. Azure Active Directory otomatik olarak oluşturulan hizmet sorumlusu **Geliştirici**kullanıcının Azure Active Directory kiracısı.
- İçindeki kullanıcılar **Geliştirici**kullanıcının Azure Active Directory kiracısı için ardından [OAuth 2.0 belirteç edinme](./security-authenticating-apis.md) dijital İkizlerini Azure hizmetinden.
- **Geliştirici** artık doğrudan Azure dijital İkizlerini yönetim API'leri ile tümleşen mobil bir uygulama oluşturur.
- **Geliştirici** sağlayan **müşteri** mobil uygulama kullanımını.
- **Müşteri** Azure dijital İkizlerini yönetim API'si içinde kullanmak için yetkilendirilmesi gerekir **Geliştirici**ait uygulama.

Sorun:

- Zaman **müşteri** oturum açtığı **Geliştirici**ait uygulama, uygulama olamaz almak için belirteçleri **müşteri**ait Azure dijital İkizlerini yönetim API'leri ile kimlik doğrulaması için kullanıcılar.
- Azure dijital İkizlerini içinde tanınmıyor gösteren Azure Active Directory'de verilen bir özel durum **müşteri**ait dizin.

## <a name="problem-solution"></a>Sorunun çözümü

Önceki sorun senaryoyu çözmek için aşağıdaki eylemleri Azure ile dijital bir İkizlerini içinde hizmet sorumlusu oluşturmak için gereken **müşteri**kullanıcının Azure Active Directory kiracısı:

- Varsa **müşteri** zaten bir Azure Active Directory kiracısı ile bir Azure aboneliğine sahip değil:

  - **Müşteri**kullanıcının Azure Active Directory Kiracı yöneticisiyseniz gerekir almak bir [Kullandıkça Öde Azure aboneliğine](https://azure.microsoft.com/offers/ms-azr-0003p/).
  - **Müşteri**gerekir sonra kullanıcının Azure Active Directory Kiracı yöneticisiyseniz [, kiracılarının yeni aboneliği ile bağlantı](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity).

- Üzerinde [Azure portalında](https://portal.azure.com), **müşteri**kullanıcının Azure Active Directory Kiracı yöneticisiyseniz, aşağıdaki adımları alır:

  1. Açık **abonelikleri**.
  1. Kullanılacak Azure Active Directory kiracısına sahip bir abonelik seçin **Geliştirici**ait uygulama.

     ![Azure Active Directory aboneliği][1]

  1. Seçin **kaynak sağlayıcıları**.
  1. Arama **Microsoft.IoTSpaces**.
  1. **Kaydol**’u seçin.

     ![Azure Active Directory kaynak sağlayıcıları][2]
  
## <a name="next-steps"></a>Sonraki adımlar

- Kullanıcı tanımlı işlevler, Azure ile dijital İkizlerini kullanma hakkında daha fazla bilgi edinmek için [Azure dijital İkizlerini kullanıcı tanımlı işlevler oluşturma](./how-to-user-defined-functions.md).

- Daha fazla uygulama rolü atamalarını ile güvenli hale getirmek için rol tabanlı erişim denetimi kullanmayı öğrenmek için [nasıl oluşturulup Azure dijital İkizlerini rol tabanlı erişim denetimini yönetme](./security-create-manage-role-assignments.md).

<!-- Images -->
[1]: media/multitenant/ad-subscriptions.png
[2]: media/multitenant/ad-resource-providers.png
