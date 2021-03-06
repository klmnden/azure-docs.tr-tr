---
title: Coğrafi-çoğaltılan Azure container registry
description: Oluşturma ve coğrafi olarak çoğaltılmış Azure kapsayıcı kayıt defterleri yönetmeye başlayın.
services: container-registry
author: stevelas
manager: jeconnoc
ms.service: container-registry
ms.topic: overview
ms.date: 05/24/2019
ms.author: stevelas
ms.openlocfilehash: a26b261a900dfae742e00d9540e744524b781815
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66384113"
---
# <a name="geo-replication-in-azure-container-registry"></a>Azure Container Registry’de coğrafi çoğaltma

Yerel Varlığınızı veya sık erişimli bir yedekleme isteyen şirketler birden çok Azure bölgelerinden hizmetlerini çalıştırmak seçin. En iyi uygulama, kapsayıcı kayıt defteri görüntüleri çalıştırdığı her bölgeye yerleştirilmesi, hızlı ve güvenilir bir görüntü katmanı aktarımları etkinleştirme ağa yakın işlemleri sağlar. Coğrafi çoğaltma, tek bir kayıt defteri gibi çalışması bir Azure container registry ile bölgesel çok yöneticili kayıt defterleri birden çok bölgede hizmet sağlar. 

Coğrafi olarak çoğaltılmış bir kayıt defteri aşağıdaki avantajları sağlar:

* Tek bir görüntü/kayıt/etiket adları birden çok bölgede kullanılabilir
* Bölgesel dağıtımlara ağa yakın kayıt defteri erişimi
* Hiçbir ek çıkış ücretleri, kapsayıcı konağı ile aynı bölgede yerel, çoğaltılmış bir kayıt defterinden görüntü olarak alınır
* Kayıt defteri birden çok bölgede tek yönetim

> [!NOTE]
> Birden fazla Azure container registry'den kapsayıcı görüntüleri kopyalarını bulundurmak gerekiyorsa, Azure Container Registry de destekler [görüntü alma](container-registry-import-images.md). Örneğin, bir DevOps iş akışında, görüntüyü bir geliştirme kayıt defterinden bir üretim kayıt defterine Docker komutlarını kullanmaya gerek kalmadan alabilirsiniz.
>

## <a name="example-use-case"></a>Örnek Kullanım örneği
Contoso, ABD, Kanada ve Avrupa arasında yer alan bir genel durum Web sitesini çalıştırır. Yerel ve ağa yakın içeriğin bulunduğu bu pazarlar için Contoso çalıştıran [Azure Kubernetes hizmeti](/azure/aks/) (AKS), Batı ABD, Doğu ABD, Kanada Orta ve Batı Avrupa kümeleri. Bir Docker görüntüsü dağıtılan Web uygulaması, tüm bölgelerde aynı kodu ve görüntü kullanır. Bu bölge için yerel içerik, her bölgeye benzersiz olarak sağlanan bir veritabanından alınır. Bölgesel her dağıtım, yerel veritabanı gibi kaynakları için benzersiz yapılandırmasını sahiptir.

Geliştirme ekibi, Seattle WA, West US veri merkezini kullanarak bulunur.

![Birden çok kayıt defterleri için gönderme](media/container-registry-geo-replication/before-geo-replicate.png)<br />*Birden çok kayıt defterleri için gönderme*

Coğrafi çoğaltma özelliklerini kullanılmadan önce bir ABD bankasına bağlı kayıt defterinde Batı ABD, Batı Avrupa'daki ek bir kayıt defteri ile Contoso vardı. Bu farklı bölgelerdeki hizmet vermek için iki farklı kayıt defterlerinde görüntüleri Geliştirme takımına gönderilir.

```bash
docker push contoso.azurecr.io/public/products/web:1.2
docker push contosowesteu.azurecr.io/public/products/web:1.2
```
![Birden çok kayıt defterlerinden çekiliyor](media/container-registry-geo-replication/before-geo-replicate-pull.png)<br />*Birden çok kayıt defterlerinden çekiliyor*

Birden çok kayıt defterleri tipik sorunları şunlardır:

* Kayıt defterinden Batı ABD veri merkezlerinden görüntüleri bu uzak kapsayıcı konakların her biri istek olarak çıkış ücretleri yansıtılmasını Batı ABD, Doğu ABD, Batı ABD ve Kanada Orta kümelerinin tamamı çekin.
* Geliştirme ekibi, Batı ABD ve Batı Avrupa kayıt defterleri için görüntü göndermeniz gerekir.
* Geliştirme ekibi, yapılandırmanız ve görüntü adları ile yerel kayıt başvuran her bölgesel dağıtım korumak gerekir.
* Kayıt defteri erişimini, her bölge için yapılandırılmalıdır.

## <a name="benefits-of-geo-replication"></a>Coğrafi çoğaltma avantajları

![Coğrafi olarak çoğaltılmış bir kayıt defterinden çekiliyor](media/container-registry-geo-replication/after-geo-replicate-pull.png)

Azure Container Registry'nin coğrafi çoğaltma özelliğini kullanarak, bu avantajlar gerçekleşir:

* Tek bir kayıt defterini, tüm bölgelerde yönetin: `contoso.azurecr.io`
* Tüm bölgelerin aynı görüntü URL'si kullanılan görüntüsü dağıtımları tek bir yapılandırmasını yönetin: `contoso.azurecr.io/public/products/web:1.2`
* Coğrafi çoğaltma ACR yönetirken siz tek bir kayıt defterine gönderin. Yapılandırabileceğiniz bölgesel [Web kancaları](container-registry-webhook.md) , belirli yineleme olayları bildirmek için.

## <a name="configure-geo-replication"></a>Coğrafi çoğaltmayı yapılandırma

Coğrafi çoğaltma yapılandırma, bir harita üzerindeki bölgeleri tıklatmak kadar kolaydır. Coğrafi çoğaltma ayrıca yönetebilirsiniz kullanarak araçlar dahil olmak üzere [az acr çoğaltma](/cli/azure/acr/replication) Azure CLI komutları.

Coğrafi çoğaltma özelliğidir [Premium kayıt defterlerinin](container-registry-skus.md) yalnızca. Premium, temel ve standart Premium değiştirebileceğiniz henüz kaydınız yoksa [Azure portalında](https://portal.azure.com):

![Azure Portalı'ndaki SKU'ları değiştirme](media/container-registry-skus/update-registry-sku.png)

Premium kayıt defteriniz için coğrafi çoğaltmayı yapılandırmak için oturum açın Azure Portal'daki https://portal.azure.com.

Azure kapsayıcı kayıt defterinize gidin ve **çoğaltmalar**:

![Azure portalı kapsayıcı kayıt defteri kullanıcı arabirimindeki Çoğaltmalar](media/container-registry-geo-replication/registry-services.png)

Geçerli olan tüm Azure bölgelerine gösteren bir harita görüntülenir:

 ![Azure portalındaki bölge haritası](media/container-registry-geo-replication/registry-geo-map.png)

* Mavi Altıgenler geçerli yineleme temsil eder.
* Yeşil Altıgenler olası çoğaltma bölgeleri temsil eder.
* Gri Altıgenler temsil eden Azure bölgesine çoğaltma için henüz kullanılamıyor

Bir çoğaltmayı yapılandırmak için yeşil Altıgene seçip **Oluştur**:

 ![Azure portalında çoğaltma oluşturmaya yönelik kullanıcı arabirimi](media/container-registry-geo-replication/create-replication.png)

Ek yinelemeler yapılandırmak için diğer bölgeler için yeşil altıgenlerin seçin ve ardından **Oluştur**.

ACR görüntüleri yapılandırılmış yinelemelere arasında eşitleme başlar. İşlem tamamlandıktan sonra portalda yansıtır *hazır*. Çoğaltma durumu Portalı'nda otomatik olarak güncelleştirmez. Güncelleştirilmiş durumunu görmek için Yenile düğmesini kullanın.

## <a name="considerations-for-using-a-geo-replicated-registry"></a>Coğrafi olarak çoğaltılmış bir kayıt defteri kullanma konuları

* Her bölgede coğrafi olarak çoğaltılmış bir kayıt defteri ayarladıktan bağımsızdır. Azure Container Registry SLA'ları, coğrafi olarak çoğaltılmış her bölge için geçerlidir.
* Anında iletme veya Azure Traffic Manager arka planda bir coğrafi olarak çoğaltılmış kayıt defterinden görüntüleri çekmek olduğunda size en yakın bölgede bulunan kayıt isteği gönderir.
* Görüntü veya da etiketi bir güncelleştirme en yakın bölgeyi gönderdikten sonra katmanlar ve bildirimler üstündeki kalan bölgelerine çoğaltmak Azure Container Registry için biraz zaman alabilir. Daha büyük görüntüler küçük parçalara çoğaltmak için daha uzun sürer. Görüntüleri ve etiketleri ile son tutarlılık modelini çoğaltma bölgede eşitlenir.
* Coğrafi olarak çoğaltılmış bir kayıt defterine itme güncelleştirmeleri bağımlı iş akışları yönetmek için yapılandırdığınız öneririz [Web kancaları](container-registry-webhook.md) anında iletme olaylara yanıt vermesi. Coğrafi olarak çoğaltılmış bölgeler arasında tamamlandıkça olayları göndermek izlemek için coğrafi olarak çoğaltılmış bir kayıt içindeki bölgesel Web kancalarını ayarlayabilirsiniz.


## <a name="geo-replication-pricing"></a>Coğrafi çoğaltma fiyatlandırması

Coğrafi çoğaltma özelliğidir [Premium SKU](container-registry-skus.md) Azure Container Registry'nin. İstenen bölge için bir kayıt defteri çoğalttığınızda, her bölge için Premium kayıt defteri ücretler doğurur.

Önceki örnekte, Doğu ABD, Kanada Orta ve Batı Avrupa için çoğaltmaları ekleme, iki kayıt defterleri, aşağı Contoso birleştirildi. Contoso dört kez Premium hiçbir ek yapılandırma veya yönetim ile aylık ödersiniz. Her bölgede yerel olarak, performans, güvenilirlik Kanada Batı ABD ve Doğu ABD ağ çıkışı ücretleri olmadan geliştirme resimlerinin artık çeker.

## <a name="next-steps"></a>Sonraki adımlar

Üç bölümlü öğretici serisinin denetleyin [Azure Container Registry coğrafi çoğaltma](container-registry-tutorial-prepare-registry.md). Coğrafi olarak çoğaltılmış bir kayıt defteri oluşturma, bir kapsayıcı oluşturma ve tek bir dağıttıktan sonra yol `docker push` kapsayıcı örnekleri için birden fazla bölgesel Web uygulaması için komutu.

> [!div class="nextstepaction"]
> [Azure Container Registry'de coğrafi çoğaltma](container-registry-tutorial-prepare-registry.md)
