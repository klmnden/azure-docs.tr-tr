Yerel olarak yedekli depolama (LRS), en az %99.999999999 sağlamak için tasarlanmıştır (11 9'in) dayanıklılık verilerinizi depolama hesabınız oluşturulan bölgede bir veri merkezinde bulunan bir depolama ölçek birimi içinde çoğaltma tarafından verilen bir yıl içinde nesne. Tüm çoğaltmalar için yalnızca yazıldıktan sonra Yazma isteği başarıyla döndürür. Her bu çoğaltmalar ayrı hata etki alanları ve bir depolama ölçek birimi içinde yükseltme etki alanları bulunur.

Bir depolama ölçek birimi depolama düğümleri raflarının koleksiyonudur. Hata etki alanı (FD) hata fiziksel bir birimi temsil eder ve aynı fiziksel raf ait düğümleri olarak kabul düğümlerinin bir gruptur. Bir yükseltme etki alanına (UD), hizmet yükseltmesi (sunum) işlemi sırasında birlikte yükseltilir düğümleri grubudur. Çoğaltmalar, verileri tek bir rafa donanım arızası etkiler olsa bile veya düğümler piyasaya sürme sırasında yükseltildiğinde olduğundan emin olmak için bir depolama ölçek birimi içinde UDs ve FDs yayılır.

LRS en düşük maliyeti seçeneği ve diğer seçenekleri karşılaştırıldığında en az düzeyde dayanıklılık sunar. Bir veri merkezi düzeyi olağanüstü (vb. taşmasını yangın) durumda tüm çoğaltmaları kayıp veya kurtarılamaz olabilir. Bu riski azaltmak için coğrafi olarak yedekli depolama (GRS) çoğu uygulama için önerilir.

Yerel olarak yedekli depolama hala belirli senaryolarda istenebilir:

* En yüksek bant genişliği üst sınırı Azure Storage çoğaltma seçenekleri sağlar.
* Uygulamanızı kolayca canlandırılabilir veri depoluyorsa, LRS için tercih edebilirsiniz.
* Bazı uygulamalar, yalnızca veri idare gereksinimleri nedeniyle bir ülke içinde veri çoğaltmak için kısıtlanır. Eşleştirilmiş bir bölge başka bir ülkede olabilir. Bölge çiftleri hakkında daha fazla bilgi için bkz: [Azure bölgeleri](https://azure.microsoft.com/regions/).
