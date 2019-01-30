---
title: Belge biçimleri ve adlandırma kuralları - özel Translator
titleSuffix: Azure Cognitive Services
description: Bu belge biçimlerini ve özel Translator adlandırma kuralı kılavuzudur. Bu kavramı daha iyi abd adlandırma çakışmalarını önlemek belgeleri adları yönetilmesine yardımcı olur.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.subservice: custom-translator
ms.date: 11/13/2018
ms.author: v-rada
ms.topic: conceptual
ms.openlocfilehash: afd3192117bd22c62fd8e36752515166e4c6e043
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55225491"
---
# <a name="document-formats-and-naming-convention-guidance"></a>Belge biçimleri ve adlandırma kuralı Kılavuzu

Özel çeviri için kullanılan herhangi bir dosya en az olmalıdır **dört** karakter uzunluğunda.

Bu tablo, çeviri sisteminizi oluşturmak için kullanabileceğiniz tüm desteklenen dosya biçimleri içerir:

| Biçimlendir            | Uzantılar   | Açıklama                                                                                                                                                                                                                                                                    |
|-------------------|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| XLIFF             | . XLF. XLIFF | Bir paralel belge biçimi, çeviri bellek sistemlerinin dışarı aktarma. Kullanılan dilleri dosyanın içinde tanımlanır.                                                                                                                                                              |
| TMX               | . TMX         | Bir paralel belge biçimi, çeviri bellek sistemlerinin dışarı aktarma. Kullanılan dilleri dosyanın içinde tanımlanır.                                                                                                                                                              |
| ZIP               | . ZIP         | ZIP arşivi dosya biçimidir.                                                                                                                                                                                                        |
| Locstudio         | . LCL         | Paralel belgeleri için Microsoft biçimi                                                                                                                                                                                                                                      |
| Microsoft Word    | . DOCX        | Microsoft Word belgesi                                                                                                                                                                                                                                                        |
| Adobe Acrobat     | . PDF         | Adobe Acrobat Taşınabilir Belge                                                                                                                                                                                                                                                |
| HTML              | .HTML, .HTM  | HTML belgesi                                                                                                                                                                                                                                                                  |
| Metin dosyası         | .TXT         | UTF-16 veya UTF-8 olarak kodlanmış metin dosyaları                                                                                                                                                                                                                                             |
| Hizalanmış bir metin dosyası | . HİZALA       | Uzantı `.ALIGN` belge çifti cümleleri mükemmel bir şekilde hizalı olup biliyorsanız kullanabileceğiniz özel bir uzantısıdır. Sağlarsanız bir `.ALIGN` dosya, özel Translator değil hizalanacağı cümleleri sizin için. |
| Excel dosyası        | . XLSX        | Excel dosyası (2013 veya üzeri). İlk satır / satır elektronik dil kodu olmalıdır.                                                                                                                                                                                                                                                      |

## <a name="dictionary-formats"></a>Sözlük biçimleri

Sözlük için özel Translator olanlar için Eğitim kümesi desteklenen tüm dosya biçimlerini destekler. Excel sözlük kullanıyorsanız, ilk satırı emin olun / dil kodlarını. elektronik tablonun satır olmalıdır.

## <a name="zip-file-formats"></a>Zip dosya biçimleri

Belgeler, gruplandırılmış bir tek bir ZIP dosyasına ekleyin ve karşıya yüklendi. Özel Translator desteklediği dosya biçimleri (ZIP, GZ ve TGZ) zip.

Her belge uzantılı zip dosyasında TXT, HTML, HTM, PDF, DOCX, hizalama, bu adlandırma kuralları izlemelidir:

{Belge adı} \_{language kodu} {dil kodu} burada {belge adı}, belgenin adını, ISO belge o dilde cümleler içerdiğini gösteren LanguageID (iki karakter) karşılık gelir. Dil kodu önce bir alt çizgi (_) olması gerekir.

Örneğin, bir İngilizce, İspanyolca sistemine bir ZIP içindeki iki paralel belgeler karşıya yüklemek için dosyaları "data_en" ve "data_es" adlı olmalıdır.

Çeviri bellek dosyaları (TMX, XLF, XLIFF, LCL, XLSX) belirli bir dil adlandırma kuralını uyguladığınızdan gerekli değildir.  

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [proje](workspace-and-project.md#what-is-a-custom-translator-project) oluşturmak ve bunları yönetmek için.
