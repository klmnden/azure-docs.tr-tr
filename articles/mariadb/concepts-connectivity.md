---
title: Geçici bağlantı hatalarının MariaDB için Azure veritabanı işleme | Microsoft Docs
description: Geçici bağlantı hataları için MariaDB için Azure veritabanı işleme hakkında bilgi edinin.
keywords: MySQL bağlantı, bağlantı dizesi, bağlantı sorunları, geçici bir hata oluştu, bağlantı hatası
author: jan-eng
ms.author: janeng
ms.service: mariadb
ms.topic: conceptual
ms.date: 11/09/2018
ms.openlocfilehash: f5f5915e6fdb240fa519ee10526c935a524cb5b4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61042225"
---
# <a name="handling-of-transient-connectivity-errors-for-azure-database-for-mariadb"></a>Geçici bağlantı hataları MariaDB için işleme için Azure veritabanı

Bu makalede, MariaDB için Azure veritabanına bağlanırken geçici hataları işlemek nasıl açıklar.

## <a name="transient-errors"></a>Geçici hatalar

Geçici bir hata olarak da bilinen bir geçici hata kendi hatadır. Bırakılan veritabanı sunucusuna bir bağlantı olarak çoğunlukla bu hataları bildirimi. Ayrıca yeni bir sunucuya bağlantı açılamıyor. Örneğin geçici hatalar donanım veya ağ hatası durumda oluşabilir. Başka bir nedeni, yeni bir sürümü kullanıma sunulacaktır bir PaaS hizmeti olabilir. Çoğu bu olayların otomatik olarak sistem tarafından 60 saniyeden kısa bir süre içinde azalır. Tasarım ve bulut uygulamaları geliştirmek için en iyi uygulama, geçici hatalar beklediğiniz sağlamaktır. Bu durumların üstesinden uygun mantıksal var ve herhangi bir zamanda herhangi bir bileşeni oluşabilir varsayılır.

## <a name="handling-transient-errors"></a>Geçici hataları işleme

Geçici hataları yeniden deneme mantığını kullanmayı yapılmalıdır. Bu durum dikkate alınmalıdır:

* Bir bağlantı açmaya çalıştığınızda hata oluşur.
* Boştaki bir bağlantının sunucu tarafında bırakılır. Bir komutu çalıştığınızda yürütülemiyor
* Bir komut şu anda yürüten bir etkin bağlantı bırakılır.

Birinci ve ikinci durumda olan oldukça oldukça işlemek için. Bağlantıyı yeniden açmayı deneyin. Başarılı olduğunda, geçici bir hata sistem tarafından azaltılabilir. MariaDB için Azure veritabanınızı yeniden kullanabilirsiniz. Bağlantıyı yeniden denemeden önce beklediği olması önerilir. İlk yeniden deneme işlemleri başarısız olursa geri alma. Bu şekilde sistemin hata durumuna üstesinden gelmek kullanılabilir olan tüm kaynakları kullanabilirsiniz. İzlemek için iyi bir modelidir:

* Denemeden önce ilk 5 saniye bekleyin.
* Aşağıdaki denemeler, bekleme artırmak için katlanarak, 60 saniye olarak ayarlama.
* En fazla bir noktada uygulamanızı işlemi başarısız oldu olarak değerlendirir. yeniden deneme sayısını ayarlayın.

Etkin bir işlem ile bağlantı başarısız olursa kurtarma doğru bir şekilde işlemek daha zordur. Olası iki durum vardır: İşlem, doğası gereği salt okunur ise, güvenli bağlantıyı yeniden açmalı ve işlemi yeniden deneyin. İşlem, ayrıca veritabanına yazma, Bununla birlikte, işlem geri alınamaz belirlemeniz gerekiyorsa veya önce başarılı olursa, geçici bir hata oluştu. Bu durumda, işleme bildirim veritabanı sunucusundan aldığınız değil.

Bunu yapmanın bir yolu, istemcide yeniden denemeler için kullanılan benzersiz bir kimliği oluşturmaktır. Sunucu ve bir sütunda benzersiz kısıtlama ile depolamak için bu benzersiz kimliği işlemin bir parçası geçirin. Bu şekilde işlem güvenli bir şekilde yeniden deneyebilirsiniz. Önceki işlem geri alındı ve oluşturulan istemci benzersiz kimliği henüz sistemde yok. başarılı olur. Önceki işlem başarıyla tamamlanmadığından benzersiz kimliği daha önce depolanmışsa, yinelenen bir anahtar ihlali gösteren başarısız olur.

Programınızı MariaDB için Azure veritabanı ile üçüncü taraf ara yazılımı iletişim kurduğunda satıcı ara yazılım geçici hatalar için yeniden deneme mantığı içerip içermediğini isteyin.

Yeniden deneme mantığı, test etmek emin olun. Örneğin, ölçeği artırılabilen veya azaltılabilen MariaDB sunucusu için Azure veritabanı, işlem kaynaklarını sırasında kodunuzu çalıştırmak deneyin. Uygulamanızı herhangi bir sorun olmadan bu işlemi sırasında karşılaşılan kısa bir kapalı kalma işlemelidir.

## <a name="next-steps"></a>Sonraki adımlar

* [MariaDB için Azure Veritabanı bağlantı sorunlarını giderme](howto-troubleshoot-common-connection-issues.md)
