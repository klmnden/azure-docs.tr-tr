---
title: Microsoft Azure üzerinde Cloud Foundry için ilk uygulamanızı dağıtma | Microsoft Docs
description: Azure'da Cloud Foundry uygulama dağıtma
services: virtual-machines-linux
documentationcenter: ''
author: seanmck
manager: gwallace
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
ms.openlocfilehash: fe510865e687b6a44538627e4ef9025b41416841
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67668340"
---
# <a name="deploy-your-first-app-to-cloud-foundry-on-microsoft-azure"></a>Microsoft Azure üzerinde Cloud Foundry için ilk uygulamanızı dağıtma

[Cloud Foundry](https://cloudfoundry.org) popüler açık kaynak uygulama platformunu Microsoft Azure'da kullanıma sunuldu. Bu makalede, dağıtmak ve Cloud Foundry üzerinde bir uygulamayı Azure ortamınızda yönetmek nasıl göstereceğiz.

## <a name="create-a-cloud-foundry-environment"></a>Cloud Foundry ortamı oluşturma

Azure'da Cloud Foundry ortamı oluşturmak için birkaç seçenek vardır:

- Kullanım [Pivotal Cloud Foundry teklif][pcf-azuremarketplace] in the Azure Marketplace to create a standard environment that includes PCF Ops Manager and the Azure Service Broker. You can find [complete instructions][pcf-azuremarketplace-pivotaldocs] Market dağıtmak için Pivotal belgelerinde sunar.
- Özelleştirilmiş bir ortamı tarafından oluşturma [el ile Pivotal Cloud Foundry dağıtma][pcf-custom].
- [Açık kaynaklı Cloud Foundry paketlerini doğrudan dağıtma][oss-cf-bosh] oluşturarak bir [BOSH](https://bosh.io) Direktörü, Cloud Foundry ortamın dağıtımı koordine eden bir VM.

> [!IMPORTANT] 
> Azure Market'ten PCF dağıtıyorsanız, SYSTEMDOMAINURL ve ikisi için de Market Dağıtım Kılavuzu'nda açıklanan Pivotal uygulamaları Yöneticisi'ne erişmek için gereken yönetici kimlik bilgilerini not edin. Bu öğreticiyi tamamlamak için gereklidir. Market dağıtımları için SYSTEMDOMAINURL biçimindedir https://system. *IP adresi*. cf.pcfazure.com.

## <a name="connect-to-the-cloud-controller"></a>Bulut denetleyiciyi bağlama

Bulutu denetleyicisi, Cloud Foundry ortamına dağıtmak ve uygulamaları yönetmek için birincil giriş noktasıdır. Çekirdek bulut denetleyicisi API (CCAPI) REST API, ancak çeşitli araçlar üzerinden erişilebilir. Bu durumda, biz üzerinden etkileşimde [Cloud Foundry CLI][cf-cli]. You can install the CLI on Linux, MacOS, or Windows, but if you'd prefer not to install it at all, it is available pre-installed in the [Azure Cloud Shell][cloudshell-docs].

Oturum açmak için önüne ekleyin `api` Market dağıtımından elde ettiğiniz SYSTEMDOMAINURL için. Varsayılan dağıtım otomatik olarak imzalanan bir sertifika kullandığından, aynı zamanda içermelidir `skip-ssl-validation` geçin.

```bash
cf login -a https://api.SYSTEMDOMAINURL --skip-ssl-validation
```

Oturum açmak için bulut denetleyici istenir. Market dağıtım adımları aldığınız yönetici hesabı kimlik bilgilerini kullanın.

Cloud Foundry sağlar *düzenlemeler* ve *alanları* ekipler ve ortamlar paylaşılan bir dağıtım içinde yalıtmak için ad alanları olarak. PCF Market dağıtım varsayılan içerir *sistem* kuruluş ve bir dizi temel bileşenler içerecek şekilde oluşturulan alanları otomatik ölçeklendirme hizmeti ve Azure hizmet aracısı gibi. Şimdilik seçin *sistem* alanı.


## <a name="create-an-org-and-space"></a>Bir kuruluş oluşturup alanı

Yazarsanız `cf apps`, bir dizi sistem Krlş. içinde sistem alanında dağıtılan sistem uygulamaları bakın 

Tutmanız gerekir *sistem* kuruluş sistem uygulamaları için ayrılmış şekilde bir kuruluş ve örnek uygulamamız barındırmak için alanı oluşturma.

```bash
cf create-org myorg
cf create-space dev -o myorg
```

Alan ve yeni bir kuruluş için geçiş yapmak için hedef komutu kullanın:

```bash
cf target -o testorg -s dev
```

Artık, bir uygulamayı dağıttığınızda, otomatik olarak yeni bir kuruluş ve alanı oluşturulur. Şu anda hiçbir uygulamaların olduğuna yeni kuruluş/alanı doğrulamak için şunu yazın `cf apps` yeniden.

> [!NOTE] 
> Düzenlemeler ve alanları ve rol tabanlı erişim denetimi (RBAC), nasıl kullanılabileceği hakkında daha fazla bilgi için bkz. [Cloud Foundry belgeleri][cf-orgs-spaces-docs].

## <a name="deploy-an-application"></a>Uygulama dağıtma

Hello Spring Java dilinde yazılmış olan ve temel Cloud, adında örnek Cloud Foundry uygulama kullanalım [Spring Framework](https://spring.io) ve [Spring Boot](https://projects.spring.io/spring-boot/).

### <a name="clone-the-hello-spring-cloud-repository"></a>Hello Spring Cloud depoyu kopyalama

Hello Spring Cloud örnek uygulama, Github'da kullanılabilir. Ortamınıza kopyalayın ve yeni dizine değiştirin:

```bash
git clone https://github.com/cloudfoundry-samples/hello-spring-cloud
cd hello-spring-cloud
```

### <a name="build-the-application"></a>Uygulama oluşturma

Uygulamasını kullanarak yapı [Apache Maven](https://maven.apache.org).

```bash
mvn clean package
```

### <a name="deploy-the-application-with-cf-push"></a>Cf itme ile uygulama dağıtma

Cloud Foundry kullanmaya çoğu uygulama dağıtabileceğiniz `push` komutu:

```bash
cf push
```

Olduğunda, *anında iletme* Cloud Foundry uygulamanın türü uygulama (Bu durumda, bir uygulamada Java) algılar ve bağımlılıkları (Bu durumda, Spring framework) tanımlar. Bunun ardından olarak bilinen bir tek başına bir kapsayıcı görüntüsü kodunuzu çalıştırmak için gereken her şeyi paketleri bir *Damlacık*. Son olarak, Cloud Foundry, biri ortamınızdaki kullanılabilir makineler, uygulamayı zamanlar ve size, komut çıktısında kullanılabildiği ulaşabilecekleri bir URL oluşturur.

![Cf anında iletme komut çıktısı][cf-push-output]

Merhaba spring cloud uygulamayı görmek için belirtilen URL'yi tarayıcınızda açın:

![Merhaba Spring bulut için varsayılan kullanıcı Arabirimi][hello-spring-cloud-basic]

> [!NOTE] 
> Sırasında neler olduğu hakkında daha fazla bilgi edinmek için `cf push`, bkz: [uygulamaları nasıl hazırlanır][cf-push-docs] Cloud Foundry belgelerinde.

## <a name="view-application-logs"></a>Uygulama günlüklerini görüntüle

Cloud Foundry CLI adını kullanarak bir uygulama için günlükleri görüntülemek için kullanabilirsiniz:

```bash
cf logs hello-spring-cloud
```

Varsayılan olarak, günlükleri kullanan komut *tail*, yazıldıkları şekilde yeni günlüklerini gösterir. Görünen yeni günlüklerini görmek için tarayıcıdaki Merhaba spring cloud app yenileyin.

Zaten yazıldığını günlükleri görüntülemek için Ekle `recent` geçin:

```bash
cf logs --recent hello-spring-cloud
```

## <a name="scale-the-application"></a>Uygulama ölçeklendirme

Varsayılan olarak, `cf push` yalnızca tek bir uygulama örneği oluşturur. Yüksek kullanılabilirlik ve ölçek genişletme için daha yüksek performans sağlamak için genellikle birden fazla örneğini uygulamalarınızı çalıştırmak istersiniz. Zaten dağıtılmış uygulamalarının kullanarak kolayca ölçeklendirebilirsiniz `scale` komutu:

```bash
cf scale -i 2 hello-spring-cloud
```

Çalışan `cf app` uygulamasında komut, Cloud Foundry uygulamanın başka bir örneğini oluşturuyor gösterir. Uygulama başlatıldıktan sonra Cloud Foundry, Yük Dengeleme, trafik otomatik olarak başlar.


## <a name="next-steps"></a>Sonraki adımlar

- [Cloud Foundry belgeleri okuyun][cloudfoundry-docs]
- [Azure DevOps Services eklentisi, Cloud Foundry için ayarlayın][vsts-plugin]
- [Cloud Foundry için Microsoft Log Analytics Nozzle yapılandırın][loganalytics-nozzle]

<!-- LINKS -->

[pcf-azuremarketplace]: https://azuremarketplace.microsoft.com/marketplace/apps/pivotal.pivotal-cloud-foundry
[pcf-custom]: https://docs.pivotal.io/pivotalcf/1-10/customizing/azure.html
[oss-cf-bosh]: https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs
[pcf-azuremarketplace-pivotaldocs]: https://docs.pivotal.io/pivotalcf/customizing/pcf_azure.html
[cf-cli]: https://github.com/cloudfoundry/cli
[cloudshell-docs]: https://docs.microsoft.com/azure/cloud-shell/overview
[cf-orgs-spaces-docs]: https://docs.cloudfoundry.org/concepts/roles.html
[spring-boot]: https://projects.spring.io/spring-boot/
[spring-framework]: https://spring.io
[cf-push-docs]: https://docs.cloudfoundry.org/concepts/how-applications-are-staged.html
[cloudfoundry-docs]: https://docs.cloudfoundry.org
[vsts-plugin]: https://github.com/Microsoft/vsts-cloudfoundry
[loganalytics-nozzle]: https://github.com/Azure/oms-log-analytics-firehose-nozzle

<!-- IMAGES -->
[cf-push-output]: ./media/cloudfoundry-deploy-your-first-app/cf-push-output.png
[hello-spring-cloud-basic]: ./media/cloudfoundry-deploy-your-first-app/hello-spring-cloud-basic.png
