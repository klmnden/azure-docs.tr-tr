<properties
   pageTitle="SQL Data Warehouse önizleme beklentileri | Microsoft Azure"
   description="Genel önizleme özellikleri özeti ve SQL Data Warehouse'un genel kullanılabilirliğine yönelik hedeflerimiz."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="happynicolle"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/05/2016"
   ms.author="nicw;barbkess;sonyama"/>


# SQL Data Warehouse önizleme beklentileri

Bu makalede SQL Data Warehouse önizleme işlevleri ve hizmetin genel kullanılabilirliğine (GA) yönelik hedeflerimiz açıklanmıştır. Genel önizleme özelliklerini geliştirdikçe bu bilgileri güncelleştireceğiz.

SQL Data Warehouse'a yönelik hedeflerimiz:

- Tahmin edilebilir performans ve petabayt boyutlarına ulaşan veriler için doğrusal ölçeklenebilirlik
- Tüm veri ambarı işlemleri için yüksek güvenilirlik
- İlişkisel olan ve ilişkisel olmayan verilerin yüklenmesinden sonra kısa sürede veri öngörülerinin elde edilmesi

SQL Data Warehouse önizlemesi boyunca bu hedefler için sürekli olarak çalışacağız.

## Tahmin edilebilir ve ölçeklenebilir performans

Azure SQL Data Warehouse, veri ambarı için kullanılabilir durumda olan bilgi işlem kaynaklarının (CPU'lar, bellek, depolama alanı G/Ç'si) ölçülmesine ilişkin bir yol olarak Veri Ambarı Birimlerini (DWU'lar) kullanıma sunmuştur. DWU sayısı artırıldığında kaynak sayısı da artar. DWU sayısı arttıkça SQL Data Warehouse, daha fazla dağıtılmış kaynak üzerinde paralel işlemler (ör. veri yükleme veya sorgu) gerçekleştirir. Bu, gecikmeyi azaltır ve performansı artırır.

Tüm veri ambarlarında şu 2 temel performans ölçümü mevcuttur:

- Yükleme oranı. Saniye başına veri ambarına yüklenebilen kayıt sayısı. PolyBase aracılığıyla Azure Blob Storage'dan Kümelenmiş Columnstore Dizinine sahip bir tabloya aktarılabilecek kayıt sayısını özel olarak ölçüyoruz.
- Tarama oranı. Saniye başına veri ambarından sırayla alınabilecek kayıt sayısı. Bir kümelenmiş columnstore dizinindeki bir sorgu tarafından döndürülen kayıt sayısını özellikle ölçeriz.

Bazı önemli performans iyileştirme ölçümleri gerçekleştiriyoruz ve beklenen oranları sizlerle kısa bir süre içinde paylaşacağız. Önizleme süresince bu oranları artırmak ve oranların tahmin edilebilir şekilde ölçeklenmesini sağlamak için sürekli olarak iyileştirmeler (ör. sıkıştırmayı ve önbelleğe almayı artırma) yapacağız.  

## Veri Koruma

SQL Data Warehouse, yerel olarak yedekli depolama özelliğini kullanarak tüm verileri Azure depolama alanında depolar. Yerelleştirilmiş hata olasılıklarına karşın saydam veri koruma olanağı sağlamak amacıyla yerel veri merkezinde verilerin birden çok zaman uyumlu kopyası saklanır. 

## Yedeklemeler

Azure SQL Data Warehouse, Azure Storage Anlık Görüntülerini kullanarak her 8 saatte bir tüm verileri yedekler. Bu anlık görüntüler 7 gün boyunca saklanır. Bu sayede, verilerin 7 gün öncesinden başlanarak son anlık görüntünün alındığı ana kadar en az 21 noktaya geri yüklenmesine olanak sağlanır. PowerShell veya REST API'leri aracılığıyla bir anlık görüntüden veri geri yüklemesi yapabilirsiniz.

## Sorgu güvenilirliği

SQL Data Warehouse, yüksek düzeyde paralel işleme (MPP) mimarisi üzerine kuruludur. SQL Data Warehouse, İşlem ve Denetim düğümü hatalarını otomatik olarak algılar ve geçirir. Yine de bir işlem (ör. veri yüklemesi veya sorgu), bir düğüm hatası veya geçiş sonucunda başarısız olabilir. Önizleme süresince, düğüm hatalarına rağmen işlemleri başarılı bir şekilde tamamlamak amacıyla sürekli olarak iyileştirmeler yapıyoruz.


## Yükseltmeler ve kesinti

SQL Data Warehouse, yeni özelliklerin eklenmesi ve önemli düzeltmelerin yüklenmesi amacıyla düzenli aralıklarla yükseltilecektir.  Bu yükseltme işlemleri karışıklığa neden olabilir ve şu anda yükseltme işlemleri tahmin edilebilir bir zamanlamaya göre yapılmamaktadır.  Bu işlemin büyük ölçüde karışıklığa yol açtığını düşünüyorsanız söz konusu işlemle ilgili sorununuzun çözülmesine yardımcı olabilmemiz için [destek bileti oluşturmanızı][] öneriyoruz.


## Sonraki adımlar

Genel önizlemeye [başlarken][]

<!--Image references-->

<!--Article references-->
[destek bileti oluşturmanızı]: ./sql-data-warehouse-get-started-create-support-ticket.md
[başlarken]: ./sql-data-warehouse-get-started-provision.md

<!--MSDN references-->

<!--Other Web references-->



<!----HONumber=Jun16_HO2-->


