---
title: Microsoft Flow ile Azure İzleyici günlük işlemlerini otomatik hale getirin
description: Hızlı bir şekilde Azure Log Analytics bağlayıcısını kullanarak tekrarlanabilir süreçlerini otomatikleştirmek için Microsoft Flow nasıl kullanabileceğinizi öğrenin.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
ms.service: log-analytics
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 09/29/2017
ms.author: bwren
ms.openlocfilehash: c3732dd2fa87b00eec38f88ab828605b33567235
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60396606"
---
# <a name="automate-azure-monitor-log-processes-with-the-connector-for-microsoft-flow"></a>Bağlayıcısı ile Azure İzleyici günlük işlemlerini otomatikleştirmek için Microsoft Flow
[Microsoft Flow](https://ms.flow.microsoft.com) çeşitli hizmetler için Eylemler yüzlerce kullanarak otomatik iş akışları oluşturmanıza olanak sağlar. Bir eylem çıktısını, başka farklı hizmetleri arasında tümleştirme oluşturmanızı sağlayan giriş olarak kullanılabilir.  Azure Log Analytics bağlayıcısını Microsoft Flow için Azure İzleyici'de bir Log Analytics çalışma alanından günlük sorguları tarafından alınan verileri içeren iş akışları oluşturmanıza olanak tanır.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

Örneğin, Microsoft Flow, Office 365'ten bir e-posta bildirimi LAzure izleme günlük verilerini kullanın, Azure DevOps bir hata oluşturun veya bir Slack iletisi göndermek için kullanabilirsiniz.  Basit bir zamanlama veya bağlı bir hizmetteki bir posta ya da bir tweet alındığında gibi bazı eylemleri bir iş akışı tetikleyebilirsiniz.  

Bu makalede öğreticide otomatik olarak e-posta, Log Analytics Bağlayıcısı'nı Microsoft Flow nasıl kullanabileceğinize bir örnek bir Azure İzleyici günlük sorgusu sonuçlarını gönderen bir akış oluşturma işlemini gösterir. 


## <a name="step-1-create-a-flow"></a>1. Adım: Akış oluşturun
1. Oturum [Microsoft Flow](https://flow.microsoft.com)seçip **Akışlarım**.
2. Tıklayın **+ boş akış Oluştur**.

## <a name="step-2-create-a-trigger-for-your-flow"></a>2. Adım: Akışınız için bir Tetikleyici oluşturma
1. Tıklayın **yüzlerce bağlayıcı ve tetikleyicide arama**.
2. Tür **zamanlama** arama kutusuna.
3. Seçin **zamanlama**ve ardından **zamanlama - yinelenme**.
4. İçinde **sıklığı** seçim kutusunu **gün** ve **aralığı** kutusuna **1**.<br><br>![Microsoft Flow tetikleyici iletişim kutusu](media/flow-tutorial/flow01.png)


## <a name="step-3-add-a-log-analytics-action"></a>3. Adım: Log Analytics eylemi ekleme
1. Tıklayın **+ yeni adım**ve ardından **Eylem Ekle**.
2. Arama **Log Analytics**.
3. Tıklayın **Azure Log Analytics – sorgu çalıştırmak ve sonuçlarını görselleştirme**.<br><br>![Log Analytics sorgu penceresinde çalıştırma](media/flow-tutorial/flow02.png)

## <a name="step-4-configure-the-log-analytics-action"></a>4. Adım: Log Analytics eylemi yapılandırın

1. Abonelik kimliği, kaynak grubu ve çalışma alanı adı da dahil olmak üzere bir çalışma alanınız için ayrıntıları belirtin.
2. Aşağıdaki günlük sorguya eklemek **sorgu** penceresi.  Bu yalnızca bir örnek sorgu ve veri döndüren diğer tüm değiştirebilirsiniz.
   ```
    Event
    | where EventLevelName == "Error" 
    | where TimeGenerated > ago(1day)
    | summarize count() by Computer
    | sort by Computer
   ```

2. Seçin **HTML tablosu** için **grafik türü**.<br><br>![Log Analytics eylemi](media/flow-tutorial/flow03.png)

## <a name="step-5-configure-the-flow-to-send-email"></a>5. Adım: E-posta gönderme akışı Yapılandır

1. Tıklayın **yeni adım**ve ardından **+ Eylem Ekle**.
2. Arama **Office 365 Outlook**.
3. Tıklayın **Office 365 Outlook-e-posta Gönder**.<br><br>![Office 365 Outlook seçim penceresi](media/flow-tutorial/flow04.png)

4. Bir alıcıya e-posta adresini belirtin **için** penceresi ve e-postada konu **konu**.
5. Herhangi bir yeri tıklatın **gövdesi** kutusu.  A **dinamik içerik** önceki Eylemlerdeki değerlerle penceresi açılır.  
6. Seçin **gövdesi**.  Log Analytics eylemi sorgu sonuçlarını budur.
6. Tıklayın **Gelişmiş Seçenekleri Göster**.
7. İçinde **HTML'dir** kutusunda **Evet**.<br><br>![Office 365 e-posta Yapılandırması penceresi](media/flow-tutorial/flow05.png)

## <a name="step-6-save-and-test-your-flow"></a>6. Adım: Akışınızı test edin ve Kaydet
1. İçinde **Akış adı** kutusuna akışınız için bir ad ekleyin ve ardından **akış oluşturma**.<br><br>![Akışı kaydedin](media/flow-tutorial/flow06.png)
2. Akış oluşturulur ve belirttiğiniz zamanlamaya olan bir gün çalışır. 
3. Hemen akışı test etmek için tıklayın **Şimdi Çalıştır** ardından **akış çalıştırması**.<br><br>![Akışı Çalıştır](media/flow-tutorial/flow07.png)
3. Akışı tamamlandığında e-posta, belirttiğiniz alıcının denetleyin.  Aşağıdakine benzer bir gövde içeren bir e-posta almış olmanız gerekir:<br><br>![Örnek e-posta](media/flow-tutorial/flow08.png)


## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [sorgular Azure İzleyici'de oturum](../log-query/log-query-overview.md).
- Daha fazla bilgi edinin [Microsoft Flow](https://ms.flow.microsoft.com).



