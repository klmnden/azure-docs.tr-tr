---
title: "Oluşturmak ve paylaşmak Azure günlük analizi veri panolar | Microsoft Docs"
description: "Bu öğretici günlük analizi panolar tüm kaydedilmiş günlük işlemlerinizin nasıl görselleştirebilirsiniz ortamınızı görüntülemek için tek bir mercek vermiş anlamanıza yardımcı olur."
services: log-analytics
documentationcenter: log-analytics
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/14/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 272945134b534a5ded794379ce5e96b0902a4227
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2017
---
# <a name="create-and-share-dashboards-of-log-analytics-data"></a>Oluşturma ve panolar günlük analizi veri paylaşma

Panolar tüm kaydedilmiş günlük işlemlerinizin görselleştirebilirsiniz günlük analizi bulmak için olanağı veren ilişkilendirmek ve kuruluştaki BT işletimsel veri paylaşın.  Bu öğretici, BT işlemleri destek ekibiniz tarafından erişilecek bir paylaşılan Pano desteklemek için kullanılacak günlük arama oluşturulması ele alınmaktadır.  Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Azure portalında paylaşılan bir pano oluşturun
> * Bir performans günlük arama Görselleştirme 
> * Günlük arama paylaşılan bir Pano ekleyin 
> * Paylaşılan bir Panoda bir kutucuğu özelleştirme

Bu öğreticide örnek tamamlamak için var olan bir sanal makine olması [için günlük analizi çalışma alanına bağlı](log-analytics-quick-collect-azurevm.md).  
 
## <a name="log-in-to-azure-portal"></a>Azure portalında oturum açın
Oturum açtığınızda Azure portalında [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-a-shared-dashboard"></a>Paylaşılan bir pano oluşturun

Microsoft Azure portalında oturum açtıktan sonra gördüğünüz ilk şey bir [Pano](../azure-portal/azure-portal-dashboards.md).<br> ![Azure portalı panosunun](media/log-analytics-tutorial-dashboards/log-analytics-portal-dashboard.png)

Burada birlikte en önemli işletimsel veri getirebilirsiniz BT Azure günlük analizi telemetrisinden dahil olmak üzere tüm Azure kaynaklarınızı, üzerinden.  Günlük arama görselleştirme içine adım önce şimdi ilk olarak bir pano oluşturun ve paylaşın.  Bu size bir çizgi grafiği işlemek ve Pano ekleyin bizim örnek performans günlük arama, olabilmesi göz önünden almak için bize sağlar.  

Bir Pano oluşturmak için seçin **yeni Pano** geçerli Panodaki adının yanındaki düğmesi.<br> ![Azure portalında yeni bir pano oluşturun](media/log-analytics-tutorial-dashboards/log-analytics-create-dashboard-01.png)

Bu eylem, yeni, boş, özel bir Pano oluşturur ve burada panonuzu adlandırın ve ekleyebilir veya kutucukları yeniden özelleştirme moduna geçirir. Pano adını düzenleyin ve belirtin *örnek Pano* Bu öğretici ve ardından **özelleştirme Bitti**.<br><br> ![Özelleştirilmiş Azure panoyu kaydedin](media/log-analytics-tutorial-dashboards/log-analytics-create-dashboard-02.png)

Bir Pano oluşturduğunuzda, görebileceğiniz tek kişi olduğunuz anlamına gelir varsayılan olarak özeldir. Başkaları için görünür yapmak için kullanın **paylaşımı** yanı sıra diğer Pano komutları görünür düğmesi.<br> ![Azure portalında yeni bir pano paylaşımı](media/log-analytics-tutorial-dashboards/log-analytics-share-dashboard.png) 

Abonelik ve kaynak grubu için yayımlanması panonuz için seçim istenir. Kolaylık olması için portal, bir kaynak grubunda panolar nereye bir desen doğru olarak adlandırılan deneyimi kılavuzları yayımlama kullanıcının **panolar**.  Seçili olan abonelikte doğrulayın ve ardından **Yayımla**.  Panoda görüntülenen bilgilere erişimi ile denetlenir [Azure kaynak tabanlı erişim denetimi](../active-directory/role-based-access-control-configure.md).   

## <a name="visualize-a-log-search"></a>Günlük arama Görselleştirme

Azure portalında günlük arama portalından tek bir satırda temel sorgular oluşturabilirsiniz. Bir dış portal başlatmadan günlük arama portal kullanılabilir ve günlük aramalarında uyarı kuralları oluşturma, bilgisayar gruplarını oluşturma ve sorgunun sonuçlarını dışarı aktarma gibi çeşitli işlevleri gerçekleştirmek için kullanabilirsiniz. 

[Gelişmiş analizler portal](https://docs.loganalytics.io/docs/Learn/Getting-Started/Getting-started-with-the-Analytics-portal) günlük arama Portalı'nda kullanılamıyor gelişmiş işlevselliği sağlayan adanmış bir portalıdır. Özellikler sorguda birden çok satıra düzenleme, seçime bağlı olarak kod, içeriğe duyarlı IntelliSense ve akıllı analizi yürütmek yeteneğini içerir. Advanced Analytics Portalı'nda grafik formunda bir performans görünümü oluşturma, gelecekteki bir ara kaydedin ve daha önce oluşturduğunuz paylaşılan panoya Sabitle.   

Günlük arama portalındaki bir bağlantı Advanced Analytics portalından başlatın.<br> ![Advanced Analytics Portalı'nı başlatma](media/log-analytics-tutorial-dashboards/log-analytics-advancedportal-01.png)

Analytics Portalı'nda Windows ve Linux bilgisayarlar, bilgisayar ve TimeGenerated göre gruplandırılmış ve görsel bir grafikte görüntülenen kullanımı kayıtları yalnızca işlemci döndürmek için aşağıdaki sorguyu girin:

```
Perf | where CounterName == "% Processor Time" and ObjectName == "Processor" and InstanceName == "_Total" | summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 1m), Computer | render timechart
```

Seçerek sorguyu kaydetmek **Kaydet sorgu** sağ üst köşesinde düğmesinden.<br> ![Sorgu Advanced Analytics portalından Kaydet](media/log-analytics-tutorial-dashboards/log-analytics-advancedportal-02.png)<br><br> İçinde **Sorguyu Kaydet** Denetim Masası, bir ad sağlayın *Azure Vm'leri - işlemci kullanımı* ve ardından **kaydetmek**.  Bu şekilde ile arama veya tamamen yeniden yazmak zorunda kalmadan değiştirmek için ortak sorguları kitaplığını oluşturabilirsiniz.  Son olarak, bu seçerek daha önce oluşturduğunuz paylaşılan panoya Sabitle **PIN grafiği Azure panonuza** sayfasının Orta-sağ köşesinde düğmesinden.  

Biz panosuna sabitlediğiniz bir sorgu sahip olduğunuza göre bir genel başlığı ve açıklamayı altındaki olduğundan fark edeceksiniz.<br> ![Azure Pano örnek](media/log-analytics-tutorial-dashboards/log-analytics-modify-dashboard-01.png)<br><br>  Biz bunu anlamlı, kolayca görüntülemesini bu tarafından anlaşılabilir için yeniden adlandırmanız gerekir.  Seçin ve döşeme sağ **düzenleme döşeme**.  Başlık ve alt başlık döşemeye özelleştirme tamamladıktan sonra tıklayın **güncelleştirme**.  Değişiklikleri yayımladığınızda veya atmak isteyen bir başlığı görüntülenir.  ' I tıklatın **yayımlama değişiklikleri** ve kapatın **Düzenle kutucuğu** denetim bölmesi.  

![Örnek Pano yapılandırması tamamlandı](media/log-analytics-tutorial-dashboards/log-analytics-modify-dashboard-02.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure portalında bir pano oluşturun ve günlük arama ekleyin öğrendiniz.  Gelişmiş, uygulayabileceğiniz farklı yanıtlar öğrenmek için sonraki öğretici için günlük arama sonuçlarına dayanır.  

> [!div class="nextstepaction"]
> [Günlük analizi uyarılarla olaylara yanıt](log-analytics-tutorial-response.md)