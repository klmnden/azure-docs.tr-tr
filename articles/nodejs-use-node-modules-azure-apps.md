---
title: Node.js modülleriyle çalışma
description: Azure App Service veya Cloud Services'ı kullanırken, node.js modülleri ile çalışmayı öğrenin.
services: ''
documentationcenter: nodejs
author: rloutlaw
manager: rloutlaw
editor: ''
ms.assetid: c0e6cd3d-932d-433e-b72d-e513e23b4eb6
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: routlaw
ms.openlocfilehash: 571e8d640e068b6635ab4091a01283d698b0264d
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67595660"
---
# <a name="using-nodejs-modules-with-azure-applications"></a>Azure uygulamalarıyla Node.js Modüllerini kullanma
Bu belgede, Azure'da barındırılan uygulamalarla Node.js modüllerini kullanma hakkında bir Rehber sağlanır. Bu, uygulamanızın bir modülün belirli bir sürümünü kullandığını sağlama yanı sıra Azure'la yerel modülleri kullanma hakkında yönergeler sağlanır.

Zaten Node.js modüllerini kullanma ile ilgili bilgi sahibi olduğunuz **package.json** ve **npm shrinkwrap.json** dosyaları, aşağıdaki bilgileri bu makalede ele alınan, kısa bir Özet sağlar:

* Azure App Service'ı anlayan **package.json** ve **npm shrinkwrap.json** dosyaları ve bu dosyalardaki girdileri temel alan modüller yükleyebilirsiniz.

* Azure bulut Hizmetleri, tüm modülleri geliştirme ortamınızda yüklü olmasını bekliyor ve **düğüm\_modülleri** dağıtım paketinin bir parçası olarak dahil edilmesi için dizin. Kullanarak modülleri yükleme desteği etkinleştirmek mümkündür **package.json** veya **npm shrinkwrap.json** dosyaları Cloud Services; ancak, bu yapılandırma varsayılan özelleştirme gerektirir betikleri bulut hizmeti projeleri tarafından kullanılır. Örneği bu ortamın nasıl yapılandıracağınızı öğrenmek için bkz: [node modülleri dağıtma önlemek için npm yükleme çalıştırmak için Azure başlangıç görevi](https://github.com/woloski/nodeonazure-blog/blob/master/articles/startup-task-to-run-npm-in-azure.markdown)

> [!NOTE]
> Bir VM'deki dağıtım deneyimi sanal makine tarafından barındırılan işletim sistemine bağımlı olduğu gibi bu makalede, azure sanal makineleri ele alınmamaktadır.
> 
> 

## <a name="nodejs-modules"></a>Node.js modüllerini
Uygulamanız için belirli işlevleri sağlayan yüklenebilir JavaScript paketleri modüllerdir. Modüller, kullanarak genellikle yüklenir **npm** komut satırı aracı, ancak bazı modüller (örneğin, http Modülü) çekirdek Node.js paketinin bir parçası sağlanır.

Modüller yüklendiğinde depolandıkları **düğüm\_modülleri** uygulama directory yapınızın kök dizin. İçinde her modül **düğüm\_modülleri** directory bağımlı olduğu ve bu davranışı için her bir modüle bağimlılık zincirindeki tüm aşağı yineler modüllerin içeren kendi dizin tutar. Bu ortam, ancak bunu oldukça büyük bir dizin yapısına neden olabilir, bağlı kendi sürüm gereksinimleri modüller için yüklü her bir modül sağlar.

Dağıtımı **düğüm\_modülleri** uygulamanızın bölümünü kullanarak karşılaştırıldığında dağıtım boyutu arttıkça dizin bir **package.json** veya  **npm shrinkwrap.json** dosya; ancak, üretim ortamında kullanılan modüller sürümleri geliştirmede kullanılan modülleri ile aynı olduğunu garanti etmez.

### <a name="native-modules"></a>Yerel modülleri
Modüllerinin çoğu yalnızca düz metin JavaScript dosyaları olsa da, bazı modüller platforma özgü ikili görüntüleridir. Bu modüller Python ve düğüm gyp kullanarak yükleme zamanında genellikle derlenir. Azure Cloud Services kullanan bu yana **düğüm\_modülleri** yüklü olduğu sürece, klasör yüklü modülleri bir parçası olarak dahil edilen uygulama, herhangi bir yerel modül bir parçası olarak dağıtılan bir bulut hizmetinde çalışmalıdır ve Windows geliştirme sistemindeki derlenmiş.

Azure App Service, tüm yerel modülleri desteklemez ve modülleri belirli önkoşulları ile derleme yapılırken başarısız olabilir. Neredeyse tüm yerel modülleri ile kullanılabilir iki geçici çözüm bugün MongoDB gibi bazı popüler modülleri isteğe bağlı yerel bağımlılıkları vardır ve bunlar olmadan işe gösterdi:

* Çalıştırma **npm yükleme** bir Windows makinede yüklü olan tüm yerel modülün önkoşulları vardır. Ardından, oluşturulan dağıtın **düğüm\_modülleri** uygulamasını Azure App Service'e bir parçası olarak klasörü.

  * Derleme öncesinde, yerel Node.js yüklemenizi eşleşen bir mimariye sahiptir ve sürüm mümkün olduğunca yakın bir Azure'da kullanılan olduğunu kontrol edin (geçerli değerler, çalışma zamanı özelliklerinden üzerinde denetlenebilir **process.arch** ve **process.version**).

* Azure App Service yapılandırılabilir dağıtımı sırasında özel bash veya kabuk betikleri çalıştırmak için özel komutları yürütmek ve kesin şekilde yapılandırmak için fırsatı veren **npm yükleme** çalıştırılıyor. Bu ortam yapılandırma gösteren bir video için bkz [Kudu ile özel Web sitesi dağıtım betikleri](https://azure.microsoft.com/resources/videos/custom-web-site-deployment-scripts-with-kudu/).

### <a name="using-a-packagejson-file"></a>Bir package.json dosyası kullanma

**Package.json** dosyasıdır barındırma platformu eklemenizi gerektirmek yerine bağımlılıklarını yükleyebilirsiniz. böylece uygulamanızın gerektirdiği en üst düzey bağımlılıklarını belirtmek için bir yol **düğüm\_ Modüller** klasör dağıtımının bir parçası olarak. Uygulama dağıtıldıktan sonra **npm yükleme** ayrıştırmak için kullanılan komut **package.json** dosya ve listelenen tüm bağımlılıkları yükler.

Geliştirme sırasında kullandığınız **--Kaydet**, **--Kaydet geliştirme**, veya **--Kaydet isteğe bağlı** modüller için modülüiçingirişeklemekiçinyüklerkenparametreleri**package.json** otomatik olarak dosya. Daha fazla bilgi için [npm yükleme](https://docs.npmjs.com/cli/install).

Olası bir sorun **package.json** dosyasıdır, yalnızca üst düzey bağımlılıkları için sürümünü belirtir. Her Modülü yüklü olabilir veya bağımlı modülleri sürümünü belirtemezsiniz ve bu nedenle, bir geliştirme kullanılan farklı bağımlılık zinciri ile bitirebilirsiniz olduğunu mümkündür.

> [!NOTE]
> Azure App Service'e, dağıtırken, <b>package.json</b> dosyasına başvuran bir yerel modül Git kullanarak uygulamayı yayımlarken aşağıdaki örneğe benzer bir hata görebilirsiniz:
> 
> npm hata! module-name@0.6.0 Yükleme: 'yapı düğümü gyp'Yapılandır '
> 
> npm hata! ' cmd "/ c" "düğümü gyp Yapılandır derleme" ' 1 ile başarısız oldu
> 
> 

### <a name="using-a-npm-shrinkwrapjson-file"></a>Bir npm shrinkwrap.json dosyası kullanma
**Npm shrinkwrap.json** dosyasıdır modülü sürüm oluşturma sınırlamaları adres girişimi **package.json** dosya. Sırada **package.json** dosyası, yalnızca üst düzey modüller için sürümlerini içerir **npm shrinkwrap.json** dosyası tam modül bağımlılık zinciri sürüm gereksinimlerini içerir.

Uygulamanızı üretime hazır olduğunda, sürüm gereksinimleri kilitlemek ve oluşturma bir **npm shrinkwrap.json** kullanarak dosya **npm shrinkwrap** komutu. Bu komut şu anda yüklü sürümlerini kullanacaktır **düğüm\_modülleri** klasörünü ve bu sürümleri için kayıt **npm shrinkwrap.json** dosya. Uygulamanın barındırma ortamına dağıtıldıktan sonra **npm yükleme** ayrıştırmak için kullanılan komut **npm shrinkwrap.json** dosya ve listelenen tüm bağımlılıkları yükler. Daha fazla bilgi için [npm shrinkwrap](https://docs.npmjs.com/cli/shrinkwrap).

> [!NOTE]
> Azure App Service'e, dağıtırken, <b>npm shrinkwrap.json</b> dosyasına başvuran bir yerel modül Git kullanarak uygulamayı yayımlarken aşağıdaki örneğe benzer bir hata görebilirsiniz:
> 
> npm hata! module-name@0.6.0 Yükleme: 'yapı düğümü gyp'Yapılandır '
> 
> npm hata! ' cmd "/ c" "düğümü gyp Yapılandır derleme" ' 1 ile başarısız oldu
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Azure ile Node.js modüllerini kullanma anladığınıza göre bilgi nasıl [Node.js sürümünü belirtme](https://github.com/squillace/staging/blob/master/articles/nodejs-specify-node-version-azure-apps.md), [bir Node.js web uygulaması derleyip dağıtacaksınız](app-service/app-service-web-get-started-nodejs.md), ve [Azure komut satırı kullanma Mac ve Linux için arabirimi](https://azure.microsoft.com/blog/using-windows-azure-with-the-command-line-tools-for-mac-and-linux/).

Daha fazla bilgi için bkz. [Node.js Geliştirici Merkezi](/nodejs/azure/).

[specify the Node.js version]: nodejs-specify-node-version-azure-apps.md
[How to use the Azure Command-Line Interface for Mac and Linux]:cli-install-nodejs.md
[Custom Website Deployment Scripts with Kudu]: https://channel9.msdn.com/Shows/Azure-Friday/Custom-Web-Site-Deployment-Scripts-with-Kudu-with-David-Ebbo
