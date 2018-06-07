---
title: Azure SQL veritabanı Azure örnek olay incelemesi - Snelstart | Microsoft Docs
description: Nasıl SnelStart SQL veritabanı hızlı bir şekilde genişletilmiş için iş hizmetlerinin 1.000 yeni Azure SQL veritabanlarının ayda bir hızda kullandığını öğrenin
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: reference
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: carlrab
ms.openlocfilehash: 6a82c57f3540aaf160a61fd5fe3b01919dd9109e
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34649134"
---
# <a name="with-azure-snelstart-has-rapidly-expanded-its-business-services-at-a-rate-of-1000-new-azure-sql-databases-per-month"></a>Azure ile SnelStart hızlı bir şekilde iş hizmetlerinin 1.000 yeni Azure SQL veritabanlarının ayda bir hızda genişletilmiştir
![SnelStartLogo](./media/sql-database-implementation-snelstart/snelstartlogo.png)

SnelStart popüler finansal ve iş-yönetimi yazılımı küçük ve orta ölçekli işletmeler (SMB) Hollanda yapar. 55,000 müşterilerine 110 çalışanlar, bir BT personeli 35 dahil olmak üzere bir personeli tarafından sunulur. Azure üzerinde oluşturulmuş bir yazılım olarak-hizmet (SaaS) teklifi için Masaüstü yazılımlarını taşıyarak, SnelStart en yerleşik Hizmetleri tanıdık ortamlarında C# kullanarak ve performans ve ölçeklenebilirlik hiçbiri üzerinden - tarafından en iyi duruma getirme Yönetimi otomatikleştirme yapılan veya Esnek havuzları kullanan altında sağlama işletmeler. Azure kullanarak şirket içi ve bulut arasında taşıma müşteriler ortadan SnelStart sağlar.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-Case-Study-SnelStart/player]
> 
> 

## <a name="why-snelstart-extended-services-from-the-desktop-to-the-cloud"></a>Buluta neden SnelStart Hizmetleri masaüstünden genişletilmiş
> "Azure ile çalışma biz yazılım daha hızlı teslim etmek, hızlı bir şekilde müşteri taleplerini tepki taleplerini artırdığınızda çözümlerini ölçeklendirme anlamına gelir."
> 
> — Henry bırakıldı, Yazılım Mimarı
> 
> 

SnelStart çalışan başarılı yazılım iş yıllardır, geleneksel geliştirme modelini kullanarak: kod, paketi, sevk ve yineleyin. Zaman içinde büyüdü değişikliği hızını daha hızlı ve daha hızlı. Düzenlemeleri sık değiştirilen ve müşteriler finansal kayıtlarını işlemek ve kendi hesap uzmanları ve kamu bu değişikliklerle tutmaya ile birlikte çalışmak için daha kolay şekilde gerekli.

"Sevkiyat yazılım müşterilere Henry bırakıldı, Yazılım Mimarı göre pahalı ve sınırlama,". Ne sıklıkta biz yazılımı yayınlamak "üretim, paketleme ve nakliye maliyetleri sınırlı. Biz düzenli sevkiyat paket güncelleştirmelerini gerekiyordu, ancak, müşterilerimizin değişen gereksinimlerini karşılamak sabit yapılan. Biz hiçbir zaman müşterilerimizin en son ürün sürümüne yükselttiğiniz gösterilmeyeceği. Bu nedenle, biz birden çok sürümü desteklemek destek iş yanı sıra çok zor hale var."

Edilmiş ekler, "biz program ve sürüm güncelleştirmeleri bir hızlandırılmış şekilde istedik hızını biz daha hızlı yenilik ve müşterilerimiz için yeni hizmetler oluşturmak için. Biz de müşterilerimizin iş yönetimi ihtiyaçlarını basitleştirmek için daha fazla süreçlerini otomatikleştirmek için bir yol istedik."

SnelStart için bir bulut tabanlı SaaS sağlayıcısı olma tarafından hizmetlerinin genişletmek için çözüm bulunuyordu. Azure SQL veritabanı platform vardır-olarak-hizmet altyapı (Iaas) çözümünü gerekir önemli BT yükünü yansıtılmasını olmadan alma SnelStart olunmasına yardımcı oldu.

Bulut modeli de hataları düzeltin ve yeni özellikler hızlı bir şekilde, indirin ve yazılımları yükseltmek ihtiyaç duyan müşteriler sağlamak SnelStart sağlar. Göre bırakıldı "kullanarak Azure bulut hizmetleri sağlar bize üçüncü tarafların gereksinimlerini değiştirdikten sonra hızlı bir şekilde davranır. Müşteriler binlerce için yeni bir sürüm sevk gerek kalmadan yerine, biz Masaüstü uygulamamız üçüncü taraflar tarafından gerekli yeni biçimlerine gönderilen bilgi uyarlayabilirsiniz."

## <a name="a-modern-company-with-traditional-roots"></a>Geleneksel kökleri modern şirketle
SnelStart genel bir müzik Gereci bölümleri üretici olarak şirket başladığında 1964 dating humble kökleri ile modern, Çevik, yüksek teknoloji bir iştir. SnelStart yazılım iş Kalp gerçekten 1980'lerin, kişisel bilgisayar artışı sırasında vuruş başlatıldı. Şirket anda hesap yazılım daha iyi bir alternatiftir gerekli, bu nedenle kendi ellerini içine önemlidir sürdü. 1982 içinde şirket ne sonunda SnelStart hesap yazılım olacak temel oluşturuldu. Basitliği ve hız için en başından itibaren yazılım admired — çok böylece şirket sonunda değiştirilen odak ve kendisi bir yazılım şirketine reinvented.

1995'te, ilk Windows muhasebe uygulama SnelStart yayımladı. Microsoft Visual Basic 1.0 ve Microsoft Access veritabanlarında, yerleşik uygulama, müşteri 5. 000'den fazla kullanıcılara temel büyümesine olunmasına yardımcı oldu.

Bugün, SnelStart bir ana iş kolu (LOB) ve iş yönetimi uygulama Felemenkçe SMB'ler ve kendi kendine employed Girişimciler hedefleyen sağlayıcısıdır. Carlo Kuip, BT Mimarı diyor, "Amacımız yüzde 100 Otomasyon müşterilerimiz için iş yönetimi hizmetleri sağlamak için aynıdır."

## <a name="optimizing-performance-and-cost-with-elastic-pools"></a>Performans ve esnek havuzlar maliyetle en iyi duruma getirme
Esnek havuzların büyük ölçekli bir erken katılım SnelStart oluştu. Esnek havuzlar maliyetleri sınırlamak ve performans gereksinimlerini daha verimli bir şekilde yönetme şirket yardımcı olur. Göre bırakıldı "esnek havuzlar kullanarak, biz en iyi duruma getirebilir aşırı sağlama olmadan, müşterilerimizin ihtiyaçlarına göre performans. Biz sağlamak en yüksek yüküne göre olsaydı, oldukça maliyetli olacaktır. Bunun yerine, birden çok arasında kaynakları paylaşmak için seçeneği düşük kullanım veritabanları bize iyi gerçekleştiren ve uygun maliyetli bir çözüm oluşturmak izin verir. "

## <a name="azure-sql-databases-help-containerize-data-for-isolation-and-security"></a>Azure SQL veritabanları yalıtım ve güvenlik için verileri containerize Yardım
Azure SQL veritabanı kolayca ve şeffaf bir şekilde müşterilerin içi Azure iş yönetimi veri taşımak SnelStart sağlar. Azure SQL veritabanı sağlayan yalıtımı, bir sınır kimlik doğrulama, yetkilendirme ve kolay yedekleme ve geri yükleme özellikleri için kullanışlı bir kapsayıcıdır. Veritabanları iş yönetimi için oldukça uygun kavramsal model sağlar. Carlo Kuip, BT Mimarı göre "öğeleri bu kapsayıcı sınırları içinde bir iş için kritik önem taşıyan hassas verileri içeren, ve bu öğeleri yalıtılmış bir veritabanında saklamak bunları iyi korumalı tutar. Biz veritabanı düzeyinde yetkilendirme yönetebilir ve Veritabanı Yöneticileri (DBAs) çalışanlarını gerek kalmadan yönetim ve bu veritabanlarının genişleme bile otomatikleştirmek."

Azure SQL Data Warehouse aynı zamanda bir rol SnelStart güvenlik ve yönetim yazıdaki izinsiz giriş algılama, kullanıcı etkinlik günlüğü ve bağlantısı gibi telemetri verilerini toplamak şirket yardımcı olarak oynar.

## <a name="azure-removes-overhead-so-that-developers-can-spend-more-time-delivering-value"></a>Böylece geliştiriciler değeri teslim daha fazla zaman harcayabilir azure yükü kaldırır
Azure platformu modeli altyapı yükünü kaldırıldı ve C# yönetim kitaplıklarını kullanarak dağıtımları otomatikleştirmek SnelStart etkin. Kuip durumları gibi "bizim istemciler için ölçeklenebilirlik, hızlı ve olağanüstü durum kurtarma seçenekleri aynı anda artırırken çok küçük bir ekibiniz geçerli bizim işlemleriyle büyümeye bulduk. Kaydırma Hizmetleri geliştirme yeni hizmetler ve özellikler, yalnızca yeni düzenlemelere yukarı tutmak için var olan kodu veya vergi kodları güncelleştirme yerine odaklanmak için kaynakları serbest." Hüseyin, "tarafından yönetim otomatikleştirme ve sunumu, size daha fazla değer müşterilerimizin işletimsel personeli büyük yatırımlar yapmak zorunda kalmadan gönderemez SaaS kullanıyor." ekler Örneğin, Azure ve esnek havuzlar kullanarak SnelStart yeni hizmetleri, küçük işletme arka plan kontrollerinden ve e-posta hizmetleri faturalama yeni özellikler, Banka ile daha sağlam müşteri verileri tümleştirme de dahil olmak üzere çeşitli ekleyebilirsiniz.

> "Yalnızca ilk birkaç ay içinde 2016, biz hakkında 5,500 bizim Azure SQL veritabanı dağıtımları birden fazla 12.000 genişletilmiş ve biz aylık yaklaşık 1.000 veritabanları şu anda ekliyoruz."
> 
> — Henry bırakıldı, Yazılım Mimarı
> 
> 

Veritabanı Yönetimi, daha esnek iş özelliğini kullanarak otomatik hale getirilmiştir. Kuip durumları gibi "yüksek oranda SQL DB [sunucu] örneğine veritabanı otomatik olarak bulmayı veriyoruz." Bu ek yükü temel kendi dinamik olarak artan müşteri üzerinden yönetim işlemlerini yürütmek SnelStart sağlar.

SnelStart de müşteri verilerini üçüncü taraf yazılım iş ortakları tarafından oluşturulan uygulamaları arasındaki bir aracı gibi davranan bir API geliştirme. Kuip durumları olarak "Bu API veri girişi faturaları ve diğer belgeler için ortadan gibi bizim yazılım işlevselliği eklemek için diğer satıcıları etkinleştirir." Müşteriler artık faturalar küçük işletme hesap yazılımlarını el ile yazmanız gerekir; SnelStart yazılım gerekli bilgileri doğrudan değiştirecektir. Bu iş yönetim görevlerini endüstri dijital dönüşümünde gelen Gelişmekte olan bilgi ekosistemi ile birleştirme olanak tanır.  

## <a name="how-azure-services-enable-saas-for-snelstart"></a>Azure Hizmetleri için SnelStart SaaS etkinleştirmek nasıl
Azure kullanarak SnelStart müşterilerine ve daha sorunsuz bir şekilde bulutta kendi muhasebe hizmet verebilir. Örneğin, müşteriler ve muhasebe bilgileri Azure üzerinde barındırılan SnelStart'ın istemci API doğrudan erişerek paylaşabilirsiniz. Kuip durumları, "Bu yeniden kullanılabilir hizmetleri bizim müşteri kullanıma yönelik web uygulamaları için kullanılabilir ve ortak altyapı ve müşteriler için iş yönetimi ve müşterilerimizin tarafından kullanılan üçüncü taraf yazılımlar için erişime izin verecek şekilde işlevleri sağlarlar. Ürün yapılandırma yetenekleri sağlayan, güvenlik duvarı kurallarını yönetme ve uzun süre çalışan işlemleri yedeklemeler gibi yönetme örnekler."

> Amacımız yüzde 100 Otomasyon müşterilerimiz için iş Yönetimi Hizmetleri sağlamaktır." 
> 
> — Carlo Kuip, BT Mimarı
> 
> 

Ayrıca, SnelStart web Hizmetleri, müşteriler ve muhasebe kolayca Azure SQL Database esnek havuzlar verilerine erişmek izin verin. Veritabanı esneklik ve Azure Resource Manager ile birlikte bu SaaS modeli SnelStart her Azure dağıtım tamamlayan ölçeklenebilirlik özelliklerle sağlar. Uygulama, tamamen C# yönetim kitaplıklarını kullanarak otomatik hale getirilmiştir.

![SnelStart mimarisi](./media/sql-database-implementation-snelstart/figure1.png)

Şekil 1 '. Haziran 2016 itibariyle SnelStart, birden fazla 11.000 veritabanları ve 50'den fazla esnek havuzlar sahiptir.

## <a name="simplicity-from-the-cloud"></a>Basitlik bulutta
Bir Azure bulut tabanlı çözüm ile taşıma itibaren SnelStart yenilikçi özellikleri ve Hizmetleri sunarken hızlı müşteri büyüme desteği mümkün olmuştur. Göre bırakıldı "Azure ile biz teslim edebilir yakın sürekli güncelleştirmeleri müşterilerimiz için bizim personelinin genişletmeden. Ve biz diğer tüm harika Azure özelliklerini alın — ister ölçeklenebilirlik ve olağanüstü durum kurtarma — içinde paketlenmiş. "

> "Ne zaman gerçekte üzerinden Redmond bulamadığımız... Belirli bir sorun hakkında bana telefon bir geliştiriciden Hollanda edilene bir çağrı aldım. Bir değişiklik bizim sorunu çözmek için 48 saat içinde üretim ortamlarında sunmak için Microsoft almak bulduk."
> 
> — Henry bırakıldı, Yazılım Mimarı
> 
> 

SnelStart aynı zamanda Microsoft Azure SQL DB ekibiyle geliştirdik güçlü ortaklığı appreciates. "Tartışmalara özelliklere sahip olduğumuz ve iki tarafta bir şeyin teknolojisini kullanmak nasıl takdir." Kuip durumları olarak
Hemen SnelStart için kendi memnun Müşteri temel büyüyor için belirtilir. Edilmiş olarak durumları, "biz ISV vardı teknik ve kaynak kısıtlamaları, biz ne kadar büyüyebilir için sınır yoktur."

## <a name="more-information"></a>Daha fazla bilgi
* Azure esnek havuzları hakkında daha fazla bilgi için bkz: [esnek havuzlar](sql-database-elastic-pool.md).
* Azure SQL Data Warehouse hakkında daha fazla bilgi için bkz: [SQL veri ambarı](https://azure.microsoft.com/documentation/services/sql-data-warehouse/)
* SnelStart hakkında daha fazla bilgi için bkz: [SnelStart](http://www.snelstart.nl).

