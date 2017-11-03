---
title: "Azure SQL veri ambarı sık sorulan sorular | Microsoft Docs"
description: "Bu makalede Azure SQL veri ambarı hakkında sık sorulan sorular müşteriler ve geliştiricilerin çıkışı listeler"
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: 
ms.assetid: 812CA525-3BF3-49DF-8DF3-FB4342464F4F
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: overview
ms.date: 3/1/2017
ms.author: elbutter;barbkess
ms.openlocfilehash: 4c00710ecc0c91f8407eca81b78176075fcbd6ad
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="sql-data-warehouse-frequently-asked-questions"></a>SQL veri ambarı sık sorulan sorular

## <a name="general"></a>Genel

Q. Ne SQL DW veri güvenliği için sunar?

A. SQL DW TDE gibi verileri korumak ve denetlemek için çeşitli çözümler sunar. Daha fazla bilgi için bkz: [güvenlik].

Q. Hangi yasal veya iş standartları SQL DW olduğunu uyumlu nereden bulabilirim?

A. Ziyaret [Microsoft Compliance] ürün SOC ve ISO gibi çeşitli uyumluluk sunumları için sayfa. İlk uyumluluk başlığı seçin, ardından Azure genişletin Microsoft kapsam içinde bulut hizmetlerini bölümünde Azure hangi hizmetlerin olduğunu görmek için sayfanın sağ tarafında Hizmetleri uyumludur.

Q. Powerbı bağlayabilir miyim?

A. Evet! SQL DW ile doğrudan sorgu Powerbı destekler ancak çok sayıda kullanıcı veya gerçek zamanlı verileri için tasarlanmamıştır. Powerbı üretim kullanımı için Azure Analysis Services ya da analiz hizmeti Iaas üstünde Powerbı kullanmanızı öneririz. 

Q. SQL veri ambarı kapasite sınırları nelerdir?

A. Bizim geçerli bkz [kapasite limitlerini] sayfası. 

Q. Neden my ölçek/Duraklat/Sürdür çok uzun sürüyor?

A. İşlem yönetimi işlemleri için süre çeşitli etkenlere etkileyebilir. Bir ortak işlemleri uzun süre çalışan işlem geri alma için durumda. Bir ölçek veya duraklatma işlemi başlatıldığında, tüm gelen oturumları engellenir ve sorguları boşaltmış. Bir işlem yeniden başlatılmadan önce sistem kararlı durumda bırakın için işlemleri geri alınması gerekir. Büyük sayı ve daha büyük günlük boyutu hareketlerinin, uzun işlemi sistem kararlı bir duruma geri yükleme durduruldu.

## <a name="user-support"></a>Kullanıcı desteği

Q. Özellik isteği burada gönderme sahip miyim?

A. Özellik isteği varsa, üzerinde gönderme bizim [UserVoice] sayfası

Q. Nasıl yapabilirim x?

A. SQL veri ambarı ile geliştirilmesi konusunda daha fazla yardım için üzerinde sorular sorabilirsiniz bizim [yığın taşması] sayfası. 

Q. Bir destek bileti nasıl gönderilsin mi?

A. [Destek biletlerini Göster] Azure portalı üzerinden kaydedildi.

## <a name="sql-languagefeature-support"></a>SQL dil/özellik desteği 

Q. Hangi veri türleri SQL Data Warehouse destekliyor mu?

A. SQL veri ambarı bkz [veri türleri].

Q. Tablo özellikleri destekliyor?

A. SQL veri ambarı birçok özellik desteklerken, bazı desteklenmez ve belgelenmiştir [desteklenmeyen Tablo Özellikler].

## <a name="tooling-and-administration"></a>Araçları ve yönetim

Q. Visual Studio veritabanı projelerini desteklemez.

A. Şu anda veritabanı projeleri Visual Studio ile SQL Data Warehouse için desteklemiyoruz. Bu özellik almak için bir oy dönüştürmek istiyorsanız, bizim kullanıcı sesi ziyaret [veritabanı projeleri özellik isteği].

Q. SQL Data Warehouse, REST API'leri destekliyor mu?

A. Evet. SQL veritabanı ile kullanılan çoğu REST işlevselliği de SQL Data Warehouse ile kullanılabilir. REST belge sayfalarının içinde API bilgi bulabilirsiniz veya [MSDN].


## <a name="loading"></a>Yükleniyor

Q. Hangi istemci sürücüleri destekliyor?

A. DW sürücü desteği bulunabilir [bağlantı dizeleri] sayfası

S: hangi dosya biçimleri, SQL veri ambarı ile PolyBase tarafından desteklenir?

Y: Orc, RC, Parquet ve düz sınırlandırılmış metin

S: ne ı PolyBase kullanarak SQL DW bağlanabilir mi? 

Y: [Azure Data Lake Store] ve [Azure depolama BLOB'ları]

S: hesaplama aşağı itme Azure Storage Bloblarında veya ADLS bağlanırken mi? 

A: Hayır SQL DW PolyBase yalnızca depolama bileşenleri etkileşim kurar. 

S: HDI için bağlanabilir?

Y: HDI ADLS veya WASB HDFS katmanı olarak kullanabilirsiniz. HDFS katmanı olarak ya da varsa, bu verileri SQL DW yükleyebilirsiniz. Ancak, aşağı itme hesaplama HDI örneği oluşturulamıyor. 

## <a name="next-steps"></a>Sonraki adımlar
Bir bütün olarak SQL Data Warehouse hakkında daha fazla bilgi için bkz: bizim [genel bakış] sayfası.


<!-- Article references -->
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[bağlantı dizeleri]: ./sql-data-warehouse-connection-strings.md
[yığın taşması]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Destek biletlerini Göster]: ./sql-data-warehouse-get-started-create-support-ticket.md
[güvenlik]: ./sql-data-warehouse-overview-manage-security.md
[Microsoft Compliance]: https://www.microsoft.com/en-us/trustcenter/compliance/complianceofferings
[kapasite limitlerini]: ./sql-data-warehouse-service-capacity-limits.md
[veri türleri]: ./sql-data-warehouse-tables-data-types.md
[desteklenmeyen Tablo Özellikler]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[Azure Data Lake Store]: ./sql-data-warehouse-load-from-azure-data-lake-store.md
[Azure depolama BLOB'ları]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[veritabanı projeleri özellik isteği]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu
[MSDN]: https://msdn.microsoft.com/en-us/library/azure/mt163685.aspx
[genel bakış]: ./sql-data-warehouse-overview-faq.md