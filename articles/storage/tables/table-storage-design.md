---
title: Tasarım ölçeklenebilir ve Azure tablo depolaması kullanıcı tablolarında. | Microsoft Docs
description: Tasarım ölçeklenebilir ve Azure tablo depolaması kullanıcı tablolarında.
services: storage
documentationcenter: na
author: SnehaGunda
manager: kfile
ms.assetid: 8e228b0c-2998-4462-8101-9f16517393ca
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/23/2018
ms.author: sngun
ms.openlocfilehash: ba48283371ac099b162aeb3cbffd6127ccce1676
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661047"
---
# <a name="design-scalable-and-performant-tables"></a>Ölçeklenebilir ve performansa yönelik tablolar tasarlayın

[!INCLUDE [storage-table-cosmos-db-tip-include](../../../includes/storage-table-cosmos-db-tip-include.md)]

Tasarım ölçeklenebilir ve kullanıcı tablolara, performans, ölçeklenebilirlik ve maliyet gibi etkenlere dikkate almanız gerekir. Daha önce şemaları ilişkisel veritabanları için tasarlanmış durumunda bu noktalar sahibiyseniz, ancak Azure Table hizmet depolama modeli ve ilişkisel modelleri arasında bazı benzerlikler olsa da, ayrıca önemli farklılıklar vardır. Bu farklılıklar genellikle counter-intuitive veya ilişkisel veritabanları ile tanıdık birine yanlış görünüyor, ancak Azure tablo hizmeti gibi bir NoSQL anahtar/değer deposu için tanımlıyorsanız anlamlı farklı tasarımlarını neden. Tasarım farklar çoğunu tablo hizmeti veri veya yüksek işlem desteklemelidir veri kümeleri için varlık (veya ilişkisel veritabanı terminolojisi satırlarda) milyarlarca içerebilir bulut ölçekli uygulamaları destekleyecek şekilde tasarlanmıştır olgu yansıtır birimler. Bu nedenle, farklı verilerinizi depolamak nasıl hakkında düşünün ve tablo hizmeti nasıl çalıştığını anlamanız gerekir. İyi tasarlanmış bir NoSQL veri deposu çok daha ve ilişkisel veritabanı kullanan bir çözüm daha düşük bir maliyetle ölçeklendirmek, çözümünüzün etkinleştirebilirsiniz. Bu kılavuz, bu konularda yardımcı olur.  

## <a name="about-the-azure-table-service"></a>Azure tablo hizmeti hakkında
Bu bölümde bazı performans ve ölçeklenebilirlik için tasarlama yakından ilgili olan anahtar özellikleri Tablo hizmetinin vurgular. Azure Storage ve tablo hizmeti yeniyseniz, ilk okuma [Microsoft Azure Storage'a giriş](../../storage/common/storage-introduction.md) ve [.NET kullanarak Azure Table Storage ile çalışmaya başlama](../../cosmos-db/table-storage-how-to-use-dotnet.md) önce bu makalenin sonraki bölümlerinde okuma . Bu kılavuzun odağı tablo hizmetinde olsa da, Azure kuyruk ve Blob Hizmetleri ve nasıl bunları tablo hizmetiyle kullanabilir tartışma içerir.  

Tablo hizmeti nedir? Tablo hizmeti tablosal biçimde adından beklediğiniz gibi verileri depolamak için kullanır. Standart terminoloji, tablodaki her satır bir varlığı temsil eden ve sütunları varlık çeşitli özelliklerini depolamak. Her varlığın bir çift ve tablo hizmeti varlık son güncelleştirildiği izlemek için kullandığı bir zaman damgası sütunu benzersiz şekilde tanımlamak için anahtar yok. Zaman damgası otomatik olarak uygulanır ve zaman damgası rasgele bir değerle el ile üzerine yazılamıyor. Tablo hizmeti iyimser eşzamanlılık yönetmek için bu son değiştirilen zaman damgası (LMT) kullanır.  

> [!NOTE]
> Tablo hizmeti REST API işlemleri aynı zamanda sonuç bir **ETag** LMT türetilen değeri. Aynı temel alınan veri başvurduğundan bu belgeyi ETag ve LMT terimleri birbirlerinin yerine kullanır.  
> 
> 

Aşağıdaki örnek, çalışan ve departman varlıkları depolamak için bir basit Tablo Tasarımı gösterir. Bu kılavuzda gösterilen örneklerin çoğu bu basit tasarım temel alır.  

<table>
<tr>
<th>PartitionKey</th>
<th>RowKey</th>
<th>Zaman damgası</th>
<th></th>
</tr>
<tr>
<td>Pazarlama</td>
<td>00001</td>
<td>2014-08-22T00:50:32Z</td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>Soyadı</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td>Tan</td>
<td>Hall</td>
<td>34</td>
<td>donh@contoso.com</td>
</tr>
</table>
</tr>
<tr>
<td>Pazarlama</td>
<td>00002</td>
<td>2014-08-22T00:50:34Z</td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>Soyadı</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td>Haz</td>
<td>CAO</td>
<td>47</td>
<td>junc@contoso.com</td>
</tr>
</table>
</tr>
<tr>
<td>Pazarlama</td>
<td>Bölüm</td>
<td>2014-08-22T00:50:30Z</td>
<td>
<table>
<tr>
<th>Bölüm adı</th>
<th>EmployeeCount</th>
</tr>
<tr>
<td>Pazarlama</td>
<td>153</td>
</tr>
</table>
</td>
</tr>
<tr>
<td>Satış</td>
<td>00010</td>
<td>2014-08-22T00:50:44Z</td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>Soyadı</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td>Ken</td>
<td>Kwok</td>
<td>23</td>
<td>kenk@contoso.com</td>
</tr>
</table>
</td>
</tr>
</table>


Şu ana kadar bu verileri zorunlu sütunları ve öğeleri aynı tabloda birden çok varlık türleri Depolama olanağı temel farklılıklar ile benzer şekilde ilişkisel bir veritabanındaki bir tablo görünür. Ayrıca, her kullanıcı tarafından tanımlanan özellikler gibi **FirstName** veya **yaş** tamsayı veya dize, yalnızca gibi ilişkisel bir veritabanındaki sütun gibi bir veri türüne sahip. Farklı bir ilişkisel veritabanı içinde tablo hizmeti şema daha az yapısını bir özellik aynı veri her varlığın türü olması gerekmez, ancak. Karmaşık veri türleri tek bir özellikte depolamak için JSON veya XML gibi serileştirilmiş bir format kullanmanız gerekir. Tablo hizmeti gibi desteklenen veri türleri, desteklenen bir tarih aralıkları, adlandırma kuralları ve boyutu kısıtlamaları hakkında daha fazla bilgi için bkz: [tablo hizmeti veri modelini anlama](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Tercih ettiğiniz **PartitionKey** ve **RowKey** iyi tablo tasarımı için temel önemdedir. Bir tablodaki her varlığın benzersiz bir birleşimi olmalıdır **PartitionKey** ve **RowKey**. Bir ilişkisel veritabanı tablosundaki gibi anahtarlarla **PartitionKey** ve **RowKey** değerleri dizine hızlı göz atmayı etkinleştirmek için kümelenmiş bir dizin oluşturmak için. Ancak, tablo hizmeti tüm ikincil dizinlerin şekilde oluşturmaz **PartitionKey** ve **RowKey** yalnızca Dizinli Özellikler. Bazı açıklanan desenleri [tablo Tasarım desenleri](table-storage-design-patterns.md) görünen bu sınırlamaya geçici nasıl çalıştığını gösteren.  

Bir tablo bir veya daha fazla bölüm oluşur ve tasarım kararları çoğunu uygun bir seçme geçici bir çözüm olacaktır **PartitionKey** ve **RowKey** çözümünüzü en iyi duruma getirme. Bir çözüm tüm varlıklarını bölümlere düzenlenmiş içeren tek bir tablonun oluşabilir, ancak genellikle birden çok tablo bir çözümü vardır. Tablolar, mantıksal olarak varlıklarınızı düzenlemek, erişim denetim listeleri kullanarak veri erişimi yönetmenize yardımcı olmak için Yardım ve tek bir depolama işlemi kullanarak tüm bir tabloyu düşürebilir.  

## <a name="table-partitions"></a>Tablo bölümleri
Tablo adı, hesap adı ve **PartitionKey** birlikte tablo hizmeti, varlığı depoladığı depolama hizmeti içinde bölümü tanımlayın. Adres düzeni varlıklar için parçası olmasının yanı sıra, bölüm işlemleri için kapsam tanımlayın (bkz [varlık Grup hareketleri](#entity-group-transactions) aşağıda), ve tablo hizmeti nasıl ölçeklendirir temeli oluştururlar. Bölümleri hakkında daha fazla bilgi için bkz: [Azure Storage ölçeklenebilirlik ve performans hedefleri](../../storage/common/storage-scalability-targets.md).  

Tablo hizmetinde hizmetlerini tek tek bir düğüm tek tek ya da daha fazla tamamlamak bölümler ve hizmet ölçekler dinamik Yük Dengeleme tarafından düğümleri arasında bölümler. Bir düğümü yük altında ise, tablo hizmeti için *bölme* bölümleri aralığını hizmet farklı düğümleri üzerine bu düğüm tarafından; trafiği subsides hizmet yapabilirsiniz *birleştirme* sessiz düğümleri bölüm aralığından tek bir düğüme geri.  

Daha fazla bilgi için iç Ayrıntılar tablo hizmetinin ve özellikle bölümleri hizmet yöneten nasıl incelemesine bakın [Microsoft Azure Storage: yüksek oranda kullanılabilir bulut depolama hizmet güçlü tutarlılık](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).  

## <a name="entity-group-transactions"></a>Varlık Grup işlemleri
Tablo hizmetinde varlık Grup hareketleri (EGTs) birden çok varlık arasında atomik güncelleştirmeleri gerçekleştirmek için yalnızca yerleşik sistemdir. EGTs bazen de denir *toplu işlemler*. EGTs aynı bölümünde depolanan varlıklar üzerinde yalnızca çalışabilir (diğer bir deyişle, belirli bir tablodaki aynı bölüm anahtarı paylaşımı). Bu nedenle birden çok varlık arasında atomik işlem davranışı gerektiren her zaman, bu varlıkların aynı bölümde olduğundan emin olmalısınız. Genellikle birden fazla tablo farklı varlık türlerine yönelik kullanmayan ve birden çok varlık türleri aynı tablonun (ve bölüm) tutulması için bir neden de budur. Tek bir EGT en fazla 100 varlık üzerinde çalışabilir.  İşleme için birden çok eşzamanlı EGTs gönderirseniz, bu EGTs EGTs arasında ortak olan varlıklar üzerinde çalıştırmayın olmak önemlidir; Aksi takdirde, işleme ertelenebilir.

EGTs de tasarımınızda değerlendirebilmeniz için olası bir denge tanıtır. Diğer bir deyişle, daha fazla bölüm kullanarak Azure düğümleri arasında dengelemesini istekleri için daha fazla fırsatlarını olduğundan, uygulamanızın ölçeklenebilirliğini artırır. Ancak daha fazla bölüm kullanarak atomik işlemleri gerçekleştirmek ve verileriniz için güçlü tutarlılık sağlamak için uygulamanızın özelliğini sınırlayabilir. Ayrıca, var. belirli ölçeklenebilirlik hedefleri için tek bir düğüm beklediğiniz işlemleri verimini sınırlayabilir bir bölümün düzeyinde Azure depolama hesapları ve tablo hizmeti ölçeklenebilirlik hedefleri hakkında daha fazla bilgi için bkz: [Azure Storage ölçeklenebilirlik ve performans hedefleri](../../storage/common/storage-scalability-targets.md).   

## <a name="capacity-considerations"></a>Kapasite dikkat edilecek noktalar
Aşağıdaki tabloda bazı ne zaman, bir tablo hizmeti çözümü tasarlarken bulundurmanız anahtar değerleri açıklanır:  

| Bir Azure depolama hesabının toplam kapasite | 500 TB |
| --- | --- |
| Bir Azure depolama hesabı tablo sayısı |Depolama hesabı kapasitesini yalnızca sınırlıdır |
| Bir tabloda bölüm sayısı |Depolama hesabı kapasitesini yalnızca sınırlıdır |
| Bir bölümdeki varlıkların sayısı |Depolama hesabı kapasitesini yalnızca sınırlıdır |
| Tek bir varlık boyutu |En çok 1 MB ile en fazla 255 özellikleri (de dahil olmak üzere **PartitionKey**, **RowKey**, ve **zaman damgası**) |
| Boyutunu **PartitionKey** |Dizenin en çok 1 KB boyutunda |
| Boyutunu **RowKey** |Dizenin en çok 1 KB boyutunda |
| Bir varlık grubu işlem boyutu |Bir işlem en fazla 100 varlık içerebilir ve yükü boyutu 4 MB'den az olmalıdır. Bir EGT yalnızca bir kez bir varlık güncelleştirebilirsiniz. |

Daha fazla bilgi için bkz: [tablo hizmeti veri modelini anlama](http://msdn.microsoft.com/library/azure/dd179338.aspx).  

## <a name="cost-considerations"></a>Maliyet dikkat edilecek noktalar
Tablo depolama oldukça masrafsız, ancak herhangi bir tablo hizmeti çözümü değerlendirmenize bir parçası olarak kapasite kullanımı ve hareketlerinin miktarı için maliyet tahminleri içermelidir. Bununla birlikte, birçok senaryoda performansı veya çözümünüzü ölçeklenebilirliğini geliştirmek için Normalleştirilmemiş veya yinelenen veri depolama bir geçerli bir yaklaşımdır. Fiyatlandırma hakkında daha fazla bilgi için bkz: [Azure Storage fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).  

## <a name="next-steps"></a>Sonraki adımlar

- [Tablo Tasarım desenleri](table-storage-design-patterns.md)
- [Modelleme ilişkileri](table-storage-design-modeling.md)
- [Sorgulama için Tasarım](table-storage-design-for-query.md)
- [Tablo verileri şifreleme](table-storage-design-encrypt-data.md)
- [Veri değişikliği için Tasarım](table-storage-design-for-modification.md)