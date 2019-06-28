---
title: İstemciler Microsoft Azure FXT Edge dosyalayıcı kümede bağlama
description: NFS istemci makineleri Azure FXT Edge dosyalayıcı karma depolama önbelleği nasıl bağlayabilir
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: tutorial
ms.date: 06/20/2019
ms.author: v-erkell
ms.openlocfilehash: dd74b82d4edad2b0176a3724176d924c2387f6fe
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67450327"
---
# <a name="tutorial-mount-the-cluster"></a>Öğretici: Küme bağlama

Bu öğreticide Azure FXT Edge dosyalayıcı kümeye NFS istemcilerinin bağlama öğretir. İstemciler, arka uç depolama eklendiğinde size atanan sanal ad alanı yollar bağlayın. 

Bu öğretici öğretir: 

> [!div class="checklist"]
> * Stratejileri istemciye yönelik IP adresleri aralığı karşı istemcileri yükleme
> * İstemciye yönelik IP adresi ve ad alanı birleşim bağlama yolundan oluşturmak nasıl
> * Bir bağlama komutu kullanmak için hangi bağımsız değişkenleri

Bu öğreticinin tamamlanması yaklaşık 45 dakika sürer.

## <a name="steps-to-mount-the-cluster"></a>Kümeye bağlanacak adımları

İstemci makineler Azure FXT Edge dosyalayıcı kümenize bağlanmak için aşağıdaki adımları izleyin.

1. Karar, küme düğümleri arasında istemci trafik yükünü dengele öğreneceksiniz. Okuma [istemci Yük Dengeleme](#balance-client-load), aşağıdaki Ayrıntılar için. 
1. Bağlama için küme IP adresi ve bağlantı yolunu belirleyin.
1. İstemciye yönelik bağlama yolunu belirleyin.
1. Sorunu [bağlama komutu](#use-recommended-mount-command-options), uygun bağımsız değişkenlerle.

## <a name="balance-client-load"></a>İstemci Yük Dengeleme

Dengeleme istemci isteklerini kümedeki tüm düğümler arasında yardımcı olmak için tam istemci'e dönük IP adresleri aralığını istemcilere bağlamak. Bu görevi otomatik hale getirmek için birkaç yolu vardır.

Hepsini bir kez deneme DNS Yük Dengeleme için kümesi hakkında bilgi edinmek için [Azure FXT Edge dosyalayıcı kümesi için DNS yapılandırma](fxt-configure-network.md#configure-dns-for-load-balancing). Bu yöntemi kullanmak için aşağıdaki makalelerde açıklanan olmayan bir DNS sunucusu sürdürmeniz gerekir.

Küçük yüklemeleri için daha basit bir yöntem istemci bağlama zaman aralığı boyunca IP adresleri atamak için bir komut dosyası kullanmaktır. 

Diğer Yük Dengeleme yöntemleriyle büyük veya karmaşık sistemleri için uygun olabilir. Microsoft temsilcinize başvurun veya Yardım için bir destek isteği açın. (Şu anda azure Load Balancer *desteklenmiyor* ile Azure FXT Edge dosyalayıcı.)

## <a name="create-the-mount-command"></a>Bağlama komutu oluştur 

İstemcisinden ``mount`` komutu, yerel dosya sisteminde bir yolu Azure FXT Edge dosyalayıcı kümedeki sanal sunucu (vserver) eşler. 

Biçimi ``mount <FXT cluster path> <local path> {options}``

Bağlama komut üç öğe vardır: 

* Küme yol - aşağıda açıklanan IP adresi ve ad alanı birleşim yolu birleşimi
* Yerel yol - istemci üzerindeki yol 
* komut seçenekleri - bağlama (listelenen [kullanmak önerilen bağlama komut seçeneklerini](#use-recommended-mount-command-options))

### <a name="create-the-cluster-path"></a>Küme yol oluşturma

Küme yolu vserver birleşimidir *IP adresi* yolu artı bir *ad alanı birleşim*. Ad alanı birleşim ne zaman tanımlanan sanal bir yol olduğunu, [depolama sistemi eklenen](fxt-add-storage.md#create-a-junction).

Örneğin, kullandıysanız ``/avere/files`` , ad alanı yolu, istemcilerinizin bağlaması *IP*: / avere/dosyalarını kendi yerel bağlama noktasına. 

<!-- to do: update screenshot to use fxt/files instead of avere/files -->

!["Yeni birleşim Ekle" iletişim/avere/dosyalarıyla ad alanı yolu alanı](media/fxt-mount/fxt-create-junction-example.png)

IP adresini istemciye yönelik IP adresleri için vserver tanımlı biridir. Denetim Masası kümesindeki iki yerde IP'ler istemciye yönelik çeşitli bulabilirsiniz:

* **VServers** tablo (Pano sekmesi) - 

  ![Daire içinde VServer sekmesi, graf ve IP adresi bölümü altında veri tablosundaki seçili Denetim Masası Pano sekmesi](media/fxt-mount/fxt-ip-addresses-dashboard.png)

* **İstemci'e yönelik ağ** Ayarları sayfası - 

  ![Ayarlar > VServer > Tablo belirli vserver için adres aralığı bölümünü etrafında bir daire ile istemci bakan ağ yapılandırma sayfası](media/fxt-mount/fxt-ip-addresses-settings.png)

IP adresi ve bağlama komutu için küme yolu oluşturmak için ad alanı yolu birleştirin. 

Örnek istemci bağlama komutu: ``mount 10.0.0.12:/sd-access /mnt/fxt {options}``

### <a name="create-the-local-path"></a>Yerel yol oluşturma

Bağlama komutu için yerel yol size bağlıdır. Yol yapısını istediğiniz sanal ad alanı bir parçası olarak ayarlayabilirsiniz. Ad alanı ve istemci iş akışınız için uygun olan yerel yolu tasarlayın. 

İstemciye yönelik ad alanı hakkında daha fazla bilgi için küme yapılandırması kılavuz okuma [ad alanı genel bakış](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gns_overview.html).

Yol yanı sıra eklemeyi [bağlama komut seçenekleri](#use-recommended-mount-command-options) her istemci bağlarken aşağıda açıklanmıştır.

### <a name="use-recommended-mount-command-options"></a>Önerilen bağlama komut seçeneklerini kullanın

Sorunsuz istemci bağlama emin olmak için bu ayarları ve bağımsız değişkenler, bağlama komutu geçirin: 

``mount -o hard,nointr,proto=tcp,mountproto=tcp,retry=30 ${VSERVER_IP_ADDRESS}:/${NAMESPACE_PATH} ${LOCAL_FILESYSTEM_MOUNT_POINT}``

| Gerekli ayarları | |
--- | --- 
``hard`` | Yazılım takar Azure FXT Edge dosyalayıcı kümeye uygulama hatalarına ve olası veri kaybı ile ilişkilidir. 
``proto=netid`` | Bu seçenek NFS ağ hataları uygun olarak işlenmesini destekler.
``mountproto=netid`` | Bu seçenek, bağlama işlemleri için uygun ağ hatalarının işlenmesini destekler.
``retry=n`` | Ayarlama ``retry=30`` geçici bağlama hatalarını önlemek için. (Farklı bir değer ön plan takar önerilir.)

| Tercih edilen ayarları  | |
--- | --- 
``nointr``            | İstemcilerinize bu seçeneği desteklemek daha eski işletim sistemi çekirdek (2008'den önceki Nisan) kullanıyorsanız, bunu kullanın. ' % S'seçeneği "Giriş" varsayılandır.

## <a name="next-steps"></a>Sonraki adımlar

İstemciler bağladınız sonra iş akışınızı test edebilir ve kümenizle kullanmaya başlayın.

Paralel verileri kullanarak önbellek yapısı avantajlarından verileri taşımak için yeni bir bulut çekirdek dosyalayıcı gerekiyorsa ele alın. Bazı stratejiler açıklanmıştır [vFXT kümeye veri taşıma](https://docs.microsoft.com/azure/avere-vfxt/avere-vfxt-data-ingest). (Azure Avere vFXT Azure FXT Edge gösterecek şekilde çok benzer önbelleğe alma teknolojisini kullanan bulut tabanlı bir ürün var.)

Okuma [İzleyici Azure FXT Edge dosyalayıcı donanım durumunu](fxt-monitor.md) donanım sorunları gidermek gerekiyorsa. 
