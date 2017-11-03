---
title: "Bir uygulama hizmeti yapılandırma ortamı v1"
description: "Yapılandırma, yönetim ve uygulama hizmeti ortamı v1 izleme"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: b5a1da49-4cab-460d-b5d2-edd086ec32f4
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.openlocfilehash: 34fb3f15c03a3d3ef5f0a27081539bf0a6d19c5f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configuring-an-app-service-environment-v1"></a>Bir uygulama hizmeti ortamı v1

> [!NOTE]
> Bu makale hakkında uygulama hizmeti ortamı v1 yazılmıştır.  Uygulama hizmeti ortamı kullanmak daha kolay ve daha güçlü altyapısı üzerinde çalışan daha yeni bir sürümü var. Yeni sürüm Başlarken hakkında daha fazla bilgi edinmek için [uygulama hizmeti ortamı giriş](intro.md).
> 

## <a name="overview"></a>Genel Bakış
Yüksek bir düzeyde bir Azure uygulama hizmeti ortamı birkaç önemli bileşenlerden oluşur:

* Uygulama hizmeti ortamı'nda çalışan işlem kaynakları barındıran hizmet
* Depolama
* Bir veritabanı
* Classic(V1) veya kaynak Manager(V2) Azure sanal ağ (VNet) 
* Bir alt ağ içinde çalışan uygulama hizmeti ortamı'nın barındırılan hizmeti ile

### <a name="compute-resources"></a>İşlem kaynaklarını
İşlem kaynaklarını dört kaynak havuzları için kullanın.  Her uygulama hizmeti ortamı'nın (ana) ön uçlar ve üç olası çalışan havuzu kümesi vardır. Tüm üç alt havuzları--kullanmanız gerekmez isterseniz, yalnızca bir veya iki kullanabilirsiniz.

Ana bilgisayar (ön uçlar ve çalışanlar) kaynak havuzlarında kiracıların doğrudan erişilebilir değildir. Bağlanabilmek için bunların sağlama değiştirmek için Uzak Masaüstü Protokolü (RDP) kullanın veya bunları bir yönetim görevi görür.

Kaynak havuzu miktarı ve boyutu ayarlayabilirsiniz. ASE'de, P1 P4 aracılığıyla etiketli dört boyutu seçeneğiniz vardır. Bu boyutları ve bunların fiyatlandırma hakkında daha fazla ayrıntı için bkz: [uygulama hizmeti fiyatlandırma](https://azure.microsoft.com/pricing/details/app-service/).
Miktar veya boyutunu değiştirme bir ölçeklendirme işlemi adı verilir.  Yalnızca bir ölçeklendirme işlemi aynı anda ediyor olabilir.

**Ön Uçları**: ön uçlar, ana tutulan uygulamalarınız için HTTP/HTTPS noktalarıdır. Ön Uçları iş yükleri çalıştırmayın.

* Bir ana geliştirme ve test iş yükleri ve alt düzey üretim iş yükleri için yeterli olan iki P2s ile başlar. P3s Orta Ağır üretim iş yükleri için önerilir.
* Orta ila çok yoğun üretim iş yükleri, zamanlanmış bakım oluştuğunda çalıştıran yeterli ön uçlar emin olmak üzere en az dört P3s sahip olmasını öneririz. Zamanlanmış bakım etkinlikler aynı anda bir ön uç çıkarır. Bu genel azaltır bakım etkinlikleri sırasında kullanılabilir ön uç kapasitesi.
* Ön Uçları sağlamak için bir saat sürebilir. 
* Daha fazla ölçek ince ayar için CPU yüzdesi ve bellek yüzdesi ön uç havuzu için etkin istek ölçümleri izlemeniz gerekir. CPU veya bellek yüzdelerini P3s çalıştırırken yüzde 70 varsa, daha fazla ön uçlar ekleyin. Etkin istek değer 15.000 ön uç başına 20.000 istekleri için ortalama, daha fazla ön uçlar eklemeniz gerekir. Genel amacı % 70 CPU ve bellek yüzdelerini aşağıdaki korumaktır ve P3s çalıştırırken etkin ön başına 15.000 istek aşağıda için ortalama istekleri bitmelidir.  

**Çalışanlar**: uygulamalarınızı gerçekte çalıştırdığı çalışanlardır. Uygulama hizmeti planlarınızı ölçeklendirdiğinizde ilişkili çalışan havuzunda yukarı kullanılır.

* Çalışanlar anında ekleyemezsiniz. Bunlar sağlamak için bir saate kadar sürebilir.
* Herhangi bir havuz için bir işlem kaynağı boyutunu ölçeklendirme güncelleştirme etki alanı başına < 1 saat sürer. ASE'de 20 güncelleştirme etki alanı yoktur. İşlem boyutu 10 örneğiyle çalışan havuzunda ölçeği varsa, tamamlanması 10 saat sürebilir.
* Çalışan havuzunda kullanılan işlem kaynaklarını boyutunu değiştirirseniz, çalışan havuzunda çalışan uygulamaların soğuk başlatır neden olur.

Tüm uygulamalar çalıştırmayan bir çalışan havuzu işlem kaynak boyutunu değiştirmek için en hızlı yolu şudur:

* 2 çalışanlarına miktarını ölçeğini.  Minimum ölçek portal boyutunda aşağı 2'dir. Örneklerinizin ayırması için birkaç dakika sürer. 
* Örneklerinin sayısını ve yeni işlem boyutu seçin. Buradan, işlemin tamamlanması 2 saat sürecek.

Uygulamalarınızı daha büyük bir işlem kaynak boyutu gerektiriyorsa, önceki kılavuzunun yararlanamaz. Bu uygulamaları barındıran alt havuzunun boyutunu değiştirme yerine çalışanları için istenen boyutta sahip başka bir çalışan havuzu doldurun ve uygulamalarınızı ilgili havuza üzerine getirin.

* Gerekli işlem boyutu ek örneklerini başka bir çalışan havuzunda oluşturun. Bu bir saat tamamlanması için gereken.
* Yeni yapılandırılan çalışan havuzuna daha büyük bir boyutu gereken uygulamaları barındıran, uygulama hizmeti planları yeniden atayın. Tamamlanması bir dakikadan az zamanınızı hızlı bir işlemdir.  
* Artık kullanılmayan Bu örneklerde gerekmiyorsa ilk çalışan havuzunda ölçeklendirin. Bu işlemin tamamlanması birkaç dakika sürer.

**Otomatik ölçeklendirmeyi**: otomatik ölçeklendirmeyi işlem kaynak tüketimini yönetmenize yardımcı olacak araçlar biridir. Otomatik ölçeklendirme için kullanabileceğiniz ön uç veya çalışan havuzlarında. Örneklerinizin herhangi bir havuz türde sabah şeyler artış gibi ve akşam azaltır. Veya bir çalışan havuzunda kullanılabilir çalışan sayısı belirli bir eşiğin altına düştüğünde örnekleri belki de ekleyebilirsiniz.

Otomatik ölçeklendirme kurallarını işlem kaynak havuzu ölçümleri geçici ayarlamak istiyorsanız, ardından sağlanması gerekir. zaman unutmayın. Otomatik ölçeklendirmeyi App Service ortamları hakkında daha fazla ayrıntı için bkz: [otomatik ölçeklendirme bir uygulama hizmeti ortamı'nda yapılandırma][ASEAutoscale].

### <a name="storage"></a>Depolama
Her ana 500 GB depolama yapılandırılır. Bu alan, ana tüm uygulamalar arasında kullanılır. Bu depolama alanı ana bir parçasıdır ve depolama alanınızı kullanmak için şu anda alınamaz. Sanal ağ yönlendirme veya güvenlik ayarlamalar yapıyorsanız, yine Azure Storage--erişmesine izin vermek gereken veya ana çalışamaz.

### <a name="database"></a>Database
Veritabanı içinde çalışan uygulamalar hakkında ayrıntılar yanı sıra, ortamı tanımlayan bilgileri tutar. Bu çok bir Azure tutulan abonelik parçasıdır. İşlemek için doğrudan bir özelliği olan bir şey değil. Sanal ağ yönlendirme veya güvenlik ayarlamalar yapıyorsanız, hala SQL Azure--erişmesine izin vermek gereken veya ana çalışamaz.

### <a name="network"></a>Ağ
İle ana kullanılan VNet ana oluşturduğunuzda yaptığınız veya önceden yapılan olabilir. Ana oluşturma sırasında alt ağ oluşturduğunuzda, sanal ağ ile aynı kaynak grubunda olması için ana zorlar. Ağınızı farklı olması için ana tarafından kullanılan kaynak grubu gerekiyorsa, resource manager şablonu kullanarak, ana oluşturmanız gerekir.

Bir ana için kullanılan sanal ağda bazı sınırlamalar vardır:

* Sanal ağı bölgesel bir sanal ağ olmalıdır.
* Var. 8 veya daha fazla adresleri olan bir alt ağ ana dağıtıldığı olması gerekir.
* Alt ağ adres aralığı, bir alt ağ bir ana barındırmak için kullanılan sonra değiştirilemez. Bu nedenle, alt ağ gelecekteki ana büyümesine uyum sağlamak için en az 64 adreslerini içeren öneririz.
* Olabilir alt ancak ana başka bir şey.

Ana içerir barındırılan hizmet aksine [sanal ağ] [ virtualnetwork] ve alt ağ kullanıcı denetimi altında.  Sanal ağ kullanıcı Arabirimi veya PowerShell ile sanal ağınızı yönetebilirsiniz.  Bir ana Klasik veya Resource Manager Vnet'i dağıtılabilir.  Portal ve API deneyimleri Klasik ve Resource Manager sanal ağlar arasında biraz farklılık gösterir, ancak ana deneyimi aynıdır.

Bir ana barındırmak için kullanılan VNet ya da özel RFC1918 IP adresleri kullanabilirsiniz veya genel IP adresleri kullanabilirsiniz.  RFC1918 tarafından kapsanmayan bir IP aralığı kullanmak isteyip istemediğinizi (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16) yeniden ana oluşturma öncesinde, ana tarafından kullanılacak sanal ağ ve alt ağ oluşturmak gerekiyor.

Bu özellik Azure App Service sanal ağınıza yerleştirir olduğundan, ana barındırılan uygulamalarınızı şimdi doğrudan ExpressRoute veya siteden siteye sanal özel ağlar (VPN) kullanılabilir hale getirilir kaynaklara erişebilir anlamına gelir. Uygulama hizmeti ortamınızı içinde olan uygulamaları, uygulama hizmeti ortamınızı barındıran sanal ağ için kullanılabilir kaynaklara erişmek için ek ağ özellikleri gerekmez. Başka bir deyişle, VNET tümleştirme ya da karma bağlantılar da ya da sanal ağınıza bağlı kaynaklara almak için kullanmanız gerekmez. Bu özelliklerin her ikisi de olsa kaynaklara erişmek için sanal ağınıza bağlı olmayan ağlarda kullanmaya devam edebilirsiniz.

Örneğin, aboneliğinizde olmakla birlikte, ana bulunduğu sanal ağa bağlı olmayan bir sanal ağ ile tümleştirmek için VNET tümleştirme özelliğini kullanabilirsiniz. Karma bağlantılar, normalde yapabildiğiniz gibi diğer ağlarda bulunan kaynaklara erişmek için yine de kullanabilirsiniz.  

Bir ExpressRoute VPN ile yapılandırılmış sanal ağınız varsa, bir ana sahip yönlendirme gereksinimlerini bazıları farkında olmalıdır. Bir ana ile uyumlu olmayan bazı kullanıcı tanımlı yönlendirme (UDR) yapılandırmaları vardır. ExpressRoute ile bir sanal ağ içinde bir ana çalıştırma hakkında daha fazla ayrıntı için bkz: [bir sanal ağ ExpressRoute ile bir uygulama hizmeti ortamı çalışan][ExpressRoute].

#### <a name="securing-inbound-traffic"></a>Gelen trafiğinin güvenliğini sağlama
Ana gelen trafiği denetlemek için başlıca iki yöntem vardır.  Ağ güvenlik Groups(NSGs) hangi IP denetlemek için kullanabileceğiniz adresleri burada açıklandığı şekilde, ana erişim [bir uygulama hizmeti ortamına gelen trafiği denetlemek nasıl](app-service-app-service-environment-control-inbound-traffic.md) ve bir iç yük dengeleyici ile ana de yapılandırabilirsiniz (ILB).  ILB ana Nsg'leri kullanarak erişimi kısıtlamak istiyorsanız bu özellikleri de birlikte kullanılabilir.

Bir ana oluşturduğunuzda, sanal ağınızda bir VIP oluşturur.  İki VIP türleri, iç ve dış vardır.  Bir dış VIP ile bir ana oluşturduğunuzda, ana uygulamalarınızda bir Internet yönlendirilebilir IP adresi erişilebilir olacaktır. İç, ana seçtiğinizde, bir ILB ile yapılandırılmış ve doğrudan internet erişilebilir olmaz.  Bir ILB ana hala bir dış VIP gerektiriyor, ancak yalnızca Azure yönetim ve Bakım erişimi için kullanılır.  

ILB ana oluşturma sırasında ILB ana tarafından kullanılan alt etki alanı sağlamak ve belirttiğiniz alt etki alanı kendi DNS yönetmek gerekir.  Alt etki alanı adı ayarlamak için de HTTPS erişimi için kullanılan sertifika yönetmesi gerekir.  Ana oluşturulduktan sonra sertifika sağlamanız istenir.  Oluşturma ve bir ILB ana kullanma hakkında daha fazla bilgi için okuma [bir uygulama hizmeti ortamı ile bir iç yük dengeleyici kullanarak][ILBASE]. 

## <a name="portal"></a>Portal
Yönetebilir ve uygulama hizmeti ortamınızı Azure portalında kullanıcı arabirimini kullanarak izleyebilirsiniz. Bir ana varsa, daha sonra uygulama hizmeti simgenin Kenar çubuğunda görmeniz olasıdır. Bu simge, App Service ortamları Azure portalında temsil etmek için kullanılır:

![Uygulama hizmeti ortamı simgesi][1]

Tüm uygulama hizmeti ortamları listeler kullanıcı arabirimini açmak için simgeyi kullanın veya Köşeli Çift Ayraca seçin (">" simgesi) App Service ortamları seçmek için Kenar Çubuğu'nu ekranın alt kısmındaki. Listelenen ASEs birini seçerek, izlemek ve yönetmek için kullanılan kullanıcı arabirimini açın.

![İzleme ve uygulama hizmeti ortamınızı yönetmek için kullanıcı Arabirimi][2]

Bu ilk Kama kaynak havuzu başına ölçüm grafik yanı sıra, ana bazı özelliklerini gösterir. ' Nda gösterilen özelliklerin bazıları **Essentials** blok olan de onunla ilişkilendirilen dikey yukarı açılır köprüler. Örneğin, seçebileceğiniz **sanal ağ** UI'yi açmak için adı, ana çalışır durumda sanal ağ ile ilişkili. **Uygulama hizmeti planları** ve **uygulamaları** , ana bu öğeleri listesinde Kanatlar her açın.  

### <a name="monitoring"></a>İzleme
Grafikler, çeşitli performans ölçümleri her kaynak havuzundaki görmek izin verir. Ön uç havuzu için ortalama CPU ve bellek izleyebilirsiniz. Çalışan havuzlarında kullanılan miktar ve kullanılabilir miktar izleyebilirsiniz.

Birden çok uygulama hizmeti planları yapabilirsiniz çalışan havuzunda çalışanlar kullanın. Bu nedenle ön uç sunucuları ile yok CPU ve bellek kullanımı sağlamak gibi çok yararlı bilgiler in the way of iş yükünü aynı şekilde dağıtılmaz. Özellikle başkalarının kullanmak bu sistem yönetiyorsanız kaç tanesinin kullanıldığını çalışanları ve kullanılabilir--izleyen daha önemlidir.  

Ayrıca, uyarıları ayarlamak için grafiklerde tüm izlenebilir ölçümleri de kullanabilirsiniz. Burada uyarılarını ayarlama aynı olarak başka bir yere App Service içinde çalışır. Bir uyarı ayarlayabilirsiniz **uyarıları** UI parçası ya da kullanıcı Arabirimi ve seçerek herhangi ölçümleri inmelerini **eklemek uyarı**.

![Ölçümleri kullanıcı Arabirimi][3]

Yalnızca bahsedilen ölçümleri uygulama hizmeti ortamı ölçümleridir. Uygulama hizmeti plan düzeyinde kullanılabilir ölçümleri de vardır. Bu, CPU ve bellek izleme algılama çok kılan unsurdur.

ASE'de, uygulama hizmeti planları ayrılmış uygulama hizmeti planları tümü. Bu, uygulama hizmeti planı olduğunu uygulamalar bu uygulama hizmeti planında çalıştıran ayrılmış konaklarda uygulamalar yalnızca anlamına gelir. Uygulama hizmeti planınızın ayrıntılarını görmek için herhangi bir ana UI veya gelen listeleri uygulama hizmeti planınızı Getir **Gözat App Service planlarına** (listelerinin bunların tümünün).   

### <a name="settings"></a>Ayarlar
Ana Dikey içinde var olan bir **ayarları** çeşitli önemli özellikleri içeren bölümü:

**Ayarları** > **özellikleri**: **ayarları** dikey penceresi otomatik olarak açılır, ana dikey getirdiğinizde. Üst kısmındaki **özellikleri**. İçinde gördükleri için yedekli öğe burada sayısı vardır **Essentials**, ancak çok kullanışlı nedir **sanal IP adresi**, yanı **giden IP adreslerini**.

![Ayarlar dikey ve özellikleri][4]

**Ayarları** > **IP adreslerini**: bir IP Güvenli Yuva Katmanı (SSL) uygulama, ana oluşturduğunuzda, bir IP SSL adresi gerekir. Bir sertifika edinmeniz için sahip olduğu IP SSL adresleri ayrılabilen, ana gerekir. Bir ana oluşturulduğunda, bu amaç için bir IP SSL adresi vardır, ancak daha ekleyebilirsiniz. Var olan bir ücret ek IP SSL adresleri için gösterildiği gibi [uygulama hizmeti fiyatlandırma] [ AppServicePricing] (SSL bağlantılarını bölümünde). IP SSL fiyat ek fiyatıdır.

**Ayarları** > **ön uç havuzu** / **çalışan havuzlarında**: her bu kaynak havuzu dikey yalnızca bu kaynak havuzu hakkında bilgi görmek için olanak sağlar tam olarak bu kaynak havuzu ölçeklendirmek için Denetim sağlamanın yanı sıra.  

Her kaynak havuzu için temel dikey bu kaynak havuzu için bir grafik ölçümlerle sağlar. Gibi ana dikey penceresinden grafiklerle, grafiğe gidin ve Uyarıları istenen olarak ayarlayın. Belirli bir kaynak havuzu için ana dikey penceresinden bir uyarı ayarını kaynak havuzundan yapmakta olarak aynı şeyi yapar. Çalışan havuzundan **ayarları** dikey penceresinde tüm uygulamalara erişim sahibi veya uygulama hizmeti planları bu çalışan havuzunda çalışmakta olan.

![Çalışan havuzu ayarları kullanıcı Arabirimi][5]

### <a name="portal-scale-capabilities"></a>Portal ölçeği özellikleri
Üç ölçeklendirme işlemleri şunlardır:

* IP SSL kullanımı için uygun olan ana IP adresleri sayısını değiştirme.
* Bir kaynak havuzunda kullanılan hesaplama kaynağı boyutunu değiştirme.
* Bir kaynak havuzu el ile veya otomatik ölçeklendirmeyi aracılığıyla kullanılan işlem kaynakları sayısını değiştirme.

Portalı'nda, kaynak havuzlarında sahip kaç tane sunucuya denetlemek için üç yolu vardır:

* Üst ana ana dikey penceresinden bir ölçeklendirme işlemi. Birden fazla ölçek için ön uç ve çalışan havuzlarını yapılandırma değişiklikleri yapabilirsiniz. Tek bir işlem olarak tüm uygulanır.
* Tek başına bir kaynak havuzundan bir el ile bir ölçeklendirme işlemi **ölçek** altındaki dikey **ayarları**.
* Tek tek kaynak havuzundan ayarladığınız otomatik ölçeklendirmeyi **ölçek** dikey.

Ana dikey penceresinde ölçeklendirme işlemi kullanmak için istediğiniz ve kaydetme miktarı için kaydırıcıyı sürükleyin. Bu UI boyutunu değiştirme de destekler.  

![Ölçek kullanıcı Arabirimi][6]

Belirli kaynak havuzunda el ile veya otomatik ölçeklendirme özelliklerini kullanmak için şu adrese gidin **ayarları** > **ön uç havuzu** / **çalışan havuzlarında** olarak uygun. Daha sonra değiştirmek istediğiniz kuyruğunu açın. Git **ayarları** > **ölçeğini** veya **ayarları** > **ölçeği**. **Ölçeği Genişlet** dikey örneği miktar denetlemenize olanak sağlar. **Ölçeği Artır** kaynak boyutu denetlemenizi sağlar.  

![Ölçek ayarları kullanıcı Arabirimi][7]

## <a name="fault-tolerance-considerations"></a>Hataya dayanıklılık konuları
Bir uygulama hizmeti ortamı kadar 55 toplam işlem kaynaklarını kullanmak için yapılandırabilirsiniz. Bu 55 işlem kaynakları yalnızca 50 iş yüklerini barındırmak için kullanılabilir. Bunun nedeni iki yönlüdür. En az 2 ön uç işlem kaynakları yoktur.  Çalışan havuzu ayırma desteklemek için en fazla 53 bırakır. Hataya dayanıklılık sağlamak için aşağıdaki kurallara göre ayrılmış bir ek hesaplama kaynak sahip olmanız gerekir:

* Her bir çalışan havuzu bir iş yükü atanacak kullanılabilir olmayan en az 1 ek hesaplama kaynak gerekir.
* Çalışan havuzunda işlem kaynakları miktarı belirli bir değere gittiğinde, başka bir işlem kaynağı hataya dayanıklılık için gerekli değildir. Bu ön uç havuzu durumda değil.

Herhangi bir tek çalışan havuzunda içinde hataya dayanıklılık gereksinimleri, belirli bir değerini çalışan havuzuna atanan kaynakların X şunlardır:

* X 2 ile 20 arasında iş yükleri için kullanabileceğiniz kullanılabilir bilgi işlem kaynaklarının miktarını X-1 ise.
* X 21 ile 40 arasında iş yükleri için kullanabileceğiniz kullanılabilir bilgi işlem kaynaklarının miktarını X-2 ise.
* X 41 ve 53 arasında iş yükleri için kullanabileceğiniz kullanılabilir bilgi işlem kaynaklarının miktarını X-3 ise.

Minimum ayak 2 ön uç sunucuları ve 2 çalışan vardır.  Yukarıdaki ifadelerle daha sonra burada açıklamak için birkaç örnekler verilmiştir:  

* Tek bir havuzda 30 çalışanları varsa, bunların 28 iş yüklerini barındırmak için kullanılabilir.
* Tek bir havuzda 2 çalışan varsa, 1 ana iş yükleri için kullanılabilir.
* Tek bir havuzda 20 çalışanları varsa, 19 iş yüklerini barındırmak için kullanılabilir.  
* Tek bir havuzda 21 çalışanları varsa, sonra hala yalnızca 19 iş yüklerini barındırmak için kullanılabilir.  

Hataya dayanıklılık en boy önemlidir, ancak belirli eşikleri ölçeklendirmenize göre göz önünde bulundurmanız gerekir. 20 örneklerden giderek daha fazla Kapasite eklemek istiyorsanız, 21 herhangi daha fazla Kapasite eklemek değil çünkü 22 ya da daha yüksek geçin. Aynı kapasite ekler sonraki sayıyı 42 olduğu 40 giderek geçerlidir.  

## <a name="deleting-an-app-service-environment"></a>Bir uygulama hizmeti ortamı siliniyor
Bir uygulama hizmeti ortamı silmek istiyorsanız, yalnızca kullanmak **silmek** uygulama hizmeti ortamı dikey pencerenin üstündeki eylem. Bunu yaptığınızda, gerçekten Bunu yapmak istediğinizi onaylamak için uygulama hizmeti ortamınızı adını girmeniz istenir. Bir uygulama hizmeti ortamı sildiğinizde, tüm içeriğin içindeki silmek unutmayın.  

![Bir uygulama hizmeti ortamı UI silin.][9]  

## <a name="getting-started"></a>Başlarken
Uygulama hizmeti ortamları ile çalışmaya başlamak için bkz: [bir uygulama hizmeti ortamı oluşturmak nasıl](app-service-web-how-to-create-an-app-service-environment.md).

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-web-configure-an-app-service-environment/ase-icon.png
[2]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-aseblade.png
[3]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolchart.png
[4]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-properties.png
[5]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolblade.png
[6]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-scalecommand.png
[7]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolscale.png
[8]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-pricingtiers.png
[9]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-deletease.png

<!--Links-->
[WhatisASE]: app-service-app-service-environment-intro.md
[Appserviceplans]: ../azure-web-sites-web-hosting-plans-in-depth-overview.md
[HowtoCreateASE]: app-service-web-how-to-create-an-app-service-environment.md
[HowtoScale]: app-service-web-scale-a-web-app-in-an-app-service-environment.md
[ControlInbound]: app-service-app-service-environment-control-inbound-traffic.md
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[ASEAutoscale]: app-service-environment-auto-scale.md
[ExpressRoute]: app-service-app-service-environment-network-configuration-expressroute.md
[ILBASE]: app-service-environment-with-internal-load-balancer.md
