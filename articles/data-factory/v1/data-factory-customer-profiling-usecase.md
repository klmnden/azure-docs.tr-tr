---
title: Kullanım Örneği - Müşteri Profili Oluşturma
description: Azure Data Factory oyun müşteriler profil için bir veri odaklı iş akışı (işlem hattı) oluşturmak için nasıl kullanıldığını öğrenin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: e07d55cf-8051-4203-9966-bdfa1035104b
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: bb7d6531da330bcfbf6de786ffb19984cfd1964e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60487221"
---
# <a name="use-case---customer-profiling"></a>Kullanım Örneği - Müşteri Profili Oluşturma
Azure Data Factory Çözüm Hızlandırıcıları, Cortana Intelligence Suite uygulamak için kullanılan birçok hizmetlerden biridir.  Cortana Intelligence hakkında daha fazla bilgi için ziyaret [Cortana Intelligence Suite](https://www.microsoft.com/cortanaanalytics). Bu belgede, Azure Data Factory ortak analytics sorunlarını nasıl çözebilirsiniz anlama ile çalışmaya başlamanıza yardımcı olmak için basit bir kullanım örneği açıklanmaktadır.

## <a name="scenario"></a>Senaryo
Contoso olan bir oyun şirketi, oyunlar için eksiksiz bir çözüm oluşturur: oyun konsolları, elde tutulan cihazları ve kişisel bilgisayarlara (bilgisayarlar). Oyuncuların bu oyunları oynarken, yüksek hacimli verilerin günlük kullanım düzenlerini, oyun stil ve kullanıcı tercihleriyle izleyen oluşturulur.  Demografik, bölge ile birlikte kullanıldığında ve ürün verileri Contoso oyuncuların deneyimini geliştirmek hakkında kılavuza analiz gerçekleştirebilir ve hedef için yükseltmeleri ve oyun içi satın alma. 

Contoso'nun hedef işletmelerini büyütmesine ilgi çekici özellikler eklemek, oyuncuların oyun geçmişi temel alarak yukarı satış/çapraz satış fırsatlarını belirlemek için ve müşteriler için daha iyi bir deneyim sağlar. Bu kullanım örneği için bir iş, örnek olarak bir oyun şirketi kullanırız. Şirket, oyunların oyuncuların davranışına göre iyileştirmek istiyor. Bu ilkeler, mal ve hizmet geçici olarak, müşterilerle etkileşim kurun ve müşterilerinin deneyimini geliştirmek isteyen tüm işletmeler için geçerlidir.

Bu çözümde, yakın zamanda sundu bir pazarlama kampanyasının verimliliğini değerlendirmek Contoso istiyor. Biz ham oyun günlüklerle başlayın, işlem ve bunları coğrafi konum verilerle zenginleştirerek, başvuru verileri tanıtılması ile katılın ve son olarak bunları kampanyanın etkisini analiz etmek için Azure SQL veritabanı kopyalayın.

## <a name="deploy-solution"></a>Çözümü dağıtma
Erişmek ve bu basit bir kullanım örneği denemek için ihtiyacınız olan bir [Azure aboneliği](https://azure.microsoft.com/pricing/free-trial/)e [Azure Blob Depolama hesabı](../../storage/common/storage-quickstart-create-account.md)ve bir [Azure SQL veritabanı](../../sql-database/sql-database-get-started.md). Müşteri profili oluşturma işlem hattından dağıttığınız **örnek işlem hatları** kutucuğuna veri fabrikanızın giriş sayfasında.

1. Veri Fabrikası oluşturma veya mevcut bir data factory açın. Bkz: [Blob depolamadan SQL veritabanına veri kopyalama kullanarak veri fabrikası için](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) veri fabrikası oluşturma adımları için.
2. İçinde **DATA FACTORY** data factory dikey penceresine tıklayın **örnek işlem hatları** Döşe.

    ![Örnek işlem hatları kutucuğu](./media/data-factory-samples/SamplePipelinesTile.png)
3. İçinde **örnek işlem hatları** dikey penceresinde tıklayın **müşteri profili oluşturma** dağıtmak istediğiniz.

    ![Örnek işlem hatları dikey penceresi](./media/data-factory-samples/SampleTile.png)
4. Örnek için yapılandırma ayarlarını belirtin. Örneğin, Azure depolama hesabı adı ve anahtarı, Azure SQL sunucu adı, veritabanı, kullanıcı kimliği ve parola.

    ![Örnek dikey penceresi](./media/data-factory-samples/SampleBlade.png)
5. Yapılandırma ayarlarını belirten ile bitirdikten sonra tıklayın **Oluştur** dağıtmak oluşturma / örnek işlem hatları ve ardışık düzen tarafından kullanılan bağlı hizmetler/tablolar için.
6. Dağıtım durumu örnek kutucuğa tıklandığında daha önce gördüğünüz **örnek işlem hatları** dikey penceresi.

    ![Dağıtım durumu](./media/data-factory-samples/DeploymentStatus.png)
7. Gördüğünüzde **dağıtım başarılı** kutucuğuna örnek, kapatmak için ileti **örnek işlem hatları** dikey penceresi.  
8. Üzerinde **DATA FACTORY** dikey penceresinde görürsünüz bağlı hizmetler, veri kümeleri ve işlem hatlarını veri fabrikanıza eklenir.  

    ![Data Factory dikey penceresi](./media/data-factory-samples/DataFactoryBladeAfter.png)

## <a name="solution-overview"></a>Çözüme Genel Bakış
Bu basit bir kullanım örneği içe alma, hazırlama, dönüştürme, analiz ve veri yayımlama için Azure Data Factory nasıl kullanabileceğinize bir örnek olarak kullanılabilir.

![Uçtan uca iş akışı](./media/data-factory-customer-profiling-usecase/EndToEndWorkflow.png)

Bu şekilde dağıtıldıktan sonra Azure portalında veri işlem hatlarını nasıl göründüğünü gösterilmektedir.

1. **PartitionGameLogsPipeline** ham oyun olayları blob depolama alanından okur ve yıl, ay ve gün bölüm oluşturur.
2. **EnrichGameLogsPipeline** coğrafi kod başvuru verileri ile bölümlenmiş oyun olayları birleştirir ve karşılık gelen coğrafi konumları için IP adreslerini eşleyerek veri zenginleştirir.
3. **AnalyzeMarketingCampaignPipeline** işlem hattı zenginleştirilmiş verileri kullanır ve reklam verileriyle pazarlama kampanyası verimliliğini içeren son çıktı oluşturmak için işler.

Bu örnekte, Data Factory giriş verileri, dönüştürme ve işlem verileri kopyalayın ve bir Azure SQL veritabanı için son veri çıkış etkinlikleri düzenlemek için kullanılır.  Ayrıca, veri işlem hatlarından oluşan ağ görselleştirin, yönetmek ve kullanıcı arabiriminden bunların durumunu izleyebilirsiniz.

## <a name="benefits"></a>Avantajlar
Kendi kullanıcı profili analiz en iyi duruma getirme ve iş hedeflerine hizalama oyun şirketi hızla kullanım düzenlerini toplamak ve kendi pazarlama kampanyalarının etkisini analiz etmek kullanabilirsiniz.

