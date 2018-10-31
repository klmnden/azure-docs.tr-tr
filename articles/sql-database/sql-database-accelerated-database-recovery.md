---
title: Veritabanı kurtarma - Azure SQL veritabanı hızlandırılmış | Microsoft Docs
description: Azure SQL veritabanı, tek veritabanları, elastik havuzlar ve Azure SQL veri ambarı için hızlı ve tutarlı veritabanı kurtarma anlık bir işlem geri alma ve agresif günlük kesme sağlayan yeni bir özelliği vardır.
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 10/11/2018
ms.openlocfilehash: 20ad792ffa1c06e75cd39e4549b8d897a663db3e
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50239976"
---
# <a name="accelerated-database-recovery-preview"></a>Hızlandırılmış veritabanı kurtarma (Önizleme)

**Veritabanı kurtarma (ADR) hızlandırılmış** özellikle uzun olduğu durumda, veritabanı kullanılabilirlik büyük ölçüde geliştiren yeni bir SQL veritabanı altyapısı özelliği çalıştıran işlem, SQL veritabanı altyapısı kurtarma işlemini yeniden tasarlanmasını tarafından. ADR, tek veritabanları, elastik havuzlar ve Azure SQL veri ambarı şu anda kullanılabilir. ADR başlıca yararları şunlardır:

- **Hızlı ve tutarlı veritabanı kurtarma**

  ADR ile hızlı ve tutarlı veritabanı kurtarma etkin işlem sayısı ne olursa olsun, sistem veya boyutlarının etkinleştirme genel kurtarma zamanı uzun süre çalışan işlemleri etkilemez.

- **Anlık bir işlem geri alma**

  ADR ile işlemi geri alma işlemi etkin olduğu süre veya yürüttü güncelleştirme sayısı bağımsız olarak anlık.

- **Agresif günlüğünün kesilmesi**

  ADR ile işlem günlüğü agresif, bile etkin uzun süre çalışan işlemler varsa, bu denetimi dışında büyümesini önleyen kesilir.

## <a name="the-current-database-recovery-process"></a>Geçerli veritabanı kurtarma işlemi

SQL Server veritabanını Kurtarma aşağıdaki [ARIES](https://people.eecs.berkeley.edu/~brewer/cs262/Aries.pdf) kurtarma modelini ve diyagramda şu daha ayrıntılı olarak açıklanan ve aşağıdaki diyagramda gösterildiği üç aşamadan oluşur.

![Geçerli kurtarma işlemi](./media/sql-database-accelerated-database-recovery/current-recovery-process.png)

- **Analiz aşaması**

  Son başarılı denetim (veya en eski LSN sayfa) sona, SQL Server SE zastavil s zaman her bir işlem durumunu belirlemek için başından itibaren işlem günlüğünün taramayı iletin.

- **Aşama Yinele**

  Veritabanı yineleme tüm işlemler tarafından kilitlenme zamanında olduğu duruma getirmek için sonuna kadar eski işlenmemiş işlemden işlem günlüğünün taramayı iletin.

- **Aşama Geri Al**

  Kilitlenme süresini itibariyle etkin olduğu her bir işlem için bu işlem gerçekleştirilen işlemleri geri alma günlük geriye doğru erişir.

Bu tasarımına bağlı olarak, SQL veritabanı altyapısı beklenmeyen bir yeniden başlatma kurtarmak için gereken süreyi sistemde kilitlenme anda (yaklaşık) en uzun etkin işlem boyutunu orantılıdır. Kurtarma tamamlanmamış tüm işlemler, bir geri alma gerektirir. İşlem gerçekleştirdi iş için gereken süre uzunluğunu orantılıdır ve zaman etkin olmuştur. Bu nedenle, (büyük toplu ekleme gibi işlemleri veya dizin oluşturma işlemleri büyük bir tabloda) SQL Server kurtarma işlemi uzun süre çalışan işlemler oradayken uzun zaman alabilir.

Yukarıda açıklandığı gibi aynı geri kurtarma aşamasını kullanarak olarak da iptal ediliyor/bu tasarıma göre büyük bir işlem geri ayrıca bir uzun zaman alabilir.

Ayrıca, olduğunda uzun SQL veritabanı altyapısı işlem günlüğü kesilemiyor karşılık gelen günlük kayıtlarını kurtarma ve geri alma işlemleri için gerekli olduğu işlemler çalışıyor. Bu SQL veritabanı altyapısı tasarımı sonucu olarak, bazı müşteriler işlem günlüğü boyutu çok büyük büyür ve günlük alanının büyük miktarlarda tüketir sorun karşı karşıyadır.

## <a name="the-accelerated-database-recovery-process"></a>Hızlandırılmış veritabanı kurtarma işlemi

ADR tamamen SQL veritabanı altyapısı kurtarma işlemini yeniden tasarlanmasını yukarıdaki sorunları ele alır:

- Sabit olun zaman/anında/en eski aktif işlem başına günlük tarama zorunda tarafından. ADR ile son başarılı denetim noktasından (veya eski kirli sayfasındaki günlük sıra Number(LSN). yalnızca işlem günlüğü işlenir Sonuç olarak, Kurtarma süresi uzun tarafından etkilenmez işlemlerin çalıştırılması.
- Artık bu yana tam işlem günlüğü işlem yapmanız gerekli işlem günlüğü alanını en aza indirin. Sonuç olarak, işlem günlüğü agresif bir biçimde kontrol noktaları kesilebiliyorsa ve yedeklemelerin.

Yüksek bir düzeyde ADR, hızlı veritabanı kurtarma tüm fiziksel veritabanı değişikliklerini ve sınırlıdır ve neredeyse anında geri alınabilir yalnızca geri alma mantıksal işlemleri, sürüm oluşturma tarafından ulaşır. Bir kilitlenme süresini itibariyle etkin herhangi bir işlem iptal edildi olarak işaretlenmiş ve bu nedenle, bu işlemler tarafından oluşturulan tüm sürümleri eş zamanlı kullanıcı sorgular tarafından göz ardı edilebilir.

ADR kurtarma işlemi, geçerli kurtarma işlemi olarak aynı üç aşamadan oluşur. Bu aşamalar ADR ile nasıl çalışır? Aşağıdaki diyagramda gösterildiği ve diyagramı aşağıdaki daha ayrıntılı olarak açıklanmıştır.

![ADR kurtarma işlemi](./media/sql-database-accelerated-database-recovery/adr-recovery-process.png)

- **Analiz aşaması**

  İşlem bugün sLog yeniden oluşturuluyor ve günlük kayıtları tutulmayan ops için kopyalama eklenmesi ile aynı kalır.
- **Yinele** aşaması

  Ayrılmış iki aşamaya (P)
  - 1. Aşama

      SLog (son denetim noktasından kadar eski işlenmemiş hareket) öğesinden yineler. Yalnızca birkaç kayıtlardan sLog işlenmesi gereken şekilde Yinele hızlı bir işlemdir.
  - 2. Aşama

     İşlem günlüğü başlatır (yerine, en eski işlenmemiş işlem) en son kontrol noktasından gelen Yinele
- **Aşama Geri Al**

   Sürüm bilgisi olmayan işlemler geri sLog kullanarak geri alma aşamasında ADR ile neredeyse anında tamamlanır ve kalıcı sürüm Store (PV'ler) gerçekleştirmek için mantıksal geri ile satır düzeyi sürümü tabanlı geri.

## <a name="adr-recovery-components"></a>ADR kurtarma bileşenleri

ADR dört anahtar bileşenleri şunlardır:

- **Kalıcı sürüm Store (PV'ler)**

  Kalıcı sürüm deposu için geleneksel yerine veritabanının kendisi oluşturulan satır sürümleri kalıcı bir yeni SQL veritabanı altyapısı mekanizmadır `tempdb` sürüm deposu. PV'ler kaynak yalıtımı sağlar hem de okunabilir ikincil veritabanı kullanılabilirliğini artırır.

- **Mantıksal geri döndür**

  Mantıksal geri gerçekleştirme satır düzeyi sürümü tutulan tüm işlemler için anında işlemi geri alma ve geri alma sağlayan bir geri alma - tabanlı zaman uyumsuz işlem sorumludur.

  - Tüm iptal edilen işlem izler
  - Tüm kullanıcı işlemleri için PV'ler kullanarak geri alma gerçekleştirir
  - İşlem hemen sonra tüm kilitleri yayınlar Durdur

- **sLog**

  sLog depoları (örneğin, meta veri önbellek geçersiz kılma, kilit alımları ve benzeri) sürüm bilgisi olmayan işlemler için kayıtları oturum ikincil bellek içi günlük bir akıştır. SLog aşağıdaki gibidir:

  - Düşük hacim ve bellek içi
  - Denetim noktası işlemi sırasında serileştirilmekte olan tarafından diskte kalıcı olur
  - Düzenli aralıklarla işlem işleme kesildi
  - Hızlandırır Yinele ve yalnızca sürüm bilgisi olmayan işlemler işleyerek almayı geri al  
  - Yalnızca gerekli günlük kayıtlarını koruma tarafından agresif işlem günlüğünün kesilmesi sağlar.

- **Temizleme**

  Temizleyici düzenli aralıklarla uyanır ve gerekli olmayan sayfa sürümleri temizler, zaman uyumsuz bir işlemdir.

## <a name="who-should-consider-accelerated-database-recovery"></a>Kimin hızlandırılmış veritabanı kurtarma göz önünde bulundurmalıyım

Müşteriler aşağıdaki türde ADR etkinleştirme dikkate almanız gerekir:

- Çalışan işlemleri içeren iş yüklerine sahip müşteriler uzun.
- Müşteriler etkin işlemler önemli oranda artma işlem günlüğünün yeri neden olan durumlarda gördünüz.  
- Uzun süreler nedeniyle (örneğin, beklenmeyen SQL Server yeniden başlatma veya el ile işlem geri alma) kurtarma uzun süre çalışan SQL Server veritabanı kesintiler yaşamış müşteriler.

## <a name="to-enable-adr-during-this-preview-period"></a>Bu önizleme dönemi boyunca ADR etkinleştirmek için

Bu özellik Önizleme dönemi boyunca, bir e-posta Gönder [ adr@microsoft.com ](mailto:adr@microsoft.com) daha fazla bilgi edinin ve hızlandırılmış veritabanı kurtarma (ADR) kullanıma deneyin. E-postada, mantıksal sunucunuzun adını (için tek veritabanları, elastik havuzlar ve Azure veri ambarı) içerir. Bu bir önizleme özelliği olduğundan, test sunucunuzun bir üretim dışı sunucusu olmalıdır.
