---
title: "Service Fabric uygulama yükseltme Öğreticisi | Microsoft Docs"
description: "Bu makalede bir Service Fabric uygulaması dağıtma, kodunu değiştirme ve Visual Studio kullanarak bir yükseltmesinde deneyimi anlatılmaktadır."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: a3181a7a-9ab1-4216-b07a-05b79bd826a4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 940440688ec770a4aeb932b574bd6be173f494d4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="service-fabric-application-upgrade-tutorial-using-visual-studio"></a>Visual Studio kullanarak Service Fabric uygulaması yükseltme Öğreticisi
> [!div class="op_single_selector"]
> * [PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
> * [Visual Studio](service-fabric-application-upgrade-tutorial.md)
> 
> 

<br/>

Azure Service Fabric yalnızca değiştirilen Hizmetleri yükseltilir ve uygulama sağlığını yükseltme işlemi boyunca izlenir sağlayarak bulut uygulamaları yükseltme işlemini basitleştirir. Bu da otomatik olarak geri sorunları karşılaşıldığında önceki sürümü için uygulama yapar. Service Fabric uygulaması yükseltmelerin *sıfır kapalı kalma süresi*, bu yana uygulama kapalı kalma süresi ile yükseltilebilir. Bu öğreticide, Visual Studio'dan çalışırken yükseltmeyi tamamlamak alınmaktadır.

## <a name="step-1-build-and-publish-the-visual-objects-sample"></a>1. adım: Yapı ve görsel nesneler örnek yayımlama
İlk olarak, indirme [görsel nesneler](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Actors/VisualObjects) github'dan uygulama. Ardından, yapı ve uygulama projesine sağ tıklayarak uygulamayı yayımlamak **VisualObjects**, seçerek **Yayımla** Service Fabric menü öğesi komutu.

![Service Fabric uygulaması bağlam menüsü][image1]

Seçme **Yayımla** açılan bir pencere ortaya getirir ve ayarlayabilirsiniz **hedef profil** için **PublishProfiles\Local.xml**. Tıklamadan önce pencereyi aşağıdaki gibi görünmelidir **Yayımla**.

![Service Fabric uygulaması yayımlama][image2]

Tıklayabilirsiniz artık **Yayımla** iletişim kutusunda. Kullanabileceğiniz [küme ve uygulamayı görüntülemek için Service Fabric Explorer](service-fabric-visualizing-your-cluster.md). Görsel nesneler uygulama yazarak gidebilirsiniz bir web hizmeti olan [http://localhost:8081/visualobjects/](http://localhost:8081/visualobjects/) tarayıcınızın adres çubuğundaki.  Ekranda Dolaşma, 10, kayan görsel nesneler görmeniz gerekir.

**Not:** dağıtılması durumunda `Cloud.xml` profil (Azure Service Fabric) uygulama sonra kullanılabilir olmalıdır **http://{ServiceFabricName}. { Region}.cloudapp.Azure.com:8081/visualobjects/**. Sahip olduğunuzdan emin olun `8081/TCP` yük dengeleyici (Bul Service Fabric örneği ile aynı kaynak grubunda yük dengeleyici) yapılandırılmış.

## <a name="step-2-update-the-visual-objects-sample"></a>2. adım: görsel nesneler örnek güncelleştir
1. adımda dağıtılan sürümüyle görsel nesneler değil döndürme fark edebilirsiniz. Şimdi bir burada görsel nesneler de döndürmek için bu uygulamayı yükseltin.

VisualObjects çözümünde VisualObjects.ActorService projesini seçin ve açmak **VisualObjectActor.cs** dosya. Bu dosyanın içindeki yöntem geçin `MoveObject`, çıkışı açıklama `visualObject.Move(false)`ve açıklama durumundan çıkarmanız `visualObject.Move(true)`. Hizmet yükseltildikten sonra bu kodu değişikliği nesnesini döndürür.  **(Yeniden oluşturmaz) çözümü derleme artık**, değiştirilmiş projeleri oluşturur. Seçerseniz *tüm yeniden*, tüm projeleri için sürümleri güncelleştirmeniz gerekir.

Biz de sürüme uygulamamız gerekir. Üzerinde sağ sonra sürüm değişiklik yapmak için **VisualObjects** projesini Visual Studio'yu kullanabilirsiniz **bildirim sürümleri Düzenle** seçeneği. Bu seçeneğin belirlenmesi edition sürümleri için iletişim kutusunda aşağıdaki gibi getirir:

![Sürüm oluşturma iletişim kutusu][image3]

Değiştirilen projeleri ve sürüm 2.0.0 uygulamaya yanı sıra bunların kod paketler için sürümler güncelleştirin. Değişiklikler yapıldıktan sonra bildirimi aşağıdaki gibi görünmelidir (kalın bölümleri göster değişiklikleri):

![Güncelleştirme sürümleri][image4]

Visual Studio Araçları seçtikten sonra sürümlerinin otomatik toplamaları yapabilirsiniz **otomatik olarak uygulama ve hizmet sürümleri güncelleştirme**. Kullanırsanız [SemVer](http://www.semver.org), kod güncelleştirmeniz gerekir ve/veya yapılandırma paketi sürümü bu, tek başına seçeneği işaretlidir.

Değişiklikleri kaydetmek ve şimdi denetle **uygulama yükseltme** kutusu.

## <a name="step-3--upgrade-your-application"></a>3. adım: uygulamanızı yükseltin
İle öğrenmeniz [uygulama yükseltme parametreleri](service-fabric-application-upgrade-parameters.md) ve [yükseltme işlemi](service-fabric-application-upgrade.md) çeşitli yükseltme parametreleri, zaman aşımları ve olabilir sistem durumu ölçüt iyi anlamış almak için uygulanır. Bu kılavuz için hizmet sistem durumu değerlendirme ölçüt (izlenmeyen modu) varsayılan olarak ayarlanır. Seçerek bu ayarları yapılandırabilirsiniz **yükseltme ayarlarını yapılandır** ve istediğiniz gibi parametreleri değiştirme.

Biz seçerek uygulama yükseltme işlemini başlatmak için tüm kümesi artık **Yayımla**. Bu seçenek uygulamanızın hangi nesneleri döndürme 2.0.0, sürümüne yükseltir. Service Fabric (bazı nesneler ilk olarak, başkaları tarafından ve ardından güncelleştirilir) bir defada tek bir güncelleştirme etki alanı yükseltme ve yükseltme sırasında hizmet erişilebilir kalır. Hizmete erişim, istemci (tarayıcı) denetlenebilir.  

Artık uygulama yükseltme devam eder, olarak, Service Fabric Explorer ile kullanarak izleyebileceğiniz **yükseltme devam eden** uygulamalar sekmesinde.

Birkaç dakika içinde (tamamlandı) tüm güncelleme etki alanına yükseltilmesi ve Visual Studio çıkış penceresi de yükseltme tamamlandıktan durumlarında. Ve, bulmalıdır *tüm* Tarayıcı pencerenizde görsel nesneler artık döndürme!

Sürümleri değiştirme denemek isteyebilirsiniz ve 2.0.0 sürümünden bir alıştırma olarak 3.0.0 sürümüne veya hatta sürümünden 2.0.0 geri sürümüne 1.0.0 taşıma. Zaman aşımları ve kendiniz bunlarla bilgi sahibi olmak için sistem durumu ilkeleri ile yürütün. Yerel küme aksine Azure bir küme dağıtımı sırasında kullanılan parametreleri farklı gerekebilir. Zaman aşımları ölçülü ayarlamanızı öneririz.

## <a name="next-steps"></a>Sonraki adımlar
[PowerShell kullanarak uygulamanızı yükseltme](service-fabric-application-upgrade-tutorial-powershell.md) PowerShell kullanarak bir uygulama yükseltme yol gösterir.

Kullanarak uygulamanızı nasıl yükseltilir kontrol [yükseltme parametreleri](service-fabric-application-upgrade-parameters.md).

Uygulama yükseltme nasıl kullanılacağını öğrenerek uyumlu hale getirmek [veri seri hale getirme](service-fabric-application-upgrade-data-serialization.md).

Gelişmiş işlevselliği başvurarak uygulamanızı yükseltirken kullanmayı öğrenin [konuları Gelişmiş](service-fabric-application-upgrade-advanced.md).

Adımlarına bakarak uygulama yükseltmeleri sık karşılaşılan sorunları düzeltmek [uygulama yükseltme sorunlarını giderme](service-fabric-application-upgrade-troubleshooting.md).

[image1]: media/service-fabric-application-upgrade-tutorial/upgrade7.png
[image2]: media/service-fabric-application-upgrade-tutorial/upgrade1.png
[image3]: media/service-fabric-application-upgrade-tutorial/upgrade5.png
[image4]: media/service-fabric-application-upgrade-tutorial/upgrade6.png
