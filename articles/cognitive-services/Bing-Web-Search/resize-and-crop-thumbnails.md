---
title: Yeniden boyutlandırma ve görüntüyü küçük resim - Bing Web araması API'si kırpma
titleSuffix: Azure Cognitive Services
description: Yeniden boyutlandırma ve Bing arama API'leri tarafından sağlanan küçük resimleri kırpma hakkında bilgi edinin.
services: cognitive-services
author: aahill
manager: nitinme
ms.assetid: 05A08B01-89FF-4781-AFE7-08DA92F25047
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: article
ms.date: 07/08/2019
ms.author: aahi
ms.openlocfilehash: 6a5b2dada254a0bfc7fa60172f56221ba67ad279
ms.sourcegitcommit: 10251d2a134c37c00f0ec10e0da4a3dffa436fb3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2019
ms.locfileid: "67867969"
---
# <a name="resize-and-crop-thumbnail-images"></a>Yeniden boyutlandırma ve küçük resim görüntüleri kırpma

Bing arama API'leri bazı yanıtlardan masaüstünün yeniden Bing ve kırpma tarafından sunulan URL'leri ve sorgu parametreleri içerebilir. Örneğin:

`https://<host>/th?id=AMMS_92772df988...&w=110&h=73&rs=1&qlt=80&cdv=1&pid=16.1`

Bu küçük bir alt kümesi görüntülerseniz, kalan resimleri görüntülemek için bir seçenek sağlar.

> [!NOTE]
> Kırpma ve küçük resim görüntüleri yeniden boyutlandırmayı Bing arama API'si gerektirdiği gibi üçüncü taraf haklarını uyar bir arama senaryo sağladığınızdan emin olun [kullanın ve gereksinimlerini görüntülemek](use-display-requirements.md).

## <a name="resize-a-thumbnail"></a>Bir küçük resim yeniden boyutlandırma 

Bir küçük resim yeniden boyutlandırmak için Bing yalnızca birini belirtebilirsiniz önerir `w` (genişlik) veya `h` küçük 's URL (yükseklik) sorgu parametreleri. Yalnızca enlerini veya boylarını belirtme, özgün resmin en boy korumak Bing olanak tanır. Genişlik ve yükseklik piksel cinsinden belirtin. 

Örneğin, özgün küçük 480 x 620 ise:

`https://<host>/th?id=JN.5l3yzwy%2f%2fHj59U6XhssIQ&pid=Api&w=480&h=620`

Ve boyutunu küçültmek, ayarlamak istediğiniz `w` yeni bir değer parametresini (örneğin `336`) ve Kaldır `h` parametresi:

`https://<host>/th?id=JN.5l3yzwy%2f%2fHj59U6XhssIQ&pid=Api&w=336`

Yalnızca yükseklik veya bir küçük resim genişliğini belirtirseniz, görüntünün özgün en boy oranı korunur. Her iki parametre belirtmeniz ve en boy oranı korunur değil, Bing kenarlık görüntüsünün beyaz doldurma ekler.

Örneğin, kırpma olmadan 200 x 200 480 x 359 görüntüye yeniden boyutlandırırsanız, tam genişlikli görüntü içerir ancak üst ve alt görüntüsünün doldurma beyaz 25 piksel yüksekliği içerir. Görüntü 359 x 480 olduysa, sol ve sağ kenarlık beyaz doldurma içerecektir. Görüntü kırpma, beyaz dolgu eklenmez.  

Aşağıdaki resimde, bir küçük resim (480 x 300) özgün boyutunu gösterir.  
  
![Özgün yatay görüntü](./media/resize-crop/bing-resize-crop-landscape.png)  
  
Aşağıdaki resimde, 200 x 200'e yeniden boyutlandırılmış görüntüyü gösterir. En boy oranı korunur ve üst ve alt kenarlıklarına beyaz (burada siyah kenarlığa doldurmayı göstermek için dahil edilmiştir) ile doldurulur.  
  
![Yeniden boyutlandırılan yatay görüntü](./media/resize-crop/bing-resize-crop-landscape-resized.png)  

Görüntünün özgün genişliği ve yüksekliği büyük boyutlar belirtirseniz, Bing beyaz doldurma için sol ve üst kenarlık ekleyin.  

## <a name="request-different-thumbnail-sizes"></a>İstek farklı bir küçük resim boyutları

Farklı bir küçük resim boyutu istemek için dışında küçük 's URL'den tüm sorgu parametreleri Kaldır `id` ve `pid` parametreleri. Ya da ekleme `&w` (genişlik) veya `&h` istediğiniz görüntüyü boyutunu piksel ancak ikisini birden sorgu parametresi (yükseklik). Bing görüntü özgün en boy oranını korur. 

Yukarıdaki URL'ye 165 piksel tarafından belirtilen resmin genişliğini artırmak için aşağıdaki URL kullanırsınız:

`https://<host>/th?id=AMMS_92772df988...&w=165&pid=16.1`

Görüntünün orijinal boyuttan daha büyük bir görüntü istemesi durumunda, Bing görüntünün çevresine beyaz doldurma gerektiğinde ekler. Örneğin, görüntünün özgün boyutudur 474 x 316 ve ayarladığınız `&w` Bing 500 x 333 görüntü 500'e döndürür. Bu görüntü, üst ve alt kenarları ve 13 piksel doldurma sağ ve sol kenarlarında beyaz doldurma 8,5 piksel sahip olur.

İstenen boyut görüntünün özgün boyutundan büyükse, beyaz doldurma ekleme Bing önleyecek şekilde ayarlanmış `&p` sorgu parametresi 0. Örneğin, eklerseniz `&p=0` Bing yukarıdaki URL'deki parametresi 500 x 333 görüntüsü yerine bir 474 x 316 görüntüsünü döndürür:

`https://<host>/th?id=AMMS_92772df988...&w=500&p=0&pid=16.1`

Her ikisini de belirtirseniz `&w` ve `&h` sorgu parametreleri, Bing görüntünün en boy oranını korur ve gerektiğinde beyaz doldurma ekler. Örneğin, görüntünün özgün boyutudur 474 x 316 ve genişlik ve yükseklik parametrelerini 200 x 200'e ayarlayın (`&w=200&h=200`), Bing, üst ve alt üzerinde beyaz doldurma 33 piksel içeren bir görüntü döndürür. Eklerseniz `&p` sorgu parametresi, Bing, 200 x 134 görüntüyü döndürür.

## <a name="crop-a-thumbnail"></a>Bir küçük resim kırpma 

Görüntü kırpma için dahil `c` (kırpma) sorgu parametresi. Aşağıdaki değerleri kullanabilirsiniz:
  
- `4` &mdash; Görme engelli oranı  
- `7` &mdash; Akıllı oranı  

### <a name="smart-ratio-cropping"></a>Akıllı kırpma oranı

Akıllı oranı kırpma istemesi durumunda (ayarlayarak `c` parametresi `7`), Bing kırpma kendi ilgi bölgesi merkezi bir görüntüden dışa doğru görüntünün en boy oranını koruyarak. İlgi bölgesi çoğu alma kısımları içeren Bing belirleyen görüntü alanıdır. Aşağıda bir örnek ilgi bölgesi gösterilmektedir.  
  
![İlgi bölgesi](./media/resize-crop/bing-resize-crop-regionofinterest.png)

Görüntüyü yeniden boyutlandırma ve akıllı oranı kırpma isteği, Bing en boy oranını korurken görüntünün istenen boyuta azaltır. Bing, ardından yeniden boyutlandırılan boyutlarına göre resmi kırpar. Örneğin, yeniden boyutlandırılan genişliği veya yüksekliği eşit azsa, Bing sola ve sağa ilgi bölgesi merkezinin resmi kırpar. Aksi takdirde, Bing, üst ve alt bölgeyi merkezinin kırpma.  
  
 
200 x oranı akıllı kırpma kullanarak 200'e azalır görüntüyü gösterir. Sol üst köşedeki görüntüden Bing ölçer nedeniyle görüntünün alt kısmında kırpılır. 
  
![200 x 200 kırpılmış yatay görüntü](./media/resize-crop/bing-resize-crop-landscape200x200c7.png) 
  
200 x oranı akıllı kırpma kullanarak 100 azaltılmış görüntüyü gösterir. Sol üst köşedeki görüntüden Bing ölçer nedeniyle görüntünün alt kısmında kırpılır. 
   
![200 x 100 kırpılmış yatay görüntü](./media/resize-crop/bing-resize-crop-landscape200x100c7.png)
  
100 x oranı akıllı kırpma kullanarak 200'e azalır görüntüyü gösterir. Bing görüntü Merkezi'nden ölçer olduğundan, sol ve sağ bölümleri kırpılır.
  
![100 x 200 kırpılmış yatay görüntü](./media/resize-crop/bing-resize-crop-landscape100x200c7.png) 

Bing görüntü ilgi bölgesi belirleyemiyorsa, hizmet görme oranı kırpma kullanın.  

### <a name="blind-ratio-cropping"></a>Görme engelli oranı kırpma

Görme oranı kırpma istemesi durumunda (ayarlayarak `c` parametresi `4`), Bing, görüntü kırpma için aşağıdaki kuralları kullanır.  
  
- Varsa `(Original Image Width / Original Image Height) < (Requested Image Width / Requested Image Height)`, görüntü sol üst köşesinden ölçülür ve altındaki kırpılır.  
- Varsa `(Original Image Width / Original Image Height) > (Requested Image Width / Requested Image Height)`, görüntünün merkezden ölçülür ve sağa ve sola kırpılmış.  

225 x 300 bir dikey görüntü gösterir.  
  
![Özgün Ay Çiçeği görüntüsü](./media/resize-crop/bing-resize-crop-sunflower.png)
  
200 x görme oranı kırpma kullanarak 200'e azalır görüntüyü gösterir. Resim kırpılmış görüntünün alt kısmında kaynaklanan sol üst köşedeki ölçülür.  
  
![200 x 200 kırpılmış Ay Çiçeği görüntüsü](./media/resize-crop/bing-resize-crop-sunflower200x200c4.png)
  
200 x görme oranı kırpma kullanarak 100 azaltılmış görüntüyü gösterir. Resim kırpılmış görüntünün alt kısmında kaynaklanan sol üst köşedeki ölçülür.  
  
![200 x 100 kırpılmış Ay Çiçeği görüntüsü](./media/resize-crop/bing-resize-crop-sunflower200x100c4.png)
  
100 x görme oranı kırpma kullanarak 200'e azalır görüntüyü gösterir. Resim kırpılmış resmin sol ve sağ bölümlerinde kaynaklanan Merkezi'nden ölçülür.  
  
![100 x 200 kırpılmış Ay Çiçeği görüntüsü](./media/resize-crop/bing-resize-crop-sunflower100x200c4.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Bing arama API'leri nelerdir?](bing-api-comparison.md)
* [Bing arama API'si kullanın ve gereksinimleri görüntüleme](use-display-requirements.md)
