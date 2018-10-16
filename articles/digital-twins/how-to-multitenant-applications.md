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
ms.openlocfilehash: e465ab95b670268f8ef31472259783fce8f24b36
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49324370"
---
# <a name="how-to-enable-multitenant-applications-with-azure-digital-twins"></a>Azure dijital çiftleri ile çok kiracılı uygulamaları etkinleştirme

Azure dijital İkizlerini platformunu kullanan geliştiriciler, büyük olasılıkla çok kiracılı uygulamalar oluşturmak isteyeceksiniz. A *çok kiracılı uygulama* birden çok müşteriyi her biri kendi bağımsız veri ve ayrıcalıkları ile desteklemek için kullanılan tek bir sağlanan örneği.

Bu belgede, çeşitli Azure Active Directory (AD) kiracılar ve müşterilerin destekleyen Azure dijital İkizlerini çok kiracılı uygulama oluşturma işlemi açıklanmaktadır.

## <a name="scenario-summary"></a>Senaryo özeti

Bu senaryoda, geliştirici D ve müşteri C: göz önünde bulundurun

- Geliştirici D, Azure AD kiracısı ile bir Azure aboneliğine sahip olur.
- Geliştirici D, bir Azure aboneliğine dijital İkizlerini örneğine dağıtıldığını.
- Azure AD hizmet sorumlusu Geliştirici D'ın Azure AD kiracısında oluşturduğu beri Geliştirici D'ın Azure AD Kiracı içindeki kullanıcılar dijital İkizlerini hizmetinden belirteç alabilir.
- Geliştirici D artık doğrudan dijital İkizlerini yönetim API'si ile tümleşen mobil bir uygulama oluşturur.
- Geliştirici D müşteri C sonra mobil uygulama kullanımına izin verir.
- Şimdi, müşteri C Geliştirici D'ın uygulama içinde dijital İkizlerini yönetim API'sini kullanmak için yetkilendirilmesi gerekir.

  > [!IMPORTANT]
  > - Müşteri C Geliştirici D'ın uygulamasına oturum açtığında, uygulama yönetimi API'sine konuşmak müşteri C'ın kullanıcılar için belirteçlerini almak mümkün olmayacaktır.
  > - Azure AD ardından dijital İkizlerini müşteri C'ın dizininde tanınmıyorsa belirten bir hata oluşturur.

## <a name="solution"></a>Çözüm

Yukarıdaki senaryoyu çözmek için bir dijital İkizlerini hizmet sorumlusu müşteri C'ın Azure AD kiracısı içinde oluşturmak için aşağıdaki eylemler gereklidir:

- Müşteri C zaten Azure AD kiracısı ile bir Azure aboneliğine sahip değilse:

  - Müşteri C'ın Azure AD Kiracı Yöneticisi gerekir almak bir [Kullandıkça Öde Azure aboneliğine](https://azure.microsoft.com/offers/ms-azr-0003p/).
  - Müşteri C'ın Azure AD Kiracı Yöneticisi için sahip olur [, kiracılarının yeni aboneliği ile bağlantı](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

- Gelen [Azure portalı](https://portal.azure.com), müşteri C'ın Azure AD Kiracı Yöneticisi ardından gerekir:
  1. Açık **abonelikleri**.
  1. Geliştirici D'ın uygulamada kullanılmak üzere Azure AD kiracısına sahip bir abonelik seçin.
  1. Seçin **kaynak sağlayıcıları**.
  1. Arama **Microsoft.IoTSpaces**.
  1. Tıklayın **kaydetme**.
  
## <a name="next-steps"></a>Sonraki adımlar

Kullanıcı tanımlı işlevler, Azure ile dijital İkizlerini kullanma hakkında daha fazla bilgi edinmek için [Azure dijital İkizlerini UDF'ler](how-to-user-defined-functions.md).

Daha fazla uygulama rolü atamalarını ile güvenli hale getirmek için rol tabanlı erişim denetimi kullanmayı öğrenmek için [dijital İkizlerini rol tabanlı erişim denetimi](security-create-manage-role-assignments.md).
