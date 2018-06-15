---
title: Dosya Bütünlüğü (Önizleme) Azure Güvenlik Merkezi'nde izleme | Microsoft Docs
description: " Dosya bütünlüğü izleme Azure Güvenlik Merkezi'nde etkinleştirmeyi öğrenin. "
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/08/2018
ms.author: terrylan
ms.openlocfilehash: 722a4fd11f35f04ed22d73638f07d15c49ea3c26
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34162179"
---
# <a name="file-integrity-monitoring-in-azure-security-center-preview"></a>Dosya Bütünlüğü (Önizleme) Azure Güvenlik Merkezi'nde izleme
Dosya bütünlüğü izleme (FIM) Azure Güvenlik Merkezi'nde bu kılavuzu kullanarak yapılandırmayı öğrenin.

## <a name="what-is-fim-in-security-center"></a>FIM Güvenlik Merkezi'nde nedir?
Dosya bütünlüğü izleme (değişiklik izleme olarak da bilinen FIM), dosya ve kayıt defterleri, işletim sistemi, uygulama yazılımı ve diğer saldırının gösterebilir değişiklikleri inceler. Bir karşılaştırma yöntemi, dosyanın geçerli durumunu dosyayı son tarama dışında farklı olup olmadığını belirlemek için kullanılır. Geçerli veya şüpheli değişiklikler dosyalarınıza yapılıp yapılmadığını belirlemek için bu karşılaştırma yararlanabilirsiniz.

Güvenlik Merkezi'nin dosya bütünlüğü izleme Windows dosyaları, Windows kayıt defteri ve Linux dosyaları bütünlüğünü doğrular. FIM etkinleştirerek izlenen istediğiniz dosyaları seçin. Güvenlik Merkezi ile FIM etkinliği gibi etkin dosyaları izler:

- Dosya ve kayıt defteri oluşturma ve kaldırma
- Dosya değişiklikleri (dosya boyutu, erişim denetim listeleri ve içeriğin karmasını değişiklikler)
- Kayıt defteri değişiklikleri (boyutu, erişim conrol listeleri, türü ve içerik değişiklikler)

Güvenlik Merkezi üzerinde FIM kolayca etkinleştirebilirsiniz izlemek için varlıklar önerir. Ayrıca, kendi FIM ilkeleri veya izlemek için varlıklar tanımlayabilirsiniz. Bu kılavuz size nasıl gösterir.

> [!NOTE]
> Dosya bütünlüğü izleme (FIM) özelliği, Windows ve Linux bilgisayarları ve VM'ler için çalışır ve Güvenlik Merkezi'nin standart katmanında mevcuttur. Güvenlik Merkezi’nin fiyatlandırma katmanları hakkında daha fazla bilgi almak için bkz. [Fiyatlandırma](security-center-pricing.md).
FIM veri günlük analizi çalışma alanına yükler. Karşıya yüklediğiniz veri miktarına göre veri ücretleri uygulanır. Bkz: [günlük analizi fiyatlandırma](https://azure.microsoft.com/pricing/details/log-analytics/) daha fazla bilgi için.
>
>

> [!NOTE]
> FIM Azure değişiklik izleme çözümü izlemek ve ortamınızdaki değişiklikleri belirlemek için kullanır. Dosya bütünlüğü izleme etkin olduğunda, elinizde bir **değişiklik izleme** çözüm türünde kaynak. Kaldırırsanız **değişiklik izleme** kaynak, devre dışı bırakırsanız özelliği Güvenlik Merkezi'ndeki izleme dosyası bütünlüğü.
>
>

## <a name="which-files-should-i-monitor"></a>Hangi dosyaların ı izlemeniz gerekir?
İzlemek için hangi dosyaların seçerken sistemi ve uygulamalar için kritik dosyaları hakkında düşünmelisiniz. Planlama olmadan değiştirmek için beklemeyen dosyaları seçerek göz önünde bulundurun. Uygulamalar veya işletim sistemi (örneğin, günlük dosyaları ve metin dosyaları) tarafından sık değiştirilen seçme dosyaları çok fazla saldırı belirlemek zor hale gürültü oluşturun.

Dosyaları Güvenlik Merkezi önerir, dosya ve kayıt defteri değişiklikleri içerir bilinen saldırı desenleri göre varsayılan olarak izlemeniz gerekir.

## <a name="using-file-integrity-monitoring"></a>Kullanarak dosya bütünlüğü izleme
1. **Güvenlik Merkezi** panosunu açın.
2. Sol bölmede altında **Gelişmiş bulut savunma**seçin **dosya bütünlüğü izleme**.
![Güvenlik Merkezi Panosu][1]

**Bütünlük izleme dosya** açar.
  ![Güvenlik Merkezi Panosu][2]

Aşağıdaki bilgiler için her çalışma sağlanır:

- Geçen hafta gerçekleşen değişikliklerin sayısı (bir tire görebilirsiniz "-" FIM çalışma alanında etkin değilse)
- Bilgisayarlar ve sanal makineleri için çalışma alanına raporlama toplam sayısı
- Çalışma alanının coğrafi konum
- Çalışma alanı altında azure aboneliğinizin

Aşağıdaki düğmeler aynı zamanda bir çalışma alanı gösterilebilir:

- ![Simge etkinleştir][3] FIM çalışma alanı için etkin olmadığını gösterir. Çalışma alanını seçerek, çalışma alanı altındaki tüm makinelerde FIM olanak tanır.
- ![Yükseltme planı simgesi][4] çalışma alanında ya da abonelik altında Güvenlik Merkezi'nin standart katmanı çalışıp çalışmadığını belirtir. FIM özelliğini kullanmak için aboneliğinizi standart çalıştırması gerekir.  Çalışma alanını seçerek standart olarak yükseltmenizi sağlar. Standart katmanı ve nasıl yükseltileceği hakkında daha fazla bilgi için bkz: [yükseltme Güvenlik Merkezi'nin Gelişmiş güvenlik için standart katmana](security-center-pricing.md).
- (Hiçbir düğmesi yoktur) boş FIM, çalışma alanı zaten etkin anlamına gelir.

Altında **dosya bütünlüğü izleme**, bu çalışma alanı için FIM etkinleştirmek için bu çalışma alanı için dosya bütünlüğü izleme panoyu görüntülemek için bir çalışma alanı seçin veya [yükseltme](security-center-pricing.md) standart çalışma alanına.

## <a name="enable-fim"></a>FIM etkinleştir
Bir çalışma alanında FIM etkinleştirmek için:

1. Altında **dosya bütünlüğü izleme**, bir çalışma alanıyla seçin **etkinleştirmek** düğmesi.
2. **Dosya bütünlüğü izlemeyi etkinleştir** çalışma alanı altında Windows ve Linux makineler sayısını görüntüleyen açılır.

   ![Dosya bütünlüğü izlemeyi etkinleştir][5]

   Windows ve Linux için önerilen ayarları da listelenir.  Genişletme **Windows dosyalarını**, **kayıt defteri**, ve **Linux dosyaları** önerilen öğelerin tam listesini görmek için.

3. FIM için uygulamak istiyor musunuz herhangi bir önerilen varlık seçeneğinin işaretini kaldırın.
4. Seçin **dosya bütünlüğü izleme uygulamak** FIM etkinleştirmek için.

> [!NOTE]
> Herhangi bir zamanda ayarlarını değiştirebilirsiniz. Bkz: [düzenleme izlenen varlık](security-center-file-integrity-monitoring.md#edit-monitored-items) daha fazla bilgi edinmek için aşağıdaki.
>
>

## <a name="view-the-fim-dashboard"></a>FIM Pano görünümü
**Dosya bütünlüğü izleme** Pano FIM etkin olduğu çalışma alanları için görüntüler. FIM bir çalışma alanı veya çalışma seçtiğinizde etkinleştirdikten sonra FIM panosu açılır **dosya bütünlüğü izleme** etkin FIM zaten penceresi.

![Dosya bütünlüğü izleme Panosu][6]

Bir çalışma alanı FIM Pano şunları görüntüler:

- Çalışma Alanı'na bağlı makineler toplam sayısı
- Seçilen dönem boyunca gerçekleşen değişikliklerin sayısı
- Değişiklik türü (dosyaları, kayıt defteri) dökümünü
- Değişiklik kategorisi dökümünü (değiştiren, eklenmiş, kaldırılmış)

Pano üstündeki filtre seçmek için değişiklikleri görmek istediğiniz süreyi uygulamak olanak sağlar.

![Zaman dönem filtresi][7]

**Bilgisayarlar** (yukarıda gösterilen) sekmesi tüm makineler bu çalışma alanına raporlama listeler. Her makine için panoyu listeler:

- Seçilen süre boyunca gerçekleşen toplam değişikliklerin
- Dosya değişiklikleri veya kayıt defteri değişikliklerini toplam değiştikçe dökümünü

**Arama oturum** aramada bir makine adı girdiğinizde açılır alan veya bilgisayarlar sekmesi altında listelenen bir makine seçin. Günlük arama makine için seçilen zaman aralığı sırasında yapılan tüm değişiklikleri görüntüler. Daha fazla bilgi için bir değişiklik genişletebilirsiniz.

![Günlük araması][8]

**Değişiklikleri** (aşağıda gösterilen) sekmesi seçili zaman diliminde çalışma alanı için tüm değişiklikleri listeler. Pano listeleri değiştirildi her bir varlık için:

- Üzerinde değişikliğinin bilgisayar
- Tür değişiklik (kayıt defteri veya dosya)
- Değişiklik kategorisi (değiştiren, eklenmiş, kaldırılmış)
- Tarih ve saatini değiştir

![Çalışma alanı için değişiklikler][9]

**Ayrıntılar değiştirmek** aramada bir değişiklik girdiğinizde açılır altında listelenen bir varlık seçmeniz veya alanı **değişiklikleri** sekmesi.

![Değişiklik ayrıntıları][10]

## <a name="edit-monitored-entities"></a>İzlenen düzenleme varlıklar

1. Geri dönüp **dosya bütünlüğü izleme Panosu** seçip **ayarları**.

  ![Ayarlar][11]

  **Çalışma alanı yapılandırmasını** üç sekme görüntüleyen açılır: **Windows kayıt defteri**, **Windows dosyalarını**, ve **Linux dosyaları**. Her sekme, bu kategorideki düzenleyebilirsiniz varlıklar listeler. FIM olup olmadığını listelenen her bir varlık için Güvenlik Merkezi tanımlar (true) etkin veya etkin değil (false).  Varlık düzenleme, etkinleştirme veya devre dışı FIM olanak tanır.

  ![Çalışma alanı yapılandırması][12]

2. Bir identityprotection seçin. Bu örnekte, Windows kayıt defteri altında bir öğe seçilmedi. **Değişiklik izleme için düzenleme** açar.

  ![Düzenleme veya değişiklik izleme][13]

Altında **değişiklik izleme için düzenleme** şunları yapabilirsiniz:

- Etkinleştirme (True) veya (False) dosya bütünlüğü izleme devre dışı
- Sağlayın veya varlık adını değiştirme
- Sağlayın veya değer veya yolunu değiştirme
- Varlığı silmek, değişikliği atmak veya değişiklik kaydetme

## <a name="add-a-new-entity-to-monitor"></a>İzlemek için yeni bir varlık ekleme
1. Dönün **Pano izleme dosyası integirty** seçip **ayarları** üstünde. **Çalışma alanı yapılandırmasını** açar.
2. Altında **çalışma alanı yapılandırmasını**, eklemek istediğiniz varlık türünü sekmesini seçin: Windows kayıt defteri, Windows dosyaları ya da Linux dosyaları. Bu örnekte, seçtik **Linux dosyaları**.

  ![İzlemek için Yeni Öğe Ekle][14]

3. **Add (Ekle)** seçeneğini belirleyin. **Değişiklik izleme için ekleme** açar.

  ![İstenen bilgileri girin][15]

4. Üzerinde **Ekle** sayfasında istenen bilgileri girin ve seçin **kaydetmek**.

## <a name="disable-monitored-entities"></a>İzlenen devre dışı bırak varlıkların
1. Geri dönüp **dosya bütünlüğü izleme** Pano.
2. FIM şu anda etkin olduğu bir çalışma alanı seçin. Bir çalışma alanı etkinleştir veya yükseltme planlama düğmesini eksik FIM için etkinleştirilir.

  ![FIM etkinleştirdiğiniz bir çalışma alanı seçin][16]

3. Dosya bütünlüğü izleme altında seçin **ayarları**.

  ![ayarlarını seçin][17]

4. Altında **çalışma alanı yapılandırmasını**, bir grup seçin nerede **etkin** ayarlanmış true.

  ![Çalışma Alanı Yapılandırması][18]

5. Altında **değişiklik izleme için düzenleme** penceresi kümesi **etkin** false.

  ![False kümesi etkin][19]

6. **Kaydet**’i seçin.

## <a name="disable-fim"></a>FIM devre dışı bırak
FIM devre dışı bırakabilirsiniz. FIM Azure değişiklik izleme çözümü izlemek ve ortamınızdaki değişiklikleri belirlemek için kullanır. FIM devre dışı bırakarak, değişiklik izleme çözümü seçilen çalışma alanından kaldırın.

1. FIM devre dışı bırakmak için dönmek **dosya bütünlüğü izleme** Pano.
2. Bir çalışma alanı seçin.
3. Altında **dosya bütünlüğü izleme**seçin **devre dışı**.

  ![FIM devre dışı bırak][20]

4. Seçin **kaldırmak** devre dışı bırakmak için.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede Güvenlik Merkezi'nde dosya bütünlüğü izleme (FIM) kullanmak öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Güvenlik ilkelerini ayarlama](security-center-policies.md) --Azure Abonelikleriniz ve kaynak grupları için güvenlik ilkeleri yapılandırmayı öğrenin.
* [Güvenlik önerilerini yönetme](security-center-recommendations.md) --nasıl önerilerin Azure kaynaklarınızı korumanıza yardımcı öğrenin.
* [Güvenlik durumunu izleme](security-center-monitoring.md)--Azure kaynaklarınızı sağlığını izlemek öğrenin.
* [Yönetme ve güvenlik uyarılarını yanıt](security-center-managing-and-responding-alerts.md)--yönetme ve güvenlik uyarılarını yanıtlama hakkında bilgi edinin.
* [İş ortağı çözümlerini izleme](security-center-partner-solutions.md) --iş ortağı çözümlerinizin sistem durumunu öğrenin.
* [Güvenlik Merkezi ile ilgili SSS](security-center-faq.md)--hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) - En son Azure güvenlik haberlerini ve bilgilerini edinin.

<!--Image references-->
[1]: ./media/security-center-file-integrity-monitoring/security-center-dashboard.png
[2]: ./media/security-center-file-integrity-monitoring/file-integrity-monitoring.png
[3]: ./media/security-center-file-integrity-monitoring/enable.png
[4]: ./media/security-center-file-integrity-monitoring/upgrade-plan.png
[5]: ./media/security-center-file-integrity-monitoring/enable-fim.png
[6]: ./media/security-center-file-integrity-monitoring/fim-dashboard.png
[7]: ./media/security-center-file-integrity-monitoring/filter.png
[8]: ./media/security-center-file-integrity-monitoring/log-search.png
[9]: ./media/security-center-file-integrity-monitoring/changes-tab.png
[10]: ./media/security-center-file-integrity-monitoring/change-details.png
[11]: ./media/security-center-file-integrity-monitoring/fim-dashboard-settings.png
[12]: ./media/security-center-file-integrity-monitoring/workspace-config.png
[13]: ./media/security-center-file-integrity-monitoring/edit.png
[14]: ./media/security-center-file-integrity-monitoring/add.png
[15]: ./media/security-center-file-integrity-monitoring/add-item.png
[16]: ./media/security-center-file-integrity-monitoring/fim-dashboard-disable.png
[17]: ./media/security-center-file-integrity-monitoring/fim-dashboard-settings-disabled.png
[18]: ./media/security-center-file-integrity-monitoring/workspace-config-disable.png
[19]: ./media/security-center-file-integrity-monitoring/edit-disable.png
[20]: ./media/security-center-file-integrity-monitoring/disable-fim.png
