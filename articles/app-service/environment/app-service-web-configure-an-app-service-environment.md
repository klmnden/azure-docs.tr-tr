---
title: Yapılandırma bir App Service ortamı v1 - Azure
description: Yapılandırma, yönetim ve izleme App Service ortamı v1
services: app-service
documentationcenter: ''
author: ccompy
manager: stefsch
editor: ''
ms.assetid: b5a1da49-4cab-460d-b5d2-edd086ec32f4
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 5c0b4117f6e7b48dce1746ad6eb3dbe29c0d16af
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62130628"
---
# <a name="configuring-an-app-service-environment-v1"></a>Bir App Service ortamı v1

> [!NOTE]
> Bu makale, App Service ortamı v1 hakkında yöneliktir.  App Service ortamı, kullanımı daha kolay ve daha güçlü bir altyapı üzerinde çalışan daha yeni bir sürümü var. Yeni sürüm başlama hakkında daha fazla bilgi edinmek için [App Service ortamı giriş](intro.md).
> 

## <a name="overview"></a>Genel Bakış
Yüksek düzeyde, bir Azure App Service ortamı birkaç önemli bileşenden oluşur:

* App Service ortamında çalışan işlem kaynakları barındırılan hizmet
* Depolama
* Bir veritabanı
* Classic(V1) veya kaynak Manager(V2) Azure sanal ağ (VNet) 
* Bir alt ağ içinde çalıştırılan App Service ortamı barındırılan hizmet ile

### <a name="compute-resources"></a>İşlem kaynakları
İşlem kaynakları, dört kaynak havuzları için kullanırsınız.  Her App Service ortamı (ASE), bir dizi ön uçlar ve üç olası çalışan havuzu vardır. Tüm üç çalışan havuzları--kullanmanız gerekmez istiyorsanız, yalnızca bir veya iki kullanabilirsiniz.

Kiracılar için ana kaynak havuzlarında (ön uçlar ve çalışanlardan) doğrudan erişilemez. Uzak Masaüstü Protokolü (RDP) kullanmak için bunları bağlayın, kendi sağlama değiştirin veya bunları bir yönetim görevi görür.

Kaynak havuzu miktarı ve boyutu ayarlayabileceğiniz. Bir ASE'de, P1-P4 etiketli dört boyutu seçeneğiniz vardır. Bu boyutlar ve ettikleri ücretler hakkında daha fazla ayrıntı için bkz: [App Service fiyatlandırması](https://azure.microsoft.com/pricing/details/app-service/).
Miktarı ve boyutu değiştirme, bir ölçeklendirme işlemi çağrılır.  Yalnızca bir ölçeklendirme işlemi, aynı anda ediyor olabilir.

**Ön uçlar**: Ön uçlar, ASE'NİZDE tutulan uygulamalarınız için HTTP/HTTPS uç noktalardır. Ön uçlar iş yüklerini çalıştırma.

* Bir ASE, geliştirme/test iş yükleri ve alt düzey üretim iş yükleri için yeterli olan iki P2s ile başlar. P3s Orta Ağır üretim iş yükleri için önerilir.
* Orta Ağır üretim iş yükleri için zamanlanan bakım meydana geldiğinde çalışan yeterli ön uçlar olmadığından emin olmak için en az dört P3s sahip olmasını öneririz. Zamanlanmış bakım etkinlikleri, aynı anda bir ön uç çıkarır. Bu genel azaltır bakım etkinlikleri sırasında kullanılabilir ön uç kapasite.
* Ön uçlar sağlamak için bir saat kadar sürebilir. 
* Daha fazla ölçek ince ayar için CPU yüzdesi ve bellek yüzdesi ön uç havuzu için etkin istek ölçümlerini izlemeniz gerekir. CPU veya bellek yüzdelerini P3s çalıştırırken yüzde 70 varsa, daha fazla ön uçlar ekleyin. Etkin istek değeri 15.000 ön uç başına 20.000 istekleri için ortalama, daha fazla ön uçlar de eklemeniz gerekir. % 70'in CPU ve bellek yüzdelerini aşağıdaki genel amaç tutmaktır ve P3s çalıştırırken aşağıda ön başına 15.000 istekleri için ortalama etkin istek bitmelidir.  

**Çalışanları**: Uygulamalarınızı gerçekten çalıştırdığı çalışanlardır. App Service planlarınızda ölçeğini daralttığınızda, ilişkili çalışan havuzunda çalışan'kurmak kullanır.

* Anında çalışanları ekleyemezsiniz. Bunlar sağlamak için bir saate kadar sürebilir.
* Herhangi bir havuz için bir işlem kaynağı boyutunu ölçeklendirme güncelleştirme etki alanı başına < 1 saat sürer. Bir ASE'de 20 güncelleme etki alanı vardır. İşlem boyutu 10 örnekleri ile çalışan havuzunda ölçeği, tamamlanması 10 saat sürebilir.
* Çalışan havuzunda kullanılan işlem kaynaklarının boyutunu değiştirirseniz, bu çalışan havuzunda çalışan uygulamaları kaldırmanıza neden olur.

Herhangi bir uygulamayı çalıştırmayan bir çalışan havuzu işlem kaynak boyutunu değiştirmek için en hızlı yolu şudur:

* 2 çalışan sayısını aşağı ölçeklendirin.  En düşük ölçek boyutu portalında aşağı 2'dir. Örneklerinizin ayırması için birkaç dakika sürer. 
* Yeni işlem boyutu ve örnek sayısını seçin. Buradan, tamamlanması 2 saat sürer.

Uygulamalarınızı daha büyük bir işlem kaynak boyutu gerekiyorsa, önceki kılavuzunun yararlanamaz. Bu uygulamaları barındırma alt havuzunun boyutunu değiştirmek yerine, başka bir çalışan havuzu istenen boyut, çalışanlar ile doldurmak ve uygulamalarınızı Bu havuza üzerine getirin.

* Gerekli işlem boyutu ek örneklerini içinde başka bir çalışan havuzu oluşturun. Bu saat tamamlanması götürecek.
* App Service planlarınızda yeni yapılandırılan çalışan havuzunu daha büyük bir boyuta gerekir. uygulamaları barındırma yeniden atayın. Bu tamamlanması bir dakikadan az zamanınızı alarak hızlı bir işlemdir.  
* Bu örneği artık ihtiyacınız yoksa ilk çalışan havuzunu ölçeklendirin. Bu işlemin tamamlanması birkaç dakika sürer.

**Otomatik ölçeklendirme**: Bir işlem kaynak tüketiminize yönetmenize yardımcı olabilecek Araçları'nın otomatik ölçeklendirme yapıyor. Otomatik ölçeklendirme için kullanabileceğiniz ön uç veya çalışan havuzları. Sabah saatlerinde örneklerinizin herhangi bir havuz tür bu artış gibi şeyler ve akşam azaltın. Veya belki de bir çalışan havuzunda kullanılabilir olan çalışanların sayısını belirli bir eşiğin altına düştüğünde örnekler ekleyebilirsiniz.

Ardından işlem kaynak havuzu ölçümleri etrafında otomatik ölçeklendirme kurallarını ayarlamak istiyorsanız, sağlanması gerekir. zaman göz önünde bulundurun. Otomatik ölçeklendirme App Service ortamları hakkında daha fazla ayrıntı için bkz. [bir App Service ortamında otomatik ölçeklendirme yapılandırma][ASEAutoscale].

### <a name="storage"></a>Depolama
Her ASE ile 500 GB depolama alanı yapılandırılır. Bu alanı, ASE tüm uygulamalarda kullanılır. Bu depolama alanı, ASE bir parçasıdır ve depolama alanınızı kullanmak için şu anda geçirilemiyor. Sanal ağ yönlendirme veya güvenlik düzeltmeleri yapıyorsanız, Azure depolama--erişimi yine de izin vermeniz gerekir veya ASE çalışamaz.

### <a name="database"></a>Database
Veritabanı içinde çalışan uygulamaların ayrıntılarını yanı sıra ortamı tanımlayan bilgileri tutar. Bu çok bir Azure tutulan abonelik parçasıdır. İşlemek için doğrudan bir özelliği olan bir şey değil. Sanal ağ yönlendirme veya güvenlik düzeltmeleri yapıyorsanız, SQL Azure--erişmesine hala izin vermeniz gerekir veya ASE çalışamaz.

### <a name="network"></a>Ağ
ASE oluşturulması sırasında yaptığınız veya önceden yaptığınız ASE'NİZLE kullanılan sanal ağ olabilir. ASE oluşturma sırasında alt ağı oluştururken, sanal ağ ile aynı kaynak grubunda yer alması ASE zorlar. Sanal ağınıza farklı olacak şekilde ASE'nizi tarafından kullanılan kaynak grubunu gerekiyorsa, bir resource manager şablonu kullanarak ASE'nizi oluşturmanız gerekir.

Bir ASE için kullanılan sanal ağ üzerinde bazı kısıtlamalar vardır:

* Sanal ağı bölgesel sanal ağ olması gerekir.
* Orada ASE dağıtıldığı bir alt ağ ile en az 8 adres olması gerekir.
* Alt ağ adres aralığı, bir alt ağ bir ASE barındırmak için kullanılan sonra değiştirilemez. Bu nedenle, alt ASE gelecekteki büyümesine uyum sağlayacak en az 64 adresleri içeren öneririz.
* Söyleneni uyarlarsak alt ancak ASE başka bir şey olabilir.

ASE içeren bir barındırılan hizmet aksine [sanal ağ] [ virtualnetwork] ve alt ağ kullanıcı denetimi altında.  Sanal ağınızda sanal ağ kullanıcı Arabirimi veya PowerShell aracılığıyla yönetebilirsiniz.  Bir ASE, Klasik veya Resource Manager Vnet'i dağıtılabilir.  Portal ve API deneyimleri Klasik ve Resource Manager sanal ağları arasında biraz farklı olmasına rağmen ASE deneyimi aynı olur.

Bir ASE barındırmak için kullanılan sanal ağ ya da özel RFC1918 IP adreslerini kullanabilir veya genel IP adresleri kullanabilirsiniz.  RFC1918 tarafından kapsanmayan bir IP aralığı kullanmak isterseniz (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16), ASE'nizi ASE oluşturma önüne tarafından kullanılacak sanal ağ ve alt ağ oluşturmanız gerekir.

Bu özellik, Azure App Service, sanal ağa yerleştirir. çünkü, ASE'NİZDE barındırılan uygulamalarınızı doğrudan ExpressRoute veya siteden siteye sanal özel ağlar (VPN) kullanıma sunulan kaynakları artık erişebileceği anlamına gelir. App Service ortamınızın içinde uygulamalar, App Service ortamınızı barındıran sanal ağa kullanılabilir kaynaklara erişmek için ek ağ özellikleri gerektirmez. Başka bir deyişle, içinde veya sanal ağınıza bağlı kaynakları almak için VNET tümleştirmesi ya da karma bağlantılar'ı kullanmanız gerekmez. Bu özelliklerin her ikisi de olsa kaynaklara erişmek için sanal ağınıza bağlı olmayan ağlarda kullanmaya devam edebilirsiniz.

Örneğin, aboneliğinizde ancak ASE'nizi bulunduğu sanal ağa bağlı olmayan bir sanal ağ ile tümleştirmek için VNET tümleştirmesi kullanabilirsiniz. Karma bağlantılar, normalde yapabildiğiniz gibi diğer ağlarda bulunan kaynaklara erişmek için yine de kullanabilirsiniz.  

Bir ExpressRoute VPN ile yapılandırılan sanal ağınız varsa, bazı ASE olan Yönlendirme gereksinimlerinin uyumlu olması gerekir. Bir ASE ile uyumlu olmayan bazı kullanıcı tanımlı yol (UDR) yapılandırmalar vardır. ExpressRoute ile bir sanal ağdaki bir ASE çalıştırma hakkında daha fazla ayrıntı için bkz. [ExpressRoute ile bir sanal ağdaki bir App Service ortamı çalıştıran][ExpressRoute].

#### <a name="securing-inbound-traffic"></a>Gelen trafiği güvenli hale getirme
ASE'NİZİN gelen trafiği denetlemek için iki birincil yöntem vardır.  Ağ güvenlik Groups(NSGs) hangi IP denetlemek için kullanabileceğiniz adresleri, ASE'nizi erişebilir, burada açıklandığı [bir App Service Ortamı'nda gelen trafiği denetleme](app-service-app-service-environment-control-inbound-traffic.md) ve ASE'nizi bir iç Load Balancer ile de yapılandırabilirsiniz (ILB).  ILB ASE'nizi Nsg'leri kullanarak erişimi kısıtlamak istiyorsanız bu özellikleri de birlikte kullanılabilir.

Bir ASE oluşturduğunuzda, ağınızda bir VIP oluşturun.  İki VIP tür, iç ve dış vardır.  Dış VIP ile ASE oluşturma, ASE'nizi uygulamalarınızda bir Internet yönlendirilebilir IP adresi erişilebilir hale gelir. Dahili ASE'nizi seçtiğinizde, ILB ile yapılandırılır ve doğrudan İnternet'ten erişilemediği olmayacaktır.  ILB ASE hala dış VIP gerektiriyor, ancak yalnızca Azure yönetim ve Bakım erişim için kullanılır.  

ILB ASE oluşturma sırasında ILB ASE tarafından kullanılan alt etki alanı sağlar ve belirttiğiniz alt etki alanı kendi DNS'İNİZİ yönetmeniz gerekir.  Alt etki alanı adı ayarlamak için de HTTPS erişim için kullanılan sertifikanın yönetmeniz gerekir.  ASE oluşturulduktan sonra sertifika sağlamanız istenir.  Oluşturma ve ILB ASE kullanma hakkında daha fazla bilgi için okumaya devam [bir App Service ortamı ile bir iç Load Balancer'ı kullanarak][ILBASE]. 

## <a name="portal"></a>Portal
Yönetebilir ve Azure Portalı'nda kullanıcı arabirimini kullanarak App Service ortamınızı izleyebilirsiniz. Bir ASE varsa, daha sonra App Service sembol, Kenar çubuğunda görmeniz olasıdır. Bu simge, Azure portalında App Service ortamları temsil etmek için kullanılır:

![App Service ortamı simgesi][1]

Tüm App Service ortamınızı listeleyen kullanıcı arabirimini açmak için bir simge kullanın veya köşeli çift ayracı seçin (">" simgesi) altındaki kenar App Service ortamları seçin. Listelenen Ase'ler birini seçerek, izlemek ve yönetmek için kullanılan kullanıcı arabirimini açın.

![İzleme ve App Service ortamınızı yönetmek için kullanıcı Arabirimi][2]

İlk bu dikey pencere, kaynak havuzu başına bir ölçüm grafiği birlikte ase'nizi bazı özellikleri gösterir. Gösterilen özelliklerin bazıları **Essentials** blok onunla ilişkilendirilen dikey'kurmak açılır köprüler de bulunur. Örneğin, seçebileceğiniz **sanal ağ** ilişkili UI'yi açmak için adı ASE'NİZİN çalıştıran bir sanal ağ ile. **App Service planları** ve **uygulamaları** ASE'NİZDE bu öğeleri listesi dikey pencereleri her açın.  

### <a name="monitoring"></a>İzleme
Grafikleri bir çeşitli performans ölçümleri her kaynak havuzundaki görmenize olanak sağlar. Ön uç havuzu için ortalama CPU ve bellek izleyebilirsiniz. Çalışan havuzları için kullanılan miktar ve kullanılabilir olan miktar izleyebilirsiniz.

Birden fazla App Service planları yapabilirsiniz çalışan havuzunda çalışan kullanın. Bu nedenle ön uç sunucuları ile yoksa CPU ve bellek kullanımı sağlamak gibi çok yararlı bilgiler in the way of iş yükü aynı şekilde dağıtılmaz. Özellikle bu sistemi kullanmak üzere başkalarını için yönetiyorsanız kaç tanesinin kullanıldığını çalışanları ve kullanılabilir--izlemek daha önemlidir.  

Ayrıca, uyarıları ayarlamak için grafikteki tüm izlenebilir ölçümleri de kullanabilirsiniz. Burada uyarıları ayarlama gibi başka bir yerde App Service'te çalışır. Bir uyarı ayarlayabilirsiniz **uyarılar** kullanıcı Arabirimi parçası veya kullanıcı Arabirimi ve seçerek herhangi ölçümlerinde ayrıntılara gelen **ekleme uyarı**.

![Ölçümleri kullanıcı Arabirimi][3]

Yalnızca ele alınan ölçümleri, App Service ortamı ölçümleridir. Ayrıca App Service planı düzeyinde kullanılabilir ölçümler de vardır. Burada CPU ve bellek izleme mantıklı hale getirir budur.

Bir ASE'de App Service planları adanmış App Service planları tümü. Bu, App Service planı olduğunu uygulamaları bu App Service planında çalıştıran konaklarda ayrılan uygulamalar yalnızca anlamına gelir. App Service planınızın ayrıntılarını görmek için herhangi listelerinin ASE kullanıcı Arabirimine gelen veya App Service planınızı Getir **Gözat App Service planları** (listeleyen tümünün).   

### <a name="settings"></a>Ayarlar
Yoktur ASE dikey pencerede bir **ayarları** çeşitli önemli özellikleri içeren bölümü:

**Ayarları** > **özellikleri**: **Ayarları** dikey penceresi otomatik olarak açılır ASE dikey pencerenizi getirdiğinizde. En üstte **özellikleri**. İçinde gördükleri için yedekli öğeleri burada birçok **Essentials**, ama çok kullanışlı nedir **sanal IP adresi**, yanı **giden IP adresleri**.

![Ayarlar dikey penceresinde ve Özellikler][4]

**Ayarları** > **IP adresleri**: ASE'NİZDE IP Güvenli Yuva Katmanı (SSL) uygulama oluşturduğunuzda, bir IP SSL adresi gerekir. Bir sertifika edinmeniz için ayrılan belleğe sahip IP SSL adresleri ASE'nizi gerekir. Bir ASE oluşturulurken bu amaç için bir IP SSL adresi vardır, ancak daha ekleyebilirsiniz. Bir ücret yoktur ek IP SSL adresleri için gösterildiği [App Service fiyatlandırması] [ AppServicePricing] (bölümündeki SSL bağlantıları üzerinde). Ek fiyat IP SSL fiyatıdır.

**Ayarları** > **ön uç havuzu** / **çalışan havuzları**: Her biri bu kaynak havuzu dikey tam olarak bu kaynak havuzu ölçeklendirme denetim sağlamanın yanı sıra yalnızca o kaynak havuzundaki, bilgi görme olanağı sunar.  

Her kaynak havuzu için temel dikey pencere, bu kaynak havuzu için bir ölçüm grafiği sağlar. Gibi ASE dikey penceresinden grafiklerle oluşturabilir grafiğe gidin ve istediğiniz gibi uyarıları ayarlama. Belirli bir kaynak havuzu için ASE dikey penceresinden bir uyarı ayarını kaynak havuzundan yapmakta olarak aynı şeyi yapar. Çalışan havuzundan **ayarları** dikey penceresinde App Service planları bu çalışan havuzunda çalışmakta olan veya tüm uygulamalara erişebilirsiniz.

![Çalışan havuzu ayarları kullanıcı Arabirimi][5]

### <a name="portal-scale-capabilities"></a>Portal ölçek özellikleri
Üç ölçeklendirme işlemleri vardır:

* IP SSL kullanımı için kullanılabilir olan IP adresleri ASE içindeki sayısını değiştirme.
* Bir kaynak havuzunda kullanılan bilgi işlem kaynağını boyutunu değiştirme.
* Bir kaynak havuzunda el ile veya otomatik ölçeklendirme aracılığıyla kullanılan işlem kaynaklarının sayısını değiştirme.

Portalda, kaynak havuzlarınızın sahip kaç tane sunucuya denetlemek için üç yolu vardır:

* Üst ana ASE dikey penceresinden bir ölçeklendirme işlemi. Birden çok ölçek yapılandırma için ön uç ve çalışan havuzları değişiklik yapabilirsiniz. Bunların tümü tek bir işlem olarak uygulanır.
* Tek bir kaynak havuzundan bir el ile ölçeklendirme işlemi **ölçek** altındaki dikey penceresinde **ayarları**.
* Tek bir kaynak havuzundan ayarladığınız otomatik ölçeklendirme **ölçek** dikey penceresi.

ASE dikey ölçeklendirme işlemi kullanmak için kaydetme ve istediğiniz miktar kaydırıcıyı sürükleyin. Bu kullanıcı arabirimini boyutunu değiştirme da destekler.  

![Ölçek kullanıcı Arabirimi][6]

Belirli kaynak havuzunda el ile veya otomatik ölçeklendirme özellikleri kullanmak için Git **ayarları** > **ön uç havuzu** / **çalışan havuzları** olarak uygun. Daha sonra değiştirmek istediğiniz havuz ölçeğini açın. Git **ayarları** > **ölçeğini** veya **ayarları** > **ölçeği**. **Ölçeği genişletme** dikey örneği miktar denetlemenizi sağlar. **Ölçeği Artır** kaynak boyutunu denetlemenizi sağlar.  

![Ölçek ayarları kullanıcı Arabirimi][7]

## <a name="fault-tolerance-considerations"></a>Hataya dayanıklılık konuları
Bir App Service ortamı kadar 55 toplam işlem kaynaklarını yapılandırabilirsiniz. Bu 55 işlem kaynakları yalnızca 50 iş yüklerini barındırmak için kullanılabilir. Bunun nedeni yönlüdür. En az 2 ön uç bilgi işlem kaynaklarının yoktur.  Bu, çalışan havuzu ayırma desteklemek için en fazla 53 bırakır. Hataya dayanıklılık sağlamak için aşağıdaki kurallara göre ayrılmış bir ek bilgi işlem kaynağına sahip olmanız gerekir:

* Her çalışan havuzu, bir iş yükü atanabilir değil en az 1 ek işlem kaynağı gerekir.
* Ardından belirli bir değere bir çalışan havuzu bilgi işlem kaynaklarının miktarı aşması durumunda hataya dayanıklılık için başka bir işlem kaynağı gereklidir. Ön uç havuzu durumda değil.

Herhangi tek bir çalışan havuzu içinde hata toleransı, X bir çalışan havuzu için atanan kaynakların belirli bir değeri için gereksinimleridir:

* X 2 ile 20 arasında ise, iş yükleri için kullanabileceği kullanılabilir bilgi işlem kaynakları X-1 miktarıdır.
* X 21 ile 40 arasında ise, iş yükleri için kullanabileceği kullanılabilir bilgi işlem kaynakları X-2 miktarıdır.
* X 53 41 arasında ise, iş yükleri için kullanabileceği kullanılabilir bilgi işlem kaynakları X-3 miktarıdır.

Minimum ayak 2 ön uç sunucularına ve 2 çalışan sahiptir.  Yukarıdaki ifadelerle ardından açıklamak için bazı örnekler şunlardır:  

* Tek bir havuzda 30 çalışanları varsa, bunların 28 konak iş yükleri için kullanılabilir.
* Daha sonra 2 çalışan tek bir havuzda varsa, 1 konak iş yükleri için kullanılabilir.
* Tek bir havuzda 20 çalışanları varsa, 19 konak iş yükleri için kullanılabilir.  
* Sonra hala yalnızca 19, 21 çalışan tek bir havuzda varsa, konak iş yükleri için kullanılabilir.  

Hata toleransı en boy önemlidir, ancak belirli eşikleri ölçeklendirilirken aklınızda tutmanız gerekir. 20 örneklerden giderek daha fazla Kapasite eklemek istiyorsanız, 21 herhangi bir daha fazla kapasite eklemez çünkü ardından 22 veya üzeri gidin. Aynı kapasitesi ekler sonraki sayıyı 42 olduğu 40 giderek geçerlidir.  

## <a name="deleting-an-app-service-environment"></a>Bir App Service ortamı siliniyor
App Service ortamı silmek istiyorsanız, sadece kullanın **Sil** App Service ortamı dikey pencerenin üst kısmındaki eylem. Bunu yaptığınızda, gerçekten Bunu yapmak istediğinizi onaylamak için App Service ortamınızın adını girmeniz istenir. Bir App Service ortamı sildiğinizde tüm içerik içindeki silmek unutmayın.  

![App Service ortamı UI Sil][9]  

## <a name="getting-started"></a>Başlarken
App Service ortamları ile çalışmaya başlamak için bkz. [bir App Service ortamı oluşturma](app-service-web-how-to-create-an-app-service-environment.md).

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
[Appserviceplans]: ../overview-hosting-plans.md
[HowtoCreateASE]: app-service-web-how-to-create-an-app-service-environment.md
[HowtoScale]: app-service-web-scale-a-web-app-in-an-app-service-environment.md
[ControlInbound]: app-service-app-service-environment-control-inbound-traffic.md
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: https://azure.microsoft.com/pricing/details/app-service/
[ASEAutoscale]: app-service-environment-auto-scale.md
[ExpressRoute]: app-service-app-service-environment-network-configuration-expressroute.md
[ILBASE]: app-service-environment-with-internal-load-balancer.md
