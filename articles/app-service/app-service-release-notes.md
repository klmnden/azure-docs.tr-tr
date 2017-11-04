---
title: "2.5.1 .NET için Azure SDK sürüm notları"
description: "2.5.1 .NET için Azure SDK sürüm notları"
services: app-service
documentationcenter: .net,nodejs,java
author: Juliako
manager: erikre
editor: 
ms.assetid: 8d3d815f-bb58-447e-8ff0-f9b9603c7b00
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/10/2016
ms.author: juliako
ms.openlocfilehash: 0a422b02623a18ac6a1eef460bbada681672e69f
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="azure-sdk-for-net-251-release-notes"></a>2.5.1 .NET için Azure SDK sürüm notları
Bu belge .NET 2.5.1 yayın için sürüm notları için Azure SDK'sı içerir. 

## <a name="azure-sdk-for-net-251-release-notes"></a>2.5.1 .NET için Azure SDK sürüm notları
Aşağıdaki yeni özellikleri ve güncelleştirmelerini 2.5.1 .NET için Azure SDK'sındaki geçerlidir.

* Yeni features\scenarios ilgili **Web Tools Extensions**. 
  
  * Azure Web siteleri, Azure App Service'e değiştirildi. 
  * Azure API uygulamaları (Önizleme) desteği eklendi, böylece müşteriler API uygulamaları olarak ASP.NET projeleri yayımlamak ve Ekle'ı kullanın > Azure API uygulama istemcisi hareketi C# projeleri dağıtılan API uygulaması yapısını temel alan kodu oluşturmak için. 
  * Sunucu Gezgininde Web siteleri kaynak grup tabanlı Azure API Apps, Mobile Apps ve Web uygulamaları gruplandırma için destek içerir Azure uygulama hizmeti düğümünde yerine kullanım dışıdır.
  * Böylece müşteriler yeni Mobile Apps projeleri oluşturma, Mobile Apps denetleyicileri ekleme, projeleri yayımlamak ve uygulamaları uzaktan hata ayıklama azure Mobile Apps (Önizleme) desteği eklendi.
  * Ekle > Azure API uygulama istemcisi hareketi Web API geliştiricilere Swagger oluşturmak veya el ile oluşturmak için Swashbuckle gibi üçüncü taraf NuGets kullanabilmeniz için yerel Swagger JSON dosyaları, artık destekler. Bu şekilde, istemci geliştiriciler C# projelerine herhangi bir Swagger uç nokta kullanmak için kod oluşturma özellikleri kullanabilir. 
  * Web uygulaması ve API uygulaması yayımlama iletişim kutuları gruplandırma kaynak Azure portal kavramını ve seçim/Azure kaynak gruplarını ve App Service planları oluşturulmasını desteklemek üzere geliştirilmiştir yeni Web uygulaması ve API uygulaması sağlama iletişim kutusunda gösterilir. 
  * Azure API uygulaması Sunucu Gezgini düğümleri günlük akış ve uzaktan hata ayıklama gibi diğer özelliklerin yanı sıra Azure portal, API uygulamaları ayrıntılı bağlantısı için bağlantılar sağlar.
    
    Bilinen sorunlar ve geçerli sınırlamalar Azure SDK .NET 2.5.1 içinde [bu](app-service-release-notes.md#known_issues_2_5_1) bölümüne bakın.
* Yeni features\scenarios ilgili **Hdınsight Araçları** Visual Studio'da bu sürümde etkinleştirilir. 
  
  * Yerel doğrulama hive komut dosyaları. Betiğinizde herhangi bir hata olup olmadığını görmek için araç çubuğunda Doğrula komut düğmesini tıklatın. 
  * Hive işleri hata ayıklama geliştirildi. Hive işleri Yarn günlüklerini Visual Studio'da erişerek şimdi ayıklayabilirsiniz. Uygulamanızın performans sorunları varsa, araştırma YARN günlüklerini yararlı bilgiler sağlayan...
  * (Genel Önizleme) Anahtar sözcüğü otomatik tamamlama ve IntelliSense için Hive'ı destekler. Yardımcı olmak için Hive komut dosyaları yazmak, Visual Studio için Hdınsight araçları eklenen anahtar sözcüğü otomatik tamamlama ve IntelliSense desteği için Hive.
  * Storm desteği. Artık, Storm topolojileri/Spout'lar/Cıvatalar C# geliştirmek için Visual Studio için Hdınsight araçları da kullanabilirsiniz. Bir Storm kümesine geliştirilen topolojiye göndermek ve topoloji/Cıvata/spout durumunu görmek. Storm topolojileri/Cıvatalar/Spout'lar gidermek için sistem ve müşteri günlükleri'ni kullanabilirsiniz. Ayrıca, Hdınsight üzerinde Storm varolan JAVA varlıkları de kullanabilirsiniz.
    
    Daha fazla bilgi için bkz: [Visual Studio için Hdınsight Hadoop araçlarını kullanmaya başlamanıza](../hdinsight/hadoop/apache-hadoop-visual-studio-tools-get-started.md).

## <a id="known_issues_2_5_1"></a>Azure SDK'sı 2.5.1 .NET için bilinen sorunlar ve sınırlamalar
* Azure API uygulamaları, mobil uygulamalar için dağıtım hedefi olarak görülebilir. Web uygulamaları yalnızca hedef mobil uygulamalar için kadar sonraki bir sürüm olmalıdır. 
* Azure API uygulaması sağlama başarılı şekilde neden ancak Azure App Service etkinliği penceresindeki ilerleme durumunu güncelleştirmek zaman zaman başarısız. Azure Portal'daki yeni Azure API uygulaması durumunu denetlemek için geçici bir çözüm değildir. 
* Dosya > Yeni Proje > API uygulaması > F5 deneyimi hiçbir default/index.html olduğundan HTTP hatayla sonuçlanır. El ile /api/values URL'ye göz atmak için geçici bir çözüm değildir. 
* Zaman zaman, Sunucu Gezgini simgeler düzleştirilmiş görünür. VS yeniden başlatılması bu sorunu çözer. 
* Web uygulaması veya API uygulaması (aşıldı kota hataları veya yinelenen Azure API uygulama ağ geçidi adı gibi) sağlama işlemi sırasında bir özel durum, hataları ham JSON metinleri gösterir. 
* Proje oluşturma sırasında Application Insights işaretlendiğinde aralıklı proje oluşturma sorunları.
* Bazen, oluşturulan Azure API uygulama istemcisi kodu ad alanları eksik, bunlar el ile dahil edilecek (veya Visual Studio yardımlar otomatik olarak içeri aktarılan) derleme kodunu gerekir. 
* Web uygulamaları için mobil uygulama projeleri yayımlanması gerekir, ancak mobil uygulama projeleri bir veritabanı gerektirir beri Azure portalında mobil uygulama olarak oluşturulmuş bir site seçmeniz gerekir. 
* Başlangıç sayfası mobil uygulamalar için "mobil uygulamalar" yerine "mobil hizmeti" terimini kullanır 
* Mobil uygulama projesi oluşturma oluşturmak için bir dakika sürebilir. 
* API uygulaması sırasında bir hata (bazı durumlarda) sağlama Azure API API uygulaması düzgün şekilde sağlandığını ve kullanıma hazırdır izinleri düzgün şekilde ayarlanamadı, yansıtma öğesinden gelir. Azure portalını kullanarak izinlerini el ile ayarlayabilirsiniz.
* Application Insights API'si uygulama şablonları ve mobil uygulama şablonları desteklenmiyor.
* API uygulaması projeleri, bulut hizmeti projeleri ile birlikte kullanılamaz.
* API uygulaması proje şablonları yalnızca C# dilinde kullanılabilir.
* "Azure API uygulama istemcisi Ekle" bağlam menüsü API uygulaması tüketiminin yalnızca C# ' ta desteklenir.

