---
author: jluk
ms.service: cloud-shell
ms.topic: persist-storage
ms.date: 9/7/2018
ms.author: juluk
ms.openlocfilehash: c28441b6fe25b3480a55b79682d5067b19e3023a
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188709"
---
# <a name="persist-files-in-azure-cloud-shell"></a>Azure Cloud Shell, dosyaların kalıcı olması
Cloud Shell'i dosyaları oturumlarda kalıcı hale getirilmesi için Azure dosya depolama kullanır. İlk Başlat, Cloud Shell'i dosyaları oturumlarda kalıcı hale getirmek için yeni veya varolan bir dosya paylaşımını ilişkilendirmek isteyip istemediğinizi sorar.

> [!NOTE]
> Bash ve PowerShell aynı dosya paylaşımını paylaşın. Yalnızca bir dosya paylaşımı, Cloud shell'de otomatik olarak bağlama ile ilişkilendirilebilir.

## <a name="create-new-storage"></a>Yeni depolama oluşturma

Cloud Shell, desteklenen bölgede size en yakın olanında, sizin adınıza üç kaynak oluşturur, temel ayarları kullanın ve yalnızca bir abonelik seçin:
* Kaynak grubu: `cloud-shell-storage-<region>`
* Depolama hesabı: `cs<uniqueGuid>`
* Dosya Paylaşımı: `cs-<user>-<domain>-com-<uniqueGuid>`

![Abonelik ayarı](../articles/cloud-shell/media/persisting-shell-storage/basic-storage.png)

Dosya Paylaşımı bağlar olarak `clouddrive` içinde `$Home` dizin. Bu tek seferlik bir işlemdir ve dosya paylaşımı sonraki her oturumda otomatik olarak bağlar. 

> [!NOTE]
> Güvenlik için her bir kullanıcı kendi depolama hesaplarını sağlamanız gerekir.  Rol tabanlı erişim denetimi (RBAC), kullanıcılara katkıda bulunan erişimine sahip veya hesap düzeyinde depolama alanı yukarıda gerekir.

Dosya paylaşımını da sizin için otomatik olarak oluşturulduğu bir 5 GB'lık görüntüyü içeren veri devam ederse, `$Home` dizin. Bu, hem Bash hem PowerShell için geçerlidir.

## <a name="use-existing-resources"></a>Var olan kaynakları kullan

Gelişmiş seçeneğini kullanarak, mevcut kaynaklar ilişkilendirebilirsiniz. Cloud Shell bölgesi seçerken birlikte aynı bölgede bulunan bir yedekleme depolama hesabı seçmeniz gerekir. Örneğin, bölgenize atanan ise daha Batı ABD, Batı ABD içinde de bulunduğu bir dosya paylaşımı ilişkilendirmeniz gerekir.

Depolama Kurulum istemi göründüğünde seçin **Gelişmiş ayarları göster** ek seçenekleri görmek için. Yerel olarak yedekli depolama (LRS), coğrafi olarak yedekli depolama (GRS) ve bölgesel olarak yedekli depolama (ZRS) hesapları için doldurulmuş depolama seçenekleri filtre. 

> [!NOTE]
> GRS veya ZRS kullanan depolama hesapları, yedekleme dosya paylaşımı için ek dayanıklılık için önerilir. Yedeklilik türü, hedefler ve fiyat tercih bağlıdır. [Azure depolama hesapları için çoğaltma seçenekleri hakkında daha fazla bilgi](https://docs.microsoft.com/azure/storage/common/storage-redundancy).

![Kaynak grubu ayarı](../articles/cloud-shell/media/persisting-shell-storage/advanced-storage.png)

### <a name="supported-storage-regions"></a>Desteklenen depolama bölgeleri
Azure depolama hesapları için bağlama Cloud Shell makine ile aynı bölgede bulunmalıdır ilişkili. Çalışabilir, geçerli bölge bulmayı `env` bash değişkeni bulun `ACC_LOCATION`. Dosya paylaşımları, kalıcı hale getirmek oluşturduğunuz bir 5 GB'lık görüntüsü almak, `$Home` dizin.

Cloud Shell makineler aşağıdaki bölgelerde mevcuttur:

|Alan|Bölge|
|---|---|
|Kuzey ve Güney Amerika|Doğu ABD, Güney Orta ABD, Batı ABD|
|Avrupa|Kuzey Avrupa, Batı Avrupa|
|Asya Pasifik|Hindistan Orta, Güneydoğu Asya|

## <a name="restrict-resource-creation-with-an-azure-resource-policy"></a>Bir Azure kaynak İlkesi ile kaynak oluşturmayı kısıtla
Cloud Shell'de oluşturduğunuz depolama hesapları ile etiketlenmiş `ms-resource-usage:azure-cloud-shell`. Kullanıcıların depolama hesapları, Cloud Shell'de oluşturmasını istiyorsanız oluşturma bir [etiketleri için bir Azure kaynak ilkesinden](../articles/azure-policy/json-samples.md) bu belirli bir etikete göre tetiklenir.
