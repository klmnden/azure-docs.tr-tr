---
title: Altyapı ve SAP hana (büyük örnekler) azure'da bağlantı | Microsoft Docs
description: SAP HANA (büyük örnekler) Azure'da kullanmak için gerekli bağlantı altyapıyı yapılandırın.
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 05bd09d3ab05f3ce426126e5629523fba087dad9
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67707305"
---
# <a name="sap-hana-large-instances-deployment"></a>SAP HANA (büyük örnekler) dağıtım 

Bu makale SAP hana (büyük örnekler) azure'da satın alma işleminiz tamamladıktan Microsoft'tan varsayar. Genel arka plan için bu makaleyi okumadan önce bkz [HANA büyük örnekler, yaygın terimlerin](hana-know-terms.md) ve [HANA büyük örnekler SKU'ları](hana-available-skus.md).


Microsoft, HANA büyük örnek birimleri dağıtmak için aşağıdaki bilgileri gerektirir:

- Müşteri adı.
- İş iletişim bilgileri (dahil olmak üzere e-posta adresi ve telefon numarası).
- Teknik iletişim bilgilerini (dahil olmak üzere e-posta adresi ve telefon numarası).
- Teknik ağ iletişim bilgileri (e-posta adresi ve telefon numarası dahil).
- Azure Dağıtım bölgesi (örneğin, Batı ABD, Avustralya Doğu veya Kuzey Avrupa).
- SAP HANA (büyük örnekler) azure'da SKU (yapılandırma).
- Her Azure Dağıtım bölgesi için:
    - Bir/29 HANA büyük örnekler için Azure sanal ağları bağlama ER P2P bağlantıları için IP adresi aralığı.
    - Bir/24 CIDR bloğu HANA büyük örnekler sunucu IP havuzu için kullanılır.
- Sanal ağ adres alanı özniteliğinde HANA büyük örnekleri bağlanan her Azure sanal ağının IP adresi aralığı değerleri.
- Verileri her HANA büyük örnekleri sistemi için:
  - İdeal olarak bir tam etki alanı adı ile istenen konak adı.
  - Sunucu IP havuzu adres aralığı dışında HANA büyük örnek birim için istenen IP adresi. (Sunucu IP havuzu adres aralığı ilk 30 IP adresleriyle HANA büyük örnekleri içinde iç kullanım için ayrılmıştır.)
  - SAP HANA SID (SAP HANA ile ilgili gerekli disk birimler oluşturmak için gerekli) SAP HANA örneği adı. Microsoft, HANA SID sidadm izinlerini bağlı NFS birimleri oluşturmak için gerekir. Bu birimleri HANA büyük örnek birimine bağlayın. HANA SID de takılı disk birimlerinin adı bileşenlerinden biri kullanılır. Biriminde birden çok HANA örneği çalıştırmak istiyorsanız, birden çok HANA SID listelemelisiniz. Her birine atanan birimleri ayrı kümesini alır.
  - Linux işletim sisteminde sidadm kullanıcı grubu kimliğini sahiptir. Bu kimliği, SAP HANA ile ilgili gerekli disk birimler oluşturmak için gereklidir. SAP HANA yüklemesi, genellikle 1001 ile bir grup kimliği sapsys grubu oluşturur. Bu grubun parçası sidadm kullanıcıdır.
  - Linux işletim sisteminde sidadm kullanıcı bir kullanıcı kimliği vardır. Bu kimliği, SAP HANA ile ilgili gerekli disk birimler oluşturmak için gereklidir. Birden çok HANA örnekleri birim üzerinde çalıştırıyorsanız, tüm sidadm kullanıcıları listeler. 
- Azure abonelik kimliği için hangi Azure HANA üzerinde SAP HANA büyük örnekleri doğrudan bağlı olacak bir Azure aboneliği. Bu abonelik kimliği HANA büyük örnek birim veya birim ile ücretlendirilmeye devam Azure aboneliğini başvuruyor.

Yukarıdaki bilgiler verdikten sonra Microsoft Azure (büyük örnekler) üzerinde SAP HANA sağlar. Microsoft Azure sanal ağlarınıza HANA büyük örneklerine bağlanmak için bilgi gönderir. Ayrıca, HANA büyük örnek birimleri de erişebilirsiniz.

Microsoft, dağıtıldıktan sonra HANA büyük örneklerine bağlanmak için aşağıdaki sırayı kullanın:

1. [Azure Vm'lerini bağlamak HANA büyük örnekleri](hana-connect-azure-vm-large-instances.md)
2. [HANA büyük örnekleri ExpressRoute sanal ağ bağlama](hana-connect-vnet-express-route.md)
3. [(İsteğe bağlı) ek donanım gereksinimleri](hana-additional-network-requirements.md)

