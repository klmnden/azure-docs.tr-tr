---
title: "İzleme Azure günlük analizi ile Surface Hubs | Microsoft Docs"
description: "Surface hub'larınız sağlığını izlemek ve nasıl kullanıldıkları anlamak için Surface Hub çözümü kullanın."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 8b4e56bc-2d4f-4648-a236-16e9e732ebef
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b6ecd0d09589fec85c1633f528afc1165c346b7f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-surface-hubs-with-log-analytics-to-track-their-health"></a>Surface Hubs durumlarını izlemek için günlük analizi ile izleme

![Yüzey Hub simgesi](./media/log-analytics-surface-hubs/surface-hub-symbol.png)

Bu makalede, Surface Hub çözüm günlük analizi Microsoft Surface Hub cihazları ile Microsoft Operations Management Suite'nı (OMS) izlemek için nasıl kullanabileceğinizi açıklar. Günlük analizi yanı sıra bunların nasıl kullanıldığını anlamak, Surface hub'larınız durumunu izlemenize yardımcı olur.

Her Surface Hub Microsoft izleme aracısı yüklü olan. Kendi veri için OMS Surface Hub'ından gönderebilir Aracısı üzerinden. Günlük dosyaları, Surface hub'ları ve misiniz okuma sonra OMS hizmetine gönderilir. Cihaz hesabı içine oturum açamıyor olması durumunda Skype gösterilen OMS Surface Hub Panoda veya çevrimdışı olan sunucuları eşitlemiyor, Takvim sorunları gibi. Pano verileri kullanarak, çalışmıyor veya başka sorunlara sahip olduğunu ve potansiyel olarak algılanan sorunlar için düzeltmeler uygulamak aygıtlar tanımlayabilirsiniz.

## <a name="installing-and-configuring-the-solution"></a>Yükleme ve çözüm yapılandırılıyor
Yüklemek ve çözüm yapılandırmak için aşağıdaki bilgileri kullanın. Microsoft Operations Management Suite (OMS) gelen, Surface hub'ları yönetmek için şunlar gerekir:

* Geçerli bir abonelik [OMS](http://www.microsoft.com/oms).
* Bir [OMS abonelik](https://azure.microsoft.com/pricing/details/log-analytics/) izlemek istediğiniz cihaz sayısı destekleyecek düzeyi. OMS fiyatlandırma kaç aygıtlar kaydedilir ve ne kadar veri bağlı olarak bu işlemlerin değişir. Bu, Surface Hub dağıtımı planlarken dikkate istersiniz.

Ardından, mevcut Microsoft Azure Aboneliğinize bir OMS abonelik eklemek veya OMS Portalı aracılığıyla doğrudan yeni bir çalışma alanı oluşturun. Her iki yöntemi kullanarak altındadır için ayrıntılı yönergeler [günlük Analytics ile çalışmaya başlama](log-analytics-get-started.md). OMS abonelik ayarlandıktan sonra Surface Hub cihazları kaydetmek için iki yolu vardır:

* Otomatik olarak Intune aracılığıyla
* El ile **ayarları** , Surface Hub Cihazınızda.

## <a name="set-up-monitoring"></a>İlkenin trafiği çevrimdışı durumdaki barındırılan hizmetlere
Durumu ve etkinliği yüzeyini OMS içinde günlük analizi kullanarak hub'ınızın izleyebilirsiniz. OMS Surface hub'ı Intune kullanarak veya kullanarak yerel olarak kaydedebilir **ayarları** Surface hub'ındaki.

## <a name="connect-surface-hubs-to-oms-through-intune"></a>Surface Hubs OMS Intune aracılığıyla bağlanma
Surface hub'larınız yönetecek OMS çalışma alanı için çalışma alanı kimliği ve çalışma alanı anahtarı gerekir. Bu OMS Portalı'ndan elde edebilirsiniz.

Intune, bir veya daha fazla aygıtlarınızı uygulanan OMS yapılandırma ayarlarını merkezi olarak yönetmenize olanak sağlayan bir Microsoft ürünüdür. Cihazlarınızı Intune üzerinden yapılandırmak için aşağıdaki adımları izleyin:

1. Intune'da oturum açın.
2. Gidin **ayarları** > **bağlı kaynakları**.
3. Oluşturma veya düzenleme Surface Hub şablona dayalı bir ilke.
4. İlke OMS (Azure operasyonel Öngörüler) bölümüne gidin ve ekleme *çalışma alanı kimliği* ve *çalışma alanı anahtarı* ilkesi.
5. İlkeyi kaydedin.
6. İlke aygıtları uygun grubuyla ilişkilendirin.

   ![Intune İlkesi](./media/log-analytics-surface-hubs/intune.png)

Intune, OMS ayarları sonra bunları OMS çalışma alanınızda kaydetme hedef gruptaki cihazlar ile eşitlenir.

## <a name="connect-surface-hubs-to-oms-using-the-settings-app"></a>Surface Hubs ayarları uygulamasını kullanarak OMS Bağlan
Surface hub'larınız yönetecek OMS çalışma alanı için çalışma alanı kimliği ve çalışma alanı anahtarı gerekir. Bu OMS Portalı'ndan elde edebilirsiniz.

Ortamınızı yönetmek için Intune kullanmıyorsanız, cihazları el ile kaydedebilirsiniz **ayarları** her Surface Hub üzerinde:

1. Surface Hub'açmak **ayarları**.
2. İstendiğinde cihaz Yöneticisi kimlik bilgilerini girin.
3. Tıklatın **bu aygıt**ve altında **izleme**, tıklatın **OMS ayarlarını yapılandır**.
4. Seçin **izlemeyi etkinleştir**.
5. OMS Ayarları iletişim kutusuna **çalışma alanı kimliği** ve yazın **çalışma alanı anahtarı**.  
   ![Ayarlar](./media/log-analytics-surface-hubs/settings.png)
6. Tıklatın **Tamam** yapılandırmayı tamamlamak için.

Bir onay desteklemediğini OMS yapılandırma cihaza başarıyla uygulandı söyleyen görüntülenir. Olduysa, aracı OMS hizmetine başarıyla bağlandı belirten bir ileti görüntülenir. Cihaz ardından burada görüntüleyebilir ve onun üzerinde işlem OMS veri göndermesini başlatır.

## <a name="monitor-surface-hubs"></a>Surface Hubs İzleyicisi
Surface hub'larınız izleme OMS çok diğer kaydedilen cihazların izleme gibi kullanmaktır.

1. OMS portalında oturum açın.
2. Surface Hub çözüm paketi panoya gidin.
3. Cihazınızın sistem durumu görüntülenir.

   ![Yüzey Hub Panosu](./media/log-analytics-surface-hubs/surface-hub-dashboard.png)

Oluşturabileceğiniz [uyarıları](log-analytics-alerts.md) mevcut veya özel günlük aramaları tabanlı. Surface hub'larından OMS toplar verileri kullanarak, sorunlar ve aygıtlarınızın tanımlayan koşulları uyarıdaki arayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* Kullanım [günlük analizi aramaları oturum](log-analytics-log-searches.md) ayrıntılı Surface Hub verileri görüntülemek için.
* Oluşturma [uyarıları](log-analytics-alerts.md) Surface hub'larıyla sorunları oluştuğunda sizi bilgilendirmek üzere.
