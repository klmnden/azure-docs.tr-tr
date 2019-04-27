---
title: Azure Log Analytics verilerinin panolarını oluşturma ve paylaşma | Microsoft Docs
description: Bu öğretici, Log Analytics panolarının ortamınızı görüntülemek için tek bir mercek sunarak tüm kayıtlı günlük aramalarınızı nasıl görüntüleyebileceğini anlamanız yardımcı olur.
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
ms.date: 09/14/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 5ed0cfba9abaed1f1fdbacc8fcf28918403b82f5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60589925"
---
# <a name="create-and-share-dashboards-of-log-analytics-data"></a>Log Analytics verilerinin panolarını oluşturma ve paylaşma

Log Analytics panoları tüm kayıtlı günlük aramalarınızı görselleştirebilir ve kuruluştaki BT işlem verilerini bulma, ilişkilendirme ve paylaşma olanağı sağlayabilir.  Bu öğretici, BT işlemleri destek ekibiniz tarafından erişilecek paylaşılan bir panoyu desteklemek için kullanılacak bir günlük araması oluşturma konusunu ele alınmaktadır.  Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Azure portalında paylaşılan bir pano oluşturma
> * Bir performans günlük aramasını görselleştirme 
> * Paylaşılan bir panoya günlük araması ekleme 
> * Paylaşılan bir panodaki kutucuğu özelleştirme

Bu öğreticideki örneği tamamlamak için [Log Analytics çalışma alanına bağlı](../../azure-monitor/learn/quick-collect-azurevm.md) mevcut bir sanal makinenizin olması gerekir.  
 
## <a name="log-in-to-azure-portal"></a>Azure portalında oturum açın
[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın. 

## <a name="create-a-shared-dashboard"></a>Paylaşılan pano oluşturma

Microsoft Azure portalında oturum açtıktan sonra gördüğünüz ilk şey bir [panodur](../../azure-portal/azure-portal-dashboards.md).<br> ![Azure portalı panosu](media/tutorial-logs-dashboards/log-analytics-portal-dashboard.png)

Burada tüm Azure kaynaklarınız arasında Azure Log Analytics telemetri verileri gibi BT için en önemli olan işlem verilerini bir araya getirebilirsiniz.  Bir günlük aramasını görselleştirmeye adım atmadan önce ilk olarak bir pano oluşturup paylaşalım.  Bunu yaparak, çizgi grafik halinde oluşturulacak örnek performans günlük aramamızı yapmadan önce bu engeli önümüzden kaldırabilir ve panoya ekleyebiliriz.  

Bir pano oluşturmak için geçerli pano adının yanındaki **Yeni pano** düğmesini seçin.<br> ![Azure portalında yeni pano oluşturma](media/tutorial-logs-dashboards/log-analytics-create-dashboard-01.png)

Bu işlem yeni, boş ve özel bir pano oluşturur ve panonuzu adlandırıp kutucuklar ekleyebileceğiniz ya da yeniden düzenleyebileceğiniz özelleştirme modunu açar. Panonun adını düzenleyin ve bu öğretici için *Örnek Pano*’yu belirtip **Özelleştirme bitti**’yi seçin.<br><br> ![Özelleştirilmiş Azure panosunu kaydetme](media/tutorial-logs-dashboards/log-analytics-create-dashboard-02.png)

Bir pano oluşturulduğunda varsayılan olarak gizlidir, yani onu yalnızca siz görebilirsiniz. Panoyu başkalarının görebilmesi için, diğer pano komutlarıyla birlikte görünen **Paylaş** düğmesini kullanın.<br> ![Azure portalında yeni pano paylaşma](media/tutorial-logs-dashboards/log-analytics-share-dashboard.png) 

Panonuzun yayımlanması için bir abonelik ve kaynak grubu seçmeniz istenir. Kolaylık olması için portalın yayımlama deneyimi, bir kaynak grubuna panoları yerleştirdiğiniz **panolar** adlı bir modelde size kılavuzluk eder.  Seçili olan aboneliği doğrulayın ve sonra **Yayımla**’ya tıklayın.  Panoda gösterilen bilgilere erişim, [Azure Kaynak Tabanlı Erişim Denetimi](../../role-based-access-control/role-assignments-portal.md) ile denetlenir.   

## <a name="visualize-a-log-search"></a>Günlük aramasını görselleştirme

Azure portalında günlük Araması portalından tek bir satırda temel sorgular oluşturabilirsiniz. Günlük Araması portalı bir dış portal başlatılmadan kullanılabilir ve onu kullanarak uyarı kuralları oluşturma, bilgisayar grupları oluşturma ve sorgunun sonuçlarını dışarı aktarma gibi günlük aramalarıyla ilgili çeşitli işlevler gerçekleştirebilirsiniz. 

[Log Analytics portalı](../../azure-monitor/log-query/get-started-portal.md), Günlük Araması portalda mevcut olmayan gelişmiş işlevler sağlayan özel bir portaldır. Birden fazla satırda sorgu düzenleme, seçerek kod yürütme, bağlama duyarlı IntelliSense ve Akıllı Analiz özellikleri mevcuttur. Gelişmiş Analiz portalında grafik biçiminde bir performans görünümü oluşturabilir, gelecekteki bir arama için kaydedebilir ve daha önce oluşturulan ortak panoya sabitleyebilirsiniz.   

Gelişmiş Analiz portalını Günlük Araması portalındaki bir bağlantıdan başlatabilirsiniz.<br> ![Gelişmiş Analiz portalını başlatma](media/tutorial-logs-dashboards/log-analytics-advancedportal-01.png)

Analiz portalında hem Windows hem de Linux bilgisayarlar için Computer ve TimeGenerated ölçütüne göre gruplandırılmış ve görsel bir grafikte gösterilen işlemci kullanım kayıtlarını döndürmek için aşağıdaki sorguyu girin:

```
Perf | where CounterName == "% Processor Time" and ObjectName == "Processor" and InstanceName == "_Total" | summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 1m), Computer | render timechart
```

Sağ üst köşeden **Sorguyu kaydet** düğmesini seçerek sorguyu kaydedin.<br> ![Gelişmiş Analiz portalından sorgu kaydetme](media/tutorial-logs-dashboards/log-analytics-advancedportal-02.png)<br><br> **Sorguyu Kaydet** kontrol panelinde *Azure VM - İşlemci Kullanımı* gibi bir ad belirtip **Kaydet**’e tıklayın.  Bu şekilde, birlikte arama yapmak üzere ortak sorgu kitaplığı oluşturabilir veya tamamını yeniden yazmak zorunda kalmadan değiştirebilirsiniz.  Son olarak, sayfanın sağ orta bölümündeki **Grafiği Azure panosuna sabitle** düğmesini seçerek, bunu daha önce oluşturduğunuz paylaşılan panoya sabitleyin.  

Artık panoya sabitlenmiş bir sorgumuz olduğuna göre, genel bir başlığının ve altında bir yorumun olduğunu fark edeceksiniz.<br> ![Azure panosu örneği](media/tutorial-logs-dashboards/log-analytics-modify-dashboard-01.png)<br><br>  Bu panoya, görüntüleyenler tarafından kolayca anlaşılacak, anlamlı bir ad vermeliyiz.  Kutucuğa sağ tıklayın ve **Kutucuğu düzenle**’yi seçin.  Kutucuğun başlık ve alt başlığını özelleştirmeyi tamamladıktan sonra **Güncelleştir**’e tıklayın.  Değişiklikleri yayımlamanızı veya atmanızı isteyen bir başlık görüntülenir.  **Değişiklikleri yayımla**’ya tıklayın ve sonra **Kutucuğu Düzenle** kontrol bölmesini kapatın.  

![Örnek pano yapılandırması tamamlandı](media/tutorial-logs-dashboards/log-analytics-modify-dashboard-02.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure portalında bir pano oluşturmayı ve içine bir günlük araması eklemeyi öğrendiniz.  Günlük araması sonuçlarına göre uygulayabileceğiniz farklı yanıtlar hakkında bilgi almak için sonraki öğreticiye geçin.  

> [!div class="nextstepaction"]
> [Log Analytics Uyarılarıyla olaylara yanıt verme](tutorial-response.md)
