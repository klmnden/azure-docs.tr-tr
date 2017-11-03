---
title: "Kullanım Örneği - Müşteri Profili Oluşturma"
description: "Azure Data Factory oyun müşteriler profil için bir veri temelli iş akışı (ardışık düzeni) oluşturmak için nasıl kullanıldığı hakkında bilgi edinin."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: e07d55cf-8051-4203-9966-bdfa1035104b
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: shlo
robots: noindex
ms.openlocfilehash: fcba76555902fb393830f352e6274b3ecd2fba38
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
---
# <a name="use-case---customer-profiling"></a>Kullanım Örneği - Müşteri Profili Oluşturma
Azure Data Factory Çözüm Hızlandırıcıları Cortana Intelligence Suite uygulamak için kullanılan birçok hizmetlerden biridir.  Cortana Intelligence hakkında daha fazla bilgi için ziyaret [Cortana Intelligence Suite](http://www.microsoft.com/cortanaanalytics). Bu belgede, Azure Data Factory ortak analytics sorunları nasıl çözebilir anlama ile çalışmaya başlamanıza yardımcı olmak için basit kullanım örneği açıklanmaktadır.

## <a name="scenario"></a>Senaryo
Contoso olan birden çok platform için oyunlar oluşturan bir oyun şirket: oyun konsolları, taşınabilir bilgisayar cihazları ve kişisel bilgisayarlar (bilgisayarlar). Bu oyun oynatıcıları gibi günlük verilerini büyük miktarda kullanım desenlerini, oyun stil ve kullanıcı tercihleri izleyen oluşturulur.  Demografik, bölge ile birleştirildiğinde ve ürün veri Contoso oyuncuların deneyimini geliştirmek hakkında rehberlik için analiz gerçekleştirebilir ve hedef yükseltmeler için bunları ve oyun satın alır. 

Contoso'nun hedef kendi oynatıcıları oyun geçmiş temelinde Yukarı satış/çapraz satış fırsatları tanımlamak için ve sürücü iş büyümesi ilgi çekici özellikleri ekleyin ve müşteriler için daha iyi bir deneyim sağlar. Bu kullanım durumu için bir iş örnek olarak bir oyun şirket kullanırız. Şirket oyuncuların davranışı temelinde kendi oyunlar en iyi duruma getirmek istiyor. Bu ilkeler, müşterilerinin kendi mal ve hizmet geçici devreye ve müşterilerin deneyimini geliştirmek isteyen herhangi bir işletme için geçerlidir.

Bu çözümde, Contoso son başlattı bir pazarlama kampanyası verimliliğini değerlendirmek ister. Biz ham oyun günlükleri ile başlayın, işlem ve coğrafi konum verilerle zenginleştirmek, başvuru verileri reklam ile katılması ve son olarak bunları kampanya etkisi çözümlemek için Azure SQL Database kopyalayın.

## <a name="deploy-solution"></a>Çözümü dağıtma
Tüm erişmek ve bu basit kullanım örneği denemek için gereken bir [Azure aboneliği](https://azure.microsoft.com/pricing/free-trial/), bir [Azure Blob Depolama hesabı](../../storage/common/storage-create-storage-account.md#create-a-storage-account)ve bir [Azure SQL veritabanı](../../sql-database/sql-database-get-started.md). Ardışık düzen tarafından profil oluşturma Müşteri dağıttığınız **örnek ardışık düzen** döşeme veri fabrikanızın giriş sayfasında.

1. Veri Fabrikası oluşturun veya varolan bir veri fabrikası açın. Bkz: [veri kopyalama Blob depolama alanından SQL Data Factory kullanarak veritabanına](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) veri fabrikası oluşturma adımları için.
2. İçinde **DATA FACTORY** data factory dikey penceresine tıklayın **örnek ardışık düzen** döşeme.

    ![Örnek komut zincirleri döşeme](./media/data-factory-samples/SamplePipelinesTile.png)
3. İçinde **örnek ardışık düzen** dikey penceresinde tıklatın **profil müşteri** dağıtmak istediğiniz.

    ![Örnek komut zincirleri dikey penceresi](./media/data-factory-samples/SampleTile.png)
4. Örnek yapılandırma ayarlarını belirtin. Örneğin, Azure depolama hesabı adı ve anahtar, Azure SQL sunucu adı, veritabanı, kullanıcı kimliği ve parola.

    ![Örnek dikey penceresi](./media/data-factory-samples/SampleBlade.png)
5. Yapılandırma ayarları belirterek tamamladıktan sonra tıklatın **oluşturma** oluşturun / örnek ardışık düzen ve ardışık düzen tarafından kullanılan bağlı hizmetler/tablolar dağıtmak için.
6. Daha önce üzerinde tıkladığınız örnek kutucuğu dağıtım durumunu görmek **örnek ardışık düzen** dikey.

    ![Dağıtım durumu](./media/data-factory-samples/DeploymentStatus.png)
7. Gördüğünüzde **dağıtım başarılı** örnek, kapatmak için döşeme iletide **örnek ardışık düzen** dikey.  
8. Üzerinde **DATA FACTORY** dikey penceresinde, gördüğünüz bağlı hizmetler, veri kümelerini ve ardışık düzen veri fabrikanıza eklenir.  

    ![Data Factory dikey penceresi](./media/data-factory-samples/DataFactoryBladeAfter.png)

## <a name="solution-overview"></a>Çözüme genel bakış
Bu basit kullanım örneği, alma, hazırlamak, dönüştürmek, analiz ve veri yayımlamak için Azure Data Factory nasıl kullanabileceğinize bir örnek olarak kullanılabilir.

![Uçtan uca iş akışı](./media/data-factory-customer-profiling-usecase/EndToEndWorkflow.png)

Bu şekil dağıtıldıktan sonra veri ardışık Azure portalında nasıl göründüğünü gösterir.

1. **PartitionGameLogsPipeline** ham oyun olayları blob depolama alanından okur ve yıl, ay ve gün göre bölümler oluşturur.
2. **EnrichGameLogsPipeline** bölümlenmiş oyun olaylar coğrafi kod başvuru verileri ile birleştirir ve karşılık gelen coğrafi konumlar için IP adreslerini eşleyerek veri aşağıdakilere zenginleştirir.
3. **AnalyzeMarketingCampaignPipeline** ardışık düzen zenginleştirilmiş verileri kullanır ve pazarlama kampanyası etkinliğini içeren son çıktı oluşturmak için reklam verileriyle işler.

Bu örnekte, Data Factory giriş verilerini, dönüştürme ve işlem verileri kopyalayın ve bir Azure SQL veritabanı son veri çıkış etkinlikleri düzenlemek için kullanılır.  Ağ veri ardışık düzenleri, görselleştirmenize, bunları yönetmek ve kendi kullanıcı arabirimini durumundan izleme.

## <a name="benefits"></a>Avantajlar
Kendi kullanıcı profili analiz en iyi duruma getirme ve iş hedeflerini ile hizalama, oyun şirket hızla kullanım desenlerini toplamak ve pazarlama kampanyalarının etkisini analiz etmek mümkün olacaktır.

