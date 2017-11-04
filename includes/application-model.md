# <a name="compute"></a>İşlem
Azure, dağıtmanıza ve bir Microsoft Veri Merkezi içinde çalışan uygulama kodunuz izlemenize olanak sağlar. Bir uygulama oluşturduğunuzda ve Azure üzerinde çalıştırmak, kod ve yapılandırma birlikte adlı bir Azure hizmeti barındırılan. Barındırılan hizmetler, yönetme, yukarı ve aşağı doğru ölçeklendirme, yeniden yapılandırın ve uygulamanızın kodu yeni sürümlerle güncelleştirmek kolaydır. Bu makalede Azure üzerinde barındırılan odaklanır hizmet uygulaması modeli.<a id="compare" name="compare"></a>

## İçindekiler tablosu<a id="_GoBack" name="_GoBack"></a>
* [Azure uygulama modeli avantajları][Azure Application Model Benefits]
* [Barındırılan hizmet temel kavramlar][Hosted Service Core Concepts]
* [Barındırılan hizmet tasarım konuları][Hosted Service Design Considerations]
* [Ölçek için uygulamanızı tasarlama][Designing your Application for Scale]
* [Barındırılan Hizmet tanım ve yapılandırma][Hosted Service Definition and Configuration]
* [Hizmet tanımı dosyası][The Service Definition File]
* [Hizmet yapılandırma dosyası][The Service Configuration File]
* [Oluşturma ve bir barındırılan hizmet dağıtma][Creating and Deploying a Hosted Service]
* [Başvuruları][References]

## <a id="benefits"></a>Azure uygulama modeli avantajları
Uygulamanızı bir barındırılan hizmet olarak dağıttığınızda, Azure bir veya daha fazla uygulama kodu içeren makineleri (VM'ler) oluşturur ve sanal makineleri bir Azure veri merkezlerinde bulunan fiziksel makinelerde önyüklenir. İstemci isteklerini barındırılan uygulamanız için veri merkezi girerken, bir yük dengeleyici bu istekleri VM'ler için eşit olarak dağıtır. Uygulamanızı Azure'da barındırılan olsa da, üç avantajları alır:

* **Yüksek kullanılabilirlik.** Yüksek kullanılabilirlik Azure uygulamanızı mümkün olduğunca çalıştıran istemci isteklerine yanıt verip olmasını sağlar ve anlamına gelir. (İşlenmeyen bir özel durum nedeniyle, örneğin), uygulamanızın sonlandırır sonra Azure bunu algılar ve otomatik olarak ayarlanır, uygulamanızı yeniden başlatın. Makine uygulamanızı donanım arızası çeşit deneyimleri üzerinde çalışıyorsa, ardından Azure de bu algılamak ve otomatik olarak yeni bir VM başka bir çalışma fiziksel makine oluşturur ve buradan kodunuzu çalıştırmak. Not: Microsoft'un hizmet düzeyi sözleşmesi kullanılabilir 99,95 oranında almak, uygulamanız için sırada uygulama kodu çalıştıran en az iki VM olması gerekir. Bu, Azure, kodunuzu yeni, iyi bir VM için başarısız bir VM'den taşırken istemci isteklerini işlemek bir VM sağlar.
* **Ölçeklenebilirlik.** Azure kolayca sağlar ve uygulamanızda yerleştirilen gerçek yükü işlemek üzere uygulama kodunuz çalışan VM sayısı dinamik olarak değiştirin. Bu, müşterilerinizin üzerinde gereksinim duyduğunuzda ihtiyacınız VM'ler için ödeme sırasında yerleştirdiğinizi iş yükü uygulamanıza ayarlamanıza olanak sağlar. VM sayısını değiştirmek istediğinizde, Azure dinamik olarak değiştirmek istediğiniz gibi sık olarak çalışan VM sayısı edinerek dakika içinde yanıt verir.
* **Yönetilebilirlik.** Azure platformu bir hizmet olarak platform'dur (PaaS) olduğundan, bu makineleri çalışır durumda tutmak için gerekli altyapının (donanım kendisi, elektrik ve ağ) yönetir. Azure, ayrıca tüm doğru düzeltme ekleri ve güvenlik güncelleştirmeleri, hem de .NET Framework ve Internet Information Server gibi bileşen güncelleştirmeleri ile güncel bir işletim sistemi sağlama platform yönetir. Windows Server 2008 çalıştıran tüm sanal makineleri olduğundan Azure Tanılama izleme, Uzak Masaüstü desteği, güvenlik duvarları ve sertifika deposu yapılandırma gibi ek özellikler sağlar. Bu özelliklerin tümü olarak sağlanan ekstra maliyet. Aslında, Azure'da uygulamanızı çalıştırdığınızda, Windows Server 2008 işletim sistemi (OS) lisans dahil edilir. Windows Server 2008 çalıştıran tüm sanal makineleri olduğundan, Windows Server 2008 üzerinde çalışan herhangi bir kod Azure içinde çalışırken düzgün çalışır.

## <a id="concepts"></a>Barındırılan hizmeti temel kavramlar
Uygulamanızı azure'da barındırılan bir hizmet olarak dağıtıldığında, bir veya daha fazla olarak çalıştırdığı *rolleri.* A *rol* yalnızca uygulama dosyalarını ve yapılandırmasını gösterir. Uygulamanız için her uygulama dosyaları ve yapılandırması, kendi kümesiyle bir veya daha fazla rolü tanımlayabilirsiniz. Uygulamanızdaki her rol için VM sayısını belirtebilirsiniz veya *rol örnekleri*, çalıştırmak için. Aşağıdaki şekilde rolleri ve rol örnekleri kullanan bir barındırılan hizmet olarak Modellenen uygulamanın iki basit örnekleri göster.

##### <a name="figure-1-a-single-role-with-three-instances-vms-running-in-an-azure-data-center"></a>Şekil 1: Tek bir rol çalıştıran bir Azure veri merkezinde üç örnekleri (VM'ler) ile
![Görüntü][0]

##### <a name="figure-2-two-roles-each-with-two-instances-vms-running-in-an-azure-data-center"></a>Şekil 2: İki roller, bir Azure veri merkezinde çalışan her iki örnekli (VM'ler)
![Görüntü][1]

Rol örnekleri genellikle ne adlı aracılığıyla veri merkezi girme Internet istemcisi isteklerini işleme bir *giriş uç noktası*. Tek bir rol, 0 veya daha fazla giriş uç noktaları olabilir. Her uç nokta bir protokol (HTTP, HTTPS veya TCP) ve bir bağlantı noktasını belirtir. İki giriş uç noktaları için bir rolünü yapılandırmak için ortaktır: HTTP bağlantı noktası 80 ve 443 numaralı bağlantı noktasını dinlemeye HTTPS dinleme. Aşağıdaki şekilde, bunları istemci isteklerine yönlendiren farklı giriş uç noktaları ile iki farklı rol örneği gösterilmektedir.

![Görüntü][2]

Azure üzerinde barındırılan bir hizmet oluşturduğunuzda, istemcilerin erişmek için kullandığı genel olarak adreslenebilir bir IP adresi atanır. Barındırılan hizmet oluşturma sırasında bu IP adresine eşlenmiş bir URL öneki de seçmeniz gerekir. Temelde ayırma gibi bu öneki benzersiz olmalıdır *önek*. başka bir bulunabilir şekilde cloudapp.net URL. İstemciler, URL'yi kullanarak rolü örneklerinizi ile iletişim kurar. Genellikle, değil dağıtmak veya kaldıracak Azure yayımlama *önek*. cloudapp.net URL. Bunun yerine, bir DNS adı seçim, DNS kayıt şirketinden satın alma ve Azure URL'sine istemci isteklerini yeniden yönlendirmek için DNS adını yapılandırın. Daha fazla ayrıntı için bkz: [Azure'da bir özel etki alanı adı yapılandırma][Configuring a Custom Domain Name in Azure].

## <a id="considerations"></a>Barındırılan hizmet tasarım konuları
Bulut ortamında çalıştırmak için bir uygulama tasarlarken, gecikme, yüksek kullanılabilirlik ve ölçeklenebilirlik gibi dikkat etmeniz gereken bazı noktalar vardır.

Uygulama kodunuz bulunacağı yere karar verme önemli bir barındırılan hizmet Azure'da çalışırken konudur. Gecikme süresini azaltmak ve olası en iyi performansı elde için müşterilerinize en yakın olan veri merkezlerinde uygulamanızı dağıtmak için yaygın bir durumdur.
Ancak, bazı verileriniz hakkında yasal veya yasal sorunları ve şu bulunduğu varsa, şirketinizin yakın veya verilerinize daha yakın bir veri merkezi seçebilirsiniz. Uygulama kodunuz barındırma özellikli dünya altı veri merkezleri vardır. Aşağıdaki tabloda kullanılabilir konumlarını gösterilmektedir:

<table border="2" cellspacing="0" cellpadding="5" style="border: 2px solid #000000;">

<tbody>

<tr>

<td style="width: 100px;">
**Ülke/bölge**

</td>

<td style="width: 200px;">
**Alt bölgeleri**

</td>
</tr>

<tr>

<td>
Amerika Birleşik Devletleri

</td>

<td>
Güney Orta & Kuzey Orta

</td>
</tr>

<tr>

<td>
Avrupa

</td>

<td>
Kuzey ve Batı

</td>
</tr>

<tr>

<td>
Asya

</td>

<td>
Güneydoğu & Doğu

</td>
</tr>
</tbody>
</table>
Barındırılan hizmet oluştururken, kodunuzu çalıştırmak istediğiniz konumu belirten bir alt bölge seçin.

Yüksek kullanılabilirlik ve ölçeklenebilirlik elde etmek için uygulamanızın veri merkezi bir depoya birden çok rol örneklerine erişilebilir tutulması oldukça önemlidir. Bu konuda yardımcı olmak için Azure BLOB'ları, tabloları ve SQL veritabanı gibi çeşitli depolama seçenekleri sunar. Lütfen bakın [azure'da veri depolama teklifleri] [ Data Storage Offerings in Azure] bu depolama teknolojileri hakkında daha fazla bilgi için makalenin. Aşağıdaki şekilde, Azure veri merkezi içinde yük dengeleyici istemci isteklerini tümü aynı veri depolama erişimi farklı rol örneklerine nasıl dağıtır gösterilmektedir.

![Görüntü][3]

Genellikle, bu düşük gecikme süresi (daha iyi performans) için izin verdiği ölçüde uygulama kodunuz ve verileriniz aynı veri merkezinde bulmak istediğiniz uygulama kodunuz, verileri eriştiği. Veri içinde aynı veri merkezinde taşındığında Ayrıca, bant genişliği için sizden ücret istenmese.

## <a id="scale"></a>Ölçek için uygulamanızı tasarlama
Bazı durumlarda, tek bir uygulama (gibi basit bir web sitesi) alır ve Azure üzerinde barındırılan sağlamak isteyebilirsiniz. Ancak tüm birlikte çalıştığını uygulamanızı birkaç rolleri sık oluşabilir. Örneğin, aşağıdaki şekilde vardır iki Web Sitesi Rol örnekleri, sipariş işleme rol üç örneklerini ve Rapor Oluşturucu rol bir örneği. Bu rolleri tüm birlikte çalışıyorsanız ve bunların tümünün kodunu bir arada paketlenmiş ve Azure kadar tek bir birim olarak dağıtılabilir.

![Görüntü][4]

Her rol örnekleri (VM'ler) kendi kümesi üzerinde çalışan bir uygulamaya farklı roller bölmek için ana nedeni, rollerden bağımsız olarak ölçeklendirebilirsiniz olmasıdır. Sipariş işleme rolünü çalıştıran rol örneği sayısı yanı sıra, Web sitesi rolü çalıştıran rol örneklerinin sayısını artırmak isteyebilirsiniz örneğin, tatil sezonu sırasında birçok müşteri ürünler, şirketten satın. Tatilde sonra Web sitesi örnekleri ancak daha az sipariş işleme örnek çok hala gerekebilir şekilde döndürdü, ürünlerin çok elde edebilirsiniz. Yılın rest sırasında birkaç Web sitesi ve sipariş işleme örnekleri yalnızca gerekebilir. Tüm bu yalnızca bir rapor oluşturucu örneği gerekebilir. Azure rol tabanlı dağıtımlarda esnekliğini iş gereksinimlerinizi uygulamanıza kolayca uyum sağlar.

Barındırılan hizmet içindeki örnekleri birbirleriyle iletişim rolüne sahip yaygındır. Örneğin, bir müşterinin siparişi Web sitesi rolü kabul ancak sipariş işleme rol örneklerine işlem sırası boşaltır. Başka bir örnek kümesini rol örneklerine bir dizi Azure tarafından sağlanan queuing teknolojisi kullanarak iş formunu geçirmek için en iyi yolu sıra hizmet veya hizmet veri yolu kuyrukları. Bir kuyruk kullanımını buraya yazıdaki kritik bir parçasıdır. Sıranın bağımsız olarak Maliyet karşı iş yükünü dengelemek sağlayarak rollerini ölçeklendirmek barındırılan hizmet sağlar. Sonra sıradaki iletilerin sayısını zaman içinde yükseliyorsa, sipariş işleme rol örneklerinin sayısını ölçeklendirebilirsiniz. Zaman içinde sırasındaki ileti sayısını azaltır, sipariş işleme rol örneklerinin sayısını ölçekleme yapabilirsiniz. Bu şekilde, gerçek iş yükünü işlemek için gerekli örnekleri için yalnızca ödeme yapıyorsanız.

Sıranın güvenilirlik de sağlar. Sipariş işleme rol örneklerinin sayısını ölçekleme sırasında Azure sonlandırmak için hangi örnekleri karar verir. Bir kuyruk iletisi işleme ortasında olan bir örneği sonlandırmaya karar verebilirsiniz. Ancak, ileti işleme başarıyla tamamlanmazsa olduğundan ileti, alır ve işler yeniden başka bir sipariş işleme rol örneği için görünür hale gelir. Kuyruk iletisi görünürlük nedeniyle iletileri garanti sonunda işlenir. Sıranın de iletileri iletileri istekte tüm rol örnekleri için etkili bir şekilde dağıtarak yük dengeleyici olarak işlev görür.

Web Sitesi Rol örnekleri için bunlara gelen trafik izleyebilir ve bunları sayısını ölçeklendirmek karar yukarı veya aşağı de. Sıra, sipariş işleme rol örnekleri bağımsız olarak Web Sitesi Rol örnekleri sayısını ölçeklendirmek sağlar. Bu çok güçlü ve bir büyük esneklik sağlar. Elbette, uygulamanızın ek roller oluşuyorsa, avantajları maliyet ve aynı ölçeklendirme yararlanın için aralarında iletişim kurmak için conduit olarak ek sıraları ekleyebilirsiniz.

## <a id="defandcfg"></a>Barındırılan hizmet tanımı ve yapılandırma
Azure için barındırılan hizmet dağıtma Hizmet tanım dosyası ve bir hizmet yapılandırma dosyası da olmasını gerektirir. Bu dosyaların her ikisini de XML dosyalarıdır ve bildirimli olarak barındırılan hizmet için dağıtım seçeneklerini belirtmenizi sağlar. Hizmet tanımı dosyası barındırılan hizmet ve bunların nasıl iletişim kuracağını oluşturan rollerinin tümünü açıklar. Hizmet yapılandırma dosyası, her bir rolü ve her rol örneği yapılandırmak için kullanılan ayarları için örnek sayısını tanımlar.

## <a id="def"></a>Hizmet tanımı dosyası
Daha önce hizmet tanımı (CSDEF) belirtildiği gibi dosyası tam uygulamayı oluşturan çeşitli rolleri tanımlayan bir XML dosyasıdır. XML dosyası için tam şeması şurada bulunabilir: [http://msdn.microsoft.com/library/windowsazure/ee758711.aspx] ['].
CSDEF dosya uygulamanızda istediğiniz her rol için WebRole veya örn bir öğe içeriyor. Bir rolü (WebRole öğesi kullanarak) bir web rolü olarak dağıtma kod Windows Server 2008 ve Internet Information Server (IIS) içeren bir rol örneği üzerinde çalışacağı anlamına gelir.
Bir rol (örn öğesi kullanarak) bir çalışan rolü rol örneği Windows Server 2008 üzerinde olacağı anlamına gelir olarak dağıtma (IIS yüklenmez).

Kesinlikle oluşturma ve gelen web isteklerini dinlemek için başka bir mekanizma kullanan çalışan rolü dağıtma (örneğin, kodunuzu oluşturma ve .NET HttpListener kullanın). Rol örnekleri tüm Windows Server 2008 çalıştıran olduğundan, normal Windows sunucusu üzerinde çalışan bir uygulama tarafından kullanılabilir herhangi bir işlem kodunuzu gerçekleştirebilirsiniz
2008.

Her rol için bu rolün örneğini kullanması gereken istenen VM boyutu belirtin. Aşağıdaki tabloda, günümüzün çeşitli VM boyutları ve her özniteliklerini gösterilmektedir:

<table border="2" cellspacing="0" cellpadding="5" style="border: 2px solid #000000;">

<tbody>

<tr align="left" valign="top">

<td>
**VM boyutu**

</td>

<td>
**CPU**

</td>

<td>
**RAM**

</td>

<td>
**Disk**

</td>

<td>
**En yüksek ağ g/ç**

</td>
</tr>

<tr align="left" valign="top">

<td>
**Ek çok küçük**

</td>

<td>
1 x 1.0 GHz

</td>

<td>
768 MB

</td>

<td>
20GB

</td>

<td>
\~5 MB/sn

</td>
</tr>

<tr align="left" valign="top">

<td>
**Küçük**

</td>

<td>
1 x 1.6 GHz

</td>

<td>
1,75 GB

</td>

<td>
225GB

</td>

<td>
\~100 MB/sn

</td>
</tr>

<tr align="left" valign="top">

<td>
**Orta**

</td>

<td>
2 x 1.6 GHz

</td>

<td>
3,5 GB

</td>

<td>
490GB

</td>

<td>
\~200 MB/sn

</td>
</tr>

<tr align="left" valign="top">

<td>
**Büyük**

</td>

<td>
4 x 1.6 GHz

</td>

<td>
7 GB

</td>

<td>
1TB

</td>

<td>
\~400 MB/sn

</td>
</tr>

<tr align="left" valign="top">

<td>
**Çok büyük**

</td>

<td>
8 x 1.6 GHz

</td>

<td>
14 GB

</td>

<td>
2TB

</td>

<td>
\~800 MB/sn

</td>
</tr>
</tbody>
</table>
Saatlik bir rol örneği olarak kullanın ve rolü örneklerinizi veri merkezi dışında göndermek için herhangi bir veri ücretlendirilen her VM için ücretlendirilirsiniz. Veri Merkezi girme veri için ücret değil. [Azure fiyatlandırması] [Azure fiyatlandırması] daha fazla bilgi için bkz. Genel olarak, uygulamanızın arızalarına karşı daha esnek olmasını sağlamak birkaç büyük örnekleri aksine çok sayıda küçük rol örnekleri kullanmanız önerilir. Tüm daha az sayıda rol örneği olması, fazla felaket niteliğinde bir tanesine hatası bir genel uygulamanıza. Ayrıca, önceden belirtildiği gibi Microsoft sağlar %99,95 hizmet düzeyi sözleşmesi alabilmek için her rolün en az iki örnek dağıtmalısınız.

Hizmet tanımı (CSDEF) de burada uygulamanızda birçok öznitelikler her rol hakkında belirtirsiniz dosyasıdır. Burada daha kullanışlı öğelerin bazıları için kullanılabilir:

* **Sertifikaları**. Veri ya da web hizmetini SSL destekleyip desteklemediğini şifrelemek için sertifikalar kullanır. Azure için yüklenecek herhangi bir sertifika gerekir. Daha fazla bilgi için bkz: [Azure sertifikaları yönetme][Managing Certificates in Azure]. Bu XML ayarı, daha önce yüklenen sertifika rol örneğinin sertifika deposuna yüklenir, böylece uygulama kodu tarafından kullanılabilir.
* **Yapılandırma ayarı adları**. Rol örneği üzerinde çalıştığı sırada okumak için uygulamaları istediğiniz değerleri. Yapılandırma ayarları gerçek değeri kodunuzu yeniden dağıtmak gerek kalmadan herhangi bir zamanda güncelleştirilebilir hizmet yapılandırma (CSCFG) dosyası olarak ayarlanır. Aslında, uygulamalarınızın kapalı kalma süresi yansıtılmasını olmadan değiştirilen yapılandırma değerlerini algılamak için şekilde kodlayabilirsiniz.
* **Giriş uç noktaları**. Dış dünya kullanıma sunmak istediğiniz tüm HTTP, HTTPS veya TCP uç (ile bağlantı noktaları) burada belirtin, *önek*. cloadapp.net URL. Azure rolünüze dağıtırken, güvenlik duvarı rol örneği üzerinde otomatik olarak yapılandırır.
* **İç uç noktalar**. Burada, uygulamanızın bir parçası dağıtılan diğer rol örneklerine maruz istediğiniz HTTP veya TCP uç nokta belirtin. İç uç noktalar birbirleriyle iletişim kurmalarını uygulamanıza içindeki tüm rol örnekleri ver, ancak uygulamanızı dışında olan tüm rol örnekleri için erişilebilir değil.
* **Modülleri içe**. Bu isteğe bağlı olarak, rol örneklerinde yararlı bileşenlerini yükleyin. Tanılama izleme, Uzak Masaüstü ve Azure (güvenli bir kanal şirket içi kaynaklara erişmek, rol örneği sağlayan) bağlanmak için bileşenleri yok.
* **Yerel depolama**. Bu rol örneğinde kullanmak, uygulamanız için bir alt ayırır. Daha ayrıntılı olarak açıklanmıştır [azure'da veri depolama teklifleri] [ Data Storage Offerings in Azure] makalesi.
* **Başlangıç görevi**. Başlangıç görevi yukarı önyükleme gibi bir rol örneğinde Önkoşul bileşenlerini yüklemek için bir yol sağlar. Görevler gerekirse yönetici olarak yükseltilmiş çalıştırabilirsiniz.

## <a id="cfg"></a>Hizmet yapılandırma dosyası
Hizmet yapılandırma (CSCFG) dosyası, uygulamanızın yeniden dağıtmadan değiştirilebilir ayarları tanımlayan bir XML dosyasıdır. XML dosyası için tam şeması şurada bulunabilir: [http://msdn.microsoft.com/library/windowsazure/ee758710.aspx][http://msdn.microsoft.com/library/windowsazure/ee758710.aspx].
CSCFG dosyası, uygulamanızda her rol için bir rol öğesi içerir. CSCFG dosyasında belirtebilir öğelerin bazıları şunlardır:

* **İşletim sistemi sürümü**. Bu öznitelik, uygulama kodu çalıştıran tüm rol örnekleri için kullanılan istediğiniz işletim sistemi (OS) sürümünü seçmenize olanak sağlar. Bu işletim sistemi olarak bilinen *konuk işletim sistemi*, ve her yeni sürümü en son güvenlik düzeltme eki ve konuk işletim sistemini yayımlanır aynı anda kullanılabilir güncelleştirmeleri içerir. OsVersion öznitelik değeri ayarlamak, "\*", sonra da yeni konuk işletim sistemi sürümleri kullanılabilir duruma geldiğinde Azure konuk işletim sistemi her rolü örneklerinizi otomatik olarak güncelleştirir. Ancak, belirli bir konuk işletim sistemi sürümü seçerek Otomatik Güncelleştirmeler dışında tercih edebilirsiniz. Örneğin, osVersion özniteliği için bir değer ayarlamak "WA-GUEST-OS-2.8\_201109-01" Bu web sayfasında açıklanan almak tüm rolü örneklerinizi neden olur: [http://msdn.microsoft.com/library/hh560567.aspx] [http://msdn.microsoft.com/library/hh560567.aspx]. Konuk işletim sistemi sürümleri hakkında daha fazla bilgi için bkz: [Azure konuk işletim sistemi yükseltmeleri yönetme].
* **Örnekleri**. Bu öğenin değeri, belirli bir rol için bir kod çalıştırmasını sağlanan istediğiniz rol örneklerinin sayısını gösterir. Yeni bir CSCFG dosyası (uygulamanızın gerek olmadan) Azure'a karşıya yükleyebilir olduğundan, bu öğenin değerini değiştirin ve dinamik olarak artırmak veya uygulama kodunuz çalışan rol örneklerinin sayısını azaltmak için yeni bir CSCFG dosyası karşıya yüklemek trivially basit kalır . Bu yukarı veya aşağı ayrıca ne kadar rol örnekleri çalıştırmak için ücretlendirilirsiniz denetleme sırasında karşılayan gerçek iş yükü taleplerini uygulamanızı kolayca ölçeklendirmenizi sağlar.
* **Yapılandırma değerleri ayarı**. Bu öğe (CSDEF dosyasında tanımlanan) ayarları için değerleri gösterir. Çalışırken sizin rolünüz bu değerleri okuyabilir. Bu yapılandırma ayarlarının değerleri genellikle SQL veritabanına veya Azure Storage bağlantı dizelerini için kullanılır, ancak istediğiniz herhangi bir amaç için kullanılabilir.

## <a id="hostedservices"></a>Oluşturma ve bir barındırılan hizmet dağıtma
Barındırılan bir hizmet oluşturmak için gerekir, ilk gidin [Azure Yönetim Portalı] ve bir DNS öneki ve verileri belirterek bir barındırılan hizmet Merkezi'nden sağlama sonuçta kodunuzu çalışır durumda. Ardından, geliştirme ortamında, hizmet tanımı (CSDEF) dosyanızı oluşturun, uygulama kodu ve bunlar tüm dosyalar paket (ZIP) bir hizmet paketi (CSPKG) dosyasına yapı. Ayrıca, hizmet yapılandırma (CSCFG) dosyanızı hazırlamanız gerekir. Rolünüzün dağıtılacağı Azure Hizmet Yönetimi API'si ile CSPKG ve CSCFG dosyaları karşıya yükleme. Dağıtıldığında, Azure, sağlama rol örnekleri (yapılandırma verilerine göre) veri merkezindeki Uygulama kodunuz paketinden ayıklayın, rol örneklerine kopyalayın ve örnekleri önyükleme. Şimdi, kodunuzu çalışır durumda.

Aşağıdaki şekilde geliştirme bilgisayarınızda oluşturduğunuz CSPKG ve CSCFG dosyaları gösterir. CSPKG dosyası CSDEF dosya ve iki rolü kodunu içerir. Azure Hizmet Yönetimi API'si ile CSPKG ve CSCFG dosyaları karşıya yükleme sonrasında, Azure veri merkezinde rol örneği oluşturur. Bu örnekte, Azure üç rol örneklerini oluşturmalısınız CSCFG dosyası belirtilen \#rolün 1 ve iki örneğini \#2.

![Görüntü][5]

Yükseltme ve rollerinizi, yeniden dağıtma hakkında daha fazla bilgi için bkz: [dağıtma ve güncelleştirme Azure uygulamaları] [ Deploying and Updating Azure Applications] makalesi.<a id="Ref" name="Ref"></a>

## <a id="references"></a>Başvuruları
* [Azure için barındırılan bir hizmet oluşturma][Creating a Hosted Service for Azure]
* [Azure'da barındırılan hizmetleri yönetme][Managing Hosted Services in Azure]
* [Azure uygulamalarını geçirme][Migrating Applications to Azure]
* [Bir Azure uygulamanızı yapılandırma][Configuring an Azure Application]

<div style="width: 700px; border-top: solid; margin-top: 5px; padding-top: 5px; border-top-width: 1px;">

<p>Gamze Richter (Wintellect) tarafından yazılmış</p>

</div>

[Azure Application Model Benefits]: #benefits
[Hosted Service Core Concepts]: #concepts
[Hosted Service Design Considerations]: #considerations
[Designing your Application for Scale]: #scale
[Hosted Service Definition and Configuration]: #defandcfg
[The Service Definition File]: #def
[The Service Configuration File]: #cfg
[Creating and Deploying a Hosted Service]: #hostedservices
[References]: #references
[0]: ./media/application-model/application-model-3.jpg
[1]: ./media/application-model/application-model-4.jpg
[2]: ./media/application-model/application-model-5.jpg
[Configuring a Custom Domain Name in Azure]: http://www.windowsazure.com/develop/net/common-tasks/custom-dns/
[Data Storage Offerings in Azure]: http://www.windowsazure.com/develop/net/fundamentals/cloud-storage/
[3]: ./media/application-model/application-model-6.jpg
[4]: ./media/application-model/application-model-7.jpg

[Azure Pricing]: http://www.windowsazure.com/pricing/calculator/
[Managing Certificates in Azure]: http://msdn.microsoft.com/library/windowsazure/gg981929.aspx
[http://msdn.microsoft.com/library/windowsazure/ee758710.aspx]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
[http://msdn.microsoft.com/library/hh560567.aspx]: http://msdn.microsoft.com/library/hh560567.aspx
[Azure konuk işletim sistemi yükseltmeleri yönetme]: http://msdn.microsoft.com/library/ee924680.aspx
[Azure Yönetim Portalı]: http://manage.windowsazure.com/
[5]: ./media/application-model/application-model-8.jpg
[Deploying and Updating Azure Applications]: http://www.windowsazure.com/develop/net/fundamentals/deploying-applications/
[Creating a Hosted Service for Azure]: http://msdn.microsoft.com/library/gg432967.aspx
[Managing Hosted Services in Azure]: http://msdn.microsoft.com/library/gg433038.aspx
[Migrating Applications to Azure]: http://msdn.microsoft.com/library/gg186051.aspx
[Configuring an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405486.aspx
