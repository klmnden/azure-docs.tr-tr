---
title: Azure'da Cloudyn panoları ile ana ölçümleri görüntüleme | Microsoft Docs
description: Bu makalede, panolar sayesinde önemli ölçümleri Cloudyn'de nasıl görüntüleyebileceğiniz açıklanır.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 05/21/2019
ms.topic: conceptual
ms.service: cost-management
manager: vitavor
ms.custom: seodec18
ms.openlocfilehash: b83368b913bf1303b49e3a56e3a15248af222cbe
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66002060"
---
# <a name="view-key-cost-metrics-with-dashboards"></a>Ölçümleri panolarla maliyet anahtarını görüntüle

Cloudyn panoları, raporları üst düzey bir görünümünü sağlar. Panolar, önemli maliyet ölçümleri tek bir görünümde olanak tanır. Ayrıca, önemli iş kararları almanıza yardımcı olmak için iş eğilim vurgular sağlarlar.

Panolar için farklı sorumluluklara içerebilir, kuruluşunuzdaki kişilerle görünümleri oluşturmak için de kullanılır:

- Finansal denetleyicisi
- Uygulama veya proje sahibi
- DevOps mühendisi
- Yöneticiler

Pano pencere öğelerinizi yapılır ve aslında bir rapor küçük resim her pencere öğesi ise. Bir pencere öğesi, bir raporu açmak için tıklayın. Raporları'nı özelleştirdiğinizde raporlarım için Kaydet ve panonuza.

Pano sürüm Yönetimi (MSP), Enterprise ve Premium Cloudyn kullanıcılar için farklı. Farklar varlık erişim düzeyleri tarafından belirlenir. Erişim düzeyleri hakkında daha fazla bilgi için bkz. [varlık erişim düzeyleri](tutorial-user-access.md#entity-access-levels).

Pano kullanılabilirlik panolar görüntülenirken kullanılan bulut hizmet sağlayıcısı hesabı türüne bağlıdır. Panolar, raporlar kullanılabilir ve Cloudyn tarafından toplanan bilgi türünü etkiler. Örneğin, bir AWS hesabınız yoksa S3 İzleyicisi panoyu görmezsiniz. Benzer şekilde, Azure Resource Manager Cloudyn'e erişim izni etkinleştirmezseniz iyileştirici Pano pencere öğeleri herhangi bir Azure özel bilgi görmezsiniz.

Önceden hazırlanmış panolar dilediğinizi kullanabilirsiniz veya kendi panonuzu ile özelleştirilmiş raporlar oluşturabilirsiniz. Cloudyn raporlarla bilmiyorsanız bkz [kullanım Cloudyn raporlarını](use-reports.md).

## <a name="create-a-custom-dashboard"></a>Özel bir pano oluşturma

Özel bir pano ile hızla çalışmaya başlamak için özelliklerini kullanmak için mevcut bir çoğaltabilirsiniz. Ardından, gereksinimlerinize uyacak şekilde değiştirebilirsiniz. Kopyalamak istediğiniz panosunda **Kaydet**. Özelleştirilmiş Panolar yalnızca çoğaltabilirsiniz — Cloudyn'e dahil olan panoları yineleyemez.

Özel bir Pano oluşturmak için:

1. Giriş sayfasında tıklayın **yeni Ekle +**. Panom'u sayfası görüntülenir.  
    ![Yeni raporlar eklediğiniz my Pano sayfası](./media/dashboards/my-dashboard.png)
2. Tıklayın **yeni bir rapor eklemek**. Rapor Ekle kutusu görüntülenir.
3. Pano pencere öğesine eklemek istediğiniz raporu seçin. Pencere öğesinin panoya eklendi.
4. Pano tamamlanana kadar yukarıdaki adımları yineleyin.
5. Panonun adını değiştirmek için Pano giriş sayfası panonun adına tıklayın ve yeni adı yazın.

## <a name="modify-a-custom-dashboard"></a>Özel bir Pano değiştirme

Özel bir pano oluşturma gibi Cloudyn'e dahil panolar değiştiremezsiniz. Özel Pano raporunu değiştirmek için:

1. Panoda tıklayın ve değiştirmek istediğiniz raporu Bul **Düzenle**. Rapor görüntülenir.
2. Raporu istediğiniz değişiklikleri yapın **Kaydet**. Rapor güncelleştirilir ve yaptığınız değişiklikleri görüntüler.

## <a name="share-a-custom-dashboard"></a>Özel bir panoyu paylaşma

Özel bir Pano için diğer kişilerle paylaşabilirsiniz _genel_ veya _My varlık_. Tüm kullanıcılar, ortak paylaştığınızda panoyu görüntüleyebilirsiniz. Yalnızca geçerli varlık erişimi olan kullanıcılar, My varlığa paylaştığınızda panoyu görüntüleyebilirsiniz. My varlık ve genel ile özel bir pano paylaşma adımları benzerdir.

Kamu için özel bir panoyu paylaşmak için:

1. Bir Panoda tıklayın **Pano ayarları**. Pano ayarları kutusu görüntülenir.  
    ![özel bir Pano için Pano ayarları](./media/dashboards/dashboard-options.png)
2. Pano Ayarları iletişim kutusunda, ok simgesine tıklayın ve ardından **genel**. Genel Pano onay iletişim kutusunda görüntülenir.
3. Tıklayın **Evet**. Panoyu başkalarının kullanıma sunuldu.

## <a name="delete-a-custom-dashboard-report"></a>Özel Pano raporu Sil

Panodan bir özel rapor bileşeni silebilirsiniz. Panodan raporu sildiğinizde raporun raporlar listesinden silinmesine neden olmaz. Bunun yerine, raporu sildiğinizde yalnızca panodan kaldırır.

Pano bileşen üzerinde bir Pano bileşeni silmek için tıklayın **Sil**. Tıklayarak **Sil** hemen Pano bileşenleri siler.

## <a name="share-a-dashboard-enterprise"></a>(Kurumsal) pano paylaşma

Tüm kullanıcılar için özel panolar, kuruluşunuzdaki veya geçerli varlık kullanıcılarla paylaşabilirsiniz. Pano paylaşımını diğerleri KPI'niz hızlı üst düzey bir görünümünü verebilir. Bir panoyu paylaştığınızda otomatik olarak tüm Cloudyn varlık/müşterileriniz için panoya çoğaltır. Paylaşılan panonun değişiklikler otomatik olarak güncelleştirilir.

Bir panoyu subentities dahil olmak üzere tüm kullanıcılarla paylaşmak için:

1. Pano giriş sayfasında tıklayın **Düzenle**.
2. Tıklayın **paylaşımı** seçip **genel**.
3. Genel genel Pano onay kutusu görüntülenir.
4. Tıklayın **Evet** Pano genel genel Pano olarak ayarlamak için.

Geçerli bir varlığın tüm kullanıcılarla pano paylaşma için:

1. Pano giriş sayfasından tıklayın **Düzenle**.
2. Tıklayın **paylaşımı** seçip **My varlık**.
3. Tıklayın **Evet** Pano genel bir pano olarak ayarlamak için.

## <a name="duplicate-a-custom-dashboard"></a>Özel bir panoyu Yinele

Yeni bir Pano oluşturduğunuzda, mevcut bir panoya benzer özellikleri kullanmak isteyebilirsiniz. Panoda yeni bir tane oluşturabilirsiniz yineleyebilirsiniz.

Yalnızca özel panolar çoğaltabilirsiniz. Standart panolar yineleyemez.

Yinelenen (kopya) özel bir Pano için:

1. Çoğaltmak istediğiniz panosunda **Kaydet**. Yeni bir pano, aynı ada ve bir sayı ile açılır.
2. Yinelenen Panoyu yeniden adlandırma ve istediğiniz gibi değiştirin.

-Veya-

1. Pano Ayarları'nda tıklatın **Kaydet** çoğaltmak istediğiniz panonun satırında.
2. Yinelenen Pano açılır.
3. Panoyu yeniden adlandırma ve istediğiniz gibi değiştirin.

## <a name="set-a-default-dashboard"></a>Varsayılan bir Pano Ayarla

Varsayılan olarak, herhangi bir panoyu ayarlayabilirsiniz. İçin varsayılan ayar, Pano sekmesi listenin en sol sekmede görünür hale getirir. Varsayılan Pano ne zaman görüntüler Cloudyn portalını açın.

- Varsayılan olarak ayarlayın ve ardından istediğiniz Pano sekmesini **varsayılan** sağ.

-Veya-

1. Tıklayın **Pano ayarları** kullanılabilir durumdaki panoların listesini görmek ve varsayılan olarak ayarlamak istediğiniz panoyu seçin.  
    ![Varsayılan bir Pano için Pano Seçenekleri](./media/dashboards/dashboard-options.png)
2. Tıklayın **varsayılan** satırında Pano. Varsayılan Pano onay kutusu görüntülenir.
3. **Evet**'e tıklayın. Pano, varsayılan olarak ayarlanır.

## <a name="management-dashboard"></a>Yönetim panosu
Yönetim (veya MSP Pano MSP kullanıcılar için) Pano Ana rapor türleri en önemli özellikleri içerir.  
![Çeşitli raporlar gösteren Yönetim Panosu](./media/dashboards/management-dash.png)

### <a name="cost-entity-summary-enterprise-only"></a>Maliyet varlığı örneği (yalnızca Kurumsal)
Bu pencere öğesi varlıkların sayısı ve hesap sayısı dahil olmak üzere yönetilen maliyet varlıkları özetler.
- Pencere öğesi, kuruluş Ayrıntıları raporu açmak için tıklayın.

### <a name="cost-over-time"></a>Zaman temelinde maliyet
Bu pencere öğesi maliyet eğilimleri yardımcı olabilir. Son günün maliyeti eğilimini ve hataların son 30 gün üzerinde temel vurgular.
- Pencere öğesi görüntülemek için zaman içinde gerçek maliyet raporu açmak için tıklayın ve filtre ek ayrıntılar.

### <a name="asset-controller"></a>Varlık denetleyicisi
Bu pencere öğesini son 30 gün içinde kullanım eğilim yukarıda önceki günden itibaren örnekleri çalışan sayısını vurgular.
- Pencere denetleyicisi varlık panoyu açmak için tıklayın.

### <a name="unused-ri-detector"></a>Kullanılmamış RI algılayıcısı
Amazon EC2 sayısı bu pencere öğesini vurgular kullanılmamış durumdaki ayırmalar.
- Pencere öğesi görüntüleyebileceğiniz kullanılmayan şu anda kullanılmamış durumdaki ayırmalar raporu açmak için tıklayın ayırmaları değiştirebilirsiniz.

### <a name="cost-by-service"></a>Hizmete göre maliyet
Bu pencere öğesi, son 30 gün için hizmete göre amorti edilmiş maliyet vurgular. Hizmet başına maliyetleri görmek için pasta grafiği üzerine gelin.
- Pencere öğesi gerçek maliyet analizi raporu açmak için tıklayın.

### <a name="potential-savings"></a>Olası tasarruf
Bu pencere öğesi örnek türü fiyatlandırma önerileri Amazon EC2 ve Amazon RDS'yi gösterir
- Tasarruf analiz raporu pencere öğesi Aç'a tıklayın. Olası tasarruf ile örnek türlerine göre maliyetlerinizi listeler.

### <a name="compute-instances---daily-trend"></a>İşlem örnekleri - günlük eğilim
Bu pencere öğesi, son 30 gün boyunca etkin örnekler türüne göre görüntüler.
- Pencere öğesinin son 30 gün içinde çalışan tüm örneklerinin dökümünü görüntüleyebileceğiniz zamana göre örnekler raporu açmak için tıklayın.

### <a name="storage-by-department"></a>Departmana göre depolama
Bu pencere öğesi departmanları tarafından kullanılan depolama hizmetleri görüntüler. Departmana göre depolama tüketiminiz görmek için pasta grafiğin üzerine gelin.
- Pencere öğesi S3 İzleyicisi panoyu açmak için tıklayın.

## <a name="cost-controller-dashboard"></a>Maliyet denetleyicisi Panosu
Maliyet denetleyicisi Panosu, önceden ayarlanmış maliyet ayırma öne çıkan özellikleri gösterir.  
![Çeşitli raporlar gösteren maliyet denetleyici Panosu](./media/dashboards/cost-controller-dashboard.png)

### <a name="cost-over-time"></a>Zaman temelinde maliyet
Bu pencere öğesi maliyet eğilimleri yardımcı olur. Son günün maliyeti eğilimini ve hataların son 30 gün üzerinde temel vurgular.
- Pencere öğesi görüntülemek için zaman içinde gerçek maliyet raporu açmak için tıklayın ve filtre ek ayrıntılar.

### <a name="monthly-cost-trends"></a>Aylık maliyet eğilimleri
Amorti edilmiş harcama tahmini Bu pencere öğesini vurgular ve gerçek itibaren ayın başlangıcını ayırabilirsiniz.
- Pencere öğesi, bir ay başından bu yana maliyet özeti sağlar geçerli ay öngörülen maliyet raporu açmak için tıklayın.

Bu rapor, ay, önceki ay maliyeti ve geçerli ay öngörülen maliyet başına maliyeti gösterir. Geçerli ay öngörülen maliyet yansıtma ve güncel aylık maliyet ekleyerek hesaplanır. Son 30 gün içindeki izlenen Maliyet Yansıtma dayanır.

### <a name="12-month-planner"></a>12 aylık Planlayıcısı
Bu pencere öğesi, sonraki 12 ay ve olası tasarruf üzerinden tahmini maliyetleri vurgular.
- Pencere öğesi yıllık öngörülen maliyet raporu açmak için tıklayın.

### <a name="cost-by-service"></a>Hizmete göre maliyet
Bu pencere öğesi, son 30 gün için hizmete göre amorti edilmiş maliyet vurgular.
- Hizmet başına maliyetleri görmek için pasta grafiği üzerine gelin.
- Pencere öğesi gerçek maliyet analizi raporu açmak için tıklayın.

### <a name="cost-by-account"></a>Hesaba göre maliyet
Bu pencere öğesi, son 30 güne ait hesaba göre amorti edilmiş maliyet vurgular.
- Hesap başına maliyetleri görmek için pasta grafiği üzerine gelin.
- Pencere öğesi gerçek maliyet analizi raporu açmak için tıklayın.

### <a name="cost-trend-by-day"></a>Günlük maliyet eğilimi
Bu pencere öğesini vurgular son 30 gün içindeki ayırın.
- Günlük maliyetleri görmek üzere çubuk grafik üzerine gelin.
- Pencere öğesi, zaman içinde gerçek maliyet raporunu açmak için tıklayın.

### <a name="cost-trend-by-month---last-6-months"></a>Maliyet ay - son 6 aya göre edinme miktarı eğilimi

Bu pencere öğesini vurgular son altı ay içinde harcama.
- Aylık maliyetleri görmek üzere çubuk grafik üzerine gelin.
- Pencere öğesi, zaman içinde gerçek maliyet raporunu açmak için tıklayın.

## <a name="asset-controller-dashboard"></a>Varlık denetleyici Panosu

Bu pano, örnekler, kullanılabilir ve kullanımdaki diskleri, örnek türleri ve Depolama bilgileri dağıtım çalışan sayısını görüntüler.  
![Çeşitli raporlar gösteren varlık denetleyici Panosu](./media/dashboards/asset-controller-dashboard.png)

### <a name="compute-instances"></a>İşlem örnekleri
Bu pencere öğesi son 30 gün içindeki kullanım eğilim üzerindeki göre örnek çalışan sayısını görüntüler.
- Pencere öğesinin örnekleri zaman içinde bir raporu açmak için tıklayın.

### <a name="disks"></a>Diskler
Bu pencere öğesi, toplam sayısı ve kullanımdaki ve kullanılabilir olan diskler hacmi vurgular.
- Pencere etkin diskleri raporu açmak için tıklayın.

### <a name="instance-type-distribution"></a>Örnek türü dağılımı
Bu pencere öğesi örnek türleri bir pasta grafiğinin vurgular.
- Pencere öğesi tarafından seçilen toplama etkin örneklerinizin dökümünü sağlar örnek dağıtım raporu açmak için tıklayın.

### <a name="compute-instances---daily-trend"></a>İşlem örnekleri - günlük eğilim
Son 30 gün için günde bir bilgi işlem örnekleri (nokta, ayrılmış ve isteğe bağlı) Bu pencere öğesini vurgular.
- Günlük türü başına işlem örneği sayısını görüntülemek için grafik üzerinde gezdirin.
- Pencere öğesinin örnekleri zaman içinde bir raporu açmak için tıklayın.

### <a name="all-buckets-s3"></a>Tüm demetleri (S3 için)
Bu pencere öğesi, depolanan nesnelerin sayısı ve toplam S3 depolama vurgular.
- Pencere öğesi S3 İzleyicisi panoyu açmak için tıklayın. Pano geçerli depolama kullanım ve eğilimleri görüntülemek, çözümlemek ve yardımcı olur.

### <a name="sql-db-instances-rds"></a>SQL DB örnekleri (RDS)
Bu pencere öğesi, Amazon RDS örneği son 30 gün eğilimini üzerinde çalışan sayısını vurgular.
- Pencere öğesi RDS örnek zaman içinde bir raporu açmak için tıklayın.

## <a name="optimizer-dashboard"></a>İyileştirici Panosu
Bu Pano downsizing öneriler, kullanılmamış kaynakları ve olası tasarruf görüntüler.  
![Çeşitli raporlar gösteren iyileştirici Panosu](./media/dashboards/optimizer-dashboard.png)

### <a name="ri-calculator"></a>RI hesaplayıcı
Bu pencere öğesi, RI satın alma önerileri sayısını görüntüler ve olası yıllık tasarrufları vurgular.
- Fiyatlandırma planı ayrılmış pencere öğesinin ayrılmış örneği ne zaman isteğe bağlı ve kullanılacağını belirleme burada Hesap Makinesi'ni açmak için tıklayın.

### <a name="sizing"></a>Boyutlandırma
Uygulanırsa, önerilen boyutlandırma ve olası tasarruf Bu pencere öğesini vurgular.
- Pencere öğesi EC2 uygun maliyetli boyutlandırma önerileri raporu açmak için tıklayın.

### <a name="unused-ri-detector"></a>Kullanılmamış RI algılayıcısı
Amazon EC2 sayısı bu pencere öğesini vurgular kullanılmamış durumdaki ayırmalar.
- Pencere öğesi görüntüleyebileceğiniz ve değiştirebileceğiniz kullanılmamış durumdaki ayırmalar şu anda kullanılmamış durumdaki ayırmalar raporu açmak için tıklayın.

###  <a name="available-disks"></a>Kullanılabilir diskler
Bu pencere öğesi, dağıtımınızdaki eklenmemiş disk sayısını vurgular.
- Pencere öğesi eklenmemiş disk raporu açmak için tıklayın.

### <a name="rds-ri-calculator"></a>RDS RI hesaplayıcı
Bu pencere öğesi, Amazon RDS örneklerinizin ve olası tasarruf için öneriler ayırma sayısını vurgular.
- Pencere öğesi ayrılmış örnekler isteğe bağlı örnekleri yerine kullanılacak Cloudyn önerileri görebileceğiniz RDS RI satın alma önerileri raporu açmak için tıklayın.

### <a name="rds-sizing"></a>RDS boyutlandırma
Bu pencere öğesi boyutlandırma önerileri ve olası tasarruf sayısını gösterir.
- Pencere öğesi görüntüleyen Amazon boyutlandırma önerileri RDS ayrıntılı RDS boyutlandırma önerileri raporu açmak için tıklayın.

Geçen ay izlenen kullanım ve performans verilerini iyileştirme önerileri temel alır.

## <a name="s3-tracker-dashboard"></a>S3 İzleyici Panosu
S3 İzleyicisi panoyu geçerli depolama kullanım ve eğilimleri görüntülemek, çözümlemek ve yardımcı olur.  
![Çeşitli raporlar gösteren S3 İzleyici Panosu](./media/dashboards/s3-tracker-dashboard.png)

### <a name="all-buckets"></a>Tüm demetleri
Bu pencere öğesi, tüm, demet GB ve toplam sayısı, demet nesnelerin toplam boyutu vurgular.
- Pencere öğesi S3 dağıtım boyutunu bir raporu açmak için tıklayın. Rapor S3 boyutunuz sepet, üst düzey klasör, depolama sınıfı ve sürüm oluşturma durumu tarafından analiz etmenize yardımcı olur.

### <a name="bucket-properties"></a>Demet özellikleri
Bu pencere öğesi, toplam depolama demet sayısını vurgular.
- Pencere öğesi S3 Demetini özellikleri raporu görüntülemek için tıklayın.

### <a name="scan-status"></a>Tarama durumu
Bu pencere öğesinin son S3 tarama bittiğinde ve bir sonraki başlayacak vurgular.
- Pencere öğesi S3 tarama durumu raporu açmak için tıklayın.

### <a name="storage-by-bucket"></a>Sepet olarak depolama
Bu pencere öğesi her demet depolama sınıfı kullanarak yüzde vurgular.
- Pencere öğesi S3 dağıtım boyutunu bir raporu açmak için tıklayın. Rapor S3 boyutunuz sepet, üst düzey klasör, depolama sınıfı ve sürüm oluşturma durumu tarafından analiz etmenize yardımcı olur.

### <a name="number-of-objects-by-bucket"></a>Nesne tarafından demet sayısı
Bu pencere öğesi gerçek sayısı ve yüzdesi, demet başına nesne sayısını vurgular. Toplam nesne görmek için demet gelin.
- Pencere öğesi (temel tarama) S3 boyutu dağıtım raporu açmak için tıklayın.

## <a name="cloud-comparison-dashboard"></a>Bulut karşılaştırma Panosu
Bulut karşılaştırma Pano maliyetleri fiyatlandırması, CPU türü ve RAM boyutu olmak üzere göre farklı bulut sağlayıcılardan karşılaştırmanıza yardımcı olur.  
![Çeşitli raporlar gösteren bulut karşılaştırma Panosu](./media/dashboards/cloud-comparison-dashboard.png)

### <a name="ec2-cost-in-azure-by-instance-type"></a>EC2 Azure örnek türü maliyeti
Bu pencere öğesi, isteğe bağlı oranları kullanımı son 30 Günün vurgular. Bu, Azure'da olası maliyet geçerli Amazon EC2 maliyet ve maliyetle karşılaştırır.
- Örnek türü başına maliyeti Karşılaştırılacak çubukların üzerinde gezdirin.
- Pencere öğesi taşıma dağıtımınızı – maliyet analizi raporunu açmak için tıklayın.

### <a name="ec2-cost-in-azure"></a>EC2 Azure'da maliyeti
Bu pencere öğesi, geçerli Amazon EC2 maliyetlerinizi gösterir ve bunları Azure'a karşılaştırır. Karşılaştırma kullanımı isteğe bağlı oranları, son 30 gün temel alır.
- Pencere öğesi taşıma dağıtımınızı - maliyet analizi raporunu açmak için tıklayın.

### <a name="ec2azure-instance-type-mapping"></a>EC2/Azure örneği tür eşlemesi
Bu pencere öğesi, Amazon EC2 ile Azure arasında esnek işlem birimleri en iyi Eşleştirme vurgular.
- Pencere öğesi örnekleri eşleme türü bir raporu açmak için tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
- Okuma [kullanım Cloudyn raporlarını](use-reports.md) raporlar hakkında daha fazla bilgi için makale.
