---
title: Azure tabanlı uzaktan izleme çözümündeki cihaz sorunlarını algılama | Microsoft Docs
description: Bu öğreticide Uzaktan İzleme çözümündeki eşik değer tabanlı cihaz sorunlarını otomatik olarak algılama amacıyla kuralların ve eylemlerin nasıl kullanılacağı gösterilmektedir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 06/08/2018
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 1e3eaeec1d2eae3c36f285a3e4c536657504cbb8
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37098490"
---
# <a name="tutorial-detect-issues-with-devices-connected-to-your-monitoring-solution"></a>Öğretici: İzleme çözümünüze bağlı cihazlarla sorunları algılama

Bu öğreticide bağlı IoT cihazlarınızla ilgili sorunları algılamak için Uzaktan İzleme çözümü hızlandırıcısını yapılandıracaksınız. Cihazlarınızla ilgili sorunları algılamak için çözüm panosunda uyarı oluşturan kurallar ekleyeceksiniz.

Öğreticide kurallar ve uyarılar için sanal bir soğutucu cihazı kullanılmaktadır. Soğutucu, Contoso adlı bir kuruluş tarafından yönetilmektedir ve Uzaktan İzleme çözümü hızlandırıcısına bağlıdır. Contoso, soğutucu içindeki basınç 298 PSI değerinin üzerine çıktığında kritik uyarı oluşturan bir kurala sahiptir. Contoso'da çalışan bir operatör olarak ilk basınç artışlarına bakarak sensörleri sorunlu olabilecek soğutucu cihazlarını belirlemek istiyorsunuz. Bu cihazları tanımlamak için soğutucu içindeki basınç 150 PSI seviyesinin üzerine çıktığında uyarı oluşturan bir kural ekleyeceksiniz.

Aynı zamanda son beş dakika içinde cihaz içindeki ortalama nem oranı %80'den fazla olan ve cihaz sıcaklığı 75 Fahrenhayt seviyesinin üzerine çıkan soğutucular için kritik uyarı oluşturulması yönünde bir istek aldınız.

Bu öğreticide şunları yaptınız:

>[!div class="checklist"]
> * Çözümünüzdeki kuralları görüntüleme
> * Kural oluşturma
> * Birden fazla koşula sahip kural oluşturma
> * Var olan kuralı düzenleme
> * Kuralları açma ve kapatma

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi takip etmek için Azure aboneliğinizde Uzaktan İzleme çözümü hızlandırıcısının dağıtılmış örneğine sahip olmanız gerekir.

Uzaktan İzleme çözümü hızlandırıcısını henüz dağıtmadıysanız [Bulut tabanlı uzaktan izleme çözümü dağıtma](quickstart-remote-monitoring-deploy.md) hızlı başlangıcını tamamlamanız gerekir.

## <a name="view-the-existing-rules"></a>Var olan kuralları görüntüleme

Çözüm hızlandırıcısının **Rules** (Kurallar) sayfasında geçerli kuralların listesi görüntülenir:

[![Rules (Kurallar) Sayfası](./media/iot-accelerators-remote-monitoring-automate/rulesactions_v2-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactions_v2-expanded.png#lightbox)

Yalnızca soğutucular için geçerli olan kuralları görüntülemek için bir filtre uygulayın:

[![Kural listesini filtreleme](./media/iot-accelerators-remote-monitoring-automate/rulesactionsfilter_v2-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactionsfilter_v2-expanded.png#lightbox)

Listedeki kurallardan birini seçerek hakkında daha fazla bilgi görüntüleyebilir ve düzenleyebilirsiniz:

[![Kural ayrıntılarını görüntüleme](./media/iot-accelerators-remote-monitoring-automate/rulesactionsdetail_v2-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactionsdetail_v2-expanded.png#lightbox)

Kurallardan birini veya daha fazlasını devre dışı bırakmak veya etkinleştirmek için listeden bir veya daha fazla kural seçin:

[![Birden fazla kural seçme](./media/iot-accelerators-remote-monitoring-automate/rulesactionsmultiselect_v2-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactionsmultiselect_v2-expanded.png#lightbox)

## <a name="create-a-rule"></a>Kural oluşturma

Bir soğutucu cihazın basıncı 150 PSI seviyesinin üzerine çıktığında uyarı oluşturan bir kural oluşturmak için **New rule** (Yeni kural) öğesine tıklayın. Kuralı oluşturmak için aşağıdaki değerleri kullanın:

| Ayar          | Değer                                 |
| ---------------- | ------------------------------------- |
| Kural adı        | Soğutucu uyarısı                       |
| Açıklama      | Soğutucu basıncı 150 PSI seviyesinin üzerine çıktı |
| Cihaz grubu     | **Soğutucular** cihaz grubu             |
| Hesaplama      | Anında                               |
| Koşul 1 Alanı| basınç                              |
| Koşul 1 işleci | Büyüktür                      |
| Koşul 1 değeri    | 150                               |
| Önem derecesi  | Uyarı                               |

[![Uyarı kuralı oluşturma](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_v2-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_v2-expanded.png#lightbox)

Yeni kuralı kaydetmek için **Apply** (Uygula) öğesine tıklayın.

Kuralın tetiklendiğini **Rules** (Kurallar) veya **Dashboard** (Pano) sayfasında görebilirsiniz:

[![Uyarı kuralı tetiklendi](./media/iot-accelerators-remote-monitoring-automate/warningruletriggered-inline.png)](./media/iot-accelerators-remote-monitoring-automate/warningruletriggered-expanded.png#lightbox)

## <a name="create-a-rule-with-multiple-conditions"></a>Birden fazla koşula sahip kural oluşturma

Son beş dakika içinde cihaz içindeki ortalama nem oranı %80'den fazla olan ve cihaz sıcaklığı 75 Fahrenhayt seviyesinin üzerine çıkan soğutucular için kritik uyarı oluşturan birden fazla koşula sahip bir kural oluşturmak için **New rule** (Yeni kural) öğesine tıklayın. Kuralı oluşturmak için aşağıdaki değerleri kullanın:

| Ayar          | Değer                                 |
| ---------------- | ------------------------------------- |
| Kural adı        | Soğutucuda kritik nem ve sıcaklık    |
| Açıklama      | Nem ve sıcaklık kritik düzeyde |
| Cihaz grubu     | **Soğutucular** cihaz grubu             |
| Hesaplama      | Ortalama                               |
| Süre      | 5                                     |
| Koşul 1 Alanı| Nem oranı                              |
| Koşul 1 işleci | Büyüktür                      |
| Koşul 1 değeri    | 80                                |
| Önem derecesi  | Kritik                              |

[![Birden fazla koşula sahip kural oluşturma birinci bölüm](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_mult_v2-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_mult_v2-expanded.png#lightbox)

İkinci koşulu eklemek için "+ Add condition" (Koşul ekle) öğesine tıklayın. Yeni koşul için aşağıdaki değerleri kullanın:

| Ayar          | Değer                                 |
| ---------------- | ------------------------------------- |
| Koşul 2 Alanı| sıcaklık                           |
| Koşul 2 işleci | Büyüktür                      |
| Koşul 2 değeri    | 75                                |

[![Birden fazla koşula sahip kural oluşturma ikinci bölüm](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_mult_cond2_v2-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_mult_cond2_v2-expanded.png#lightbox)

Yeni kuralı kaydetmek için **Apply** (Uygula) öğesine tıklayın.

Kuralın tetiklendiğini **Rules** (Kurallar) veya **Dashboard** (Pano) sayfasında görebilirsiniz:

[![Birden fazla koşula sahip kural tetiklendi](./media/iot-accelerators-remote-monitoring-automate/criticalruletriggered-inline.png)](./media/iot-accelerators-remote-monitoring-automate/criticalruletriggered-expanded.png#lightbox)

## <a name="edit-an-existing-rule"></a>Var olan kuralı düzenleme

Var olan bir kuralı değiştirmek için kural listesinden seçip **Edit** (Düzenle) öğesine tıklayın:

[![Kuralı düzenleme](./media/iot-accelerators-remote-monitoring-automate/rulesactionsedit_v2-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactionsedit_v2-expanded.png#lightbox)

## <a name="disable-a-rule"></a>Kuralı devre dışı bırakma

Bir kuralı geçici olarak kapatmak için kural listesinden devre dışı bırakabilirsiniz. Devre dışı bırakmak istediğiniz kuralı seçip **Disable** (Devre dışı bırak) öğesine tıklayın. Kuralın listedeki **Status** (Durum) alanı değişir ve kuralın devre dışı olduğunu belirtir. Aynı yordamı kullanarak önceden devre dışı bıraktığınız bir kuralı yeniden etkinleştirebilirsiniz.

[![Kuralı devre dışı bırakma](./media/iot-accelerators-remote-monitoring-automate/rulesactionsdisable-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactionsdisable-expanded.png#lightbox)

Listeden birden fazla kural seçerek aynı anda etkinleştirebilir ve devre dışı bırakabilirsiniz.

<!-- ## Delete a rule

To permanently delete a rule, choose the rule in the list of rules and then choose **Delete**.

You can delete multiple rules at the same time if you select multiple rules in the list.-->

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki öğreticiye geçmeyi planlıyorsanız Uzaktan İzleme çözümü hızlandırıcısı dağıtımını bırakın. Kullanmadığınız zaman çözüm hızlandırıcısının maliyetlerini düşürmek için ayarlar panelinden sanal cihazları durdurabilirsiniz:

[![Telemetriyi duraklatma](./media/iot-accelerators-remote-monitoring-automate/togglesimulation-inline.png)](./media/iot-accelerators-remote-monitoring-automate/togglesimulation-expanded.png#lightbox)

Bir sonraki öğreticiye başlamaya hazır olduğunuzda sanal cihazları yeniden başlatabilirsiniz.

Çözüm hızlandırıcısına ihtiyacınız kalmadıysa [Provisioned solutions](https://www.azureiotsolutions.com/Accelerators#dashboard) (Sağlanan çözümler) sayfasından silebilirsiniz:

![Çözümü sil](media/iot-accelerators-remote-monitoring-automate/deletesolution.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Uzaktan İzleme çözümü hızlandırıcısındaki **Rules** (Kurallar) Sayfasını kullanarak çözümdeki uyarıları tetikleyen kurallar oluşturmayı ve yönetmeyi gösterilmiştir. Çözüm hızlandırıcısını bağlı cihazlarınızı yönetme ve yapılandırma amacıyla kullanmayı öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [İzleme çözümünüze bağlı cihazları yapılandırma ve yönetme](iot-accelerators-remote-monitoring-manage.md)