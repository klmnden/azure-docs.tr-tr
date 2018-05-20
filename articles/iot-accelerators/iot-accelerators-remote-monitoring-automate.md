---
title: Uzaktan izleme çözümünde - Azure cihaz sorunlarını algılamak | Microsoft Docs
description: Bu öğretici nasıl Uzaktan izleme çözümünde eşik tabanlı aygıt sorunları otomatik olarak algılamak için kurallar ve Eylemler kullanılacağını gösterir.
services: iot-suite
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 05/01/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 461490a312cbcfda50f4b2e9db39c40250d716fd
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="detect-issues-using-threshold-based-rules"></a>Eşik tabanlı kurallar kullanarak sorunlarını Algıla

Bu öğretici, Uzaktan izleme çözümünde kurallar altyapısı özelliklerini gösterir. Bu özellikler tanıtmak için öğretici senaryo Contoso IOT uygulamada kullanır.

Contoso sahip baskısı tarafından bildirilen kritik uyarı oluşturur olan bir kural bir **Soğutucu** aygıt 250 PSI aşıyor. Bir operatör olarak tanımlamak istediğiniz **Soğutucu** sorunlu algılayıcılar bakarak olabilecek aygıtlar ilk baskısı ani. Bu cihazları tanımlamak için baskısı 150 PSI aştığında bir uyarı oluşturmak için bir kural oluşturun.

Ayrıca bir kritik uyarı gerektiğini istiyordu ayarlandığında tetiklenen, ortalama nem **Soğutucu** aygıt son 5 dakika içinde % 80 sıcaklık'dan büyük ve **Soğutucu** aygıtı son 5 dakikadır 75 derece Fahrenhayt büyük.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

>[!div class="checklist"]
> * Çözümünüzde kuralları görüntülemek
> * Yeni bir kural oluşturun
> * Birden çok koşul ile yeni bir kural oluşturun
> * Mevcut bir kuralı Düzenle
> * Kural silme

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi izlemek için Azure aboneliğinizde Uzaktan izleme çözümü dağıtılan bir örneğini gerekir.

Uzaktan izleme çözümü dağıtılan henüz henüz tamamlanmış olmalıdır, [Uzaktan izleme Çözüm Hızlandırıcısı dağıtmak](iot-accelerators-remote-monitoring-deploy.md) Öğreticisi.

## <a name="view-the-rules-in-your-solution"></a>Çözümünüzde kuralları görüntülemek

**Kuralları** çözümü sayfasında geçerli olan tüm kuralların listesini görüntüler:

![Kurallar ve Eylemler sayfası](./media/iot-accelerators-remote-monitoring-automate/rulesactions_v2.png)

Geçerli kurallarını görüntülemek için **Soğutucu** aygıtlar, bir filtre uygula:

![Kurallar listesini filtreler](./media/iot-accelerators-remote-monitoring-automate/rulesactionsfilter_v2.png)

Bir kural hakkında daha fazla bilgi görüntülemek ve listede seçtiğinizde düzenleyin:

![Kuralın ayrıntılarını görüntüleme](./media/iot-accelerators-remote-monitoring-automate/rulesactionsdetail_v2.png)

Devre dışı bırakma, etkinleştirme veya bir veya daha fazla kural silmek için birden çok kural listeden seçin:

![Birden çok kural seçin](./media/iot-accelerators-remote-monitoring-automate/rulesactionsmultiselect_v2.png)

## <a name="create-a-new-rule"></a>Yeni bir kural oluşturun

Bir uyarı oluşturur Yeni bir kural eklemek için zaman baskısı bir **Soğutucu** aygıt 150 PSI aşıyor, seçin **yeni kural**:

![Kural oluşturma](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_v2.png)

Kuralı oluşturmak için aşağıdaki değerleri kullanın:

| Ayar          | Değer                                 |
| ---------------- | ------------------------------------- |
| Kural adı        | Soğutucu uyarı                       |
| Açıklama      | Soğutucu baskısı 150 PSI aştı |
| Cihaz grubu     | **Chillers** cihaz grubu             |
| Hesaplama      | Anlık                               |
| Koşul 1 alan| basınç                              |
| Koşul 1 işleci | Şu değerden fazla:                      |
| Koşul 1 değeri    | 150                               |
| Serverity düzeyi  | Uyarı                               |

Yeni Kural kaydetmek üzere seçim yapın **Uygula**.

Üzerinde kuralı tetiklendiğinde görüntüleyebileceğiniz **kuralları** sayfa veya **Pano** sayfası.

## <a name="create-a-new-rule-with-multiple-conditions"></a>Birden çok koşul ile yeni bir kural oluşturun

Kritik bir oluşturur birden çok koşul ile yeni bir kural oluşturmak için ne zaman uyarı, ortalama nem **Soğutucu** aygıt son 5 dakika içinde % 80 sıcaklık'dan büyük ve **Soğutucu** Aygıtın son 5 dakika içinde 75 derece Fahrenhayt büyük olduğundan, seçin **yeni kural**:

![Mult kuralı oluşturma](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_mult_v2.png)

Kuralı oluşturmak için aşağıdaki değerleri kullanın:

| Ayar          | Değer                                 |
| ---------------- | ------------------------------------- |
| Kural adı        | Soğutucu nem ve temp kritik    |
| Açıklama      | Nem ve sıcaklık kritik |
| Cihaz grubu     | **Chillers** cihaz grubu             |
| Hesaplama      | Ortalama                               |
| Süre      | 5                                     |
| Koşul 1 alan| nem oranı                              |
| Koşul 1 işleci | Şu değerden fazla:                      |
| Koşul 1 değeri    | 80                               |
| Serverity düzeyi  | Kritik                              |

İkinci koşulu eklemek için tıklayın "+ koşul Ekle".

![Koşul 2 oluşturun](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_mult_cond2_v2.png)

Yeni koşulu aşağıdaki değerleri kullanın:

| Ayar          | Değer                                 |
| ---------------- | ------------------------------------- |
| Koşul 2 alan| Sıcaklık                           |
| Koşul 2 işleci | Şu değerden fazla:                      |
| Koşul 2 değeri    | 75                                |

Yeni Kural kaydetmek üzere seçim yapın **Uygula**.

Üzerinde kuralı tetiklendiğinde görüntüleyebileceğiniz **kuralları** sayfa veya **Pano** sayfası.

## <a name="edit-an-existing-rule"></a>Mevcut bir kuralı Düzenle

Mevcut bir kuralı bir değişiklik yapmak için kurallar listesinde seçin.

![Kuralını Düzenle](./media/iot-accelerators-remote-monitoring-automate/rulesactionsedit_v2.png)

<!--## Disable a rule

To temporarily switch off a rule, you can disable it in the list of rules. Choose the rule to disable, and then choose **Disable**. The **Status** of the rule in the list changes to indicate the rule is now disabled. You can re-enable a rule that you previously disabled using the same procedure.

![Disable rule](./media/iot-accelerators-remote-monitoring-automate/rulesactionsdisable.png)

You can enable and disable multiple rules at the same time if you select multiple rules in the list.-->

<!--## Delete a rule

To permanently delete a rule, choose the rule in the list of rules and then choose **Delete**.

You can delete multiple rules at the same time if you select multiple rules in the list.-->

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide gösterilen, nasıl için:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Çözümünüzde kuralları görüntülemek
> * Yeni bir kural oluşturun
> * Mevcut bir kuralı Düzenle
> * Kural silme

Eşik tabanlı kurallar kullanarak sorunlarını algılamak öğrendiniz, bilgi edinmek için önerilen sonraki adımlar olan nasıl yapılır:

* [Yönetmek ve cihazlarınızı yapılandırmak](./../iot-suite/iot-suite-remote-monitoring-manage.md).
* [Aygıt sorunlarını gidermek ve](./../iot-suite/iot-suite-remote-monitoring-maintain.md).
* [Sanal cihazlar ile çözümünüzü test](../iot-suite/iot-suite-remote-monitoring-test.md).

<!-- Next tutorials in the sequence -->