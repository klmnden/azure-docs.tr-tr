---
title: MySQL için Azure veritabanı için veritabanı uygulaması geliştirmeye genel bakış
description: MySQL için Azure veritabanı'na bağlanmak için uygulama kodu yazarken Geliştirici izlemelidir tasarım konuları tanıtır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: 946f7011c51b7c6844e023d03e01e4c2043d2578
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60615648"
---
# <a name="application-development-overview-for-azure-database-for-mysql"></a>Uygulama geliştirmeye genel bakış MySQL için Azure veritabanı 
Bu makalede, bir geliştirici, MySQL için Azure veritabanı'na bağlanmak için uygulama kodu yazarken izlemelidir tasarım konuları açıklanmaktadır. 

> [!TIP]
> Sunucu oluşturma, sunucu tabanlı güvenlik duvarı oluşturma, sunucu özelliklerini görüntüleme, veritabanı oluşturun ve bağlanın ve workbench ve mysql.exe kullanarak sorgulama adımlarını gösteren bir öğretici için bkz [ilk MySQL veritabanı için Azure veritabanı tasarlama](tutorial-design-database-using-portal.md)

## <a name="language-and-platform"></a>Dil ve platform
Çeşitli programlama dilleri ve platformları için kod örnekleri mevcuttur. Kod örneklerinin bağlantılarını şu sayfada bulabilirsiniz: [MySQL için Azure veritabanı'na bağlanmak için kullanılan bağlantı kitaplıkları](concepts-connection-libraries.md)

## <a name="tools"></a>Araçlar
MySQL için Azure veritabanı gibi mysql.exe, Workbench veya MySQL yardımcı programları gibi yaygın yönetim araçları MySQL ile uyumlu MySQL topluluk sürümünü kullanan [phpMyAdmin](https://www.phpmyadmin.net/), [Navicat](https://www.navicat.com/products/navicat-for-mysql)ve diğerleri. Veritabanı hizmetiyle etkileşim kurmak için Azure portalı, Azure CLI ve REST API'lerini kullanabilirsiniz.

## <a name="resource-limitations"></a>Kaynak sınırlamaları
MySQL için Azure veritabanı, iki farklı mekanizma kullanarak bir sunucuya kullanabileceği kaynakları yönetir: 
- Kaynak İdaresi.
- Sınırları zorlama.

## <a name="security"></a>Güvenlik
MySQL için Azure veritabanı, sınırlama erişim, koruma verileri, yapılandırma kullanıcılar ve roller ve izleme etkinliklerin bir MySQL veritabanı için kaynakları sağlar.

## <a name="authentication"></a>Kimlik Doğrulaması
MySQL için Azure veritabanı, sunucu kimlik doğrulaması kullanıcı ve oturum açma bilgileri destekler.

## <a name="resiliency"></a>Dayanıklılık
Bir MySQL veritabanına bağlanırken geçici bir hata oluştuğunda, kodunuzun çağrıyı yeniden denemesi gerekir. SQL veritabanını aynı anda yeniden deneniyor birden fazla istemciyle boğmaması için yeniden deneme mantığı geri mantığını devre dışı kullanmanızı öneririz.

- Kod örnekleri: Gösteren kod örnekleri için yeniden deneme mantığı, istediğiniz dili için örneklerine bakın: [MySQL için Azure veritabanı'na bağlanmak için kullanılan bağlantı kitaplıkları](concepts-connection-libraries.md)

## <a name="managing-connections"></a>Bağlantıları yönetme
Veritabanı bağlantıları sınırlı bir kaynak olduğundan, daha iyi performans elde etmek için MySQL veritabanınızı erişirken bağlantıları mantıklı kullanılmasını öneririz.
- Bağlantı havuzu veya kalıcı bağlantılar kullanarak veritabanına erişir.
- Kısa bağlantı ömrü kullanarak veritabanına erişir. 
- Eşzamanlı bağlantılarını kaynaklanan hataları yakalamak için bağlantı denemesi noktasında uygulamanızda kullanım yeniden deneme mantığı izin verilen maksimum sınırına. Yeniden deneme mantığı, kısa bir gecikme ayarlama ve ek bağlantı denemeleri öncesinde rastgele bir süre bekleyin.