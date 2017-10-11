---
title: "2.6 .NET için Azure SDK sürüm notları"
description: "2.6 .NET için Azure SDK sürüm notları"
services: app-service/web
documentationcenter: .net
author: chrissfanos
editor: 
ms.assetid: b45853d5-a2b8-4962-a22d-579cb36ae14c
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: 21817b09440fc98a54dc45c9129d104b01fa387d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="azure-sdk-for-net-26-release-notes"></a>2.6 .NET için Azure SDK sürüm notları
Bu belge, .NET 2.6 yayın için Azure SDK'sı için sürüm notlarını içermektedir. 

Azure SDK 2.6 ile bulut hizmet uygulamaları (PaaS) bulut hizmeti rolü'nde el ile hedef .NET Framework'ü yüklemeniz koşuluyla .NET 4.5.2 veya .NET 4.6 hedefleme geliştirebilirsiniz. Bkz: [bir bulut hizmeti rolü'nde .NET yükleme](http://go.microsoft.com/fwlink/?LinkID=309796).

## <a name="service-bus-updates"></a>Hizmet veri yolu güncelleştirmeleri
* Olay hub'ları: 
  
  * Şimdi olayları Event Hubs için ek yayımcı uç nokta göstererek gönderirken hedeflenen erişim denetimi sağlar.
  * Ek kararlılık ve geliştirme için olay hub'ları özellik eklemiştir.
  * İleti için Amqp protokolünü desteği WebSocket üzerinden ekleme ve Event Hubs.

## <a name="hdinsight-tools-for-visual-studio-updates"></a>Visual Studio için Hdınsight araçları güncelleştirir
* **IntelliSense geliştirme**: Uzak meta veri önerisi
  
    Şimdi, Hive betiğinizi düzenlerken Visual Studio için Hdınsight araçları alma uzak meta veri destekler. Örneğin, yazabilirsiniz **seçin * FROM** ve tüm tablo adlarının gösterilir. Ayrıca, sütun adlarının bir tablo belirttikten sonra gösterilir.
* **Hdınsight öykünücü desteği**
  
    Şimdi Visual Studio için Hdınsight araçları Hdınsight öykünücüsünde bağlanma desteği, herhangi bir maliyeti oluşturmaksızın Hive komut dosyalarınızı yerel olarak geliştirebilir şekilde bu komut, Hdınsight kümeleri karşı yürütün. 
  
    Daha fazla bilgi için bkz [bu el ile](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409).
* **Visual Studio için Hdınsight araçları desteklemek için genel Hadoop kümeleri** (Önizleme)
  
    Şimdi aşağıdakileri yapmak için Visual Studio için Hdınsight araçları kullanabilmeniz için Visual Studio için Hdınsight Araçları Genel Hadoop kümelerini destekler:
  
  * kümenize bağlanmak, 
  * Otomatik/IntelliSense-tamamlama desteği Hive sorgusu Yaz, 
  * sezgisel bir kullanıcı Arabirimi ile kümenizdeki tüm işleri görüntüleyin. 
    
    Daha fazla bilgi için bkz [bu el ile](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409).

## <a name="in-role-cache-updates"></a>Rol içi önbelleği güncelleştirmeleri
* **Rol içi önbelleği** kullanmak için güncelleştirilmiş **Microsoft Azure depolama SDK'sı** sürümü 4.3. Şimdiye kadar **rol içi önbelleği** Azure depolama SDK'sı 1.7 sürümü kullanıyordu.
  
    Müşterilerin Azure SDK 2.5 kullanarak veya aşağıda güncelleştirmek için Azure SDK 2.6 ve Azure depolama SDK'sının yeni sürüme taşıyın. 
  
    Şu anda Azure Storage 2011 08 18 sürüm 1 Ağustos 2016 kaldırılacak zamanlanır. Rol içi önbellek Azure SDK 2.5 veya aşağıda tüm geçişler için 2.6 Bu zamana kadar tamamlanmış olması gerekir. Azure depolama sürüm 2011 08 18 devre dışı bırakma hakkında daha fazla bilgi için bkz: [Microsoft Azure depolama hizmeti sürüm kaldırma güncelleştirmesi: 2016 uzantısı](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx).

> [!IMPORTANT]
> Biz Azure yönetilen önbellek hizmeti ve Azure rol içi önbelleği için 30 Kasım 2016 devre dışı bırakma Duyurusu. Bu devre dışı bırakma hazırlığı Azure Redis önbelleğine geçiş öneririz. Tarihler ve Geçiş Kılavuzu hakkında daha fazla bilgi için bkz: [Azure önbelleği tekliftir benim için en uygun?](../redis-cache/cache-faq.md#which-azure-cache-offering-is-right-for-me)
> 
> 

## <a name="azure-app-service-tools"></a>Azure uygulama hizmeti araçları
Aşağıdaki öğeler güncelleştirilmiştir Azure SDK 2.6 sürümündeki.

* Azure yayımlama dağıtım hedefi olarak Azure API uygulamaları içerecek şekilde geliştirilmiştir.
* API işlevleri sağlama kullanıcılar ile API uygulaması oluşturma ve sağlama işlevselliğini etkinleştirmek için uygulamaları.
* Sunucu Gezgini kaynak grubuna göre gruplandırılmış Web, mobil ve API apps ile yeni uygulama hizmeti düğümünde yansıtacak şekilde değiştirildi.
* Swagger özellikli API kullanıcının Azure aboneliğindeki çalışan uygulamalar otomatik olarak oluşturulmasını sağlayacak çoğu C# projeleri için eklenen Azure API uygulama istemcisi hareketi ekleyin.
* API Apps araçları ve uygulama hizmeti düğümünde Sunucu Gezgini yalnızca Visual Studio 2013'te kullanılabilir. 

## <a name="azure-resource-manager-tools-updates"></a>Azure Kaynak Yöneticisi Araçları güncelleştirmeleri
Azure Kaynak Yöneticisi Araçları şablonları sanal makineleri, ağ ve depolama alanı içerecek şekilde güncelleştirildi. Düzenleme deneyimi JSON şablonları ve JSON parçacıkları şablonları düzenleme özelliği için yeni bir anahat görünümü içerecek şekilde güncelleştirildi. Visual Studio'dan dağıtılan şablonlarını komut dosyasına yapılan tüm değişiklikler Visual Studio tarafından kullanılacak şekilde proje ile sağlanan bir PowerShell komut dosyasını kullanın.

## <a name="diagnostics-improvements-for-cloud-services"></a>Bulut Hizmetleri için tanılama geliştirmeleri
Azure SDK 2.6 geri Azure işlem öykünücüsü'nde tanılama günlüklerinin toplanması ve geliştirme depolama alanına aktarılması için destek getirir. Tüm tanılama günlükleri (uygulama izleme de dahil olmak üzere Windows (ETW) günlükleri, performans sayaçları, altyapı izleme günlükleri ve windows olay günlüklerini Olay günlükleri) oluşturulan uygulamayı öykünücüde çalışırken geliştirme depolama alanına aktarılabilir Tanılama günlükleri, yerel makinenizde çalıştığını doğrulamak için. 

Tanılama depolama hesabı, farklı tanılama depolama hesapları farklı ortamlar için kullanımını kolaylaştırma hizmet yapılandırma (.cscfg) dosyasında şimdi belirtilebilir. Azure SDK 2.4 ve Azure SDK 2.6 bağlantı dizesini nasıl çalıştığı arasında önemli bazı farklar vardır. Tanılama depolama bağlantı dizesi kullanmak nasıl ve ne projelerinizi etkiler hakkında daha fazla bilgi için bkz: [Azure bulut Hizmetleri için yapılandırma tanılama](http://go.microsoft.com/fwlink/?LinkID=532784).

## <a name="breaking-changes"></a>Yeni değişiklikler
### <a name="azure-resource-manager-tools"></a>Azure Kaynak Yöneticisi Araçları
* **Bulut dağıtım projeleri** proje türü Azure SDK 2.5 kullanılabilir adlandırılmıştır **Azure kaynak grubu**.
* **Bulut dağıtım projeleri** Azure SDK 2.5 oluşturulan projeleri türünü 2.6 içinde kullanılabilir ancak Visual Studio şablonu dağıtımı başarısız olur. Ancak, PowerShell Betiği ile dağıtma işlemi, hala daha önce yaptığınız gibi çalışır.  Nasıl kullanılacağı hakkında bilgi için **bulut dağıtım projeleri** bu okuma 2.6 sürümündeki [sonrası](http://go.microsoft.com/fwlink/?LinkID=534086).

## <a name="known-issues"></a>Bilinen sorunlar
* Öykünücü tanılama günlükleri toplama 64-bit işletim sistemi gerektirir. Tanılama günlüklerini bir 32 bit işletim sisteminde çalışırken toplanmaz. Bu, herhangi bir öykünücü işlevsellik etkilemez. 
* Azure SDK 2.6 4/29/2015 tarihinde yayımlanan iki sorunları vardı: 
  
  * Azure SDK 2.6 makinede yüklendiğinde Evrensel uygulama Visual Studio 2015'te yüklenemedi.
  * Bir bulut hizmeti projesi hata ayıklama Visual Studio 2013 ve Visual Studio burada yanıt vermemeye başlıyor ve "Yapılandırma tanılama öykünücüsü" iletisini içeren bir iletişim kutusu görüntülenirken çöküyor Visual Studio 2015 başarısız olur.
    
    Azure SDK 2.6 için bir güncelleştirme 18/5/2015 tarihinde yayımlanmıştır. Güncelleştirilmiş sürüm 2.6.30508.1601 ise; Yukarıda açıklanan iki sorunlar için düzeltmeler içerir. Denetim Masası'ndan SDK derleme tanımlayabilirsiniz -> Programlar ve Özellikler -> Microsoft Azure Araçları Microsoft Visual Studio 2013 için – v 2.6. Sürüm sütununda yapı numarası görüntüler.
    
    Yukarıdaki sorunları hala bakacak için Azure 2.6 SDK en son sürümünü yükleyin [VS 2012](http://go.microsoft.com/fwlink/p/?linkid=323511&clcid=0x409), [VS 2013](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) veya [VS 2015](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409).

## <a name="see-also"></a>Ayrıca Bkz.
[Destek ve .NET ve API'ler için devre dışı bırakma bilgi Azure SDK'sı](https://msdn.microsoft.com/library/azure/dn479282.aspx/)

