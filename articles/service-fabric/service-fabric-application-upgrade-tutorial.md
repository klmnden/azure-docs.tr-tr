---
title: Service Fabric uygulaması yükseltme Öğreticisi | Microsoft Docs
description: Bu makalede, Service Fabric uygulaması dağıtma, kod değiştirme ve Visual Studio kullanarak bir yükseltmesinde yönelik deneyimi gösterilmektedir.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: chackdan
editor: ''
ms.assetid: a3181a7a-9ab1-4216-b07a-05b79bd826a4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: 8fe0bf9c8827b7248195f89377176fd834845e32
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58663681"
---
# <a name="service-fabric-application-upgrade-tutorial-using-visual-studio"></a>Visual Studio kullanarak Service Fabric uygulaması yükseltme Öğreticisi
> [!div class="op_single_selector"]
> * [PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
> * [Visual Studio](service-fabric-application-upgrade-tutorial.md)
> 
> 

<br/>

Azure Service Fabric yalnızca değiştirilen Hizmetleri yükseltilir ve yükseltme işlemi uygulama sağlığı izlenip izlenmediğini sağlayarak bulut uygulamalarını yükseltme işlemini basitleştirir. Bu da otomatik olarak geri uygulamayı sorunları karşılaşıldığında önceki sürüme yapar. Service Fabric uygulama yükseltmeleri *sıfır kapalı kalma süresi*, bu yana kesinti yaşamadan uygulama yükseltilebilir. Bu öğretici, Visual Studio'dan bir sıralı yükseltmesinin nasıl tamamlanacağı konusunda kapsar.

## <a name="step-1-build-and-publish-the-visual-objects-sample"></a>1. Adım: Derleme ve görsel nesneler örnek yayımlama
İlk olarak, indirme [görsel nesneler](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Actors/VisualObjects) github'dan uygulama. Sonra derleme ve uygulamayı uygulama projesine sağ tıklayarak Yayımla **VisualObjects**, seçerek **Yayımla** Service Fabric menü öğesi komutu.

![Service Fabric uygulaması için bağlam menüsü][image1]

Seçme **Yayımla** bir açılır pencere taşır ve ayarlayabileceğiniz **hedef profil** için **PublishProfiles\Local.xml**. Tıklamadan önce penceresi aşağıdaki gibi görünmelidir **Yayımla**.

![Service Fabric uygulaması yayımlama][image2]

Tıklayabilirsiniz artık **Yayımla** iletişim kutusunda. Kullanabileceğiniz [Service Fabric Explorer'ı, küme ve uygulamayı görüntülemek için](service-fabric-visualizing-your-cluster.md). Görsel nesneler uygulama yazarak gidebilirsiniz bir web hizmeti sahip [ http://localhost:8081/visualobjects/ ](http://localhost:8081/visualobjects/) tarayıcınızın adres çubuğuna.  Ekranda taşınması, 10, kayan görsel nesneler görmeniz gerekir.

**NOT:** Dağıtma, `Cloud.xml` profili (Azure Service Fabric), uygulama daha sonra kullanılabilir olmalıdır **http://{ServiceFabricName}. { Region}.cloudapp.Azure.com:8081/visualobjects/**. Sahip olduğunuzdan emin olun `8081/TCP` yük dengeleyicide (Service Fabric örnekle aynı kaynak grubunu yük Dengeleyicide Bul) yapılandırılmış.

## <a name="step-2-update-the-visual-objects-sample"></a>2. Adım: Görsel nesneler örnek güncelleştirme
1. adımda dağıtılmış sürümle görsel nesneler değil döndürme fark edebilirsiniz. Şimdi bir görsel nesneler burada döndürmek için bu uygulamayı yükseltin.

VisualObjects çözüm VisualObjects.ActorService projeyi seçin ve açın **VisualObjectActor.cs** dosya. Bu dosyanın içindeki yöntemine gidin `MoveObject`, açıklama `visualObject.Move(false)`ve açıklama durumundan çıkarın `visualObject.Move(true)`. Hizmet yükseltildikten sonra bu kod değişikliği nesnesini döndürür.  **(Değil, yeniden derleme) çözümü oluşturabilirsiniz artık**, değiştirilen projeleri oluşturur. Seçerseniz *tümünü yeniden derle*, tüm projeler için sürümüne sahip.

Biz de sürüme uygulamamız gerekir. Sonra sağ tıklayın sürüm değişiklikleri yapmak için **VisualObjects** proje, Visual Studio kullanabilirsiniz **bildirim sürümlerini Düzenle** seçeneği. Bu seçeneğin belirlenmesi edition sürümleri için iletişim kutusu şu şekilde getirir:

![Sürüm oluşturma iletişim kutusu][image3]

Sürümler değiştirilmiş projeler ve uygulama sürüm 2.0.0 ile birlikte kendi kod paketleri için güncelleştirin. Değişiklikler yapıldıktan sonra bildirimin aşağıdakine benzer olması gerekir (kalın bölümleri değişiklikleri göster):

![Güncelleştirme sürümleri][image4]

Visual Studio Araçları seçerek temel sürümleri otomatik toplamaları yapabilirsiniz **uygulama ve hizmet sürümlerini otomatik olarak güncelleştir**. Kullanırsanız [SemVer](http://www.semver.org), kodu güncelleştirmeniz gerekir ve/veya yapılandırma paketi sürümü olmadığını tek başına seçeneğinin işaretli.

Değişiklikleri kaydetmek ve şimdi denetle **uygulamayı Yükselt** kutusu.

## <a name="step-3--upgrade-your-application"></a>3. adım:  Uygulamanızı yükseltme
İle kendinizi alıştırın [uygulama yükseltme parametreleri](service-fabric-application-upgrade-parameters.md) ve [yükseltme işlemi](service-fabric-application-upgrade.md) çeşitli yükseltme parametreleri, zaman aşımları ve olabilir sistem durumu ölçütü iyi bir anlayış edinmek için uygulanır. Bu kılavuz için hizmet sistem durumu değerlendirme ölçütü (izlenmeyen modu) varsayılan olarak ayarlanır. Seçerek bu ayarları yapılandırabilirsiniz **yükseltme ayarlarını yapılandırma** ve ardından istediğiniz gibi parametreleri değiştirme.

Seçerek uygulama yükseltme işlemini başlatmak için tüm küme duyuyoruz artık **Yayımla**. Bu seçenek, sürüm 2.0.0, hangi nesneleri döndürme uygulamanıza yükseltir. Service Fabric (bazı nesneler ilk olarak, başkaları tarafından izlenen güncelleştirilir) bir anda bir güncelleştirme etki alanı yükseltir ve yükseltme sırasında hizmet erişilebilir kalır. Hizmete erişimi istemci (tarayıcı) denetlenebilir.  

Artık uygulama yükseltme işlemi olarak, Service Fabric Explorer ile kullanarak izleyebileceğiniz **devam eden yükseltmeler** uygulamalar sekmesinde.

Birkaç dakika içinde tüm güncelleştirme etki alanları (tamamlanmış) yükseltilmiş olmalıdır ve Visual Studio çıkış penceresine ayrıca yükseltme tamamlandığını belirtmelidir. Ve bulmanız gerekir *tüm* Tarayıcı pencerenizde görsel nesneler artık döndürme!

Sürümleri değiştirmeyi deneyin isteyebilirsiniz ve sürüm 2.0.0 bir alıştırma olarak 3.0.0 sürümüne ya da sürüm 2.0.0 sürüm 1.0.0 dön taşıma. Zaman aşımları ve kendiniz bunlarla ilgili bilgi sahibi olmak için sistem durumu ilkeleri yürütün. Azure kümesine yerine yerel bir küme dağıtılırken kullanılan parametreler farklı olabilir. Zaman aşımlarını ölçülü ayarlamanızı öneririz.

## <a name="next-steps"></a>Sonraki adımlar
[PowerShell kullanarak uygulamanızı yükseltmek](service-fabric-application-upgrade-tutorial-powershell.md) PowerShell kullanarak bir uygulama yükseltmesi size yol gösterir.

Uygulamanızı kullanarak nasıl yükseltileceğini kontrol [yükseltme parametreleri](service-fabric-application-upgrade-parameters.md).

Nasıl kullanılacağını öğrenerek, uygulama yükseltmeleri uyumlu olma [veri seri hale getirme](service-fabric-application-upgrade-data-serialization.md).

Uygulamanızı yükseltilirken başvurarak gelişmiş işlevselliği kullanmanıza öğrenin [Gelişmiş konular](service-fabric-application-upgrade-advanced.md).

Yaygın sorunlar uygulama yükseltmeleri adımları başvurarak düzeltme [uygulama yükseltme sorunlarını giderme](service-fabric-application-upgrade-troubleshooting.md).

[image1]: media/service-fabric-application-upgrade-tutorial/upgrade7.png
[image2]: media/service-fabric-application-upgrade-tutorial/upgrade1.png
[image3]: media/service-fabric-application-upgrade-tutorial/upgrade5.png
[image4]: media/service-fabric-application-upgrade-tutorial/upgrade6.png
