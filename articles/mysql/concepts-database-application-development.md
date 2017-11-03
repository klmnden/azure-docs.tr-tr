---
title: "Azure veritabanı için MySQL veritabanı uygulaması geliştirmeye genel bakış | Microsoft Docs"
description: "Bir geliştirici MySQL için Azure veritabanına bağlanmak için uygulama kodu yazarken izlemeniz gereken tasarım konuları sunar"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 09/29/2017
ms.openlocfilehash: 6a9bd8f88383b5186e470163bc67f9233172fd49
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="application-development-overview-for-azure-database-for-mysql"></a>Azure veritabanı için MySQL için uygulama geliştirmeye genel bakış 
Bu makalede bir geliştirici MySQL için Azure veritabanına bağlanmak için uygulama kodu yazarken izlemeniz gereken tasarım konuları açıklanmaktadır. 

> [!TIP]
> Bir sunucu oluşturmak, sunucu tabanlı bir güvenlik duvarı oluşturma, sunucu özelliklerini görüntülemek, veritabanı oluşturmak ve bağlamak ve çalışma ekranı ve mysql.exe kullanarak sorgulama gösteren bir öğretici için bkz: [ilk Azure veritabanınızı MySQL veritabanı için tasarlama](tutorial-design-database-using-portal.md)

## <a name="language-and-platform"></a>Dil ve platform
Çeşitli programlama dilleri ve platformları için kod örnekleri mevcuttur. Kod örnekleri bağlantılar bulabilirsiniz: [MySQL için Azure veritabanına bağlanmak için kullanılan bağlantı kitaplıkları](concepts-connection-libraries.md)

## <a name="tools"></a>Araçlar
Azure veritabanı için MySQL kullanır gibi mysql.exe, çalışma ekranı veya MySQL yardımcı programları gibi genel yönetim araçlarını MySQL ile uyumlu MySQL community sürümü [phpMyAdmin](https://www.phpmyadmin.net/), [Navicat](https://www.navicat.com/products/navicat-for-mysql)ve diğerleri. Veritabanı hizmetiyle etkileşim kurmak için Azure portalı, Azure CLI ve REST API de kullanabilirsiniz.

## <a name="resource-limitations"></a>Kaynak sınırlamaları
Azure veritabanı MySQL için iki farklı mekanizmalarını kullanarak bir sunucuya kullanılabilir kaynakları yönetir: 
- Kaynak İdaresi.
- Sınırları zorlama.

## <a name="security"></a>Güvenlik
Azure veritabanı için MySQL sınırlama erişim, koruma verileri, bir MySQL veritabanı izleme etkinliklerini, yapılandırma kullanıcılar ve roller için kaynaklar sağlar.

## <a name="authentication"></a>Kimlik Doğrulaması
Azure veritabanı için MySQL server ve kimlik doğrulaması için kullanıcıların oturum açma bilgileri destekler.

## <a name="resiliency"></a>Dayanıklılık
Bir MySQL veritabanına bağlanırken geçici bir hata ortaya çıktığında, kodunuzu çağrı yeniden denemeniz gerekir. Böylece, SQL veritabanı ile birden çok istemci aynı anda yeniden deneniyor doldurmaya değil yeniden deneme mantığı geri mantığı kullanmanızı öneririz.

- Kod örnekleri: örnekler için tercih ettiğiniz dili gösteren kod örnekleri mantığı yeniden dene için bkz: [MySQL için Azure veritabanına bağlanmak için kullanılan bağlantı kitaplıkları](concepts-connection-libraries.md)

## <a name="managing-connections"></a>Bağlantıları yönetme
Veritabanı bağlantıları sınırlı bir kaynak olduğundan, daha iyi performans elde etmek için MySQL veritabanınızı erişirken bağlantıların duyarlı kullanılmasını öneririz.
- Veritabanı bağlantı havuzunu veya kalıcı bağlantılar kullanarak erişir.
- Veritabanı kısa bağlantı ömrü kullanarak erişir. 
- Bağlantı denemesi eşzamanlı bağlantılarından kaynaklanan hataları catch noktasında, uygulamanızda kullanmak yeniden deneme mantığı izin verilen maksimum ulaştı. Yeniden deneme mantığı kısa bir gecikme ayarlayın ve ardından ek bağlantı denemeleri önce rastgele bir süre bekleyin.