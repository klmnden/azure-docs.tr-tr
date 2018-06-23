---
title: Yüz API'si hizmeti için Sözlük | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Terimler sözlüğü açıklanmaktadır yüz API hizmetiyle çalışacak şekilde karşılaşabileceğiniz.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: ec3068c013be9f2fa4ff34b1c24596e46e7c5d05
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351928"
---
# <a name="glossary"></a>Sözlük

## <a name="a"></a>A

#### <a name="Attributes"></a>Öznitelikleri

Öznitelikleri isteğe bağlı olarak [algılama](#Detection-Face-Detection) gibi sonuçları [yaş](#Age-Attribute), [cinsiyetiniz](#Gender-Attribute), [baş poz](#Head-Pose-Attribute), [yüz artı](#Facial-Hair-Attribute), [gülen](#Smile-Attribute).
Adresten edinilebilir [algılama](#Detection-Face-Detection) sorgu parametrelerini belirterek API: returnFaceAttributes. Öznitelikleri ek bilgi vermek ilgili seçili [yazıtiplerinin](#Face); ek olarak [kimliği yüz](#Face-ID) ve [dikdörtgen](#Face-Rectangle).

Daha fazla ayrıntı için lütfen Kılavuzu'na bakın [yüz - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="Age-Attribute"></a>Geçerlilik süresi (özniteliği)

Geçerlilik süresi biridir [öznitelikleri](#Attributes) belirli bir yazıtipi yaşını açıklar. Geçerlilik süresi özniteliği isteğe bağlı olarak [algılama](#Detection-Face-Detection) sonuçlanır ve ile denetlenen bir [algılama](#Detection-Face-Detection) returnFaceAttributes parametresini belirterek isteği.

Daha fazla ayrıntı için lütfen Kılavuzu'na bakın [yüz - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="b"></a>B

## <a name="c"></a>C

#### <a name="Candidate"></a>Aday

Adaylar temelde [kimliği](#Identification) sonuçları (örneğin tanımlanan kişi ve algılamaları güvenirlik düzeyini). Aday tarafından temsil edilen [PersonID](#Person-ID) ve [güvenirlik](#Confidence), belirten kişi güvenirlik yüksek düzeyde ile tanımlanır.

Daha fazla ayrıntı için lütfen Kılavuzu'na bakın [yüz - tanımlama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

#### <a name="Confidence"></a>Güven

Güven olduğunu arasında benzerlik ortaya ölçüm [yazıtiplerinin](#Face) veya [kişi](#Person) sayısal değerleri – içinde kullanılan [kimliği](#Identification), ve [ doğrulama](#Verification) Aranan, tanımlanan ve doğrulanmış sonuçları benzerlikler belirtmek için.

Daha fazla ayrıntı için lütfen aşağıdaki kılavuzlara bakın: [yüz - bulma benzer](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237), [yüz - tanımlama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [yüz - doğrulayın](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

## <a name="d"></a>D

#### <a name="Detection-Face-Detection"></a>Algılama/yüz algılama

Yüz algılama görüntülerinde yüzeyleri bulma eylemdir. Kullanıcılar bir görüntüyü karşıya yükleyin veya istekte bir resim URL'si belirtin. Algılanan yazıtipleri ile döndürülen [kimlikleri yüz](#Face-ID) yüz API'sindeki benzersiz bir kimlik belirten. Dikdörtgenler piksel yanı sıra isteğe bağlı görüntüde yüz konumlarda belirtmek [öznitelikleri](#Attributes) gibi her yüz için [yaş](#Age-Attribute), [cinsiyetiniz](#Gender-Attribute), [baş poz ](#Head-Pose-Attribute), [yüz artı](#Facial-Hair-Attribute) ve [gülen](#Smile-Attribute).

Daha fazla ayrıntı için lütfen Kılavuzu'na bakın [yüz - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="e"></a>E

## <a name="f"></a>F

#### <a name="Face"></a>Yüz

Yüz yüz algılanan yazıtipleri ile ilgili API türetilmiş sonuçlar için birleştirilmiş bir terimdir. Sonuç olarak, yüz birleşik kimliği tarafından temsil edilen ([yüz kimliği](#Face-ID)), görüntüleri belirli bir bölgede ([yüz dikdörtgen](#Face-Rectangle)) ve ilgili ek yüz [öznitelikleri](#Face-Attributes-Facial-Attributes), gibi[yaş](#Age-Attribute), [cinsiyetiniz](#Gender-Attribute), [bilinen yerler](#Face-Landmarks-Facial-Landmarks) ve [baş poz](#Head-Pose-Attribute). Ayrıca, yüzler gelen döndürülebilecek [algılama](#Detection-Face-Detection).

Daha fazla ayrıntı için lütfen Kılavuzu'na bakın [yüz - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="Face-API"></a>Yüz API

Yüz API yüz algılama ve tanınması için en gelişmiş algoritmalar sağlayan bir bulut tabanlı bir API'dir. Yüz API ana işlevselliğini iki kategoriye ayrılabilir: yüz [algılama](#Detection-Face-Detection) ile [öznitelikleri](#Face-Attributes-Facial-Attributes)ve yüz [tanıma](#Recognition).

Daha fazla ayrıntı için lütfen aşağıdaki kılavuzlara bakın: [yüz API genel bakış](./Overview.md), [yüz - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [yüz - bulma benzer](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237), [yüz - grup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238), [Yüz - tanımlama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [yüz - doğrulayın](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

#### <a name="Face-Attributes-Facial-Attributes"></a>Yüz öznitelikleri/yüz öznitelikleri

Lütfen bakın [öznitelikleri](#Attributes).

#### <a name="Face-ID"></a>Yüz kimliği

Yüz kimliği türetilir [algılama](#Detection-Face-Detection) sonuçları, bir dizeyi temsil eden bir [yüz](#Face) içinde [yüz API](#Face-API).

Daha fazla ayrıntı için lütfen Kılavuzu'na bakın [yüz - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="Face-Landmarks-Facial-Landmarks"></a>Yüz işaretleri/yüz bilinen yerler

Yer işaretlerini isteğe bağlı olarak [algılama](#Detection-Face-Detection) göz, burun ve ağzınıza (aşağıdaki şekilde gösterilmiştir) gibi anlamsal yüz noktaları sonuçları;. Yer işaretlerini ile denetlenebilir bir [algılama](#Detection-Face-Detection) Boolean numara returnFaceLandmarks isteğiyle. ReturnFaceLandmarks true ayarlanırsa, döndürülen yüzeyleri yer işareti özniteliklere sahip olacaktır.

Daha fazla ayrıntı için lütfen Kılavuzu'na bakın [yüz - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

![HowToDetectFace](./Images/landmarks.1.jpg)

#### <a name="Face-Rectangle"></a>Yüz dikdörtgen

Yüz dikdörtgen türetilir [algılama](#Detection-Face-Detection) bir dik dikdörtgen (sol, üst düzey, genişlik, yükseklik) piksel cinsinden görüntülerinde sonuçları. Sol üst köşesindeki bir [yüz](#Face) (sol, top), genişlik ve yükseklik yanı sıra x yüz boyutlarında gösterir ve y eksenlerinin sırasıyla.

Daha fazla ayrıntı için lütfen Kılavuzu'na bakın [yüz - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="Facial-Hair-Attribute"></a>Yüz artı (özniteliği)

Yüz artı biridir [öznitelikleri](#Attributes) kullanılabilir yüzeyleri yüz artı uzunluğu açıklamak için kullanılır. Yüz artı özniteliği isteğe bağlı olarak [algılama](#Detection-Face-Detection) sonuçlanır ve ile denetlenen bir [algılama](#Detection-Face-Detection) returnFaceAttributes isteğiyle. ReturnFaceAttributes 'facialHair' içeriyorsa, döndürülen yüzeyleri yüz artı özniteliklere sahip olacaktır.

Daha fazla ayrıntı için lütfen Kılavuzu'na bakın [yüz - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="FaceList"></a>FaceList

FaceList koleksiyonudur [PersistedFace](#PersistedFace) ve birimidir [Bul benzer](#Find-Similar). Bir FaceList birlikte bir [FaceList kimliği](#FaceList-ID), diğer öznitelikleri gibi yanı sıra [adı](#Name) ve [kullanıcı verilerini](#UserData-User-Data).

Daha fazla ayrıntı için lütfen aşağıdaki kılavuzlara bakın: [FaceList - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b), [FaceList - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c).

#### <a name="FaceList-ID"></a>FaceList kimliği

FaceList kimliği olan bir tanımlayıcı olarak kullanılan bir kullanıcı tarafından sağlanan dize bir [FaceList](#FaceList). FaceList kimliği abonelik içinde benzersiz olmalıdır.

Daha fazla ayrıntı için lütfen aşağıdaki kılavuzlara bakın: [FaceList - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b), [FaceList - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c).

#### <a name="Find-Similar"></a>Benzer Bul

Bu API, arama/benzer yüzeyleri yazıtipleri, bir koleksiyona bağlı sorgu için kullanılır. Sorgu yüzeyleri ve yüz koleksiyonları olarak temsil [kimlikleri yüz](#Face-ID) veya [FceList kimliği](#FaceList-ID)/[LargeFaceList kimliği](#LargeFaceList-ID) isteği. Sonuçları tarafından temsil edilen Aranan benzer yüzeyleri döndürülür [kimlikleri yüz](#Face-ID) veya [PersistedFace kimlikleri](#PersistedFace-ID).

Daha fazla ayrıntı için lütfen aşağıdaki kılavuzlara bakın: [yüz - bulma benzer](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237), [LargeFaceList - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc), [FaceList - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b).

## <a name="g"></a>G

#### <a name="Gender-Attribute"></a>Cinsiyeti (özniteliği)

Cinsiyeti biridir [öznitelikleri](#Attributes) kullanılabilir yüzeyleri genders açıklamak için kullanılır. Cinsiyeti özniteliği isteğe bağlı olarak [algılama](#Detection-Face-Detection) sonuçlanır ve ile denetlenen bir [algılama](#Detection-Face-Detection) returnFaceAttributes isteğiyle. ReturnfaceAttributes 'cinsiyetiniz' içeriyorsa, döndürülen yüzeyleri cinsiyetiniz özniteliklere sahip olacaktır.

Daha fazla ayrıntı için lütfen Kılavuzu'na bakın [yüz - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="Grouping"></a>Gruplandırma

Yüz gruplandırma yüz benzerlikler göre yazıtipleri koleksiyonu gruplandırmasıdır. Yüz koleksiyonları istek yüz kimliği koleksiyonlarda belirtilir. Gruplandırma sonucunda benzer yüzeyleri olarak gruplanmış [grupları](#Groups), ve diğer bir yüze benzer olmayan yüzeyleri düzensiz grup olarak birlikte birleştirilir. En çok bir olduğundan [düzensiz grup](#Messy-Group) gruplandırma sonuç.

Daha fazla ayrıntı için lütfen Kılavuzu'na bakın [yüz - grup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

#### <a name="Groups"></a>Grupları

Grupları türetilir [gruplandırma](#Grouping) sonuçları. Her grubu benzer yüz, burada yüzeyleri belirtilir tarafından oluşan bir koleksiyon içeren [kimlikleri yüz](#Face-ID).

Daha fazla ayrıntı için lütfen Kılavuzu'na bakın [yüz - grup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

## <a name="h"></a>H

#### <a name="Head-Pose-Attribute"></a>HEAD teşkil (özniteliği)

HEAD poz biridir [öznitelikleri](#Attributes) temsil aşağıdaki şekilde gösterildiği gibi 3B uzaydaki Top, aralık ve yaw açıları göre yönlendirmesini yüz. Değer aralıklarını alma ve öğeler [-180, 180] ve [-90, 90] derece cinsinden yaw. Geçerli sürümde algılama döndürülen aralık değeri her zaman olduğu 0. Head poz özniteliği isteğe bağlı olarak [algılama](#Detection-Face-Detection) sonuçlanır ve ile denetlenen bir [algılama](#Detection-Face-Detection) returnFaceAttributes parametresi tarafından isteği. ReturnFaceAttributes parametresi 'headPose' içeriyorsa, döndürülen yüzeyleri head öznitelikleri neden olacaktır.

Daha fazla ayrıntı için lütfen Kılavuzu'na bakın [yüz - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

![GlossaryHeadPose](./Images/headpose.1.jpg)

## <a name="i"></a>I

#### <a name="Identification"></a>Tanımlama

Bir veya daha fazla yüzeyleri LargePersonGroup/PersonGroup gelen tanımlamak için kimliğidir.
A [PersonGroup](#PersonGroup)/[LargePersonGroup](#LargePersonGroup) koleksiyonudur [kişi](#Person).
Yüzeyleri ve LargePersonGroup/PersonGroup temsil ettiği [kimlikleri yüz](#Face-ID) ve [LargePersonGroup kimlikleri](#LargePersonGroup-ID)/[PersonGroup kimlikleri](#PersonGroup-ID) sırasıyla içinde İstek.
Tanımlanan sonuçlar [adayları](#Candidate), tarafından gösterilen [kişi](#Person) güvenle.
Giriş birden çok yüzeyleri ayrı olarak kabul edilir ve her yüzü tanımlanmış sonucunu gerekir.

> [!NOTE]
> LargePersonGroup/PersonGroup önce kimliği başarıyla Eğitilecek. LargePersonGroup/PersonGroup eğitildi değil, veya eğitim [durum](#Status-Train) başarılı' (yani ' çalışıyor', 'başarısız' veya 'zaman aşımı') 'gibi istek yanıt olan 400 gösterilmez.
> 

Daha fazla ayrıntı için lütfen aşağıdaki kılavuzlara bakın: [yüz - tanımlama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [LargePersonGroup kişi - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40), [LargePersonGroup - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d), [ LargePersonGroup - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4), [PersonGroup kişi - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c), [PersonGroup - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244), [PersonGroup - tren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249).

#### <a name="Is-Identical"></a>IsIdentical

IsIdentical olan bir Boole alanı [doğrulama](#Verification) iki yüz aynı kişiye ait olup olmadığını belirten sonuçları.

Daha fazla ayrıntı için lütfen Kılavuzu'na bakın [yüz - doğrulayın](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

## <a name="j"></a>K

## <a name="k"></a>K

## <a name="l"></a>L

#### <a name="landmarks"></a>Bilinen yerler

Lütfen bakın [yüz işaretleri](#Face-Landmarks-Facial-Landmarks).

#### <a name="LargeFaceList"></a>LargeFaceList

LargeFaceList koleksiyonudur [PersistedFace](#PersistedFace) ve birimidir [Bul benzer](#Find-Similar). Bir LargeFaceList birlikte bir [LargeFaceList kimliği](#LargeFaceList-ID), diğer öznitelikleri gibi yanı sıra [adı](#Name) ve [kullanıcı verilerini](#UserData-User-Data).

Daha fazla ayrıntı için lütfen aşağıdaki kılavuzlara bakın: [LargeFaceList - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc), [LargeFaceList - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a15827cd2de3616c086f2ce), [LargeFaceList - liste yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158db4d2de3616c086f2d6).

#### <a name="LargeFaceList-ID"></a>LargeFaceList kimliği

LargeFaceList kimliği olan bir tanımlayıcı olarak kullanılan bir kullanıcı tarafından sağlanan dize bir [LargeFaceList](#LargeFaceList). LargeFaceList kimliği abonelik içinde benzersiz olmalıdır.

Daha fazla ayrıntı için lütfen aşağıdaki kılavuzlara bakın: [LargeFaceList - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc), [LargeFaceList - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a15827cd2de3616c086f2ce).

#### <a name="LargePersonGroup"></a>LargePersonGroup

LargePersonGroup koleksiyonudur [kişi](#Person) ve birimidir [kimliği](#Identification). Bir LargePersonGroup birlikte bir [LargePersonGroup kimliği](#LargePersonGroup-ID), diğer öznitelikleri gibi yanı sıra [adı](#Name) ve [kullanıcı verilerini](#UserData-User-Data).

Daha fazla ayrıntı için lütfen aşağıdaki kılavuzlara bakın: [LargePersonGroup - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d), [LargePersonGroup - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acebb6ac60f11b48b5a9e), [LargePersonGroup kişi - liste](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adda06ac60f11b48b5aa1).

#### <a name="LargePersonGroup-ID"></a>LargePersonGroup kimliği

LargePersonGroup kimliği olan bir tanımlayıcı olarak kullanılan bir kullanıcı tarafından sağlanan dize bir [LargePersonGroup](#LargePersonGroup). LargePersonGroup kimliği abonelik içinde benzersiz olmalıdır.

Daha fazla ayrıntı için lütfen aşağıdaki kılavuzlara bakın: [LargePersonGroup - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d), [LargePersonGroup - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acebb6ac60f11b48b5a9e).

## <a name="m"></a>M

#### <a name="Messy-Group"></a>Düzensiz grubu

Düzensiz Grup türetilir [gruplandırma](#Grouping) yüzeyleri için başka bir yazıtipi benzer değil içeren sonuçları;. Düzensiz grubundaki her yüz belirtilir [kimliği yüz](#Face-ID).

Daha fazla ayrıntı için lütfen Kılavuzu'na bakın [yüz - grup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

## <a name="n"></a>N

#### <a name="name-person"></a>Adı (kişi)

Ad olduğu için kullanıcı dostu tanımlayıcı bir dize [kişi](#Person). Farklı [kişi kimliği](#Person-ID), kişilerin adını grup içinde çoğaltılabilir.

Daha fazla ayrıntı için lütfen aşağıdaki kılavuzlara bakın: [LargePersonGroup kişi - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40), [LargePersonGroup kişi - alma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599add376ac60f11b48b5aa0), [PersonGroup kişi - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c), [ PersonGroup kişi - alma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523f).

#### <a name="Name"></a>Adı (LargePersonGroup/PersonGroup)

Adıdır ayrıca için kullanıcı dostu tanımlayıcı bir dize [LargePersonGroup](#LargePersonGroup)/[PersonGroup](#PersonGroup). Farklı [LargePersonGroup kimliği](#LargePersonGroup-ID)/[PersonGroup kimliği](#PersonGroup-ID), LargePersonGroups/PersonGroups adı bir abonelikte çoğaltılabilir.

Daha fazla ayrıntı için lütfen aşağıdaki kılavuzlara bakın: [LargePersonGroup - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d), [LargePersonGroup - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acebb6ac60f11b48b5a9e), [PersonGroup - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244), [ PersonGroup - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395246).

## <a name="o"></a>O

## <a name="p"></a>P

#### <a name="PersistedFace"></a>PersistedFace

PersistedFace yüz API'sindeki bir veri yapısıdır. PersistedFace ile birlikte gelen bir [PersistedFace kimliği](#PersistedFace-ID), diğer öznitelikleri gibi yanı sıra [adı](#Name), ve [kullanıcı verilerini](#UserData-User-Data).

Daha fazla ayrıntı için lütfen aşağıdaki kılavuzlara bakın: [LargeFaceList - eklemek yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [FaceList - eklemek yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [LargePersonGroup kişi - eklemek yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42), [ PersonGroup kişi - eklemek yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

#### <a name="Person-ID"></a>Kişi kimliği

Kişinin kimliği üretilir olduğunda bir [PersistedFace](#PersistedFace) başarıyla oluşturuldu. Bu yüz temsil eden bir dize oluşturulur [yüz API](#Face-API).

Daha fazla ayrıntı için lütfen aşağıdaki kılavuzlara bakın: [LargeFaceList - eklemek yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [FaceList - eklemek yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [LargePersonGroup kişi - eklemek yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42), [ PersonGroup kişi - eklemek yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

#### <a name="Person"></a>Kişi

Kişi yüz API yönetilen bir veri yapısıdır. Kişi ile birlikte gelen bir [kişi kimliği](#Person-ID), diğer öznitelikleri gibi yanı sıra [adı](#Name), koleksiyonu [PersistedFace](#PersistedFace), ve [kullanıcı verilerini](#UserData-User-Data).

Daha fazla ayrıntı için lütfen aşağıdaki kılavuzlara bakın: [LargePersonGroup kişi - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40), [LargePersonGroup kişi - alma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599add376ac60f11b48b5aa0), [PersonGroup kişi - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c), [ PersonGroup kişi - alma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523f).

#### <a name="Person-ID"></a>Kişi kimliği

Kişinin kimliği üretilir olduğunda bir [kişi](#Person) başarıyla oluşturuldu. Bu kişinin temsil eden bir dize oluşturulur [yüz API](#Face-API).

Daha fazla ayrıntı için lütfen aşağıdaki kılavuzlara bakın: [LargePersonGroup kişi - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40), [LargePersonGroup kişi - alma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599add376ac60f11b48b5aa0), [PersonGroup kişi - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c), [ PersonGroup kişi - alma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523f).

#### <a name="PersonGroup"></a>PersonGroup

PersonGroup koleksiyonudur [kişi](#Person) ve birimidir [kimliği](#Identification). Bir PersonGroup birlikte bir [PersonGroup kimliği](#PersonGroup-ID), diğer öznitelikleri gibi yanı sıra [adı](#Name) ve [kullanıcı verilerini](#UserData-User-Data).

Daha fazla ayrıntı için lütfen aşağıdaki kılavuzlara bakın: [PersonGroup - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244), [PersonGroup - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395246), [PersonGroup kişi - liste](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395241).

#### <a name="PersonGroup-ID"></a>PersonGroup kimliği

PersonGroup kimliği olan bir tanımlayıcı olarak kullanılan bir kullanıcı tarafından sağlanan dize bir [PersonGroup](#PersonGroup). Grup Kimliği abonelik içinde benzersiz olmalıdır.

Daha fazla ayrıntı için lütfen aşağıdaki kılavuzlara bakın: [PersonGroup - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244), [PersonGroup - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395246).

#### <a name="pose-attribute"></a>(Öznitelik) neden

Lütfen bakın [Head teşkil](#Head-Pose-Attribute).

## <a name="q"></a>Q

## <a name="r"></a>R

#### <a name="Recognition"></a>Tanıma

Tanıma olan popüler uygulama alanında yüz teknolojileri için gibi [Benzerini Bul](#Find-Similar), [gruplandırma](#Grouping), [tanımla](#Identification),[iki doğrulama bakarken aynı ya da değil ](#Verification).

Daha fazla ayrıntı için lütfen aşağıdaki kılavuzlara bakın: [yüz - bulma benzer](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237), [yüz - grup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238), [yüz - tanımlama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [yüz - doğrulayın](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

#### <a name="rectangle-face"></a>Dikdörtgen (yüz)

Lütfen bakın [yüz dikdörtgen](#Face-Rectangle).

## <a name="s"></a>S

#### <a name="Smile-Attribute"></a>(Öznitelik) smile

Gülümseme gönderme biridir [öznitelikleri](#Attributes) kullanılabilir yüzeyleri gülümseme gönderme ifade açıklamak için kullanılır. Gülümseme gönderme özniteliği isteğe bağlı olarak [algılama](#Detection-Face-Detection) sonuçlanır ve ile denetlenen bir [algılama](#Detection-Face-Detection) returnFaceAttributes isteğiyle. ReturnFaceAttributes 'smile' içeriyorsa, döndürülen yüzeyleri gülümseme gönderme özniteliklere sahip olacaktır.

Daha fazla ayrıntı için lütfen Kılavuzu'na bakın [yüz - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="similar-face-searching"></a>Benzer Yüz Arama

Lütfen bakın [Bul benzer](#Find-Similar).

#### <a name="Status-Train"></a>Durum (Train)

Durumudur yordamı için tanımlamak için kullanılan bir dize [eğitim LargeFaceList/LargePersonGroups/PersonGroups](#Train), '' çalışıyor' notstarted de dahil olmak üzere', 'başarılı', 'failed'.

Daha fazla ayrıntı için lütfen Kılavuzu'na bakın [LargeFaceList - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158422d2de3616c086f2d1), [LargePersonGroup - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4), [PersonGroup - tren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249).

#### <a name="subscription-key"></a>Abonelik anahtarı

Abonelik, herhangi bir yazıtipi API'yi çağırmak için bir sorgu dizesi parametresi belirtmeniz gerekir dize bir anahtardır. Microsoft Bilişsel hizmetler portalına oturum açtıktan sonra abonelik anahtarı My abonelikler sayfasında bulunabilir. Her abonelikle ilişkili iki anahtar olacaktır: bir birincil anahtar ve ikincil bir anahtar. Her ikisi de aynı API'sini çağırmak için kullanılabilir. My abonelikler sayfasında herhangi bir zamanda abonelik anahtarları yeniden ve Abonelik anahtarları güvenli tutmak gerekir.

## <a name="t"></a>T

#### <a name="Train"></a>Tren (LargePersonGroup/LargeFaceList/PersonGroup)

Bu API önceden işlemek için kullanılan [LargeFaceList](#LargeFaceList)/[LargePersonGroup](#LargePersonGroup)/[PersonGroup](#PersonGroup) emin olmak için [Bul Benzer](#Find-Similar)/[kimliği](#Identification) performans. Eğitim işletilen değil, veya [eğitim durumu](#Status-Train) başarılı olarak bu PersonGroup kimliği yüklenememesine neden olacak gösterilmez.

Daha fazla ayrıntı için lütfen aşağıdaki kılavuzlara bakın: [LargeFaceList - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158422d2de3616c086f2d1), [LargePersonGroup - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4), [PersonGroup - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249), [yüz - tanımlama ](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

## <a name="u"></a>U

#### <a name="UserData-User-Data"></a>UserData/kullanıcı verileri

Kullanıcı verilerinin olduğundan ile ilişkili ek bilgileri [kişi](#Person) ve [PersonGroup](#PersonGroup)/[LargePersonGroup](#LargePersonGroup). Kullanıcı verileri, veri kullanmak için anlamak ve unutmayın daha kolay hale getirmek için kullanıcılar tarafından ayarlanır.

Daha fazla ayrıntı için lütfen aşağıdaki kılavuzlara bakın: [LargePersonGroup - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d), [LargePersonGroup - güncelleştirme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acfc83a7b9412a4d53f3f), [LargePersonGroup kişi - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40), [LargePersonGroup kişi - güncelleştirme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ade043a7b9412a4d53f41), [PersonGroup - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244), [PersonGroup - güncelleştirme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524a), [PersonGroup kişi - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c), [ PersonGroup kişi - güncelleştirme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395242).

## <a name="v"></a>V

#### <a name="Verification"></a>Doğrulama

Bu API, iki yüz aynı olup olmadığını doğrulamak için kullanılır. Her iki yüz kimlikleri istekte yüz olarak temsil edilir. Sonuçlarını içeren bir Boole alanı doğrulandı ([isIdentical](#Is-Identical)) aynı true ise belirten ve bir sayı alanında ([güvenirlik](#Confidence)) güvenirlik düzeyini gösteren.

Daha fazla ayrıntı için lütfen Kılavuzu'na bakın [yüz - doğrulayın](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

## <a name="w"></a>W

## <a name="x"></a>X

## <a name="y"></a>E

## <a name="z"></a>Z
