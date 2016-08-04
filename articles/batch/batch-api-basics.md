<properties
    pageTitle="Azure Batch özelliklerine genel bakış | Microsoft Azure"
    description="Batch hizmeti özelliklerini ve API’lerini geliştirme açısından öğrenin."
    services="batch"
    documentationCenter=".net"
    authors="yidingzhou"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="05/12/2016"
    ms.author="yidingz;marsma"/>

# Azure Batch özelliklerine genel bakış

Bu makale Azure Batch hizmetinin temel API özelliklerine temel bir bakış sağlar. [Batch REST][batch_rest_api] ya da [Batch .NET][batch_net_api] API’leri kullanarak dağıtılmış hesaplama çözümü geliştirirken aşağıda bahsedilen birçok varlığı ve özelliği kullanacaksınız.

> [AZURE.TIP] Batch hizmetine daha yüksek düzeyde teknik genel bakış için bkz. [Azure Batch temelleri](batch-technical-overview.md).

## <a name="workflow"></a>Batch hizmeti iş akışı

Aşağıdaki, Batch hizmetinde geliştirilen neredeyse tüm dağıtılmış hesaplama senaryoları tarafından kullanılan tipik bir üst düzey iş akışıdır.

1. Dağıtılmış hesaplama senaryonuzda kullanmak istediğiniz *veri dosyalarını* bir [Azure Storage][azure_storage] hesabına yükleyin. Bu dosyalar, Batch hizmetinin bunlara erişebilmesi için Storage hesabında olmalıdır. Görevler çalıştıklarında bu dosyaları [işlem düğümlerine](#computenode) indirir.

2. Bağımlı *ikili dosyaları* Storage hesabınıza yükleyin. Bu ikili dosyalar, görevler ile bunların bağımlı derlemeleri tarafından çalıştırılacak programı içerir. Görevler tarafından işlem düğümlerine indirilebilmeleri için bu dosyalara Storage hesabınızdan da erişilebilmelidir.

3. İşlem düğümleri [Havuzu](#pool) oluşturun. Havuz oluşturulduğunda kullanılacak [hesaplama düğümleri boyutunu][cloud_service_sizes] belirlersiniz; bir görev çalıştırıldığında bu, havuzdaki bir düğüme atanır.

4. Bir [İş](#job) oluşturun. Bir iş, toplama görevlerini yönetmenizi sağlar.

5. İşe [Görevler](#task) ekleyin. Her görev, Storage hesabınıza yüklediğiniz veri dosyalarındaki bilgileri işlemek üzere karşıya yüklediğiniz programı kullanır.

6. İşin ilerleme durumunu izleyin ve sonuçları alın.

> [AZURE.NOTE] Batch hizmetini kullanmak için bir [Batch hesabına](batch-account-create-portal.md) ihtiyacınız olur ve neredeyse tüm çözümler dosya depolama ve alma amacıyla [Azure Storage][azure_storage] hesabı kullanır. Batch şu anda, [Azure Storage hesapları hakkında](../storage/storage-create-storage-account.md) bölümünde 5. adım olan [Depolama hesabı oluşturma](../storage/storage-create-storage-account.md#create-a-storage-account) adımında açıklandığı gibi, sadece **Genel amaçlı** depolama hesabı türünü destekler.

Aşağıdaki bölümlerde yukarıdaki iş akışında bahsedilen her bir kaynağın yanı sıra Batch’in, dağıtılmış hesaplama senaryonuzu olanaklı kılacak birçok özelliği hakkında bilgi edineceksiniz.

## <a name="resource"></a> Batch hizmetinin kaynakları

Batch hizmetini kullanırken aşağıdaki kaynaklardan birçoğunu kullanırsınız. Bu kaynakların bazıları (hesaplar, işlem düğümleri, havuzlar, işler ve görevler gibi) tüm Batch çözümlerinde kullanılır. İş zamanlamaları ve uygulama paketleri gibi diğer kaynaklar da faydalıdır, ancak isteğe bağlıdır.

- [Hesap](#account)
- [İşlem düğümü](#computenode)
- [Havuz](#pool)
- [İş](#job)
- [Görev](#task)
    - [Başlangıç görevi](#starttask)
    - [İş yöneticisi görevi](#jobmanagertask)
    - [İş hazırlama ve bırakma görevleri](#jobpreprelease)
    - [Çok örnekli görevler](#multiinstance)
    - [Görev bağımlılıkları](#taskdep)
- [İş zamanlamaları](#jobschedule)
- [Uygulama paketleri](#appkg)

### <a name="account"></a>Hesap

Bir Batch hesabı Batch hizmeti dahilinde benzersiz şekilde tanımlanan bir varlıktır. Tüm işlemler bir Batch hesabıyla ilişkilendirilir. Batch hizmeti ile işlemler gerçekleştirdiğinizde, size hem hesap adı hem de hesap anahtarı gerekir. Bir Batch hesabı oluşturmak için bkz. [Azure portalda bir Azure Batch hesabı oluşturma ve yönetme](batch-account-create-portal.md).

### <a name="computenode"></a>İşlem düğümü

İşlem düğümü, uygulamanız için belirli bir iş yüküne ayrılmış Azure sanal makinesidir. Bir düğümün boyutu; düğüm için ayrılan CPU çekirdekleri sayısı, bellek kapasitesi ve yerel dosya sistemi boyutunu belirler. Bir düğüm, A0 dışında herhangi bir [bulut hizmeti düğümü boyutunda][cloud_service_sizes] olabilir.

Düğümler; yürütülebilir dosyalar (.exe), komut (.cmd) dosyaları, toplu iş (.bat) dosyaları ve PowerShell betikleri gibi yürütebilir dosyaları ve betikleri çalıştırabilir. Bir düğüm ayrıca aşağıdaki özniteliklere sahiptir:

- Her işlem düğümünde, düğümün yolunu ayrıntılı şekilde gösteren standart **klasör yapısı** ve ilişkili **ortam değişkenleri** oluşturulur. Daha fazla bilgi için aşağıdaki [Dosyalar ve dizinler](#files)’e bakın.
- Referans için görevlere göre **ortam değişkenleri** verilmiştir.
- Erişimi denetlemek için yapılandırılan **Güvenlik Duvarı** ayarları.
- Bir işlem düğümüne **uzaktan erişim** gerekiyorsa (örneğin hata ayıklama için), *Uzak Masaüstü* aracılığıyla düğüme erişmek üzere daha sonra kullanılabilecek bir RDP dosyası edinilebilir.

### <a name="pool"></a>Havuz

Havuz, uygulamanızın üzerinde çalıştığı bir koleksiyondur. Havuz sizin tarafınızdan elle ya da siz yapılacak işleri belirttiğinizde Batch hizmeti tarafından otomatik olarak oluşturulabilir. Uygulamanızın ihtiyaçlarını karşılayan bir havuz oluşturabilir ve yönetebilirsiniz; havuzlar yalnızca oluşturulduğu Batch hesabı tarafından kullanılabilir. Bir Batch hesabı birden fazla havuza sahip olabilir.

Azure Batch havuzları temel Azure işlem platformu üzerine oluşturulur: Batch havuzları; büyük ölçekli ayırma, uygulama yüklemesi, veri dağıtımı ve durum izlemenin yanı sıra bir havuzdaki işlem düğümü sayısının esnek şekilde ayarlanmasını (ölçeklendirme) sağlar.

Bir havuza eklenen her düğüme benzersiz bir ad ve IP adresi atanır. Bir düğüm havuzdan kaldırıldığında, işletim sistemi ya da dosyalara yapılan tüm değişiklikler kaybedilir ve düğümün adı ile IP adresi gelecekte kullanım için boşta kalır. Bir düğüm havuzdan ayrıldığında ömrü sona erer.

Bir havuzu içindeki düğümler arasında iletişime izin verecek şekilde yapılandırabilirsiniz. Bir havuz tarafından havuzlar arası bir iletişim istenirse Batch hizmeti havuzdaki her düğümde 1100’den fazla bağlantı noktasını etkinleştirir. Havuzdaki her düğüm yalnızca bu bağlantı noktası aralığına ve sadece havuzdaki diğer düğümlerden gelen iletişimlere izin vermek üzere yapılandırılır. Uygulamanız düğümler arasında iletişim gerektirmiyorsa Batch hizmeti, artan paralel işleme gücünü etkinleştirmek amacıyla birçok kümeden ve veri merkezinden çok sayıda düğümü havuza ayırabilir.

Bir havuz oluşturduğunuzda aşağıdaki öznitelikleri belirtebilirsiniz:

- Havuzdaki **düğümlerin boyutu** 
    - Uygun bir düğüm boyutu, düğümlerde çalıştırılacak olan uygulamanın veya uygulamaların özellikleri ve gereksinimleri dikkate alınarak seçilmelidir. Düğüm boyutu genelde düğümde bir seferde bir görev çalışacağı varsayılarak seçilir. Uygulamanın çok iş parçacıklı olup olmadığı ve ne kadar bellek kullandığı gibi konular dikkate alındığında en uygun ve ekonomik düğüm boyutu belirlenir. Birden fazla göreve sahip olunması ve birden fazla uygulama örneğinin paralel olarak çalıştırılması mümkündür; bu durumda genelde daha büyük bir düğüm seçilir. Daha fazla bilgi için aşağıdaki “Görev zamanlama ilkesi” bölümüne bakın.
    - Bir havuzdaki tüm düğümlerin aynı boyutta olması gerekir. Farklı sistem gereksinimlerine ve/veya yük düzeylerine sahip farklı uygulamalar çalıştırılacaksa ayrı havuzlar oluşturulmalıdır.
    - Bir havuz için, A0 dışında tüm [bulut hizmeti düğümü boyutları][cloud_service_sizes] yapılandırılabilir.

- Düğümlerde çalışan **işletim sistemi ailesi** ve **sürümü**
    - Cloud Services dahilindeki çalışan rollerinde, *İşletim Sistemi Ailesi* ve *İşletim Sistemi Sürümü* belirlenebilir (çalışan rolleri hakkında daha fazla bilgi için bkz. *Azure Tarafından Sağlanan İşlem Barındırma Seçenekleri* içindeki [Bana Cloud Services hakkında bilgi ver][about_cloud_services] bölümü).
    - İşletim Sistemi Ailesi, işletim sistemiyle hangi .NET sürümlerinin yüklendiğini de belirler.
    - Çalışan rollerinde, düğümlerin otomatik olarak yükseltilmesi için işletim sistemi sürümüne yönelik `*` belirtilmesi önerilir ve yeni yayımlanmış sürümlerin gereksinimini karşılamak için çalışma yapılması gerekmez. Belirli bir işletim sistemi sürümünün seçildiği birincil kullanım durumu, sürümün güncelleştirilmesine izin vermeden önce geriye dönük uyumluluk testinin gerçekleştirilmesine izin vererek uygulama uyumluluğunun sağlandığından emin olmaktır. Doğrulandığında, havuzun işletim sistemi sürümü güncelleştirilebilir ve yeni işletimi sistemi yüklenebilir; çalışan tüm görevler kesilir ve yeniden kuyruğa alınır.

- Havuz için kullanılabilir olması gereken **hedef düğüm sayısı**

- Havuz için **ölçeklendirme ilkesi**
    - Düğüm sayısının yanı sıra bir havuz için [otomatik ölçeklendirme formülü](batch-automatic-scaling.md) de belirtebilirsiniz. Batch hizmeti formülü yürütür ve ardından belirtebileceğiniz çeşitli havuz, iş ve görev parametrelerindeki düğüm sayısı temelinde düğüm sayısını ayarlar.

- **Görev zamanlama** ilkesi
    - [Düğüm başına maksimum görev](batch-parallel-node-tasks.md) yapılandırma seçeneği havuzdaki her bir düğümde paralel olarak çalıştırabilecek maksimum görev sayısını belirler.
    - Varsayılan yapılandırma, tek seferde bir işlem düğümünde çalıştırılacak bir görevdir, ancak bir düğümde aynı anda birden fazla görev yürütülmesinin faydalı olduğu senaryolar da vardır. Bir örneği, uygulamanın G/Ç için beklemesi gerektiğinde düğüm kullanımını artırmaktır. Eşzamanlı olarak yürütülen birden fazla uygulamaya sahip olunması CPU kullanımı artırır. Başka bir örnek havuzdaki düğüm sayısını azaltmaktır. Bu, büyük referans veri kümeleri için gerekli veri miktarını düşürebilir; bir uygulama için A1 düğüm boyutu yeterliyse, bunun yerine A4 düğüm boyutu seçilebilir ve her biri bir çekirdek kullanan 8 paralel görev için havuz yapılandırılabilir.
    - Batch hizmetinin tüm düğümlerde görevlere eşit olarak yayılıp yayılmayacağını belirleyen ya da havuzdaki başka bir düğüme göre atamadan önce maksimum görev sayısıyla her bir düğümü paketleyen “dolgu türü” de belirlenebilir. 

- Havuzdaki düğümlerin **iletişim durumu** 
    - Bir havuz, temel aldığı ağ altyapısını belirleyen havuzdaki düğümler arasında iletişime izin verecek şekilde yapılandırılabilir. Bunun kümelerdeki düğümlerin yerleşimini de etkilediğini unutmayın.
    - Çoğu senaryoda, görevler bağımsız olarak çalışır ve birbirleriyle iletişim kurması gerekmez, ancak görevlerin iletişim kurması gereken bazı uygulamalar olabilir.

- Havuzdaki düğümler için **başlangıç görevi** 
    - *Başlangıç görevi*, bir işlem düğümünün havuza her katılışında ve bir düğüm her başlatıldığında belirlenebilir. Bu genellikle düğümde çalışan görevler tarafından kullanılacak bir uygulama yüklemek için kullanılır.

### <a name="job"></a>İş

Bir iş, görevlerin koleksiyonudur ve bir havuzdaki işlem düğümlerinde hesaplamanın nasıl gerçekleştirileceğini belirtir.

- İş, çalışmanın çalıştırılacağı **havuzu** belirtir. Havuz, birçok iş tarafından kullanılmak için daha önce oluşturulmuş mevcut bir havuz olabilir ya da iş zamanlamasıyla ilişkili her bir iş için isteğe bağlı olarak ya da bir iş zamanlamasıyla ilişkili tüm işler için oluşturulabilir.
- İsteğe bağlı **İş önceliği** belirtilebilir. Bir iş, devam eden işlerden daha yüksek öncelikle gönderilirse yüksek önceliğe sahip işin görevleri düşük önceliğe sahip iş görevlerinin önünde kuyruğa eklenir. Çalışmakta olan düşük öncelikli görevler engellenemez.
- İş **kısıtlamaları** işleriniz için belirli sınırları belirler.
    - İşler için **maksimum duvar saati süresi** ayarlanabilir. İşler belirtilen maksimum duvar saati süresinden daha uzun çalışırsa, iş ve ilişkili tüm görevler sonlandırılır.
    - Azure Batch başarısız görevleri algılayabilir ve yeniden deneyebilir. **Maksimum görev yeniden deneme sayısı**, bir görevin her zaman yeniden deneneceği veya hiçbir zaman yeniden denenmeyeceği gibi kısıtlama olarak belirtilebilir. Bir görevin yeniden denenmesi tekrar çalıştırmak üzere yeniden kuyruğa alınması anlamına gelir.
- Görevler istemci uygulamanız tarafından işe eklenebilir ya da [İş Yöneticisi görevi](#jobmanagertask) belirtilebilir. Bir proje yöneticisi görevi, Batch API’sini kullanır ve havuzdaki işlem düğümlerinden birinde çalıştırılan görevle birlikte bir iş için gereken görevleri oluşturmak üzere gerekli bilgileri içerir. İş yöneticisi görevi özellikle Batch tarafından işlenir. İşin oluşturulmasının hemen ardından kuyruğa alınır ve başarısız olursa yeniden başlatılır. İş örneği oluşturulmadan görevleri tanımlamanın tek yolu olduğundan İş Yöneticisi görevi, bir iş zamanlaması tarafından oluşturulan işler için gereklidir. İş yöneticisi görevleri hakkında daha fazla bilgi aşağıda yer almaktadır.

### <a name="task"></a>Görev

Görev bir işle ilişkili hesaplama birimidir ve düğüm üzerinde çalışır. Görevler yürütülmek için bir düğüme atanır veya bir düğüm serbest kalana kadar kuyruğa alınır. Bir görev aşağıdaki kaynakları kullanır:

- Görevin **komut satırında** belirtilen uygulama.

- İşlenecek verileri içeren **kaynak dosyalar**. Bu dosyalar, **Genel amaçlı** Azure Storage hesabındaki bir Blob Storage’dan düğüme otomatik olarak kopyalanır. Daha fazla bilgi için aşağıda *Başlangıç görevi* ve [Dosyalar ve dizinler](#files)’e bakın.

- Uygulamanın gerektirdiği **ortam değişkenleri**. Daha fazla bilgi için aşağıda yer alan [Görevler için ortam ayarları](#environment) bölümüne bakın.

- Hesaplar gerçekleşirken kullanılacak **kısıtlamalar**. Örneğin, görevin çalışmasına izin verilen maksimum süre, başarısız olan bir görevin maksimum yeniden denenme sayısı ve çalışma dizinindeki dosyaların maksimum elde tutulma süresi.

Bir düğümde hesaplama gerçekleştirmek üzere tanımladığınız görevlere ek olarak, aşağıdaki özel görevler de Batch hizmeti tarafından sağlanır:

- [Başlangıç görevi](#starttask)
- [İş yöneticisi görevi](#jobmanagertask)
- [İş hazırlama ve bırakma görevleri](#jobmanagertask)
- [Çok örnekli görevler](#multiinstance)
- [Görev bağımlılıkları](#taskdep)

#### <a name="starttask"></a>Başlangıç görevi

**Başlangıç görevini** bir havuzla ilişkilendirerek, yazılım yükleme ya da arka plan işlemlerini başlatma gibi eylemleri gerçekleştirerek, görev düğümlerinin işletim ortamını yapılandırabilirsiniz. Başlangıç görevi, havuzda kaldığı sürece, düğümün havuza ilk eklendiği zaman dahil olmak üzere, bir düğüm her başlatıldığında çalışır. Başlangıç görevinin birincil avantajı, işlem düğümlerini yapılandırmada ve iş görevi yürütmede gereken uygulamaları yüklemek için gerekli tüm bilgileri içermesidir. Bu nedenle, bir havuzdaki düğümlerin sayısını artırmak yeni hedef düğüm sayısını belirtmek kadar kolaydır. Batch halihazırda yeni düğümleri yapılandırmak ve görevleri kabul etmek için bunları hazır hale getirmek için gerekli tüm bilgilere sahiptir.

Her Batch görevinde olduğu gibi, [Azure Storage][azure_storage]’daki **kaynak dosyaların** bir listesi, yürütülecek **komut satırına** ek olarak belirtilebilir. Azure Batch önce dosyaları Azure Storage’dan kopyalayacak, sonra komut satırını çalıştıracaktır. Havuz başlangıç görevinde, dosya listesi genellikle uygulama paketi veya dosyaları içerir, ancak işlem düğümlerinde çalışan tüm görevler tarafından kullanılacak başvuru verilerini de içerebilir. Başlangıç görevinin komut satırı bir PowerShell betiğini çalıştırabilir ya da `robocopy` işlemi gerçekleştirebilir; örneğin, uygulama dosyalarını “paylaşılan” klasöre kopyalamak ve ardından bir MSI veya `setup.exe` çalıştırmak için.

> [AZURE.IMPORTANT] Batch şu anda, [Azure Storage hesapları hakkında](../storage/storage-create-storage-account.md) bölümünde 5. adım olan [Depolama hesabı oluşturma](../storage/storage-create-storage-account.md#create-a-storage-account) adımında açıklandığı gibi, *sadece* **Genel amaçlı** depolama hesabı türünü destekler. Batch görevleriniz (standart görevler, başlangıç görevleri, iş hazırlama ve iş sürüm görevleri dahil), *yalnızca* **Genel amaçlı** depolama hesaplarında yer alan kaynak dosyalarını belirtmelidir.

Bu, düğümün görevlere atanmak üzere hazır olduğunu düşünmeden önce başlangıç görevinin tamamlanmasını beklemek amacıyla Batch hizmeti için genelde istenen bir durumdur, ancak yapılandırılabilir.

Bir işlem düğümünde başlangıç görevi başarısız olursa, düğümün durumu hatayı yansıtacak şekilde güncelleştirilir ve düğüm, atanacak görevler için kullanılamaz. Bir başlangıç görevi, depolamadan kaynak dosya kopyalamada bir sorun olması ya da komut satırı tarafından yürütülen işlemin, sıfır olmayan bir çıkış kodu döndürmesi durumunda başarısız olabilir.

#### <a name="jobmanagertask"></a>İş yöneticisi görevi

Bir **İş Yöneticisi görev** genellikle iş yürütmenin denetlenmesi ve/veya izlenmesi için kullanılır. Örneğin, bir iş için görevler oluşturma ve gönderme, çalıştırılacak ek görevleri ve işin ne zaman tamamlanacağını belirleme. İş Yöneticisi görevi bu etkinliklerle sınırlı değildir, ancak bu, iş için gereken tüm eylemleri gerçekleştirebilecek tam kapsamlı bir görevdir. Örneğin, bir İş Yöneticisi görevi parametre olarak belirtilen bir dosyayı indirebilir, dosyanın içeriğini çözümleyebilir ve bu içeriğe göre ek görevler gönderebilir.

Bir proje yöneticisi görevi diğer tüm görevlerden önce başlatılır ve aşağıdaki özellikleri sağlar:

- İş oluşturulduğunda, Batch hizmeti tarafından bir görev olarak otomatik olarak gönderilir.

- Bir işte diğer görevlerden önce yürütülecek şekilde zamanlanır.

- Havuzun boyutu küçültülürken bu görevin ilişkili düğümü havuzdan en son kaldırılacak düğümdür.

- Görevin sonlandırılması, işteki tüm görevlerin sonlandırılmasına bağlıdır.

- Yeniden başlatılması gerektiğinde iş yöneticisi görevine en yüksek öncelik verilir. Boş bir düğüm yoksa Batch hizmeti, iş yöneticisi görevinin çalışması için yer açmak amacıyla havuzundaki çalışan diğer görevlerden birini sonlandırabilir.

- Bir işteki iş yöneticisi görevinin, diğer işlerin görevleri üzerinde önceliği yoktur. İşlerde, yalnızca iş düzeyinde öncelikler gözetilir.

#### <a name="jobpreprelease"></a>İş hazırlama ve bırakma görevleri

Batch, iş öncesi yürütme kurulumu için iş hazırlama görevi ve iş sonrası bakım ya da temizleme için iş bırakma görevi sağlar.

- **İş hazırlama görevi**: İş hazırlama görevi, diğer iş görevlerinden herhangi biri yürütülmeden önce, görevleri çalıştırmak için zamanlanan tüm işlem düğümlerinde çalışır. Örneğin, tüm görevler tarafından paylaşılan ancak işe özel olan verileri kopyalamak için iş hazırlama görevini kullanın.
- **İş bırakma görevi**: Bir iş tamamlandığında iş bırakma görevi, havuzun en az bir görev yürütmüş her düğümünde çalıştırılır. Örneğin, iş hazırlama görevi tarafından kopyalanan verileri silmek ya da tanılama günlük verilerini sıkıştırmak veya karşıya yüklemek için iş bırakma görevini kullanın.

Hem iş hazırlama hem de bırakma görevi, görev çağrıldığında çalışacak bir komut satırı belirlemenize olanak tanır ve dosya indirme, yükseltilmiş yürütme, özel ortam değişkenleri, maksimum yürütme süresi, yeniden deneme sayısı ve dosya elde tutma zamanı gibi özellikler sunar.

İş hazırlama ve bırakma görevleri hakkında daha fazla bilgi için bkz. [Azure Batch işlem düğümlerinde iş hazırlama ve tamamlama görevlerini çalıştırma](batch-job-prep-release.md).

#### <a name="multiinstance"></a>Çok örnekli görevler

[Çok örnekli görev](batch-mpi.md) aynı anda birden fazla işlem düğümü üzerinde çalışacak şekilde yapılandırılmış bir görevdir. Çok örnekli görevlerle, bir grup işlem düğümünün tek bir iş yükünü işlemek üzere bir arada ayrılmasını gerektiren, İleti Geçirme Arabirimi (MPI) gibi yüksek performanslı bilgi işlem senaryolarına olanak sağlayabilirsiniz.

Batch .NET kitaplığını kullanarak MPI işlerini Batch’de çalıştırma hakkında ayrıntılı bilgi için bkz. [Azure Batch’de İleti Geçirme Arabirimi (MPI) uygulamalarını çalıştırmak için çok örnekli görevleri kullanma](batch-mpi.md).

#### <a name="taskdep"></a>Görev bağımlılıkları

Adından da anlaşılacağı gibi görev bağımlılıkları, bir görevin yürütülmesinin diğer görevlerin tamamlanmasına bağlı olduğunu belirtmenizi sağlar. Bu özellik “yukarı akış” görevinin çıktısını kullanan bir “aşağı akış” görevi durumları ya da bir yukarı akış görevi, aşağı akış görevi tarafından istenen bazı başlatma işlemlerini gerçekleştirdiğinde destek sağlar. Bu özelliği kullanmak için önce Batch işinizde görev bağımlılıklarını etkinleştirmelisiniz. Ardından, bir başka göreve (ya da birçok başka göreve) bağlı her görev için, görevin bağımlı olduğu görevleri belirtirsiniz.

Görev bağımlılıkları ile aşağıdaki gibi senaryoları yapılandırabilirsiniz:

* *görevB* *görevA*’ya bağlıdır (*görevB* *görevA* tamamlanana kadar yürütülmeye başlamaz)
* *görevC* hem *görevA* hem de *görevB*’ye bağlıdır
* *görevD* yürütülmeden önce bir dizi göreve (örneğin görev *1* ile *10* arası) bağlıdır

[azure-batch-samples][github_samples] GitHub deposunda [TaskDependencies][github_sample_taskdeps] kodu örneğine göz atın. Burada, [Batch .NET][batch_net_api] kitaplığı kullanarak diğer görevlere bağlı görevlerin nasıl yapılandırıldığını görürsünüz.

### <a name="jobschedule"></a>Zamanlanan işler

İş zamanlamaları Batch hizmetinde yinelenen işler oluşturmanızı sağlar. Bir iş zamanlaması işlerin ne zaman çalıştırılacağını belirtir ve çalıştırılacak işlerin özelliklerini içerir. Bir iş zamanlaması, zamanlama süresinin (zamanlamanın ne kadar süreyle ve ne zaman etkin olacağının) ve bu sürede işlerin ne sıklıkta oluşturulacağının belirlenmesine olanak tanır.

### <a name="appkg"></a>Uygulama paketleri

[Uygulama paketleri](batch-application-packages.md) özelliği kolay uygulama yönetimi ve havuzlarınızdaki işlem düğümlerine kolay uygulama dağıtımı sağlar. Uygulama paketlerinde, ikili dosyalar ve destek dosyaları gibi, görevleriniz tarafından çalıştırılan uygulamaların birden fazla sürümünü kolayca karşıya yükleyebilir ve yönetebilirsiniz, sonra bu uygulamalardan bir ya da daha fazlasını havuzunuzdaki işlem düğümlerine otomatik olarak dağıtabilirsiniz.

Batch, uygulama paketlerinizi işlem düğümlerine güvenli şekilde depolamak ve dağıtmak amacıyla arka planda Azure Storage ile çalışma ayrıntılarını işler, böylece hem kodunuz hem de yönetim yükünüz basit hale getirilebilir.

Uygulama paketi özelliği hakkında daha fazla bilgi almak için bkz. [Azure Batch uygulama paketleri ile uygulama dağıtımı](batch-application-packages.md).

## <a name="files"></a>Dosyalar ve dizinler

Her görevin bir çalışma dizini vardır. Görev, bu çalışma dizini altında görev tarafından çalıştırılan programı, işlediği verileri ve görev tarafından gerçekleştirilen işlem çıktısını depolamak için sıfır ya da daha fazla dosya ve dizin oluşturur. Bu dosya ve dizinler daha sonra bir işin çalıştırılması sırasında diğer görevlerin kullanımına sunulur. Bir düğümdeki tüm görevler, dosyalar ve dizinlere tek bir kullanıcı hesabı sahip olur.

Batch hizmeti bir düğümdeki dosya sisteminin bir bölümünü “kök dizin” olarak kullanıma sunar. Kök dizin `%AZ_BATCH_NODE_ROOT_DIR%` ortamı değişkenine erişerek bir görevin kullanımına sunulur. Ortam değişkenlerini kullanma hakkında daha fazla bilgi için bkz. [Görevler için ortam ayarları](#environment).

![İşlem düğümü dizin yapısı][1]

Kök dizin aşağıdaki dizin yapısını içerir:

- **Paylaşılan**: Bu konum, işe bakılmaksızın bir düğümde çalışan tüm görevler için paylaşılan bir dizindir. Düğümde, paylaşılan dizine `%AZ_BATCH_NODE_SHARED_DIR%` aracılığıyla erişilir. Bu dizin, düğümde yürütülen tüm görevler için okuma/yazma erişimi sağlar. Görevler bu dizinde dosya oluşturabilir, okuyabilir, güncelleştirebilir ve silebilir.

- **Başlangıç**: Bu konum, bir başlangıç görevi tarafından çalışma dizini olarak kullanılır. Başlangıç görevini başlatmak üzere Batch hizmeti tarafından indirilen tüm dosyalar da bu dizin altında depolanır. Düğümde, başlangıç dizini, `%AZ_BATCH_NODE_STARTUP_DIR%` ortam değişkeni aracılığıyla kullanıma sunulur. Başlangıç görevi bu dizin altında dosya oluşturabilir, okuyabilir, güncelleştirebilir ve silebilir ve bu dizin, işletim sistemini yapılandırmak üzere başlangıç görevleri tarafından kullanılabilir.

- **Görevler**: Düğümde çalışan her görev için`%AZ_BATCH_TASK_DIR%` aracılığıyla erişilen bir dizin oluşturulur. Her bir görev dizininde Batch hizmeti, bir çalışma dizini (`wd`) oluşturur. Dizinin benzersiz yolu `%AZ_BATCH_TASK_WORKING_DIR%` ortam değişkeni tarafından belirtilir. Bu dizin göreve okuma/yazma erişimi sağlar. Görev bu dizin altında dosya oluşturabilir, okuyabilir, güncelleştirebilir ve silebilir; bu dizin görev tarafından belirtilen *Elde Tutma Süresi* kısıtlaması temelinde korunur.
  - `stdout.txt` ve `stderr.txt`: Bu dosyalar görevin yürütülmesi sırasında görev klasörüne yazılır.

Bir düğüm havuzdan kaldırıldığında, düğümde depolanan dosyaların tümü kaldırılır.

## <a name="lifetime"></a>Havuz ve işlem düğümü ömrü

Azure Batch çözümünüzü tasarlarken havuzların nasıl ve ne zaman oluşturulacağı ve bu havuzlardaki işlem düğümlerinin ne kadar süre kullanımda tutulacağına ilişkin bir tasarım kararı verilmelidir.

Spektrumun bir ucunda, iş gönderildiğinde her iş için bir havuz oluşturulabilir ve bunun düğümleri görevlerin yürütülmesi biter bitmez kaldırılır. Bu, düğümler yalnızca kesinlikle gerekli olduğunda ayrıldığından ve boşta durumuna gelir gelmez kapandığından kullanımı en iyi duruma getirir. Bu da işin, düğümlerin ayrılmasını beklemesi gerektiği anlamına gelmekle birlikte, tek tek kullanılabilir, ayrılmış olmalarının ve başlangıç görevinin tamamlanmasının hemen ardından görevlerin düğümlere zamanlanacağını bilmek önemlidir. Batch, görev atamadan önce havuzdaki tüm düğümlerin kullanılabilir olmasını *beklemez*, böylece kullanılabilir tüm düğümlerden en iyi şekilde faydalanılmasını sağlar.

Spektrumun diğer ucunda, işlerin hemen başlatılması en yüksek önceliğe sahipse işler gönderilmeden önce bir havuz oluşturulabilir ve bu havuzun düğümleri kullanıma sunulabilir. Bu senaryoda, iş görevleri hemen başlayabilir ancak görevlerin atanmasını beklerken düğümler boşta durumda kalmaya devam edebilir.

Genelde, değişken ancak devam eden yükü işlerken kullanılan birleşik yaklaşım, birden çok işin gönderildiği bir havuza sahip olmak ancak, iş yüküne göre düğüm sayısının ölçeğini artırmak ya da azaltmaktır (aşağıdaki *Ölçeklendirme uygulamaları* bölümüne bakın). Bu işlem, mevcut yüke bağlı olarak reaktif ya da yük öngörülebiliyorsa proaktif olarak yapılabilir.

## <a name="scaling"></a>Ölçeklendirme uygulamaları

[Otomatik ölçeklendirme](batch-automatic-scaling.md) kullanarak, Batch hizmetinin, bir havuzdaki işlem düğümleri sayısını mevcut iş yüküne ve işlem senaryonuzun kaynak kullanımına göre dinamik olarak ayarlamasını sağlayabilirsiniz. Bu, yalnızca ihtiyacınız olan kaynakları kullanarak ve ihtiyacınız olmayanları bırakarak uygulamanızı çalıştırmaya ilişkin genel maliyeti düşürmenizi sağlar. Oluşturulduğunda havuz için otomatik ölçeklendirme ayarlarını belirtebilir veya ölçeklendirmeyi daha sonra etkinleştirebilir ve otomatik ölçeklendirme özellikli bir havuzda ölçeklendirme ayarlarını güncelleştirebilirsiniz.

Otomatik ölçeklendirme, bir havuz için **otomatik ölçeklendirme formülü** belirtilerek gerçekleştirilir. Batch hizmeti, sonraki ölçeklendirme aralığı (sizin belirleyebileceğiniz bir aralık) için havuzdaki düğümlerin hedef sayısını belirlemek amacıyla bu formülü kullanır.

Örneğin, yürütme işlemi için zamanlanacak çok sayıda görev göndermeniz gerekebilir. Havuza, beklemedeki görevlerin mevcut sayısı ve bu görevlerin tamamlanma oranı temelinde havuzdaki düğüm sayısını ayarlayan bir ölçeklendirme formülü atayabilirsiniz. Batch hizmeti düzenli aralıklarla formülü değerlendirir ve havuzu, iş yükü ile formül ayarlarınız temelinde yeniden boyutlandırır.

Ölçeklendirme formülü aşağıdaki ölçümleri temel alabilir:

- **Zaman ölçümleri**: Belirtilen saat sayısınca beş dakikada bir toplanan istatistikleri temel alır.

- **Kaynak ölçümleri**: CPU kullanımı, bant genişliği kullanımı, bellek kullanımı ve düğüm sayısını temel alır.

- **Görev ölçümleri**: Etkin, Beklemede ve Tamamlandı gibi görev durumlarını temel alır.

Otomatik ölçeklendirme, bir havuzdaki işlem düğümü sayısını azalttığında çalışmakta olan görevler dikkate alınmalıdır. Buna uyum sağlamak için formülünüz, çalışmakta olan görevlerin hemen durdurulup durdurulmayacağı veya düğüm havuzdan kaldırılmadan önce tamamlanmalarına izin verilip verilmeyeceğini belirten bir düğüm ayırmayı kaldırma ilkesi ayarı içerebilir.

> [AZURE.TIP] İşlem kaynağından en iyi şekilde faydalanabilmek için işin sonundaki düğüm sayısını sıfır olarak ayarlayın ancak çalışan görevlerin bitmesine izin verin.

Bir uygulamayı otomatik olarak ölçeklendirme hakkında daha fazla bilgi için bkz. [Azure Batch havuzunda işlem düğümlerini otomatik olarak ölçeklendirme](batch-automatic-scaling.md).

## <a name="cert"></a>Sertifikalar ile güvenlik

[Azure Storage hesabı][azure_storage] anahtarı gibi, görevler için hassas bilgileri şifrelerken ya da bunların şifrelerini çözerken genelde sertifikalar kullanmanız gerekir. Bunu desteklemek için sertifikalar düğümlere yüklenebilir. Şifrelenmiş parolalar komut satırı parametreleri aracılığıyla düğümlere geçirilir ya da görev kaynaklarından birine eklenir ve yüklü sertifikalar bunların şifrelerini çözmek için kullanılabilir.

Batch hesabına bir sertifika eklemek için [Sertifika ekle][rest_add_cert] işlemini (Batch REST API’si) ya da [CertificateOperations.CreateCertificate][net_create_cert] yöntemini kullanırsınız. Sonra sertifikayı yeni ya da mevcut bir havuzla ilişkilendirebilirsiniz. Sertifika bir havuzla ilişkilendirildiğinde, Batch hizmeti havuzdaki her düğüme sertifikayı yükler. Batch hizmeti, düğüm başlatıldığında, başlangıç ve iş yöneticisi görevleri dahil herhangi bir görevi başlatmadan önce, uygun sertifikaları yükler.

## <a name="scheduling"></a>Zamanlama önceliği

Batch’de oluşturduğunuz işlere öncelik atayabilirsiniz. Batch hizmeti bir hesaptaki iş zamanlama sırasını belirlemek üzere işin öncelik değerini kullanır. Öncelik değeri, -1000 en düşük öncelik ve 1000 en yüksek öncelik olmak üzere, -1000 ile 1000 aralığındadır. [İşin önceliklerini güncelleştirme][rest_update_job] işlemini (Batch REST API’si) ya da [CloudJob.Priority][net_cloudjob_priority] özelliğini (Batch .NET API’si) değiştirerek bir işin önceliğini güncelleştirebilirsiniz.

Aynı hesapta, yüksek öncelikli işlerin düşük öncelikli işlere göre zamanlama üstünlüğü vardır. Bir hesaptaki yüksek öncelik değerine sahip iş farklı bir hesaptaki düşük öncelik değerine sahip başka bir işe karşı zamanlama üstünlüğüne sahip değildir.

Havuzlardaki iş zamanlaması bağımsızdır. Farklı havuzlar arasında, ilişkili havuzunda boşta düğüm eksikliği olması durumunda yüksek öncelikli işin önce zamanlanacağı kesin değildir. Aynı havuzda, aynı öncelik değerine sahip işlerin zamanlanma şansı eşittir.

## <a name="environment"></a>Görevler için ortam ayarları

Bir Batch işinde yürütülen her görevin, Batch hizmeti (sistem tarafından tanımlanan, aşağıdaki tabloya bakın) ve ayrıca kullanıcı tanımlı ortam değişkenleri tarafından ayarlanan ortam değişkenlerine erişimi vardır. İşlem düğümlerindeki görevler tarafından çalıştırılan uygulamalar ve betikler, düğümde yürütme sırasında bu ortam değişkenlerine erişime sahiptir.

Bir işe görev eklerken, [Bir işe görev ekleme][rest_add_task] işlemini (Batch REST API’si) kullanarak ya da [CloudTask.EnvironmentSettings][net_cloudtask_env] özelliğini (Batch .NET API’si) değiştirerek kullanıcı tanımlı ortam değişkenlerini ayarlayın.

[Bir görev hakkında bilgi alma][rest_get_task_info] işlemini (Batch REST API’si) kullanarak ya da [CloudTask.EnvironmentSettings][net_cloudtask_env] özelliğine (Batch .NET API’si) erişerek bir görevin ortam değişkenlerini (sistem tarafından tanımlanan ve kullanıcı tanımlı) alın. Belirtildiği gibi, bir hesaplama düğümünde yürütülen işlemler de tüm ortam değişkenlerine erişebilir, örneğin bilinen `%VARIABLE_NAME%` söz dizimini kullanarak.

Bir işte zamanlanan her görev için, sistem tarafından tanımlanan aşağıdaki ortam değişkenleri grubu Batch hizmeti tarafından ayarlanır:

| Ortam Değişkeni Adı       | Açıklama                                                              |
|---------------------------------|--------------------------------------------------------------------------|
| `AZ_BATCH_ACCOUNT_NAME`         | Görevin ait olduğu hesabın adı.                       |
| `AZ_BATCH_JOB_ID`               | Görevin ait olduğu işin kimliği.                             |
| `AZ_BATCH_JOB_PREP_DIR`         | Düğümdeki iş hazırlama görevi dizininin tam yolu.         |
| `AZ_BATCH_JOB_PREP_WORKING_DIR` | Düğümdeki iş hazırlama görevi çalışma dizininin tam yolu. |
| `AZ_BATCH_NODE_ID`              | Görevin çalıştığı düğümün kimliği.                         |
| `AZ_BATCH_NODE_ROOT_DIR`        | Düğümdeki kök dizinin tam yolu.                         |
| `AZ_BATCH_NODE_SHARED_DIR`      | Düğümdeki paylaşılan dizinin tam yolu.                       |
| `AZ_BATCH_NODE_STARTUP_DIR`     | Düğümdeki işlem düğümü başlangıç görevi dizininin tam yolu.    |
| `AZ_BATCH_POOL_ID`              | Görevin çalıştığı havuzun kimliği.                         |
| `AZ_BATCH_TASK_DIR`             | Düğümdeki görev dizininin tam yolu.                         |
| `AZ_BATCH_TASK_ID`              | Geçerli görevin kimliği.                                              |
| `AZ_BATCH_TASK_WORKING_DIR`     | Düğümdeki görev çalışma dizininin tam yolu.                 |

>[AZURE.NOTE] Sistem tarafından tanımlanan yukarıdaki değişkenlerin üzerine yazamazsınız; bunlar salt okunurdur.

## <a name="errorhandling"></a>Hata işleme

Batch çözümündeki görev ve uygulama hatalarını işlemeyi gerekli görebilirsiniz.

### Görev hatası işleme
Görev hataları kategorileri şunlardır:

- **Zamanlama hataları**
    - Bir görev için belirtilen dosya aktarımı herhangi bir nedenle başarısız olursa, görev için "zamanlama hatası" ayarlanır.
    - Zamanlama hatalarının nedenleri; dosyaların taşınmış olması, Storage hesabının artık kullanılmaması veya dosyaların düğüme başarıyla kopyalanmasını önleyen bir sorunla karşılaşılması olabilir.
- **Uygulama hataları**
    - Görevin komut satırı tarafından belirtilen işlem de başarısız olabilir. Görev tarafından yürütülen işlem sıfır olmayan çıkış kodu döndürdüğünde işlemin başarısız olduğu kabul edilir.
    - Uygulama hataları için Batch’in, görevi belirlenen sayıya kadar otomatik olarak yeniden deneyecek şekilde yapılandırılması mümkündür.
- **Kısıtlama hataları**
    - Bir iş ya da görev için maksimum yürütme süresini belirleyen (*maxWallClockTime*) bir kısıtlama ayarlanabilir. Bu, “askıdaki” görevlerin sonlandırılması için faydalı olabilir.
    - Maksimum süre aşıldığında görev *tamamlandı*olarak işaretlenir ancak çıkış kodu `0xC000013A` olarak ayarlanır ve *schedulingError* alanı `{ category:"ServerError", code="TaskEnded"}` olarak işaretlenir.

### Uygulama hatalarını ayıklama

Yürütme sırasında bir uygulama, sorunları gidermek için kullanılabilecek tanılama çıktıları üretebilir. Yukarıdaki [Dosyalar ve dizinler](#files) bölümünde belirtildiği gibi, Batch hizmeti işlem düğümündeki görev dizininde yer alan `stdout.txt` ve `stderr.txt` dosyalarına stdout ve stderr çıktısı gönderir. Batch .NET API’sindeki [ComputeNode.GetNodeFile][net_getfile_node] ve [CloudTask.GetNodeFile][net_getfile_task] yöntemini kullanarak sorun giderme amacıyla bu ve diğer dosyaları alabilirsiniz.

*Uzak Masaüstü* kullanarak işlem düğümünde oturum açma yoluyla daha da geniş çaplı bir hata ayıklama gerçekleştirilebilir. Uzaktan oturum açma için [bir düğümden uzak masaüstü protokol dosyasını alabilir][rest_rdp] (Batch REST API’si) ya da [ComputeNode.GetRDPFile][net_rdp] yöntemini (Batch .NET API’si) kullanabilirsiniz.

>[AZURE.NOTE] RDP aracılığıyla bir düğüme bağlanmak için düğümde bir kullanıcı oluşturmanız gerekir. Batch REST API’sinde [Bir düğüme kullanıcı hesabı ekleme][rest_create_user] ya da Batch .NET’te [ComputeNode.CreateComputeNodeUser][net_create_user] yöntemini kullanın.

### Görev hataları veya kesintilerinin sebepleri

Görevler zaman zaman başarısız olabilir veya kesintiye uğrayabilir. Görevin kendisi başarısız olabilir, görevin üzerinde çalıştığı düğüm yeniden başlatılabilir ya da havuzun ayırmayı kaldırma ilkesi görevin bitmesini beklemeden düğümleri kaldırmak üzere ayarlandıysa, yeniden boyutlandırma işlemi sırasında düğüm havuzdan kaldırılabilir. Her durumda, görev başka bir düğümde yürütülmek üzere Batch tarafından otomatik olarak yeniden kuyruğa alınabilir.

Aralıklı bir sorunun görevin askıda kalmasına ya da yürütülmesinin uzun sürmesine neden olması mümkündür. Bir görev için maksimum yürüte süresi ayarlanabilir ve bu süre aşılırsa Batch, görev uygulamasını kesintiye uğratır.

### "Bozuk" işlem düğümleri sorunlarını giderme

Bazı görevlerinizin başarısız olduğu durumlarda, Batch istemci uygulamanız ya da hizmetiniz, hatalı davranan düğümü tanımlamak üzere başarısız görevlerin meta verilerini inceleyebilir. Bir havuzdaki her düğüme benzersiz bir kimlik verilir ve bir görevin çalıştığı düğüm görev meta verilerine eklenir. Tanımlama işleminden sonra çeşitli eylemleri gerçekleştirebilirsiniz:

- **Düğümü yeniden başlatma** ([REST][rest_reboot] | [.NET][net_reboot])

    Düğümü yeniden başlatmak bazen takılan veya çöken işlemler gibi görünmeyen sorunları temizleyebilir. Havuzunuz bir başlangıç görevi kullanıyorsa ya da işiniz iş hazırlama görevi kullanıyorsa, düğüm başlatıldığında bunlar yürütülür.

- **Düğümün görüntüsünü yeniden oluşturma** ([REST][rest_reimage] | [.NET][net_reimage])

    Bu işlem, işletim sistemini düğüme yeniden yükler. Düğümün yeniden başlatılmasıyla düğümün görüntüsünün yeniden oluşturulmasının ardından başlangıç görevleri ve iş hazırlama görevleri yeniden çalıştırılır.

- **Düğümü havuzdan kaldırma** ([REST][rest_remove] | [.NET][net_remove])

    Bazen düğümün havuzdan tamamen kaldırılması gereklidir.

- **Düğümde görev zamanlamayı devre dışı bırakma** ([REST][rest_offline] | [.NET][net_offline])

    Bu, başka görev atanmayacak şekilde düğümü “çevrimdışı” durumuna alır, ancak düğümün çalışır durumda ve havuzda kalmasına izin verir. Bu, başarısız olan görevin verilerini kaybetmeden ve düğüm, başka görev hatalarına neden olmadan hatalara ilişkin ayrıntılı araştırma gerçekleştirmenizi sağlar. Örneğin düğümde görev zamanlamayı devre dışı bırakabilir, sonra düğümün olay günlüklerini uzaktan inceleyebilir ya da başka sorun giderme işlemleri uygulayabilirsiniz. Araştırmanızı bitirdiğinizde görev zamanlamayı tekrar çevrimiçi olarak etkinleştirebilir ([REST][rest_online], [.NET][net_online]) ya da yukarıda belirtilen diğer eylemlerden birini gerçekleştirebilirsiniz.

> [AZURE.IMPORTANT] Yukarıdaki her bir eylemde (yeniden başlatma, yeniden görüntüleme, görev zamanlamayı kaldırma, devre dışı bırakma), eylemi gerçekleştirdiğinizde düğümde çalışmakta olan görevlerin nasıl işleneceğini belirtebilirsiniz. Örneğin, Batch .NET istemci kitaplığında görev zamanlamayı devre dışı bıraktığınızda, çalışan görevleri **Sonlandırma**, diğer düğümlerde zamanlama için **Yeniden kuyruğa alma** ya da eylemi gerçekleştirmeden önce çalışan görevlerin tamamlanmasına izin vermeyi (**TaskCompletion**) belirtmek üzere [DisableComputeNodeSchedulingOption][net_offline_option] listeleme değeri belirleyebilirsiniz.

## Sonraki adımlar

- [.NET için Azure Batch Kitaplığını kullanmaya başlama](batch-dotnet-get-started.md) adımlarını izleyerek ilk Batch uygulamanızı oluşturma
- Batch çözümlerinizi geliştirirken kullanmak üzere [Batch Explorer][batch_explorer_project] örnek projesini indirin ve derleyin. Batch Explorer’ı kullanarak, aşağıdakileri ve daha fazlasını gerçekleştirebilirsiniz:
  - Batch hesabınızdaki havuzlar, işler ve görevleri izleme ve düzenleme
  - Düğümlerden `stdout.txt`, `stderr.txt` ve diğer dosyaları indirme
  - Düğümlerde kullanıcı oluşturma ve uzaktan oturum açma için RDP dosyalarını indirme

[1]: ./media/batch-api-basics/node-folder-structure.png

[about_cloud_services]: ../cloud-services/cloud-services-choose-me.md
[azure_storage]: https://azure.microsoft.com/services/storage/
[batch_explorer_project]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[cloud_service_sizes]: ../cloud-services/cloud-services-sizes-specs.md
[msmpi]: https://msdn.microsoft.com/library/bb524831.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_sample_taskdeps]:  https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies

[batch_net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_cloudjob_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobmanagertask.aspx
[net_cloudjob_priority]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.priority.aspx
[net_cloudpool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_cloudtask_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.environmentsettings.aspx
[net_create_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.createcertificate.aspx
[net_create_user]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.createcomputenodeuser.aspx
[net_getfile_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getnodefile.aspx
[net_getfile_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.getnodefile.aspx
[net_multiinstancesettings]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_rdp]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getrdpfile.aspx
[net_reboot]: https://msdn.microsoft.com/library/azure/mt631495.aspx
[net_reimage]: https://msdn.microsoft.com/library/azure/mt631496.aspx
[net_remove]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.removefrompoolasync.aspx
[net_offline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.disableschedulingasync.aspx
[net_online]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.enableschedulingasync.aspx
[net_offline_option]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.disablecomputenodeschedulingoption.aspx

[batch_rest_api]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[rest_add_job]: https://msdn.microsoft.com/library/azure/mt282178.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_cert]: https://msdn.microsoft.com/library/azure/dn820169.aspx
[rest_add_task]: https://msdn.microsoft.com/library/azure/dn820105.aspx
[rest_create_user]: https://msdn.microsoft.com/library/azure/dn820137.aspx
[rest_get_task_info]: https://msdn.microsoft.com/library/azure/dn820133.aspx
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx
[rest_multiinstancesettings]: https://msdn.microsoft.com/library/azure/dn820105.aspx#multiInstanceSettings
[rest_update_job]: https://msdn.microsoft.com/library/azure/dn820162.aspx
[rest_rdp]: https://msdn.microsoft.com/library/azure/dn820120.aspx
[rest_reboot]: https://msdn.microsoft.com/library/azure/dn820171.aspx
[rest_reimage]: https://msdn.microsoft.com/library/azure/dn820157.aspx
[rest_remove]: https://msdn.microsoft.com/library/azure/dn820194.aspx
[rest_offline]: https://msdn.microsoft.com/library/azure/mt637904.aspx
[rest_online]: https://msdn.microsoft.com/library/azure/mt637907.aspx



<!----HONumber=Jun16_HO2-->


