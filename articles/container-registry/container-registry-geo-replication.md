---
title: Coğrafi-çoğaltan bir Azure kapsayıcı kayıt defteri
description: Oluşturma ve yönetme coğrafi olarak çoğaltılmış Azure kapsayıcı kayıt defterleri kullanmaya başlayın.
services: container-registry
author: stevelas
manager: jeconnoc
ms.service: container-registry
ms.topic: overview-article
ms.date: 04/10/2018
ms.author: stevelas
ms.openlocfilehash: e4695428b03961f5e899007609dfb1088dde77a8
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="geo-replication-in-azure-container-registry"></a>Azure Container Registry’de coğrafi çoğaltma

Birden çok Azure bölgelerinden hizmetlerini çalıştırmak yerel bir varlık veya bir dinamik yedek isteyen şirketler seçin. En iyi uygulama, ağ Kapat işlemlerine, hızlı ve güvenilir görüntü katman aktarımları etkinleştirme görüntüleri çalıştırdığı her bölgede bir kapsayıcı kayıt defteri yerleştirme izin verir. Coğrafi çoğaltma, tek bir kayıt defteri çalışması bir Azure kapsayıcı kayıt defteri birden çok yöneticili bölgesel kayıt defterleri ile birden çok bölgeye hizmet sağlar.

Coğrafi olarak çoğaltılmış bir kayıt defteri aşağıdaki avantajları sağlar:

* Birden çok bölgeler arasında tek görüntü/kayıt/etiket adları kullanılabilir
* Bölgesel dağıtımlarından ağ Kapat kayıt defteri erişimi
* Hiçbir ek çıkış ücretleri kapsayıcı ana bilgisayar ile aynı bölgede yerel, çoğaltılmış bir kayıt defterinden görüntü olarak alınır
* Kayıt defteri birden çok bölgede tek yönetim

## <a name="example-use-case"></a>Örnek Kullanım örneği
Contoso ABD, Kanada ve Avrupa arasında bulunan bir ortak varlığı Web sitesini çalıştırır. Contoso yerel ve ağ Kapat içeriğin bulunduğu bu pazarda çalışan [Azure kapsayıcı hizmeti](/azure/container-service/kubernetes/) (ACS) Kubernetes, Batı ABD, Doğu ABD, Orta Kanada ve Batı Avrupa kümeleri. Docker resim olarak dağıtılan Web uygulaması tüm bölgeler arasında aynı kodu ve görüntü kullanır. İçerik, o bölge için yerel her bölgeye benzersiz olarak sağlanan bir veritabanından alınır. Bölgesel her dağıtım benzersiz yapılandırmasıyla yerel veritabanı gibi kaynakların sahiptir.

Geliştirme ekibi, Seattle WA, Batı ABD veri merkezi kullanılarak bulunur.

![Birden çok kayıt defterleri iletme](media/container-registry-geo-replication/before-geo-replicate.png)<br />*Birden çok kayıt defterleri iletme*

Coğrafi çoğaltma özelliği kullanılmadan önce bir ABD tabanlı kayıt defterinde Batı ABD, Batı Avrupa'da ek bir kayıt defteri ile Contoso vardı. Bu farklı bölgelerde hizmet vermek için geliştirme ekibi görüntüleri için iki farklı kayıt defterleri itme gerekiyordu.

```bash
docker push contoso.azurecr.io/pubic/products/web:1.2
docker push contosowesteu.azurecr.io/pubic/products/web:1.2
```
![Birden çok kayıt defterlerinden çekme](media/container-registry-geo-replication/before-geo-replicate-pull.png)<br />*Birden çok kayıt defterlerinden çekme*

Birden çok kayıt defterleri, tipik zorluklar içerir:

* Kayıt defterinden Batı ABD veri merkezleri görüntüleri bu uzak kapsayıcı konakların her biri çekme gibi çıkış ücretlerinin yansıtılmasını Batı ABD, Doğu ABD, Batı ABD ve Kanada merkezi kümeleri tüm çeker.
* Geliştirme ekibi, Batı ABD ve Batı Avrupa kayıt defterleri görüntüleri göndermelisiniz.
* Geliştirme ekibi, yapılandırmanız ve her bölgesel dağıtım yerel kayıt defterinde başvuran resim adları ile korumak gerekir.
* Kayıt defteri erişimi her bölge için yapılandırılmış olması gerekir.

## <a name="benefits-of-geo-replication"></a>Coğrafi çoğaltma yararları

![Coğrafi olarak çoğaltılmış bir kayıt defterinden çekme](media/container-registry-geo-replication/after-geo-replicate-pull.png)

Azure kapsayıcı kayıt defteri coğrafi çoğaltma özelliğini kullanarak, bu faydaları gerçekleşir:

* Tek bir kayıt defteri tüm bölgeler arasında yönetin: `contoso.azurecr.io`
* Tüm bölgeler aynı resim URL'si kullanılan görüntüsü dağıtımları için tek bir yapılandırması yönetin: `contoso.azurecr.io/public/products/web:1.2`
* Coğrafi yerel bildirimler için bölgesel Web kancalarını da dahil olmak üzere çoğaltma, ACR yönetir ancak tek bir kayıt defteri bildirme

## <a name="configure-geo-replication"></a>Coğrafi çoğaltmayı yapılandırma
Coğrafi çoğaltma yapılandırma, bir harita bölgeler tıklatmak kadar kolaydır.

Coğrafi çoğaltma özelliğidir [Premium kayıt defterleri](container-registry-skus.md) yalnızca. Premium, temel ve standart Premium'da için değiştirebileceğiniz henüz kaydınız değilse [Azure portal](https://portal.azure.com):

![Azure portalında SKU'ları değiştirme](media/container-registry-skus/update-registry-sku.png)

Coğrafi çoğaltma Premium kayıt için yapılandırmak için oturum açtığınızda Azure portalında http://portal.azure.com.

Azure kapsayıcı kaydınız gidin ve **çoğaltmalar**:

![Azure portalı kapsayıcı kayıt defteri kullanıcı arabirimindeki Çoğaltmalar](media/container-registry-geo-replication/registry-services.png)

Geçerli tüm Azure bölgeleri gösteren bir harita görüntülenir:

 ![Azure portalındaki bölge haritası](media/container-registry-geo-replication/registry-geo-map.png)

* Mavi Altıgenler geçerli çoğaltmaları temsil eder
* Yeşil Altıgenler olası çoğaltma bölgeleri temsil eder
* Gri Altıgenler Azure bölgeleri çoğaltma için henüz kullanılamıyor temsil eder

Bir çoğaltmayı yapılandırmak için yeşil Altıgene seçin, sonra seçin **oluşturma**:

 ![Azure portalında çoğaltma oluşturmaya yönelik kullanıcı arabirimi](media/container-registry-geo-replication/create-replication.png)

Ek çoğaltmaları yapılandırmak için diğer bölgeler için yeşil Altıgenler seçin ve ardından **oluşturma**.

ACR görüntüleri genelinde yapılandırılmış çoğaltma eşitleme başlar. Tamamlandıktan sonra portal yansıtır *hazır*. Çoğaltma durum Portalı'nda otomatik olarak güncelleştirmez. Güncelleştirilmiş durumunu görmek için Yenile düğmesini kullanın.

## <a name="geo-replication-pricing"></a>Coğrafi çoğaltma fiyatlandırma

Coğrafi çoğaltma özelliğidir [Premium SKU](container-registry-skus.md) Azure kapsayıcı kayıt defteri. Bir kayıt defteri, istenen bölgelere çoğalttığınızda, her bölge için Premium kayıt defteri ücretleri doğurur.

Önceki örnekte, Doğu ABD, Orta Kanada ve Batı Avrupa çoğaltmaları ekleme Contoso iki kayıt defterleri, aşağıya doğru birleştirildi. Contoso dört kez Premium hiçbir ek yapılandırma veya yönetim ile aylık ödeme. Her bölge, performans, güvenilirlik ağ çıkış ücretleri Kanada Batı ABD ve Doğu ABD olmadan geliştirme yerel olarak resimlerinin şimdi çeker.

## <a name="summary"></a>Özet

Coğrafi çoğaltma ile bir genel bulut olarak bölgesel veri merkezleri yönetebilirsiniz. Görüntüleri birçok Azure Hizmetleri genelinde kullanılan gibi tek bir yönetim düzlemi hızlı ağ close korurken yararlanabilir ve güvenilir yerel görüntü çeker.

## <a name="next-steps"></a>Sonraki adımlar

Üç bölümlü öğretici serisi denetleyin [coğrafi çoğaltma Azure kapsayıcı kayıt defterinde](container-registry-tutorial-prepare-registry.md). Coğrafi olarak çoğaltılmış bir kayıt defteri oluşturma, bir kapsayıcı oluşturma ve tek bir dağıtımı rehberlik `docker push` kapsayıcıları örnekleri için birden çok bölgesel Web uygulamaları için komutu.

> [!div class="nextstepaction"]
> [Azure kapsayıcı kayıt defterinde coğrafi çoğaltma](container-registry-tutorial-prepare-registry.md)