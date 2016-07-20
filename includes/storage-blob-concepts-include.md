## Blob Storage nedir?

Azure Blob Storage; HTTP veya HTTPS aracılığıyla dünyanın her yerinde erişilebilen metin veya ikili veriler gibi büyük miktarda yapılandırılmamış nesne verilerinin depolanması için bir hizmettir. Verileri genel olarak herkese açık kullanıma sunmak veya uygulama verilerini özel olarak depolamak için Blob Storage’ı kullanabilirsiniz.

Blob Storage’ın yaygın kullanımları şunlardır:

- Görüntülerin veya belgelerin doğrudan bir tarayıcıya sunulması
- Dağıtılan erişim için dosyaların depolanması
- Video ve ses akışları
- Yedekleme ve geri yükleme, olağanüstü durum kurtarma ve arşivleme için verilerin depolanması
- Şirket içi veya Azure barındırılan hizmetle analiz için verilerin depolanması

## Blob hizmeti kavramları

Blob hizmetinde şu bileşenler bulunur:

![Blob1][Blob1]

- **Depolama Hesabı:** Tüm Azure Storage erişimi bir depolama hesabıyla yapılır. Bu depolama hesabı **Genel amaçlı depolama hesabı** veya nesnelerin/blobların depolanması için özelleştirilen **Blob Storage hesabı** olabilir. Depolama hesapları hakkında daha fazla bilgi için bkz. [Azure Storage hesabı](../articles/storage/storage-create-storage-account.md).

- **Kapsayıcı:** Kapsayıcı, bir dizi blobun gruplandırılmasını sağlar. Tüm bloblar bir kapsayıcıda olmalıdır. Bir hesapta sınırsız sayıda kapsayıcı olabilir. Kapsayıcıda sınırsız sayıda blob depolanabilir. Kapsayıcı adındaki harflerin küçük harf olması gerektiğini unutmayın.

- **Blob:** Herhangi bir türde ve boyutta bir dosya. Azure Storage üç tür blob sunar: blok blobları, sayfa blobları ve ekleme blobları.

    *Blok blobları*, belgeler ve medya dosyaları gibi metin veya ikili dosyaların depolanması için idealdir. *Ekleme blobları* blok bloblarına benzer; bloklardan oluşturulmuş olsalar da ekleme işlemleri için iyileştirilmişlerdir; bu nedenle, günlük kaydı senaryoları için kullanışlıdırlar. Tek bir blok blobunda veya ekleme blobunda her biri 4 MB büyüklükte olabilen 50.000 blok olabilir; toplam boyut 195 GB’tan (4 MB X 50.000) biraz fazladır.

    *Sayfa blobları* boyut olarak 1 TB'ye kadar olabilir; sık gerçekleştirilen okuma/yazma işlemleri için daha verimlidir. Azure Virtual Machines sayfa bloblarını işletim sistemi ve veri diskleri olarak kullanır.

    Kapsayıcıları ve blobları adlandırma hakkında ayrıntılı bilgi için bkz. [Kapsayıcıları, Blobları ve Meta Verileri Adlandırma ve Bunlara Başvurma](https://msdn.microsoft.com/library/azure/dd135715.aspx).


[Blob1]: ./media/storage-blob-concepts-include/blob1.jpg



<!--HONumber=Jun16_HO2-->


