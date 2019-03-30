---
title: Sözlük - yüz tanıma API'si hizmeti
titleSuffix: Azure Cognitive Services
description: Sözlükte, Yüz Tanıma API’si Hizmeti ile çalışırken karşılaşabileceğiniz terimler açıklanmaktadır.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: d627c3c4419affa0d71cdb23df945c96d9fd7585
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58652442"
---
# <a name="glossary"></a>Sözlük

## <a name="a"></a>A

#### <a name="attributes"></a>Öznitelikler

Öznitelikler, gibi algılanabilir isteğe bağlı yüz özellikleri olan [yaş](#age-attribute), [cinsiyet](#gender-attribute), [baş poz](#head-pose-attribute), [bıyığın](#facial-hair-attribute)ve [gülümseyin](#smile-attribute). Algılama API'si bunlar alınabilir belirterek _returnFaceAttributes_ sorgu parametresi.

Yüz öznitelikleri tam bir listesi için başvuru belgelerine bakın: [Yüz tanıma - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="age-attribute"></a>Yaş (öznitelik)

Geçerlilik süresi aşağıdakilerden birini olan [öznitelikleri](#attributes) , belirli bir yüzün yaşını açıklar. Yaş öznitelik algılama sonuçları isteğe bağlıdır ve algılama istekle returnFaceAttributes parametresi belirtilerek denetlenebilir.

Daha fazla bilgi için başvuru belgelerine bakın: [Yüz tanıma - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="c"></a>C

#### <a name="candidate"></a>Aday

Aday niteliği aslında [kimliği](#identification) sonuçları (örn, tanımlanan kişiler ve algılamalar güven düzeyini). Bir aday tarafından temsil edilen [Personıd](#person-id) ve [güvenle](#confidence), belirten kişi yüksek bir güven düzeyinde ile tanımlanır.

Daha fazla bilgi için başvuru belgelerine bakın: [Yüz tanıma - tanımlamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

#### <a name="confidence"></a>Güven

Güven: arasında benzerlik ortaya çıkaran bir ölçü [yüzleri](#face) veya [kişi](#person) sayısal değerleri – kullanılan [kimliği](#identification), ve [ doğrulama](#verification) benzerlikler Aranan, tanımlanan ve doğrulanmış sonuçları göstermek için.

Daha fazla bilgi için başvuru belgelerine bakın: [Yüz tanıma - benzer Bul](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237), [yüz tanıma - tanımlamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [yüz tanıma - doğrulama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

## <a name="d"></a>D

#### <a name="detectionface-detection"></a>Algılama/yüz algılama

Yüz algılama, yüz görüntüleri bulma eylemdir. Kullanıcılar, bir görüntüyü karşıya yükleme veya istekte bir resim URL'si belirtin. Algılanan yüzeylere ile döndürülür [kimlikleri yüz](#face-id) belirten yüz tanıma API'si, bir benzersiz kimliği. Dikdörtgenler piksel yanı sıra isteğe bağlı olarak görüntüde yüz konumları belirtin [öznitelikleri](#attributes) gibi her yüz için [yaş](#age-attribute), [cinsiyet](#gender-attribute), [baş poz ](#head-pose-attribute), [bıyığın](#facial-hair-attribute) ve [gülümseyen](#smile-attribute).

Daha fazla bilgi için başvuru belgelerine bakın: [Yüz tanıma - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="e"></a>E

#### <a name="emotion-attribute"></a>Duygu tanıma (öznitelik)

Duygu tanıma biridir [yüz öznitelikleri](#attributes). Sorgulandığında, duyguları ve belirli yüz için kendi algılama güvenle listesini döndürür. Güven puanları normalleştirilmiş: puanları tüm duyguları arasında en fazla bir ekleyeceksiniz. Döndürülen duyguları olan mutluluk, üzüntü, nötr, kızgınlık, küçümseme, iğrenme, şaşkınlık ve Korkmayın.

Daha fazla bilgi için başvuru belgelerine bakın: [Yüz tanıma - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="f"></a>C

#### <a name="face"></a>Yüz

Yüz tanıma, yüz tanıma API'si ile algılanan yüzeylere ilgili türetilen sonuçları için birleştirilmiş bir terimdir. Sonuç olarak, yüz tanıma tarafından birleştirilmiş bir kimlik gösterilir ([Face ID](#face-id)), görüntüleri belirli bir bölgede ([yüz dikdörtgeni](#face-rectangle)) ve ek yüz tanıma ile ilgili özellikler gibi [yaş](#age-attribute), [cinsiyet](#gender-attribute), yer işareti ve [baş poz](#head-pose-attribute). Ayrıca, yüz algılama döndürülebilir.

Daha fazla bilgi için başvuru belgelerine bakın: [Yüz tanıma - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="face-api"></a>Yüz Tanıma API'si

Yüz tanıma API'si, yüz algılama ve tanıma için en gelişmiş algoritmalar sağlayan bir bulut tabanlı bir API'dir. Yüz tanıma API'si ana işlevselliğini iki kategoriye ayrılabilir: yüz öznitelikleri ve yüz algılama [tanıma](#recognition).

Daha fazla bilgi için başvuru belgelerine bakın: [Yüz tanıma API'sine genel bakış](./Overview.md), [yüz tanıma - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [yüz tanıma - benzer Bul](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237), [yüz tanıma - grup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238), [yüz tanıma - tanımlamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [ Yüz tanıma - doğrulama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

#### <a name="face-attributesfacial-attributes"></a>Yüz öznitelikleri/yanı sıra yüz öznitelikleri

Lütfen [öznitelikleri](#attributes).

#### <a name="face-id"></a>Yüz kimliği

Face ID bir dizeyi temsil eden algılama sonuçlarından türetilmiş bir [yüz](#face) içinde [yüz tanıma API'si](#face-api).

Daha fazla bilgi için başvuru belgelerine bakın: [Yüz tanıma - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="face-landmarksfacial-landmarks"></a>Yüz tanıma yer işareti/yanı sıra yüz yer işareti

Yer işareti algılama sonuçları isteğe bağlıdır; gözler ve burun ağız (aşağıdaki şekilde gösterilen) gibi anlamsal yüz noktaları olduğu. Yer işareti algılama istekle Boole sayı returnFaceLandmarks tarafından denetlenebilir. ReturnFaceLandmarks true ayarlanırsa, döndürülen yüzleri yer işareti özniteliğe sahip olacaktır.

Daha fazla bilgi için başvuru belgelerine bakın: [Yüz tanıma - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

![HowToDetectFace](./Images/landmarks.1.jpg)

#### <a name="face-rectangle"></a>Yüz dikdörtgeni

Yüz dikdörtgeni piksel cinsinden görüntü içinde dikey bir dikdörtgen (sol, üst düzey, genişlik, yükseklik) olan algılama sonuçlardan türetilir. Sol üst köşesinde bir [yüz](#face) (sol, üst), genişlik ve yükseklik yanı sıra x nominal boyutlarının gösterir ve y eksenlerinin sırasıyla.

Daha fazla bilgi için başvuru belgelerine bakın: [Yüz tanıma - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="facial-hair-attribute"></a>Bıyığın (öznitelik)

Bıyığın biridir [öznitelikleri](#attributes) kullanılabilir yüzleri bıyığın uzunluğunu tanımlamak için kullanılır. Bıyığın öznitelik algılama sonuçları isteğe bağlıdır ve algılama istekle returnFaceAttributes tarafından denetlenebilir. ReturnFaceAttributes 'facialHair' içeriyorsa, döndürülen yüzleri bıyığın özniteliğe sahip olacaktır.

Daha fazla bilgi için başvuru belgelerine bakın: [Yüz tanıma - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="facelist"></a>FaceList

FaceList, koleksiyonudur [PersistedFace](#persistedface) ve birimidir [Bul benzer](#find-similar). Bir FaceList birlikte bir [FaceList kimliği](#facelist-id), yanı sıra adı ve kullanıcı verileri gibi diğer öznitelikleri.

Daha fazla bilgi için başvuru belgelerine bakın: [FaceList - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b), [FaceList - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c).

#### <a name="facelist-id"></a>FaceList kimliği

FaceList kimliği olan bir tanımlayıcı olarak kullanılan bir kullanıcı tarafından sağlanan dize bir [FaceList](#facelist). FaceList kimliği abonelik içinde benzersiz olmalıdır.

Daha fazla bilgi için başvuru belgelerine bakın: [FaceList - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b), [FaceList - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c).

#### <a name="find-similar"></a>Benzer bulun

Bu API, benzer yüzlerden yüzleri üzerindeki bir koleksiyona göre arama/sorgu için kullanılır. Sorgu yüzleri ve yüz koleksiyonları olarak gösterilir [kimlikleri yüz](#face-id) veya [FceList kimliği](#facelist-id)/[LargeFaceList kimliği](#largefacelist-id) istek. Sonuçları tarafından temsil edilen Aranan benzer yüzleri döndürülür [kimlikleri yüz](#face-id) veya PersistedFace kimlikleri.

Daha fazla bilgi için başvuru belgelerine bakın: [Yüz tanıma - benzer Bul](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237), [LargeFaceList - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc), [FaceList - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b).

## <a name="g"></a>G

#### <a name="gender-attribute"></a>Cinsiyet (öznitelik)

Cinsiyet biridir [öznitelikleri](#attributes) kullanılabilir dikdörtgenlerini cinsiyetleri tanımlamak için kullanılır. Cinsiyet öznitelik algılama sonuçları isteğe bağlıdır ve algılama istekle returnFaceAttributes tarafından denetlenebilir. ReturnfaceAttributes 'cinsiyet' içeriyorsa, döndürülen yüzleri cinsiyet özniteliğe sahip olacaktır.

Daha fazla bilgi için başvuru belgelerine bakın: [Yüz tanıma - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="grouping"></a>Gruplandırma

Yüz gruplama yüz benzerlikler göre yüz koleksiyonu gruplandırmasıdır. Yüz tanıma koleksiyonları istek yüz kimliği koleksiyonlar gösterilir. Gruplandırma sonucu olarak, benzer yüzlerden olarak gruplanmış [grupları](#groups), ve herhangi bir yüz için benzer olmayan yüzleri kullanmak istemiyor grup olarak birlikte birleştirilir. En fazla bir tane olduğunu [kullanmak istemiyor grubu](#messy-group) gruplandırma sonuç.

Daha fazla bilgi için başvuru belgelerine bakın: [Yüz tanıma - grup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

#### <a name="groups"></a>Gruplar

Grupları türetilir [gruplandırma](#grouping) sonuçları. Her grup, burada yüzleri gösterilen, benzer yüzlerden oluşan bir koleksiyon içeren [kimlikleri yüz](#face-id).

Daha fazla bilgi için başvuru belgelerine bakın: [Yüz tanıma - grup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

## <a name="h"></a>H

#### <a name="head-pose-attribute"></a>HEAD (öznitelik) konusunda sizi uyarmayı

HEAD poz biridir [öznitelikleri](#attributes) temsil aşağıdaki şekilde gösterildiği gibi yönlendirme 3B alanda Top, aralık ve yaw açıları göre yüz. Değer aralıkları alma ve derece cinsinden olan [-180, 180] ve [-90, 90] yaw. Geçerli sürümde, her zaman algılamadan döndürülen aralık değer olan 0. Baş poz öznitelik algılama sonuçları isteğe bağlıdır ve algılama istekle returnFaceAttributes parametresi tarafından denetlenebilir. ReturnFaceAttributes parametresi 'headPose' içeriyorsa, döndürülen yüz öznitelikleri konusunda sizi uyarmayı baş sahip olur.

Daha fazla bilgi için başvuru belgelerine bakın: [Yüz tanıma - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

![GlossaryHeadPose](./Images/headpose.1.jpg)

## <a name="i"></a>I

#### <a name="identification"></a>Tanımlama

Kimlik, bir veya daha fazla yüzleri LargePersonGroup/PersonGroup belirlemektir.
A [PersonGroup](#persongroup)/[LargePersonGroup](#largepersongroup) koleksiyonudur [kişiler](#person).
Yüzleri ve LargePersonGroup/PersonGroup temsil edilir [kimlikleri yüz](#face-id) ve [LargePersonGroup kimlikleri](#largepersongroup-id)/[PersonGroup kimlikleri](#persongroup-id) sırasıyla içinde İstek.
Tanımlanan sonuçlar [adayları](#candidate), tarafından gösterilen [kişiler](#person) güvenle.
Birden çok giriş yüzlerini ayrı olarak kabul edilir ve her yüz tanımlanan sonucunu sahip olur.

> [!NOTE]
> Önce tanımlama LargePersonGroup/PersonGroup başarıyla eğitim verilmelidir. LargePersonGroup/PersonGroup eğitim almamış ise veya eğitim [durumu](#status-train) istek yanıt 400 '(yani ' çalışıyor', 'başarısız' veya 'timeout')'de başarılı olarak' gösterilmez.
> 

Daha fazla bilgi için başvuru belgelerine bakın: [Yüz tanıma - tanımlamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [LargePersonGroup kişi - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40), [LargePersonGroup - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d), [LargePersonGroup - eğitme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4), [PersonGroup Kişi - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c), [PersonGroup - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244), [PersonGroup - eğitme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249).

#### <a name="isidentical"></a>IsIdentical

IsIdentical olan Boolean alanı [doğrulama](#verification) iki yüzün aynı insana ait olup olmadığını belirten bir sonuç.

Daha fazla bilgi için başvuru belgelerine bakın: [Yüz tanıma - doğrulama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

## <a name="l"></a>L

#### <a name="landmarks"></a>Yer işareti

Lütfen yüz yer işareti görürsünüz.

#### <a name="largefacelist"></a>LargeFaceList

LargeFaceList, koleksiyonudur [PersistedFace](#persistedface) ve birimidir [Bul benzer](#find-similar). Bir LargeFaceList birlikte bir [LargeFaceList kimliği](#largefacelist-id), yanı sıra adı ve kullanıcı verileri gibi diğer öznitelikleri.

Daha fazla bilgi için başvuru belgelerine bakın: [LargeFaceList - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc), [LargeFaceList - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a15827cd2de3616c086f2ce), [LargeFaceList - liste yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158db4d2de3616c086f2d6).

#### <a name="largefacelist-id"></a>LargeFaceList kimliği

LargeFaceList kimliği olan bir tanımlayıcı olarak kullanılan bir kullanıcı tarafından sağlanan dize bir [LargeFaceList](#largefacelist). LargeFaceList kimliği abonelik içinde benzersiz olmalıdır.

Daha fazla bilgi için başvuru belgelerine bakın: [LargeFaceList - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc), [LargeFaceList - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a15827cd2de3616c086f2ce).

#### <a name="largepersongroup"></a>LargePersonGroup

LargePersonGroup, koleksiyonudur [kişiler](#person) ve birimidir [kimliği](#identification). Bir LargePersonGroup birlikte bir [LargePersonGroup kimliği](#largepersongroup-id), yanı sıra adı ve kullanıcı verileri gibi diğer öznitelikleri.

Daha fazla bilgi için başvuru belgelerine bakın: [LargePersonGroup - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d), [LargePersonGroup - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acebb6ac60f11b48b5a9e), [LargePersonGroup kişi - listesinde](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adda06ac60f11b48b5aa1).

#### <a name="largepersongroup-id"></a>LargePersonGroup kimliği

LargePersonGroup kimliği olan bir tanımlayıcı olarak kullanılan bir kullanıcı tarafından sağlanan dize bir [LargePersonGroup](#largepersongroup). LargePersonGroup kimliği abonelik içinde benzersiz olmalıdır.

Daha fazla bilgi için başvuru belgelerine bakın: [LargePersonGroup - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d), [LargePersonGroup - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acebb6ac60f11b48b5a9e).

## <a name="m"></a>Milyon

#### <a name="messy-group"></a>İşleyerek grubu

İşleyerek grubu türetilen [gruplandırma](#grouping) değil herhangi bir yüz için benzer yüzleri içeren sonuçları;. Her yüz kullanmak istemiyor grubundaki belirtilir [yüz kimliği](#face-id).

Daha fazla bilgi için başvuru belgelerine bakın: [Yüz tanıma - grup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

## <a name="n"></a>N

#### <a name="name-person"></a>Ad (kişi)

İçin kullanımı kolay tanımlayıcı bir dize addır [kişi](#person). Farklı [kişinin kimliği](#person-id), bir grup kişilerin adını çoğaltılabilir.

Daha fazla bilgi için başvuru belgelerine bakın: [LargePersonGroup kişi - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40), [LargePersonGroup kişi - alma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599add376ac60f11b48b5aa0), [PersonGroup kişi - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c), [PersonGroup kişi - alma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523f).

#### <a name="name-largepersongrouppersongroup"></a>Ad (LargePersonGroup/PersonGroup)

Adı da olduğu için kullanımı kolay tanımlayıcı bir dize [LargePersonGroup](#largepersongroup)/[PersonGroup](#persongroup). Farklı [LargePersonGroup kimliği](#largepersongroup-id)/[PersonGroup kimliği](#persongroup-id), bir abonelikte LargePersonGroups/kişi adını çoğaltılabilir.

Daha fazla bilgi için başvuru belgelerine bakın: [LargePersonGroup - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d), [LargePersonGroup - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acebb6ac60f11b48b5a9e), [PersonGroup - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244), [PersonGroup - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395246).

## <a name="p"></a>P

#### <a name="persistedface"></a>PersistedFace

PersistedFace, yüz tanıma API'si, bir veri yapısıdır. PersistedFace PersistedFace kimliği yanı sıra adı ve kullanıcı verileri gibi diğer öznitelikleri ile birlikte gelir.

Daha fazla bilgi için başvuru belgelerine bakın: [LargeFaceList - yüz tanıma Ekle](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [FaceList - yüz Ekle](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [LargePersonGroup kişi - yüz tanıma eklemek](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42), [PersonGroup kişi - yüz tanıma eklemek](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

#### <a name="person-id"></a>Kişinin kimliği

Kişinin kimliği üretilir olduğunda bir [PersistedFace](#persistedface) başarıyla oluşturuldu. Bu bir yüz temsil eden bir dize oluşturulur [yüz tanıma API'si](#face-api).

Daha fazla bilgi için başvuru belgelerine bakın: [LargeFaceList - yüz tanıma Ekle](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [FaceList - yüz Ekle](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [LargePersonGroup kişi - yüz tanıma eklemek](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42), [PersonGroup kişi - yüz tanıma eklemek](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

#### <a name="person"></a>Kişi

Kişi, yüz tanıma API'si yönetilen bir veri yapısıdır. Kişi ile birlikte gelen bir [kişinin kimliği](#person-id), adı, koleksiyonu gibi diğer öznitelikler yanı sıra [PersistedFace](#persistedface)ve kullanıcı verileri.

Daha fazla bilgi için başvuru belgelerine bakın: [LargePersonGroup kişi - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40), [LargePersonGroup kişi - alma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599add376ac60f11b48b5aa0), [PersonGroup kişi - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c), [PersonGroup kişi - alma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523f).

#### <a name="person-id"></a>Kişinin kimliği

Kişinin kimliği üretilir olduğunda bir [kişi](#person) başarıyla oluşturuldu. Bu kişinin temsil eden bir dize oluşturulur [yüz tanıma API'si](#face-api).

Daha fazla bilgi için başvuru belgelerine bakın: [LargePersonGroup kişi - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40), [LargePersonGroup kişi - alma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599add376ac60f11b48b5aa0), [PersonGroup kişi - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c), [PersonGroup kişi - alma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523f).

#### <a name="persongroup"></a>PersonGroup

PersonGroup, koleksiyonudur [kişiler](#person) ve birimidir [kimliği](#identification). Bir PersonGroup birlikte bir [PersonGroup kimliği](#persongroup-id), yanı sıra adı ve kullanıcı verileri gibi diğer öznitelikleri.

Daha fazla bilgi için başvuru belgelerine bakın: [PersonGroup - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244), [PersonGroup - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395246), [PersonGroup kişi - listesinde](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395241).

#### <a name="persongroup-id"></a>PersonGroup kimliği

PersonGroup kimliği olan bir tanımlayıcı olarak kullanılan bir kullanıcı tarafından sağlanan dize bir [PersonGroup](#persongroup). Grup Kimliği abonelik içinde benzersiz olmalıdır.

Daha fazla bilgi için başvuru belgelerine bakın: [PersonGroup - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244), [PersonGroup - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395246).

#### <a name="pose-attribute"></a>Neden (öznitelik)

Lütfen [baş konusunda sizi uyarmayı](#head-pose-attribute).

## <a name="r"></a>R

#### <a name="recognition"></a>Tanıma

Tanıma olan popüler uygulama alan yüz teknolojileri gibi [Bul benzer](#find-similar), [gruplandırma](#grouping), [tanımla](#identification),[iki doğrulama yüzler aynı olmadığını ](#verification).

Daha fazla bilgi için başvuru belgelerine bakın: [Yüz tanıma - benzer Bul](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237), [yüz tanıma - grup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238), [yüz tanıma - tanımlamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [yüz tanıma - doğrulama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

#### <a name="rectangle-face"></a>Dikdörtgen (yüz)

Lütfen [yüz dikdörtgeni](#face-rectangle).

## <a name="s"></a>S

#### <a name="similar-face-searching"></a>Benzer Yüz Arama

Lütfen [Bul benzer](#find-similar).

#### <a name="smile-attribute"></a>Gülümseyin (öznitelik)

Gülümseme Gönder, biridir [öznitelikleri](#attributes) kullanılabilir dikdörtgenlerini gülümseme ifade tanımlamak için kullanılır. Gülümseme Gönder öznitelik algılama sonuçları isteğe bağlıdır ve algılama istekle returnFaceAttributes tarafından denetlenebilir. ReturnFaceAttributes 'gülümseyin' içeriyorsa, döndürülen yüzleri gülümseme özniteliğe sahip olacaktır.

Daha fazla bilgi için başvuru belgelerine bakın: [Yüz tanıma - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="snapshot"></a>Anlık görüntü

Anlık görüntü belirli yüz veri türleri için geçici bir uzak depolamadır. Pano verileri bir abonelikten diğerine kopyalamak için bir tür olarak işlev görür. İlk kullanıcı "verilerin bir anlık görüntüsünü kaynak aboneliği alır" ve ardından ", hedef abonelikte yeni bir veri nesnesi uygulandıkları". 

Daha fazla ayrıntı için [yüz Geçiş Kılavuzu](./face-api-how-to-topics/how-to-migrate-face-data.md) yanı sıra [anlık görüntü - Al](/rest/api/cognitiveservices/face/snapshot/take) ve [anlık görüntüsü - uygulama](/rest/api/cognitiveservices/face/snapshot/apply) başvuru belgeleri (REST).

#### <a name="status-train"></a>Durum (eğitme)

Eğitim LargeFaceList/LargePersonGroups / 'notstarted', 'çalışıyor' dahil olmak üzere belirlenmiştir için yordam tanımlamak için kullanılan bir dize 'başarılı', 'başarısız' durumudur.

Daha fazla bilgi için başvuru belgelerine bakın: [LargeFaceList - eğitme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158422d2de3616c086f2d1), [LargePersonGroup - eğitme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4), [PersonGroup - eğitme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249).

#### <a name="subscription-key"></a>Abonelik anahtarı

Abonelik anahtarı herhangi bir yüz tanıma API'sini çağırmak için bir sorgu dizesi parametresi belirtmeniz gereken bir dizedir. Microsoft Bilişsel Hizmetleri portalında oturum açtıktan sonra abonelik anahtarını Aboneliklerim sayfasında bulunabilir. Her bir aboneliği ile ilişkili iki anahtar olacaktır: bir birincil anahtar ve ikincil bir anahtar. Her ikisi de aynı şekilde API'yi çağırmak için kullanılabilir. Abonelik anahtarları güvenli tutmak gereken ve Abonelik anahtarları Aboneliklerim sayfasından dilediğiniz zaman yeniden oluşturabilirsiniz.

## <a name="t"></a>P

#### <a name="train-largefacelistlargepersongrouppersongroup"></a>Train (LargeFaceList/LargePersonGroup/PersonGroup)

Bu API, önceden işlemek için kullanılan [LargeFaceList](#largefacelist)/[LargePersonGroup](#largepersongroup)/[PersonGroup](#persongroup) emin olmak için [bulun Benzer](#find-similar)/[kimliği](#identification) performans. Eğitim işletilen değil, ya da [eğitim durumu](#status-train) başarılı olarak bu PersonGroup kimliği hataya neden gösterilmez.

Daha fazla bilgi için başvuru belgelerine bakın: [LargeFaceList - eğitme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158422d2de3616c086f2d1), [LargePersonGroup - eğitme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4), [PersonGroup - eğitme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249), [yüz tanıma - tanımlamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

## <a name="u"></a>U

#### <a name="userdatauser-data"></a>UserData/kullanıcı verileri

Kullanıcı verilerinin fazladan bilgi ile ilişkili olduğundan [kişi](#person) ve [PersonGroup](#persongroup)/[LargePersonGroup](#largepersongroup). Kullanıcı verileri, veri kullanmak için anlamak ve unutmayın daha kolay hale getirmek için kullanıcılar tarafından ayarlanır.

Daha fazla bilgi için başvuru belgelerine bakın: [LargePersonGroup - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d), [LargePersonGroup - güncelleştirme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acfc83a7b9412a4d53f3f), [LargePersonGroup kişi - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40), [LargePersonGroup kişi - güncelleştirme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ade043a7b9412a4d53f41), [PersonGroup - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244), [PersonGroup - güncelleştirme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524a), [PersonGroup kişi - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c), [PersonGroup kişi - güncelleştirme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395242).

## <a name="v"></a>V

#### <a name="verification"></a>Doğrulama

Bu API, iki yüzün aynı olup olmadığını doğrulamak için kullanılır. Her iki yüz, istekte kimlikleri yüz olarak temsil edilir. Sonuçları içeren aynı true ise belirten bir Boole alanı (isIdentical) ve bir sayı alanında doğrulandı ([güvenle](#confidence)) güven düzeyini gösteren.

Daha fazla bilgi için başvuru belgelerine bakın: [Yüz tanıma - doğrulama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).
