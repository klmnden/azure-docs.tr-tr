---
title: Microsoft Flow ile Azure Log Analytics işlemlerini otomatik hale getirin
description: Hızlı bir şekilde Azure Log Analytics bağlayıcısını kullanarak tekrarlanabilir süreçlerini otomatikleştirmek için Microsoft Flow nasıl kullanabileceğinizi öğrenin.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
ms.service: log-analytics
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/29/2017
ms.author: bwren
ms.component: na
ms.openlocfilehash: d623cd4cb1a62fab7b5f8cc4e9686d88cde94ed8
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44303278"
---
# <a name="automate-log-analytics-processes-with-the-connector-for-microsoft-flow"></a>Bağlayıcı ile log Analytics işlemlerini otomatikleştirmek için Microsoft Flow
[Microsoft Flow](https://ms.flow.microsoft.com) çeşitli hizmetler için Eylemler yüzlerce kullanarak otomatik iş akışları oluşturmanıza olanak sağlar. Bir eylem çıktısını, başka farklı hizmetleri arasında tümleştirme oluşturmanızı sağlayan giriş olarak kullanılabilir.  Azure Log Analytics bağlayıcısını Microsoft Flow için Log analytics'te günlük aramaları tarafından alınan verileri içeren iş akışları oluşturmanıza olanak tanır.

Örneğin, Microsoft Flow, Office 365'ten bir e-posta bildirimi Log Analytics verilerini kullanın, Azure DevOps bir hata oluşturun veya bir Slack iletisi göndermek için kullanabilirsiniz.  Basit bir zamanlama veya bağlı bir hizmetteki bir posta ya da bir tweet alındığında gibi bazı eylemleri bir iş akışı tetikleyebilirsiniz.  

Bu makalede öğreticide otomatik olarak e-posta, Log Analytics Microsoft Flow nasıl kullanabileceğinize bir örnek Log Analytics günlük araması sonuçlarının gönderen bir akış oluşturma işlemini gösterir. 


## <a name="step-1-create-a-flow"></a>1. adım: bir akış oluşturma
1. Oturum [Microsoft Flow](http://flow.microsoft.com)seçip **Akışlarım**.
2. Tıklayın **+ boş akış Oluştur**.

## <a name="step-2-create-a-trigger-for-your-flow"></a>2. adım: akışınız için bir Tetikleyici oluşturma
1. Tıklayın **yüzlerce bağlayıcı ve tetikleyicide arama**.
2. Tür **zamanlama** arama kutusuna.
3. Seçin **zamanlama**ve ardından **zamanlama - yinelenme**.
4. İçinde **sıklığı** seçim kutusunu **gün** ve **aralığı** kutusuna **1**.<br><br>![Microsoft Flow tetikleyici iletişim kutusu](media/log-analytics-flow-tutorial/flow01.png)


## <a name="step-3-add-a-log-analytics-action"></a>3. adım: bir Log Analytics eylemi ekleme
1. Tıklayın **+ yeni adım**ve ardından **Eylem Ekle**.
2. Arama **Log Analytics**.
3. Tıklayın **Azure Log Analytics – sorgu çalıştırmak ve sonuçlarını görselleştirme**.<br><br>![Log Analytics sorgu penceresinde çalıştırma](media/log-analytics-flow-tutorial/flow02.png)

## <a name="step-4-configure-the-log-analytics-action"></a>4. adım: Log Analytics eylemi yapılandırın

1. Abonelik kimliği, kaynak grubu ve çalışma alanı adı da dahil olmak üzere bir çalışma alanınız için ayrıntıları belirtin.
2. Aşağıdaki Log Analytics sorguya eklemek **sorgu** penceresi.  Bu yalnızca bir örnek sorgu ve veri döndüren diğer tüm değiştirebilirsiniz.
```
    Event
    | where EventLevelName == "Error" 
    | where TimeGenerated > ago(1day)
    | summarize count() by Computer
    | sort by Computer
```

2. Seçin **HTML tablosu** için **grafik türü**.<br><br>![Log Analytics eylemi](media/log-analytics-flow-tutorial/flow03.png)

## <a name="step-5-configure-the-flow-to-send-email"></a>5. adım: e-posta gönderme akışı Yapılandır

1. Tıklayın **yeni adım**ve ardından **+ Eylem Ekle**.
2. Arama **Office 365 Outlook**.
3. Tıklayın **Office 365 Outlook-e-posta Gönder**.<br><br>![Office 365 Outlook seçim penceresi](media/log-analytics-flow-tutorial/flow04.png)

4. Bir alıcıya e-posta adresini belirtin **için** penceresi ve e-postada konu **konu**.
5. Herhangi bir yeri tıklatın **gövdesi** kutusu.  A **dinamik içerik** önceki Eylemlerdeki değerlerle penceresi açılır.  
6. Seçin **gövdesi**.  Log Analytics eylemi sorgu sonuçlarını budur.
6. Tıklayın **Gelişmiş Seçenekleri Göster**.
7. İçinde **HTML'dir** kutusunda **Evet**.<br><br>![Office 365 e-posta Yapılandırması penceresi](media/log-analytics-flow-tutorial/flow05.png)

## <a name="step-6-save-and-test-your-flow"></a>6. adım: Kaydedin ve akışınızı test edin
1. İçinde **Akış adı** kutusuna akışınız için bir ad ekleyin ve ardından **akış oluşturma**.<br><br>![Akışı kaydedin](media/log-analytics-flow-tutorial/flow06.png)
2. Akış oluşturulur ve belirttiğiniz zamanlamaya olan bir gün çalışır. 
3. Hemen akışı test etmek için tıklayın **Şimdi Çalıştır** ardından **akış çalıştırması**.<br><br>![Akışı Çalıştır](media/log-analytics-flow-tutorial/flow07.png)
3. Akışı tamamlandığında e-posta, belirttiğiniz alıcının denetleyin.  Aşağıdakine benzer bir gövde içeren bir e-posta almış olmanız gerekir:<br><br>![Örnek e-posta](media/log-analytics-flow-tutorial/flow08.png)


## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [Log Analytics'te günlük aramaları](log-analytics-log-search-new.md).
- Daha fazla bilgi edinin [Microsoft Flow](https://ms.flow.microsoft.com).



