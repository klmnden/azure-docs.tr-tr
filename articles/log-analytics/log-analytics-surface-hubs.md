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
ms.date: 01/16/2018
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4f56369e412bdd285d3c370f5153fee4f539dfcf
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="monitor-surface-hubs-with-log-analytics-to-track-their-health"></a>Surface Hubs durumlarını izlemek için günlük analizi ile izleme

![Yüzey Hub simgesi](./media/log-analytics-surface-hubs/surface-hub-symbol.png)

Bu makalede, Surface Hub çözüm günlük analizi Microsoft Surface Hub cihazları izlemek için nasıl kullanabileceğinizi açıklar. Günlük analizi yanı sıra bunların nasıl kullanıldığını anlamak, Surface hub'larınız durumunu izlemenize yardımcı olur.

Her Surface Hub Microsoft izleme aracısı yüklü olan. Kendi veri için günlük analizi Surface Hub'ından gönderebilir Aracısı üzerinden. Günlük dosyaları, Surface hub'ları ve misiniz okuma sonra günlük analizi için gönderilir. Cihaz hesabı içine oturum açamıyor olması durumunda Skype gösterilmiştir günlük analizi Surface Hub panosunda veya çevrimdışı olan sunucuları eşitlemiyor, Takvim sorunları gibi. Pano verileri kullanarak, çalışmıyor veya başka sorunlara sahip olduğunu ve potansiyel olarak algılanan sorunlar için düzeltmeler uygulamak aygıtlar tanımlayabilirsiniz.

## <a name="install-and-configure-the-solution"></a>Yükleyin ve yapılandırın
Yüklemek ve çözüm yapılandırmak için aşağıdaki bilgileri kullanın. Günlük analizi, Surface hub'larınız yönetmek için şunlar gerekir:

* A [günlük analizi abonelik](https://azure.microsoft.com/pricing/details/log-analytics/) izlemek istediğiniz cihaz sayısı destekleyecek düzeyi. Günlük analizi fiyatlandırma kaç aygıtlar kaydedilir ve ne kadar veri bağlı olarak bu işlemleri değişir. Bu, Surface Hub dağıtımı planlarken dikkate istersiniz.

Ardından, varolan bir günlük analizi çalışma alanını ekleyin veya yeni bir tane oluşturun. Her iki yöntemi kullanarak altındadır için ayrıntılı yönergeler [günlük Analytics ile çalışmaya başlama](log-analytics-get-started.md). Günlük analizi çalışma alanı yapılandırıldıktan sonra Surface Hub cihazları kaydetmek için iki yolu vardır:

* Otomatik olarak Intune aracılığıyla
* El ile **ayarları** , Surface Hub Cihazınızda.

## <a name="set-up-monitoring"></a>İzleme işlevini ayarlama
Durumu ve etkinliği yüzeyini günlük analizi kullanarak hub'ınızın izleyebilirsiniz. Surface Hub Intune kullanarak veya kullanarak yerel olarak kaydedebilir **ayarları** Surface hub'ındaki.

## <a name="connect-surface-hubs-to-log-analytics-through-intune"></a>Günlük analizi Intune aracılığıyla Surface Hubs bağlanmak
Surface hub'larınız yönetecek günlük analizi çalışma alanı için çalışma alanı kimliği ve çalışma alanı anahtarı gerekir. Bu çalışma alanı ayarlarından Azure portalında alabilirsiniz.

Intune, bir veya daha fazla aygıtlarınızı uygulanan günlük analizi yapılandırma ayarlarını merkezi olarak yönetmenize olanak sağlayan bir Microsoft ürünüdür. Cihazlarınızı Intune üzerinden yapılandırmak için aşağıdaki adımları izleyin:

1. Intune'da oturum açın.
2. Gidin **ayarları** > **bağlı kaynakları**.
3. Oluşturma veya düzenleme Surface Hub şablona dayalı bir ilke.
4. İlke OMS (Azure operasyonel Öngörüler) bölümüne gidin ve günlük analizi ekleyin *çalışma alanı kimliği* ve *çalışma alanı anahtarı* ilkesi.
5. İlkeyi kaydedin.
6. İlke aygıtları uygun grubuyla ilişkilendirin.

   ![Intune İlkesi](./media/log-analytics-surface-hubs/intune.png)

Intune, günlük analizi ayarları daha sonra bunları günlük analizi çalışma alanınızda kaydetme hedef gruptaki cihazlar ile eşitlenir.

## <a name="connect-surface-hubs-to-log-analytics-using-the-settings-app"></a>Surface Hubs ayarları uygulamasını kullanarak günlük Analizi'ne bağlayın
Surface hub'larınız yönetecek günlük analizi çalışma alanı için çalışma alanı kimliği ve çalışma alanı anahtarı gerekir. Azure portalında günlük analizi çalışma alanı için ayarları olanlardan alabilirsiniz.

Ortamınızı yönetmek için Intune kullanmıyorsanız, cihazları el ile kaydedebilirsiniz **ayarları** her Surface Hub üzerinde:

1. Surface Hub'açmak **ayarları**.
2. İstendiğinde cihaz Yöneticisi kimlik bilgilerini girin.
3. Tıklatın **bu aygıt**ve altında **izleme**, tıklatın **OMS ayarlarını yapılandır**.
4. Seçin **izlemeyi etkinleştir**.
5. Günlük analizi OMS Ayarları iletişim kutusunda yazın **çalışma alanı kimliği** ve yazın **çalışma alanı anahtarı**.  
   ![Ayarlar](./media/log-analytics-surface-hubs/settings.png)
6. Tıklatın **Tamam** yapılandırmayı tamamlamak için.

Bir onay desteklemediğini yapılandırma cihaza başarıyla uygulandı söyleyen görüntülenir. Olduysa, aracı için günlük analizi başarıyla bağlandı belirten bir ileti görüntülenir. Cihaz sonra günlük burada görüntüleyebilir ve onun üzerinde işlem analizi veri göndermesini başlatır.

## <a name="monitor-surface-hubs"></a>Surface Hubs İzleyicisi
Surface hub'larınız izleme günlük analizi diğer kaydedilen cihazların izleme benzer kullanmaktır.

1. Azure Portal’da oturum açın.
2. Günlük analizi çalışma alanına gidin ve seçin **genel bakış**.
2. Surface Hub kutucuğuna tıklayın.
3. Cihazınızın sistem durumu görüntülenir.

   ![Yüzey Hub Panosu](./media/log-analytics-surface-hubs/surface-hub-dashboard.png)

Oluşturabileceğiniz [uyarıları](log-analytics-alerts.md) mevcut veya özel günlük aramaları tabanlı. Günlük analizi Surface hub'larından toplar verileri kullanarak, sorunlar ve aygıtlarınızın tanımlayan koşulları uyarıdaki arayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* Kullanım [günlük analizi aramaları oturum](log-analytics-log-searches.md) ayrıntılı Surface Hub verileri görüntülemek için.
* Oluşturma [uyarıları](log-analytics-alerts.md) Surface hub'larıyla sorunları oluştuğunda sizi bilgilendirmek üzere.
