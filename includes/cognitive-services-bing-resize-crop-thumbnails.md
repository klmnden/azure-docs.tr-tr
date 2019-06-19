---
author: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 11/09/2018
ms.author: nitinme
ms.openlocfilehash: f96c3a693ce8fc099374c998b35ce2fa90f4bb3f
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188652"
---
Bing tarafından sunulan küçük resimler için URL'leri bazı Bing yanıtlarını içerir. Yeniden boyutlandırma ve küçük resim görüntüleri kırpma. 

> [!NOTE]
> Boyutu emin olun ve küçük resmini kırpma arama senaryosu sağlayın ve Bing arama API'si kullanılarak gerektiği gibi üçüncü taraf haklarına saygı ve gereksinimlerini görüntüleyin.


Bir görüntüyü yeniden boyutlandırmak için w (genişlik) sorgu içeren sorgu parametresi ya da hem küçük 's URL parametresi, h (yükseklik). Genişlik ve yükseklik piksel cinsinden belirtin. Örneğin:  
  
`https://<host>/th?id=JN.5l3yzwy%2f%2fHj59U6XhssIQ&pid=Api&w=200&h=200`  
  
Yalnızca genişliğini veya yüksekliğini sorgu parametresi yalnızca belirtirseniz, Bing görüntünün en boy oranını korur. Genişlik ve yükseklik belirtin ve görüntünün özgün en boy oranını koru yoksa, Bing kenarlık görüntüsünün beyaz doldurma ekler. Örneğin, kırpma olmadan 200 x 200 480 x 359 görüntüye yeniden boyutlandırırsanız, görüntü tam genişlik içeriyor ancak üst ve alt görüntüsünün doldurma beyaz 25 piksel yüksekliği içerir. Aynı oluştuysa görüntü 359 x 480 dışında sol ve sağ kenarlık beyaz doldurma içerecektir true olur. Görüntü kırpma, beyaz dolgu eklenmez.  

 
Aşağıdaki resimde, bir küçük resim (480 x 300) özgün boyutunu gösterir.  
  
![Özgün yatay görüntü](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape.PNG)  
  
Aşağıdaki resimde, 200 x 200'e yeniden boyutlandırılmış görüntüyü gösterir. En boy oranı korunur ve üst ve alt kenarlıklarına beyaz (siyah kenarlığa doldurmayı göstermek için dahil edilmiştir) ile doldurulur.  
  
![Yeniden boyutlandırılan yatay görüntü](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape-resized.PNG)  



Görüntünün özgün genişliği ve yüksekliği büyük boyutlar belirtirseniz, resmin sol ve üst kenarlıklar üzerinde beyaz sıfır eklenir.  
  
Görüntü kırpma için ' % s'c (kırpma) içerir. sorgu parametresi. Belirttiğiniz olası değerler şunlardır:  
  
- 4&mdash;görme oranı  
- 7&mdash;akıllı oranı  
  
Akıllı oranı kırpma istemesi durumunda (c = 7), görüntü görüntünün ilgi bölgesi Merkezi'nden görüntünün en boy oranını koruyarak dışa doğru kırpılır. İlgi bölgesi çoğu alma kısımları içeren Bing belirleyen görüntü alanıdır. Aşağıda bir örnek ilgi bölgesi gösterilmektedir.  
  
![İlgi bölgesi](./media/cognitive-services-bing-resize-crop/bing-resize-crop-regionofinterest.PNG)

Görüntüyü yeniden boyutlandırma ve akıllı oranı kırpma isteği, görüntünün en boy oranını koruyarak istenen boyuta azaltılır. Görüntüyü yeniden boyutlandırılan boyutlarına göre kırpılır. Örneğin, yeniden boyutlandırılan genişliği veya yüksekliği eşit azsa, görüntü sola ve sağa ilgi bölgesi merkezi kırpılır. Aksi takdirde, görüntünün üst ve alt bölgeyi merkezinin kırpılır.  
  
 
200 x oranı akıllı kırpma kullanarak 200'e azalır görüntüyü gösterir.  
  
![200 x 200 kırpılmış yatay görüntü](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape200x200c7.PNG)
  
200 x oranı akıllı kırpma kullanarak 100 azaltılmış görüntüyü gösterir.  
   
![200 x 100 kırpılmış yatay görüntü](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape200x100c7.PNG)
  
100 x oranı akıllı kırpma kullanarak 200'e azalır görüntüyü gösterir.  
  
![100 x 200 kırpılmış yatay görüntü](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape100x200c7.PNG)



Bing görüntü ilgi bölgesi belirleyemiyorsa, Bing, görme oranı kırpma kullanır.  
  
Görme engelli oranı kırpma istemesi durumunda (c = 4), Bing, görüntü kırpma için aşağıdaki kuralları kullanır.  
  
- Varsa (özgün görüntü genişliğini / özgün resim yüksekliği) < (görüntü genişliğini istenen / resim yüksekliği istenen), görüntü üstten sol köşe ve alt kısmında kırpılmış ölçülür.  
- Varsa (özgün görüntü genişliğini / özgün resim yüksekliği) > (görüntü genişliğini istenen / resim yüksekliği istenen), görüntünün merkezden ölçülür ve sağa ve sola kırpılmış.  



225 x 300 bir dikey görüntü gösterir.  
  
![Özgün Ay Çiçeği görüntüsü](./media/cognitive-services-bing-resize-crop/bing-resize-crop-sunflower.PNG)
  
200 x görme oranı kırpma kullanarak 200'e azalır görüntüyü gösterir. Resim kırpılmış görüntünün alt kısmında kaynaklanan sol üst köşedeki zamandan ölçülür.  
  
![200 x 200 kırpılmış Ay Çiçeği görüntüsü](./media/cognitive-services-bing-resize-crop/bing-resize-crop-sunflower200x200c4.PNG)
  
200 x görme oranı kırpma kullanarak 100 azaltılmış görüntüyü gösterir. Resim kırpılmış görüntünün alt kısmında kaynaklanan sol üst köşedeki zamandan ölçülür.  
  
![200 x 100 kırpılmış Ay Çiçeği görüntüsü](./media/cognitive-services-bing-resize-crop/bing-resize-crop-sunflower200x100c4.PNG)
  
100 x görme oranı kırpma kullanarak 200'e azalır görüntüyü gösterir. Resim kırpılmış resmin sol ve sağ bölümlerinde kaynaklanan Merkezi'nden ölçülür.  
  
![100 x 200 kırpılmış Ay Çiçeği görüntüsü](./media/cognitive-services-bing-resize-crop/bing-resize-crop-sunflower100x200c4.PNG)

