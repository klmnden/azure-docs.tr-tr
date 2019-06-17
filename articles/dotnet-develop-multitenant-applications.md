---
title: Çok Kiracılı Web uygulama düzeni | Microsoft Docs
description: Mimari genel bakış ve bir çok kiracılı web uygulamasının Azure'da nasıl uygulanacağını açıklayan tasarım desenlerini bulur.
services: ''
documentationcenter: .net
author: wadepickett
manager: wpickett
editor: ''
ms.assetid: 4f0281d2-1555-42b0-a99d-1222fade0b0f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/05/2015
ms.author: wpickett
ms.openlocfilehash: 92a0caedca34756228dbf57ec9099fd2ece3d84e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66225982"
---
# <a name="multitenant-applications-in-azure"></a>Azure'daki çok müşterili uygulamalar
Çok kiracılı bir uygulama farklı kullanıcılar veya "Kiracı" kendi olduğu gibi ancak uygulamayı görüntülemek izin veren bir paylaşılan bir kaynaktır. Kendi çok kiracılı bir uygulama için çok uygundur tipik bir senaryo, kullanıcı deneyimini özelleştirme ancak Aksi halde aynı temel iş gereksinimlerine sahip tüm kullanıcılar uygulamanın isteyebilir biridir. Office 365 ve Outlook.com visualstudio.com büyük çok kiracılı uygulamalar örnekleridir.

Çok kiracılı mimari avantajları, uygulama sağlayıcısının açısından bakıldığında, işletimsel ve maliyet verimliliği için çoğunlukla ilgilidir. Uygulamanızın bir sürümünü, birçok kiracılar/yönetim görevlerini izleme, performans ayarlama, yazılım bakımı ve veri yedeklemeleri gibi sistem birleştirilmesi sağlayan müşteri, ihtiyaçlarına karşılayabilirsiniz.

Hedefleri ve gereksinimleri bir sağlayıcının açısından en önemli bir listesini sağlar.

* **Sağlama**: Uygulama için yeni kiracılar sağlama olması gerekir.  Çok sayıda kiracılar ile çok kiracılı uygulamaları için Self Servis sağlama etkinleştirilerek bu işlemi otomatikleştirmek genellikle gereklidir.
* **Bakım**: Uygulama yükseltme ve birden çok kiracının kullanırken diğer bakım görevleri gerçekleştirmeniz mümkün olması gerekir.
* **İzleme**: Sorunları tanımlamak ve bunları gidermek için her zaman uygulamayı izleme olması gerekir. Bu, her Kiracı uygulamanın nasıl kullandığını izleme içerir.

Düzgün uygulanan bir çok kiracılı uygulaması kullanıcıların aşağıdaki avantajları sağlar.

* **Yalıtım**: Etkinlikleri tek bir kiracının başka kiracılar tarafından uygulama kullanımını etkilemez. Kiracılar, diğer tarafın verilere erişemez. Uygulamanın özel kullanımda olsa gibi kiracıya görünür.
* **Kullanılabilirlik**: Tek tek kiracılar uygulama belki de bir SLA'da tanımlanmış Garantisi ile sürekli kullanılabilir olmasını istiyorsunuz. Yeniden diğer kiracıların etkinliklerini uygulamanın kullanılabilirliğini etkilemeyecektir.
* **Ölçeklenebilirlik**: Uygulama, bireysel Kiracı talebi karşılamak üzere ölçeklendirir. Varlığı ve diğer kiracıların Eylemler uygulamanın kullanılabilirliğini etkilemeyecektir.
* **Maliyetleri**: Maliyetleri, çok kiracılı kaynaklarının paylaşımı sağladığından, ayrılmış, tek kiracılı bir uygulama çalıştıran daha düşüktür.
* **Özelleştirmeyi ölçme**. Uygulama ekleme veya özellik kaldırma, renkleri ve logoları değiştirme veya hatta kendi kod veya betik ekleme gibi çeşitli şekillerde tek bir kiracı için özelleştirme yeteneği.

Kısacası, yüksek oranda ölçeklenebilir bir hizmet sağlamak için dikkate almanız gereken birçok konuları olsa da, ayrıca vardır hedefleri ve gereksinimleri pek çok çok müşterili uygulamalar için ortak olan bir dizi. Bazı belirli senaryolarda ilgili olmayabilir ve bireysel hedefleri ve gereksinimleri önemini her senaryoda farklılık gösterir. Çok kiracılı uygulama sağlayıcısı olarak, ayrıca hedefleri ve gereksinimleri gibi kiracının hedefleri ve gereksinimleri, kârlılığı, faturalama, birden çok hizmet düzeyleri, sağlama, devamlılık izleme ve Otomasyon toplantı da sahip olacaksınız.

Çok kiracılı bir uygulamanın diğer tasarım konuları hakkında daha fazla bilgi için bkz. [azure'da çok Kiracılı bir uygulama barındırma][Hosting a Multi-Tenant Application on Azure]. Çok kiracılı hizmet olarak yazılım (SaaS) veritabanı uygulamalarının ortak veri mimarisi düzenlerine ilişkin bilgi için bkz. [Azure SQL Veritabanı ile Çok Kiracılı SaaS Uygulamaları için Tasarım Düzenleri](sql-database/sql-database-design-patterns-multi-tenancy-saas-applications.md). 

Azure, çok kiracılı bir sistem tasarlanırken karşılaşılan temel sorunları ele almak birçok özellik sağlar.

**Yalıtım**

* Segment Web sitesi kiracılar tarafından barındırma üstbilgileri veya kaydetmeden SSL iletişimi
* Web sitesi kiracılar sorgu parametreleri tarafından segmentlere ayırın.
* Web Hizmetleri, çalışan rollerinde
  * Genellikle bir uygulamanın arka uç verileri işleyen çalışan rolleri.
  * Web rolleri genellikle uygulamalar için ön uç işlevi görür.

**Depolama**

Veri Yönetimi Hizmetleri, büyük miktarda yapılandırılmamış verileri depolama ve Blob hizmeti, büyük miktarda yapılandırılmamış metin depolamak için hizmetleri sağlayan sağlayan tablo hizmeti gibi Azure SQL veritabanı veya Azure depolama hizmetleri gibi veya video, ses ve görüntü gibi ikili veriler.

* Çok Kiracılı verileri SQL veritabanı'nda güvenlik altına alma SQL Server oturumları Kiracı başına.
* Azure tabloları, kapsayıcı düzeyi erişim ilkesi belirterek için uygulama kaynakları kullanarak, yeni URL'leri paylaşılan erişim imzaları ile korumalı kaynaklar için vermek zorunda kalmadan izinleri ayarlama olanağı sağlayabilirsiniz.
* Azure Kuyrukları için uygulama kaynakları Azure kuyrukları, kiracılar adına sürücü işleme için yaygın olarak kullanılır, ancak sağlama veya Yönetim için gereken işi dağıtmak için de kullanılabilir.
* Hizmet veri yolu kuyrukları gönderim uygulama kaynakları için iş paylaşılan bir hizmet, her Kiracı gönderen yalnızca izinleri (ACS'den verilen talepleri türetilen gibi) sahip olduğu tek bir kuyruk kullanabilirsiniz yalnızca alıcılar hizmetinden olmakla birlikte, bu kuyruğa göndermek için kuyruktan birden çok kiracıdan gelen veri isteme izni.

**Bağlantı ve güvenlik hizmetleri**

* Azure Service Bus, dayanıklılık ve Gelişmiş ölçek için gevşek bir şekilde mesaj alışverişi uygulamalar arasında yer alan bir Mesajlaşma altyapısıdır.

**Ağ Hizmetleri**

Azure kimlik doğrulamasını desteklemek ve yönetilebilirliğini barındırılan uygulamalarınızı geliştirmek, çeşitli ağ hizmetleri sağlar. Bu hizmetler şunları içerir:

* Azure sanal ağ sağlar, sağlama ve Azure'da sanal özel ağlar (VPN'ler) yönetmek hem de güvenli bir şekilde bağlantı şirket içi BT altyapısı.
* Sanal ağ Traffic Manager, ister aynı veri merkezinde ister dünyanın dört bir yanındaki farklı veri merkezlerinde çalışıyor olsun çoklu barındırılan Azure hizmetlerinde gelen trafiğin yükünü dengelemenizi yüklenecek sağlar.
* Azure Active Directory (Azure AD), bulut uygulamalarınız için kimlik yönetimi ve erişim kontrolü özellikleri sağlayan modern, REST tabanlı bir hizmettir. Azure AD için uygulama kaynaklarını kullanarak web uygulamaları ve Hizmetleri kimlik doğrulaması ve yetkilendirme, kodunuzu faktörlenebilen için özelliklerinin verirken erişmek için kimlik doğrulama ve kullanıcıları yetkilendirme kolay bir yol sağlar.
* Azure Service Bus, güvenli Mesajlaşma sağlar ve veri akış özelliği için dağıtılmış ve karmaşık güvenlik duvarı ve güvenlik gerek kalmadan karma uygulamaları, Azure arasındaki iletişimi gibi uygulamalar ve şirket içi uygulamalar ve hizmetler, barındırılan altyapıları. Uç noktaları olarak kullanıma sunulan hizmetlere erişmek için uygulama kaynakları için Service Bus Geçişi'ni kullanarak (örneğin, şirket içi gibi sistemin dışında barındırılan) kiracısına ait olabilir ya da (çünkü özellikle Kiracı için sağlanan hizmetler olabilir duyarlı, kiracıya özgü verileri bunların arasında dolaşır).

**Kaynak sağlama**

Azure, bir uygulama için yeni kiracılar sağlama için çeşitli yöntemler sağlar. Çok sayıda kiracılar ile çok kiracılı uygulamaları için Self Servis sağlama etkinleştirilerek bu işlemi otomatikleştirmek genellikle gereklidir.

* Çalışan rolleri ölçümü kullanın ve belirli bir zamanlamaya uygun ölçekte yönetin veya yanıt olarak ana performans eşiklerini kesen ölçümleri toplamak, kaynakları (örneğin, ne zaman yeni bir kiracı kaydolduğunda veya iptal eder) sağlama ve Kiracı başına yinelenenleri sağlama olanak sağlar Göstergeleri. Bu aynı rol güncelleştirmeleri göndermek için de kullanılabilir ve çözüme yükseltmeleri.
* Azure BLOB'ları işlem sağlamak için kullanılan ya da önceden başlatılan işlem korumak yeni kiracılar kapsayıcı düzeyinde erişim ilkeleri sağlarken için depolama kaynaklarını hizmet paketleri, VHD görüntüleri ve diğer kaynaklar.
* SQL veritabanı kaynakları için bir kiracı sağlama için Seçenekler şunlardır:
  
  * DDL betiklerdeki veya katıştırılmış kaynaklar derlemelere içinde.
  * SQL Server 2008 R2 DAC program aracılığıyla dağıtılan paketler.
  * Bir ana başvuru veritabanından kopyalanıyor.
  * Veritabanı içeri aktarma ve dışarı aktarma dosyasından yeni veritabanları sağlamak için kullanma.

<!--links-->

[Hosting a Multi-Tenant Application on Azure]: https://msdn.microsoft.com/library/hh534480.aspx
[Designing Multitenant Applications on Azure]: https://msdn.microsoft.com/library/windowsazure/hh689716
