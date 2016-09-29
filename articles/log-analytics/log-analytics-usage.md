<properties
    pageTitle="Log Analytics'te veri kullanımını çözümleme | Microsoft Azure"
    description="OMS hizmetine gönderilen veri miktarını görüntülemek için Log Analytics'teki Kullanım sayfasını inceleyebilirsiniz."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/11/2016"
    ms.author="banders"/>


# Log Analytics'te veri kullanımını çözümleme

Operations Management Suite'te (OMS) bulunan Log Analytics, verileri toplar ve düzenli aralıklarla OMS hizmetine gönderir.  OMS hizmetine gönderilen veri miktarını görüntülemek için **Kullanım** sayfasını inceleyebilirsiniz. **Kullanım** sayfası, aynı zamanda çözümler tarafından günlük olarak ne kadar veri gönderildiğini ve sunucularınızın ne sıklıkta veri gönderdiğini de gösterir.

>[AZURE.NOTE] [OMS web sitesi](http://www.microsoft.com/oms) aracılığıyla oluşturulmuş bir ücretsiz hesabınız varsa OMS hizmetine günlük olarak en fazla 500 MB veri gönderebilirsiniz. Günlük sınıra ulaştıysanız veri çözümlemesi durur ve sonraki günün başlangıcında devam eder. Ayrıca OMS tarafından kabul edilmemiş ve işlenmemiş tüm verileri de yeniden göndermeniz gerekir.

OMS'deki **Genel Bakış** panosunda yer alan **Kullanım** kutucuğunu inceleyerek kullanımınızı görüntüleyebilirsiniz.

![kullanım kutucuğu](./media/log-analytics-usage/usage-tile.png)

Günlük kullanım sınırınızı aştıysanız veya sınırınıza yaklaştıysanız OMS hizmetine gönderdiğiniz veri miktarını azaltmak için isteğe bağlı olarak bir çözümü kaldırabilirsiniz. Çözümleri kaldırma hakkında daha fazla bilgi için bkz. [Çözüm Galerisinden Log Analytics çözümleri ekleme](log-analytics-add-solutions.md).

![kullanım panosu](./media/log-analytics-usage/usage-dashboard.png)

**Kullanım** sayfasında şu bilgiler görüntülenir:

- Gün başına ortalama kullanım
- Son 30 günde her çözüm için veri kullanımı
- Son 30 günde ortamınızdaki sunucuların OMS hizmetine ne kadar veri gönderdiği
- Veri planınızın fiyatlandırma katmanı ve tahmini maliyet
- OMS'nin verilerinizi işleme süresi de dahil olmak üzere hizmet düzeyi sözleşmeniz (SLA) hakkında bilgi

## Kullanım verileriyle çalışma

1. **Genel Bakış** sayfasında, **Kullanım** kutucuğuna tıklayın.
2. **Kullanım** sayfasında, ilgilendiğiniz alanları gösteren kullanım kategorilerini görüntüleyin.
3. Günlük karşıya yükleme kotanızı çok fazla tüketen bir çözümünüzün olması durumunda, bu çözümü kaldırmayı göz önünde bulundurabilirsiniz.

## Tahmini maliyetinizi ve fatura bilgilerinizi görüntüleme
1. **Genel Bakış** sayfasında, **Kullanım** kutucuğuna tıklayın.
2. **Kullanım** altındaki **Kullanım** sayfasında, **Tahmini maliyet**'in yanındaki köşeli çift ayraca (**>**) tıklayın.
3. Genişletilmiş **Veri planınız** ayrıntılarında tahmini aylık maliyetinizi görebilirsiniz.  
    ![Veri planınız](./media/log-analytics-usage/usage-data-plan.png)
4. Fatura bilgilerinizi görüntülemek istiyorsanız abonelik bilgilerinizi görüntülemek için **Faturamı görüntüle**'ye tıklayın.
    - Abonelikler sayfasında, ayrıntıları ve kalemlere ayrılmış kullanım listesini görüntülemek için aboneliğinize tıklayın.  
        ![aboneliği](./media/log-analytics-usage/usage-sub01.png)
    - Aboneliğinizin Özet sayfasında, aboneliğinize ilişkin daha fazla ayrıntıyı yönetmek ve görüntülemek için çeşitli görevler gerçekleştirebilirsiniz.  
        ![abonelik ayrıntıları](./media/log-analytics-usage/usage-sub02.png)

## SLA'nız için veri yığınlarını görüntüleme
1. **Genel Bakış** sayfasında, **Kullanım** kutucuğuna tıklayın.
2. **Hizmet Düzeyi Sözleşmesi** altında, **SLA ayrıntılarını indir**'e tıklayın.
3. İncelemeniz için bir Excel XLSX dosyası indirilir.  
    ![SLA ayrıntıları](./media/log-analytics-usage/usage-sla-details.png)

## Sonraki adımlar

- Çözümler tarafından toplanan ayrıntılı bilgileri görüntülemek için bkz. [Log Analytics'te günlük aramaları](log-analytics-log-searches.md).



<!--HONumber=Sep16_HO3-->


