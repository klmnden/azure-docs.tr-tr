---
title: Belge biçimleri ve adlandırma kuralları - özel Translator
titleSuffix: Azure Cognitive Services
description: Bu belge biçimlerini ve özel Translator adlandırma kuralı kılavuzudur. Bu kavramı daha iyi abd adlandırma çakışmalarını önlemek belgeleri adları yönetilmesine yardımcı olur.
author: swmachan
manager: christw
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: 2f7a83be510e608bb3f630a2fb1860502d8e4475
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443418"
---
# <a name="document-formats-and-naming-convention-guidance"></a>Belge biçimleri ve adlandırma kuralı Kılavuzu

Özel çeviri için kullanılan herhangi bir dosya en az olmalıdır **dört** karakter uzunluğunda.

Bu tablo, çeviri sisteminizi oluşturmak için kullanabileceğiniz tüm desteklenen dosya biçimleri içerir:

| Biçimi            | Uzantılar   | Açıklama                                                                                                                                                                                                                                                                    |
|-------------------|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| XLIFF             | . XLF. XLIFF | Bir paralel belge biçimi, çeviri bellek sistemlerinin dışarı aktarma. Kullanılan dilleri dosyanın içinde tanımlanır.                                                                                                                                                              |
| TMX               | . TMX         | Bir paralel belge biçimi, çeviri bellek sistemlerinin dışarı aktarma. Kullanılan dilleri dosyanın içinde tanımlanır.                                                                                                                                                              |
| ZIP               | . ZIP         | ZIP arşivi dosya biçimidir.                                                                                                                                                                                                        |
| Locstudio         | . LCL         | Paralel belgeleri için Microsoft biçimi                                                                                                                                                                                                                                      |
| Microsoft Word    | . DOCX        | Microsoft Word belgesi                                                                                                                                                                                                                                                        |
| Adobe Acrobat     | . PDF         | Adobe Acrobat Taşınabilir Belge                                                                                                                                                                                                                                                |
| HTML              | .HTML, .HTM  | HTML belgesi                                                                                                                                                                                                                                                                  |
| Metin dosyası         | .TXT         | UTF-16 veya UTF-8 olarak kodlanmış metin dosyaları. Dosya adı, Japonca karakterler içermemelidir.                                                                                                                                                                                        |
| Hizalanmış bir metin dosyası | . HİZALA       | Uzantı `.ALIGN` belge çifti cümleleri mükemmel bir şekilde hizalı olup biliyorsanız kullanabileceğiniz özel bir uzantısıdır. Sağlarsanız bir `.ALIGN` dosya, özel Translator değil hizalanacağı cümleleri sizin için. |
| Excel dosyası        | . XLSX        | Excel dosyası (2013 veya üzeri). İlk satır / satır elektronik dil kodu olmalıdır.                                                                                                                                                                                                                                                      |

## <a name="dictionary-formats"></a>Sözlük biçimleri

Sözlük için özel Translator eğitim kümeleri için desteklenen tüm dosya biçimlerini destekler. İlk satır bir Excel sözlük kullandığınız / dil kodlarını. elektronik tablonun satır olmalıdır

## <a name="zip-file-formats"></a>Zip dosya biçimleri

Belgeler, gruplandırılmış bir tek bir ZIP dosyasına ekleyin ve karşıya yüklendi. Özel Translator desteklediği dosya biçimleri (ZIP, GZ ve TGZ) zip.

Her belge uzantılı zip dosyasında TXT, HTML, HTM, PDF, DOCX, hizalama, bu adlandırma kuralları izlemelidir:

{Belge adı} \_{language kodu} {dil kodu} burada {belge adı}, belgenin adını, ISO belge o dilde cümleler içerdiğini gösteren LanguageID (iki karakter) karşılık gelir. Dil kodu önce bir alt çizgi (_) olması gerekir.

Örneğin, bir İngilizce, İspanyolca sistemine bir ZIP içindeki iki paralel belgeler karşıya yüklemek için dosyaları "data_en" ve "data_es" adlı olmalıdır.

Çeviri bellek dosyaları (TMX, XLF, XLIFF, LCL, XLSX) belirli bir dil adlandırma kuralını uyguladığınızdan gerekli değildir.  

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [proje](workspace-and-project.md#what-is-a-custom-translator-project) oluşturmak ve bunları yönetmek için.
