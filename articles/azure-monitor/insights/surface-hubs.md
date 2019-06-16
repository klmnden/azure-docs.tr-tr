---
title: Surface hub'ları Azure İzleyici ile izleme | Microsoft Docs
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
ms.topic: conceptual
ms.date: 01/16/2018
ms.author: magoedte
ms.openlocfilehash: 7e0dbb4c3cd8ae4bb552e7b7f0748f1bde2f51de
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65232790"
---
# <a name="monitor-surface-hubs-with-azure-monitor-to-track-their-health"></a>Surface hub'lar durumlarını izlemek için Azure İzleyici ile izleme

![Surface Hub simgesi](./media/surface-hubs/surface-hub-symbol.png)

Bu makalede, Surface Hub çözümü Azure İzleyici'de Microsoft Surface Hub cihazları izlemek için nasıl kullanabileceğinizi açıklar. Çözüm yanı sıra bunların nasıl kullanıldığını anlamanıza, Surface hub'larınız durumunu izlemenize yardımcı olur.

Microsoft Monitoring Agent yüklüyse her Surface Hub sahiptir. Kendi, veri, Surface Hub'ından Azure İzleyici'de bir Log Analytics çalışma alanına gönderebildiğini Aracısı aracılığıyla. Günlük dosyaları, Surface hub'ları ve olan okuma sonra Azure İzleyicisi'ne gönderilir. Çevrimdışı olan sunucuları eşitlemiyor, takvim gibi özellikler veya cihaz hesabının oturum kaydedemediği Skype gösterilen Surface Hub panosunda bulunan Azure İzleyici. Pano verileri kullanarak, çalışmıyorsa veya başka sorunlar yaşıyorsanız ve potansiyel olarak algılanan sorunlar için düzeltmeler uygulama cihazları tanımlayabilir.

## <a name="install-and-configure-the-solution"></a>Yükleme ve çözüm yapılandırma
Çözümü yüklemek ve yapılandırmak için aşağıdaki bilgileri kullanın. Azure İzleyici'de, Surface hub'ı yönetmek için şunlar gerekir:

* A [Log Analytics aboneliği](https://azure.microsoft.com/pricing/details/log-analytics/) izlemek istediğiniz cihaz sayısını destekleyen düzeyi. Log Analytics fiyatlandırma kaç cihazın kayıtlı ve elinizdeki verilere bağlı olarak bu işlemleri değişir. Bu, Surface Hub sunum planlarken dikkate almasını istersiniz.

Ardından, mevcut bir Log Analytics çalışma alanı ekleyin veya yeni bir tane oluşturun. Ayrıntılı yönergeler için her iki yöntemi kullanarak altındadır [Azure portalında Log Analytics çalışma alanı oluşturma](../learn/quick-create-workspace.md). Log Analytics çalışma alanı yapılandırıldıktan sonra Surface Hub cihazları kaydetmek için iki yolu vardır:

* Otomatik olarak Intune aracılığıyla
* El ile **ayarları** Surface Hub Cihazınızda.

## <a name="set-up-monitoring"></a>İzlemeyi ayarlama
Azure İzleyicisi'ni kullanarak, Surface Hub, etkinlik ve sistem durumu izleyebilirsiniz. Surface hub'ı kullanarak yerel olarak veya Intune kullanarak kaydedebilirsiniz **ayarları** Surface hub'da.

## <a name="connect-surface-hubs-to-azure-monitor-through-intune"></a>Surface hub'ları Azure İzleyici Intune aracılığıyla bağlanma
Surface hub'ları yönetecek Log Analytics çalışma alanı için çalışma alanı Kimliğiniz ve çalışma alanı anahtarı gerekir. Bu çalışma alanı ayarları Azure portalından alabilirsiniz.

Intune, bir veya daha fazla cihazlarınızı uygulanan Log Analytics çalışma alanı yapılandırma ayarlarını merkezi olarak yönetmenize olanak tanıyan bir Microsoft ürünüdür. Cihazlarınızı Intune ile yapılandırmak için aşağıdaki adımları izleyin:

1. Intune'da oturum açın.
2. Gidin **ayarları** > **bağlı kaynakları**.
3. Oluşturabilir ve Surface Hub şablonu temel alan bir ilkeyi düzenleyebilirsiniz.
4. İlkeyi Azure operasyonel İçgörüler bölümüne gidin ve Log Analytics ekleme *çalışma alanı kimliği* ve *çalışma alanı anahtarı* ilkesi.
5. İlkeyi kaydedin.
6. İlkeyi cihazların uygun grubu ile ilişkilendirin.

   ![Intune İlkesi](./media/surface-hubs/intune.png)

Intune, Log Analytics ayarlarını daha sonra bunları Log Analytics çalışma alanınızda kaydetme, hedef gruptaki cihazlar ile eşitler.

## <a name="connect-surface-hubs-to-azure-monitor-using-the-settings-app"></a>Surface hub'ları ayarlar uygulamasını kullanarak Azure İzleyici için Bağlan
Surface hub'ları yönetecek Log Analytics çalışma alanı için çalışma alanı Kimliğiniz ve çalışma alanı anahtarı gerekir. Azure portalında Log Analytics çalışma alanı ayarlarını olanlardan alabilirsiniz.

Ortamınızı yönetmek için Intune kullanmıyorsanız, cihazları el ile kaydedebilirsiniz **ayarları** her Surface hub'da:

1. Surface hub'ınızdan, açık **ayarları**.
2. İstendiğinde cihaz Yöneticisi kimlik bilgilerini girin.
3. Tıklayın **bu cihaz**ve altında **izleme**, tıklayın **günlük analizi ayarları yapılandırma**.
4. Seçin **izlemeyi etkinleştir**.
5. Log Analytics günlük analizi Ayarları iletişim kutusuna **çalışma alanı kimliği** yazın **çalışma alanı anahtarı**.  
   ![Ayarlar](./media/surface-hubs/settings.png)
6. Tıklayın **Tamam** yapılandırmasını tamamlamak için.

Olup olmadığını yapılandırma cihaza başarıyla uygulandı bildiren bir onay görünür. Bu durumda, aracıyı Azure İzleyici'ı başarıyla bağlandı belirten bir ileti görüntülenir. Cihaz daha sonra bir Azure burada görüntülemek ve üzerinde işlem İzleyici için veri gönderen başlatır.

## <a name="monitor-surface-hubs"></a>İzleyici Surface hub'lar
Surface hub'ları izlenmesi Azure İzleyicisi'ni kullanarak diğer kaydedilen cihazların izleme gibi büyük önem taşır.

[!INCLUDE [azure-monitor-solutions-overview-page](../../../includes/azure-monitor-solutions-overview-page.md)]

Surface Hub kutucuğuna tıkladığınızda, cihazınızın durumu görüntülenir.

   ![Surface Hub Panosu](./media/surface-hubs/surface-hub-dashboard.png)

Oluşturabileceğiniz [uyarılar](../platform/alerts-overview.md) mevcut veya özel günlük aramaları göre. Azure İzleyici, Surface hub'larından topladığı veriler kullanarak, sorunlar ve cihazlarınız için tanımladığınız koşulların uyarısında için arama yapabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* Kullanım [sorgular Azure İzleyici'de oturum](../log-query/log-query-overview.md) ayrıntılı Surface Hub verilerini görüntülemek için.
* Oluşturma [uyarılar](../platform/alerts-overview.md) Surface hub'larıyla sorunları ortaya çıktığında bunu size bildirecek.
