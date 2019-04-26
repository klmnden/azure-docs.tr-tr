---
title: Azure depolama Tablo Tasarımı ilişkileri modelleme | Microsoft Docs
description: Tablo depolama çözümünüzü tasarlarken modelleme işlemi anlayın.
services: storage
author: WenJason
ms.service: storage
ms.topic: article
origin.date: 04/23/2018
ms.date: 02/25/2019
ms.author: v-jay
ms.subservice: tables
ms.openlocfilehash: 5d83e61282d2f21a3016997e324d0f58eff15e78
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60502545"
---
# <a name="modeling-relationships"></a>İlişkileri modelleme
Bu makalede, Azure tablo depolama çözümleri tasarlamanıza yardımcı olacak modelleme işlemi açıklanmaktadır.

Etki alanı modellerini oluşturma karmaşık sistemlerin tasarımında önemli bir adımdır. Genellikle, varlıklar ve iş etki alanını anlamak ve sisteminizin tasarımına bildirmek için bir yol olarak arasındaki ilişkileri tanımlamak için modelleme işlemi kullanın. Bu bölüm, nasıl, bazı yaygın ilişki türleri tasarımı için tablo hizmeti için etki alanı modelleri bulunan çevirebilir üzerinde odaklanır. İlişkisel bir veritabanı tasarlarken kullanılan farklı bir fiziksel NoSQL tabanlı veri modeli için bir mantıksal veri modelinden eşleme işlemidir. İlişkisel veritabanları tasarım yedeklilik – ve nasıl uygulanması veritabanı işleyişi soyutlayan bir bildirim temelli sorgulanırken özelliği en aza indirmek için en iyi duruma getirilmiş veri normalleştirme işlemi genellikle varsayar.  

## <a name="one-to-many-relationships"></a>Bire çok ilişkileri
İş etki alanı nesneler arasındaki bire çok ilişkileri ortaya sık: Örneğin, bir departmandaki çalışanların çoğu vardır. Tablo hizmetinde her Artıları ve eksileri belirli senaryoya ilgili olabilecek ile bire çok ilişkileri uygulamak için birkaç yolu vardır.  

On binlerce Departmanlar ve çalışan varlıkların her departman çok sayıda çalışan ve her çalışana belirli bir bölümle ilişkili olarak sahip olduğu büyük bir Uluslararası Şirket örneği göz önünde bulundurun. Ayrı bir departman ve bunlar gibi çalışan varlıkları depolamak için bir yaklaşım şöyledir:  


![Ayrı bir departman ve çalışan varlıkları Store](media/storage-table-design-guide/storage-table-design-IMAGE01.png)

Bu örnek, temel türleri arasında örtük bir-çok ilişkisi gösterir **PartitionKey** değeri. Her departmandaki çalışanların çoğu olabilir.  

Bu örnek ayrıca departmanı varlık ve ilgili çalışan varlıkları aynı bölümde gösterir. Farklı bölümleri, tablolar veya hatta depolama hesapları farklı varlık türleri için kullanılacak seçebilirsiniz.  

Verileri normalleştirilmişlikten çıkarmak ve yalnızca aşağıdaki örnekte gösterildiği gibi normalleştirilmişlikten çıkarılmış departman verilerine sahip çalışan varlıklar depolamak için alternatif bir yoludur. Bunu yapmak için her çalışanın departmanındaki güncelleştirmeye gerek duyduğunuz olduğundan Bölüm Yöneticisi'nin ayrıntılar değişiklik yapabilmek için bir gereksinimi varsa bu belirli bir senaryoda en iyi normalleştirilmişlikten çıkarılmış bu yaklaşım olmayabilir.  

![Çalışan varlık](media/storage-table-design-guide/storage-table-design-IMAGE02.png)

Daha fazla bilgi için [normalleştirilmişlikten çıkarma deseni](table-storage-design-patterns.md#denormalization-pattern) bu kılavuzun sonraki.  

Aşağıdaki tabloda, Artıları ve eksileri her çalışan ve bir-çok ilişkisi departmanı varlıkları depolamak için yukarıda açıklanan yaklaşımlardan biri özetlenmektedir. Ne sıklıkta çeşitli işlemler gerçekleştirmek beklediğiniz düşünmelisiniz: Bu işlem yalnızca sık meydana gelirse, pahalı bir işlem içeren bir tasarım için kabul edilebilir olabilir.  

<table>
<tr>
<th>Yaklaşım</th>
<th>Uzmanları</th>
<th>Simgeler</th>
</tr>
<tr>
<td>Ayrı varlık türleri, aynı bölüme, aynı tabloya</td>
<td>
<ul>
<li>Tek bir işlemle bir departman varlık güncelleştirebilirsiniz.</li>
<li>Bir EGT departmanı varlığı değiştirmek için bir gereksinimi varsa tutarlılık sağlamak için kullanabileceğiniz olduğunda, güncelleştirme/ekleme/silme çalışan varlık. Örneğin, bir departman çalışan sayısı her departman için korur.</li>
</ul>
</td>
<td>
<ul>
<li>Hem çalışan hem de bazı istemci etkinlikleri için bir bölüm varlık almanız gerekebilir.</li>
<li>Depolama işlemleri aynı bölümde gerçekleşir. Yüksek işlem birimleri, bir etkin nokta bu neden olabilir.</li>
<li>Bir çalışan bir EGT kullanarak yeni bir bölüme taşınamıyor.</li>
</ul>
</td>
</tr>
<tr>
<td>Ayrı varlık türleri, farklı bölümler veya tablolar veya depolama hesapları</td>
<td>
<ul>
<li>Bir departman veya çalışan bir varlıktan tek bir işlemle güncelleştirebilirsiniz.</li>
<li>Yüksek işlem hacmi, bu yükü daha fazla bölümler arasında yaymak yardımcı olabilir.</li>
</ul>
</td>
<td>
<ul>
<li>Hem çalışan hem de bazı istemci etkinlikleri için bir bölüm varlık almanız gerekebilir.</li>
<li>Tutarlılık sağlamak için EGTs kullanılamaz olduğunda, güncelleştirme/ekleme/silme bir çalışan ve güncelleştirme bir bölüm. Örneğin, bir çalışan sayısı departmanı varlıklardaki güncelleştiriliyor.</li>
<li>Bir çalışan bir EGT kullanarak yeni bir bölüme taşınamıyor.</li>
</ul>
</td>
</tr>
<tr>
<td>Tek varlık türüne normalleştirmeyi geri alın</td>
<td>
<ul>
<li>Tek bir istekle gereken tüm bilgileri alabilirsiniz.</li>
</ul>
</td>
<td>
<ul>
<li>Bölüm bilgileri (Bu, bir departmandaki tüm çalışanlar güncelleştirmenizi gerektirir) güncelleştirmeniz gerekiyorsa tutarlılık sağlamak pahalı olabilir.</li>
</ul>
</td>
</tr>
</table>

Nasıl Bu seçenekler ve Artıları ve eksileri hangisinin en önemli, belirli bir uygulama senaryolarınız bağlıdır arasında seçin. Örneğin, ne sıklıkta, departman varlıkları değiştirmeyin; Tüm çalışan sorguları ek departman bilgi gerekiyor; ölçeklenebilirlik sınırları, bölüm veya depolama hesabınız için ne kadar yakın misiniz?  

## <a name="one-to-one-relationships"></a>Bire bir ilişkiler
Etki alanı modelleri varlıklar arasında bire bir ilişkiler içerebilir. Tablo hizmetinde bire bir ilişki uygulamak gerekiyorsa, her ikisinin de almanız gerektiğinde, iki ilişkili varlık bağlama da seçmeniz gerekir. Bu bağlantıyı bağlantı biçiminde depolayarak örtük, anahtar değerlerinin bir kurala göre veya açık olabilir **PartitionKey** ve **RowKey** her varlık kendi ilgili varlık için değerler. İlgili varlıkları aynı bölümde olup saklamalısınız bir açıklaması için konudaki [bire çok ilişkileri](#one-to-many-relationships).  

Tablo hizmetinde bire bir ilişkiler uygulamak için yol açabilecek bir uygulama konuları vardır:  

* Büyük varlıklar işleme (daha fazla bilgi için [büyük varlıklar deseni](table-storage-design-patterns.md#large-entities-pattern)).  
* Erişim denetimleri uygulama (daha fazla bilgi için paylaşılan erişim imzaları ile erişimi denetleme bakın).  

## <a name="join-in-the-client"></a>İstemci katılın
Tablo hizmetinde ilişkileri modellemek için yol olsa da, tablo hizmeti kullanarak iki ana nedeni ölçeklenebilirlik ve performans olduğunu unutmayın. Performans ve ölçeklenebilirlik çözümünüzün tehlikeye çok ilişkileri modelleme fark ederseniz, tablo tasarımınızla tüm veri ilişkileri oluşturmak gerekli olup olmadığını kendiniz istemeniz gerekir. Tasarımınızı basitleştirmek ve istemci uygulamanızı gerekli tüm birleştirmeler gerçekleştirme izin verirseniz, çözümünüzün performansını ve ölçeklenebilirliğini artırmak mümkün olabilir.  

Örneğin, genellikle değiştirmez veriler içeren küçük tablolar varsa, daha sonra bu verileri bir kez alabilir ve istemcide önbellek. Bu, aynı verileri almak için yinelenen gidiş-dönüş önleyebilirsiniz. Bu kılavuzda inceledik örneklerde, küçük ve seyrek veri ara olarak istemci uygulama bir kez indirebilirsiniz verileri ve önbellek için iyi bir adaydır yapmadan değiştirmek küçük bir kuruluş departmanlara kümesini olasıdır.  

## <a name="inheritance-relationships"></a>Kalıtım ilişkileri
İstemci uygulamanız bir dizi iş varlığı temsil eden bir devralma ilişkisi parçasını sınıfı kullanıyorsa, bu varlıkların tablo hizmetindeki kolayca kalıcı hale getirebilirsiniz. Örneğin, aşağıdaki istemci uygulamanızda tanımlanan sınıflar kümesini olabilir. burada **kişi** bir Özet sınıf.

![Soyut kişi sınıfı](media/storage-table-design-guide/storage-table-design-IMAGE03.png)

Bu görünüm bu gibi varlıkları kullanarak tek bir kişi tablosunu kullanarak tablo hizmetindeki iki somut sınıfların örneklerini kalıcı hale getirebilirsiniz:  

![Kişi tablosu](media/storage-table-design-guide/storage-table-design-IMAGE04.png)

İstemci kodu aynı tabloda birden fazla varlık türleri ile çalışma hakkında daha fazla bilgi için bkz bu kılavuzun devamında heterojen varlık türleri ile çalışma. Bu varlık türünde istemci kodu anlamayı örnekleri sağlar.  


## <a name="next-steps"></a>Sonraki adımlar

- [Tablo Tasarım desenleri](table-storage-design-patterns.md)
- [Sorgulama için Tasarım](table-storage-design-for-query.md)
- [Tablo verilerini şifreleme](table-storage-design-encrypt-data.md)
- [Veri değişikliği için Tasarım](table-storage-design-for-modification.md)