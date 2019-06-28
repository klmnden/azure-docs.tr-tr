---
title: Azure veri bilimi sanal makinelerini kullanın
description: İçin bir Azure veri bilimi sanal makinesi (Azure not defterleri için kullanılabilir olan işlem gücünü genişletmek için DSVM) bağlanın.
services: app-service
documentationcenter: ''
author: getroyer
manager: andneil
ms.assetid: 0ccc2529-e17f-4221-b7c7-9496d6a731cc
ms.service: azure-notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2019
ms.author: getroyer
ms.openlocfilehash: 0ac50a5f52682c4315b8d08cf5632c4a6fa5242f
ms.sourcegitcommit: a7ea412ca4411fc28431cbe7d2cc399900267585
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2019
ms.locfileid: "67357579"
---
# <a name="use-azure-data-science-virtual-machines"></a>Azure veri bilimi sanal makinelerini kullanın

Varsayılan olarak, projeler çalıştıracağınız **ücretsiz işlem** katmanı, kötüye kullanımı önlemek için 4 GB bellek ve veri 1 GB ile sınırlıdır. Bir Azure aboneliğinde sağladıktan farklı bir sanal makine kullanarak bu kısıtlamaları devre dışı bırakabilir. Bu amaç için bir Azure veri bilimi sanal makinesi (DSVM) en iyi seçenek olduğu kullanarak **Linux (Ubuntu) için veri bilimi sanal makinesi** görüntü. Böyle bir DSVM her şeyi içeren Azure not defterleri için gereken ve otomatik görünür önceden yapılandırılmış olarak gelir **çalıştırma** aşağı açılan listeden Azure not defterleri.

> [!Note]
> Azure not defterleri yalnızca üzerinde bir Linux Ubuntu görüntüsünü ile oluşturulan Dsvm'leri üzerinde desteklenir. Dizüstü bilgisayarlar, Windows 2012, Windows 2016 ya da Linux CentOS görüntülerinde desteklenmez.

## <a name="create-a-dsvm-instance"></a>DSVM örneği oluşturma

Yeni bir DSVM örneği oluşturmak için yönergeleri takip edin [Ubuntu veri bilimi sanal makinesi oluşturma](/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro). Fiyatlandırma ayrıntıları dahil olmak üzere daha fazla bilgi için bkz. [veri bilimi sanal makineleri](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/).

## <a name="connect-to-the-dsvm"></a>Değerini DSVM Örneğinize bağlanın

DSVM oluşturulduktan seçin **çalıştırma** Azure not defterleri aşağı açılan listede proje panosu ve uygun DSVM örneğini seçin. Aşağı açılan listesinde, aşağıdaki koşullar doğruysa, DSVM örnekleri gösterilmektedir:

- Azure Active Directory (AAD), bir şirket hesabı gibi kullanan bir hesapla Azure not defterlerine oturumunuz.
- Hesabınız bir Azure aboneliğine bağlı.
- Bu Abonelikteki bir veya daha fazla sanal makineler ile en az sahip olduğunuz veri bilimi sanal makinesi için Linux (Ubuntu) görüntüsü kullanan okuyucu erişimi.)

![Proje panosu açılır listede veri bilimi sanal makine örnekleri](media/project-compute-tier-dsvm.png)

Azure not defterleri DSVM örneği seçtiğinizde, sanal Makineyi oluştururken kullanılan belirli bir makine kimlik bilgilerini isteyebilir.

Koşullardan herhangi biri karşılanmadığı takdirde DSVM'ye bağlanmaya devam edebilirler. Aşağı açılan listesinde seçin **doğrudan işlem** seçeneği (listesinde gösterilecek) bir ad, sanal makinenin IP adresi ve 8000 numaralı bağlantı noktasını (genellikle, hangi JupyterHub dinlediği varsayılan bağlantı noktasına) ve VM kimlik bilgileri ister:

![Doğrudan işlem seçeneği sunucu bilgilerini toplamak için sor](media/project-compute-tier-direct.png)

DSVM sayfasından Azure portalında bu değerleri alın.

## <a name="accessing-azure-notebooks-files-from-the-dsvm"></a>DSVM Azure not defterleri dosyalardan erişme

Dosya sistemi erişimini DSVM sürümleri 19.06.15 veya üzeri. Sürümü denetlemek için önce DSVM'ye (IP adresini Azure portalında kullanılabilir) SSH aracılığıyla bağlanın. Ardından aşağıdaki komutla çalıştırın, `<ip_address>`: `curl -H Metadata:true "http://<ip_address>/metadata/instance?api-version=2018-10-01"`. Sürüm numarası, "Sürüm" için çıktıda gösterilir.

Dosya yollarının ile eşlik korumak için **ücretsiz işlem** katmanı tarafından yalnızca bir DSVM üzerinde bir seferde bir proje açın. Yeni bir proje açmak için açık projenin ilk kapatmanız gerekir.

Bir VM üzerinde bir proje çalıştırıldığında, dosyaları (dizin JupyterHub gösterilen), Jupyter sunucu kök dizininin bağlı varsayılan Azure not defterleri dosyaları değiştirme. Ne zaman bilgisayarı tuşunu kullanarak VM'yi **kapatma** not defterini kullanıcı Arabirimi, Azure Not Defterleri düğmede varsayılan dosyaları geri yükler.

![Azure not defterlerinde kapatma düğmesi](media/shutdown.png)

## <a name="create-new-dsvm-users"></a>Yeni DSVM kullanıcılar oluşturma

Birden çok kullanıcı bir DSVM paylaşıyorsanız, birbirine oluşturarak ve her bir not defteri kullanıcı için DSVM kullanan engelleme önleyebilirsiniz:

1. Üzerinde [Azure portalı](https://portal.azure.com), sanal makinenize gidin.
1. Altında **destek + sorun giderme** sol kenar boşluğunda seçin **parolayı Sıfırla**.
1. Yeni kullanıcı adı ve parola girin ve seçin **güncelleştirme**. (Mevcut kullanıcı adlarını etkilenmez.)
1. Tüm kullanıcılar için önceki adımı yineleyin.

## <a name="next-steps"></a>Sonraki adımlar

Dsvm'leri hakkında daha fazla bilgi [giriş için Azure veri bilimi sanal makineleri](/azure/machine-learning/data-science-virtual-machine/overview).
