---
title: App Service - Azure işletim sistemi işlevi
description: Web uygulamaları, mobil uygulama arka uçları ve Azure App Service'te API apps için kullanılabilen işletim sistemi işlevler hakkında bilgi edinin
services: app-service
documentationcenter: ''
author: cephalin
manager: erikre
editor: mollybos
ms.assetid: 39d5514f-0139-453a-b52e-4a1c06d8d914
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2018
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: e5ab6651503766844b2aeef1849bffff9cf4d7bb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60835525"
---
# <a name="operating-system-functionality-on-azure-app-service"></a>Azure App Service üzerindeki işletim sistemi işlevi
Bu makalede çalışan tüm Windows uygulamaları için kullanılabilir olan ortak temel işletim sistemi işlevi [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714). Bu işlev, dosya, ağ ve kayıt defteri erişim ve tanılama günlüklerini ve olayları içerir. 

> [!NOTE] 
> [Linux uygulamaları](containers/app-service-linux-intro.md) App Service'te kendi kapsayıcılarda çalıştırın. Hiçbir konak işletim sistemi erişimine izin verileceğini, kapsayıcıya kök erişimi vardır. Benzer şekilde, için [Windows kapsayıcılarında çalıştırılan uygulamalar](app-service-web-get-started-windows-container.md), kapsayıcıya yönetim erişimi, ancak konak işletim sistemine erişimi vardır. 
>

<a id="tiers"></a>

## <a name="app-service-plan-tiers"></a>App Service planı katmanları
App Service, müşteri uygulamalarını bir çok kiracılı barındırma ortamında çalışır. Dağıtılan uygulamaları içinde **ücretsiz** ve **paylaşılan** Katmanlar dağıtılmış uygulamalar sırada bu alt işlemler paylaşılan sanal makinelerde çalıştırır **standart** ve **Premium**  özellikle tek bir müşteriye ile ilişkili uygulamalar için ayrılmış sanal makine katmanları çalıştırın.

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

App Service farklı katmanlar arasında ölçeklendirme sorunsuzca desteklediğinden, App Service uygulamaları için zorunlu güvenlik yapılandırması aynı kalır. Bu uygulamaları aniden farklı olarak, App Service planı bir katmandan diğerine geçiş yaparken, beklenmeyen şekillerde başarısız davrandığını yoksa sağlar.

<a id="developmentframeworks"></a>

## <a name="development-frameworks"></a>Geliştirme çerçeveleri
App Service fiyatlandırma katmanları işlem kaynaklarının (CPU, disk depolama, bellek ve ağ çıkışı) uygulamaları için kullanılabilir miktarını denetler. Ancak işlevselliği framework uygulamaları için kullanılabilir çeşitliliğini ölçeklendirme katmanları bağımsız olarak aynı kalır.

App Service geliştirme çerçeveleri, ASP.NET, klasik ASP, node.js, PHP ve Python uzantıları IIS içinde Çalıştır tümü - dahil olmak üzere çeşitli destekler. Basitleştirin ve Güvenlik Yapılandırması'leri normalleştirmek için App Service uygulamaları genellikle kendi varsayılan ayarlarla çeşitli geliştirme çerçeveleri çalıştırın. İşlevselliği için her bireysel geliştirme çerçevesi ve API yüzey alanı özelleştirmek için uygulamaları yapılandırma bir yaklaşım silinmiş. App Service, ortak temel işletim sistemi işlevselliği bir uygulamanın geliştirme framework bağımsız olarak sağlayarak daha genel bir yaklaşım bunun yerine alır.

Aşağıdaki bölümlerde, App Service uygulamaları için kullanılabilir olan işletim sistemi işlevi genel türleri özetlenmektedir.

<a id="FileAccess"></a>

## <a name="file-access"></a>Dosya erişimi
Çeşitli sürücüleri, yerel sürücülere ve ağ sürücülerini dahil olmak üzere App Service içinde mevcut.

<a id="LocalDrives"></a>

### <a name="local-drives"></a>Yerel sürücüler
Özünde, App Service Azure PaaS (hizmet olarak platform) altyapısı üzerinde çalışan bir hizmettir. Sonuç olarak, "bir sanal makineye bağlı olan" yerel sürücüler aynı sürücü Azure'da çalışan herhangi bir çalışan rolü için kullanılabilir türleridir. Buna aşağıdakiler dahildir:

- Bir işletim sistemi sürücüsü (D:\ sürücüsüne)
- (Ve müşteriler için erişilemez) yalnızca App Service tarafından kullanılan Azure paket cspkg dosyalarını içeren bir uygulama sürücüsü
- Boyutu VM boyutuna bağlı olarak farklılık gösterir. "kullanıcı" sürücü (C:\ sürücüsü). 

Uygulamanız büyüdükçe, disk kullanımını izlemek önemlidir. Disk kotası üst sınırına ulaşıldığında, uygulamanıza olumsuz etkileri olabilir. Örneğin: 

- Uygulama, yeterli disk alanı olduğunu belirten bir hata oluşturabilecek.
- Kudu konsoluna göz atarken, disk hataları görebilirsiniz.
- VSTS veya Visual Studio dağıtım başarısız olabilir `ERROR_NOT_ENOUGH_DISK_SPACE: Web deployment task failed. (Web Deploy detected insufficient space on disk)`.
- Uygulamanız yavaş performans düşebilir.

<a id="NetworkDrives"></a>

### <a name="network-drives-aka-unc-shares"></a>Ağ sürücülerinde (diğer adıyla UNC paylaşımları)
App Service, uygulama dağıtımı ve bakımı kolaylaştırıyor benzersiz yönleri tüm kullanıcı içerik UNC paylaşımlarını kümesinde depolanır biridir. Bu modeli de şirket içi web barındırma ortamları, birden çok yük dengeli sunucusu tarafından kullanılan içerik depolama ortak desen eşleşir. 

App Service içinde UNC paylaşımlarını her veri merkezi içinde oluşturulan bir sayısı yoktur. Ayrılmışın yüzdesi kullanıcı içeriği her veri merkezinde tüm müşteriler için her bir UNC paylaşımı. Ayrıca, tüm dosya tek bir müşterinin aboneliğini her zaman aynı UNC yerleştirildiği için içerik paylaşın. 

Azure hizmetlerinin çalışması nedeniyle bir UNC paylaşımı barındırmak için sorumlu belirli sanal makine zaman içinde değişecektir. Azure işlemleri sırasında normal çalıştıysa duruma yukarı ve aşağı doğru olarak UNC paylaşımlarını farklı sanal makineler tarafından bağlanacaktır sağlanır. Bu nedenle, uygulamaları hiçbir zaman sabit kodlanmış tahminler makine bilgiler UNC dosya yolunu, zaman içinde kararlı kalacak olmanız gerekir. Bunun yerine, bunlar uygun kullanmalısınız *sahte* mutlak yol **D:\home\site** , App Service sağlar. Bu sahte bir mutlak yol birinin kendi uygulamasına başvuran için taşınabilir, uygulama ve kullanıcı belirsiz bir yöntem sağlar. Kullanarak **D:\home\site**, bir aktarılabileceği paylaşılan dosyalar uygulama başka bir uygulama her aktarımı için yeni bir mutlak yol yapılandırmak zorunda kalmadan.

<a id="TypesOfFileAccess"></a>

### <a name="types-of-file-access-granted-to-an-app"></a>Bir uygulamaya verilen dosya erişim türleri
Her müşterinin hizmet aboneliği, bir veri merkezi içinde belirli bir UNC paylaşımında ayrılmış dizin yapısı vardır. Bir müşteri paylaşan birden fazla uygulama tüm dizinlerin bir tek bir müşteri aboneliğine ait aynı UNC oluşmasını bir özel veri merkezi içinde oluşturulmuş olabilir. Paylaşımın içeriği, hata ve tanılama günlükleri ve kaynak denetimi tarafından oluşturulan uygulamanın önceki sürümleri için olanlar gibi dizinler içerebilir. Beklendiği gibi bir müşterinin uygulamayı dizinleri okuma ve yazma erişimi çalışma zamanında uygulamanın uygulama kodu tarafından kullanılabilir.

Bir uygulamayı çalıştıran sanal makineye bağlı yerel sürücülerde App Service bir öbek C:\ sürücüsüne uygulamaya özgü geçici yerel depolama alanı ayırır. Uygulama kendi yerel geçici depolama birimine tam okuma/yazma erişimi olsa da, depolama gerçekten doğrudan uygulama kodu tarafından kullanılmak üzere tasarlanmamıştır. Bunun yerine, amaç geçici dosyaları depolamak için IIS ve web uygulama çerçeveleri sağlamaktır. App Service Ayrıca, tek tek uygulamalar, yerel dosya depolama alanının aşırı miktarda tüketmesini önlemek için her uygulama için kullanılabilir geçici yerel depolama miktarını sınırlar.

App Service'nın yerel geçici depolama nasıl kullandığını iki dizin için ASP.NET dosyaları örnekleridir ve IIS dizinini sıkıştırılmış dosyaları dışarıda bırak. ASP.NET derleme sistemi "ASP.NET dosyaları" dizininin bir geçici derleme önbellek konumu olarak kullanır. IIS "IIS geçici sıkıştırılmış dosyalar" dizini sıkıştırılmış yanıt çıkışı depolamak için kullanır. İki dosya kullanım (yanı sıra diğer) bu tür, App Service'te uygulama başına geçici yerel depolama alanına eşleştirilir. Bu yeniden eşleme işlevselliği beklendiği gibi devam etmesini sağlar.

App Service her uygulamada "daha fazla burada açıklanan uygulama havuzu kimliği" olarak adlandırılan bir rastgele benzersiz düşük ayrıcalıklı çalışan işlem kimliği çalışan: [ https://www.iis.net/learn/manage/configuring-security/application-pool-identities ](https://www.iis.net/learn/manage/configuring-security/application-pool-identities). Uygulama kodu, işletim sistemi sürücüsüne (D:\ sürücüsüne) temel salt okunur erişim için bu kimliği kullanır. Bu, uygulama kodu ortak bir dizin yapıları listesi ve işletim sistemi sürücüsünde ortak dosyaları okuma anlamına gelir. Bu, sağladığınızda bir biraz geniş erişim düzeyini, aynı dizinleri ve dosyaları erişilebilir olacak şekilde görünebilir ancak bir Azure çalışan rolünde barındırılan hizmet ve sürücü içeriğini okuyun. 

<a name="multipleinstances"></a>

### <a name="file-access-across-multiple-instances"></a>Birden fazla örnek arasında dosya erişimi
Bir uygulamanın içeriği giriş dizini içerir ve kendisine uygulama kodu yazabilirsiniz. Bir uygulama birden çok örnek üzerinde çalışıyorsa, tüm örnekleri aynı dizine görebilmesi için giriş dizini tüm örnekleri arasında paylaşılır. Bir uygulama için giriş dizini karşıya yüklenen dosyaları kaydederse, bu nedenle, örneğin, bu dosyaları tüm örnekleri için hemen kullanılabilir. 

<a id="NetworkAccess"></a>

## <a name="network-access"></a>Ağ erişimi
Uygulama kodu, TCP/IP ve UDP tabanlı protokolleri, dış hizmetler sunan Internet erişime açık Uç noktalara giden ağ bağlantılarını yapmak için kullanabilirsiniz. Uygulamaları, azure'daki hizmetlere bağlanmak için bu aynı protokolleri kullanabilir&#151;SQL veritabanı için HTTPS bağlantıları oluşturarak örneğin.

Bir yerel bir geri döngü bağlantısını kurmak ve bu yerel bir geri döngü yuvadaki dinleme bir uygulamaya sahip uygulamalar için sınırlı bir özelliği de mevcuttur. Öncelikle işlevleri bir parçası olarak yerel bir geri döngü yuvalarda dinleme uygulamaları etkinleştirmek için bu özelliği var. Her bir uygulama bir "özel" geri döngü bağlantısını görür. Uygulama "A", "B" uygulama tarafından oluşturulan yerel bir geri döngü yuva için dinleyemiyor.

Adlandırılmış Kanallar topluca bir uygulamayı çalıştırmayı farklı işlemler arasında işlemler arası iletişim (IPC) mekanizması olarak da desteklenir. Örneğin, IIS Fastcgı modülü PHP sayfaları çalıştıran tek tek işlemleri koordine etmek için adlandırılmış kanallar kullanır.

<a id="Code"></a>

## <a name="code-execution-processes-and-memory"></a>Kod yürütme, süreçleri ve bellek
Daha önce belirtildiği gibi uygulamalar bir rastgele bir uygulama havuzu kimliği kullanarak düşük ayrıcalıklı çalışan işlemleri içinde çalıştırın. Uygulama kodu, CGI işlemleri veya diğer uygulamalar tarafından kökenli alt işlemlerin yanı sıra, çalışan işlemi ile ilişkili bellek alanına erişebilir. Ancak, aynı sanal makinede olsa bile, tek bir uygulama bellek veya başka bir uygulamanın veri erişemez.

Uygulamaları, betikleri veya desteklenen web geliştirme çerçeveleri ile yazılan sayfaların çalıştırabilirsiniz. App Service, daha kısıtlı modları herhangi bir web çerçevesi ayarı yapılandırmaz. Örneğin, App Service üzerinde çalışan ASP.NET uygulamaları "tam" güven aksine daha kısıtlı bir güven modunda çalışır. Web çerçeveleri, hem klasik ASP ve ASP.NET dahil olmak üzere, Windows işletim sistemindeki varsayılan kayıtlı ADO (ActiveX Data Objects) gibi işlemde COM bileşenleri (ancak işlem COM bileşenleri dışı) çağırabilirsiniz.

Uygulamalar, oluşturma ve rastgele kod çalıştırın. Bir komut kabuğunu üretme yapma veya bir PowerShell betiğini çalıştırmak için bir uygulama için izin verilebilir. Rastgele kod ve işlemleri bir uygulamadan kökenli olsa da, ancak yürütülebilir programları ve betikleri üst uygulama havuzuna verilen ayrıcalıkları sınırlı kalır. Örneğin, yürütülebilir aynı IP adresini bir sanal makineden, NIC bağlantısını girişimi olamaz ancak bu, bir giden HTTP çağrısı yapan bir yürütülebilir bir uygulama oluşturabilir Giden ağ çağırmak için düşük ayrıcalıklı koda izin verilir, ancak çalışan bir sanal makinede ağ ayarlarını yapılandırmak yönetici ayrıcalıkları gerektirir.

<a id="Diagnostics"></a>

## <a name="diagnostics-logs-and-events"></a>Tanılama günlüklerini ve olayları
Günlük bilgilerini erişmeye çalışan bazı uygulamalar verileri başka bir kümesidir. App Service'te çalışan kod için kullanılabilir günlük bilgi türlerini tanılama içerir ve ayrıca uygulamaya kolayca erişilebilir bir uygulama tarafından oluşturulan bilgileri günlüğe kaydetmek. 

Örneğin, etkin bir uygulama tarafından oluşturulan W3C HTTP günlükleri ya da bir günlük dizini W3C günlük depolama için bir müşteri ayarlanırsa veya blob depolama alanındaki kullanılabilir uygulama için oluşturulan ağ paylaşımı konumu üzerinde kullanılabilir. İkinci seçeneği, büyük miktarda ağ paylaşımıyla ilişkili dosya depolama sınırları aşma riski olmadan toplanması günlükleri sağlar.

Benzer bir damarlı .NET uygulamalarından gerçek zamanlı tanılama bilgilerini de .NET izleme ve tanılama altyapısı, uygulamanın ağ paylaşımı için izleme bilgilerini yazmak için seçenekleri kullanarak kaydedilebilir veya alternatif olarak bir blob depolama konumuna.

Uygulamaları için kullanılabilir olmayan günlüğe kaydetme ve izleme tanılama alanları şunlardır: Windows ETW olayları ve ortak Windows olay günlükleri (örneğin, sistem, uygulama ve güvenlik olay günlükleri). ETW İzleme bilgilerini (sağ ACL'ler ile) görüntülenebilir makineye potansiyel olarak olamayacağından, okuma ve yazma erişimi için ETW olayları engellenir. Geliştiricilerin başarılı olması için görünecekleri API okuma ve ETW olayları ve ortak Windows olay günlüklerini çalışmıyor görünüyor, ancak App Service "çağrıları faking çünkü" yazma çağrılarının olduğunu fark edebilirsiniz. Gerçekte, uygulama kodu bu olay verilerine erişim yok.

<a id="RegistryAccess"></a>

## <a name="registry-access"></a>Kayıt defteri erişimi
Uygulamanız salt okunur erişim sağlayan (tümüyle olmasa da) runtime'daki sanal makinesinin kayıt defteri. Uygulamada, bu uygulamaları tarafından erişilebilir bir yerel kullanıcı grubu salt okunur erişime izin vermek kayıt defteri anahtarlarını anlamına gelir. Bir okuma veya yazma erişimi için şu anda desteklenmiyor kayıt defteri alanıdır HKEY\_geçerli\_kullanıcı hive.

Herhangi bir kullanıcı başına kayıt defteri anahtarı erişim dahil olmak üzere kayıt defteri yazma erişimi engellenir. Uygulamalar (yapıp bu yana) uygulama açısından bakıldığında, kayıt defterine yazma erişimi hiçbir zaman bağlı Azure ortamında dayanan farklı sanal makineler arasında geçirilen. Bir uygulama tarafından bir işleve bağımlı yalnızca kalıcı yazılabilir depolama, uygulama hizmeti UNC paylaşımlarına depolanmış uygulama başına içerik dizini yapısıdır. 

## <a name="remote-desktop-access"></a>Uzak Masaüstü erişimi

App Service, sanal makine örneklerine Uzak Masaüstü erişimi sağlamaz.

## <a name="more-information"></a>Daha fazla bilgi

[Azure App Service korumalı](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) -App Service'in yürütme ortamı hakkında en güncel bilgileri. Bu sayfa, doğrudan App Service geliştirme ekibi tarafından korunur.

