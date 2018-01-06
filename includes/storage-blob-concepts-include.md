## <a name="what-is-blob-storage"></a>Blob storage nedir?
Azure Blob Storage; HTTP veya HTTPS aracılığıyla dünyanın her yerinde erişilebilen metin veya ikili veriler gibi büyük miktarda yapılandırılmamış nesne verilerinin depolanması için bir hizmettir. Verileri genel olarak herkese açık kullanıma sunmak veya uygulama verilerini özel olarak depolamak için Blob Storage’ı kullanabilirsiniz.

Blob Storage’ın yaygın kullanımları şunlardır:

* Görüntülerin veya belgelerin doğrudan bir tarayıcıya hizmet.
* Dağıtılan erişim için dosyaların depolanması.
* Video ve ses akışları.
* Yedekleme ve geri yükleme, olağanüstü durum kurtarma ve arşivleme için verilerin depolanması.
* Bir şirket içi tarafından analiz için verilerin depolanması veya Azure barındırılan hizmet.

## <a name="blob-service-concepts"></a>Blob hizmeti kavramları
Blob hizmetinde şu bileşenler bulunur:

![Blob hizmeti mimarisi diyagramı](./media/storage-blob-concepts-include/blob1.png)

* **Depolama hesabı:** tüm Azure Storage erişimi bir depolama hesabıyla yapılır. Bu depolama hesabı olabilir bir **genel amaçlı depolama hesabı** veya **Blob storage hesabı**, hangi özelleştirilmiş nesneleri veya bloblarını depolamak için. Daha fazla bilgi için bkz. [Azure depolama hesapları hakkında](../articles/storage/common/storage-create-storage-account.md).
* **Kapsayıcı:** Kapsayıcı, bir dizi blobun gruplandırılmasını sağlar. Tüm bloblar bir kapsayıcıda olmalıdır. Bir hesapta sınırsız sayıda kapsayıcı olabilir. Kapsayıcıda sınırsız sayıda blob depolanabilir. Kapsayıcı adındaki harflerin küçük harf olması gerektiğini unutmayın.
* **Blob:** Herhangi bir türde ve boyutta bir dosya. Azure Storage üç tür BLOB sunar: blok blobları, ekleme blobları ve sayfa blobları.
  
    *Blok blobları*, belgeler ve medya dosyaları gibi metin veya ikili dosyaların depolanması için idealdir. Tek bir blok blobu, her birinin büyüklüğü 100 MB’a kadar olabilen 50.000 blok içerebilir; toplam boyut 4,75 TB'tan biraz fazladır (100 MB X 50.000). 

    *Ekleme blobları* blok bloblarına benzer; bloklardan oluşturulmuş olsalar da ekleme işlemleri için iyileştirilmişlerdir; bu nedenle, günlük kaydı senaryoları için kullanışlıdırlar. Tek bir ekleme blobu, her birinin büyüklüğü 4 MB’a kadar olabilen 50.000 blok içerebilir; toplam boyut 195 GB'tan biraz fazladır (4 MB X 50.000).
  
    *Sayfa blobları* boyut olarak 1 TB'ye kadar olabilir; sık gerçekleştirilen okuma/yazma işlemleri için daha verimlidir. Azure Virtual Machines sayfa bloblarını işletim sistemi ve veri diskleri kullanın.
  
    Kapsayıcıları ve blobları adlandırma hakkında daha fazla bilgi için bkz [adlandırma ve kapsayıcıları, blobları ve meta verileri başvuran](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata).

