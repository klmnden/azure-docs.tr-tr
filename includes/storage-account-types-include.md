İki tür depolama hesabı vardır:

### <a name="generalpurpose-storage-accounts"></a>Genel Amaçlı Depolama Hesapları
Genel amaçlı depolama hesabı, tek bir hesap altında Tablolar, Kuyruklar, Dosyalar, Bloblar ve Azure sanal makinesi diskleri gibi Azure Storage hizmetlerine erişim sağlar. Bu tür depolama hesabında iki performans katmanı bulunur:

* Tabloları, Kuyrukları, Dosyaları, Blobları ve Azure sanal makinesi disklerini depolamanızı sağlayan standart depolama performans katmanı.
* Şu anda yalnızca Azure Sanal Makinesi disklerini destekleyen Premium Storage performans katmanı. Premium Storage’a yönelik ayrıntılı genel bakış için bkz. [Premium Storage: Azure Virtual Machine İş Yükleri için Yüksek Performanslı Depolama](../articles/storage/storage-premium-storage.md).

### <a name="blob-storage-accounts"></a>Blob Storage Hesapları
Blob Storage hesabı, yapılandırılmamış verilerinizi bloblar (nesneler) olarak Azure Storage’da depolamanıza yönelik özel depolama hesabıdır. Blob Storage hesapları, varolan genel amaçlı depolama hesaplarınıza benzer ve blok blobları ve ilave blobları için %100 API tutarlığı dahil günümüzde kullandığınız tüm harika dayanıklılık, kullanılabilirlik, ölçeklenebilirlik ve performans özelliklerini paylaşır. Yalnızca blok veya engelleme blobunun gerektiği uygulamalar için Blob Storage hesaplarının kullanılmasını öneririz.

> [!NOTE]
> Blob Storage hesapları yalnızca blok ve ilave bloblarını destekler, sayfa bloblarını desteklemez.
> 
> 

Blob Storage hesapları, hesap oluşturma sırasında belirtilebilen, gerektiğinde de sonra değiştirilebilen **Erişim Katmanı** özniteliğini gösterir. Erişim deseniniz temelinde belirtilebilecek iki tür erişim katmanı vardır:

* **Sık Erişimli** erişim katmanı; depolama hesabındaki nesnelere erişimin daha sık olduğunu belirtir. Bu, verileri daha düşük bir erişim maliyetiyle depolamanızı sağlar.
* **Seyrek Erişimli** erişim katmanı; depolama hesabındaki nesnelere erişimin daha seyrek olduğunu belirtir. Bu, verileri daha düşük bir veri depolama maliyetiyle depolamanızı sağlar.

Verilerinizin kullanım düzeninde bir değişiklik olursa,herhangi bir zamanda bu erişim katmanları arasında geçiş yapabilirsiniz. Erişim katmanının değiştirilmesi ek ücretlere neden olabilir. Daha fazla ayrıntı için lütfen bkz. [Blob Storage hesapları için fiyatlandırma ve faturalama](../articles/storage/storage-blob-storage-tiers.md#pricing-and-billing).

Blob Storage hesapları hakkında daha fazla ayrıntı için bkz. [Azure Blob Storage: Seyrek Erişimli ve Sık Erişimli katmanlar](../articles/storage/storage-blob-storage-tiers.md).

Depolama hesabı oluşturabilmeniz için, öncelikle çeşitli Azure hizmetlerine erişim sağlayan bir plan olan Azure aboneliğiniz olması gerekir. [Ücretsiz hesapla](https://azure.microsoft.com/pricing/free-trial/) Azure’a başlayabilirsiniz. Bir abonelik planı satın almaya karar verdiğinizde bir dizi [satın alma seçeneği](https://azure.microsoft.com/pricing/purchase-options/) arasından seçim yapabilirsiniz. [MSDN abonesiyseniz](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), Azure Storage da dahil, Azure hizmetlerinde kullanabileceğiniz aylık ücretsiz krediler alırsınız. Birim fiyatlandırma hakkında bilgi için bkz. [Azure Storage Fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).

Depolama hesabı oluşturmayı öğrenmek amacıyla daha fazla bilgi için bkz. [Depolama hesabı oluşturma](../articles/storage/storage-create-storage-account.md#create-a-storage-account). Tek bir abonelikte benzersiz olarak adlandırılmış 100 depolama hesabına kadar oluşturabilirsiniz. Depolama hesabı limitleri hakkında ayrıntılı bilgi için bkz. [Azure Storage Ölçeklenebilirlik ve Performans Hedefleri](../articles/storage/storage-scalability-targets.md).



<!--HONumber=Nov16_HO2-->


