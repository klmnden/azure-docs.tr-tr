---
title: Azure maliyeti Yönetimi'nde bütçelerini yönetme | Microsoft Docs
description: Bu makalede oluşturma ve maliyet yönetim bütçelerini yönetmenize yardımcı olur.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 06/25/2018
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: 9d6bf29909393846ec17a1bcc210fb989efd7f99
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938502"
---
# <a name="manage-budgets"></a>Bütçelerini yönetme

Ayar bütçeleri yukarı ve bütçe tabanlı bulut idare ve Sorumluluk geliştirmek için Yardım uyarır. Bu makalede, hızlı bir şekilde bütçeleri oluşturma ve bunları maliyet Yönetimi başlatma yardımcı olur.

Bir kuruluş ya da MSP hesabınız varsa, farklı iş birimleri, Departmanlar veya herhangi bir maliyet varlık aylık bütçe kotaları atamak için hiyerarşik maliyet varlık yapınızı kullanabilirsiniz. Premium hesabı varsa, tüm bulut harcamalarını sonra uygulanan bütçe yönetim işlevini kullanabilirsiniz. Tüm bütçeleri el ile atanır.

Atanan bütçeleri temel, tüketilen bütçenizi yüzdesinin temel eşik Uyarıları ayarlayın ve her eşik önemini tanımlayın.

Bütçe raporlar atanan bütçe gösterir. Kullanıcılar kendi harcama üzerinden, altında veya nominal zamanla kullanıcıların kullanımına sahip olduğunda görüntüleyebilir. Seçtiğinizde, **Göster/Gizle alanları** Bütçe Raporu üst kısmında, maliyet, bütçe, birikmiş maliyet veya toplam bütçe görüntüleyebilirsiniz.

## <a name="create-budgets"></a>Bütçeleri oluşturma

Bütçe oluştururken, mali yılın ayarlayın ve belirli bir varlık için geçerlidir.

Bir bütçe oluşturmak ve bir varlığa atamak için:

1. Gidin **maliyetleri** &gt; **yönetim maliyeti** &gt; **bütçe**.
2. Bütçe Yönetimi sayfasında altında **varlıklar**, bütçe oluşturmak istediğiniz varlığı seçin.
3. Bütçe yılda bütçe oluşturmak istediğiniz yılı seçin.
4. Her ay için bir bütçe değerini ayarlayın. İşiniz bittiğinde tıklatın **kaydetmek**.
Bu örnekte, Haziran 2018 için aylık bütçe 135.000 için ayarlanır. Toplam bütçe yıl $1,615,000.00 olacaktır.
![Bir bütçe oluşturmak](./media/manage-budgets/set-budget.png)


Yıllık bütçe bir dosyayı içe aktarmak için:

1. Altında **Eylemler**seçin **dışarı** bütçe için temel olarak kullanmak için boş bir CSV şablonu indirilemedi.
2. Bütçe girişlerinizi içeren CSV dosyası doldurun ve yerel olarak kaydedin.
3. Altında **Eylemler**seçin **alma**.
4. Kaydedilen dosyanızı seçin ve ardından **Tamam**.

Tamamlanan bütçenizi altında bir CSV dosyası olarak dışarı aktarmak için **Eylemler**seçin **verme** dosyasını karşıdan yüklemek için.

## <a name="view-budget-in-reports"></a>Görünüm bütçe raporlarında

Tamamlandığında, bütçenizi çoğu maliyet raporları altında gösterilen **maliyetleri** &gt; **maliyet analizi** ve maliyet vs. Zaman içinde bütçe rapor. Raporları kullanarak bütçe eşiklere dayanarak de zamanlayabilirsiniz **Eylemler**.

Maliyet analiz raporu bir örneği burada verilmiştir. İş yükü ve kullanım türleri maliyeti ve toplam bütçe itibaren yılın başlangıcını gösterir.

![Bütçe ile örnek maliyet analizi raporu](./media/manage-budgets/cost-analysis-budget-example.png)

Bu örnekte, geçerli tarihe Haziran 22 olduğunu varsayalım. $ $135.000 aylık bütçeye karşılaştırıldığında 71,611.28 Haziran 2018 için maliyetidir. Ayın sonunu önce harcama, sekiz gün yine olduğundan maliyet aylık bütçe çok düşüktür.

Raporu görüntülemek için başka bir yolu, bütçenizi Birikmiş maliyet vs aramaktır. Birikmiş maliyetlerini altında görmek için **Göster/Gizle alanları**seçin **Birikmiş maliyet** ve **toplam bütçe**. Yılın başlangıcını itibaren birikmiş maliyetini gösteren bir örnek aşağıda verilmiştir.

![Birikmiş bütçe](./media/manage-budgets/accumulated-budget.png)

Süre gelecekte birikmiş maliyetinizi bütçenizi aşabilir. Grafik görünümü değiştirirseniz, daha kolay görebilirsiniz _satır_ türü.

![Çizgi grafikte gösterildiği bütçe](./media/manage-budgets/budget-line.png)

## <a name="create-budget-alerts-for-a-filter"></a>Bir filtre için bütçe uyarı oluşturma

Önceki örnekte Birikmiş maliyet bütçe yaklaşıldığında görebilirsiniz. Böylece yaklaşımlar harcama zaman haberdar veya bütçenizi aşıyor otomatik bütçe uyarılar oluşturabilirsiniz. Bir eşik ile uyarı temel olarak, zamanlanmış bir rapordur. Bütçe uyarı eşiği ölçümler şunlardır:

- Kalan Maliyet para birimi değeri eşiği belirtmek için Bütçe – karşılaştırması
- -Bir yüzde değeri eşiği belirtmek için bütçe – maliyet yüzdesi

Bir örneğe bakalım.

Maliyet vs. Bütçe zaman raporu, tıklatın **Eylemler** ve ardından **zamanlama rapor**. Eşik sekmesinde bir eşik ölçümü seçin. Örneğin, **maliyet yüzdesi vs bütçe**. Bir uyarı türü seçin ve bütçe yüzde değeri girin. Yalnızca bir kez bildirim istiyorsanız, seçin **ardışık uyarı sayısı** ve ardından _1_. **Kaydet**’e tıklayın.

![Bütçe Uyarısı](./media/manage-budgets/budget-alert.png)

## <a name="next-steps"></a>Sonraki adımlar

- İlk öğreticide maliyet yönetimi için zaten tamamlanmış yapmadıysanız, hem okuma [gözden kullanım ve maliyetleri](https://docs.microsoft.com/en-us/azure/cost-management/tutorial-review-usage).
- Daha fazla bilgi edinmek [maliyet Yönetimi'nde kullanılabilir raporlar](use-reports.md).
