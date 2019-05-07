---
title: Okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) kullanarak yüksek oranda kullanılabilir Aaplications tasarlama | Microsoft Docs
description: Nasıl kesintilerden işlemek için esnek yüksek oranda kullanılabilir bir uygulama oluşturmak için Azure RA-GRS depolama kullanın.
services: storage
author: tamram
ms.service: storage
ms.devlang: dotnet
ms.topic: article
ms.date: 01/17/2019
ms.author: tamram
ms.reviewer: artek
ms.subservice: common
ms.openlocfilehash: 6dc497ac2afd54965485ff553bb25f47d7cf0491
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65139338"
---
# <a name="designing-highly-available-applications-using-ra-grs"></a>RA-GRS'yi kullanarak yüksek kullanılabilirliğe sahip uygulamalar tasarlama

Azure depolama gibi bulut tabanlı altyapılarının ortak bir özellik uygulamalarını barındırmak için yüksek oranda kullanılabilir platformu sağlayan ' dir. Bulut tabanlı uygulamaların geliştiricileri, kullanıcılara yüksek oranda kullanılabilir uygulamalar sunmak için bu bir platformdan yararlanın nasıl dikkatli bir şekilde dikkate almanız gerekir. Bu makalede nasıl geliştiriciler okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) sağlamak için Azure depolama uygulamalarını yüksek oranda kullanılabilir olan kullanabilirsiniz üzerinde odaklanır.

[!INCLUDE [storage-common-redundancy-options](../../../includes/storage-common-redundancy-options.md)]

Bu makalede, GRS ve RA-GRS odaklanır. GRS ile verilerinizin üç kopyasını birincil bölgelerindeki depolama hesabı ayarlarken seçtiğiniz tutulur. Üç ek kopya, ikincil bölgede Azure tarafından belirtilen zaman uyumsuz olarak saklanır. RA-GRS ikincil kopyaya okuma erişimli coğrafi olarak yedekli depolama sunar.

Hangi birincil bölgede hangi ikincil bölgeler ile halindedir bilgi için bkz: [iş sürekliliği ve olağanüstü durum kurtarma (BCDR): Azure eşleştirilmiş bölgeleri](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

Bu makalede, indirme ve çalıştırma sonunda eksiksiz bir örnek için bir bağlantı bulunan kod parçacıkları vardır.

> [!NOTE]
> Azure Storage artık yüksek oranda kullanılabilir uygulamalar oluşturmak için bölgesel olarak yedekli depolama (ZRS) destekler. ZRS, yedeklilik ihtiyaçlarını birçok uygulama için basit bir çözüm sunar. ZRS, donanım arızalarında ya da tek bir veri merkezi etkileyen yıkıcı felaketlere karşı koruma sağlar. Daha fazla bilgi için [bölgesel olarak yedekli depolama (ZRS): Azure depolama yüksek kullanılabilirliğe sahip uygulamalar](storage-redundancy-zrs.md).

## <a name="key-features-of-ra-grs"></a>RA-GRS temel özellikleri

RA-GRS için uygulamanızı tasarlarken aşağıdaki önemli noktalara dikkat edin:

* Azure depolama, ikincil bölgede, birincil bölgedeki depoladığınız veri salt okunur bir kopyasını tutar. Yukarıda belirtildiği gibi depolama hizmeti ikincil bölgeye konumunu belirler.

* Salt okunur bir kopyasıdır [sonunda tutarlı](https://en.wikipedia.org/wiki/Eventual_consistency) ile birincil bölgedeki veriler.

* Bloblar, tablolar ve Kuyruklar için ikincil bölgeye Sorgulayabileceğiniz bir *son eşitleme zamanı* son birincil çoğaltmadan ikincil bölgeye gerçekleştiği belirten değer. (Bu RA-GRS yedekleme şu anda yok. Azure dosyaları için desteklenmiyor.)

* Birincil veya ikincil bölgede verilerle etkileşim kurmak için depolama istemcisi Kitaplığı'nı kullanabilirsiniz. Ayrıca yönlendirebilirsiniz istekleri ikincil bölgeye otomatik olarak birincil bölgeye Okuma isteği zaman aşımına uğrarsa okuyun.

* Birincil bölge kullanılamaz duruma gelirse bir hesabı yük devretme başlatabilirsiniz. İkincil bölgeye yük devretme, birincil bölgeye işaret eden DNS girişlerini ikincil bölgeye işaret edecek şekilde değiştirilir. Yazma erişimi, GRS ve RA-GRS hesapları için yük devretme işlemi tamamlandıktan sonra geri yüklenir. Daha fazla bilgi için [olağanüstü durum kurtarma ve depolama hesabı yük devretme (Önizleme) Azure Depolama'daki](storage-disaster-recovery-guidance.md).

## <a name="application-design-considerations-when-using-ra-grs"></a>RA-GRS kullanırken uygulama tasarımı noktaları hakkında

Bu makalenin amacı, (içinde sınırlı bir kapasite barındırabilir) birincil veri merkezinde önemli bir olağanüstü durum olması durumunda bile çalışmaya devam edeceği bir uygulama tasarlamak nasıl size göstermektir. Birincil bölgeden okuma ile uğratan bir sorun olduğunda ikincil bölgeden okuma tarafından geçici veya uzun ömürlü sorunlarını işlemek için uygulamanızı tasarlayabilirsiniz. Birincil bölge tekrar kullanılabilir duruma geldiğinde, uygulamanızı birincil bölgeden okuma için geri dönebilirsiniz.

### <a name="using-eventually-consistent-data"></a>Sonunda tutarlı veri kullanma

Önerilen çözüm, çağıran uygulama için büyük olasılıkla eski veri döndürmek için kabul edilebilir olduğunu varsayar. İkincil bölgedeki veri sonunda tutarlı olduğundan, bir güncelleştirme ikincil bölgeye çoğaltma bitmeden önce birincil bölgeye erişilemez duruma gelebilir mümkündür.

Örneğin, müşteri bir güncelleştirme başarıyla gönderir, ancak birincil bölgeden ikincil bölgeye güncelleştirme yayılmadan önce başarısız olduğunu varsayalım. Müşteri geri verileri okumak sorduğunda, güncelleştirilmiş veriler yerine ikincil bölgeden yaptığı eski veri alır. Müşteri ileti nasıl uygulamanızı tasarlarken, bu kabul edilebilir olup olmadığını ve bu durumda, karar vermeniz gerekir. 

Bu makalenin sonraki bölümlerinde son eşitleme ikincil güncel olup olmadığını denetlemek için veriler için zaman ikincil nasıl kontrol edileceğini göstereceğiz.

### <a name="handling-services-separately-or-all-together"></a>Hizmetler ayrı olarak veya tümünü bir araya işleme

Nadiren de olsa bir hizmet diğer hizmetleri yine de tam işlevlidir sırasında kullanılamaz hale gelmesine mümkündür. İşleyebilirsiniz yeniden deneme sayısı ve her biri için salt okunur modda hizmet ayrı olarak (BLOB'lar, kuyruklar, tablolar) ya da depolama hizmetleri için genel yeniden deneme birlikte işleyebilir.

Örneğin, uygulamanızda kuyrukları ve blobları kullanırsanız, bunların her biri için yeniden denenebilir hataları işlemek için ayrı bir kod koymak karar verebilirsiniz. Ardından blob hizmetinden bir yeniden deneme alabilirsiniz, ama kuyruk hizmeti çalışmaya devam ediyoruz, yalnızca BLOB'lar işleyen uygulamanızın parçası etkilenir. Tüm depolama hizmeti yeniden deneme genel işlemeye karar verin ve blob hizmetine bir çağrı yeniden denenebilir bir hata döndürür, blob ve kuyruk hizmeti hem istekleri etkilenir.

Sonuç olarak, bu, uygulamanızın karmaşıklığına bağlıdır. Hizmet tarafından hataları işlememesi karar verebilirsiniz, ancak bunun yerine yeniden yönlendirmek için okuma istekleri ikincil bölgeye tüm depolama hizmetleri için birincil bölgede bir depolama hizmetindeki bir sorun birisini algıladığında uygulamayı ve salt okunur modda çalıştırmak.

### <a name="other-considerations"></a>Dikkat edilecek diğer noktalar

Bu makalenin geri kalanında ele alınacaktır diğer değerlendirmeler şunlardır.

*   Yeniden deneme devre kesici düzeni kullanarak okuma isteklerinin işlenmesi

*   Sonunda tutarlı veri ve son eşitleme zamanı

*   Test Etme

## <a name="running-your-application-in-read-only-mode"></a>Uygulamanız salt okunur modda çalışan

RA-GRS depolama kullanmak için başarısız olan her iki okuma istekleri işleyebilmesi gerekir ve (Bu durumda ekler anlamı güncelleştirme, güncelleştirmeleri ve silme işlemleri) güncelleştirme isteği başarısız oldu. Başarısız olursa birincil veri merkezi, istekleri ikincil veri merkezine yönlendirilebilir okuyun. Ancak, ikincil salt okunur olduğu için güncelleştirme istekleri için ikincil yönlendirilemez. Bu nedenle, uygulamanız salt okunur modunda çalışacak şekilde tasarlamanız gerekir.

Örneğin, herhangi bir güncelleştirme istekleri için Azure depolama gönderilmeden önce denetlenir bayrak ayarlayabilirsiniz. Bir güncelleştirme istekleri söz konusu olduğunda, bu adımı atlarsanız ve müşteri için uygun bir yanıt döndürür. Hatta bazı özellikleri devre dışı bırakmak isteyebilirsiniz sorun çözümlenir ve bu özellikler geçici olarak devre dışı kullanıcı bildirimi sayısı kadar toptan.

Her hizmet için hataları ayrı ayrı işlemek karar verirseniz, uygulamanızın hizmet tarafından salt okunur modda çalıştırma olanağı işlemek gerekir. Örneğin, etkin ve devre dışı her hizmet için salt okunur bayrakları olabilir. Daha sonra kodunuzda uygun yerlerde bayrağı işleyebilir.

Uygulamanız salt okunur modda çalıştırmak için başka bir yan avantajı vardır: ana uygulama yükseltme sırasında sınırlı işlevsellik sağlamak için özelliği sağlar. Yükseltme yaparken veriler birincil bölgede hiç kimse erişiyor sağlayarak uygulamanızın salt okunur modda çalıştırın ve ikincil veri merkezine işaret tetikleyebilirsiniz.

## <a name="handling-updates-when-running-in-read-only-mode"></a>Salt okunur modda çalışırken güncelleştirmeleri işleme

Salt okunur modda çalışırken güncelleştirme isteklerini işlemek için birçok yolu vardır. Bu kapsamlı ele olmaz, ancak genellikle birkaç olduğunu düşündüğünüz desenlerinin vardır.

1.  Kullanıcıya yanıt vermek ve bunları, şu anda güncelleştirmeleri kabul etmiyoruz söyleyin. Örneğin, bir kişi yönetim sistemi, kişi bilgilerine erişip ancak güncelleştirmeleri olmamasını müşterilere sağlayabilir.

2.  Sıraya alma, güncelleştirmeleri başka bir bölgede kullanabilirsiniz. Bu durumda, farklı bir bölgeye kuyrukta bekleyen güncelleştirme isteklerinizden yazmak ve sonra birincil veri merkezinde yeniden çevrimiçi olduktan sonra bu istekleri işlemek için bir yönteme sahip. Bu senaryoda, istenen güncelleştirmeyi daha sonra işlenmek üzere kuyruğa bilmeniz müşteri izin vermelisiniz.

3.  Başka bir bölgedeki bir depolama hesabına güncelleştirmelerinizi yazabilirsiniz. Ardından birincil veri merkezi tekrar çevrimiçi olduğunda, bu güncelleştirmeleri veri yapısına bağlı olarak, birincil veri birleştirmek için bir yol olabilir. Örneğin, adında bir tarih/saat damgasının sahip ayrı dosyalar oluşturuyorsanız, bu dosyaları birincil bölgeye geri kopyalayabilirsiniz. Bu, verileri günlüğe kaydetme ve IOT gibi bazı iş yükleri için çalışır.

## <a name="handling-retries"></a>Yeniden deneme işleme

Bunu nasıl hangi hataları yeniden denenebilir olduğunu biliyor? Bu işlem, depolama istemcisi kitaplığı tarafından belirlenir. Örneğin, bu yeniden deneme başarılı şekilde neden olabilir olmadığından bir 404 hatası (kaynak bulunamadı) yeniden denenebilir değil. Öte yandan, bir 500 sunucu hatası olduğundan ve yalnızca geçici bir sorun olabilir çünkü yeniden denenebilir hatasıdır. Daha fazla ayrıntı için kullanıma [açık ExponentialRetry sınıfı için kaynak kodu](https://github.com/Azure/azure-storage-net/blob/87b84b3d5ee884c7adc10e494e2c7060956515d0/Lib/Common/RetryPolicies/ExponentialRetry.cs) .NET depolama istemci Kitaplığı'nda. (ShouldRetry yöntemi arayın.)

### <a name="read-requests"></a>Okuma istekleri

Birincil depolama ile ilgili bir sorun varsa ikincil depolamaya okuma istekleri yönlendirilebilir. İçinde yukarıda olarak belirtildiği [kullanarak sonunda tutarlı veri](#using-eventually-consistent-data), büyük olasılıkla eski verileri okumak, uygulamanız için kabul edilebilir olmalıdır. RA-GRS verilere erişmek için depolama istemci kitaplığı kullanıyorsanız, yeniden deneme davranışı Okuma isteği için bir değer ayarlayarak belirtebilirsiniz **LocationMode** özelliğini aşağıdakilerden biri:

*   **PrimaryOnly** (varsayılan)

*   **PrimaryThenSecondary**

*   **SecondaryOnly**

*   **SecondaryThenPrimary**

Ayarladığınızda **LocationMode** için **PrimaryThenSecondary**, istemci yeniden denenebilir bir hata ile birincil uç noktayı başarısız ilk Okuma isteği otomatik olarak başka bir okuma isteğinde bulunur İkincil uç nokta. Bir sunucu zaman aşımı hatası ise İstemci hizmetini yeniden denenebilir bir hata almadan önce beklenecek zaman aşımını beklemesi gerekecektir.

Temel olarak ne zaman nasıl yeniden denenebilir bir hata için yanıt verirken dikkate almanız iki senaryo vardır:

*   Bu yalıtılmış bir sorundur ve sonraki istekleri birincil uç noktasına yeniden denenebilir bir hata döndürmez. Geçici ağ hatası olduğunda burada gerçekleşebilir bir örnektir.

    Bu senaryoda yoktur önemli performans cezası etmeyle **LocationMode** kümesine **PrimaryThenSecondary** gibi yalnızca nadiren gerçekleşir.

*   Bu en az bir birincil bölgelerindeki depolama hizmetleri ile ilgili bir sorun ve sonraki isteklere hizmet birincil bölgede bir süreliğine yeniden denenebilir bir hata döndürüyor olma olasılıkları. Buna örnek olarak, birincil bölgenin tamamen erişilemez ' dir.

    Bu senaryoda var. bir performans cezası çünkü tüm okuma isteklerinin birincil uç noktadan önce deneyin, zaman aşımı sona bekleyin ve ardından ikincil uç noktaya geçiş

Bu senaryolar için olmadığını birincil uç nokta ile devam eden bir sorundur ve tüm okuma doğrudan ikincil uç noktaya yönelik istekler ayarlayarak gönderme tanımlamalıdır **LocationMode** özelliğini **SecondaryOnly** . Şu anda salt okunur modda çalıştırmak için uygulamayı değiştirmeniz gerekir. Bu yaklaşım olarak da bilinen [devre kesici düzeni](/azure/architecture/patterns/circuit-breaker).

### <a name="update-requests"></a>Güncelleştirme istekleri

Devre kesici düzeni, güncelleştirme istekleri için de uygulanabilir. Ancak, güncelleştirme istekleri salt okunur ikincil depolama yönlendirilemiyor. Bu istekler için bırakmanız **LocationMode** özelliğini **PrimaryOnly** (varsayılan). Bu hataları işlemek için bu istekleri – bir satırda – 10 hataları gibi bir ölçüm uygulamak ve, eşiğine ulaşıldığında, salt okunur modda uygulamasına geçiş yapabilirsiniz. Devre kesici düzeni hakkında sonraki bölümde, aşağıda açıklanan gideriyor modu güncelleştirmek için döndürmek için aynı yöntemi kullanabilirsiniz.

## <a name="circuit-breaker-pattern"></a>Devre Kesici düzeni

Devre kesici düzeni kullanarak uygulamanızda art arda başarısız olma olasılığı yüksek bir işlemin yeniden denenmesi öğesinden engelleyebilirsiniz. Uygulama çalışmaya devam etmesini sağlar yerine işlem zaman ayırdığınız katlanarak denenir. Hata düzeltildi, aynı zamanda uygulama işlemi yeniden deneyebilirsiniz de algılar.

### <a name="how-to-implement-the-circuit-breaker-pattern"></a>Devre kesici düzeni nasıl uygulayacağınıza karar

Bir birincil uç noktası ile devam eden bir sorun olduğunu belirlemek için ne sıklıkta istemci yeniden denenebilir hatayla karşılaştığında izleyebilirsiniz. Her durumda farklı olduğundan, karar için ikincil uç noktaya geçmek ve salt okunur modda uygulamayı çalıştırmak için kullanmak istediğiniz eşik üzerinde karar vermelisiniz. Örneğin, bir satırda hiçbir başarıları ile 10 hata varsa, geçişi gerçekleştirmek karar. %90 2 dakikalık dönemdeki istek başarısız olursa geçiş başka bir örnektir.

İlk senaryo için basitçe hataların sayısını tutabilir ve kullanabilirsiniz varsa bir başarı en ulaşmadan önce sayısı sıfır olarak ayarlayın. İkinci senaryo için uygulamaya yollarından biri MemoryCache nesne (.NET) kullanmaktır. Her istek için bir CacheItem önbelleğe (1) başarılı durumuna ayarlayın veya başarısız (0) ekleyip sona erme zamanı şimdi (veya, bir zaman kısıtlaması ne olursa olsun) 2 dakika olarak ayarlayın. Bir girişin zaman aşımı süresine ulaşıldığında, giriş otomatik olarak kaldırılır. Bu bir sıralı 2 dakikalık penceresi sağlar. Bir istek depolama hizmetine yapılan her değişiklikte, öncelikle bir LINQ Sorgu MemoryCache nesne arasında başarı yüzdesi değerlerin toplamını ve bazında sayı bölen tarafından hesaplamak için kullanın. Başarı yüzdesi (örneğin, % 10) bazı eşiğin altına düştüğünde ayarlamak **LocationMode** özelliği okuma istekleri için **SecondaryOnly** ve devam etmeden önce uygulama salt okunur moduna geçiş yapın.

Yapılandırılabilir parametreler yönetilmelerini dikkate almanız gereken şekilde ne zaman geçiş yapmak belirlemek için kullanılan hata eşiği uygulamanızda, hizmetten hizmete farklılık gösterebilir. Bu ayrıca her hizmet yeniden denenebilir hatayla ayrı ayrı işlemek burada karar verin, veya bir, daha önce açıklandığı gibi.

Bir uygulama birden çok örneğini nasıl ele alınacağını ve her bir örneğinde yeniden denenebilir hata algılama ne yapılacağını bunun başka bir husustur. Örneğin, yüklenen uygulamasıyla çalışan 20 VM olabilir. Her örnek ayrı ayrı işlemek? Bir örneği bir sorun olduğunda tüm örnekleri aynı şekilde yanıt, sorunları tek bir örneğini başlatır, yalnızca bu tek örneği yanıta sınırlamak istiyorsunuz veya olmasını denemek istiyor musunuz? Ayrı ayrı örnekleri işleme yanıt arasında koordine etmek çalışmaktan daha çok basittir, ancak bunu nasıl yapacağınız, uygulamanızın mimarisine bağlıdır.

### <a name="options-for-monitoring-the-error-frequency"></a>Hata sıklığı izleme seçenekleri

Birincil bölgede yeniden denemeler sıklığını ikincil bölge'ye geçiş yapın ve uygulamayı salt okunur modunda çalışacak şekilde değiştirmek ne zaman belirlemek için izleme için üç ana seçeneğiniz vardır.

*   İçin bir işleyici eklemek [ **yeniden deneniyor** ](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.operationcontext.retrying.aspx) olayda [ **OperationContext** ](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.operationcontext.aspx) depolama geçirdiğiniz nesne istekleri – bu yöntem, Bu makalede gösterilen ve eşlik eden örnek kullanılır. Bu olayları, istemcinin ne sıklıkta yeniden denenebilir hata birincil uç noktasında istemci karşılaştığında izlemenize olanak sağlayan bir isteği yeniden deneme zaman kov.

    ```csharp 
    operationContext.Retrying += (sender, arguments) =>
    {
        // Retrying in the primary region
        if (arguments.Request.Host == primaryhostname)
            ...
    };
    ```

*   İçinde [ **değerlendir** ](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.retrypolicies.iextendedretrypolicy.evaluate.aspx) yöntemi özel bir yeniden deneme ilkesinde çalıştırabilirsiniz özel kod her bir yeniden deneme gerçekleşir. Bir yeniden deneme zaman kaydı yanı sıra olur, bu Ayrıca, yeniden deneme davranışı değiştirme olanağı sağlar.

    ```csharp 
    public RetryInfo Evaluate(RetryContext retryContext,
    OperationContext operationContext)
    {
        var statusCode = retryContext.LastRequestResult.HttpStatusCode;
        if (retryContext.CurrentRetryCount >= this.maximumAttempts
            || ((statusCode >= 300 && statusCode < 500 && statusCode != 408)
            || statusCode == 501 // Not Implemented
            || statusCode == 505 // Version Not Supported
            ))
        {
            // Do not retry
            return null;
        }

        // Monitor retries in the primary location
        ...

        // Determine RetryInterval and TargetLocation
        RetryInfo info =
            CreateRetryInfo(retryContext.CurrentRetryCount);

        return info;
    }
    ```

*   Birincil depolama uç noktanız dummy okuma istekleri (örneğin, küçük bir blob okuma) ile sürekli olarak ping atar, uygulamanızda özel bir izleme bileşeni uygulamak için üçüncü bir yaklaşım olan sistem durumu belirlemek için. Bu, bazı kaynaklar ancak önemli ölçüde yararlanın. Ardından eşiğine ulaştığında bir sorun algılandığında anahtara gerçekleştirecek **SecondaryOnly** ve salt okunur modda.

Belirli bir noktada geri birincil uç noktayı kullanarak ve güncelleştirmelere izin için geçiş yapmak isteyeceksiniz. Rasgele seçilen bir zaman miktarı veya işlemlerin sayısı gerçekleştirildikten sonra yukarıda listelenen ilk iki yöntemden birini kullanarak varsa yalnızca birincil uç noktayı ve güncelleştirme modunu etkinleştirmek için geçiş. Ardından, yeniden deneme mantığı yeniden kullanıma izin verebilirsiniz. Sorun düzeltildi, birincil uç noktayı kullanın ve güncelleştirmelere izin vermek devam eder. Yine de bir sorun varsa, bunu bir kez daha ikincil uç nokta ve salt okunur mod için belirlediğiniz ölçütlere başarısız geçiş yapar.

Üçüncü senaryo için birincil depolama uç nokta ping yeniden başarılı olduğunda anahtar tetikleyebilirsiniz geri **PrimaryOnly** ve güncelleştirmelere izin devam edin.

## <a name="handling-eventually-consistent-data"></a>Sonunda tutarlı bir veri işleme

RA-GRS, birincil bölgedeki işlemleri ikincil bölgede çoğaltarak çalışır. Bu çoğaltma işlemi, ikincil bölgedeki verilerin olduğunu garanti *sonunda tutarlı*. Bu ikincil bölgede, birincil bölgedeki tüm işlemlerin sonunda görünür ancak bunlar görünmeden önce bir gecikme olabilir ve işlemleri geldiğinde, burada aynı sırada ikincil bölgedeki bir garanti yoktur anlamına gelir, ilk olarak birincil bölgeye uygulandı. İşlemlerinizi sırası, ikincil bölgede ulaşırsa, *olabilir* hizmet arayı kapatıncaya kadar tutarsız bir durumda olmasını ikincil bölgede göz önünde bulundurun.

Aşağıdaki tabloda her bir üyesi yapmak için bir çalışan ayrıntılarını güncelleştirdiğinizde ne olacağını örneği gösterilmektedir *Yöneticiler* rol. Bu örnek için bu güncelleştirme gerektirir. **çalışan** varlık ve güncelleştirme bir **Yönetici rolü** Yöneticiler toplam sayısı sayısı olan varlık. Güncelleştirmeleri sıralamaya ikincil bölgede uygulanma dikkat edin.

| **saat** | **İşlem**                                            | **Çoğaltma**                       | **Son eşitleme zamanı** | **Sonuç** |
|----------|------------------------------------------------------------|---------------------------------------|--------------------|------------| 
| T0       | İşlem y: <br> Çalışan Ekle <br> birincil varlık |                                   |                    | Birincil bölgeye eklenen bir işlem<br> henüz çoğaltılmamış. |
| T1       |                                                            | Bir işlem <br> çoğaltılır<br> ikincil | T1 | İşlem bir ikincil siteden çoğaltılan. <br>Son eşitleme zamanı güncelleştirildi.    |
| T2       | İşlem B:<br>Güncelleştirme<br> Çalışan varlık<br> birincil  |                                | T1                 | Birincil veritabanına yazılan B işlem<br> henüz çoğaltılmamış.  |
| T3       | İşlem C:<br> Güncelleştirme <br>yönetici<br>Rol varlık<br>birincil |                    | T1                 | C birincil veritabanına yazılan işlem<br> henüz çoğaltılmamış.  |
| *T4*     |                                                       | İşlem C <br>çoğaltılır<br> ikincil | T1         | İşlem için ikincil çoğaltılan C.<br>LastSyncTime olduğundan güncelleştirilmedi <br>işlem B henüz çoğaltılmamış.|
| *T5*     | Varlıkları okuma <br>İkincil bölgeden                           |                                  | T1                 | Çalışana ait eski değeri Al <br> Varlık işlem B taşınmadığından çünkü <br> henüz çoğaltılır. Yeni değeri Al<br> yönetici rol varlığını C olduğundan<br> Çoğaltılmış. Son eşitleme zamanı hala henüz<br> Silinmiş olduğundan güncelleştirilmiş işlem B<br> Çoğaltılmış edilmemiş. Söyleyin<br>yönetici rol varlığını tutarsız. <br>Varlık tarih/saat sonra olduğu için <br>Son eşitleme zamanı. |
| *T6*     |                                                      | İşlem B<br> çoğaltılır<br> ikincil | T6                 | *T6* – C aracılığıyla tüm işlemlerin <br>edilmiş çoğaltılan, son eşitleme zamanı<br> güncelleştirilir. |

Bu örnekte, istemci T5 konumunda ikincil bölgeden okuma için anahtarları varsayılır. Başarılı bir şekilde okuyabilir **Yönetici rolü** varlık şu anda, ancak varlık sayısı ile tutarlı değil yönetici sayısı için bir değer içeren **çalışan** olan varlıklar Yöneticiler, ikincil bölgede şu anda işaretli. İstemci, yalnızca bu değerle tutarsız bilgiler olduğunu risk görüntüleyebilirsiniz. Alternatif olarak, istemci belirleyen yararlanmaya **Yönetici rolü** güncelleştirmeleri sıralama dışında gerçekleşen ve ardından bu olayının kullanıcıyı bilgilendirmeniz büyük olasılıkla tutarsız bir durumda olmasıdır.

Büyük olasılıkla tutarsız veri, değerinin istemcinin kullanabileceği tanımak için *son eşitleme zamanı* herhangi bir zamanda depolama hizmeti sorgulayarak alabilirsiniz. Bu, verileri ikincil bölgedeki son zaman bildirir tutarlı ve ne zaman hizmeti uygulanan o noktadan önce tüm işlemlerin zaman. Hizmet ekledikten sonra yukarıda gösterilen örnekte **çalışan** ikincil bölgede, son eşitleme zamanı varlık kümesine *T1*. Kalır *T1* hizmet güncelleştirmeleri kadar **çalışan** varlık ayarlandığında ikincil bölgedeki *T6*. İstemcinin okuduğu karşı tarafındaki varlığın ne zaman son eşitleme zamanı alır, *T5*, bu varlık üzerinde zaman damgası ile karşılaştırabilirsiniz. Varlık üzerinde zaman damgası, son eşitleme zamanından daha sonra ise, varlık büyük olasılıkla tutarsız bir durumda ise ve uygulamanız için uygun eylemi ne olursa olsun alabilir. Bu alanı kullanarak birincil son güncelleştirmeye tamamlandığı bilmeniz gerekir.

## <a name="testing"></a>Test Etme

Uygulamanızı yeniden denenebilir bir hata ile karşılaştığında beklediğiniz gibi davrandığını test etmek önemlidir. Örneğin, birincil bölge tekrar kullanılabilir hale geldiğinde uygulama anahtarları ikincil ve bir sorun algılar ve anahtarları salt okunur moduna geri test etmek gerekir. Bunu yapmak için yeniden denenebilir hata ve ne sıklıkta ortaya denetim benzetimini yapmak için bir yol gerekir.

Kullanabileceğiniz [Fiddler](https://www.telerik.com/fiddler) kesecek ve HTTP yanıtlarını betikteki değiştirin. Bu betik, birincil uç noktadan gelen yanıtları tanımlayabilir ve depolama istemci kitaplığı yeniden denenebilir bir hata olarak tanıdığı bir HTTP durum kodu değiştirin. Bu kod parçacığı istekler okumak için yanıtları başvurarak Fiddler komut dosyasının basit bir örnek göstermektedir **employeedata** 502 durumuna döndürmek için Tablo:

```java
static function OnBeforeResponse(oSession: Session) {
    ...
    if ((oSession.hostname == "\[yourstorageaccount\].table.core.windows.net")
      && (oSession.PathAndQuery.StartsWith("/employeedata?$filter"))) {
        oSession.responseCode = 502;
    }
}
```

Bu örnek, geniş bir istek kesecek ve sadece değişiklik genişletilebiliyordu **yanıt kodu** bazı bunları gerçek dünya senaryoları daha iyi benzetmek için. Fiddler betikleri özelleştirme hakkında daha fazla bilgi için bkz. [istek veya yanıtı değiştirerek](https://docs.telerik.com/fiddler/KnowledgeBase/FiddlerScript/ModifyRequestOrResponse) Fiddler belgelerinde.

Salt okunur moda yapılandırılabilir uygulamanızı geçişi için eşikler yaptıysanız, üretim dışı işlem birimleri ile davranışı test etmek daha kolay olacaktır.

## <a name="next-steps"></a>Sonraki Adımlar

* LastSyncTime nasıl belirlendiğini, başka bir örnek de dahil olmak üzere ilgili okuma erişimli coğrafi olarak Yedeklilik daha fazla bilgi için lütfen bkz [Windows Azure depolama Yedekliliği seçenekleri ve okuma erişimli coğrafi olarak yedekli depolama](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/).

* Birincil ve ikincil uç noktaları arasında sürekli geçiş yapmak nasıl gösteren tam bir örnek için bkz. Lütfen [Azure devre kesici düzeni RA-GRS depolama ile kullanma örnekleri –](https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs).
