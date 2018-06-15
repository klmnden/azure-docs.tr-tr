---
title: Yüksek oranda kullanılabilir Azure okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) kullanan uygulamalar tasarlama | Microsoft Docs
description: Azure RA-GRS depolama bir yüksek oranda kullanılabilir uygulama kesintiler işlemek için esnek mimari için nasıl kullanılacağını.
services: storage
documentationcenter: .net
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 8f040b0f-8926-4831-ac07-79f646f31926
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/21/2018
ms.author: tamram
ms.openlocfilehash: f7f3f2d99e5582a1bcb672cc176258dfff9c3217
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
ms.locfileid: "30322939"
---
# <a name="designing-highly-available-applications-using-ra-grs"></a>Yüksek oranda kullanılabilir RA-GRS kullanarak uygulamalar tasarlama

Ortak bir Azure depolama gibi bulut tabanlı altyapılarının uygulamalarını barındırmak için yüksek oranda kullanılabilir bir platform sağlar özelliktir. Bulut tabanlı uygulaması geliştiricileri, kullanıcılar için yüksek oranda kullanılabilir uygulamalarını sunmak üzere bu platform yararlanmak nasıl dikkatle dikkate almanız gerekir. Bu makalede nasıl geliştiriciler okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) sağlamak için Azure Storage uygulamalarını yüksek oranda kullanılabilir olan kullanabilirsiniz üzerine odaklanır.

[!INCLUDE [storage-common-redundancy-options](../../../includes/storage-common-redundancy-options.md)]

Bu makalede, GRS ve RA-GRS odaklanır. GRS ile depolama hesabı oluştururken seçtiğiniz birincil bölgede verilerinizin üç kopyasını tutulur. Üç ek kopya Azure tarafından belirtilen bir ikincil bölge içinde zaman uyumsuz olarak korunur. İkincil kopya okuma erişimi olması dışında RA-GRS GRS olarak aynı şeydir. Farklı Azure Storage artıklık seçenekleri hakkında daha fazla bilgi için bkz: [Azure Storage çoğaltma](https://docs.microsoft.com/azure/storage/storage-redundancy). Çoğaltma makalede ayrıca birincil ve ikincil bölgeler eşleştirmeleri gösterilmektedir.

Bu makalede ve tam bir örnek, indirin ve çalıştırın sonunda bağlantı dahil kod parçacıkları vardır.

> [!NOTE]
> Azure Storage artık yüksek oranda kullanılabilir uygulamaları oluşturmak için bölge olarak yedekli depolama (ZRS) destekler. ZRS birçok uygulama, artıklık ihtiyaçları için basit bir çözüm sunar. ZRS donanım hataları veya tek bir veri merkezinde etkileyen geri dönülemez afetler karşı koruma sağlar. Daha fazla bilgi için bkz: [bölge olarak yedekli depolama (ZRS): yüksek oranda kullanılabilir Azure Storage uygulamaları](storage-redundancy-zrs.md).

## <a name="key-features-of-ra-grs"></a>RA-GRS temel özellikleri

Bu anahtar noktalarını uygulamanız için RA-GRS tasarlarken unutmayın:

* Azure depolama ikincil bir bölgede birincil bölgenizde verileri salt okunur bir kopyasını tutar. Yukarıda belirtildiği gibi depolama birimi hizmeti ikincil bölge konumunu belirler.

* Salt okunur kopyasıdır [sonuçta tutarlı](https://en.wikipedia.org/wiki/Eventual_consistency) birincil bölge verileri.

* BLOB'lar, tablolar ve Kuyruklar için ikincil bölge için Sorgulayabileceğiniz bir *son eşitleme zamanı* ikincil bölge son birincil çoğaltmayı gerçekleştiği belirten değer. (Bu RA-GRS artıklık şu anda yok Azure dosyaları için desteklenmiyor.)

* Birincil veya ikincil bölge veri ile etkileşim kurmak için depolama istemci Kitaplığı'nı kullanabilirsiniz. Ayrıca yönlendirebilirsiniz birincil bölge için Okuma isteği zaman aşımına uğrarsa istekleri otomatik olarak ikincil bölge okuyun.

* Birincil bölge verilerde erişilebilirliğini etkileyen önemli bir sorun varsa, bir coğrafi hangi noktada birincil bölgesine işaret eden DNS girişlerini ikincil bölge'ye işaret edecek şekilde değiştirilecek yük devretme, Azure ekibi tetikleyebilir.

* Bir coğrafi yük devretme gerçekleşirse, Azure yeni bir ikincil konum seçin ve bu konum için veri çoğaltmak sonra ikincil DNS girişlerini üzerine. Depolama hesabı çoğaltma tamamlanana kadar ikincil uç kullanılamaz. Daha fazla bilgi için lütfen bkz [bir Azure Storage kesinti oluşursa yapmanız gerekenler](https://docs.microsoft.com/azure/storage/storage-disaster-recovery-guidance).

## <a name="application-design-considerations-when-using-ra-grs"></a>RA-GRS kullanırken uygulama tasarım konuları

Bu makalede amacı (içinde bir sınırlı kapasite ile birlikte) birincil veri merkezinde büyük bir felaket durumunda bile çalışmaya devam edecek bir uygulama tasarlamanızı gösterilmektedir olmaktır. Birincil bölge okunurken ile uğratan sorun olduğunda ikincil bölge okunurken tarafından geçici veya uzun süre çalışan sorunlarını gidermek için uygulamanızın tasarlayabilirsiniz. Birincil bölge yeniden kullanılabilir duruma geldiğinde, uygulamanızı birincil bölgesinden okunurken geri dönebilirsiniz.

### <a name="using-eventually-consistent-data"></a>Sonuçta tutarlı verileri kullanma

Önerilen çözüm, çağıran uygulama olası eski verileri döndürmek için kabul edilebilir olduğunu varsayar. İkincil bölge verilerde sonuçta tutarlı olduğundan, ikincil bölge için bir güncelleştirme çoğaltma bitmeden birincil bölge erişilemez hale gelebilir mümkündür.

Örneğin, müşteriniz bir güncelleştirme başarıyla gönderir, ancak ikincil bölge'ye güncelleştirmeyi yayılmadan önce birincil bölge başarısız varsayalım. Müşteri verilerini geri okumak istediğinde, kendisinin eski veri ikincil bölge güncelleştirilen verileri yerine alır. Müşteri ileti nasıl uygulamanızı tasarlarken, bu kabul edilebilir olup olmadığına ve bu durumda, karar vermeniz gerekir. 

Bu makalenin sonraki bölümlerinde son eşitleme ikincil güncel olup olmadığını denetlemek için veriler için zaman ikincil nasıl kontrol edileceğini gösterir.

### <a name="handling-services-separately-or-all-together"></a>Ayrı ayrı veya hepsini bir araya Hizmetleri işleme

Olası olsa da, diğer hizmetlerin hala tamamen işlevseldir, ancak kullanılamaz hale için bir hizmet için mümkündür. İşleyebilir yeniden deneme ve her biri için salt okunur modda hizmet ayrı olarak (BLOB, kuyruklar, tablolar) ya da yeniden deneme tüm depolama hizmetleri için genel birlikte işleyebilir.

Örneğin, uygulamanızda kuyruklar ve BLOB'lar kullanırsanız, bunların her biri için yeniden denenebilir hataları işlemek için ayrı kodda almaya karar verebilirsiniz. Daha sonra bir yeniden deneme blob hizmetinden Al, ancak sıra hizmeti hala çalıştığından, yalnızca parçası blobları işlediği uygulamanızı etkilenir. Tüm depolama hizmeti yeniden deneme genel olarak işlemek karar verin ve yeniden denenebilir hata blob hizmetine yapılan bir çağrı döndürürse blob hizmeti ve sıra hizmeti isteklerine etkilenir.

Sonuç olarak, bu, uygulamanızın karmaşıklığına bağlıdır. Hataları hizmeti tarafından değil işlemeye karar verebilirsiniz, ancak bunun yerine yönlendirmek için okuma istekleri ikincil bölge tüm depolama hizmetleri için birincil bölgesinde bulunan herhangi bir depolama hizmeti ile ilgili bir sorun algılandığında uygulamasını ve salt okunur modda çalıştırmak.

### <a name="other-considerations"></a>Diğer konular

Bu makalenin geri kalanında aşağıdakiler ele alınacaktır diğer konular şunlardır.

*   Yeniden deneme devre kesici desenini kullanarak okuma isteklerinin işlenmesi

*   Sonuçta tutarlı verileri ve son eşitleme zamanı

*   Test Etme

## <a name="running-your-application-in-read-only-mode"></a>Salt okunur modda da uygulamanızı çalıştırma

RA-GRS depolama kullanmak için her iki başarısız okuma isteklerini işlemek için olmalıdır ve güncelleştirme isteği başarısız oldu (Bu durumda eklemeleri anlamı update, güncelleştirmeleri ve silme ile). Başarısız birincil veri merkezi, istekleri ikincil veri merkezine yönlendirildi bölümünü okuyun. Ancak, ikincil salt okunur olduğu için güncelleştirme isteklerinin ikincil yönlendirilemez. Bu nedenle, salt okunur modda çalıştırmak için uygulamanızın tasarlamanız gerekir.

Örneğin, herhangi bir güncelleştirme isteğini Azure depolama birimine gönderilmeden önce işaretli bir bayrak ayarlayabilirsiniz. Bir güncelleştirme isteği geldiğinde, atlayın ve müşteri için uygun bir yanıt döndürür. Hatta belirli özellikler devre dışı bırakmak isteyebilirsiniz Sorun çözülene ve kullanıcılara bu özellikleri geçici olarak devre dışı olduğunu bildirmek kadar değerlerinin.

Hataları her hizmet için ayrı ayrı işlemek karar verirseniz, uygulamanızın hizmeti tarafından salt okunur modda çalıştırma yeteneği işlemek gerekir. Örneğin, etkin ve devre dışı her hizmet için salt okunur bayrakları olabilir. Daha sonra kodunuzda uygun yerlerde bayrağı işleyebilir.

Başka bir yan avantajına sahiptir, uygulamanızın salt okunur modda çalıştırmak için –, bir ana uygulama yükseltmesi sırasında sınırlı işlevsellik sağlamak için olanak sağlar. Yükseltme yaparken hiç kimse birincil bölge verilere sağlayarak uygulamanızın salt okunur modda çalıştırmak ve ikincil veri merkezine noktası tetikleyebilir.

## <a name="handling-updates-when-running-in-read-only-mode"></a>Güncelleştirmeler salt okunur modda çalışırken işleme

Salt okunur modda çalışırken güncelleştirme isteklerini işlemek için birçok yolu vardır. Biz bu kapsamlı kapak olmaz ancak genellikle birkaç düşündüğünüz desenleri vardır.

1.  Kullanıcınız için yanıt ve şu anda güncelleştirmeleri kabul ettiğiniz değil söyleyin. Örneğin, bir kişi yönetim sistemi kişi bilgilerine erişmek, ancak güncelleştirmeleri olmamasını müşterilere sağlayabilir.

2.  Sıraya alma güncelleştirmelerinizi başka bir bölgede olabilir. Bu durumda, farklı bir bölgeye sırada bekleyen güncelleştirme isteklerinizi yazmak ve birincil veri merkezi yeniden çevrimiçi olduktan sonra bu istekleri işlemek için bir yol olması. Bu senaryoda, istenen güncelleştirme daha sonra işlenmek üzere sıraya bilmeniz müşteri izin vermemelisiniz.

3.  Başka bir bölgede depolama hesabı güncelleştirmelerinizi yazabilirsiniz. Birincil veri merkezi tekrar çevrimiçi olduğunda, birincil veri yapısı verilerin bağlı olarak bu güncelleştirmelerin birleştirmek için bir yol olabilir. Örneğin, adında bir tarih damgası ile ayrı dosyaları oluşturuyorsanız, bu dosyaları birincil bölgesine kopyalayabilirsiniz. Bu günlüğe kaydetme ve IOT verileri gibi bazı iş yükleri için çalışır.

## <a name="handling-retries"></a>Yeniden deneme işleme

Bunu nasıl hangi hataları yeniden denenebilir bilmeniz? Bu depolama istemcisi kitaplığı tarafından belirlenir. Örneğin, onu yeniden deneniyor başarılı şekilde neden büyük olasılıkla olmadığından 404 hatası (kaynak bulunamadı) yeniden denenebilir değil. Öte yandan, bir 500 sunucu hatası olduğundan ve yalnızca geçici bir sorun olabilir yeniden denenebilir hatasıdır. Daha fazla ayrıntı için kullanıma [açık kaynak kodu ExponentialRetry sınıfı için](https://github.com/Azure/azure-storage-net/blob/87b84b3d5ee884c7adc10e494e2c7060956515d0/Lib/Common/RetryPolicies/ExponentialRetry.cs) .NET depolama istemci Kitaplığı'nda. (ShouldRetry yöntemi arayın.)

### <a name="read-requests"></a>Okuma istekleri

Birincil depolama ile ilgili bir sorun ise ikincil depolamaya Okuma isteği yönlendirilebilir. İçinde yukarıda olarak belirtildiği [sonunda tutarlı verileri kullanarak](#using-eventually-consistent-data), potansiyel olarak eski verileri okumak, uygulamanız için kabul edilebilir olmalıdır. RA-GRS verilere erişmek için depolama istemci kitaplığı kullanıyorsanız için bir değer ayarlayarak Okuma isteği yeniden deneme davranışını belirtebilirsiniz **LocationMode** özelliğini şunlardan biri:

*   **PrimaryOnly** (varsayılan)

*   **PrimaryThenSecondary**

*   **SecondaryOnly**

*   **SecondaryThenPrimary**

Ayarladığınızda **LocationMode** için **PrimaryThenSecondary**, istemci yeniden denenebilir bir hatasıyla birincil uç noktası başarısız ilk Okuma isteği ikincil uç noktasına başka bir okuma isteği otomatik olarak yapıyorsa. Bir sunucu zaman aşımı hatası ise, istemci hizmetinden yeniden denenebilir hata almadan önce süresi dolacak şekilde zaman aşımı beklemeniz gerekecektir.

Temel olarak ne zaman yeniden denenebilir hata isteklerine nasıl karar verirken dikkate alınması gereken iki senaryo vardır:

*   Bu yalıtılmış bir sorundur ve birincil uç noktası için sonraki istekleri yeniden denenebilir hata döndürmez. Geçici ağ hatası olduğunda, bu durum oluşabilir bir örnektir.

    Bu senaryoda, yoktur önemli performansta düşme olmaz elde etmeyle **LocationMode** kümesine **PrimaryThenSecondary** yalnızca seyrek böyle gibi.

*   Bu en az bir birincil bölge içinde depolama hizmetleri ile ilgili bir sorun ve tüm istekler o hizmet birincil bölgede bir süre için yeniden denenebilir hata döndürüyor olabilir. Bu birincil bölge tamamen erişilemez örneğidir.

    Bu senaryoda, bulunmaktadır performans tüm okuma istekleri birincil endpoint ilk deneyin, bekleme zaman aşımı süresi sonra ikincil uç noktasına geçiş için.

Bu senaryolarda, bunları olmadığını birincil uç noktası ile devam eden bir sorundur ve ayarlayarak isteklerini doğrudan ikincil uç noktaya okuma gönderme tanımlamanız gerekir **LocationMode** özelliğine **SecondaryOnly**. Şu anda salt okunur modda çalıştırmak için uygulama aynı zamanda değiştirmeniz gerekir. Bu yaklaşım olarak bilinen [devre kesici düzeni](https://msdn.microsoft.com/library/dn589784.aspx).

### <a name="update-requests"></a>Güncelleştirme isteği

Devre kesici düzeni, ayrıca güncelleştirme isteklerinin uygulanabilir. Ancak, güncelleştirme isteklerinin salt okunur ikincil depolama yönlendirilemez. Bu istekler için bırakmanız **LocationMode** özelliğini **PrimaryOnly** (varsayılan). Bu hataları işlemek için bu istekleri – bir satırda – 10 hataları gibi bir ölçümü uygulamak ve, eşiğine ulaşıldığında salt okunur modda uygulamasına geçiş yapabilirsiniz. Aynı yöntemleri döndürmek için bu aşağıda devre kesici desen hakkında sonraki bölümde açıklanan şekilde modu güncelleştirmek için kullanabilirsiniz.

## <a name="circuit-breaker-pattern"></a>Devre Kesici düzeni

Devre kesici düzeni uygulamanızda kullanarak büyük olasılıkla art arda başarısız olan işlem yeniden deneniyor engelleyebilirsiniz. Uygulama çalışmaya devam etmesini sağlar yapmak yerine alma işlemi sırasında süresini üstel olarak denenir. Hataya sabit olduğunda, aynı zamanda uygulama işlemi yeniden deneyebilirsiniz de algılar.

### <a name="how-to-implement-the-circuit-breaker-pattern"></a>Devre kesici düzeni uygulama

Birincil bir uç noktası ile devam eden bir sorun olduğunu belirlemek için ne sıklıkta istemci yeniden denenebilir hatayla karşılaştığında izleyebilirsiniz. Her durumda farklı olduğundan, ikincil uç noktasına geçin ve salt okunur modda uygulamayı çalıştırmak için kararı için kullanmak istediğiniz eşik karar vermeniz gerekir. Örneğin, bir satırda hiçbir başarı ile 10 hata varsa, geçişi gerçekleştirmek karar. 2 dakikalık bir süre içinde istekleri % 90'ını başarısız olursa geçiş başka bir örnektir.

İlk senaryo için yalnızca başarısızlık sayısı tutmak ve varsa bir başarı maksimum ulaşmadan önce sayısı sıfır olarak ayarlayın. İkinci senaryo için uygulamak için bir yol MemoryCache nesnesi (.NET) kullanmaktır. Her istek için bir CacheItem önbelleğine ekleme değeri (1) başarılı durumuna ayarlayın veya (0) başarısız ve sona erme zamanı şimdi (veya ne olursa olsun, zaman sınırlaması) 2 dakika olarak ayarlayın. Bir girdi sona erme süresine ulaşıldığında, giriş otomatik olarak kaldırılır. Bu bir çalışırken 2 dakikalık penceresi verir. Depolama hizmetine bir istek yaptığınızda, ilk LINQ sorgusu MemoryCache nesne boyunca yüzde başarı değerlerin toplamını ve bazında sayı bölen tarafından hesaplamak için kullanın. Yüzde başarı (örneğin, % 10) bazı eşiğin altına düştüğünde ayarlamak **LocationMode** özelliği okuma isteklerini **SecondaryOnly** ve devam etmeden önce salt okunur modda uygulamasına geçin.

Yapılandırılabilir parametreler yapmadan dikkate almanız gereken şekilde geçiş yapmak ne zaman belirlemek için kullanılan hataları eşiğinin uygulamanızda, hizmetten hizmete farklılık gösterebilir. Bu aynı zamanda her hizmetinden yeniden denenebilir hataları ayrı ayrı işlemek karar burada olup ya da bir, daha önce bahsedildiği gibi olarak.

Bir uygulama birden çok örneğini nasıl ele alınacağını ve her örnek yeniden denenebilir hata algılamak ne yapacakları bunun başka bir konudur. Örneğin, 20 VM'ler yüklenen uygulamayla çalışıyor olabilir. Her örneği ayrı ayrı işlemek? Bir örnek bir sorun olduğunda sorunlarınız bir örneğini başlatır, örneğine yanıt olarak yalnızca bir sınırlamak istediğiniz ya da sahip denemek istiyor musunuz tüm örnekleri aynı şekilde yanıt? Ayrı ayrı örnekleri işleme yanıtı koordine etmek çalışırken daha çok daha kolaydır, ancak bunu nasıl yapacağınız uygulamanızın mimarisine bağlıdır.

### <a name="options-for-monitoring-the-error-frequency"></a>Hata sıklığı izleme seçenekleri

İkincil bölge'ye geçiş yapın ve uygulamayı salt okunur modunda çalışacak şekilde değiştirmek ne zaman belirlemek üzere birincil bölge içinde yeniden deneme sıklığını izleme için üç ana seçeneğiniz vardır.

*   İçin bir işleyici ekleyin [ **yeniden deneniyor** ](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.operationcontext.retrying.aspx) olayda [ **OperationContext** ](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.operationcontext.aspx) depolama alanınızın geçirdiğiniz nesne istekleri – bu makalede gösterilen ve eşlik eden örnekte kullanılan yöntem budur. İstemci ne sıklıkta istemci birincil bir uç noktada yeniden denenebilir hatayla karşılaştığında izlemenizi sağlayan bir isteği yeniden deneme zaman bu olayları kov.

    ```csharp 
    operationContext.Retrying += (sender, arguments) =>
    {
        // Retrying in the primary region
        if (arguments.Request.Host == primaryhostname)
            ...
    };
    ```

*   İçinde [ **değerlendir** ](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.retrypolicies.iextendedretrypolicy.evaluate.aspx) yöntemi bir özel yeniden deneme İlkesi'nde çalıştırabilirsiniz özel kod her bir yeniden deneme gerçekleşir. Yeniden deneme zaman kaydı yanı sıra olur, bu da, yeniden deneme davranışı değiştirme olanağı sağlar.

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

*   Birincil storage uç noktanız okuma (örneğin, küçük bir blob okuma) istekleri kukla ile sürekli olarak ping atar uygulamanızda özel bir izleme bileşeni uygulamak için üçüncü yaklaşımdır, sistem durumunu belirlemek için. Bu, bazı kaynaklar ancak önemli ölçüde kalırsınız. Ardından, eşiğe ulaştığında bir sorun algılandığında anahtara gerçekleştireceği **SecondaryOnly** ve salt okunur modda.

Belirli bir noktada geri kullanan birincil uç noktasını ve güncelleştirmelere izin için geçiş yapmak istersiniz. Rasgele seçilen bir zaman miktarı veya işlemlerinin sayısı gerçekleştirildikten sonra yukarıda listelenen ilk iki yöntemden birini kullanıyorsanız, yalnızca birincil endpoint ve güncelleştirme modunu etkinleştirmek için geçiş. Ardından, yeniden deneme mantığı yeniden Git izin verebilirsiniz. Sorunun giderilmiş, birincil uç noktası kullan ve güncelleştirmelere izin vermek devam eder. Hala bir sorun varsa, bu kez daha ikincil uç noktaya ve salt okunur modda için ayarladığınız ölçütleri başarısız sonra geçiş yapar.

Üçüncü senaryo için birincil depolama endpoint ping yeniden başarılı olduğunda anahtar tetikleyebilir geri **PrimaryOnly** ve güncelleştirmelere izin devam edin.

## <a name="handling-eventually-consistent-data"></a>Sonuçta tutarlı verileri işleme

RA-GRS, birincil bölgedeki işlemleri ikincil bölgede çoğaltarak çalışır. Bu çoğaltma işlemi ikincil bölge verilerde olduğunu garanti *sonuçta tutarlı*. Birincil bölge içinde tüm işlemlerin sonunda ikincil bölge'de görünür, ancak bunlar ilk olarak birincil bölgede uygulanan aynı sırada ikincil bölgede olabileceğini bir gecikme göründükleri önce ve işlemleri garantisi yoktur gelmesini bu anlamına gelir. İşlemlerinizi sırasıyla dışında ikincil bölgede ulaşırsa, *olabilir* hizmet arayı kapatıncaya kadar tutarsız bir durumda olmasını ikincil bölge verilerinizi göz önünde bulundurun.

Aşağıdaki tabloda her bir üyesi yapmak için bir çalışan ayrıntılarını güncelleştirdiğinizde ne gerçekleşebilir örneği gösterilmektedir *Yöneticiler* rol. Bu örnek amacıyla, bu güncelleştirme gerektirir **çalışan** varlık ve güncelleştirme bir **Yönetici rolü** varlık ile yöneticiler sayısı toplam sayısı. Nasıl güncelleştirmeleri bozuk ikincil bölge'de uygulandığını dikkat edin.

| **Saat** | **İşlem**                                            | **Çoğaltma**                       | **Son eşitleme zamanı** | **Sonuç** |
|----------|------------------------------------------------------------|---------------------------------------|--------------------|------------| 
| T0       | İşlem A: <br> Çalışanı Ekle <br> birincil varlık |                                   |                    | Birincil olarak eklenen bir işlem,<br> henüz çoğaltılmamış. |
| T1       |                                                            | İşlem A <br> Çoğaltılan<br> ikincil | T1 | İşlem bir ikincil yinelenmiş. <br>Son eşitleme zamanı güncelleştirildi.    |
| T2       | İşlem B:<br>Güncelleştirme<br> çalışan varlık<br> birincil  |                                | T1                 | İşlem için birincil yazılmış B,<br> henüz çoğaltılmamış.  |
| T3       | İşlem C:<br> Güncelleştirme <br>Yönetici<br>Rol varlığı<br>birincil |                    | T1                 | İşlem için birincil yazılmış C,<br> henüz çoğaltılmamış.  |
| *T4*     |                                                       | İşlem C <br>Çoğaltılan<br> ikincil | T1         | İşlem için ikincil çoğaltılan C.<br>Olduğundan güncelleştirilmedi LastSyncTime <br>işlem B henüz çoğaltılmamış.|
| *T5*     | Varlıkların okuma <br>İkincil                           |                                  | T1                 | Çalışanın eski değer alma <br> Varlık işlem B kurmadı çünkü <br> henüz çoğaltılan. Yeni değerini alın<br> Yönetici rolü varlığı C olduğundan<br> Çoğaltılan. Son eşitleme zamanı hala tamamlanmamış<br> Silinmiş olduğundan güncelleştirilmiş işlem B<br> Çoğaltılan kurmadı. Size söyleyebilir<br>Yönetici rolü varlığı tutarsız. <br>Varlık tarih/saat sonra olduğundan <br>Son eşitleme zamanı. |
| *T6*     |                                                      | İşlem B<br> Çoğaltılan<br> ikincil | T6                 | *T6* – C aracılığıyla tüm işlemlerin <br>edilmiş çoğaltılan, son eşitleme zamanı<br> güncelleştirilir. |

Bu örnekte, istemci T5 konumunda ikincil bölgesinden okunurken geçer varsayalım. Başarıyla okuyabilirsiniz **Yönetici rolü** varlık şu anda, ancak varlık sayısı ile tutarlı değil yöneticiler sayısı için bir değer içeriyorsa **çalışan** şu anda ikincil bölge yöneticileri olarak işaretlenmiş varlıklar. İstemci, yalnızca bu değerle tutarsız bilgiler olduğunu risk görüntüleyebilirsiniz. Alternatif olarak, istemci belirleyen yararlanmaya **Yönetici rolü** güncelleştirmeleri sıralama dışında gerçekleşen ve sonra bu olgu kullanıcı bildirmeniz büyük olasılıkla tutarsız bir durumda olduğundan.

Büyük olasılıkla tutarsız veri sahipse, istemci değerini kullanabilir tanımak için *son eşitleme zamanı* herhangi bir anda bir depolama birimi hizmeti sorgulayarak alabilirsiniz. Bu, ikincil bölge verilerinde son zaman bildirir tutarlı ve ne zaman hizmeti uygulanan o noktadan önce tüm işlemlerin süre. Hizmet ekledikten sonra yukarıda gösterilen örnekte **çalışan** ikincil bölge, son eşitleme zamanı varlık ayarlanmış *T1*. Kalır *T1* hizmet güncelleştirmeleri kadar **çalışan** ayarlandığında ikincil bölge varlık *T6*. İstemci varlıkta, okur, son eşitleme zamanı alır, *T5*, bu varlık üzerinde damgasıyla karşılaştırabilirsiniz. Varlık zaman damgasını son eşitleme zamanından daha sonra ise, varlık büyük olasılıkla tutarsız bir durumda ise ve ne olursa olsun, uygulamanız için uygun eylemi gerçekleştirin. Bu alanı kullanarak birincil son güncelleştirmeyi tamamlandığı bilmeniz gerekir.

## <a name="testing"></a>Test Etme

Uygulamanızı yeniden denenebilir hatalarla karşılaştığında beklendiği gibi davranır test etmek önemlidir. Örneğin, birincil bölge yeniden kullanılabilir duruma geldiğinde uygulama ikincil ve bir sorun algılar ve anahtarları salt okunur moduna geçiş yapar, test gerekir. Bunu yapmak için yeniden denenebilir hata ve oluşma sıklığını denetim benzetimini yapmak için bir yönteme ihtiyacınız vardır.

Kullanabileceğiniz [Fiddler](http://www.telerik.com/fiddler) kesecek ve HTTP yanıtını bir betik içinde değiştirin. Bu komut dosyasını birincil uç noktasından gelen yanıtları tanımlamak ve depolama istemci kitaplığı yeniden denenebilir hata olarak tanıdığı bir HTTP durum kodu değiştirin. Bu kod parçacığını karşı Okunan isteklere yanıt karşılar Fiddler komut basit bir örnek gösterilmektedir **employeedata** tablo 502 durumuna döndürmek için:

```java
static function OnBeforeResponse(oSession: Session) {
    ...
    if ((oSession.hostname == "\[yourstorageaccount\].table.core.windows.net")
      && (oSession.PathAndQuery.StartsWith("/employeedata?$filter"))) {
        oSession.responseCode = 502;
    }
}
```

Bu örnek, geniş bir istekleri kesecek ve yalnızca değiştirme genişletebilirsiniz **yanıt kodu** bazı bunları gerçek dünya senaryoları daha iyi benzetmek için. Fiddler betikleri özelleştirme hakkında daha fazla bilgi için bkz: [bir istek veya yanıt değiştirme](http://docs.telerik.com/fiddler/KnowledgeBase/FiddlerScript/ModifyRequestOrResponse) Fiddler belgelerinde.

Salt okunur modda yapılandırılabilir uygulamanıza geçiş yapmak için eşikler yaptıysanız, üretim dışı işlem birimleri ile davranışı test etmek daha kolay olacaktır.

## <a name="next-steps"></a>Sonraki Adımlar

* Başka bir örneği LastSyncTime ayarlanma dahil olmak üzere ilgili okuma erişimi coğrafi Yedeklilik daha fazla bilgi için lütfen bkz [Windows Azure Depolama artıklık seçenekleri ve coğrafi olarak yedekli depolamaya okuma erişimi](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/).

* İleri ve geri birincil ve ikincil uç noktalar arasında geçiş yapmak nasıl gösteren tam bir örnek için lütfen bkz. [devre kesici desen ile RA-GRS depolama kullanan Azure örnekler –](https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs).
