---
title: Avere vFXT - Azure'ı bağlama
description: Azure için Avere vFXT istemcilerle takma
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 41065b4ac6bc486e204c2bfd72b78ba8722270c4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60409384"
---
# <a name="mount-the-avere-vfxt-cluster"></a>Avere vFXT kümesini takma  

İstemci makineler vFXT kümenize bağlanmak için aşağıdaki adımları izleyin.

1. Karar, küme düğümleri arasında istemci trafik yükünü dengele öğreneceksiniz. Okuma [istemci Yük Dengeleme](#balance-client-load), aşağıdaki Ayrıntılar için. 
1. Bağlama için IP adresi ve bağlantı yolunu belirleyin.
1. Sorunu [bağlama komutu](#mount-command-arguments), uygun bağımsız değişkenlerle.

## <a name="balance-client-load"></a>İstemci Yük Dengeleme

Dengeleme istemci isteklerini kümedeki tüm düğümler arasında yardımcı olmak için tam istemci'e dönük IP adresleri aralığını istemcilere bağlamak. Bu görevi otomatik hale getirmek için basit birkaç yolu vardır.

> [!TIP] 
> Diğer Yük Dengeleme yöntemi büyük veya karmaşık sistemleri için uygun olabilir; [bir destek bileti açın](avere-vfxt-open-ticket.md#open-a-support-ticket-for-your-avere-vfxt) Yardım.)
> 
> Otomatik sunucu tarafı Yük Dengeleme için DNS sunucusu kullanmak istiyorsanız, ayarlayın ve Azure içinde kendi DNS sunucunuzu yönetin. Bu durumda, bu belgede göre vFXT kümesi için DNS hepsini bir kez deneme yapılandırabilirsiniz: [Avere küme DNS yapılandırması](avere-vfxt-configure-dns.md).

### <a name="sample-balanced-client-mounting-script"></a>Örnek dengeli istemci betiği oluşturma

Bu kod örneği, tüm vFXT kümenin kullanılabilir IP adreslerini istemcilere dağıtmak için randomizing bir öğe olarak istemci IP adreslerini kullanır.

```bash
function mount_round_robin() {
    # to ensure the nodes are spread out somewhat evenly the default 
    # mount point is based on this node's IP octet4 % vFXT node count.
    declare -a AVEREVFXT_NODES="($(echo ${NFS_IP_CSV} | sed "s/,/ /g"))"
    OCTET4=$((`hostname -i | sed -e 's/^.*\.\([0-9]*\)/\1/'`))
    DEFAULT_MOUNT_INDEX=$((${OCTET4} % ${#AVEREVFXT_NODES[@]}))
    ROUND_ROBIN_IP=${AVEREVFXT_NODES[${DEFAULT_MOUNT_INDEX}]}

    DEFAULT_MOUNT_POINT="${BASE_DIR}/default"

    # no need to write again if it is already there
    if ! grep --quiet "${DEFAULT_MOUNT_POINT}" /etc/fstab; then
        echo "${ROUND_ROBIN_IP}:${NFS_PATH}    ${DEFAULT_MOUNT_POINT}    nfs hard,nointr,proto=tcp,mountproto=tcp,retry=30 0 0" >> /etc/fstab
        mkdir -p "${DEFAULT_MOUNT_POINT}"
        chown nfsnobody:nfsnobody "${DEFAULT_MOUNT_POINT}"
    fi
    if ! grep -qs "${DEFAULT_MOUNT_POINT} " /proc/mounts; then
        retrycmd_if_failure 12 20 mount "${DEFAULT_MOUNT_POINT}" || exit 1
    fi   
} 
```

Yukarıdaki işlevi kullanılabilir Batch örnek parçasıdır [Avere vFXT örnekler](https://github.com/Azure/Avere#tutorials) site.

## <a name="create-the-mount-command"></a>Bağlama komutu oluştur 

> [!NOTE]
> Bağlantısındaki Avere vFXT kümenizi oluştururken, yeni bir Blob kapsayıcı oluşturmadıysanız, [depolamayı yapılandırma](avere-vfxt-add-storage.md) istemcileri bağlamaya çalışırken önce.

İstemcisinden ``mount`` komutu, yerel dosya sisteminde bir yola vFXT kümesinde sanal sunucu (vserver) eşler. Biçimi ``mount <vFXT path> <local path> {options}``

Bağlama komut üç öğe vardır: 

* vFXT yol - (aşağıda açıklanan IP adresi ve ad alanı birleşim yolu bir birleşimi)
* Yerel yol - istemci üzerindeki yol 
* komut seçenekleri - bağlama (listelenen [bağlama komut satırı bağımsız değişkenlerini](#mount-command-arguments))

### <a name="junction-and-ip"></a>Birleşim ve IP

Bir birleşimi vserver yoludur, *IP adresi* yolu artı bir *ad alanı birleşim*. Ad alanı birleşim depolama sistemi eklendiğinde, tanımlanan sanal bir yoludur.

Kümenizi, Blob Depolama ile oluşturulmuş olsa bile, ad alanı yoludur `/msazure`

Örnek: ``mount 10.0.0.12:/msazure /mnt/vfxt``

Küme oluşturulduktan sonra depolama eklediyseniz, ad alanı birleşim yolun içinde ayarlanan değere karşılık gelen **Namespace yolu** birleşim oluştururken. Örneğin, kullandıysanız ``/avere/files`` , ad alanı yolu, istemcilerinizin bağlaması *IP*: / avere/dosyalarını kendi yerel bağlama noktasına.

!["Yeni birleşim Ekle" iletişim/avere/dosyalarıyla ad alanı yolu alanı](media/avere-vfxt-create-junction-example.png)


IP adresini istemciye yönelik IP adresleri için vserver tanımlı biridir. İstemciye yönelik iki yerde Avere Denetim Masası'nda IP'ler, aralığın bulabilirsiniz:

* **VServers** tablo (Pano sekmesi) - 

  ![Daire içinde VServer sekmesi, graf ve IP adresi bölümü altında veri tablosundaki seçili Avere Denetim Masası Pano sekmesi](media/avere-vfxt-ip-addresses-dashboard.png)

* **İstemci'e yönelik ağ** Ayarları sayfası - 

  ![Ayarlar > VServer > Tablo belirli vserver için adres aralığı bölümünü etrafında bir daire ile istemci bakan ağ yapılandırma sayfası](media/avere-vfxt-ip-addresses-settings.png)

Yol yanı sıra eklemeyi [bağlama komut satırı bağımsız değişkenlerini](#mount-command-arguments) her istemci bağlarken aşağıda açıklanmıştır.

### <a name="mount-command-arguments"></a>Komut satırı bağımsız değişkenlerini bağlama

Sorunsuz istemci bağlama emin olmak için bu ayarları ve bağımsız değişkenler, bağlama komutu geçirin: 

``mount -o hard,nointr,proto=tcp,mountproto=tcp,retry=30 ${VSERVER_IP_ADDRESS}:/${NAMESPACE_PATH} ${LOCAL_FILESYSTEM_MOUNT_POINT}``


| Gerekli ayarları | |
--- | --- 
``hard`` | Yazılım takar vFXT kümeye uygulama hatalarına ve olası veri kaybı ile ilişkilidir. 
``proto=netid`` | Bu seçenek NFS ağ hataları uygun olarak işlenmesini destekler.
``mountproto=netid`` | Bu seçenek, bağlama işlemleri için uygun ağ hatalarının işlenmesini destekler.
``retry=n`` | Ayarlama ``retry=30`` geçici bağlama hatalarını önlemek için. (Farklı bir değer ön plan takar önerilir.)

| Tercih edilen ayarları  | |
--- | --- 
``nointr``            | "nointr" seçeneği ile eski çekirdekler (Nisan 2008 önce) bu seçeneği desteklemek istemciler için tercih edilir. ' % S'seçeneği "Giriş" varsayılan olduğuna dikkat edin.


## <a name="next-steps"></a>Sonraki adımlar

İstemciler bağladınız sonra arka uç veri depolama (çekirdek dosyalayıcı) doldurmak için kullanabilirsiniz. Ek kurulum görevleri hakkında daha fazla bilgi için bu belgelere bakın:

* [Verileri taşımak için küme çekirdek dosyalayıcı](avere-vfxt-data-ingest.md) -verimli bir şekilde verilerinizi karşıya yüklemek için birden çok istemci ve iş parçacığı kullanma
* [Küme ayarlama özelleştirme](avere-vfxt-tuning.md) -küme ayarlarını yükünüz uyacak şekilde uyarlayın
* [Kümeyi yönetmek](avere-vfxt-manage-cluster.md) -başlatma veya kümeyi durdurun ve düğümlerini yönetme
