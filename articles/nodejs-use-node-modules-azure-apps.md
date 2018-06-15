---
title: Node.js modülleriyle çalışma
description: Azure uygulama hizmeti veya Bulut hizmetlerini kullanırken node.js modüllerini ile çalışmayı öğrenin.
services: ''
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: ''
ms.assetid: c0e6cd3d-932d-433e-b72d-e513e23b4eb6
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: 76679ea0ff2c1e88d1923488717a245351437165
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23864059"
---
# <a name="using-nodejs-modules-with-azure-applications"></a>Azure uygulamalarıyla Node.js Modüllerini kullanma
Bu belge, Azure üzerinde barındırılan uygulamalarıyla Node.js modüllerini kullanma yönergeleri sağlar. Ayrıca, uygulamanızda modülün belirli bir sürümünün kullanıldığından nasıl emin olacağınız ve Azure'la yerel modülleri nasıl kullanacağınız da anlatılır.

Node.js modüllerini kullanma ile bilginiz varsa **package.json** ve **npm shrinkwrap.json** dosyaları, aşağıdaki bilgileri bu makalede ele alınan kısa bir Özet sağlar:

* Azure uygulama hizmeti anlar **package.json** ve **npm shrinkwrap.json** dosyaları ve bu dosyalardaki girişleri temel modülleri yükleyebilirsiniz.

* Azure bulut Hizmetleri beklediğiniz geliştirme ortamında, yüklü olan tüm modülleri ve **düğümü\_modülleri** dağıtım paketinin bir parçası olarak dahil olacak şekilde dizini. Kullanarak modüllerini yüklemeden desteğini etkinleştirmek mümkündür **package.json** veya **npm shrinkwrap.json** dosyalarını bulut Hizmetleri; ancak, bu yapılandırma varsayılan özelleştirmesini gerektirir Bulut hizmeti projeleri tarafından kullanılan komutlar. Bu ortam yapılandırma konusunda bir örnek için bkz: [düğümü modülleri dağıtma önlemek için npm yükleme çalıştırmak için Azure başlangıç görevi](https://github.com/woloski/nodeonazure-blog/blob/master/articles/startup-task-to-run-npm-in-azure.markdown)

> [!NOTE]
> VM Dağıtım deneyimi sanal makine tarafından barındırılan işletim sistemine bağlı olarak bu makalede, azure sanal makineleri açıklanmamaktadır.
> 
> 

## <a name="nodejs-modules"></a>Node.js modüllerini
Modüller, uygulamanız için belirli işlevler sağlayan yüklenebilir JavaScript paketlerdir. Modüller, kullanarak genellikle yüklenir **npm** komut satırı aracı, ancak bazı modüller (örneğin, http Modülü) çekirdek Node.js paketinin bir parçası sağlanır.

Modülleri yüklendiğinde depolandıkları **düğümü\_modülleri** uygulama dizin yapısının kökü, dizin. İçinde her modül **düğümü\_modülleri** directory tutar kendi **düğümü\_modülleri** bağlıdır ve bu davranış yinelenir tüm modülleri içeren dizini Her bir modüle tüm bağımlılık zinciri kapalı. Bu ortamda, ancak bu oldukça büyük dizin yapısında sonuçlanabilir, bağlı olduğu modülleri kendi sürüm gereksinimleri yüklü modüllerin sağlar.

Dağıtma **düğümü\_modülleri** uygulamanızın parçası kullanmaya karşılaştırıldığında dağıtımın boyutu arttıkça dizin bir **package.json** veya  **npm shrinkwrap.json** dosya; üretimde kullanılan modülleri sürümleri geliştirme kullanılan modülleri aynı olduğunu ancak garanti etmez.

### <a name="native-modules"></a>Yerel modülleri
Çoğu Modüller yalnızca düz metin JavaScript dosyaları olsa da, bazı platforma özgü ikili görüntülerini modüllerdir. Bu modüller yükleme zamanında Python ve düğüm gyp kullanarak genellikle derlenir. Azure Cloud Services bağlı olduğundan **düğümü\_modülleri** yüklü olduğu sürece, klasör yüklü modülleri bir parçası olarak dahil edilen uygulama, tüm yerel modül bir parçası olarak dağıtılan bir bulut hizmetinde iş ve bir Windows geliştirme sisteminde derlenmiş.

Azure uygulama hizmeti, tüm yerel modülleri desteklemez ve belirli önkoşulları modüllerle derlerken başarısız olabilir. MongoDB gibi bazı popüler modülleri isteğe bağlı yerel bağımlılıkları vardır ve onlar olmadan uygundur olsa da, iki geçici çözüm bugün neredeyse tüm yerel modülleri ile kullanılabilir başarılı oluyor uygulamasına yol açıyordu:

* Çalıştırma **npm yükleme** bir Windows makinesinde yüklü tüm yerel modülün önkoşulları vardır. Sonra oluşturulan dağıtmak **düğümü\_modülleri** klasörü uygulamanın Azure App Service'e bir parçası olarak.

  * Derleme önce yerel Node.js yüklemenizi eşleşen mimariye sahip ve sürüm mümkün olduğunca yakın bir Azure içinde kullanılan olup olmadığını denetleyin (geçerli değerler çalışma zamanında özelliklerinden denetlenebilir **process.arch** ve **process.version**).

* Azure uygulama hizmeti yapılandırılabilir özel bash veya kabuk betikleri dağıtımı sırasında yürütülecek özel komutları yürütün ve tam olarak şekilde yapılandırmak için fırsat vermiş **npm yükleme** çalıştırılıyor. Bu ortamda yapılandırmak nasıl gösteren bir video için bkz: [özel Web sitesi dağıtım betikleri Kudu ile].

### <a name="using-a-packagejson-file"></a>Bir package.json dosyası kullanma

**Package.json** dosyasıdır uygulamanızın gerektirdiği barındırma platformuna eklemenizi gerektirmek yerine bağımlılıkları yükleyebilmek için en üst düzey bağımlılıkları belirtmek için bir yol **düğümü\_ modülleri** klasörü dağıtımının bir parçası olarak. Uygulama dağıtıldıktan sonra **npm yükleme** ayrıştırmak için kullanılan komut **package.json** dosya ve listelenen tüm bağımlılıkları yükler.

Geliştirme sırasında kullandığınız **--Kaydet**, **--Kaydet geliştirme**, veya **--Kaydet isteğe bağlı** , Modüliçinbirgirişeklemekiçinmodülleriyüklerkenparametreleri**package.json** dosya otomatik olarak. Daha fazla bilgi için bkz: [npm yükleme](https://docs.npmjs.com/cli/install).

Olası bir sorun **package.json** dosyadır, yalnızca üst düzey bağımlılıklar için sürümünü belirtir. Her modül yüklü olabilir veya bağımlı modülleri sürümü belirtemezsiniz ve bu nedenle, farklı bir bağımlılığı geliştirme kullanılan fazla ile anlamayabilir olduğunu mümkündür.

> [!NOTE]
> Azure App Service'e dağıtırken yüklendiğinde, <b>package.json</b> dosyaya başvuruda bulunan bir yerel modül Git kullanarak uygulama yayımlama sırasında aşağıdaki örneğe benzer bir hata görebilirsiniz:
> 
> npm hata! module-name@0.6.0yükleyin: 'düğümü gyp Yapılandır yapı'
> 
> npm hata! ' cmd "/ c" "düğümü gyp yapılandırma derleme" ' 1 ile başarısız oldu
> 
> 

### <a name="using-a-npm-shrinkwrapjson-file"></a>Bir npm shrinkwrap.json dosyası kullanma
**Npm shrinkwrap.json** dosyasıdır modülü sürüm oluşturma sınırlamaları adres girişimi **package.json** dosya. Sırada **package.json** dosyası, yalnızca üst düzey modüller için sürümleri içerir **npm shrinkwrap.json** dosyası tam modül bağımlılık zinciri sürüm gereksinimlerini içerir.

Uygulamanızın üretime hazır olduğunda, kilitleme sürüm gereksinimlerini ve oluşturma bir **npm shrinkwrap.json** kullanarak dosya **npm shrinkwrap** komutu. Bu komut şu anda yüklü sürümleri kullanacağı **düğümü\_modülleri** klasörünü ve bu sürümleri için kayıt **npm shrinkwrap.json** dosya. Uygulama barındırma ortamına dağıtıldıktan sonra **npm yükleme** ayrıştırmak için kullanılan komut **npm shrinkwrap.json** dosya ve listelenen tüm bağımlılıkları yükler. Daha fazla bilgi için bkz: [npm shrinkwrap](https://docs.npmjs.com/cli/shrinkwrap).

> [!NOTE]
> Azure App Service'e dağıtırken yüklendiğinde, <b>npm shrinkwrap.json</b> dosyaya başvuruda bulunan bir yerel modül Git kullanarak uygulama yayımlama sırasında aşağıdaki örneğe benzer bir hata görebilirsiniz:
> 
> npm hata! module-name@0.6.0yükleyin: 'düğümü gyp Yapılandır yapı'
> 
> npm hata! ' cmd "/ c" "düğümü gyp yapılandırma derleme" ' 1 ile başarısız oldu
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Azure ile Node.js modüllerini kullanma anladığınıza göre bilgi nasıl [Node.js sürümünü belirtme], [derleme ve Node.js web uygulamasına dağıtma](app-service/app-service-web-get-started-nodejs.md), ve [Azure komut satırı kullanma Mac ve Linux için arabirimi].

Daha fazla bilgi için bkz. [Node.js Geliştirici Merkezi](/nodejs/azure/).

[Node.js sürümünü belirtme]: nodejs-specify-node-version-azure-apps.md
[Azure komut satırı kullanma Mac ve Linux için arabirimi]:cli-install-nodejs.md
[özel Web sitesi dağıtım betikleri Kudu ile]: https://channel9.msdn.com/Shows/Azure-Friday/Custom-Web-Site-Deployment-Scripts-with-Kudu-with-David-Ebbo
