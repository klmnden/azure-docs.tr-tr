---
title: Azure SQL veritabanı Azure örnek olay incelemesi - Snelstart | Microsoft Docs
description: SnelStart SQL veritabanı için hızlı genişletilmiş bir hızda ayda 1.000 yeni Azure SQL veritabanı hizmetlerini kullanma hakkında bilgi edinin
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: reference
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: carlrab
ms.openlocfilehash: c919a3cb47017d2f65b141e358ab318f4b764627
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44713723"
---
# <a name="with-azure-snelstart-has-rapidly-expanded-its-business-services-at-a-rate-of-1000-new-azure-sql-databases-per-month"></a>Azure ile SnelStart ayda 1.000 yeni Azure SQL veritabanı bir hızda Kurumsal hizmetlerini hızla genişletildiğinden
![SnelStartLogo](./media/sql-database-implementation-snelstart/snelstartlogo.png)

SnelStart, Hollanda popüler finansal ve işletme-yönetimi yazılımı küçük ve orta ölçekli işletmelerin (SMB'ler) sağlar. Bir BT personeliniz 35'ın da dahil olmak üzere, 110 çalışanlar personeli tarafından 55.000 müşterilerine sunulur. Masaüstü yazılımlarını Azure üzerinde derlenen bir hizmet olarak yazılım-a-(SaaS) teklifi taşıyarak, SnelStart en yerleşik Hizmetleri kitaplıklarıyla C# kullanarak ve performans ve ölçeklenebilirlik hiçbiri üzerinden tarafından iyileştiren yönetimini otomatikleştirme yapılan veya Esnek havuzları kullanan sağlama işletmeler. SnelStart, Azure'ı kullanarak şirket içi ve bulut arasında taşınan müşterilerin hızlı sağlar.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-Case-Study-SnelStart/player]
> 
> 

## <a name="why-snelstart-extended-services-from-the-desktop-to-the-cloud"></a>Buluta neden SnelStart Hizmetleri masaüstünden genişletilmiş
> "Azure ile çalışma yazılımları daha hızlı sunun, hızla Müşteri taleplerine react ve talepleri artırdığınızda çözümlerini ölçeklendirme anlamına gelir."
> 
> — Henry Been, Yazılım Mimarı
> 
> 

SnelStart çalıştırdığınız başarılı yazılım iş yıl boyunca geleneksel geliştirme modelini kullanarak: kod, paketleme, gönderme ve yineleyin. Zaman içinde değişiklik hızını daha hızlı bir şekilde büyüdü ve daha hızlı. Düzenlemelere sık değiştirilenlerin ve müşterilere finansal kayıtlarını işlemek ve kendi müşavirler ve kamu bu değişikliklerle kalmasını sağlamak için birlikte çalışmak için daha kolay yollar gerekli.

"Yazılım müşterilere Henry Been, Yazılım Mimarı göre pahalı ve kısıtlayıcı,". Biz yazılımı ne sıklıkta serbest bırakamadı "üretim, paketleme ve nakliye maliyetlerini sınırlı. Dönemsel sevk irsaliyesi için paket güncelleştirmeleri vardı, ancak bu, müşterilerimizin değişen gereksinimlerini karşılamak sabit yapılan. Biz hiçbir zaman müşterilerimizin en son ürün sürümüne yükseltilmiş yararlandığından emin. Bu nedenle, birden çok sürümü desteklemek destek işi yanı sıra çok zor hale vardı."

Edilmiş ekler, "program ve sürüm güncelleştirmeleri hızlı bir şekilde istedik hızını, böylece biz daha hızlı yenilik yapın ve müşterilerimiz için yeni hizmetler oluşturmak. Ayrıca müşterilerin iş yönetimi ihtiyaçlarını basitleştirmek için daha fazla işlemleri otomatik hale getirmek için bir yol istedik."

SnelStart için çözüm Hizmetleri olma bir bulut tabanlı SaaS sağlayıcısı tarafından genişletmek için oluştu. Azure SQL veritabanı platform-olarak-hizmet altyapı (Iaas) çözümünü yapmamızı gerektiriyordu; önemli BT ek yükünü ödemeden gitmek SnelStart yardımcı olmuştur.

Bulut modeli SnelStart hataları düzeltin ve yeni özellikleri hızla, indirin ve yazılım yükseltme gerek olmayan müşteriler sağlamak de sağlar. Şunlara göre bırakıldı "kullanarak Azure bulut Hizmetleri olanak sağlıyor gereksinimleri Üçüncü taraflardan değiştirme sırasında hızlı bir şekilde yapacak. Binlerce müşteri için yeni bir sürüm göndermek zorunda yerine, size üçüncü taraflarca gereken yeni biçimleri müşterilerimizin Masaüstü uygulamasından gönderilen bilgiler uyarlayabilirsiniz."

## <a name="a-modern-company-with-traditional-roots"></a>Geleneksel kökleri ile modern bir şirket
SnelStart, 1964 kurucularıyla şirket Müzik Aleti bölümlerinin bir üretici olarak başlatıldığında için ilk humble kökleri ile modern, Çevik, yüksek teknoloji bir iştir. SnelStart yazılım iş kalbi gerçekten 1980'lerin kişisel bilgisayar çoğalan sırasında beating başlatıldı. Önemli olan konuya, kendi uygulamalı olarak sürdüğü için daha iyi bir alternatif hesap yazılım anda kullanılabilir şirket gerekli. 1982 içinde şirket ne sonunda SnelStart muhasebe yazılım olacak temel oluşturuldu. Başlangıçtan itibaren hız ve kolaylık olması için yazılım admired — çok böylece şirket sonunda odak değiştirildi ve kendisini bir yazılım şirketine yeniden oluşturuldu.

1995'te, ilk muhasebe uygulama Windows için SnelStart yayımladı. Microsoft Visual Basic 1.0 ve Microsoft Access veritabanları üzerinde oluşturulmuş bir uygulama müşteri 5. 000'den fazla kullanıcılara temel büyümesine yardımcı oldu.

Bir iş kolu (LOB) ve iş yönetimi uygulama Felemenkçe SMB'ler ve şirket içinde çalıştırılan Girişimciler hedefleyen SnelStart Bugün, büyük bir sağlayıcıdır. Carlo Kuip, BT Mimarı, diyor gibi "Hedefimiz yüzde 100 müşterilerimiz için iş Yönetimi Hizmetleri otomasyonunu sağlamaktır."

## <a name="optimizing-performance-and-cost-with-elastic-pools"></a>Performans ve maliyet elastik havuzlar sayesinde en iyi duruma getirme
SnelStart, elastik havuzlar, büyük ölçekli bir erken benimseyen oluştu. Elastik havuzlar şirket maliyetleri sınırlamak ve performans gereksinimlerini daha verimli yönetilmesine yardımcı olur. Şunlara göre bırakıldı "elastik havuzlarını kullanarak, biz aşırı sağlama olmadan, müşterilerimizin ihtiyaçlarına göre performansını iyileştirin. Sağlamak en yüksek yüküne göre vardı, oldukça yüksek maliyetli olabilir. Bunun yerine, birden çok arasında kaynakları paylaşabilirsiniz düşük kullanım veritabanları da gerçekleştiren ve uygun maliyetli bir çözüm oluşturmak bize izin verir. "

## <a name="azure-sql-databases-help-containerize-data-for-isolation-and-security"></a>Azure SQL veritabanları, yalıtım ve güvenlik verileri kapsayıcılı hale getirme Yardım
Azure SQL veritabanı, müşterilerin şirket iş yönetimi verileri azure'a kolayca ve şeffaf bir şekilde taşımak SnelStart sağlar. Azure SQL veritabanı kimlik doğrulaması, yetkilendirme ve kolay yedekleme ve geri yükleme özellikleri için yalıtımı, bir sınır sağlayan kullanışlı bir kapsayıcıdır. Veritabanları, iş yönetimi için uygun bir kavramsal model sağlar. Carlo Kuip, BT Mimarı, göre "öğeleri bu kapsayıcının sınırları içinde bir iş için kritik öneme sahip olan hassas veriler içeren ve bu öğeleri yalıtılmış bir veritabanında saklamak bunları iyi korumalı tutar. Şu veritabanı düzeyinde yetkilendirme yönetebilir ve Veritabanı Yöneticileri (Dba'lar) çalışanlarını gerek kalmadan bu veritabanlarının ölçek genişletme ve yönetim bile otomatikleştirin."

SnelStart güvenlik ve yönetim hikayenizi ayrıca oynadığı rol izinsiz giriş algılama ve kullanıcı etkinlik günlüğü bağlantısı gibi telemetri verilerini toplamak şirket yardımcı olarak Azure SQL veri ambarı.

## <a name="azure-removes-overhead-so-that-developers-can-spend-more-time-delivering-value"></a>Böylece geliştiriciler değer sunmaya daha fazla zaman ayırmasına azure yükünü kaldırır
Azure platformu modeli, altyapı yükü kaldırıldı ve C# yönetim kitaplıklarını kullanarak dağıtımları otomatik hale getirmek SnelStart etkin. Kuip durumları gibi "biz çok küçük bir ekibiniz ile geçerli işlemlerimiz müşterilerimize için ölçeklenebilirlik, hız ve olağanüstü durum kurtarma seçenekleri aynı anda artırırken büyüme fırsatı yakaladık. Shift ile taşıma hizmetleri geliştirmeye yeni hizmetler ve özellikler, yalnızca yeni düzenlemelere yukarı korumak için var olan kod veya vergi kodları güncelleştirmek yerine odaklanmak için kaynakları serbest." Hüseyin, "tarafından yönetim otomatikleştirme ve operasyon personeli büyük yatırımlar yapmak zorunda kalmadan, müşterilerimizin daha fazla değer sunmak yükümlü sunan SaaS kullanarak." ekler Örneğin, Azure ve elastik havuzlar kullanarak SnelStart yeni hizmetleri, küçük işletme arka plan denetim ve e-posta hizmetleri faturalandırma yeni özellikler, bankalar, daha güçlü müşteri veri tümleştirme dahil olmak üzere çeşitli ekleyebilirsiniz.

> "Yalnızca ilk birkaç ay içinde 2016'ın, biz hakkında 5,500 bizim Azure SQL veritabanı dağıtımlarını 12.000'den fazla genişletilmiş ve ayda yaklaşık 1.000 veritabanı şu anda ekliyoruz."
> 
> — Henry Been, Yazılım Mimarı
> 
> 

Veritabanı yönetimi daha esnek işler özelliğini kullanarak otomatik hale getirilmiştir. Kuip durumları gibi "yüksek düzeyde bir SQL DB [sunucu] örneğindeki veritabanları otomatik olarak bulunmasını veriyoruz." Bu, üzerinde dinamik olarak büyüyen müşterisine ek yükü temel yönetim işlemlerini yürütmek SnelStart sağlar.

SnelStart, müşteri verileri ve üçüncü taraf yazılım iş ortakları tarafından oluşturulan uygulamalar arasında bir aracı gibi davranan bir API de geliştirme. Kuip durumları olarak "Bu API gibi faturaları ve diğer belgeler için veri girişinin ortadan yazılımımızı işlevselliği eklemek diğer satıcıları etkinleştirir." Artık müşteriler, faturalar küçük işletme muhasebesi yazılımlarını el ile yazın zorunda kalırlar. SnelStart yazılım gerekli bilgileri doğrudan değiştirecektir. Bu, müşterilerin dijital dönüşüm sektördeki gelen Gelişmekte olan bilgi ekosistemi ile iş-yönetim görevlerini katılın olanak tanır.  

## <a name="how-azure-services-enable-saas-for-snelstart"></a>Nasıl Azure Hizmetleri, SaaS SnelStart için etkinleştirin.
Azure'ı kullanarak, müşterilerine ve onların müşavirler daha sorunsuz bir şekilde bulutta SnelStart görebilir. Örneğin, hem müşterilerin hem de müşavirler bilgileri SnelStart'ın istemci API'si, Azure üzerinde barındırılan doğrudan erişerek paylaşabilirsiniz. Kuip durumları, "bizim müşterilere yönelik web uygulamaları için yeniden kullanılabilir hizmetlerin kullanılabilir ve ortak altyapı ve müşterilerin iş yönetimi ve müşterilerimiz tarafından kullanılan üçüncü taraf yazılım için erişime izin vermek için işlevler sağlar. Ürün yapılandırma özellikleriyle, güvenlik duvarı kurallarını yönetme ve yedeklemeler gibi uzun süre çalışan işlemleri yönetme verilebilir."

> Hedefimiz yüzde 100 müşterilerimiz için iş Yönetimi Hizmetleri otomasyonunu sağlamaktır." 
> 
> — Carlo Kuip, BT Mimarı
> 
> 

Ayrıca, SnelStart web Hizmetleri, hem müşterilerin hem de müşavirler kolayca Azure SQL veritabanı elastik havuzları verilere erişmek izin verin. Veritabanı elastiklik ve Azure Resource Manager ile birlikte bu SaaS modeli, her Azure dağıtım tamamlayan ölçeklenebilirlik özelliklerle SnelStart sağlar. Uygulama, tam olarak C# yönetim kitaplıklarını kullanmaya otomatik değildir.

![SnelStart mimarisi](./media/sql-database-implementation-snelstart/figure1.png)

Şekil 1. Haziran 2016 itibarıyla SnelStart, birden fazla 11.000 veritabanımız ve 50'den fazla elastik havuzlar vardır.

## <a name="simplicity-from-the-cloud"></a>Buluttan basitliği
Bir Azure bulut tabanlı çözüm ile taşıyarak bu yana SnelStart yenilikçi özellikler ve hizmetler sunan sırasında büyüme hızlı müşteri destekleyebilecek olmuştur. Şunlara göre olan "Azure sayesinde, biz teslim edebilir yakın güncelleştirmeleri müşterilerimiz için sunduğumuz operasyon personeli genişletmeden. Ve diğer tüm harika Azure özellikleri aldığımız — ister ölçeklenebilirlik ve olağanüstü durum kurtarma — içinde paketlenmiş. "

> "Ne zaman biz aslında üzerinden Redmond'da olan... Belirli bir sorun hakkında bana telefon bir geliştirici, Hollanda, geri gelen çağrı aldım. Biz, üretim ortamında, sorunu çözmek için 48 saat içinde bir değişiklik için Microsoft alınamadı."
> 
> — Henry Been, Yazılım Mimarı
> 
> 

SnelStart, ayrıca bunlar Microsoft Azure SQL DB ekibi ile geliştirdik güçlü iş ortaklığı appreciates. "Durum teknolojisi kullanmak nasıl her iki kenarı da takdir ve tartışmalar özellikleri sunuyoruz." Kuip durumları olarak
Hemen SnelStart için memnun müşterisi temel giderek büyüyor olmaktır. Edilmiş gibi durumları "bir ISV vardı teknik ve kaynak kısıtlamaları, biz ne kadar büyüyebilir için sınır yoktur."

## <a name="more-information"></a>Daha fazla bilgi
* Azure elastik havuzlar hakkında daha fazla bilgi için bkz: [elastik havuzlar](sql-database-elastic-pool.md).
* Azure SQL veri ambarı hakkında daha fazla bilgi için bkz: [SQL veri ambarı](https://azure.microsoft.com/documentation/services/sql-data-warehouse/)
* SnelStart hakkında daha fazla bilgi için bkz: [SnelStart](http://www.snelstart.nl).

