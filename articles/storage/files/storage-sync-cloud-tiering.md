---
title: Understanding Azure dosya eşitleme bulut Katmanlaması | Microsoft Docs
description: Azure dosya eşitleme'nin özellikleri hakkında bilgi bulut Katmanlandırma öğrenin
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 09/21/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 1851e9b2bb5ff86583228136dee977001cf0a3fd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64714956"
---
# <a name="cloud-tiering-overview"></a>Bulut katmanlama genel bakış
Bulut katmanlaması olduğundan, sık erişilen dosyaları önbelleğe alınır yerel sunucuda tüm dosyaları Azure İlkesi ayarlarına göre dosyaları katmanlı sırasında Azure dosya eşitleme'nin isteğe bağlı bir özelliktir. Bir dosya katmanlı, Azure dosya eşitleme dosya sistemi filtresi (StorageSync.sys) dosyasını yerel olarak bir işaretçi veya yeniden ayrıştırma noktası ile değiştirir. Yeniden ayrıştırma noktası, Azure dosyaları'nda bir dosyaya bir URL temsil eder. Katmanlanmış bir dosyanın, hem "Çevrimdışı" özniteliği hem de üçüncü taraf uygulamaların katmanlı dosyaları güvenli bir şekilde belirleyebilmek NTFS FILE_ATTRIBUTE_RECALL_ON_DATA_ACCESS özniteliği vardır.
 
Bir kullanıcı bir katmanlı dosya açıldığında, Azure dosya eşitleme kullanıcının dosyayı gerçekten Azure'da depolanan bilmenize gerek olmadan Azure dosyaları dosya verileri sorunsuz bir şekilde çeker. 
 
 > [!Important]  
 > Bulut katmanlaması desteklenmiyor Windows sistemi birimlerinde sunucu uç noktaları için ve yalnızca 64 KiB boyutu büyüktür dosyalara Azure dosyaları'na katmanlı.
    
Azure dosya eşitleme katmanlama ve tür küçük dosyaları geri çağırma performans yükü tasarruf daha ağır basar gibi 64 KiB küçük katmanlama dosyalarını desteklemez.

 > [!Important]  
 > Katmanlı dosyaları geri çekmek için ağ bant genişliğini en az 1 MB/sn olmalıdır. Ağ bant genişliği 1'den az MB/sn, dosyaları bir zaman aşımı hatası ile geri çağırma başarısız olabilir.

## <a name="cloud-tiering-faq"></a>Bulut Katmanlaması SSS

<a id="afs-cloud-tiering"></a>
### <a name="how-does-cloud-tiering-work"></a>Nasıl katmanlama çalışır?
Azure dosya eşitleme sistemi filtresi, ad alanınızın "ısı Haritası" her sunucu uç noktasında oluşturur. Zaman içinde (okuma ve yazma işlemleri) erişir izler ve ardından, sıklığı ve erişim eden temel alınarak, ısı bir atar her dosyaya puanlamak. Şekilleri bilgiler ve bir süre için erişilemeyen bir dosya seyrek erişimli olarak değerlendirilir, ancak kısa bir süre önce açıldı sık erişilen bir dosya sık erişimli, kabul edilir. Bir sunucuda dosya birimi ayarladığınız birimi boş alan eşiği aştığında, boş alan yüzdesi karşılanana kadar ilgilendiği dosyaları Azure dosyaları katmanı.

Sürümlerinde 4.0 ve yukarıda Azure dosya eşitleme aracısının, ayrıca bir tarih ilke değil erişilen veya belirtilen gün sayısı içinde değiştirilen dosyaları katmanı her bir sunucu uç noktasında belirtebilirsiniz.

<a id="afs-volume-free-space"></a>
### <a name="how-does-the-volume-free-space-tiering-policy-work"></a>Birim boş alanı katmanlama İlkesi nasıl çalışır?
Birim boş alanı sunucu uç noktası bulunduğu birimde ayırmak istediğiniz boş alan miktarıdır. Örneğin, bu alana uymayan kalan dosyalarla birlikte en son erişilen dosyaların Azure'a katmanlı, birim boş alanı %20 bir sunucu uç noktası olan bir birim üzerinde ayarlanır, Yukarı 80 oranında birim alanı tarafından kullanılıyor. Birim boş alanı birim düzeyinde yerine tek tek dizinleri veya eşitleme grubu düzeyinde uygulanır. 

<a id="volume-free-space-fastdr"></a>
### <a name="how-does-the-volume-free-space-tiering-policy-work-with-regards-to-new-server-endpoints"></a>Birim boş alanı katmanlama ilkesini, yeni sunucu uç noktaları bakımından nasıl çalışır?
Sunucu uç noktası yeni sağlanır ve bir Azure dosya paylaşımına bağlı sunucu önce ad alanı çeker ve birim boş alanı eşiğini İsabetleri kadar ardından gerçek dosyaları çeker. Bu işlem de hızlı bir olağanüstü durum kurtarma veya hızlı ad alanı geri yükleme denir.

<a id="afs-effective-vfs"></a>
### <a name="how-is-volume-free-space-interpreted-when-i-have-multiple-server-endpoints-on-a-volume"></a>Birden çok sunucu uç noktaları bir birimde olduğunda birim boş alanı nasıl yorumlanacağını?
Bir birimde birden fazla sunucu uç noktası olduğunda, geçerli toplu boş alan eşik o birimdeki tüm sunucu uç noktası üzerinden belirtilen en büyük birim boş alanı olur. Kullanım düzenlerini göre ait oldukları hangi sunucu uç noktası bağımsız olarak dosya katmanlanmış olmaz. Bir birimde, bitiş noktası 1 ve Endpoint2, iki sunucu uç noktaları varsa, örneğin, burada bitiş noktası 1 birim boş alanı eşik % 25 varsa ve Endpoint2 birimi boş alan eşiği % 50, her iki sunucu uç noktaları için birim boş alan eşik % 50 olacaktır. 

<a id="date-tiering-policy"></a>
### <a name="how-does-the-date-tiering-policy-work-in-conjunction-with-the-volume-free-space-tiering-policy"></a>Tarih katmanlama İlkesi birlikte birim boş alanı katmanlama İlkesi nasıl çalışır? 
Bulut katmanlaması bir sunucu uç noktasında etkinleştirirken, bir birim boş alanı ilke ayarlayın. Tarih politikası dahil diğer herhangi bir ilke, her zaman öncelik kazanır. İsteğe bağlı olarak, ilke için bir birim, bu ilke açıklar gün aralığında yalnızca dosyaları (yani ise, okuma veya yazma) erişilen anlamı olan staler dosyalar yerel saklanacağı her bir sunucu uç noktasında katmanlı bir tarih etkinleştirebilirsiniz. Birim boş alanı ilke her zaman önceliklidir ve tarih ilke tarafından açıklandığı dosyaları sayıda gün değerinde tutmak için birimde yeterli boş alan olmadığında Azure dosya eşitleme soğuk dosyaları katmanlama ücretsiz birim kadar devam eder göz önünde bulundurun alan yüzdesi karşılanır.

Örneğin, bir tarih temelli katmanlama İlkesi 60 gün ve birim boş alanı ilkesi % 20'ı olduğunu varsayalım. Tarih ilkesi uygulandıktan sonra varsa, daha az % 20'si birimdeki boş alan, birim boş alanı ilkesi etkisini göstermeye ve tarih ilkeyi geçersiz kılar. Sunucuda tutulan veri miktarı için 45 gün veri 60 günden azaltılabilir şekilde bu katmanlı veya daha çok dosya neden olur. Buna karşılık, katmanlama toplu boş olsa bile 61 günden eski olan dosya katmanlanmış olmaz böylece size, boş alan eşik – ulaşmış olabilirsiniz değil olsa bile, zaman aralığı dışında kalan dosyaları bu ilkeyi zorlar.

<a id="volume-free-space-guidelines"></a>
### <a name="how-do-i-determine-the-appropriate-amount-of-volume-free-space"></a>Uygun birim boş alanı miktarını nasıl belirlerim?
Yerel tutmak veri miktarı bazı faktörler tarafından belirlenir: bant genişliğiniz, kümenizin erişim düzeni ve bütçenizi. Düşük bant genişlikli bağlantı varsa, verilerinizin daha fazla kullanıcılarınız için en az bir gecikme var. olmak yerel tutmak isteyebilirsiniz. Aksi takdirde, bu değişim sıklığı oranı belirli bir süre boyunca temel alabilir. Yaklaşık 1 TB veri kümesi değişikliklerinizi %10 bilmiyorsanız veya 100 GB yerel tutmak isteyebileceğiniz sonra her ay etkin olarak erişilen Örneğin, bu nedenle, sık dosyaları geri çağırma değil. Toplu 2 TB'dir sonra %5 tutmak isteyeceksiniz (veya 100 GB), kalan anlamına gelir % 95, birim boş alan yüzdesi yereldir. Ancak, daha yüksek değişim sıklığı – süreleri için hesap için bir arabellek eklediğiniz diğer bir deyişle, daha düşük bir birim boş alan yüzdesi ile başlayan ve daha sonra gerekirse ayarlama öneririz. 

Daha fazla veri yerel tutarak Azure'dan daha az dosya çekilmesine daha düşük kullanım maliyetleri anlamına gelir, ancak kendi maliyetlerine şirket içi depolama, daha büyük bir miktarını korumak gerektirir. Azure dosya eşitleme dağıtılan örneğini oluşturduktan sonra birim boş alanı ayarlarınızı kullanımınız için uygun olup olmadığını kabaca ölçmek için depolama hesabınızın çıkış göz atabilirsiniz. Yalnızca, Azure dosya eşitleme bulut uç noktası (diğer bir deyişle, eşitleme paylaşımı) içeren depolama hesabını varsayılarak ve çok sayıda dosya buluttan geri yüklenir ve yerel önbelleğinizi artırmayı denemelisiniz yüksek çıkış anlamına gelir.

<a id="how-long-until-my-files-tier"></a>
### <a name="ive-added-a-new-server-endpoint-how-long-until-my-files-on-this-server-tier"></a>Yeni bir sunucu uç noktası ekledim. Ne kadar süreyle dosyalarımı kadar bu sunucu katmanı üzerinde?
Sürümlerinde 4.0 ve dosyalarınızı Azure dosya paylaşımına karşıya yüklendikten sonra yukarıda Azure dosya eşitleme aracısının, bunlar ilkelerinize göre saatte bir gerçekleşir sonraki katmanlama oturumu çalıştırmaların olan en kısa sürede katmanlanmış olmaz. Eski aracıda katmanlama gerçekleşmesi 24 saat sürebilir.

<a id="is-my-file-tiered"></a>
### <a name="how-can-i-tell-whether-a-file-has-been-tiered"></a>Bir dosya katmanlanmış olup olmadığını nasıl anlayabilirim?
Azure dosya paylaşımınıza dosya katmanlanmış olup olmadığını denetlemek için birkaç yolu vardır:
    
   *  **Dosyanın dosya özniteliklerini denetleyin.**
     Bir dosyaya sağ tıklayın, Git **ayrıntıları**, ardından ekranı aşağı kaydırarak **öznitelikleri** özelliği. Katmanlanmış bir dosya kümesi öznitelikleri şunlardır:     
        
        | Öznitelik harf | Öznitelik | Tanım |
        |:----------------:|-----------|------------|
        | A | Arşiv | Yedekleme yazılımı ile dosya yedeklenmeli gösterir. Bu öznitelik her zaman, dosyanın veya katmanlı tam olarak diskte depolanan bağımsız olarak ayarlanır. |
        | P | Seyrek dosya | Dosyası seyrek dosya olduğunu gösterir. Dosyanın disk akışında çoğunlukla boş olduğunda seyrek dosya NTFS sunan dosyasının verimli kullanım için özel bir türdür. Bir dosya katmanlı tamamen veya kısmen geri olmadığından azure dosya eşitleme seyrek dosyaları kullanır. Tam olarak katmanlı bir dosyada dosya akışı bulutta depolanır. Bir geri çekilen kısmen dosyasında, bu dosya zaten disk üzerinde parçasıdır. Tam olarak bir dosya ise, diske geri, Azure dosya eşitleme, bir seyrek dosyasından normal bir dosyaya dönüştürür. |
        | L | Yeniden ayrıştırma noktası | Dosyayı yeniden ayrıştırma noktası olduğunu gösterir. Bir dosya sistemi Filtresi tarafından kullanılmak üzere özel bir işaretçi bir ayrıştırma noktasıdır. Azure dosya eşitleme yeniden ayrıştırma noktalarını Azure dosya eşitleme dosya sistemi filtresi (StorageSync.sys) dosyanın depolandığı konumun bulut tanımlamak için kullanır. Bu, sorunsuz erişim destekler. Kullanıcılar Azure dosya eşitleme kullanılmakta olduğunu bilmek ihtiyacınız olmaz veya dosyanın Azure dosya paylaşımınızdaki erişim elde etmek. Bir dosyanın tam olarak çağrılır, Azure dosya eşitleme yeniden ayrıştırma noktası dosyasından kaldırır. |
        | O | Çevrimdışı | Dosyanın içeriğini hepsinde veya diske depolanmış değil gösterir. Azure dosya eşitleme, bir dosyanın tam olarak çağrılır, bu öznitelik kaldırır. |

        ![Ayrıntılar sekmesi seçili bir dosya için özellikleri iletişim kutusu](media/storage-files-faq/azure-file-sync-file-attributes.png)
        
        Bir klasördeki tüm dosyaları için öznitelikleri ekleyerek gördüğünüz **öznitelikleri** dosya Gezgini'nde tablo görüntülenmesini alanı. Bunu yapmak için üzerinde var olan bir sütuna sağ tıklayın (örneğin, **boyutu**) seçeneğini **daha fazla**ve ardından **öznitelikleri** aşağı açılan listeden.
        
   * **Kullanım `fsutil` bir dosyayı yeniden ayrıştırma noktalarında denetlemek için.**
       Önceki seçeneği açıklandığı gibi bir katmanlı dosya her zaman bir yeniden ayrıştırma noktası kümesi vardır. Azure dosya eşitleme dosya sistemi filtresi (StorageSync.sys) için özel bir işaretçi bir yeniden ayrıştırma işaretçisidir. Bir dosya, yükseltilmiş bir komut istemi veya PowerShell penceresinde bir yeniden ayrıştırma noktası olup olmadığını denetlemek için çalıştırın `fsutil` yardımcı programı:
    
        ```powershell
        fsutil reparsepoint query <your-file-name>
        ```

        Dosyayı yeniden ayrıştırma noktası varsa, neleri bekleyebileceğiniz **yeniden ayrıştırma etiket değeri: 0x8000001e**. Bu onaltılık değer Azure dosya eşitleme tarafından sahip olunan yeniden ayrıştırma noktası değerdir. Çıktı, Azure dosya paylaşımınızı onedrive'daki dosyanızda yolunu temsil ettiği yeniden ayrıştırma verilerini de içerir.

        > [!WARNING]  
        > `fsutil reparsepoint` Yardımcı programı komut ayrıca bir yeniden ayrıştırma noktası silme özelliğine sahiptir. Azure dosya eşitleme mühendislik ekibi isteyen sürece bu komutu yürütmek değil. Bu komutu çalıştırmak, veri kaybına neden. 

<a id="afs-recall-file"></a>

### <a name="a-file-i-want-to-use-has-been-tiered-how-can-i-recall-the-file-to-disk-to-use-it-locally"></a>Kullanmak istediğiniz bir dosya katmanlı. Yerel olarak kullanabilmek için diskten dosyasına nasıl geri çağırma?
Bir dosyayı diske geri çekmek için en kolay yolu, dosya açmaktır. Azure dosya eşitleme dosya sistemi filtresi (StorageSync.sys) dosyanın Azure dosya paylaşımınızı sizin herhangi bir çalışma yapmadan sorunsuz bir şekilde indirir. Kısmen olabilir dosya türleri için okuma, bir dosyayı açmayı multimedya veya .zip dosyası gibi tüm dosya indirilmedi.

Bir dosya çağrılmaya zorlamak için PowerShell de kullanabilirsiniz. Bu seçenek, bir klasördeki tüm dosyaları gibi birden çok dosyayı aynı anda geri çekmek istiyorsanız yararlı olabilir. Azure dosya eşitleme'nın yüklendiği sunucu düğümü için bir PowerShell oturumu açın ve sonra aşağıdaki PowerShell komutlarını çalıştırın:
    
    ```powershell
    Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
    Invoke-StorageSyncFileRecall -Path <file-or-directory-to-be-recalled>
    ```

<a id="sizeondisk-versus-size"></a>
### <a name="why-doesnt-the-size-on-disk-property-for-a-file-match-the-size-property-after-using-azure-file-sync"></a>Neden olmayan *disk boyutu* özelliği için bir dosya eşleşmesi *boyutu* Azure dosya eşitleme kullandıktan sonra özellik? 
Windows dosya Gezgini'nde, bir dosyanın boyutunu göstermek için iki özellik sunar: **Boyutu** ve **disk boyutu**. Bu özellikler, farenizin anlamları farklılık gösterir. **Boyutu** dosyasının tam boyutunu temsil eder. **Disk boyutu** diskte depolanan dosya akışı boyutunu temsil eder. Bu özelliklerin değerleri için çeşitli nedenlerle, sıkıştırma gibi farklı, yinelenen verileri kaldırma kullanın veya Bulut katmanlaması Azure dosya eşitleme ile. Bir dosya, bir Azure dosya paylaşımına katmanlı, Azure dosya paylaşımınızı ve diskteki dosya akışı depolandığı için disk boyutu sıfır olur. Bir dosya kısmen katmanlı (veya kısmen yükümlülüğümüz) olmasını da mümkündür. Kısmen katmanlı dosyasında, dosyanın disk üzerinde parçasıdır. Dosyaları kısmen multimedya oynatıcıları gibi uygulamalar tarafından okunur veya yardımcı programlarını zip olduğunda bu durum oluşabilir. 

<a id="afs-force-tiering"></a>
### <a name="how-do-i-force-a-file-or-directory-to-be-tiered"></a>Bir dosya veya dizin katmanlanmış nasıl zorlarım?
Bulut katmanlama özelliği etkinleştirilmişse, bu bulut katmanları dosyaları otomatik olarak katmanlama son erişimini ve bulut uç noktada belirtilen birim boş alanı yüzde elde etmek için bir kez değiştirin. Bazı durumlarda, yine de el ile bir dosya katmanı zorlamak isteyebilirsiniz. Bu uzun bir süredir yeniden kullanmayı düşünmüyorsanız büyük bir dosya kaydetmeniz halinde kullanışlı olabilir ve diğer dosyalar ve klasörler için kullanılacak toplu artık boş alan istediğiniz. Aşağıdaki PowerShell komutlarını kullanarak katmanlama zorlayabilirsiniz:

    ```powershell
    Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
    Invoke-StorageSyncCloudTiering -Path <file-or-directory-to-be-tiered>
    ```

## <a name="next-steps"></a>Sonraki Adımlar
* [Bir Azure dosya eşitleme dağıtımı planlama](storage-sync-files-planning.md)
