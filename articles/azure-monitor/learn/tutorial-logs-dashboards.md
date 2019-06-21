---
title: Azure Log Analytics verilerinin panolarını oluşturma ve paylaşma | Microsoft Docs
description: Bu öğretici, Log Analytics panoları tüm kayıtlı günlük sorgularınızı nasıl görselleştirebilirsiniz ortamınızı görüntülemek için tek bir mercek sunarak anlamanıza yardımcı olur.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 93cda8680bc665055d449e86c24d6565f6fc525f
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67296386"
---
# <a name="create-and-share-dashboards-of-log-analytics-data"></a>Log Analytics verilerinin panolarını oluşturma ve paylaşma

Panoları tüm kayıtlı günlük sorgularınızı görselleştirebilirsiniz log Analytics bulmak için pack'ten ilişkilendirin ve kuruluştaki BT işlem verilerini paylaşın.  Bu öğretici, BT işlemleri destek ekibiniz tarafından erişilecek paylaşılan bir panoyu desteklemek için kullanılacak bir günlük sorgusu oluşturma konusunu ele alınmaktadır.  Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Azure portalında paylaşılan bir pano oluşturma
> * Bir performans günlük sorgusu görselleştirin 
> * Paylaşılan panoya günlük sorgusu ekleme 
> * Paylaşılan bir panodaki kutucuğu özelleştirme

Bu öğreticideki örneği tamamlamak için [Log Analytics çalışma alanına bağlı](quick-collect-azurevm.md) mevcut bir sanal makinenizin olması gerekir.  
 
## <a name="sign-in-to-azure-portal"></a>Azure portalda oturum açın
[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın. 

## <a name="create-a-shared-dashboard"></a>Paylaşılan pano oluşturma
Seçin **Pano** varsayılan açmak için [Pano](../../azure-portal/azure-portal-dashboards.md). Panonuzu, aşağıdaki örnekte farklı görünecektir.

![Azure portalı Panosu](media/tutorial-logs-dashboards/log-analytics-portal-dashboard.png)

Burada tüm Azure kaynaklarınız arasında Azure Log Analytics telemetri verileri gibi BT için en önemli olan işlem verilerini bir araya getirebilirsiniz.  Günlük sorgusu görselleştirmeye adım atmadan önce şimdi ilk bir pano oluşturun ve paylaşın.  Biz, ardından bir çizgi grafik işleyin ve panoya ekleme bizim örnek performans günlük sorgusu üzerinde odaklanabilirsiniz.  

Bir pano oluşturmak için geçerli pano adının yanındaki **Yeni pano** düğmesini seçin.

![Azure portalında yeni pano oluşturma](media/tutorial-logs-dashboards/log-analytics-create-dashboard-01.png)

Bu işlem yeni, boş ve özel bir pano oluşturur ve panonuzu adlandırıp kutucuklar ekleyebileceğiniz ya da yeniden düzenleyebileceğiniz özelleştirme modunu açar. Panonun adını düzenleyin ve *Sample panosunu* Bu öğretici için ardından **özelleştirme Bitti**.<br><br> ![Özelleştirilmiş Azure panosunu kaydetme](media/tutorial-logs-dashboards/log-analytics-create-dashboard-02.png)

Bir pano oluşturulduğunda varsayılan olarak gizlidir, yani onu yalnızca siz görebilirsiniz. Panoyu başkalarının görebilmesi için, diğer pano komutlarıyla birlikte görünen **Paylaş** düğmesini kullanın.

![Azure portalında yeni pano paylaşma](media/tutorial-logs-dashboards/log-analytics-share-dashboard.png) 

Panonuzun yayımlanması için bir abonelik ve kaynak grubu seçmeniz istenir. Kolaylık olması için portalın yayımlama deneyimi, bir kaynak grubuna panoları yerleştirdiğiniz **panolar** adlı bir modelde size kılavuzluk eder.  Seçili olan aboneliği doğrulayın ve sonra **Yayımla**’ya tıklayın.  Panoda gösterilen bilgilere erişim, [Azure Kaynak Tabanlı Erişim Denetimi](../../role-based-access-control/role-assignments-portal.md) ile denetlenir.   

## <a name="visualize-a-log-query"></a>Günlük sorgusu görselleştirin
[Log Analytics](../log-query/get-started-portal.md) günlük sorguları ve sonuçları ile çalışmak için kullanılan adanmış bir portaldır. Birden fazla satırda sorgu düzenleme, seçerek kod yürütme, bağlama duyarlı IntelliSense ve Akıllı Analiz özellikleri mevcuttur. Bu öğreticide, Log Analytics kaydetmek gelecekteki bir sorgu için grafik formundaki bir performans görünümü oluşturma ve daha önce oluşturduğunuz paylaşılan panoya sabitleyebilirsiniz kullanın.

Log Analytics seçerek açın **günlükleri** Azure İzleyici menüsünde. Yeni bir boş sorgu ile başlar.

![Giriş sayfası](media/tutorial-logs-dashboards/homepage.png)

İşlemci kullanımı kayıtlarını Computer ve TimeGenerated ölçütüne göre gruplandırılmış ve görsel bir grafikte görüntülenen hem Windows hem de Linux bilgisayarlar için geri dönmek için aşağıdaki sorguyu girin. Tıklayın **çalıştırma** sorgu çalıştırmak ve sonuçta elde edilen grafiğin görüntülemek için.

```Kusto
Perf 
| where CounterName == "% Processor Time" and ObjectName == "Processor" and InstanceName == "_Total" 
| summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 1hr), Computer 
| render timechart
```

Seçerek sorguyu kaydedin **Kaydet** üst kısmındaki düğmesinden.

![Sorguyu Kaydet](media/tutorial-logs-dashboards/save-query.png)

İçinde **Sorguyu Kaydet** gibi bir ad verin, Denetim Masası *Azure Vm'leri - işlemci kullanımı* ve gibi bir kategori *panolar* ve ardından **Kaydet** .  Ortak sorgu kitaplığı oluşturabilirsiniz, bu şekilde değiştirin ve kullanın.  Son olarak, bu seçerek daha önce oluşturduğunuz paylaşılan panoya sabitleyin **PIN** sayfasının ve ardından Pano adını seçerek sağ üst köşedeki düğmesinden.

Artık panoya sabitlenmiş bir sorgumuz olduğuna göre, genel bir başlığının ve altında bir yorumun olduğunu fark edeceksiniz.

![Azure Panosu örneği](media/tutorial-logs-dashboards/log-analytics-modify-dashboard-01.png)

 Bu panoya, görüntüleyenler tarafından kolayca anlaşılacak, anlamlı bir ad vermeliyiz.  Başlık ve alt konu başlığını döşeme için özelleştirmek ve ardından Düzenle düğmesini tıklatın **güncelleştirme**.  Değişiklikleri yayımlamanızı veya atmanızı isteyen bir başlık görüntülenir.  Tıklayın **bir kopyasını Kaydet**.  

![Örnek pano yapılandırması tamamlandı](media/tutorial-logs-dashboards/log-analytics-modify-dashboard-02.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure portalında bir pano oluşturun ve bir günlük sorgusu eklemek öğrendiniz.  Uygulayabileceğiniz farklı yanıtlar hakkında bilgi almak için sonraki öğreticiye ilerleyin günlük sorgu sonuçlarına göre.  

> [!div class="nextstepaction"]
> [Log Analytics Uyarılarıyla olaylara yanıt verme](tutorial-response.md)
