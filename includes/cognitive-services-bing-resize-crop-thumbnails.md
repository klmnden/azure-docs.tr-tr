Bazı Bing yanıtları resimlerin Bing tarafından sunulan URL'leri eklenecek. Yeniden boyutlandırma ve küçük resim görüntüleri kırpma. 

> [!NOTE]
> Boyutu emin olun ve küçük resmini kırpma arama senaryo sağlar ve Bing arama API'si kullanılarak gerektiği gibi üçüncü taraf haklarını saygı ve gereksinimlerini görüntülemek.


Görüntüyü yeniden boyutlandırmak için w (genişlik) ve y (yükseklik) dahil sorgu parametreleri küçük resim ait URL. Genişlik ve yükseklik piksel cinsinden belirtin. Örneğin:  
  
`https://<host>/th?id=JN.5l3yzwy%2f%2fHj59U6XhssIQ&pid=Api&w=200&h=200`  
  
Görüntüyü yeniden boyutlandırmak, en boy oranı korunur. En boy oranını korumak için kenarlık görüntüsünün beyaz doldurma eklenebilir. Örneğin, kırpma olmadan 200 x 200 480 x 359 görüntüye yeniden boyutlandırmak, görüntü tam genişlik içerir ancak üst ve alt görüntünün doldurma beyaz 25 piksel yüksekliği içeriyor. Aynı oluştuysa, görüntünün 359 x 480 dışında sol ve sağ kenarlık beyaz doldurma içerecektir true olması. Görüntü kırpma, beyaz dolgu eklenmez.  

 
Aşağıdaki resimde, küçük resim (480 x 300) özgün boyutunu gösterir.  
  
![Özgün yatay görüntüsü](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape.PNG)  
  
Aşağıdaki resimde 200 x 200'e yeniden boyutlandırılan resim göstermektedir. En boy oranını saklanır ve üst ve Alt Kenarlık (doldurma göstermek için siyah kenarlık bulunur) beyaz renkle doldurulur.  
  
![Yeniden boyutlandırılan yatay görüntüsü](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape-resized.PNG)  



Resmin özgün genişlik ve yükseklik büyük boyutları belirtirseniz, görüntünün beyaz sol ve üst sınırlar üzerinde sıfır eklenir.  
  
Görüntü kırpma için c (kırpma) dahil sorgu parametresi. Belirttiğiniz olası değerler şunlardır:  
  
- 4&mdash;görme oranı  
- 7&mdash;akıllı oranı  
  
Akıllı oranı kırpma isterse (c = 7), görüntü görüntünün bölge ilgi Merkezi'nden resmin en boy oranını koruyarak dışa doğru kırpılır. İlgi çoğu alma bölümleri içeren Bing belirler resmin bölgedir. Aşağıdaki örnekte bölge ilgi gösterir.  
  
![Bölge ilgi](./media/cognitive-services-bing-resize-crop/bing-resize-crop-regionofinterest.PNG)

Görüntüyü yeniden boyutlandırma ve akıllı oranı kırpma isteği, resmin en boy oranını koruyarak istenen boyuta azalır. Görüntüyü yeniden boyutlandırılan boyutlara dayanan kırpılır. Örneğin, yeniden boyutlandırılan genişliği veya yüksekliği eşittir azsa, görüntü sol ve sağ ilgi bölge merkezi kırpılır. Aksi takdirde, görüntünün üst ve alt bölgenin ilgi merkezinin kırpılır.  
  
 
200 x akıllı oranı kırpma kullanarak 200'e azaltılmış görüntü gösterir.  
  
![200 x 200'e kırpılmış yatay görüntüsü](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape200x200c7.PNG)
  
200 x akıllı oranı kırpma kullanarak 100 azaltılmış görüntü gösterir.  
   
![200 x 100'e kırpılmış yatay görüntüsü](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape200x100c7.PNG)
  
100 x akıllı oranı kırpma kullanarak 200'e azaltılmış görüntü gösterir.  
  
![100 x 200'e kırpılmış yatay görüntüsü](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape100x200c7.PNG)



Bing görüntünün bölge ilgi belirleyemiyorsa, Bing görme oranı kırpma kullanır.  
  
Görme engelli oranı kırpma isterse (c = 4), Bing görüntü kırpma için aşağıdaki kuralları kullanır.  
  
- Varsa (özgün görüntü genişliği / özgün görüntü yüksekliği) < (görüntü genişliği istenen / Görüntü yüksekliği istenen), görüntü sol köşe ve alt kısmındaki kırpılmış üstten ölçülür.  
- Varsa (özgün görüntü genişliği / özgün görüntü yüksekliği) > (görüntü genişliği istenen / Görüntü yüksekliği istenen), görüntünün merkezden ölçülen ve sol ve sağ kırpılmış.  



Aşağıdaki 225 x 300 bir dikey görüntüsü gösterir.  
  
![Özgün Ayçiçeği görüntüsü](./media/cognitive-services-bing-resize-crop/bing-resize-crop-sunflower.PNG)
  
200 x görme oranı kırpma kullanarak 200'e azaltılmış görüntü gösterir. Resim kırpılmış olmaktan görüntü alt kısmında kaynaklanan sol üst köşesinde ölçülür.  
  
![200 x 200'e kırpılmış Ayçiçeği görüntüsü](./media/cognitive-services-bing-resize-crop/bing-resize-crop-sunflower200x200c4.PNG)
  
200 x görme oranı kırpma kullanarak 100 azaltılmış görüntü gösterir. Resim kırpılmış olmaktan görüntü alt kısmında kaynaklanan sol üst köşesinde ölçülür.  
  
![200 x 100'e kırpılmış Ayçiçeği görüntüsü](./media/cognitive-services-bing-resize-crop/bing-resize-crop-sunflower200x100c4.PNG)
  
100 x görme oranı kırpma kullanarak 200'e azaltılmış görüntü gösterir. Resim kırpılmış olmaktan görüntü sol ve sağ bölümlerinde kaynaklanan Merkezi'nden ölçülür.  
  
![100 x 200'e kırpılmış Ayçiçeği görüntüsü](./media/cognitive-services-bing-resize-crop/bing-resize-crop-sunflower100x200c4.PNG)

