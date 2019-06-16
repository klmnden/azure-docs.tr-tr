---
title: Deneme - özel karar alma hizmeti
titlesuffix: Azure Cognitive Services
description: Bu makalede, deneme Custom Decision Service için bir kılavuzdur.
services: cognitive-services
author: marco-rossi29
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-decision-service
ms.topic: conceptual
ms.date: 05/10/2018
ms.author: marossi
ms.openlocfilehash: b5f8c853218a1db53f4dd23e7254b35990a7132b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60829183"
---
# <a name="experimentation"></a>Deneme

Teoride, aşağıdaki [bağlamsal bandits (CB)](https://www.microsoft.com/en-us/research/blog/contextual-bandit-breakthrough-enables-deeper-personalization/), Custom Decision Service sürekli bir içerik uygulamasını gözlediğini eyleme geçen ve seçilen eylem için bir ödül gözlemler. İçerik kişiselleştirme örneğidir: bir kullanıcı bağlamı açıklayan eylemlerdir aday hikayeleri ve önerilen hikaye ne kadar kullanıcı beğenmediğinizi ödül ölçer.

Eylemler için bağlamları eşlerken özel karar alma hizmeti bir ilke oluşturur. Bir hedef ilkesiyle, beklenen ödül bilmek istiyorsunuz. Ödül tahmin etmek için bir ilke çevrimiçi kullanın ve bu eylemleri (örneğin, kullanıcılara öneri hikayeleri) seçin yoludur. Bununla birlikte, iki nedenden dolayı gibi çevrimiçi değerlendirme pahalı olabilir:

* Bu, test edilmemiş, Deneysel bir ilkeyi kullanıcılara kullanıma sunar.
* Birden çok hedef ilkelerini değerlendirme için ölçeklendirilemez.

İlkeyi-değerlendirme alternatif bir örnektir. İlkeyi-değerlendirme, günlük ilkesi izleyin var olan bir çevrimiçi sistem günlüklerinden varsa, yeni hedef ilkeleri beklenen büyüyen tahmin edebilirsiniz.

Günlük dosyası kullanarak, en yüksek tahmini, beklenen ödül ilkesiyle bulmak deneme arar. Hedef ilkeleri parametreli hale [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit/wiki) bağımsız değişkenler. Vowpal Wabbit bağımsız değişkenleri çeşitli betiği için ekleyerek varsayılan modunda test `--base_command`. Komut aşağıdaki eylemleri gerçekleştirir:

* Otomatik olarak algılar, ad alanları ilk özellikleri `--auto_lines` satır giriş dosyası.
* Hyper-parametreleriyle ilk bir tarama gerçekleştirir (`learning rate`, `L1 regularization`, ve `power_t`).
* Test İlkesi değerlendirmesi `--cb_type` (ters eğilimini puanı (`ips`) veya karakteriyle güçlü (`dr`). Daha fazla bilgi için [bağlamsal Bandit örnek](https://github.com/JohnLangford/vowpal_wabbit/wiki/Contextual-Bandit-Example).
* Testleri marginals.
* Testleri ikinci dereceden etkileşim özellikleri:
   * **deneme yanılma aşaması**: Tüm birleşimlerle testleri `--q_bruteforce_terms` çiftleri ya da daha az.
   * **doyumsuz aşaması**: Oluncaya kadar hiçbir geliştirme için en iyi çifti ekler `--q_greedy_stop` yuvarlar.
* Hyper-parametreleriyle üzerinde ikinci bir tarama gerçekleştirir (`learning rate`, `L1 regularization`, ve `power_t`).

Bu adımları denetleyen parametreler bazı Vowpal Wabbit bağımsız değişkenleri şunlardır:
- Örnek işleme seçenekleri:
  - Paylaşılan ad alanları
  - Eylem ad alanları
  - Marjinal ad alanları
  - ikinci dereceden özellikleri
- Güncelleştirme kural seçenekleri
  - öğrenme oranı
  - L1 düzenleme
  - t power değeri

Yukarıdaki bağımsız değişkenler ayrıntılı bir açıklaması için bkz: [Vowpal Wabbit komut satırı bağımsız değişkenleri](https://github.com/JohnLangford/vowpal_wabbit/wiki/Command-line-arguments).

## <a name="prerequisites"></a>Önkoşullar
- Vowpal Wabbit: Yüklü ve yolunuzu.
  - Windows: [Kullanım `.msi` yükleyici](https://github.com/eisber/vowpal_wabbit/releases).
  - Diğer platformlar: [Kaynak kodu alma](https://github.com/JohnLangford/vowpal_wabbit/releases).
- Python 3: Yüklü ve yolunuzu.
- NumPy: Seçtiğiniz Paket Yöneticisi'ni kullanın.
- *Mwt/Microsoft-ds* depo: [Depoyu kopyalayalım](https://github.com/Microsoft/mwt-ds).
- Karar alma hizmeti JSON günlük dosyası: Varsayılan olarak, temel komut içerir `--dsjson`, giriş veri dosyasını karar alma hizmeti JSON ayrıştırma sağlar. [Bu biçim örneği Al](https://github.com/JohnLangford/vowpal_wabbit/blob/master/test/train-sets/decisionservice.json).

## <a name="usage"></a>Kullanım
Git `mwt-ds/DataScience` çalıştırıp `Experimentation.py` aşağıdaki kodda ayrıntılı olarak ilgili bağımsız değişkenleriyle:

```cmd
python Experimentation.py [-h] -f FILE_PATH [-b BASE_COMMAND] [-p N_PROC]
                          [-s SHARED_NAMESPACES] [-a ACTION_NAMESPACES]
                          [-m MARGINAL_NAMESPACES] [--auto_lines AUTO_LINES]
                          [--only_hp] [-l LR_MIN_MAX_STEPS]
                          [-r REG_MIN_MAX_STEPS] [-t PT_MIN_MAX_STEPS]
                          [--q_bruteforce_terms Q_BRUTEFORCE_TERMS]
                          [--q_greedy_stop Q_GREEDY_STOP]
```

Sonuçları günlüğe eklenir *mwt-ds/DataScience/experiments.csv* dosya.

### <a name="parameters"></a>Parametreler
| Girdi | Açıklama | Varsayılan |
| --- | --- | --- |
| `-h`, `--help` | Yardım iletisini ve çıkış gösterir. | |
| `-f FILE_PATH`, `--file_path FILE_PATH` | Veri dosyası yolu (`.json` veya `.json.gz` biçimidir - her satır bir `dsjson`). | Gerekli |  
| `-b BASE_COMMAND`, `--base_command BASE_COMMAND` | Temel Vowpal Wabbit komutu.  | `vw --cb_adf --dsjson -c` |  
| `-p N_PROC`, `--n_proc N_PROC` | Kullanılacak paralel işlem sayısı. | Mantıksal işlemciler |  
| `-s SHARED_NAMESPACES, --shared_namespaces SHARED_NAMESPACES` | Paylaşılan özellik ad alanları (örneğin, `abc` ad alanları anlamına gelir `a`, `b`, ve `c`).  | Veri dosyasından otomatik olarak algıla |  
| `-a ACTION_NAMESPACES, --action_namespaces ACTION_NAMESPACES` | Eylem özellik ad alanları. | Veri dosyasından otomatik olarak algıla |  
| `-m MARGINAL_NAMESPACES, --marginal_namespaces MARGINAL_NAMESPACES` | Marjinal özellik ad alanları. | Veri dosyasından otomatik olarak algıla |  
| `--auto_lines AUTO_LINES` | Özellikleri ad alanları otomatik olarak algılamak için taramak üzere veri dosyası satırların sayısı. | `100` |  
| `--only_hp` | Yalnızca hiper parametreler üzerinde tarama (`learning rate`, `L1 regularization`, ve `power_t`). | `False` |  
| `-l LR_MIN_MAX_STEPS`, `--lr_min_max_steps LR_MIN_MAX_STEPS` | Pozitif değerler olarak hızı aralığı öğrenme `min,max,steps`. | `1e-5,0.5,4` |  
| `-r REG_MIN_MAX_STEPS`, `--reg_min_max_steps REG_MIN_MAX_STEPS` | L1 Kurallaştırma aralığı'pozitif değerler olarak `min,max,steps`. | `1e-9,0.1,5` |  
| `-t PT_MIN_MAX_STEPS`, `--pt_min_max_steps PT_MIN_MAX_STEPS` | Power_t aralığı'pozitif değerler olarak `min,max,step`. | `1e-9,0.5,5` |  
| `--q_bruteforce_terms Q_BRUTEFORCE_TERMS` | Deneme yanılma aşamasında test etmek için ikinci dereceden çiftlerinin sayısı. | `2` |  
| `--q_greedy_stop Q_GREEDY_STOP` | İkinci dereceden doyumsuz arama aşaması sonra durdurulur iyileştirmeler olmadan yuvarlar. | `3` |  

### <a name="examples"></a>Örnekler
Önceden oluşturulmuş varsayılan değerleri kullanmak için:
```cmd
python Experimentation.py -f D:\multiworld\data.json
```

Vowpal Wabbit de eşdeğer, alabilen `.json.gz` dosyaları:
```cmd
python Experimentation.py -f D:\multiworld\data.json.gz
```

Yalnızca hyper-parametreleriyle süpürmek için (`learning rate`, `L1 regularization`, ve `power_t`, durdurma sonra 2. adım):
```cmd
python Experimentation.py -f D:\multiworld\data.json --only_hp
```
