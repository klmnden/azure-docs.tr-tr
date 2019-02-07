---
title: Azure Stack'te Yönetim Portalı'nı kullanarak | Microsoft Docs
description: Azure Stack operatörü, yönetim portalının nasıl kullanılacağını öğrenin.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 02c7ff03-874e-4951-b591-28166b7a7a79
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.date: 01/07/2019
ms.author: jeffgilb
ms.reviewer: ''
ms.lastreviewed: 01/07/2019
ms.openlocfilehash: bda4edeb610fd786307a751afc5b397a2f597e54
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2019
ms.locfileid: "55767478"
---
# <a name="quickstart-use-the-azure-stack-administration-portal"></a>Hızlı Başlangıç: Azure Stack yönetim portalını kullanın

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack'te iki Portal vardır; Yönetim Portalı ve kullanıcı portalı (bazen denir *Kiracı* portal.) Azure Stack operatör Yönetim Portalı, günlük yönetimi ve işlemleri Azure Stack için kullanın.

## <a name="access-the-administrator-portal"></a>Yönetici portalına erişim

Yönetici portalına erişmek için portal URL'si ve Azure Stack operatörü kimlik bilgilerini kullanarak oturum açma göz atın. Tümleşik bir sistem için URL değişir portalı bölge adı ve Azure Stack dağıtımınıza dış tam etki alanı adı (FQDN) bağlı. Yönetim Portalı URL'si, her zaman aynı Azure Stack geliştirme Seti'ni (ASDK) dağıtımlar için. 

| Ortam | Yönetici portalı URL'si |   
| -- | -- | 
| ASDK| https://adminportal.local.azurestack.external  |
| Tümleşik sistemler | https://adminportal.&lt; *bölge*&gt;.&lt; *FQDN*&gt; | 
| | |

> [!TIP]
> ASDK ortamı için önce erişebildiğiniz emin olmanız gerekir [Geliştirme Seti ana bilgisayarına bağlanmak](azure-stack-connect-azure-stack.md) sanal özel ağ (VPN) veya Uzak Masaüstü bağlantısı aracılığıyla.

 ![Yönetim Portalı](media/azure-stack-manage-portals/admin-portal.png)

Tüm Azure Stack dağıtımlar için varsayılan saat dilimini Eşgüdümlü Evrensel Saat (UTC) olarak ayarlanır. 

Yönetici portalı'nda gibi şeyler yapabilir:

* [Azure Stack Azure ile kaydedin](azure-stack-registration.md)
* [Market'i doldurma](azure-stack-download-azure-marketplace-item.md)
* [Planlar, teklifler ve abonelikler için kullanıcılar oluşturun](azure-stack-plan-offer-quota-overview.md)
* [Sistem durumu ve uyarıları izleme](azure-stack-monitor-health.md)
* [Azure Stack güncelleştirmelerini yönetme](azure-stack-updates.md)

**Hızlı başlangıç öğreticisinde** kutucuk en yaygın görevleri için çevrimiçi belgelere bağlantılar sağlar.

Operatör Yönetim Portalı'nda sanal makineler, sanal ağlar ve depolama hesapları gibi kaynaklara karşın, şunları yapmalısınız [kullanıcı portalında oturum açın](user/azure-stack-use-portal.md) oluşturun ve kaynakları test etmek için.

>[!NOTE]
>**Sanal makine oluşturma** hızlı başlangıç Öğreticisi kutucuğunda bağlantı Yönetim Portalı'nda bir sanal makine oluşturma, sahiptir, ancak bu yalnızca Azure Stack başarıyla dağıtıldığını doğrulamak için tasarlanmıştır.

## <a name="understand-subscription-behavior"></a>Abonelik davranışı anlayın

Yönetim Portalı'nda varsayılan olarak oluşturulan üç abonelikleri vardır; Tüketim, varsayılan sağlayıcı ve ölçüm. Operatör, çoğunlukla kullanacağınız *varsayılan sağlayıcı aboneliği*. Başka bir abonelik ekleyin ve bunları Yönetim Portalı'nda kullanabilirsiniz olamaz. 

Diğer abonelikler, temel planlar ve teklifler için oluşturma kullanıcı portalında kullanıcılar tarafından oluşturulur. Ancak, kullanıcı portalı herhangi bir Yönetim Portalı'nın yönetimsel veya işletimsel özelliği için erişim sağlamaz.

Yönetim ve Kullanıcı Portalı, Azure Resource Manager'ın ayrı örnekleri tarafından desteklenir. Bu Azure Resource Manager ayırma nedeniyle, abonelikleri portalları aşmaz. Örneğin, bir Azure Stack operatör olarak, kullanıcı portalında oturum açın, erişemediğiniz *varsayılan sağlayıcı aboneliği*. Tüm yönetim işlevleri için erişiminiz yoksa olsa da, kullanılabilir genel teklifler abonelikleri kendiniz oluşturabilirsiniz. Kullanıcı portalında oturum açmadıysanız sürece bir kiracı kullanıcı olarak kabul edilir.

  >[!NOTE]
  >Bir kullanıcı olarak Azure Stack operatörü aynı Kiracı dizinine dahilse ASDK ortamında, Yöneticiler için yönetim portalında oturum engellenmez. Ancak, bunlar yönetim işlevlerini erişmek veya erişmek için abonelikler sunar, Kullanıcı Portalı'nda kullanılabilir ekleme olamaz.

## <a name="administration-portal-tips"></a>Yönetim Portalı ipuçları

### <a name="customize-the-dashboard"></a>Panoyu özelleştirin

Pano, bir dizi varsayılan kutucukla içerir. Seçebileceğiniz **panoyu Düzenle** varsayılan pano değiştirmek veya **yeni Pano** özel bir pano eklemek için. Kutucukları bir panoya kolayca ekleyebilirsiniz. Örneğin, seçebileceğiniz **+ kaynak Oluştur**, sağ **sunar + planlar**seçip **panoya Sabitle**.

Bazı durumlarda, bir boş Pano portalında görebilirsiniz. Pano kurtarmak için **Pano düzenleme**ve ardından sağ tıklayıp **varsayılan durumuna sıfırlansın**.

### <a name="quick-access-to-online-documentation"></a>Hızlı erişim için çevrimiçi belgeleri

Azure Stack operatör belgeleri erişmek için Yardımı kullanın ve yönetici portalının sağ üst köşesindeki simgeyi (soru işareti) destek. İmlecinizi simgenin üzerine taşıyın ve ardından **Yardım + Destek**.

### <a name="quick-access-to-help-and-support"></a>Yardım ve Destek hızlı erişim

Yönetici portalını sağ üst köşesinde bulunan Yardım ve Destek simgesini (soru işareti) seçin ve ardından **yeni destek isteği**, aşağıdaki sonuçları biri gerçekleşir:

- Bu eylem, tümleşik bir sistem kullanıyorsanız, burada doğrudan bir destek bileti Microsoft Müşteri Destek Hizmetleri (CSS) ile açabileceğiniz bir site açar. Başvurmak [destek alınacağı](azure-stack-manage-basics.md#where-to-get-support) zaman orijinal ekipman üreticisi (OEM) donanım satıcısı Desteğinizi veya Microsoft desteği aracılığıyla gideceğine anlamak için.
- ASDK kullanıyorsanız, bu eylem açar [Azure Stack forumları sitesi](https://social.msdn.microsoft.com/Forums/home?forum=AzureStack) doğrudan. Bu Forum düzenli olarak izlenir. ASDK değerlendirme ortamı olduğundan, Microsoft CSS sunulan resmi desteği yoktur.

### <a name="quick-access-to-the-azure-roadmap"></a>Azure yol haritası hızlı erişim

Seçerseniz **Yardım ve Destek** (soru işareti) Yönetim Portalı ve ardından sağ üst köşesindeki **Azure yol haritası**, yeni bir tarayıcı sekmesi açılır ve sizi Azure yol haritası götürür. Yazarak **Azure Stack** içinde **ürünleri** arama kutusu, tüm Azure Stack yol haritası güncelleştirmeleri görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack Azure ile kaydedin](azure-stack-registration.md) ve doldurma [Azure Stack marketini](azure-stack-marketplace.md) kullanıcılarınıza sunmak için öğeleri. 
