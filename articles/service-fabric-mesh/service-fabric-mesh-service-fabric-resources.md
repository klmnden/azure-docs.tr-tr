---
title: Azure Service Fabric kaynak modeli giriş | Microsoft Docs
description: Service Fabric Örgü uygulamalar tanımlamak için basitleştirilmiş bir yaklaşım Service Fabric kaynak modeli hakkında bilgi edinin.
services: service-fabric-mesh
documentationcenter: .net
author: vturecek
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/23/2018
ms.author: vturecek
ms.custom: mvc, devcenter
ms.openlocfilehash: 3cee0ada75c4ea265c7e9c598408eb6b01477d6c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60810749"
---
# <a name="introduction-to-service-fabric-resource-model"></a>Service Fabric kaynak modeli giriş

Service Fabric kaynak modeli, bir Service Fabric Mesh uygulaması oluşturan kaynakları tanımlamak için kolay bir yaklaşım açıklanmaktadır. Kaynakların herhangi bir Service Fabric ortama dağıtılabilir.  Service Fabric kaynak modeli, Azure Resource Manager modeli ile de uyumludur. Aşağıdaki türdeki kaynakları şu anda bu modelde desteklenir:

- Uygulamalar ve hizmetler
- Ağlar
- Ağ geçitleri
- Gizli anahtarları ve gizli anahtarları/değerleri
- Birimler

Her kaynak bildirimli olarak basit bir YAML kaynak dosyasını, veya Mesh uygulaması açıklar ve Service Fabric platform tarafından sağlanan JSON belgesinde açıklanmıştır.

## <a name="applications-and-services"></a>Uygulamalar ve hizmetler

Uygulama kaynağı dağıtım, sürüm ve Mesh uygulaması ömrünü birimidir. Bu, bir mikro hizmet temsil eden bir veya daha fazla hizmet kaynaklarını oluşur. Her hizmet kaynağı, sırasıyla bir veya daha fazla kod paket ile ilişkili kapsayıcı görüntüsünü çalıştırmak için gereken her şeyi açıklayan kod paketleri oluşur.

![Uygulamalar ve hizmetler][Image1]

Bir hizmet kaynağı aşağıdaki bildirir:

- Kapsayıcı adı, sürümü ve kayıt defteri
- Her kapsayıcı için gerekli CPU ve bellek kaynakları
- Ağ uç noktaları
- Ağları, birimleri ve gizli anahtarları gibi diğer kaynaklara başvurular 

Bir hizmet kaynağı bir parçası olarak tanımlanan tüm kod paketlerinin dağıtıldığı ve bir grup olarak birlikte etkinleştirildi. Hizmet kaynağı ayrıca kaç örneği çalıştırmak için hizmeti açıklar ve ayrıca, bağımlı diğer kaynaklar (örneğin, ağ kaynağı) başvuruyor.

Bir Mesh uygulaması birden fazla hizmet oluşuyorsa, bunlar birlikte aynı düğümde çalışan garanti edilmez. Ayrıca, uygulama bir yükseltme sırasında tek bir hizmet yükseltme hatası, önceki sürüme geri alınan tüm hizmetleri neden olur.

Daha önce alluded gibi her uygulama örneği yaşam döngüsü bağımsız olarak yönetilebilir. Örneğin, bir uygulama örneği bağımsız olarak diğer uygulama örneğinin yükseltilebilir. Yönetmek için bu duruma bir uygulamaya, daha zor yerleştirdiğiniz diğer hizmetler bağımsız olarak her servis genellikle, hizmet sayısı bir uygulamada oldukça küçük tutun.

## <a name="networks"></a>Ağlar

Ağ kaynak ayrı ayrı dağıtılabilen bağımsız olarak, bağımlılık olarak başvurduğu bir uygulama veya hizmet kaynağı kaynaktır. Uygulamalarınız için bir ağ oluşturmak için kullanılır. Birden çok farklı uygulamalar hizmetlerinden aynı ağın parçası olabilir.  Hakkında daha fazla bilgi için okuma [Service Fabric Mesh uygulamalarında ağ iletişimi](service-fabric-mesh-networks-and-gateways.md).

> [!NOTE]
> Geçerli Önizleme yalnızca uygulamaları ve ağlar arasında bire bir eşleme destekler

![Ağ ve ağ geçidi][Image2]

## <a name="gateways"></a>Ağ geçitleri
Bir ağ geçidi kaynağı, iki ağı bağlayan ve trafiği yönlendirir.  Bir ağ geçidi dış istemcilerle iletişim kurmak hizmetlerinizi ve hizmetlerinize bir giriş sağlar.  Bir ağ geçidi, kendi mevcut bir sanal ağ ile kafes uygulamanızı bağlamak için de kullanılabilir. Hakkında daha fazla bilgi için okuma [Service Fabric Mesh uygulamalarında ağ iletişimi](service-fabric-mesh-networks-and-gateways.md).

![Ağ ve ağ geçidi][Image2]

## <a name="secrets"></a>Gizli Diziler

Gizli dizileri, dağıtılabilir bağımsız olarak bir uygulama ya da ona, bağımlılık olarak yönlendirebiliriz Hizmet kaynağı kaynaklardır. Gizli dizileri, uygulamalarınız için güvenli bir şekilde sunmak için kullanılır. Birden çok farklı uygulamalar hizmetlerinden aynı gizli değerlere başvurabilirsiniz.

## <a name="volumes"></a>Birimler

Kapsayıcılar genellikle geçici diskler kullanılabilir duruma getirin. Yeni, geçici bir diskle almak ve bir kapsayıcı kilitlendiğinde bilgileri kaybedersiniz geçici diskler ancak kısa ömürlü, değildir. Ayrıca, diğer kapsayıcılarla geçici diskler hakkında bilgi paylaşmak zor olabilir. Birimler durumu kalıcı hale getirmek için kullanabileceğiniz kapsayıcı örneklerinizin içinde oluşturulmuş dizinleri listelenmiştir. Birimleri, genel amaçlı dosya depolama sağlar ve normal bir disk g/ç dosya API'lerini kullanarak dosya okuma/yazma olanak sağlar. Birim kaynağına bir dizin nasıl bağlandığını açıklamak için bildirim temelli bir yöntemini ve yedekleme depolama alanı (Azure dosya birimi veya Service Fabric güvenilir birim) için ' dir.  Daha fazla bilgi için okuma [durumu depolamak üzere](service-fabric-mesh-storing-state.md#volumes).

![Birimler][Image3]

## <a name="programming-models"></a>Programlama modelleri
Hizmet kaynağını yalnızca bu kaynakla ilgili kod paketleri içinde başvurulan çalıştırmak için kapsayıcı görüntüsü gerektirir. Bilmek gerek kalmadan kapsayıcı içinde herhangi bir çerçeveyi kullanarak herhangi bir dilde yazılmış herhangi bir kod çalıştırın ya da, Service Fabric Mesh özel API'leri kullanmak. 

Uygulama kodunuza Service Fabric bile Mesh dışında taşınabilir kalır ve uygulama dağıtımlarınızı dil veya çerçeveye hizmetlerinizi uygulamak için kullanılan bağımsız olarak tutarlı kalır. Uygulamanızı, ASP.NET Core, Git veya yalnızca işlemler ve betikler bir dizi olup Service Fabric Mesh kaynak dağıtım modeli aynı kalır. 

## <a name="packaging-and-deployment"></a>Paketleme ve dağıtım

Service Fabric Örgü uygulamalar kaynak modelini temel alan, Docker kapsayıcıları olarak paketlenir.  Service Fabric Mesh bir paylaşılan, çok kiracılı bir ortamdır ve kapsayıcılar yüksek bir yalıtım düzeyi verin.  Bu uygulamalar, JSON biçiminde veya (daha sonra JSON'a dönüştürülür) YAML biçimi kullanarak açıklanmıştır. Azure Service Fabric Mesh bir Mesh uygulamasını dağıtırken, uygulamayı tanımlamak için kullanılan JSON'u bir Azure Resource Manager şablonudur. Kaynakları Azure kaynaklarına eşlenir.  Bir Service Fabric kümesine Mesh uygulaması dağıtırken (tek başına veya Azure'da barındırılan), uygulamayı tanımlamak için kullanılan JSON için bir Azure Resource Manager şablonu benzer bir biçimidir.  Mesh uygulama dağıtıldıktan sonra HTTP arabirimleri veya Azure CLI yönetilebilir. 


## <a name="next-steps"></a>Sonraki adımlar 
Service Fabric Mesh hakkında daha fazla bilgi için genel bakış okuyun:
- [Service Fabric Mesh genel bakış](service-fabric-mesh-overview.md)

[Image1]: media/service-fabric-mesh-service-fabric-resources/AppsAndServices.png
[Image2]: media/service-fabric-mesh-service-fabric-resources/NetworkAndGateway.png
[Image3]: media/service-fabric-mesh-service-fabric-resources/volumes.png
