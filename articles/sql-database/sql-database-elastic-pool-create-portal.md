<properties
    pageTitle="Azure portalıyla yeni bir esnek havuz oluşturma | Microsoft Azure"
    description="Birçok veritabanı arasında daha kolay yönetim ve kaynak paylaşımı sağlamak amacıyla SQL veritabanı yapılandırmanıza ölçeklenebilir bir esnek veritabanı havuzu ekleme."
    keywords="ölçeklenebilir veritabanı,veritabanı yapılandırması"
    services="sql-database"
    documentationCenter=""
    authors="ninarn"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="07/20/2016"
    ms.author="ninarn"
    ms.workload="data-management"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="NA"/>


# Azure portalıyla yeni bir esnek veritabanı havuzu oluşturma

> [AZURE.SELECTOR]
- [Azure portalı](sql-database-elastic-pool-create-portal.md)
- [PowerShell](sql-database-elastic-pool-create-powershell.md)
- [C#](sql-database-elastic-pool-create-csharp.md)

Bu makalede [Azure portalı](https://portal.azure.com/) ile nasıl ölçeklenebilir bir [esnek veritabanı havuzu](sql-database-elastic-pool.md) oluşturacağınız gösterilmiştir. Havuz oluşturmanın iki yolu vardır. İstediğiniz havuz kurulumunu biliyorsanız sıfırdan oluşturabilir veya hizmet tarafından sağlanan bir öneriyle başlayabilirsiniz. SQL Database, veritabanlarınızın geçmişe yönelik kullanım telemetrisini göz önünde bulundurarak sizin için maliyet açısından daha verimli bir havuz kurulumu olması halinde onu öneren yerleşik zekaya sahiptir.

Bir sunucuya birden fazla havuz ekleyebilirsiniz ancak aynı havuza farklı sunuculara ait veritabanlarını ekleyemezsiniz. Havuz oluşturmak için V12 sunucusunda en az bir veritabanınızın olması gerekir. Veritabanınız yoksa bkz. [İlk Azure SQL veritabanınızı oluşturma](sql-database-get-started.md). Yalnızca bir veritabanı ile havuz oluşturabilirsiniz ancak havuzlar yalnızca birden çok veritabanıyla kullanıldığında maliyet açısından verimlidir. Bkz. [Esnek veritabanı havuzuna ilişkin fiyat ve performans ile ilgili dikkat edilmesi gerekenler](sql-database-elastic-pool-guidance.md).

> [AZURE.NOTE] Esnek havuzlar şu anda önizleme aşamasında oldukları Orta Kuzey ABD ve Batı Hindistan dışında tüm Azure bölgelerinde genel olarak kullanılabilir (GA) durumdadır.  Bu bölgelerdeki esnek havuzların GA durumları en kısa sürede bildirilecektir. Ayrıca, esnek havuzlar şu anda [bellek içi OLTP veya bellek içi analiz](sql-database-in-memory.md) kullanan veritabanlarını desteklememektedir.

## 1. Adım: Yeni bir havuz oluşturma

Bu makalede, var olan veritabanlarını bir havuza taşımanın en kolay yolu olarak portaldaki mevcut **sunucu** dikey penceresinden yeni bir havuz oluşturma işlemi gösterilmektedir. 

> [AZURE.NOTE] Zaten bir sunucunuzun olup olmamasına bakılmaksızın **SQL esnek havuzları** dikey penceresinden de yeni bir havuz oluşturabilirsiniz (portalın sol tarafındaki listenin altında **Gözat** **>** **SQL esnek havuzları**’na tıklayın). **SQL esnek havuzları** dikey penceresinde **+Ekle**’ye tıklanması havuz hazırlama iş akışı sırasında yeni bir sunucu oluşturma adımlarını sağlar.

1. [Azure portalında](http://portal.azure.com/) sol taraftaki listenin altındaki **Gözat** **>** **SQL sunucuları** seçeneğine tıklayın ve ardından havuza eklemek istediğiniz veritabanlarının bulunduğu sunucuya tıklayın.
2. **Yeni havuz** düğmesine tıklayın.

    ![Sunucuya havuz ekleme](./media/sql-database-elastic-pool-create-portal/new-pool.png)

    **-VEYA-**

    Sunucu için önerilen esnek veritabanı havuzlarının bulunduğunu belirten bir ileti görebilirsiniz (yalnızca V12). Geçmişe yönelik veritabanı kullanımı telemetrisi göz önünde bulundurularak önerilen havuzları görmek için iletiye tıkladıktan sonra daha fazla ayrıntı görmek ve havuzu özelleştirmek için katmana tıklayın. Önerinin nasıl yapıldığını görmek için makalenin sonraki bölümlerinde yer alan [Havuz önerilerini anlama](#understand-pool-recommendations) konu başlığına göz atın.

    ![önerilen havuz](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)

    Havuzunuzu ayarlayacağınız **Esnek veritabanı havuzu** dikey penceresi görünür. Önceki adımda **Yeni havuz** düğmesine tıkladıysanız portal, **Fiyatlandırma katmanı** altında bir **Standart havuz**, benzersiz bir **Ad** ve havuz için varsayılan bir yapılandırma belirler. Önerilen bir havuzu seçtiyseniz önerilen katman ve havuz yapılandırması seçili olarak gelir ancak yine de bunları değiştirebilirsiniz.

    ![Esnek havuzu yapılandırma](./media/sql-database-elastic-pool-create-portal/configure-elastic-pool.png)

3. Esnek havuz için bir ad belirtin veya varsayılan adı kullanın.

## 2. Adım: Fiyatlandırma katmanı seçme

Havuzun fiyatlandırma katmanı, havuzdaki esnek veritabanları için kullanılabilen özelliklerin yanı sıra her bir veritabanı için kullanılabilen maksimum eDTU (MAKS eDTU) sayısını ve depolama alanını (GB) belirler. Ayrıntılı bilgi için bkz. Hizmet Katmanları.

Havuz için fiyatlandırma katmanını değiştirmek üzere **Fiyatlandırma katmanı** seçeneğine tıklayın ve ardından istediğiniz fiyatlandırma katmanına tıklayıp **Seç**'e tıklayın.

> [AZURE.IMPORTANT] Son adımda fiyatlandırma katmanınızı seçip **Tamam**'a tıklayarak değişiklikleri uyguladıktan sonra havuzun fiyatlandırma katmanını değiştiremezsiniz. Var olan bir esnek havuzun fiyatlandırma katmanını değiştirmek için istenen fiyatlandırma katmanında yeni bir esnek havuz oluşturun ve esnek veritabanlarını bu yeni havuza geçirin.

![Fiyatlandırma katmanı seçme](./media/sql-database-elastic-pool-create-portal/pricing-tier.png)

## 3.Adım: Havuzu yapılandırma

Fiyatlandırma katmanını ayarladıktan sonra, Havuzu yapılandır'a tıklayın. Burada veritabanı ekleyebilir, havuz eDTU'larını ve depolama alanını (havuz GB'ları) ayarlayabilir ve havuzdaki esnek veritabanları için minimum ve maksimum eDTU'ları belirleyebilirsiniz.

1. **Havuzu yapılandır**'a tıklayın.
2. Havuza eklemek istediğiniz veritabanlarını seçin. Havuz oluşturulurken bu adım isteğe bağlıdır. Havuz oluşturulduktan sonra veritabanı eklenebilir.
    Veritabanı eklemek için **Veritabanı ekle**'ye tıkladıktan sonra eklemek istediğiniz veritabanlarına tıklayın ve ardından **Seç** düğmesine tıklayın.

    ![Veritabanı ekleme](./media/sql-database-elastic-pool-create-portal/add-databases.png)

    Kullandığınız veritabanları, geçmişe yönelik yeterli kullanım telemetrisine sahipse **Tahmini eDTU ve GB kullanımı** grafiği ve **Gerçek eDTU kullanımı** çubuk grafiği, yapılandırma kararları vermenize yardımcı olmak üzere güncelleştirilir. Ayrıca hizmet, havuzu düzgün bir şekilde boyutlandırmanıza yardımcı olmak üzere size bir öneri iletisi sunabilir. Bkz. [Dinamik Öneriler](#dynamic-recommendations).

3. Ayarları incelemek ve havuzunuzu yapılandırmak için **Havuz yapılandırma** sayfasındaki denetimleri kullanın. Her bir hizmet katmanına yönelik sınırlar hakkında daha ayrıntılı bilgi için bkz. [Esnek havuz sınırları](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools-and-elastic-databases); havuzu düzgün bir şekilde boyutlandırmaya ilişkin ayrıntılı bir kılavuz için bkz. [Esnek veritabanı havuzlarına ilişkin fiyat ve performans ile ilgili dikkat edilmesi gerekenler](sql-database-elastic-pool-guidance.md). Havuz ayarları hakkında daha ayrıntılı bilgi için bkz. [Esnek veritabanı havuzu özellikleri](sql-database-elastic-pool.md#elastic-database-pool-properties).

    ![Esnek Havuzu Yapılandırma](./media/sql-database-elastic-pool-create-portal/configure-performance.png)

4. Ayarları değiştirdikten sonra **Havuzu Yapılandır** dikey penceresindeki **Seç** düğmesine tıklayın.
5. Havuzu oluşturmak için **Tamam**'a tıklayın.


## Havuz önerilerini anlama

SQL Database hizmeti, kullanım geçmişini değerlendirir ve maliyet açısından tek veritabanlarını kullanmaktan daha verimliyse bir veya daha fazla havuzun kullanılmasını önerir. Her bir öneri, havuza en iyi şekilde uyan sunucu veritabanlarının benzersiz bir alt kümesiyle yapılandırılır.

![önerilen havuz](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)  

Havuz önerisi şunları kapsar:

- Havuz için bir fiyatlandırma katmanı (Temel, Standart veya Premium)
- Uygun **HAVUZ eDTU'ları** (havuz başına maksimum eDTU olarak da adlandırılır)
- Veritabanı başına **Maksimum eDTU** ve **Minimum eDTU**
- Havuz için önerilen veritabanlarının listesi

Hizmet, havuz önerisinde bulunurken telemetrinin son 30 gününü dikkate alır. Bir veritabanının esnek veritabanı havuzu adayı olarak kabul edilebilmesi için en az 7 gündür var olması gerekir. Zaten bir esnek veritabanı havuzunda bulunan veritabanları, esnek veritabanı havuz önerileri için aday olarak kabul edilmez.

Her bir hizmet katmanındaki tek veritabanının aynı katman havuzlarına taşınmasının maliyet açısından verimliliği ve kaynak ihtiyaçları hizmet tarafından değerlendirilir. Örneğin, bir sunucudaki tüm Standart veritabanları Standart Esnek Havuz'a uygunluk açısından değerlendirilir. Bu, hizmetin bir Standart veritabanının bir Premium havuza taşınması gibi çapraz katmanlı önerilerde bulunmadığı anlamına gelir.

### Dinamik öneriler

Havuza veritabanı eklendikten sonra öneriler, seçtiğiniz veritabanlarının geçmişe yönelik kullanımları göz önünde bulundurularak dinamik olarak oluşturulur. Bu öneriler, **Havuzu yapılandır** dikey penceresinin üst kısmındaki bir öneri başlığının yanı sıra eDTU ve GB kullanım grafiğinde gösterilir. Bu öneriler, belirli veritabanlarınız için iyileştirilmiş bir havuz oluşturma konusunda size yardımcı olmak üzere tasarlanmıştır 

![dinamik öneriler](./media/sql-database-elastic-pool-create-portal/dynamic-recommendation.png)

## Ek kaynaklar

- [Portal ile bir SQL Database esnek havuzunu yönetme](sql-database-elastic-pool-manage-portal.md)
- [PowerShell ile bir SQL Database esnek havuzunu yönetme](sql-database-elastic-pool-manage-powershell.md)
- [C ile bir SQL Database esnek havuzunu yönetme#](sql-database-elastic-pool-manage-csharp.md)
- [Azure SQL Database ile ölçek genişletme](sql-database-elastic-scale-introduction.md) 




<!--HONumber=sep16_HO1-->


