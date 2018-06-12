---
title: Azure maliyeti yönetim panolarında önemli metrikleri görüntüleyin | Microsoft Docs
description: Bu makalede nasıl Azure maliyeti Yönetimi'nde panolarla önemli metrikleri görüntüleyin.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 06/05/2018
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: b1dc2e2eca900ca0ae72329c3c373b2d24f1b2e0
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35304097"
---
# <a name="view-key-cost-metrics-with-dashboards"></a>Panolarla ölçümleri maliyet anahtarını görüntüle

Cloudyn panolarında raporlar üst düzey bir görünümünü sağlar. Panolar, anahtar maliyet ölçümleri tek bir görünümde olanak tanır. Ayrıca, önemli iş kararları almanıza yardımcı olmak için iş eğilimi vurgular sağlarlar.

Panolar ayrıca farklı sorumlulukları içerebilir, kuruluşunuzdaki kişiler için görünümleri oluşturmak için kullanılır:

- Finansal denetleyicisi
- Uygulama veya proje sahibi
- DevOps mühendislik
- Yöneticiler

Pano pencere öğeleri yapılır ve her bir pencere aslında bir rapor küçük resim olur. Kendi raporu açmak için bir pencere öğesini tıklatın. Raporları özelleştirme raporlarım için Kaydet ve panoya eklenir.

Pano sürümleri Yönetimi (MSP), kurumsal ve Premium Cloudyn kullanıcılar için farklı. Farkları varlık erişim düzeyleri tarafından belirlenir. Erişim düzeyleri hakkında daha fazla bilgi için bkz: [varlık erişim düzeyleri](tutorial-user-access.md#entity-access-levels).

Pano kullanılabilirlik panolar görüntülenirken kullanılan bulut hizmeti sağlayıcısı hesap türüne göre değişir. Kullanılabilir ve Cloudyn tarafından toplanan bilgiler türü panolar raporlarda etkiler. Örneğin, bir AWS hesabınız yoksa S3 İzleyicisi panoyu görmezsiniz. Benzer şekilde, Azure Resource Manager erişimi Cloudyn etkinleştirmezseniz iyileştirici Pano pencere öğeleri herhangi bir Azure özgü bilgi göremezsiniz.

Hazır panolar birini kullanabilirsiniz veya özelleştirilmiş raporlarla kendi Pano oluşturabilirsiniz. Cloudyn raporlarla emin değilseniz bkz [maliyet yönetimini kullanma raporları](use-reports.md).

## <a name="create-a-custom-dashboard"></a>Özel bir pano oluşturun

Özel bir pano ile hızlı bir şekilde başlamak için özelliklerini kullanmak için mevcut bir çoğaltabilirsiniz. Ardından, gereksinimlerinize uyacak şekilde değiştirebilirsiniz. Kopyalamak istediğiniz Panoda tıklatın **Kaydet**. Özelleştirilmiş Panolar yalnızca çoğaltabilirsiniz — Cloudyn ile birlikte panolar aynı olamaz.

Özel bir Pano oluşturmak için:

1. Giriş sayfasında tıklatın **yeni Ekle +**. My Pano sayfası görüntülenir.  
    ![Benim Panom](./media/dashboards/my-dashboard.png)
2. Tıklatın **yeni bir rapor eklemek**. Rapor Ekle kutusu görüntülenir.
3. Pano pencere öğesi için eklemek istediğiniz raporu seçin. Pencere öğesi panoya eklenir.
4. Pano tamamlanana kadar önceki adımları yineleyin.
5. Panonun adını değiştirmek için Pano giriş sayfası Panoda adına tıklayın ve yeni bir ad yazın.

## <a name="modify-a-custom-dashboard"></a>Özel bir Pano değiştirme

Özel bir pano oluşturma gibi ile Cloudyn dahil panolar değiştiremezsiniz. Özel Pano raporu değiştirmek için:

1. Panosunda, önce değiştirmek istediğiniz raporu Bul **Düzenle**. Rapor görüntülenir.
2. Raporu istediğiniz değişiklikleri yapın **kaydetmek**. Rapor güncelleştirilir ve değişikliklerinizi görüntüler.

## <a name="share-a-custom-dashboard"></a>Özel bir pano paylaşımı

Özel bir Pano için başkalarıyla paylaşabilirsiniz _ortak_ veya _My varlık_. Ortak olarak paylaştığınızda, tüm kullanıcıların Pano görüntüleyebilirsiniz. My varlığa paylaştığınızda yalnızca geçerli varlık erişimi olan kullanıcılar Pano görüntüleyebilirsiniz. Özel bir pano paylaşımı ortak ve My varlık için adımları benzerdir.

Ortak için özel bir panoyu paylaşmak için:

1. Bir Panoda tıklatın **Pano ayarları**. Pano ayarları kutusu görüntülenir.  
    ![Pano Seçenekleri](./media/dashboards/dashboard-options.png)
2. Pano ayarları kutusunda OK simgesine tıklayın ve ardından **ortak**. Ortak Pano onay iletişim kutusu görüntülenir.
3. Tıklatın **Evet**. Pano başkalarına kullanıma sunulmuştur.

## <a name="delete-a-custom-dashboard-report"></a>Özel Pano raporu silin

Panodan bir özel rapor bileşeni silebilirsiniz. Panodan raporu silmek raporu Raporlar listesinden silmez. Bunun yerine, rapor silme yalnızca panodan kaldırır.

Bir Pano bileşende Pano bileşeni silmek için tıklatın **silmek**. Tıklatarak **silmek** hemen Panosu bileşenleri siler.

## <a name="share-a-dashboard-enterprise"></a>Pano (Kurumsal) paylaşma

Tüm kullanıcılar için özel panolar, kuruluşunuzda veya geçerli varlığın kullanıcılarıyla paylaşabilirsiniz. Bir pano paylaşımı başkalarının KPI hızlı üst düzey bir görünümünü verebilirsiniz. Bir Pano paylaştığınızda, otomatik olarak tüm Cloudyn varlıklar/müşterileriniz için panoyu çoğaltır. Paylaşılan Pano değişiklikler otomatik olarak güncelleştirilir.

Bir Pano subentities dahil olmak üzere tüm kullanıcılarla paylaşmak için:

1. Pano giriş sayfasında, tıklatın **Düzenle**.
2. Tıklatın **paylaşımı** ve ardından **ortak**.
3. Genel ortak Pano onay kutusu görüntülenir.
4. Tıklatın **Evet** Pano genel ortak Pano olarak ayarlamak için.

Bir Pano geçerli bir varlık tüm kullanıcılarla paylaşmak için:

1. Pano giriş sayfasından tıklatın **Düzenle**.
2. Tıklatın **paylaşımı** ve ardından **My varlık**.
3. Tıklatın **Evet** Pano ortak bir pano olarak ayarlamak için.

## <a name="duplicate-a-custom-dashboard"></a>Özel bir Pano Çoğalt

Yeni bir Pano oluşturduğunuzda, varolan bir panoyu benzer özelliklerinden kullanmak isteyebilirsiniz. Yeni bir tane oluşturmak için panoyu çoğaltabilirsiniz.

Yalnızca özel panolar çoğaltabilirsiniz. Standart panolar aynı olamaz.

(Kopya) özel bir Pano çoğaltmak için:

1. Çoğaltmak istediğiniz Panoda tıklatın **Kaydet**. Yeni bir Pano aynı ada ve bir numara ile açılır.
2. Yinelenen Panoyu yeniden adlandırın ve istediğiniz gibi değiştirebilirsiniz.

-Veya-

1. Pano ayarlar'ı **Kaydet** çoğaltmak istediğiniz panonun satırındaki.
2. Yinelenen panosu açılır.
3. Panoyu yeniden adlandırın ve istediğiniz gibi değiştirebilirsiniz.

## <a name="set-a-default-dashboard"></a>Varsayılan Pano ayarlama

Varsayılan olarak herhangi bir pano ayarlayabilirsiniz. Varsayılan ayarı Pano sekmesi listesi en sol sekmede olarak görünmesini sağlar. Varsayılan Pano ne zaman görüntüler Cloudyn Portalı'nı açın.

- İstediğiniz varsayılan olarak ayarlayın ve ardından Pano sekmesini **varsayılan** sağdaki.

-Veya-

1. Tıklatın **Pano ayarları** kullanılabilir panolar listesini görmek ve varsayılan olarak ayarlamak istediğiniz Pano seçin.  
    ![Pano Seçenekleri](./media/dashboards/dashboard-options.png)
2. Tıklatın **varsayılan** Pano satırında. Varsayılan Pano onay kutusu görüntülenir.
3. **Evet**'e tıklayın. Pano, varsayılan olarak ayarlanır.

## <a name="management-dashboard"></a>Yönetim panosu
Yönetim (veya MSP Pano MSP kullanıcılar için) Pano vurgular ana rapor türleri içerir.  
![Yönetim panosu](./media/dashboards/management-dash.png)

### <a name="cost-entity-summary-enterprise-only"></a>Maliyet varlık özeti (yalnızca Kurumsal)
Bu pencere öğesi varlıkların sayısı ve hesap sayısı dahil olmak üzere yönetilen maliyet varlıklar özetler.
- Kuruluş Ayrıntıları raporu açmak için pencere öğesini tıklatın.

### <a name="cost-over-time"></a>Zaman içinde maliyet
Bu pencere öğesi maliyet eğilimleri yardımcı olabilir. Son 30 gün eğilimini temel maliyeti son gündür vurgular.
- Pencere öğesini görüntülemek için gerçek zamanlı maliyeti raporu açmak için tıklayın ve filtre ek ayrıntılar.

### <a name="asset-controller"></a>Varlık denetleyicisi
Bu pencere öğesini son 30 gün içinde kullanım eğilim yukarıda önceki günden örnekleri çalışan sayısı vurgular.
- Varlık denetleyicisi panosunu açmak için pencere öğesini tıklatın.

### <a name="unused-ri-detector"></a>Kullanılmayan RI algılayıcısı
Bu pencere öğesi Amazon EC2 sayısı vurgular kullanılmayan ayırmaları.
- Pencere öğesi görüntüleyebileceğiniz kullanılmayan şu anda kullanılmayan ayırmaları raporu açmak için tıklatın ayırmaları değiştirebilirsiniz.

### <a name="cost-by-service"></a>Hizmeti tarafından maliyet
Bu pencere, son 30 gün için hizmeti tarafından amortized maliyetleri vurgular. Her hizmet maliyetleri görmek için pasta grafiğin üzerine gelin.
- Gerçek maliyet analiz raporu açmak için pencere öğesini tıklatın.

### <a name="potential-savings"></a>Olası tasarrufları
Bu pencere öğesi örneği türü önerileri Amazon EC2 ve Amazon RDS için fiyatlandırma gösterir
- Tasarruf analiz raporu pencere aç'ı tıklatın. Maliyetlerinizi örneği türleriyle olası tasarrufları göre listeler.

### <a name="compute-instances---daily-trend"></a>İşlem örnekleri - günlük eğilimi
Bu pencere etkin örnekler için son 30 gün türüne göre görüntüler.
- Son 30 gün içinde çalışan tüm örneği dökümünü görüntüleyebileceğiniz zaman içinde örnekleri raporu açmak için pencere öğesini tıklatın.

### <a name="storage-by-department"></a>Depolama Birimi departmanı tarafından
Bu pencere öğesi Departmanlar tarafından kullanılan depolama hizmetleri görüntüler. Depolama tüketim bölümünüzün görmek için pasta grafiğin üzerine gelin.
- S3 İzleyicisi panoyu açmak için pencere öğesini tıklatın.

## <a name="cost-controller-dashboard"></a>Maliyet denetleyicisi Panosu
Maliyet denetleyicisi Panosu, önceden ayarlanmış maliyet ayırma vurgular gösterir.  
![Maliyet denetleyicisi Panosu](./media/dashboards/cost-controller-dashboard.png)

### <a name="cost-over-time"></a>Zaman içinde maliyet
Bu pencere öğesi maliyet eğilimleri yardımcı olur. Son 30 gün eğilimini temel maliyeti son gündür vurgular.
- Pencere öğesini görüntülemek için gerçek zamanlı maliyeti raporu açmak için tıklayın ve filtre ek ayrıntılar.

### <a name="monthly-cost-trends"></a>Aylık maliyet eğilimleri
Bu pencere öğesi tahmini amortized harcama vurgular ve ayın başlangıcını itibaren gerçek harcamanızı.
- Özet bir ay tarih maliyeti sağlar geçerli ay tahmini maliyet raporu açmak için pencere öğesini tıklatın.

Bu rapor, ay, önceki ayın maliyet ve geçerli ay tahmini maliyet başından itibaren maliyet gösterir. Geçerli ay tahmini maliyet güncel aylık maliyeti ve projeksiyon ekleyerek hesaplanır. Projeksiyon son 30 gün içinde izlenen maliyeti temel alır.

### <a name="12-month-planner"></a>12 aylık Planlayıcısı
Bu pencere öğesi, bir sonraki 12 ay ve olası tasarrufları üzerinden tahmini maliyetleri vurgular.
- Yıllık tahmini maliyet raporu açmak için pencere öğesini tıklatın.

### <a name="cost-by-service"></a>Hizmeti tarafından maliyet
Bu pencere, son 30 gün için hizmeti tarafından amortized maliyetleri vurgular.
- Her hizmet maliyetleri görmek için pasta grafiğin üzerine gelin.
- Gerçek maliyet analiz raporu açmak için pencere öğesini tıklatın.

### <a name="cost-by-account"></a>Hesaba göre maliyet
Bu pencere öğesi amortized maliyetleri hesabı tarafından son 30 gün boyunca vurgular.
- Hesap başına maliyetleri görmek için pasta grafiğin üzerine gelin.
- Gerçek maliyet analiz raporu açmak için pencere öğesini tıklatın.

### <a name="cost-trend-by-day"></a>Gün bazında maliyet eğilimi
Vurgular harcamanız son 30 gün içinde bu pencere öğesi.
- Gün başına maliyetleri görmek için çubuk grafik üzerine gelerek.
- Gerçek zamanlı maliyeti raporu açmak için pencere öğesini tıklatın.

### <a name="cost-trend-by-month---last-6-months"></a>Ay - Son 6 ay bazında maliyet eğilimi

Vurgular harcamanız son altı ay içinde bu pencere öğesi.
- Aylık maliyetleri görmek için çubuk grafik üzerine gelerek.
- Gerçek zamanlı maliyeti raporu açmak için pencere öğesini tıklatın.

## <a name="asset-controller-dashboard"></a>Varlık denetleyicisi Panosu

Bu panoyu çalışan örnekleri, kullanılabilir ve kullanımdaki diskler, dağıtım örnek türleri ve depolama bilgilerini sayısını görüntüler.  
![Varlık denetleyicisi Panosu](./media/dashboards/asset-controller-dashboard.png)

### <a name="compute-instances"></a>İşlem örnekleri
Bu pencere öğesi örnek son 30 gün içinde kullanım eğilim göre çalışan sayısını görüntüler.
- Zaman içinde örnekleri raporu açmak için pencere öğesini tıklatın.

### <a name="disks"></a>Diskler
Bu pencere öğesi sayısı ve kullanımdaki ve kullanılabilir diskler, hacmi vurgular.
- Etkin diskleri raporu açmak için pencere öğesini tıklatın.

### <a name="instance-type-distribution"></a>Örnek türü dağıtım
Bu pencere öğesi pasta grafik örneği türlerinde vurgular.
- Pencere öğesi tarafından seçilen toplama etkin örneği dökümünü sağlar örnek dağıtım raporu açmak için tıklayın.

### <a name="compute-instances---daily-trend"></a>İşlem örnekleri - günlük eğilimi
Bu pencere öğesini son 30 gün için günlük işlem örnekleri (nokta, ayrılmış ve isteğe bağlı) vurgular.
- Gün başına tür başına işlem örneklerinin sayısını görüntülemek için grafiği üzerine gelerek.
- Zaman içinde örnekleri raporu açmak için pencere öğesini tıklatın.

### <a name="all-buckets-s3"></a>Tüm demet (S3)
Bu pencere öğesi depolanan nesneler sayısı ve toplam S3 depolama vurgular.
- S3 İzleyicisi panoyu açmak için pencere öğesini tıklatın. Pano bulmak, çözümlemek ve geçerli depolama alanı kullanımı ve eğilimleri görüntülemek yardımcı olur.

### <a name="sql-db-instances-rds"></a>SQL DB örnekleri (RDS)
Bu pencere öğesi Amazon RDS örnek son 30 gün eğilimini göre çalışan sayısı vurgular.
- Zaman içinde RDS örnek raporu açmak için pencere öğesini tıklatın.

## <a name="optimizer-dashboard"></a>İyileştirici Panosu
Bu panoyu downsizing öneriler, kullanılmayan kaynaklar ve olası tasarrufları görüntüler.  
![İyileştirici Panosu](./media/dashboards/optimizer-dashboard.png)

### <a name="ri-calculator"></a>RI hesaplayıcısı
Bu pencere öğesi RI satın alma önerileri sayısını görüntüler ve olası yıllık tasarruflar vurgulanmaktadır.
- Fiyatlandırma planı ayrılmış ne zaman isteğe bağlı karşılaştırması kullanılacağını belirleme burada ayrılmış örnek hesaplayıcı açmak için pencere öğesini tıklatın.

### <a name="sizing"></a>Boyutlandırma
Bu pencere öğesi uygulanırsa, önerilen boyutlandırma ve olası tasarrufları vurgular.
- Pencere öğesi EC2 maliyet etkili boyutlandırma önerileri raporu açmak için tıklatın.

### <a name="unused-ri-detector"></a>Kullanılmayan RI algılayıcısı
Bu pencere öğesi Amazon EC2 sayısı vurgular kullanılmayan ayırmaları.
- Pencere öğesi değiştirebileceğiniz kullanılmayan ayırmaları görüntüleyebileceğiniz şu anda kullanılmayan ayırmaları raporu açmak için tıklatın.

###  <a name="available-disks"></a>Kullanılabilir diskler
Bu pencere öğesi dağıtımınızdaki eklenmemiş disk sayısı vurgular.
- Pencere öğesi eklenmemiş diskleri raporu açmak için tıklatın.

### <a name="rds-ri-calculator"></a>RDS RI hesaplayıcısı
Bu pencere öğesi Amazon RDS örneklerinizi ve olası tasarruf için ayırma önerileri sayısı vurgular.
- Ayrılmış örnekler isteğe bağlı örnekleri yerine kullanılacak Cloudyn önerileri görebileceğiniz RDS RI satın önerileri raporu açmak için pencere öğesini tıklatın.

### <a name="rds-sizing"></a>RDS boyutlandırma
Bu pencere öğesi boyutlandırma önerileri ve olası tasarrufları sayısını gösterir.
- Amazon önerileri boyutlandırma RDS ayrıntılı hangi görüntüler RDS boyutlandırma önerileri raporu açmak için pencere öğesini tıklatın.

En iyi duruma getirme önerileri son bir ayda izlenen kullanım ve performans verilerini temel alır.

## <a name="s3-tracker-dashboard"></a>S3 İzleyicisi Panosu
S3 İzleyicisi panoyu bulmak, çözümlemek ve geçerli depolama alanı kullanımı ve eğilimleri görüntülemek yardımcı olur.  
![S3 İzleyicisi Panosu](./media/dashboards/s3-tracker-dashboard.png)

### <a name="all-buckets"></a>Tüm demet
Bu pencere öğesi GB, tüm, demet ve, demet nesnelerinin toplam sayısı toplam boyutu vurgular.
- S3 boyutu dağıtım raporu açmak için pencere öğesini tıklatın. Raporun S3 boyutunuz demet, en üst düzey klasörü, depolama sınıfı ve sürüm oluşturma durumu tarafından analiz etmenize yardımcı olur.

### <a name="bucket-properties"></a>Demet özellikleri
Bu pencere öğesi toplam sayısı depolama vurgular.
- S3 Demetini özellikleri raporu görüntülemek için pencere öğesini tıklatın.

### <a name="scan-status"></a>Durum tarama
Bu pencere öğesi son S3 tarama bittiğinde ve bir sonraki başlayacağını vurgular.
- S3 tarama durum raporunu açmak için pencere öğesini tıklatın.

### <a name="storage-by-bucket"></a>Demet tarafından depolama
Bu pencere öğesi her demet depolama sınıfı kullanarak yüzdesi vurgular.
- S3 boyutu dağıtım raporu açmak için pencere öğesini tıklatın. Raporun S3 boyutunuz demet, en üst düzey klasörü, depolama sınıfı ve sürüm oluşturma durumu tarafından analiz etmenize yardımcı olur.

### <a name="number-of-objects-by-bucket"></a>Nesne tarafından demet sayısı
Bu pencere öğesi nesne başına gerçek sayısını ve yüzdesini demet sayısı vurgular. Toplam nesneleri görmek için demet gelin.
- (Temel tarama) S3 boyutu dağıtım raporu açmak için pencere öğesini tıklatın.

## <a name="cloud-comparison-dashboard"></a>Bulut karşılaştırma Panosu
Bulut karşılaştırma Pano fiyatlandırma, CPU türü ve RAM boyutu göre farklı bulut sağlayıcılardan maliyetleri karşılaştırmanıza yardımcı olur.  
![Bulut karşılaştırma Panosu](./media/dashboards/cloud-comparison-dashboard.png)

### <a name="ec2-cost-in-azure-by-instance-type"></a>EC2 Azure örneği türü maliyeti
Bu pencere öğesini son 30 gün isteğe bağlı oranları kullanımının vurgular. Bu comares geçerli Amazon EC2 maliyetle Maliyet vs olası Azure'da maliyet.
- Örnek türü başına maliyet Karşılaştırılacak çubukları üzerine gelerek.
- Bağlantı noktası oluşturma dağıtımınızı – maliyet analiz raporu açmak için pencere öğesini tıklatın.

### <a name="ec2-cost-in-azure"></a>EC2 Azure maliyeti
Bu pencere öğesi geçerli Amazon EC2 maliyetleriniz gösterir ve bunları Azure'a karşılaştırır. Karşılaştırma isteğe bağlı oranları kullanımı son 30 gün temel alır.
- Bağlantı noktası oluşturma dağıtımınızı - maliyet analiz raporu açmak için pencere öğesini tıklatın.

### <a name="ec2azure-instance-type-mapping"></a>EC2/Azure örneğinin türü eşlemesi
Bu pencere öğesi esnek işlem birimleri Amazon EC2 ve Azure arasında en iyi Eşleştirme vurgular.
- Pencere öğesi türü örnekleri eşleme raporu açmak için tıklatın.
