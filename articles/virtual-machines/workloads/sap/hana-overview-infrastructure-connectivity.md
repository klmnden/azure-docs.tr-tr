---
title: Altyapı ve SAP hana (büyük örnekler) azure'da bağlantı | Microsoft Docs
description: SAP HANA (büyük örnekler) Azure'da kullanmak için gerekli bağlantı altyapıyı yapılandırın.
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c61dffc6fd9d0c65f5e925daebdf2a02cfb5d6ba
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44161365"
---
# <a name="sap-hana-large-instances-deployment"></a>SAP HANA (büyük örnekler) dağıtım 

SAP HANA (büyük örnekler) azure'da satın siz ve Microsoft Kurumsal hesap ekibinin arasında sonlandırıldıktan sonra HANA büyük örneği birimleri dağıtmak için Microsoft tarafından aşağıdaki bilgiler gereklidir:

- Müşteri adı
- İş iletişim bilgileri (e-posta adresi ve telefon numarası dahil)
- Teknik iletişim bilgileri (e-posta adresi ve telefon numarası dahil)
- Teknik ağ iletişim bilgileri (e-posta adresi ve telefon numarası dahil)
- Azure Dağıtım bölgesi (Batı ABD, Doğu ABD, Avustralya Doğu, Avustralya Güneydoğu, Batı Avrupa ve Kuzey Avrupa Temmuz 2017'den itibaren)
- SAP HANA (büyük örnekler) SKU (yapılandırma) Azure üzerinde onaylayın
- Zaten genel bakış ve mimari belgede dağıtılıyor her Azure bölgesi için HANA büyük örnekler için ayrıntılı:
    - Bir/29 ER P2P bağlantıları, HANA büyük örnekler için Azure Vnet'leri bağlamak için IP adresi aralığı
    - Bir/24 CIDR bloğu HANA büyük örnekleri sunucu IP havuzu için kullanılan
- Her Azure HANA büyük örnekleri bağlayan vnet'in VNet adres alanı özniteliğinde kullanılan IP adres aralığı değeri
- Verileri her HANA büyük örnekleri sistemi için:
  - İstenen ana bilgisayar - ideal olarak tam etki alanı adına sahip.
  - Sunucu IP havuzu adres aralığı dışında-HANA büyük örneği birim için istenen IP adresini kullanmaya devam aklınızda sunucu IP havuzu adres aralığı ilk 30 IP adresleriyle HANA büyük örnekleri içinde iç kullanım için ayrılmıştır
  - SAP HANA SID (SAP HANA ile ilgili gerekli disk birimler oluşturmak için gerekli) SAP HANA örneği adı. HANA SID, HANA büyük örneği birimine bağlı NFS birimlerde sidadm izinlerini oluşturmak için gereklidir. Ayrıca, bağlı disk birimlerinin adı bileşenlerinden biri kullanılır. Biriminde birden çok HANA örneği çalıştırmak istiyorsanız, birden çok HANA SID listesi gerekir. Her birine atanan birimleri ayrı kümesini alır.
  - Linux işletim sisteminde sidadm kullanıcının GroupID, SAP HANA ile ilgili gerekli disk birimleri oluşturmak için gereklidir. SAP HANA yüklemesi, genellikle 1001 ile bir grup kimliği sapsys grubu oluşturur. Sidadm kullanıcı o grubun bir parçasıdır
  - Linux işletim sisteminde sidadm kullanıcının kullanıcı kimliği, SAP HANA ile ilgili gerekli disk birimleri oluşturmak için gereklidir. Birden çok HANA örnekleri birim üzerinde çalıştırıyorsanız, tüm listelemek gereken <sid>adm kullanıcılar 
- Azure abonelik kimliği için hangi Azure HANA üzerinde SAP HANA büyük örnekleri doğrudan bağlı olacak bir Azure aboneliği. Bu abonelik kimliği ile HANA büyük örneği birimi ücreti gidip Azure aboneliği başvuruyor.

Bilgileri verdikten sonra Microsoft SAP HANA (büyük örnekler) azure'da sağlar ve HANA büyük örnekleri, Azure sanal ağları bağlamak ve HANA büyük örneği birimleri erişmek için gerekli bilgileri döndürür.

Bu makalede okumadan önce hakkında bilgi edinin [HANA büyük örnekleri genel koşulları](hana-know-terms.md) ve [HANA büyük örnekleri SKU'ları](hana-available-skus.md).

Microsoft tarafından dağıtıldıktan sonra HANA büyük örneklerine bağlanmak için aşağıdaki sırayı kullanabilirsiniz:

1. [Azure sanal makineleri HANA büyük örneklerine bağlanma](hana-connect-azure-vm-large-instances.md)
2. [HANA büyük örnekler ExpressRoute için sanal ağ bağlama](hana-connect-vnet-express-route.md)
3. [(İsteğe bağlı) ek donanım gereksinimleri](hana-additional-network-requirements.md)

**Sonraki adımlar**

- Başvuru [HANA büyük örnekler için Azure Vm'lerini bağlamak](hana-connect-azure-vm-large-instances.md).
- Başvuru [HANA büyük örnek ExpressRoute için bir VNet bağlama](hana-connect-vnet-express-route.md).