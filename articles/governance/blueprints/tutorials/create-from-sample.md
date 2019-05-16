---
title: Blueprint örnekten bir ortam oluşturun
description: İki kaynak gruplarını ayarlar ve her biri için bir rol ataması yapılandıran bir şema tanımı oluşturmak için bir şema örnek kullanın.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/05/2019
ms.topic: tutorial
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: b0d5d96ff897ac1710206eb49bca785e8809cb7d
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65758845"
---
# <a name="tutorial-create-an-environment-from-a-blueprint-sample"></a>Öğretici: Blueprint örnekten bir ortam oluşturun

Örnek şemaları ne yapılabilir örnekleri sağlayan Azure şemaları kullanma. Her bir örnek ile belirli bir amaç veya amaçlı ancak eksiksiz bir ortam başlarına oluşturmaz. Her Azure şemaları çeşitli birleşimlerini dahil yapıtlar, tasarım ve parametreleri kullanarak keşfetmek için başlangıç noktası olarak yöneliktir.

Aşağıdaki öğreticide **RBAC kaynak gruplarıyla** şemaları hizmet farklı yönlerini göstermek için şema örnek. Aşağıdaki adımları ele alınmaktadır:

> [!div class="checklist"]
> - Örnekten yeni bir şema tanımını oluşturma
> - Örnek olarak kopyanızın işaretlemek **yayımlandı**
> - Blueprint kopyasını mevcut bir aboneliğe atayın
> - Dağıtılan kaynakları atama için inceleyin
> - Kilitler kaldırmak için şema atamasını Kaldır

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için bir Azure aboneliği gereklidir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="create-blueprint-definition-from-sample"></a>Örnekten şema tanımını oluşturma

İlk olarak, şema örnek uygulayın. İçeri aktarmak, ortamınızda örneği temel alarak yeni bir şema oluşturur.

1. Seçin **tüm hizmetleri** sol bölmesinde. Arayın ve seçin **şemaları**.

1. Gelen **Başlarken** seçin sol taraftaki sayfasında **Oluştur** düğmesini _blueprint oluşturma_.

1. Bulma **RBAC kaynak gruplarıyla** şema örnek altında _diğer örnekleri_ seçip **Bu örneği kullanmak**.

1. Girin _Temelleri_ şema örnek:

   - **Blueprint adı**: Şema örnek kopyası için bir ad sağlayın. Bu öğreticide, adı kullanacağız _iki rgs ile rol atamaları_.
   - **Tanım konumu**: Üç nokta kullanın ve bir yönetim grubuna veya aboneliğe örneğe kopyasını kaydetmek için'ı seçin.

1. Seçin _Yapıtları_ sayfanın üst kısmındaki sekme veya **sonraki: Yapıtları** sayfanın alt kısmındaki.

1. Şema örnek değişiklikleri yapıtların listesini gözden geçirin. Bu örnek iki kaynak gruplar olarak görünen adlarını tanımlar _ProdRG_ ve _PreProdRG_. Blueprint ataması sırasında her bir kaynak grubunun konumunu ve adını son ayarlanır. _ProdRG_ kaynak grubu atandığı _katkıda bulunan_ rol ve _PreProdRG_ kaynak grubu atandığı _sahibi_ ve  _Okuyucular_ rolleri. Tanımı üzerinde atanmış olan rolleri statiktir ancak kullanıcı, uygulamayı veya rolü atanmış Grup blueprint ataması sırasında ayarlanır.

1. Seçin **Taslağı Kaydet** bittiğinde şema örnek gözden geçirme.

Bu adım, seçili yönetim grubu veya abonelik örnek blueprint tanımının bir kopyasını oluşturur. Kaydedilen şema tanımını sıfırdan oluşturulan herhangi bir blueprint gibi yönetilir. Yönetim grubu veya abonelik gerektiği birçok örnek kaydedebilir. Ancak, her kopya benzersiz bir ad sağlanmalıdır.

Bir kez **başarılı şema tanımını kaydetme** portal bildirimi görünürse, sonraki adıma geçin.

## <a name="publish-the-sample-copy"></a>Örnek kopyalama yayımlama

Şema örnek kopyanızın artık ortamınızda oluşturuldu. İçinde oluşturulan **taslak** modu ve olmalıdır **yayımlanan** , atanan ve dağıtılan kullanılmadan önce. Blueprint kopyasını gereksinimlerini ve ortam için özelleştirilebilir. Bu öğretici için size herhangi bir değişiklik yapmaz.

1. Seçin **tüm hizmetleri** sol bölmesinde. Arayın ve seçin **şemaları**.

1. Seçin **Blueprint tanımları** soldaki sayfası. Bulmak için filtreleri kullanın; _iki rgs ile rol atamaları_ blueprint tanımının ve ardından bu seçeneği belirleyin.

1. Seçin **Yayımla şema** sayfanın üstünde. Sağ taraftaki yeni bölmede sağlamak **sürüm** olarak _1.0_ şema örnek kopyası için. Bu özellik, daha sonra bir değişikliği yapmak için yararlıdır. Sağlamak **notları değiştirmek** "ilk sürüm RBAC şema örnek kaynak gruplarıyla yayımlandığı gibi." Ardından **Yayımla** sayfanın alt kısmındaki.

Bu adım, bir abonelik için şema atamak mümkün kılar. Bir kez yayımlandıktan sonra değişiklikleri yine de yapılabilir. Ek bir değişiklik yapılması yeni bir yayımlama **sürüm** aynı şema tanımını farklı sürümleri arasında farklar izlemek için değer.

Bir kez **yayımlama başarılı tanımı blueprint** portal bildirimi görünürse, sonraki adıma geçin.

## <a name="assign-the-sample-copy"></a>Örnek kopya atama

Blueprint kopyasını başarıyla silindikten sonra **yayımlanan**, yönetim grubu için kaydedildi dahilinde bir aboneliğe atanabilir. Bu adım, burada parametreler şema kopyasını, her dağıtım benzersiz olacak şekilde sağlanır.

1. Seçin **tüm hizmetleri** sol bölmesinde. Arayın ve seçin **şemaları**.

1. Seçin **Blueprint tanımları** soldaki sayfası. Bulmak için filtreleri kullanın; _iki rgs ile rol atamaları_ blueprint tanımının ve ardından bu seçeneği belirleyin.

1. Seçin **Ata şema** şema tanımı sayfanın üstünde.

1. Blueprint ataması için parametre değerlerini sağlayın:

   - Temel

     - **Abonelikler**: Bir veya daha fazla yönetim grubuna olduğunuz abonelikleri için şema örnek kopyanızın kaydedilen seçin. Birden fazla aboneliğiniz seçerseniz, bir atama için her girdiğiniz parametreleri kullanarak oluşturulur.
     - **Ödev adı**: Şema tanımını adını temel alarak, önceden doldurulmuş adıdır.
     - **Konum**: Yönetilen kimlikle oluşturulması için bir bölge seçin. Azure Blueprint bu yönetilen kimliği kullanarak tüm yapıtları atanmış şemaya dağıtır. Daha fazla bilgi için bkz. [Azure kaynakları için yönetilen kimlikler](../../../active-directory/managed-identities-azure-resources/overview.md).
       Bu öğreticide, seçin _Doğu ABD 2_.
     - **Şema tanımı sürümü**: Çekme **yayımlanan** sürüm _1.0_ örnek şema tanımını kopyasının.

   - Atamayı Kilitle

     Seçin _salt okunur_ blueprint kilit modu. Daha fazla bilgi için bkz. [şema kaynağı kilitleme](../concepts/resource-locking.md).

   - Yönetilen Kimlik

     Varsayılan değeri bırakın _sistem tarafından atanan_ seçeneği. Daha fazla bilgi için [yönetilen kimlikleri](../../../active-directory/managed-identities-azure-resources/overview.md).

   - Yapıt parametreleri

     Bu bölümde tanımlanan parametrelerin altında tanımlandığı yapıtı için geçerlidir. Bu parametreler [dinamik parametreleri](../concepts/parameters.md#dynamic-parameters) blueprint ataması sırasında tanımlanan olduğundan. Her bir yapıt ne tanımlanan için parametre değeri ayarlamak **değer** sütun. İçin `{Your ID}`, Azure kullanıcı hesabınızı seçin.

     |Yapıt adı|Yapıt türü|Parametre adı|Değer|Açıklama|
     |-|-|-|-|-|
     |ProdRG kaynak grubu|Kaynak grubu|Ad|ProductionRG|İlk kaynak grubunun adını tanımlar.|
     |ProdRG kaynak grubu|Kaynak grubu|Location|Batı ABD 2|İlk kaynak grubunun konumunu ayarlar.|
     |Katılımcı|Rol ataması|Kullanıcı veya Grup|{ID}|Hangi kullanıcı veya grup vermek tanımlar _katkıda bulunan_ ilk kaynak grubu içinde rol ataması.|
     |PreProdRG kaynak grubu|Kaynak grubu|Ad|PreProductionRG|İkinci kaynak grubunun adını tanımlar.|
     |PreProdRG kaynak grubu|Kaynak grubu|Location|Batı ABD|İkinci kaynak grubunun konumunu ayarlar.|
     |Sahibi|Rol ataması|Kullanıcı veya Grup|{ID}|Hangi kullanıcı veya grup vermek tanımlar _sahibi_ ikinci kaynak grubu içinde rol ataması.|
     |Okuyucular|Rol ataması|Kullanıcı veya Grup|{ID}|Hangi kullanıcı veya grup vermek tanımlar _okuyucular_ ikinci kaynak grubu içinde rol ataması.|

1. Tüm parametreler girildikten sonra seçin **atama** sayfanın alt kısmındaki.

Bu adım tanımlanmış kaynakları dağıtır ve seçili yapılandırır **kilit atama**. Blueprint kilitleri uygulamak için en fazla 30 dakika sürebilir.

Bir kez **başarılı atama şema tanımını** portal bildirimi görünürse, sonraki adıma geçin.

## <a name="inspect-resources-deployed-by-the-assignment"></a>Atamaya göre dağıtılan kaynakları inceleyin

Şema atamasını oluşturur ve şema tanımında tanımlanan yapıları izler. Blueprint ataması sayfasından ve kaynakları, doğrudan bakarak kaynakların durumunu görebiliriz.

1. Seçin **tüm hizmetleri** sol bölmesinde. Arayın ve seçin **şemaları**.

1. Seçin **şemaları atanan** soldaki sayfası. Bulmak için filtreleri kullanın; _Assignment-two-rgs-with-role-assignments_ blueprint ataması ve ardından bu seçeneği belirleyin.

   Bu sayfadan atama başarılı oldu ve şema kilit durumlarını birlikte oluşturulan kaynakların listesini görebiliriz. Atama güncelleştirdiyseniz, **atama işlemi** açılan, dağıtım her tanım sürümüne ayrıntılarını gösterir. Oluşturulan her listelenen kaynak tıklanabilir ve bu kaynaklar özellik sayfası açılır.

1. Seçin **ProductionRG** kaynak grubu.

   Kaynak grubunun adının olduğunu görüyoruz **ProductionRG** ve yapıt görünen adı değil _ProdRG_. Bu ad, blueprint ataması sırasında ayarlanan değer eşleşir.

1. Seçin **erişim denetimi (IAM)** sayfasında soldaki ardından **rol atamaları** sekmesi.

   Buraya hesabınızın verildiğini bakın _katkıda bulunan_ kapsamını rolünde _bu kaynak_. _Assignment-two-rgs-with-role-assignments_ blueprint ataması _sahibi_ rolü olarak bu kaynak grubu oluşturmak için kullanıldı. Bu izinleri ayrıca yapılandırılmış şema kilit ile kaynakları yönetmek için kullanılır.

1. Azure portal Kırıntı seçin **Assignment-two-rgs-with-role-assignments** bir sayfa geri gitmek için seçip **PreProductionRG** kaynak grubu.

1. Seçin **erişim denetimi (IAM)** sayfasında soldaki ardından **rol atamaları** sekmesi.

   Buraya hesabınızın hem verildiğini bakın _sahibi_ ve _okuyucu_ rolleri, hem de kapsamını _bu kaynak_. Şema atamasını ayrıca sahip _sahibi_ rolü gibi ilk kaynak grubu.

1. Seçin **atamaları Reddet** sekmesi.

   Oluşturulan şema atamasını bir [atamasını Reddet](../../../role-based-access-control/deny-assignments.md) zorlamak için dağıtılan kaynak grubunda _salt okunur_ blueprint kilit modu. Reddetme atama uygun haklara sahip biri üzerinde engeller _rol atamaları_ belirli eylemleri gelen sekmesi. Reddetme atama etkiler _tüm ilkeleri_.

1. Reddet Ataması'nı seçin ve ardından **izinler reddedildi** soldaki sayfası.

   Reddetme atama ile yapılan tüm işlemlerde engelliyor **\*** ve **eylem** yapılandırma, ancak hariç tutarak okuma erişimi verir  **\* /Okuma**aracılığıyla **NotActions**.

1. Azure portal Kırıntı seçin **PreProductionRG - erişim denetimi (IAM)**. Ardından **genel bakış** sayfasında soldaki ardından **kaynak grubunu Sil** düğmesi. Bir ad girin _PreProductionRG_ seçin ve silmeyi onaylamak için **Sil** bölmesinin alt kısmındaki.

   Portal bildiriminden **PreProductionRG başarısız oldu. kaynak grubunu Sil** görüntülenir. Hesabınızı kaynak grubunu silme izni sahipken hatayı bildiren, şema atamasını tarafından erişim reddedildi. Biz seçildiğini unutmayın _salt okunur_ blueprint ataması sırasında şema kilit modu. Şema kilidi iznine sahip bir hesap da engeller _sahibi_, kaynak silmelerini. Daha fazla bilgi için bkz. [şema kaynağı kilitleme](../concepts/resource-locking.md).

Bu adımlar kaynaklarımızın tanımlandığı gibi oluşturulan ve şemayı kilitler silme iznine sahip bir hesaptan bile istenmeyen işlemi engelledi gösterir.

## <a name="unassign-the-blueprint"></a>Şema atamasını Kaldır

Dağıtılan kaynakları ve şema atamasını kaldırmak için son adımdır bakın.
Atamayı kaldırma, dağıtılmış yapıların kaldırmaz.

1. Seçin **tüm hizmetleri** sol bölmesinde. Arayın ve seçin **şemaları**.

1. Seçin **şemaları atanan** soldaki sayfası. Bulmak için filtreleri kullanın; _Assignment-two-rgs-with-role-assignments_ blueprint ataması ve ardından bu seçeneği belirleyin.

1. Seçin **Atamayı Kaldır blueprint** sayfanın üstünde düğme. Onay iletişim kutusunda uyarı okuyun ve ardından **Tamam**.

   Blueprint ataması kaldırıldı ile şema kilitler de kaldırılır. Oluşturulan kaynakları yeniden izinlerine sahip bir hesap tarafından silinebilir.

1. Seçin **kaynak grupları** Azure ardından menüden **ProductionRG**.

1. Seçin **erişim denetimi (IAM)** sayfasında soldaki ardından **rol atamaları** sekmesi.

Her kaynak grupları için güvenlik dağıtılan rol atamaları hala var, ancak artık şema atamasını sahip _sahibi_ erişim.

Bir kez **başarılı kaldırma şema atamasını** portal bildirimi görünürse, sonraki adıma geçin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticiyle tamamlandığında, aşağıdaki kaynakları silin:

- Kaynak grubu _ProductionRG_
- Kaynak grubu _PreProductionRG_
- Blueprint tanımının _iki rgs ile rol atamaları_

## <a name="next-steps"></a>Sonraki adımlar

- [Şema yaşam döngüsü](../concepts/lifecycle.md) hakkında bilgi edinin
- [Statik ve dinamik parametreleri](../concepts/parameters.md) kullanmayı anlayın
- [Şema kaynak kilitleme](../concepts/resource-locking.md) özelliğini kullanmayı öğrenin
- [Şema sıralamasını](../concepts/sequencing-order.md) özelleştirmeyi öğrenin
- [Var olan atamaları güncelleştirmeyi](../how-to/update-existing-assignments.md) öğrenin
- [Genel sorun giderme](../troubleshoot/general.md) adımlarıyla şema atama sorunlarını giderin