---
title: Deneme - Azure Bilişsel hizmetler | Microsoft Docs
description: Bu makalede, Azure özel karar hizmet deneme için bir kılavuzdur.
services: cognitive-services
author: marco-rossi29
manager: marco-rossi29
ms.service: cognitive-services
ms.topic: article
ms.date: 05/10/2018
ms.author: marossi
ms.openlocfilehash: b0ac0bc049d556423493f0c48dd9a548929bcd41
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354953"
---
# <a name="experimentation"></a>Deneme

Teorik olarak aşağıdaki [bağlamsal bandits (CB)](https://www.microsoft.com/en-us/research/blog/contextual-bandit-breakthrough-enables-deeper-personalization/), özel karar hizmeti sürekli bir içerik gözlemleyen, bir eylemde ve seçilen eylemi için bir ödül gözlemleyen. İçerik kişiselleştirme örneğidir: bir kullanıcı bağlamı açıklar eylemlerdir adayı hikayeler ve önerilen Öykü ne kadar kullanıcı beğendiğinizi ödül ölçer.

Eylemler için bağlamları eşlemeleri gibi özel karar hizmet bir ilke üretir. Belirli hedef ilkesiyle beklenen ödül bilmek ister. Ödül tahmin etmenin bir yolu bir ilke çevrimiçi eylemleri (örneğin, kullanıcılara öneri hikayeleri) seçin çalışmasına izin kullanmaktır. Ancak, bu tür çevrimiçi değerlendirme iki nedenden dolayı pahalı olabilir:

* Sınanmamış, Deneysel bir ilkeyi kullanıcılara gösterir.
* Birden çok hedef ilkelerini değerlendirmek için ölçeklendirilmediğini.

İlke devre dışı değerlendirmesi alternatif bir örnektir. İlke devre dışı değerlendirmesi, günlük ilkesi izleyin var olan bir çevrimiçi sistemi günlüklerinden varsa, yeni hedef ilkelerin beklenen ödül tahmin edebilirsiniz.

Günlük dosyası kullanarak, en yüksek tahmini, beklenen ödül ilkesiyle bulmak deneme arar. Hedef ilkeleri tarafından parametreli [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit/wiki) bağımsız değişkenler. Vowpal Wabbit bağımsız değişkenleri, çeşitli komut dosyası için ekleyerek varsayılan modunda testleri `--base_command`. Komut dosyası, aşağıdaki eylemleri gerçekleştirir:

* Özellikleri ilk ad alanlarını otomatik olarak algılar `--auto_lines` satırlık bir giriş dosyası.
* Hyper-parametreleri ilk tarama gerçekleştirir (`learning rate`, `L1 regularization`, ve `power_t`).
* Testleri ilke değerlendirmesi `--cb_type` (ters propensity puanı (`ips`) veya karakteriyle sağlam (`dr`). Daha fazla bilgi için bkz: [bağlamsal Bandit örnek](https://github.com/JohnLangford/vowpal_wabbit/wiki/Contextual-Bandit-Example).
* Testleri marginals.
* Testleri ikinci derece etkileşim özellikleri:
   * **yanılma aşaması**: Tüm birleşimleri olan testleri `--q_bruteforce_terms` çiftleri veya daha az.
   * **doyumsuz aşaması**: oluncaya kadar hiçbir geliştirme için en iyi çifti ekler `--q_greedy_stop` yuvarlar.
* Hyper-parametreleri ikinci bir tarama gerçekleştirir (`learning rate`, `L1 regularization`, ve `power_t`).

Bu adımları denetleyen parametreler bazı Vowpal Wabbit bağımsız değişkenleri şunlardır:
- Örnek işleme seçenekleri:
  - Paylaşılan ad alanları
  - Eylem ad alanları
  - Marjinal ad alanları
  - İkinci derece özellikleri
- Güncelleştirme kuralı seçenekleri
  - öğrenme oranı
  - L1 regularization
  - t güç değeri

Yukarıdaki bağımsız değişkenleri ayrıntılı bir açıklaması için bkz: [Vowpal Wabbit komut satırı bağımsız değişkenleri](https://github.com/JohnLangford/vowpal_wabbit/wiki/Command-line-arguments).

## <a name="prerequisites"></a>Önkoşullar
- Vowpal Wabbit: Yüklü ve yolunuzu.
  - Windows: [kullanım `.msi` yükleyici](https://github.com/eisber/vowpal_wabbit/releases).
  - Diğer platformlar: [kaynak kodunu alma](https://github.com/JohnLangford/vowpal_wabbit/releases).
- Python 3: Yüklü ve yolunuzu.
- NumPy: tercih ettiğiniz Paket Yöneticisi'ni kullanın.
- *Mwt/Microsoft-ds* deposu: [depoyu kopyalama](https://github.com/Microsoft/mwt-ds).
- Karar hizmet JSON günlük dosyası: varsayılan olarak, temel komutu içerir `--dsjson`, karar hizmet JSON giriş veri dosyasının ayrıştırılması sağlar. [Bu biçim örneği Al](https://github.com/JohnLangford/vowpal_wabbit/blob/master/test/train-sets/decisionservice.json).

## <a name="usage"></a>Kullanım
Git `mwt-ds/DataScience` çalıştırıp `Experimentation.py` aşağıdaki kodda ayrıntılı olarak ilgili bağımsız değişkenlere sahip:

```cmd
python Experimentation.py [-h] -f FILE_PATH [-b BASE_COMMAND] [-p N_PROC]
                          [-s SHARED_NAMESPACES] [-a ACTION_NAMESPACES]
                          [-m MARGINAL_NAMESPACES] [--auto_lines AUTO_LINES]
                          [--only_hp] [-l LR_MIN_MAX_STEPS]
                          [-r REG_MIN_MAX_STEPS] [-t PT_MIN_MAX_STEPS]
                          [--q_bruteforce_terms Q_BRUTEFORCE_TERMS]
                          [--q_greedy_stop Q_GREEDY_STOP]
```

Sonuçları bir günlük eklenir *mwt-ds/DataScience/experiments.csv* dosya.

### <a name="parameters"></a>Parametreler
| Girdi | Açıklama | Varsayılan |
| --- | --- | --- |
| `-h`, `--help` | Yardım iletisi ve çıkış gösterir. | |
| `-f FILE_PATH`, `--file_path FILE_PATH` | Veri dosyası yolu (`.json` veya `.json.gz` biçimidir - her satır bir `dsjson`). | Gerekli |  
| `-b BASE_COMMAND`, `--base_command BASE_COMMAND` | Temel Vowpal Wabbit komutu.  | `vw --cb_adf --dsjson -c` |  
| `-p N_PROC`, `--n_proc N_PROC` | Kullanmak için paralel işlem sayısı. | Mantıksal işlemciler |  
| `-s SHARED_NAMESPACES, --shared_namespaces SHARED_NAMESPACES` | Paylaşılan özellik ad alanları (örneğin, `abc` ad alanları anlamına gelir `a`, `b`, ve `c`).  | Veri dosyasından otomatik algıla |  
| `-a ACTION_NAMESPACES, --action_namespaces ACTION_NAMESPACES` | Eylem özelliği ad alanları. | Veri dosyasından otomatik algıla |  
| `-m MARGINAL_NAMESPACES, --marginal_namespaces MARGINAL_NAMESPACES` | Marjinal özelliği ad alanları. | Veri dosyasından otomatik algıla |  
| `--auto_lines AUTO_LINES` | Özellikleri ad alanlarını otomatik olarak algılamak için taramak için veri dosyası satır sayısı. | `100` |  
| `--only_hp` | Yalnızca hyper-parametreleri Sweep (`learning rate`, `L1 regularization`, ve `power_t`). | `False` |  
| `-l LR_MIN_MAX_STEPS`, `--lr_min_max_steps LR_MIN_MAX_STEPS` | Pozitif değerler olarak hızı aralığı öğrenme `min,max,steps`. | `1e-5,0.5,4` |  
| `-r REG_MIN_MAX_STEPS`, `--reg_min_max_steps REG_MIN_MAX_STEPS` | L1 regularization Aralık pozitif değerler olarak `min,max,steps`. | `1e-9,0.1,5` |  
| `-t PT_MIN_MAX_STEPS`, `--pt_min_max_steps PT_MIN_MAX_STEPS` | Power_t Aralık pozitif değerler olarak `min,max,step`. | `1e-9,0.5,5` |  
| `--q_bruteforce_terms Q_BRUTEFORCE_TERMS` | Yanılma aşamasında test etmek için ikinci derece çiftlerinin sayısı. | `2` |  
| `--q_greedy_stop Q_GREEDY_STOP` | İyileştirmeleri, daha sonra ikinci derece doyumsuz arama aşaması durdurulur olmadan yuvarlar. | `3` |  

### <a name="examples"></a>Örnekler
Önceden ayarlanmış varsayılan değerleri kullanmak için:
```cmd
python Experimentation.py -f D:\multiworld\data.json
```

Vowpal Wabbit de eşdeğer, işleyebilen `.json.gz` dosyaları:
```cmd
python Experimentation.py -f D:\multiworld\data.json.gz
```

Yalnızca hyper-parametreleri sweep için (`learning rate`, `L1 regularization`, ve `power_t`, 2. adım sonra durdurma):
```cmd
python Experimentation.py -f D:\multiworld\data.json --only_hp
```
