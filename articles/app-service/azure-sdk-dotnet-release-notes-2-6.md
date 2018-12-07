---
title: .NET 2.6 için Azure SDK sürüm notları
description: .NET 2.6 için Azure SDK sürüm notları
services: app-service/web
documentationcenter: .net
author: chrissfanos
editor: ''
ms.assetid: b45853d5-a2b8-4962-a22d-579cb36ae14c
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: dfc1fe223dbff178c35a969e0273ed80fb4c8be9
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53016436"
---
# <a name="azure-sdk-for-net-26-release-notes"></a>.NET 2.6 için Azure SDK sürüm notları
Bu belge, yayın .NET 2.6 için Azure SDK için sürüm notlarını içerir. 

Azure SDK 2.6 ile bulut hizmeti uygulamaları (PaaS) bulut hizmeti rolünde hedef .NET Framework'ü el ile yüklemek şartıyla .NET 4.5.2'nin veya .NET 4.6 hedefleyen geliştirebilirsiniz. Bkz: [.NET bulut hizmeti rolünde yükleme](https://go.microsoft.com/fwlink/?LinkID=309796).

## <a name="service-bus-updates"></a>Service Bus güncelleştirmeleri
* Olay hub'ları: 
  
  * Şimdi olayları Event Hubs için ek yayımcı uç noktayı kullanıma sunmayı tarafından gönderirken hedeflenen erişim denetimi sağlar.
  * Ek kararlılık ve geliştirme için Event Hubs özellik eklemiştir.
  * Mesajlaşma için WebSocket üzerinden Amqp protokol desteği eklemeyi ve Event Hubs.

## <a name="hdinsight-tools-for-visual-studio-updates"></a>Visual Studio için HDInsight araçları güncelleştirmeleri
* **IntelliSense geliştirmesi**: Uzak meta veri önerisi
  
    Şimdi, Hive betiğinizi düzenlerken Visual Studio için HDInsight araçları alma uzak meta veri destekler. Örneğin, yazabilirsiniz **seçin * FROM** ve tüm tablo adlarının gösterilir. Ayrıca, bir tablo belirttikten sonra sütun adlarını gösterilir.
* **HDInsight öykünücüsü desteği**
  
    HDInsight öykünücüsüne bağlanma Visual Studio için HDInsight araçları artık desteği, giriş herhangi bir maliyet olmadan, Hive betiklerini yerel olarak geliştirebilir, böylece söz konusu betikleri HDInsight kümelerinizi karşı sonra yürütün. 
  
    Daha fazla bilgi için [bu el ile](https://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409).
* **Visual Studio için HDInsight araçları desteği için genel bir Hadoop kümeleri** (Önizleme)
  
    Şimdi aşağıdakileri yapmak için Visual Studio için HDInsight Araçları'nı kullanabilmeniz için Visual Studio için HDInsight Araçları Genel Hadoop kümeleri, destekler:
  
  * kümenize bağlanın, 
  * Gelişmiş IntelliSense/otomatik tamamlama desteği ile Hive sorgusu Yaz, 
  * sezgisel bir kullanıcı Arabirimi ile kümenizdeki tüm işleri görüntüleyin. 
    
    Daha fazla bilgi için [bu el ile](https://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409).

## <a name="in-role-cache-updates"></a>Rol içi önbellek güncelleştirmeleri
* **Rol içi önbellek** kullanacak şekilde güncelleştirildi **Microsoft Azure depolama SDK'sı** sürümü 4.3. Şimdiye kadar **rol içi önbellek** Azure depolama SDK'sı sürüm 1.7 kullanıyordu.
  
    Müşteriler Azure SDK 2.5 kullanarak aşağıda güncelleştirmek için Azure SDK 2.6 veya Azure depolama SDK'sının yeni bir sürüme taşıyın. 
  
    Şu anda sürüm 2011 08 18 Azure depolama, 1 Ağustos 2016 kaldırılacak şekilde zamanlanır. Rol içi önbellek, Azure SDK 2.5 veya aşağıda tüm geçişler için 2.6 Bu zamana kadar tamamlanmış olması gerekir. Sürüm 2011 08 18 Azure Depolama'nın devre dışı bırakma hakkında daha fazla bilgi için bkz. [Microsoft Azure depolama hizmeti sürüm kaldırma güncelleştirmesi: 2016 uzantısı](https://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx).

> [!IMPORTANT]
> Azure yönetilen önbellek hizmeti ve Azure rol içi önbellek 30 Kasım 2016 emeklilik duyuruyoruz. Bu kullanımdan kaldırma için hazırlık için Redis önbelleğine Azure geçiş öneririz. Tarihler ve Geçiş Kılavuzu hakkında daha fazla bilgi için bkz: [hangi Azure önbellek teklifi bana uygundur?](../azure-cache-for-redis/cache-faq.md#which-azure-cache-offering-is-right-for-me)
> 
> 

## <a name="azure-app-service-tools"></a>Azure uygulama hizmeti araçları
Aşağıdaki öğeler güncelleştirilmiştir Azure SDK 2.6 sürümündeki.

* Dağıtım hedefi olarak Azure API uygulamaları kapsayacak şekilde azure yayımlama.
* API işlevleri sağlama API uygulaması oluşturma ve sağlama işlevsellikle olanağı uygulamaları.
* Sunucu Gezgini, yeni uygulama hizmeti düğümünde, kaynak grubuna göre gruplandırılmış Web, mobil ve API apps ile yansıtacak şekilde değiştirildi.
* Azure API uygulama istemcisi hareketi en eklenen ekleyin C# Swagger özellikli API'yi bir kullanıcının Azure aboneliğinde çalışan uygulamaların otomatik olarak oluşturulmasını sağlayacak projeleri.
* API Apps araçları ve sunucu Gezgini'ndeki düğümler App Service yalnızca Visual Studio 2013'te kullanılabilir. 

## <a name="azure-resource-manager-tools-updates"></a>Azure Resource Manager araçları güncelleştirmeleri
Azure resource manager araçları, sanal makineler, ağ ve depolama için şablonları içerecek şekilde güncelleştirildi. Düzenleme deneyimi JSON şablonları ve JSON kod parçacıkları kullanarak şablonları düzenleme yeteneğini için yeni bir ana hat görünümü içerecek şekilde güncelleştirildi. Şablonları Visual Studio'dan dağıtılan betiğe yapılan tüm değişiklikler Visual Studio tarafından kullanılacak şekilde projeyle birlikte sağlanan bir PowerShell betiğini kullanın.

## <a name="diagnostics-improvements-for-cloud-services"></a>Cloud Services için tanılama geliştirmeleri
Azure SDK 2.6 geri Azure işlem öykünücüsü tanılama günlüklerinin toplanması ve bunları geliştirme depolama alanına aktarılması için destek getirir. Herhangi bir tanılama günlükleri (uygulama izleme de dahil olmak üzere günlükleri, olay günlükleri ve windows olay günlüklerini Windows (ETW) günlükleri, performans sayaçları, altyapı izleme) oluşturulan uygulamayı öykünücüde çalışırken geliştirme depolama alanına aktarılabilir Tanılama günlükleri, yerel makinenizde çalıştığından emin olmak için. 

Tanılama depolama hesabı artık farklı ortamlar için farklı bir tanılama depolama hesaplarını kullanmak kolaylaştırarak hizmet yapılandırma (.cscfg) dosyasında belirtilebilir. Azure SDK 2.4 ve Azure SDK 2.6 bağlantı dizesini nasıl çalıştığını arasında bazı önemli farklılıklar vardır. Tanılama depolama bağlantı dizesi kullanma ve bunu projelerinizi nasıl etkilediğini daha fazla bilgi için bkz. [Azure Cloud Services için tanılama yapılandırma](https://go.microsoft.com/fwlink/?LinkID=532784).

## <a name="breaking-changes"></a>Yeni değişiklikler
### <a name="azure-resource-manager-tools"></a>Azure Resource Manager araçları
* **Bulut dağıtımı projelerini** proje türü Azure SDK 2.5 kullanılabilir adlandırıldı **Azure kaynak grubu**.
* **Bulut dağıtım projesi** Azure SDK 2.5 oluşturulan projeleri türünü 2.6 içinde kullanılabilir ancak Visual Studio şablonu dağıtımı başarısız olur. Bununla birlikte, PowerShell betiğiyle dağıtma işlemi, yine de daha önce yaptığınız gibi çalışır.  Nasıl kullanılacağı hakkında daha fazla bilgi için **bulut dağıtımı projelerini** okuyun 2.6 sürümündeki yenilik [sonrası](https://go.microsoft.com/fwlink/?LinkID=534086).

## <a name="known-issues"></a>Bilinen sorunlar
* Öykünücüsü tanılama günlüklerinin toplanması için 64-bit işletim sistemi gerektirir. Tanılama günlükleri bir 32-bit işletim sisteminde çalışırken toplanmaz. Bu, herhangi bir öykünücü işlevi etkilemez. 
* 29/4/2015 tarihinde yayımlanan Azure SDK 2.6 iki sorunu vardı: 
  
  * Azure SDK 2.6 makinede yüklendiğinde, Visual Studio 2015'te Evrensel uygulama yüklenemedi.
  * Bir bulut hizmeti projesi hata ayıklama Visual Studio 2013 ve Visual Studio 2015 burada Visual Studio yanıt vermemeye başlıyor ve "Yapılandırma tanılama öykünücüsü" iletisiyle birlikte bir iletişim kutusu görüntülenirken kilitleniyor başarısız olur.
    
    Bir güncelleştirme için Azure SDK 2.6 18/5/2015 tarihinde yayınlanmıştır. Güncelleştirilmiş 2.6.30508.1601 sürümüdür; Bu, yukarıda açıklanan iki sorunlar için düzeltmeler içerir. SDK Denetim Masası'ndan yapısını tanımlayabilir -> Programlar ve Özellikler -> Microsoft Azure Araçları için Microsoft Visual Studio 2013 – v 2.6. Sürüm sütununda yapı numarasını görüntüler.
    
    Yine de yukarıdaki sorunları yaşıyorsanız, Azure 2.6 SDK'sı için en son sürümünü yüklemek [VS 2012](https://go.microsoft.com/fwlink/p/?linkid=323511&clcid=0x409), [VS 2013](https://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) veya [VS 2015](https://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409).

## <a name="see-also"></a>Ayrıca Bkz.
[Destek ve kullanımdan kaldırma Azure SDK'sı ve bilgi için .NET API'leri](https://msdn.microsoft.com/library/azure/dn479282.aspx/)

