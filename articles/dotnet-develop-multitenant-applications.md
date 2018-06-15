---
title: Çok Kiracılı Web uygulama düzeni | Microsoft Docs
description: Mimari genel bakış ve Azure üzerinde bir çok kiracılı web uygulamasını uygulamak açıklar tasarım desenleri bulun.
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
ms.openlocfilehash: 57ba0e46139bda2d74c9f7db0ffab2f2122b0df2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23850717"
---
# <a name="multitenant-applications-in-azure"></a>Azure'da çok müşterili uygulamalar
Çok kiracılı uygulama farklı kullanıcılar veya "Kiracı," kendi olduğu gibi sorgulamanıza uygulamayı görüntülemek için izin veren paylaşılan bir kaynaktır. Kendi çok kiracılı bir uygulamaya uygundur tipik bir senaryo içinde uygulamanın tüm kullanıcılar kullanıcı deneyimini özelleştirme ancak Aksi halde aynı temel iş gereksinimleriniz isteyebilir biridir. Office 365, Outlook.com ve visualstudio.com büyük çok müşterili uygulamalar örnekleridir.

Bir uygulama sağlayıcının açısından bakıldığında, çoklu müşteri mimarisi yararları işletimsel ve maliyet verimliliği için çoğunlukla ilgilidir. Uygulamanızın bir sürümünü birçok kiracılar/sistem birleştirilmesi izleme, performans ayarlama, yazılım bakımı ve veri yedekleri gibi yönetim görevleri müşterilerinizle, ihtiyaçlarını karşılayabilir.

En önemli hedefleri ve gereksinimleri bir sağlayıcının açısından listesini sağlar.

* **Sağlama**: uygulama için yeni kiracılar sağlamak mümkün olması gerekir.  Çok sayıda kiracılar, çok müşterili uygulamalar için Self Servis sağlama etkinleştirerek bu işlemi otomatikleştirmek genellikle gereklidir.
* **Bakım**: Uygulama yükseltme ve birden çok kiracıya kullanırken diğer bakım görevlerini gerçekleştirmek mümkün olması gerekir.
* **İzleme**: herhangi bir sorunu belirlemek ve bunları gidermek için uygulamayı her zaman izleme olmalıdır. Bu, her bir kiracı uygulama nasıl kullanarak izlemeyi içerir.

Düzgün uygulanan bir çok kiracılı uygulama kullanıcılara aşağıdaki avantajları sağlar.

* **Yalıtım**: tek tek kiracılar etkinliklerini uygulama diğer kiracılar tarafından kullanımını etkilemez. Kiracılar, diğer kişilerin verilere erişemez. Uygulamanın özel kullanım olsa gibi Kiracı için görünür.
* **Kullanılabilirlik**: tek tek kiracılar istediğiniz uygulamanın belki de bir SLA tanımlanan garanti ile sürekli olarak kullanılabilir. Yeniden etkinlikler diğer kiracıların uygulamanın kullanılabilirliğini etkilemez.
* **Ölçeklenebilirlik**: uygulama ölçeklendirir tek tek kiracılar talebi karşılamak üzere. Varlık ve diğer kiracıların Eylemler uygulamanın performansını etkilemez.
* **Maliyetleri**: maliyetleri çoklu kiracı kaynak paylaşımı, sağladığından ayrılmış, tek kiracılı uygulama çalıştıran daha düşük.
* **Özelleştirilebilirliğini**. Uygulama ekleme veya özellik kaldırma, renklerini ve logolar, ekleme veya değiştirme bile kendi kod veya komut dosyası gibi çeşitli şekillerde tek bir kiracıya için özelleştirme yeteneği.

Kısacası, yüksek düzeyde ölçeklenebilir bir hizmet sağlamak için dikkate almanız gereken birçok konuları olsa da, de vardır hedefler ve pek çok kullanıcılı uygulamaları için ortak gereksinimler sayısı. Bazı belirli senaryolarda ilgili olmayabilir ve tek tek hedefleri ve gereksinimleri önemini her senaryoda farklılık gösterir. Bir çok kiracılı uygulama sağlayıcısı olarak da hedefleri ve gereksinimleri gibi kiracılar hedefleri ve gereksinimleri, Karlılık, faturalama, birden çok hizmet düzeyleri, sağlama, bakım izleme ve Otomasyon toplantı sahip olursunuz.

Çok kiracılı bir uygulamanın ek tasarım konuları hakkında daha fazla bilgi için bkz: [azure'da çok Kiracılı uygulama barındırma][Hosting a Multi-Tenant Application on Azure]. Çok kiracılı hizmet olarak yazılım (SaaS) veritabanı uygulamalarının ortak veri mimarisi düzenlerine ilişkin bilgi için bkz. [Azure SQL Database ile Çok Kiracılı SaaS Uygulamaları için Tasarım Düzenleri](sql-database/sql-database-design-patterns-multi-tenancy-saas-applications.md). 

Azure çok müşterili sistem tasarlanırken karşılaştı anahtar sorunları ele olanak sağlayan birçok özellik sağlar.

**Yalıtım**

* Segment Web sitesi kiracılar tarafından ana bilgisayar üstbilgisi ile veya olmadan SSL iletişimi
* Web sitesi kiracılar sorgu parametreleri tarafından segmentlere
* Çalışan rollerini Web Hizmetleri
  * Çalışan rolleri. genellikle bir uygulamanın arka uç verileri işlem.
  * Ön uç uygulamaları için genellikle görür web rolleri.

**Depolama**

Azure SQL Database veya Azure Storage Hizmetleri gibi hizmetleri, büyük miktarlarda yapılandırılmamış veri depolama ve büyük miktarlarda yapılandırılmamış metin veya ikili depolamak için hizmetleri sağlayan Blob hizmeti sağlayan tablo hizmeti gibi veri yönetimi video, ses ve görüntü gibi verileri.

* Çok kullanıcılı verileri uygun SQL veritabanında güvenlik altına alma başına-SQL Server oturumları Kiracı.
* Uygulama kaynakları bir kapsayıcı düzeyinde erişim ilkesini belirtme tarafından için Azure tabloları kullanarak yeni URL'SİNİN paylaşılan erişim imzaları ile korunan kaynaklar için sorun gerek kalmadan izinleri ayarlamanızı yapabilirsiniz.
* Uygulama kaynakları Azure sıralar Azure kuyrukları kiracılar adına sürücü işleme için yaygın olarak kullanılır, ancak sağlama veya Yönetim için gerekli iş dağıtmak için de kullanılabilir.
* Hizmet veri yolu kuyrukları iter uygulama kaynakları için iş paylaşılan bir hizmet, her Kiracı gönderen yalnızca izinleri (ACS verilen talepler türetilmiş gibi) sahip olduğu tek bir sıraya kullanabilirsiniz yalnızca alıcılar hizmetinden varken bu kuyruğa göndermek için birden çok kiracılardan gelen veriler sıradan isteme izni.

**Bağlantı ve güvenlik hizmetleri**

* Azure Service Bus, geliştirilmiş ölçeklendirme için kabaca bağlı şekilde ileti alışverişinde bulunmak için uygulamalar vermeden ve dayanıklılık arasında bulunur bir Mesajlaşma altyapısı.

**Ağ Hizmetleri**

Azure kimlik doğrulamayı desteklemek ve yönetilebilirliğini barındırılan uygulamalar geliştirmek, birçok ağ hizmetleri sağlar. Bu hizmetler şunlardır:

* Azure sanal ağ sağlar, sağlamak ve sanal özel ağları (VPN'ler) Azure'da yönetin yanı sıra bunları güvenli bir şekilde bağlantı şirket içi BT altyapısı.
* Sanal ağ trafik Yöneticisi'ni, aynı veri merkezinde veya farklı veri merkezlerinde dünya genelinde çalıştırdıkları olup olmadığını gelen trafiği dengelemek barındırılan birden çok Azure Hizmetleri genelinde yüklemenizi sağlar.
* Azure Active Directory (Azure AD) olduğu bulut uygulamalarınız için kimlik yönetimi ve erişim denetimi özelliklerinden sağlar modern, REST tabanlı bir hizmettir. Azure AD ile web uygulamaları ve Hizmetleri kodunuzu dışında oluşturmak için yetkilendirme ve kimlik doğrulaması özelliklerini verirken erişmek için kimlik doğrulaması ve Kullanıcıları yetkilendirmek kolay bir yol sağlar uygulama kaynakları için Azure AD kullanma.
* Azure Service Bus güvenli Mesajlaşma sağlar ve veri akış özelliği için dağıtılmış ve karmaşık güvenlik duvarı ve güvenlik gerek kalmadan Azure arasındaki iletişimi gibi karma uygulamalar uygulamalar ve şirket içi uygulamalar ve hizmetler, barındırılan altyapıları. Service Bus geçişi uç noktalar olarak sunulan hizmetler için uygulama kaynaklarını kullanmayı (örneğin, şirket içi gibi sistem dışında barındırılan) kiracıya ait olabilir veya Hizmetleri (çünkü özellikle Kiracı için sağlanan olabilir hassas, Kiracı özgü verileri bunların arasında geçen).

**Kaynak sağlama**

Azure uygulama için sağlama yeni kiracılar, çeşitli yollarla sağlar. Çok sayıda kiracılar, çok müşterili uygulamalar için Self Servis sağlama etkinleştirerek bu işlemi otomatikleştirmek genellikle gereklidir.

* Çalışan rollerini ölçümü kullanın ve belirli bir zamanlama uyarınca ölçek yönetmek için veya anahtar performans Eşiği aşma yanıta ölçümleri toplamak, kaynakları (örneğin, ne zaman yeni bir kiracı işaretlerini yukarı veya iptal eder) sağlama ve Kiracı başına devre dışı bırakma sağlamak için olanak sağlar Göstergeleri. Bu aynı rol ayrıca güncelleştirmeleri göndermek için kullanılabilir ve çözüme yükseltmeleri.
* Azure BLOB'ları işlem sağlamak için kullanılan ya da önceden başlatılmış depolama kaynaklarını işlem korumak yeni kiracılar kapsayıcı düzeyinde erişim ilkeleri sağlarken için hizmet paketleri, VHD görüntüleri ve diğer kaynakları.
* SQL veritabanı kaynakları için bir kiracı sağlama için Seçenekler şunlardır:
  
  * DDL komut dosyalarında veya derlemeleri içindeki kaynaklar olarak katıştırılmış
  * SQL Server 2008 R2 DAC program aracılığıyla dağıtılan paketler.
  * Bir ana başvuru veritabanından kopyalama
  * Veritabanını içeri aktarma ve dışarı aktarma dosyasından yeni veritabanları sağlamak için kullanma.

<!--links-->

[Hosting a Multi-Tenant Application on Azure]: http://msdn.microsoft.com/library/hh534480.aspx
[Designing Multitenant Applications on Azure]: http://msdn.microsoft.com/library/windowsazure/hh689716
