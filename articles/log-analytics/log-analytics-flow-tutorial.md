---
title: Microsoft Flow ile Azure günlük analizi işlemlerini otomatik hale getirme
description: Hızlı bir şekilde Azure günlük analizi Bağlayıcısı'nı kullanarak yinelenebilir süreçlerini otomatikleştirmek için Microsoft Flow nasıl kullanabileceğinizi öğrenin.
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
ms.openlocfilehash: 21cf7cf3d12902b02fcbf650a1623e78004d28b4
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37131709"
---
# <a name="automate-log-analytics-processes-with-the-connector-for-microsoft-flow"></a>İçin Microsoft Flow bağlayıcı ile günlük analizi işlemlerini otomatikleştirmeyi
[Microsoft Flow](https://ms.flow.microsoft.com) Eylemler yüzlerce çok çeşitli hizmetlerini kullanarak otomatik iş akışları oluşturmanıza olanak sağlar. Bir eylem çıktısını başka farklı hizmetleri arasında tümleştirme oluşturmanızı sağlayan giriş olarak kullanılabilir.  Azure günlük analizi bağlayıcı Microsoft Flow için günlük analizi günlük aramalarda tarafından alınan veri içeren iş akışlarını oluşturmak olanak tanır.

Örneğin, Office 365'ten bir e-posta bildiriminin günlük analizi veri kullanın, Visual Studio Team Services içinde oluşturma veya Slack ileti göndermek için Microsoft Flow kullanabilirsiniz.  Bir iş akışı, bir basit zamanlama veya bağlantılı hizmetindeki bir posta ya da bir tweet alındığında gibi bazı eylemleri tetikleyebilirsiniz.  

Bu makalede öğreticide otomatik olarak e-posta, günlük analizi Microsoft Flow nasıl kullanabileceğiniz yalnızca bir örneği tarafından günlük analizi günlük arama sonuçlarını gönderen bir akış oluşturulacağını gösterir. 


## <a name="step-1-create-a-flow"></a>1. adım: bir akışı oluşturma
1. Oturum [Microsoft Flow](http://flow.microsoft.com)seçip **My akar**.
2. Tıklatın **+ Oluştur boş**.

## <a name="step-2-create-a-trigger-for-your-flow"></a>2. adım: akışınız için bir Tetikleyici oluşturma
1. Tıklatın **arama bağlayıcılar ve Tetikleyicileri yüzlerce**.
2. Tür **zamanlama** arama kutusuna.
3. Seçin **zamanlama**ve ardından **çizelgesi - yinelenme**.
4. İçinde **sıklığı** kutusunda seçin **gün** ve **aralığı** kutusuna **1**.<br><br>![Microsoft Flow tetikleyici iletişim kutusu](media/log-analytics-flow-tutorial/flow01.png)


## <a name="step-3-add-a-log-analytics-action"></a>3. adım: bir günlük analizi eylem ekleme
1. Tıklatın **+ yeni adım**ve ardından **Eylem Ekle**.
2. Arama **oturum Analytics**.
3. Tıklatın **Azure günlük analizi – sorgu çalıştırmak ve sonuçlarını görselleştirme**.<br><br>![Günlük analizi sorgu penceresi](media/log-analytics-flow-tutorial/flow02.png)

## <a name="step-4-configure-the-log-analytics-action"></a>4. adım: günlük analizi eylemi yapılandırın

1. Abonelik kimliği, kaynak grubu ve çalışma alanı adı dahil olmak üzere çalışma alanınız için ayrıntıları belirtin.
2. Aşağıdaki günlük analizi sorguya eklemek **sorgu** penceresi.  Bu yalnızca bir örnek sorgu ve veri veren diğer tüm değiştirin.
```
    Event
    | where EventLevelName == "Error" 
    | where TimeGenerated > ago(1day)
    | summarize count() by Computer
    | sort by Computer
```

2. Seçin **HTML tablosu** için **grafik türü**.<br><br>![Günlük analizi eylemi](media/log-analytics-flow-tutorial/flow03.png)

## <a name="step-5-configure-the-flow-to-send-email"></a>5. adım: e-posta göndermek için akışı Yapılandır

1. Tıklatın **yeni adım**ve ardından **+ Eylem Ekle**.
2. Arama **Office 365 Outlook**.
3. Tıklatın **Office 365 Outlook – bir e-posta Gönder**.<br><br>![Office 365 Outlook seçim penceresi](media/log-analytics-flow-tutorial/flow04.png)

4. Bir alıcı e-posta adresini belirtin **için** penceresi ve e-postayla için bir konu **konu**.
5. Herhangi bir yeri tıklatın **gövde** kutusu.  A **dinamik içerik** önceki Eylemler değerlerle penceresi açılır.  
6. Seçin **gövde**.  Günlük analizi uygulamada sorgunun sonuçlarını budur.
6. Tıklatın **Gelişmiş Seçenekleri Göster**.
7. İçinde **HTML'dir** kutusunda **Evet**.<br><br>![Office 365 e-posta yapılandırma penceresi](media/log-analytics-flow-tutorial/flow05.png)

## <a name="step-6-save-and-test-your-flow"></a>6. adım: Kaydetmek ve akışınız test
1. İçinde **Akış adı** kutusuna akışınız için bir ad ekleyin ve ardından **akışı oluşturmak**.<br><br>![Akış Kaydet](media/log-analytics-flow-tutorial/flow06.png)
2. Akış şimdi oluşturulur ve belirttiğiniz zamanlaması olan bir gün sonra çalıştırılır. 
3. Akış hemen sınamak için **Şimdi Çalıştır** ve ardından **akışı çalıştırmak**.<br><br>![Akış çalıştırın](media/log-analytics-flow-tutorial/flow07.png)
3. Belirtilen alıcı posta akışını tamamlandığında denetleyin.  Aşağıdakine benzer bir gövde içeren bir posta almış olmanız gerekir:<br><br>![Örnek e-posta](media/log-analytics-flow-tutorial/flow08.png)


## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinmek [günlük analizi aramaları oturum](log-analytics-log-search-new.md).
- Daha fazla bilgi edinmek [Microsoft Flow](https://ms.flow.microsoft.com).



