---
title: Tehdit algılama için Azure Güvenlik Merkezi'nde Veri Hizmetleri | Microsoft Docs
description: Bu konuda, Veri Hizmetleri uyarıları kullanılabilir Azure Güvenlik Merkezi'nde sunulmaktadır.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: da960861-0b6c-4d80-932d-898cdebb4f83
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: monhaber
ms.openlocfilehash: 87cfd2769e473d26c2dcae1b7b418f6fb1739915
ms.sourcegitcommit: c0419208061b2b5579f6e16f78d9d45513bb7bbc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67626282"
---
# <a name="threat-detection-for-data-services-in-azure-security-center"></a>Tehdit algılama için Azure Güvenlik Merkezi'nde Veri Hizmetleri

 Güvenlik Merkezi, veri depolama hizmetlerinden günlükleri analiz eder ve veri kaynakları için bir tehdit algıladığında uyarı tetikler. Bu konu, aşağıdaki hizmetleri için Güvenlik Merkezi oluşturduğu uyarıların listeler:

* [Azure SQL veritabanı ve SQL veri ambarı](#data-sql)
* [Azure Depolama](#azure-storage)

## Azure SQL veritabanı ve SQL veri ambarı <a name="data-sql"></a>

SQL tehdit algılama, olağan dışı ve zararlı gösteren anormal etkinlikleri algılar, erişim veya veritabanı açıklarından yararlanmaya dener. Güvenlik Merkezi, SQL denetim günlüklerini analiz eder ve SQL altyapısı içinde yerel olarak çalışır.

|Uyarı|Açıklama|
|---|---|
|**SQL ekleme güvenlik açığı**|Bir uygulama hatalı SQL deyimi veritabanında üretti. Bu, SQL ekleme saldırılarına karşı olası bir güvenlik açığını gösterebilir. Hatalı deyim oluşturulmasının iki olası nedeni vardır: Ya da, hatalı SQL deyimi uygulama kodunda bir hata oluşturulur. Alternatif olarak, uygulama kodu veya depolanan yordamlar kullanıcı girişi için SQL ekleme yararlanılabilir hatalı SQL deyimi yapılandırılırken sırasında bu açıktan kaydetmedi.|
|**Olası SQL eklemesi**|SQL ekleme güvenlik açığı tanımlanan uygulama karşı etkin bir açıktan yararlanma oluştu. Bu, bir saldırganın güvenlik açığına sahip uygulama kodu kullanarak kötü amaçlı SQL deyimleri eklemeye çalıştığı veya saklı yordamları anlamına gelir.|
|**Olağan dışı bir konumdan erişim**|SQL Server, burada kişi SQL sunucusuna olağan dışı bir coğrafi konumdan oturum açmış olduğu erişim düzeninde bir değişiklik meydana geldi. Bazı durumlarda uyarı güvenli işlemleri (yeni bir uygulama veya geliştirici bakımı gibi) de algılar. Diğer durumlarda, uyarı kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.|
|**Erişim**|SQL sunucusunun erişim deseninde değişiklik olmuştur - birisi bir sorumludan (SQL kullanıcısı) kullanarak SQL sunucusuna oturum. Bazı durumlarda uyarı güvenli işlemleri (yeni uygulama ve geliştirici bakımı gibi) de algılar. Diğer durumlarda, uyarı kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.|
|**Zararlı olabilecek bir uygulamadan erişim**|Zararlı bir uygulama, veritabanına erişmek için kullanılır. Bazı durumlarda, uyarı güvenlik testlerini algılar. Diğer durumlarda, yarı yaygın saldırı araçlarının kullandığı saldırıları algılar.|
|**Deneme yanılma SQL kimlik bilgileri**|Farklı kimlik bilgileri başarısız oturum açma denemesi olağandışı yüksek sayıda oluştu. Bazı durumlarda, uyarı güvenlik testlerini algılar. Diğer durumlarda, uyarı deneme yanılma saldırılarını algılar.|

SQL tehdit algılama uyarıları Bkz hakkında daha fazla bilgi için[Azure SQL veritabanı tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview)ve tehdit algılama uyarıları bölümü gözden geçirin. Ayrıca bkz: [nasıl Azure Güvenlik Merkezi yardımcı olan bir Cyberattack açığa](https://azure.microsoft.com/blog/how-azure-security-center-helps-reveal-a-cyberattack/) kötü amaçlı SQL etkinliği algılama Güvenlik Merkezi bir saldırı bulmak için nasıl kullanılacağını gösteren bir örnek görüntülemek için.

## Azure depolama<a name="azure-storage"></a>

>[!NOTE]
> Azure depolama için Gelişmiş tehdit koruması yalnızca Blob Depolama için şu anda kullanılabilir. 

Azure Depolama için Gelişmiş Tehdit Koruması, depolama hesaplarına erişmeye veya güvenlik açıklarından yararlanmaya yönelik sıra dışı, zararlı olabilecek girişimleri algılayan güvenlik zekasına sahip ek bir güvenlik katmanı sağlar. Bu koruma katmanı tehditlerle için uzman güvenlik ve güvenlik izleme sistemlerine yönetmenize gerek kalmadan sağlar.

Güvenlik Merkezi, okuma, yazma ve silme isteği sayısı tehditleri algılamak için Blob Depolama için tanılama günlüklerini analiz ederek ve anomalileri etkinlik gerçekleştiğinde uyarı tetikler. Daha fazla bilgi için bkz: [depolama analizi günlük tutmayı yapılandırma](https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account#configure-logging) daha fazla bilgi için.

> [!div class="mx-tableFixed"]

|Uyarı|Açıklama|
|---|---|
|**Sıra dışı bir konumdan erişim anomali**|Örneklenen ağ trafiği analizinde dağıtımınızdaki kaynak kaynaklanan anormal giden Uzak Masaüstü Protokolü (RDP) iletişimi algılandı. Bu etkinlik, bu ortam için anormal olarak kabul edilir ve kaynağınızın tehlikeye girdiğini ve deneme yanılma dış RDP uç noktası için artık kullanılan gösterebilir. Bu etkinlik türünün, IP’nizin dış varlıklar tarafından kötü amaçlı olarak işaretlenmesine neden olabileceğini unutmayın.|
|**Uygulama erişim anomali**|Olağan dışı bir uygulama bu depolama hesabı eriştiğini belirtir. Olası bir nedeni, bir saldırganın yeni bir uygulama kullanarak depolama hesabınıza eriştiğini ' dir.|
|**Anonim erişim anomali**|Bir depolama hesabına erişim düzeninde bir değişiklik olduğunu gösterir. Örneğin, hesap anonim olarak (herhangi bir kimlik doğrulaması), bu hesapta en son erişim düzeni ile karşılaştırıldığında beklenmeyen olduğu erişilmedi. Olası bir nedeni, bir saldırganın bir kapsayıcıya ayrı tutma depolama blobda genel okuma erişimini yararlandıktan ' dir.|
|**Veri Sızdırma anomali**|Olağan dışı derecede büyük bir veri miktarını bu depolama kapsayıcısındaki son etkinliklere göre çıkartılan gösterir. Olası bir nedeni, bir saldırganın büyük miktarda veri içeren depolama blobda kapsayıcıdan ayıklanan ' dir.|
|**Anomali beklenmeyen Sil**|Bir veya daha fazla beklenmeyen silme işlemleri Bu hesapta en son etkinlik ile karşılaştırıldığında bir depolama hesabı oluştu gösterir. Olası bir nedeni, bir saldırganın verileri depolama hesabınızdan silindi olmasıdır.|
|**Azure bulut hizmeti paketi yükleme**|Bir Azure bulut hizmeti paketi (.cspkg dosyası) bir depolama hesabına Bu hesapta en son etkinlik ile karşılaştırıldığında, olağan dışı bir şekilde yüklendiğini gösterir. Olası bir nedeni, bir saldırgan kötü amaçlı kodu depolama hesabınızdan bir Azure bulut hizmeti dağıtmak hazırlama ' dir.|
|**İzni erişim anomali**|Bu depolama kapsayıcısındaki erişim izni olağan dışı bir şekilde değiştiğini gösterir. Olası nedeni, bir saldırganın kapsayıcı izinlerini kendi güvenlik duruşunu zayıflatabilir veya Kalıcılık sağlamak için değiştiğidir.|
|**İnceleme erişim anomali**|Bu hesapta en son etkinlik ile karşılaştırıldığında, olağan dışı bir şekilde bir depolama hesabına erişim izinlerini geçersiz inceledikten olmadığını gösterir. Olası bir nedeni bir saldırganın keşfi için gelecekteki bir saldırı gerçekleştirmesidir.|
|**Veri araştırma anomali**|BLOB'ları veya bir depolama hesabındaki kapsayıcıları Bu hesapta en son etkinlik kıyasla anormal bir şekilde numaralandırılmış olduğunu gösterir. Olası bir nedeni bir saldırganın keşfi için gelecekteki bir saldırı gerçekleştirmesidir.|

>[!NOTE]
>Gelişmiş tehdit koruması için Azure depolama şu anda Azure devlet kurumları ve bağımsız bulut bölgelerinde kullanılabilir değil.

Depolama için uyarılar hakkında daha fazla bilgi için bkz. [Azure depolama için Gelişmiş tehdit koruması](https://docs.microsoft.com/azure/storage/common/storage-advanced-threat-protection) makalesini inceleyin ve koruma uyarıları bölümü gözden geçirin.
