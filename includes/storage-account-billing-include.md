Depolama hesabınıza bağlı olarak Azure Storage kullanımınıza göre faturalandırılırsınız. Depolama maliyetleri şu etkenlere bağlıdır: bölge/konum, hesap türü, depolama kapasitesi, çoğaltma düzeni, depolama işlemleri ve veri çıkışı.

- Bölge, hesabınızın temel alındığı coğrafi bölgeye başvurur.
- Hesap türü, genel amaçlı bir depolama hesabı mı, yoksa Blob Storage hesabı mı kullanmanızla ilgilidir. Blob Storage hesabıyla, erişim katmanı hesabın fatura modelini de saptar.
- Depolama kapasitesi, veri depolamak için kullandığınız depolama hesabı payınızın kullandığınız kısmını belirtir.
- Çoğaltma, verilerinizin her seferinde tutulan kopya sayısını ve tutulduğu konumu belirtir.
- İşlemler, Azure Storage’a yapılan tüm okuma ve yazma işlemlerini ifade eder.
- Veri çıkışı bir Azure bölgesinin dışına aktarılan verileri ifade eder. Depolama hesabınızdaki verilere aynı bölgede çalıştırılmayan bir uygulama eriştiğinde veri çıkışı için ücretlendirilirsiniz. (Azure hizmetleri için, veri çıkış ücretlerini azaltmak veya ortadan kaldırmak için veri ve hizmetlerinizi aynı veri merkezlerinde gruplamak üzere adım atabilirsiniz.)

[Azure Storage Fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/) sayfası hesap türüne, depolama kapasitesine, çoğaltmaya ve işlemlere bağlı olarak ayrıntılı fiyatladırma bilgileri sağlar. [Veri Aktarımları Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/data-transfers/), veri çıkışı için ayrıntılı fiyatlandırma bilgileri sağlar. Maliyetlerinizi tahmin etmeye yardımcı olması için [Azure Storage Fiyatlandırması Hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/?scenario=data-management)’nı kullanabilirsiniz.


<!--HONumber=Sep16_HO3-->


