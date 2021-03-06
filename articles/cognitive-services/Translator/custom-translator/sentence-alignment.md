---
title: Eşleştirme ve hizalama - özel Translator cümle
titleSuffix: Azure Cognitive Services
description: Eğitim yürütme sırasında paralel belgelerinde mevcut cümlelerin eşleştirilmiş veya hizalanır. Özel Translator çevirileri bir cümle, bir kerede bir cümle, bu cümleyi çevirisi okuyarak öğrenir. Ardından sözcük ve tümcecikleri birbirine bu iki cümle içinde hizalar.
author: swmachan
manager: christw
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: f73c40704e10a8e2368ee1eb369ee3dccdf269ee
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448308"
---
# <a name="sentence-pairing-and-alignment-in-parallel-documents"></a>Tümce eşleştirme ve paralel belgelerde hizalama

Eğitim sırasında paralel belgelerinde mevcut cümlelerin eşleştirilmiş veya hizalanır. Özel Translator hizalı cümlelerde her veri kümeleri olarak eşleştirin mümkün cümleler sayısını raporlar.

## <a name="pairing-and-alignment-process"></a>Eşleştirme ve hizalama işlemi

Özel Translator cümleler bir cümle çevirileri aynı anda öğrenir. Bu bir kaynak ve hedefin bu cümleyi çevirisi cümle okumalar. Ardından sözcük ve tümcecikleri birbirine bu iki cümle içinde hizalar. Bu işlem bir cümle eşdeğer sözcük ve tümcecikleri bu cümleyi çevirisi sözcük ve tümcecikleri haritasını oluşturmak için sağlar. Hizalama, sistemi çevirileri birbiriyle olan cümleler üzerinde eğitir emin olmak çalışır.

## <a name="pre-aligned-documents"></a>Önceden hizalanmış belgeleri

Paralel belgeleriniz biliyorsanız, önceden hizalı metin dosyaları sağlanarak cümle hizalama geçersiz kılabilir. Metin dosyasına satır ve karşıya yükleme ile düzenlenmiş bir cümle hem belgelerden tüm cümleleri ayıklayabileceğiniz bir `.align` uzantısı. `.align` Uzantısı sinyalleri özel Translator cümle hizalama atlayın.

Her satırda bir cümle dosyalarınızı olduğundan emin olmak en iyi sonuçlar için deneyin. Bu kötü hizalamaları neden olacak şekilde bir tümce içinde yeni satır karakterleri yok.

## <a name="suggested-minimum-number-of-extracted-and-aligned-sentences"></a>Ayıklanan ve hizalanmış cümleler önerilen en düşük sayısı

Bir eğitim işleminin başarılı olması aşağıdaki tabloda ayıklanan cümleler ve her bir veri kümesi gerekli hizalanmış cümleler en az sayısını gösterir. Ayıklanan cümleler önerilen en az sayıda hizalanmış cümleler cümle hizalama tüm ayıklanan cümleler başarıyla hizalamak mümkün olmayabilir olgu hesaba katması için önerilen en düşük sayısından çok yüksektir.

| Veri kümesi   | Önerilen en düşük ayıklanan cümle sayısı | Önerilen en düşük hizalanmış cümle sayısı | En fazla hizalanmış cümle sayısı |
|------------|--------------------------------------------|------------------------------------------|--------------------------------|
| Eğitim   | 10,000                                     | 2,000                                    | Üst sınır                 |
| Ayarlama     | 2,000                                      | 500                                      | 2,500                          |
| Test Etme    | 2,000                                      | 500                                      | 2,500                          |
| Sözlük | 0                                          | 0                                        | Üst sınır                 |

## <a name="next-steps"></a>Sonraki adımlar

- Nasıl kullanacağınızı öğrenin bir [sözlük](what-is-dictionary.md) özel Translator içinde.
