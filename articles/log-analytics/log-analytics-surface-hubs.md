---
title: Surface hub'ları Azure Log Analytics ile izleme | Microsoft Docs
description: Surface Hub çözümü, Surface hub durumunu izleyin ve bunların nasıl kullanıldığını anlamak için kullanın.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: 8b4e56bc-2d4f-4648-a236-16e9e732ebef
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/16/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: f7fe7cee39468558ce503c050d5574e4be15ebf5
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "49407178"
---
# <a name="monitor-surface-hubs-with-log-analytics-to-track-their-health"></a>Surface hub'lar durumlarını izlemek için Log Analytics ile izleme

![Surface Hub simgesi](./media/log-analytics-surface-hubs/surface-hub-symbol.png)

Bu makalede, Surface Hub çözümü Log Analytics'te Microsoft Surface Hub cihazları izlemek için nasıl kullanabileceğinizi açıklar. Log Analytics'e yanı sıra bunların nasıl kullanıldığını anlamanıza, Surface hub'larınız durumunu izlemenize yardımcı olur.

Microsoft Monitoring Agent yüklüyse her Surface Hub sahiptir. Kendi verilerini Log Analytics'e Surface Hub'ından gönderebilirsiniz Aracısı aracılığıyla. Günlük dosyaları, Surface hub'ları ve olan okuma sonra Log Analytics'e gönderilir. Çevrimdışı olan sunucuları eşitlemiyor, takvim gibi özellikler veya cihaz hesabının oturum kaydedemediği Skype gösterilen Surface Hub panosunda bulunan Log Analytics. Pano verileri kullanarak, çalışmıyorsa veya başka sorunlar yaşıyorsanız ve potansiyel olarak algılanan sorunlar için düzeltmeler uygulama cihazları tanımlayabilir.

## <a name="install-and-configure-the-solution"></a>Yükleme ve çözüm yapılandırma
Çözümü yüklemek ve yapılandırmak için aşağıdaki bilgileri kullanın. Log analytics'te, Surface hub'ı yönetmek için şunlar gerekir:

* A [Log Analytics aboneliği](https://azure.microsoft.com/pricing/details/log-analytics/) izlemek istediğiniz cihaz sayısını destekleyen düzeyi. Log Analytics fiyatlandırma kaç cihazın kayıtlı ve elinizdeki verilere bağlı olarak bu işlemleri değişir. Bu, Surface Hub sunum planlarken dikkate almasını istersiniz.

Ardından, mevcut bir Log Analytics çalışma alanı ekleyin veya yeni bir tane oluşturun. Ayrıntılı yönergeler için her iki yöntemi kullanarak altındadır [Log Analytics ile çalışmaya başlama](log-analytics-get-started.md). Log Analytics çalışma alanı yapılandırıldıktan sonra Surface Hub cihazları kaydetmek için iki yolu vardır:

* Otomatik olarak Intune aracılığıyla
* El ile **ayarları** Surface Hub Cihazınızda.

## <a name="set-up-monitoring"></a>İlkenin trafiği çevrimdışı durumdaki barındırılan hizmetlere
Sistem durumunu ve Log Analytics kullanarak, Surface Hub etkinliğini izleyebilirsiniz. Surface hub'ı kullanarak yerel olarak veya Intune kullanarak kaydedebilirsiniz **ayarları** Surface hub'da.

## <a name="connect-surface-hubs-to-log-analytics-through-intune"></a>Surface hub'ları Intune üzerinden Log analytics'e bağlanma
Surface hub'ları yönetecek Log Analytics çalışma alanı için çalışma alanı Kimliğiniz ve çalışma alanı anahtarı gerekir. Bu çalışma alanı ayarları Azure portalından alabilirsiniz.

Intune, bir veya daha fazla cihazlarınızı uygulanan Log Analytics yapılandırma ayarlarını merkezi olarak yönetmenize olanak tanıyan bir Microsoft ürünüdür. Cihazlarınızı Intune ile yapılandırmak için aşağıdaki adımları izleyin:

1. Intune'da oturum açın.
2. Gidin **ayarları** > **bağlı kaynakları**.
3. Oluşturabilir ve Surface Hub şablonu temel alan bir ilkeyi düzenleyebilirsiniz.
4. İlkeyi Azure operasyonel İçgörüler bölümüne gidin ve Log Analytics ekleme *çalışma alanı kimliği* ve *çalışma alanı anahtarı* ilkesi.
5. İlkeyi kaydedin.
6. İlkeyi cihazların uygun grubu ile ilişkilendirin.

   ![Intune İlkesi](./media/log-analytics-surface-hubs/intune.png)

Intune, Log Analytics ayarlarını daha sonra bunları Log Analytics çalışma alanınızda kaydetme, hedef gruptaki cihazlar ile eşitler.

## <a name="connect-surface-hubs-to-log-analytics-using-the-settings-app"></a>Surface hub'ları ayarlar uygulamasını kullanarak Log Analytics'e bağlanma
Surface hub'ları yönetecek Log Analytics çalışma alanı için çalışma alanı Kimliğiniz ve çalışma alanı anahtarı gerekir. Azure portalında Log Analytics çalışma alanı ayarlarını olanlardan alabilirsiniz.

Ortamınızı yönetmek için Intune kullanmıyorsanız, cihazları el ile kaydedebilirsiniz **ayarları** her Surface hub'da:

1. Surface hub'ınızdan, açık **ayarları**.
2. İstendiğinde cihaz Yöneticisi kimlik bilgilerini girin.
3. Tıklayın **bu cihaz**ve altında **izleme**, tıklayın **günlük analizi ayarları yapılandırma**.
4. Seçin **izlemeyi etkinleştir**.
5. Log Analytics günlük analizi Ayarları iletişim kutusuna **çalışma alanı kimliği** yazın **çalışma alanı anahtarı**.  
   ![Ayarlar](./media/log-analytics-surface-hubs/settings.png)
6. Tıklayın **Tamam** yapılandırmasını tamamlamak için.

Olup olmadığını yapılandırma cihaza başarıyla uygulandı bildiren bir onay görünür. Bu durumda, aracı başarıyla Log Analytics'e bağlı belirten bir ileti görüntülenir. Cihaz daha sonra burada görüntülemek ve üzerinde işlem Log analytics'e veri gönderen başlatır.

## <a name="monitor-surface-hubs"></a>İzleyici Surface hub'lar
Surface hub'ları izleme Log Analytics gibi diğer kaydedilen cihazların izleme kullanmaktır.

1. Azure Portal’da oturum açın.
2. Log Analytics çalışma alanınıza gidin ve seçin **genel bakış**.
2. Surface Hub kutucuğuna tıklayın.
3. Cihazınızın durumu görüntülenir.

   ![Surface Hub Panosu](./media/log-analytics-surface-hubs/surface-hub-dashboard.png)

Oluşturabileceğiniz [uyarılar](log-analytics-alerts.md) mevcut veya özel günlük aramaları göre. Log Analytics, Surface hub'larından toplar. veriler kullanılarak, sorunlar ve cihazlarınız için tanımladığınız koşulların uyarısında için arama yapabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* Kullanım [Log Analytics'te günlük aramaları](log-analytics-log-searches.md) ayrıntılı Surface Hub verilerini görüntülemek için.
* Oluşturma [uyarılar](log-analytics-alerts.md) Surface hub'larıyla sorunları ortaya çıktığında bunu size bildirecek.
