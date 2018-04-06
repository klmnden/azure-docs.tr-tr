---
title: Microsoft azure'da bulut Foundry ilk uygulamanızı dağıtma | Microsoft Docs
description: Bulut Foundry azure'da bir uygulamayı dağıtma
services: virtual-machines-linux
documentationcenter: ''
author: seanmck
manager: jeconnoc
editor: ''
tags: ''
keywords: ''
ms.assetid: 8fa04a58-56ad-4e6c-bef4-d02c80d4b60f
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/14/2017
ms.author: seanmck
ms.openlocfilehash: 5e7b321c9fc8f8568cd8109cea0ae877048d3663
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="deploy-your-first-app-to-cloud-foundry-on-microsoft-azure"></a>Microsoft azure'da bulut Foundry ilk uygulamanızı dağıtma

[Bulut Foundry](http://cloudfoundry.org) popüler açık kaynak uygulama platformu Microsoft Azure üzerinde kullanılabilir. Bu makalede, dağıtmak ve uygulamayı bulut Foundry üzerinde bir Azure ortamı yönetmek nasıl gösterir.

## <a name="create-a-cloud-foundry-environment"></a>Bir bulut Foundry ortam oluşturma

Azure üzerinde bir bulut Foundry ortamı oluşturmak için birkaç seçenek vardır:

- Kullanım [Bileşendirler bulut Foundry teklif] [ pcf-azuremarketplace] PCF Ops Manager ve Azure hizmet aracısı içeren standart bir ortam oluşturmak için Azure Market'te. Bulabileceğiniz [tamamlamak yönergeleri] [ pcf-azuremarketplace-pivotaldocs] Market dağıtmak için Bileşendirler belgelerde sunar.
- Tarafından özelleştirilmiş bir ortam oluşturmak [Bileşendirler bulut Foundry el ile dağıtma][pcf-custom].
- [Açık kaynak bulut Foundry paketlerini doğrudan dağıtma] [ oss-cf-bosh] ayarıyla bir [BOSH](http://bosh.io) director, bulut Foundry ortamı dağıtımını koordine eden bir VM.

> [!IMPORTANT] 
> Azure Marketi'nden PCF dağıtıyorsanız, SYSTEMDOMAINURL ve her ikisi de Market Dağıtım Kılavuzu'nda açıklanan Bileşendirler uygulamaları Yöneticisi'ne erişmek için gereken yönetici kimlik bilgilerini not edin. Bu öğreticiyi tamamlamak için gereklidir. Market dağıtımları için SYSTEMDOMAINURL biçimindedir https://system. *IP adresi*. cf.pcfazure.com.

## <a name="connect-to-the-cloud-controller"></a>Bulut denetleyicisine bağlanma

Bulut denetleyicisi uygulamaları dağıtma ve yönetme için bir bulut Foundry ortam için birincil giriş noktasıdır. Çekirdek bulut denetleyicisi API (CCAPI), REST API olmakla birlikte çeşitli araçları üzerinden erişilebilir. Bu durumda, biz üzerinden etkileşimde [bulut Foundry CLI][cf-cli]. Linux, MacOS veya Windows CLI yükleyebilirsiniz, ancak bunu hiç yüklemeyi tercih ediyorsanız, önceden yüklenmiş kullanılabilir [Azure bulut Kabuk][cloudshell-docs].

Oturum açmak için başına `api` Market dağıtımından elde SYSTEMDOMAINURL için. Varsayılan dağıtım otomatik olarak imzalanan bir sertifika kullandığından, da bulundurmalısınız `skip-ssl-validation` geçin.

```bash
cf login -a https://api.SYSTEMDOMAINURL --skip-ssl-validation
```

Bulut Denetleyicisi oturum istenir. Market dağıtım yer alan adımları aldığınız Yönetici hesap kimlik bilgilerini kullanın.

Bulut Foundry sağlar *düzenlemeler* ve *alanları* ekipleri ve ortamlar paylaşılan bir dağıtım içinde yalıtmak için ad alanları olarak. Varsayılan PCF Market dağıtımının *sistem* org ve alanları temel bileşenler içerecek şekilde oluşturulmuş bir dizi gibi otomatik ölçeklendirmeyi hizmeti ve Azure hizmet Aracısı. Şimdilik, seçin *sistem* alanı.


## <a name="create-an-org-and-space"></a>Bir kuruluş oluşturup alanı

Yazarsanız, `cf apps`, sistem Krlş. içinde sistem alanında dağıtılan sistem uygulamaları bakın 

Bulundurmanız gereken *sistem* sistem uygulamaları için ayrılmış org dolayısıyla oluşturun org ve örnek uygulamamız barındırmak için alanı.

```bash
cf create-org myorg
cf create-space dev -o myorg
```

Yeni org ve alan geçiş yapmak için hedef komutu kullanın:

```bash
cf target -o testorg -s dev
```

Şimdi, bir uygulamayı dağıttığınızda, otomatik olarak yeni org ve alanı oluşturulur. Olduğunu şu anda hiçbir uygulamanın yeni org/alan doğrulamak için şunu yazın `cf apps` yeniden.

> [!NOTE] 
> Düzenlemeler ve alanları ve rol tabanlı erişim denetimi (RBAC) bunlar nasıl kullanılabileceği hakkında daha fazla bilgi için bkz: [bulut Foundry belgelerine][cf-orgs-spaces-docs].

## <a name="deploy-an-application"></a>Uygulama dağıtma

Merhaba yay Java'da yazılmış ve temel bulut adlı örnek bir bulut Foundry uygulama kullanalım [yay Framework](http://spring.io) ve [yay önyükleme](http://projects.spring.io/spring-boot/).

### <a name="clone-the-hello-spring-cloud-repository"></a>Merhaba yay bulut depoyu kopyalayın

Merhaba yay bulut örnek uygulama, GitHub üzerinde kullanılabilir. Ortamınız için kopyalama ve yeni dizine değiştirin:

```bash
git clone https://github.com/cloudfoundry-samples/hello-spring-cloud
cd hello-spring-cloud
```

### <a name="build-the-application"></a>Uygulama oluşturma

Uygulamasını kullanarak yapı [Apache Maven](http://maven.apache.org).

```bash
mvn clean package
```

### <a name="deploy-the-application-with-cf-push"></a>Uygulama cf itme ile dağıtma

Bulut Foundry kullanmanın çoğu uygulamaları dağıtabilirsiniz `push` komutu:

```bash
cf push
```

Olduğunda, *itme* bir uygulama, bulut Foundry uygulamada (Bu durumda, Java uygulaması) türünü algılar ve bağımlılıklarını (Bu durumda, yay framework) tanımlar. Bunun ardından olarak bilinen bir tek başına kapsayıcı görüntüsü kodunuzu çalıştırmak için gereken her şeyi paketleri bir *Damlacık*. Son olarak, bulut Foundry ortamınızdaki kullanılabilir makineleri birindeki uygulama zamanlar ve size, komut çıktısında kullanılabilir olduğu ulaşabilecekleri bir URL oluşturur.

![Cf itme komut çıktısı][cf-push-output]

Merhaba yay bulut uygulaması görmek için sağlanan URL'nin tarayıcınızda açın:

![Merhaba yay bulut için varsayılan kullanıcı Arabirimi][hello-spring-cloud-basic]

> [!NOTE] 
> Sırasında neler olduğu hakkında daha fazla bilgi için `cf push`, bkz: [uygulamaları nasıl hazırlanır] [ cf-push-docs] bulut Foundry belgelerinde.

## <a name="view-application-logs"></a>Uygulama günlüklerini görüntüle

Bulut Foundry CLI adını kullanarak bir uygulama için günlükleri görüntülemek için kullanabilirsiniz:

```bash
cf logs hello-spring-cloud
```

Varsayılan olarak, günlükler kullanan komut *tail*, yazıldığı gibi yeni günlükler gösterir. Yeni günlükler görünür görmek için tarayıcıda hello yay bulut uygulama yenileyin.

Zaten yazılmış günlükleri görüntülemek için add `recent` geçin:

```bash
cf logs --recent hello-spring-cloud
```

## <a name="scale-the-application"></a>Uygulama ölçeklendirme

Varsayılan olarak, `cf push` yalnızca uygulamanızın tek bir örneğini oluşturur. Yüksek kullanılabilirlik sağlamak ve ölçek genişletme için daha yüksek verimlilik etkinleştirmek için genellikle uygulamalarınızı birden fazla örneğini çalıştıran istersiniz. Zaten dağıtılmış uygulamalarının kullanarak kolayca ölçeklendirebilirsiniz `scale` komutu:

```bash
cf scale -i 2 hello-spring-cloud
```

Çalışan `cf app` uygulama komutunda gösterir bulut Foundry uygulama başka bir örneği oluşturmaktır. Uygulama başlatıldıktan sonra bulut Foundry Yük Dengeleme trafiğini onu otomatik olarak başlar.


## <a name="next-steps"></a>Sonraki adımlar

- [Bulut Foundry belgeleri okuyun][cloudfoundry-docs]
- [Visual Studio Team Services eklentisi bulut Foundry için ayarlama][vsts-plugin]
- [Microsoft günlük analizi kafa bulut Foundry için yapılandırma][loganalytics-nozzle]

<!-- LINKS -->

[pcf-azuremarketplace]: https://azuremarketplace.microsoft.com/marketplace/apps/pivotal.pivotal-cloud-foundry
[pcf-custom]: https://docs.pivotal.io/pivotalcf/1-10/customizing/azure.html
[oss-cf-bosh]: https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs
[pcf-azuremarketplace-pivotaldocs]: https://docs.pivotal.io/pivotalcf/customizing/pcf_azure.html
[cf-cli]: https://github.com/cloudfoundry/cli
[cloudshell-docs]: https://docs.microsoft.com/azure/cloud-shell/overview
[cf-orgs-spaces-docs]: https://docs.cloudfoundry.org/concepts/roles.html
[spring-boot]: https://projects.spring.io/spring-boot/
[spring-framework]: http://spring.io
[cf-push-docs]: https://docs.cloudfoundry.org/concepts/how-applications-are-staged.html
[cloudfoundry-docs]: https://docs.cloudfoundry.org
[vsts-plugin]: https://github.com/Microsoft/vsts-cloudfoundry
[loganalytics-nozzle]: https://github.com/Azure/oms-log-analytics-firehose-nozzle

<!-- IMAGES -->
[cf-push-output]: ./media/cloudfoundry-deploy-your-first-app/cf-push-output.png
[hello-spring-cloud-basic]: ./media/cloudfoundry-deploy-your-first-app/hello-spring-cloud-basic.png