---
title: Azure dijital çiftleri ile çok kiracılı uygulamalar'ı etkinleştirme | Microsoft Docs
description: Müşterilerinizin Azure Active Directory kiracıları kaydetme Azure ile dijital İkizlerini anlama
author: mavoge
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: mavoge
ms.openlocfilehash: a2d9ece119003c341f49ee03d735d5636b179a32
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51259896"
---
# <a name="enable-multitenant-applications-with-azure-digital-twins"></a>Azure dijital çiftleri ile çok kiracılı uygulamaları etkinleştirin

Azure dijital İkizlerini genellikle kullanan geliştiriciler, çok kiracılı uygulamalar oluşturmak istiyorsunuz. A *çok kiracılı uygulama* birden çok müşteriyi destekleyen tek bir sağlanan örneği. Her müşteriye kendi bağımsız veri ve ayrıcalıkları vardır.

Bu belgede, çeşitli Azure Active Directory (Azure AD) kiracılar ve müşterilerin destekleyen Azure dijital İkizlerini çok kiracılı uygulama oluşturma işlemi açıklanmaktadır.

## <a name="scenario-summary"></a>Senaryo özeti

Bu senaryoda, geliştirici D ve müşteri C: göz önünde bulundurun

- Geliştirici D, Azure AD kiracısı ile bir Azure aboneliğine sahip olur.
- Bir Azure aboneliğine dijital İkizlerini Azure örneğine Geliştirici D dağıtır.
- Azure AD hizmet sorumlusu Geliştirici D'ın Azure AD kiracısında oluşturduğundan, geliştirici D'ın Azure AD Kiracı içindeki kullanıcıları Azure dijital İkizlerini hizmetinde belirteç alabilir.
- Geliştirici D artık doğrudan Azure dijital İkizlerini yönetim API'si ile tümleşen mobil bir uygulama oluşturur.
- Geliştirici D müşteri C mobil uygulama kullanımına izin verir.
- Müşteri C Azure dijital İkizlerini yönetim API'si Geliştirici D'ın uygulama içinde kullanmak için yetkilendirilmesi gerekir.

  > [!IMPORTANT]
  > - Müşteri C Geliştirici D'ın uygulamasına oturum açtığında, uygulama belirteçleri yönetim API'sine konuşmak müşteri C'ın kullanıcılar için alınamıyor.
  > - Azure AD, Azure dijital İkizlerini müşteri C'ın dizininde tanınmıyorsa belirten bir hata oluşturur.

## <a name="solution"></a>Çözüm

Önceki senaryoyu çözmek için bir Azure dijital İkizlerini müşteri C'ın Azure AD kiracısı içinde hizmet sorumlusu oluşturmak için aşağıdaki eylemler gereklidir:

- Müşteri C zaten bir Azure AD kiracısı ile bir Azure aboneliğine sahip değilse:

  - Müşteri C'ın Azure AD Kiracı Yöneticisi gerekir almak bir [Kullandıkça Öde Azure aboneliğine](https://azure.microsoft.com/offers/ms-azr-0003p/).
  - Müşteri C'ın Azure AD Kiracı Yöneticisi ardından gerekir [, kiracılarının yeni aboneliği ile bağlantı](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

- Üzerinde [Azure portalında](https://portal.azure.com), müşteri C'ın Azure AD Kiracı Yöneticisi aşağıdaki adımları alır:

  1. Açık **abonelikleri**.
  1. Geliştirici D'ın uygulamada kullanılmak üzere Azure AD kiracısına sahip bir abonelik seçin.
  1. Seçin **kaynak sağlayıcıları**.
  1. Arama **Microsoft.IoTSpaces**.
  1. **Kaydol**’u seçin.
  
## <a name="next-steps"></a>Sonraki adımlar

Kullanıcı tanımlı işlevler, Azure ile dijital İkizlerini kullanma hakkında daha fazla bilgi edinmek için [Azure dijital İkizlerini UDF'ler](how-to-user-defined-functions.md).

Daha fazla uygulama rolü atamalarını ile güvenli hale getirmek için rol tabanlı erişim denetimi kullanmayı öğrenmek için [Azure dijital İkizlerini rol tabanlı erişim denetimi](security-create-manage-role-assignments.md).
