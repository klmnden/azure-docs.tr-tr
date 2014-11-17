Depolama hesabınızdaki veriler aynı zamanda kullanılabilirliği yüksek olan dayanıklılığı sağlayacak şekilde çoğaltılarak, geçici donanım arızaları karşısında bile [Azure Depolama SLA'sını ](/tr-tr/support/legal/sla/) karşılar. Azure Depolama dünya çapında 15 bölgede dağıtılır ve ayrıca bölgeler arasında verileri çoğaltma desteğini de içerir. Depolama hesabınızdaki verileri çoğaltmak için çeşitli seçenekleriniz vardır:

- *Yerel Olarak Yedekli Depolama (LRS)* verilerinizin üç kopyasını tutar. LRS, tek bir bölgedeki tek bir tesis içinde üç kez çoğaltılır. LRS verilerinizi normal donanım arızalarından korur, ancak tek bir tesisteki arızadan korumaz.

	LRS indirimli fiyatla sunulur. En üst düzeyde dayanıklılık için, aşağıda açıklanan coğrafi olarak yedekli depolamayı kullanmanızı öneriyoruz.

- *Sıfır Yedekli Depolama (ZRS)* verilerinizin üç kopyasını tutar. ZRS, gerek tek bir bölge içinde gerekse iki bölge genelinde iki ila üç tesise üç kez çoğaltılarak LRS'ye göre daha yüksek dayanıklılık sağlar. ZRS, verilerinizin tek bir bölge içinde dayanıklı olmasını sağlar.
 
	ZRS, LRS'ye göre üst düzeyde dayanıklılık sağlar; bununla birlikte, En üst düzeyde dayanıklılık için aşağıda açıklanan coğrafi olarak yedekli depolamayı kullanmanızı öneriyoruz.

	> [WACOM.NOTE] ZRS şu an yalnızca blok blob'lar için kullanılabilir. Depolama hesabınızı oluşturduktan ve sıfır yedekli çoğaltmayı seçtikten sonra başka bir çoğaltma türünü kullanmak üzere dönüştüremeyeceğinizi veya bunun tersini yapamayacağınızı unutmayın.

- *Coğrafi olarak yedekli depolama (GRS)*, depolama hesabınızı oluşturduğunuzda bu hesap için varsayılan olarak etkindir. GRS verilerinizin altı kopyasını tutar. GRS ile verileriniz birincil bölge içinde üç kez çoğaltılır ve ayrıca, birincil bölgeden yüzlerce kilometre uzaktaki ikincil bir bölgede de üç kez çoğaltılarak en üst düzeyde dayanıklılığı sağlar. Birincil bölgede bir arıza olması durumunda Azure Depolama ikincil bölgeye yük devri yapar. GRS, verilerinizin iki ayrı bölgede dayanıklı olmasını sağlar. 

	> [WACOM.NOTE] En üst düzeyde dayanıklılık için ZRS veya LRS'ye kıyasla GRS önerilir.

- *Okuma erişimli coğrafi olarak yedekli depolama (RA-GRS)*, coğrafi olarak yedekli depolamanın yukarıda belirtilen tüm avantajlarını sağlar ve ayrıca, birincil bölgenin kullanılamaz olması durumunda ikincil bölgedeki verilere okuma erişimi sağlar. Dayanıklılığa ek olarak en üst düzeyde kullanılabilirlik için okuma erişimli coğrafi olarak yedekli depolama önerilir.  

Çoğaltma seçenekleri hakkında daha ayrıntılı bilgi için [Azure Depolama Ekip Blogu](http://blogs.msdn.com/b/windowsazurestorage/)'na ve [Azure Depolama Artıklık Seçenekleri](http://msdn.microsoft.com/tr-tr/library/azure/dn727290.aspx)'ne bakın.
	
Çeşitli çoğaltma seçenekleri arasında fiyatlandırma farkları [Depolama Fiyatlandırması Ayrıntıları](/tr-tr/pricing/details/storage/) sayfasında bulunabilir.
