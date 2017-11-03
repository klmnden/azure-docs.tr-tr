
Azure Cosmos DB genel dağıtım bu Azure Cuma video Scott Hanselman ve sorumlu mühendislik Yöneticisi Karthik Raman hakkında bilgi edinebilirsiniz.

>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Planet-Scale-NoSQL-with-DocumentDB/player]  

Azure Cosmos DB'de nasıl genel veritabanı çoğaltma hakkında daha fazla bilgi çalıştığı için bkz: [Cosmos DB genel verilerle dağıtmak](../articles/cosmos-db/distribute-data-globally.md).

## <a id="addregion"></a>Azure Portalı'nı kullanarak genel veritabanı bölgeleri ekleyin
Azure Cosmos DB kullanılabilir tüm [Azure bölgeleri] [ azureregions] dünya çapında. Veritabanı hesabınız için varsayılan tutarlılık düzeyi seçtikten sonra bir veya daha fazla bölgeler (tercih ettiğiniz varsayılan tutarlılık düzeyi ve genel dağıtım gereksinimlerine bağlı olarak) ilişkilendirebilirsiniz.

1. İçinde [Azure portal](https://portal.azure.com/), sol çubuğunda **Azure Cosmos DB**.
2. İçinde **Azure Cosmos DB** veritabanı hesabı değiştirmek için dikey penceresinde, seçin.
3. Hesap dikey penceresinde tıklayın **genel veri çoğaltma** menüsünde.
4. İçinde **genel veri çoğaltma** dikey penceresinde, harita bölgelerde tıklayarak Ekle/Kaldır için bölge seçin ve ardından **kaydetmek**. Bölgeler ekleme maliyeti, bkz: [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/cosmos-db/) veya [Azure Cosmos DB genel verilerle dağıtmak](../articles/cosmos-db/distribute-data-globally.md) daha fazla bilgi için makalenin.
   
    ![Bölgeleri eklemek veya bunları kaldırmak için eşlemesindeki'i tıklatın][1]
    
İkinci bir bölgeye eklediğinizde **elle yük devretme** seçeneğini etkinleştirerek **genel veri çoğaltma** portaldaki dikey pencere. Sınama yük devretme işlemi veya birincil yazma bölge değiştirmek için bu seçeneği kullanın. Üçüncü bir bölgeye eklediğinizde **yük devretme öncelikleri** seçeneği, aynı dikey penceresinde etkinleştirildiyse, böylece okumalar için yük devretme sırasını değiştirebilirsiniz.  

### <a name="selecting-global-database-regions"></a>Genel veritabanı bölgeler seçme
İki veya daha fazla bölge yapılandırma için iki yaygın senaryolar şunlardır:

1. Dünya nerede bulundukları olsun, son kullanıcılara verileri için düşük gecikmeli erişim gönderiliyor
2. İş devamlılığı ve olağanüstü durum kurtarma (BCDR) için bölgesel esnekliği ekleme

Düşük gecikme süreli son kullanıcılara teslim etmek için her iki uygulamayı dağıtmak ve uygulamanın kullanıcıların bulunduğu için karşılık gelen olduğu bölgelerde Azure Cosmos DB eklemek için önerilir.

Açıklanan bölge çiftleri temel bölgeler eklemek için önerilen BCDR için [iş devamlılığı ve olağanüstü durum kurtarma (BCDR): Azure eşleştirilmiş bölgeleri] [ bcdr] makalesi.

<!--

## <a id="selectwriteregion"></a>Select the write region

While all regions associated with your Cosmos DB database account can serve reads (both, single item as well as multi-item paginated reads) and queries, only one region can actively receive the write (insert, upsert, replace, delete) requests. To set the active write region, do the following  


1. In the **Azure Cosmos DB** blade, select the database account to modify.
2. In the account blade, click **Replicate data globally** from the menu.
3. In the **Replicate data globally** blade, click **Manual Failover** from the top bar.
    ![Change the write region under Azure Cosmos DB Account > Replicate data globally > Manual Failover][2]
4. Select a read region to become the new write region, click the checkbox to confirm triggering a failover, and click OK
    ![Change the write region by selecting a new region in list under Azure Cosmos DB Account > Replicate data globally > Manual Failover][3]

--->

<!--Image references-->
[1]: ./media/cosmos-db-tutorial-global-distribution-portal/azure-cosmos-db-add-region.png
[2]: ./media/cosmos-db-tutorial-global-distribution-portal/azure-cosmos-db-manual-failover-1.png
[3]: ./media/cosmos-db-tutorial-global-distribution-portal/azure-cosmos-db-manual-failover-2.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: ../articles/cosmos-db/consistency-levels.md
[azureregions]: https://azure.microsoft.com/regions/#services
[offers]: https://azure.microsoft.com/pricing/details/cosmos-db/
