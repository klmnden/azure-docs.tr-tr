---
title: "Azure portalı ile yeni bir esnek havuz oluşturma | Microsoft Belgeleri"
description: "Birçok veritabanı arasında daha kolay yönetim ve kaynak paylaşımı sağlamak amacıyla SQL veritabanı yapılandırmanıza ölçeklenebilir bir elastik havuz ekleme."
keywords: "ölçeklenebilir veritabanı,veritabanı yapılandırması"
services: sql-database
documentationcenter: 
author: ninarn
manager: jhubbard
editor: 
ms.assetid: bf12594b-d258-40e6-a9fc-d8a8710c2d65
ms.service: sql-database
ms.custom: multiple databases
ms.devlang: NA
ms.date: 11/17/2016
ms.author: ninarn
ms.workload: data-management
ms.topic: get-started-article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: 6c8420a154d998aa95c0220049ee54b3039a872b
ms.openlocfilehash: 4be8e4f81965fa4d872e29fdb9aaa45909d18c37


---
# <a name="create-a-new-elastic-pool-with-the-azure-portal"></a>Azure portalıyla yeni bir elastik havuz oluşturma
> [!div class="op_single_selector"]
> * [Azure Portal](sql-database-elastic-pool-create-portal.md)
> * [PowerShell](sql-database-elastic-pool-create-powershell.md)
> * [C#](sql-database-elastic-pool-create-csharp.md)
>

Bu makalede [Azure portalı](https://portal.azure.com/) ile nasıl ölçeklenebilir bir [esnek havuz](sql-database-elastic-pool.md) oluşturacağınız gösterilmiştir. Havuz oluşturmanın iki yolu vardır. İstediğiniz havuz kurulumunu biliyorsanız sıfırdan oluşturabilir veya hizmet tarafından sağlanan bir öneriyle başlayabilirsiniz. SQL Database, veritabanlarınızın geçmişe yönelik kullanım telemetrisini göz önünde bulundurarak sizin için maliyet açısından daha verimli bir havuz kurulumu olması halinde onu öneren yerleşik zekaya sahiptir.

Bir sunucuya birden fazla havuz ekleyebilirsiniz ancak aynı havuza farklı sunuculara ait veritabanlarını ekleyemezsiniz. Havuz oluşturmak için V12 sunucusunda en az bir veritabanınızın olması gerekir. Veritabanınız yoksa bkz. [İlk Azure SQL veritabanınızı oluşturma](sql-database-get-started.md). Yalnızca bir veritabanı ile havuz oluşturabilirsiniz ancak havuzlar yalnızca birden çok veritabanıyla kullanıldığında maliyet açısından verimlidir. Bkz. [Elastik havuzlar için fiyat ve performans ile ilgili dikkat edilmesi gerekenler](sql-database-elastic-pool-guidance.md).

> [!NOTE]
> Esnek havuzlar şu anda önizleme aşamasında oldukları Batı Hindistan dışında tüm Azure bölgelerinde genel olarak kullanılabilir (GA) durumdadır.  Bu bölgede esnek havuz GA’sı olabildiğince çabuk ortaya çıkar.
>
>

## <a name="step-1-create-a-new-pool"></a>1. Adım: Yeni bir havuz oluşturma

Bu makalede, var olan veritabanlarını bir havuza taşımanın en kolay yolu olarak portaldaki mevcut **sunucu** dikey penceresinden yeni bir havuz oluşturma işlemi gösterilmektedir.

> [!NOTE]
> Ayrıca, **Market**’te **SQL esnek havuzu**’nu arayarak veya **SQL esnek havuzları** dikey penceresindeki **+ Ekle** seçeneğine tıklayarak yeni bir havuz oluşturabilirsiniz. Havuz sağlama iş akışı, yeni veya mevcut bir sunucu belirtmenize olanak tanır.
>
>

1. [Azure portalında](http://portal.azure.com/) sol taraftaki listenin altındaki **Diğer hizmetler** **>** **SQL sunucuları** seçeneğine tıklayın ve ardından havuza eklemek istediğiniz veritabanlarının bulunduğu sunucuya tıklayın.
2. **Yeni havuz** düğmesine tıklayın.

    ![Sunucuya havuz ekleme](./media/sql-database-elastic-pool-create-portal/new-pool.png)

    **-VEYA-**

    Sunucu için önerilen esnek havuzların bulunduğunu belirten bir ileti görebilirsiniz (yalnızca V12). Geçmişe yönelik veritabanı kullanımı telemetrisi göz önünde bulundurularak önerilen havuzları görmek için iletiye tıkladıktan sonra daha fazla ayrıntı görmek ve havuzu özelleştirmek için katmana tıklayın. Önerinin nasıl yapıldığını görmek için makalenin sonraki bölümlerinde yer alan [Havuz önerilerini anlama](#understand-pool-recommendations) konu başlığına göz atın.

    ![önerilen havuz](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)

3. Havuzunuzun ayarlarını belirleyeceğiniz **elastik havuz** dikey penceresi görünür. Önceki adımda **Yeni havuz**’a tıkladıysanız, fiyatlandırma katmanı varsayılan olarak **Standart**’tır ve seçilmiş bir veri tabanı bulunmaz. Boş bir havuz oluşturabilir veya havuza taşımak için bu sunucudan var olan bir veritabanları kümesi belirtebilirsiniz. Önerilen bir havuzu oluşturuyorsanız; önerilen fiyatlandırma katmanı, performans ayarları ve veritabanları listesi önceden doldurulur ancak bunları değiştirebilirsiniz.

    ![Esnek havuzu yapılandırma](./media/sql-database-elastic-pool-create-portal/configure-elastic-pool.png)

4. Esnek havuz için bir ad belirtin veya varsayılan adı kullanın.

## <a name="step-2-choose-a-pricing-tier"></a>2. Adım: Fiyatlandırma katmanı seçme

Havuzun fiyatlandırma katmanı, havuzdaki esnek veritabanları için kullanılabilen özelliklerin yanı sıra her bir veritabanı için kullanılabilen maksimum eDTU (MAKS eDTU) sayısını ve depolama alanını (GB) belirler. Ayrıntılı bilgi için bkz. Hizmet Katmanları.

Havuz için fiyatlandırma katmanını değiştirmek üzere **Fiyatlandırma katmanı** seçeneğine tıklayın ve ardından istediğiniz fiyatlandırma katmanına tıklayıp **Seç**'e tıklayın.

> [!IMPORTANT]
> Son adımda fiyatlandırma katmanınızı seçip **Tamam**'a tıklayarak değişiklikleri uyguladıktan sonra havuzun fiyatlandırma katmanını değiştiremezsiniz. Var olan bir esnek havuzun fiyatlandırma katmanını değiştirmek için istenen fiyatlandırma katmanında yeni bir esnek havuz oluşturun ve esnek veritabanlarını bu yeni havuza geçirin.
>
>

![Fiyatlandırma katmanı seçme](./media/sql-database-elastic-pool-create-portal/pricing-tier.png)

## <a name="step-3-configure-the-pool"></a>3.Adım: Havuzu yapılandırma

Fiyatlandırma katmanını ayarladıktan sonra, Havuzu yapılandır'a tıklayın. Burada veritabanı ekleyebilir, havuz eDTU'larını ve depolama alanını (havuz GB'ları) ayarlayabilir ve havuzdaki esnek veritabanları için minimum ve maksimum eDTU'ları belirleyebilirsiniz.

1. **Havuzu yapılandır**'a tıklayın.
2. Havuza eklemek istediğiniz veritabanlarını seçin. Havuz oluşturulurken bu adım isteğe bağlıdır. Havuz oluşturulduktan sonra veritabanı eklenebilir.
    Veritabanı eklemek için **Veritabanı ekle**'ye tıkladıktan sonra eklemek istediğiniz veritabanlarına tıklayın ve ardından **Seç** düğmesine tıklayın.

    ![Veritabanı ekleme](./media/sql-database-elastic-pool-create-portal/add-databases.png)

    Kullandığınız veritabanları, geçmişe yönelik yeterli kullanım telemetrisine sahipse **Tahmini eDTU ve GB kullanımı** grafiği ve **Gerçek eDTU kullanımı** çubuk grafiği, yapılandırma kararları vermenize yardımcı olmak üzere güncelleştirilir. Ayrıca hizmet, havuzu düzgün bir şekilde boyutlandırmanıza yardımcı olmak üzere size bir öneri iletisi sunabilir. Bkz. [Dinamik Öneriler](#dynamic-recommendations).

3. Ayarları incelemek ve havuzunuzu yapılandırmak için **Havuz yapılandırma** sayfasındaki denetimleri kullanın. Her bir hizmet katmanına yönelik sınırlar hakkında daha ayrıntılı bilgi için bkz. [Elastik havuz sınırları](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools); havuzu düzgün bir şekilde boyutlandırmaya ilişkin ayrıntılı bir kılavuz için bkz. [Elastik havuzlar için fiyat ve performans ile ilgili dikkat edilmesi gerekenler](sql-database-elastic-pool-guidance.md). Havuz ayarları hakkında daha ayrıntılı bilgi için bkz. [Esnek havuz özellikleri](sql-database-elastic-pool.md#elastic-pool-properties).

    ![Esnek Havuzu Yapılandırma](./media/sql-database-elastic-pool-create-portal/configure-performance.png)

4. Ayarları değiştirdikten sonra **Havuzu Yapılandır** dikey penceresindeki **Seç** düğmesine tıklayın.
5. Havuzu oluşturmak için **Tamam**'a tıklayın.


## <a name="understand-pool-recommendations"></a>Havuz önerilerini anlama

SQL Database hizmeti, kullanım geçmişini değerlendirir ve maliyet açısından tek veritabanlarını kullanmaktan daha verimliyse bir veya daha fazla havuzun kullanılmasını önerir. Her bir öneri, havuza en iyi şekilde uyan sunucu veritabanlarının benzersiz bir alt kümesiyle yapılandırılır.

![önerilen havuz](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)  

Havuz önerisi şunları kapsar:

- Havuz için bir fiyatlandırma katmanı (Temel, Standart veya Premium)
- Uygun **HAVUZ eDTU'ları** (havuz başına maksimum eDTU olarak da adlandırılır)
- Veritabanı başına **Maksimum eDTU** ve **Minimum eDTU**
- Havuz için önerilen veritabanlarının listesi

Hizmet, havuz önerisinde bulunurken telemetrinin son 30 gününü dikkate alır. Bir veritabanının elastik havuz adayı olarak kabul edilebilmesi için en az 7 gündür var olması gerekir. Zaten bir elastik havuzda bulunan veritabanları, elastik havuz önerileri için aday olarak kabul edilmez.

Her bir hizmet katmanındaki tek veritabanının aynı katman havuzlarına taşınmasının maliyet açısından verimliliği ve kaynak ihtiyaçları hizmet tarafından değerlendirilir. Örneğin, bir sunucudaki tüm Standart veritabanları Standart Esnek Havuz'a uygunluk açısından değerlendirilir. Bu, hizmetin bir Standart veritabanının bir Premium havuza taşınması gibi çapraz katmanlı önerilerde bulunmadığı anlamına gelir.

### <a name="dynamic-recommendations"></a>Dinamik öneriler

Havuza veritabanı eklendikten sonra öneriler, seçtiğiniz veritabanlarının geçmişe yönelik kullanımları göz önünde bulundurularak dinamik olarak oluşturulur. Bu öneriler, **Havuzu yapılandır** dikey penceresinin üst kısmındaki bir öneri başlığının yanı sıra eDTU ve GB kullanım grafiğinde gösterilir. Bu öneriler, belirli veritabanlarınız için iyileştirilmiş bir havuz oluşturma konusunda size yardımcı olmak üzere tasarlanmıştır 

![dinamik öneriler](./media/sql-database-elastic-pool-create-portal/dynamic-recommendation.png)

## <a name="additional-resources"></a>Ek kaynaklar

- [Portal ile bir SQL Veritabanı esnek havuzunu yönetme](sql-database-elastic-pool-manage-portal.md)
- [PowerShell ile bir SQL Veritabanı esnek havuzunu yönetme](sql-database-elastic-pool-manage-powershell.md)
- [C# ile bir SQL Veritabanı esnek havuzunu yönetme](sql-database-elastic-pool-manage-csharp.md)
- [Azure SQL Veritabanı ile ölçek genişletme](sql-database-elastic-scale-introduction.md)



<!--HONumber=Jan17_HO4-->


