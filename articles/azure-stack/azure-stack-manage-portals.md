---
title: Azure Stack'te Yönetici portalını kullanma | Microsoft Docs
description: Azure Stack operatörü, Yönetici portalını nasıl kullanacağınızı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 02c7ff03-874e-4951-b591-28166b7a7a79
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2018
ms.author: mabrigg
ms.openlocfilehash: 58856875fa7d7bb3ba63c489fb17790e68f99aec
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52872195"
---
# <a name="using-the-administrator-portal-in-azure-stack"></a>Azure Stack'te Yönetici portalını kullanma

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack'te iki Portal vardır; Yönetici portalını ve kullanıcı portalı (bazen denir *Kiracı* portal.) Azure Stack operatörü, günlük yönetimi ve işlemleri Azure Stack için Yönetici portalını kullanabilirsiniz.

## <a name="access-the-administrator-portal"></a>Yönetici portalına erişim

Bir geliştirme seti ortamı için ilk erişebildiğiniz emin olmanız gerekir [Geliştirme Seti ana bilgisayarına bağlanmak](azure-stack-connect-azure-stack.md) sanal özel ağ (VPN) veya Uzak Masaüstü bağlantısı aracılığıyla.

Yönetici portalına erişmek için portal URL'si ve Azure Stack operatörü kimlik bilgilerini kullanarak oturum açma göz atın. Tümleşik bir sistem için URL değişir portalı bölge adı ve Azure Stack dağıtımınıza dış tam etki alanı adı (FQDN) bağlı.

| Ortam | Yönetici portalı URL'si |   
| -- | -- | 
| Geliştirme Seti| https://adminportal.local.azurestack.external  |
| Tümleşik sistemler | https://adminportal.&lt; *bölge*&gt;.&lt; *FQDN*&gt; | 
| | |

 ![Yönetici portalı](media/azure-stack-manage-portals/admin-portal.png)

Tüm Azure Stack dağıtımlar için varsayılan saat dilimini Eşgüdümlü Evrensel Saat (UTC) olarak ayarlandığına dikkat edin. Bu otomatik olarak UTC'ye varsayılan olarak yükleme sırasında döner ancak Azure Stack, yükleme sırasında bir saat dilimi seçebilirsiniz.

Yönetici portalı'nda gibi şeyler yapabilirsiniz:

* (Sistem durumu, güncelleştirme, kapasite, vb. dahil.) altyapısını yönetme
* Market’i doldurma
* Kullanıcılar için abonelikleri oluşturma
* Plan ve teklif oluşturma

**Hızlı başlangıç öğreticisinde** kutucuk en yaygın görevleri için çevrimiçi belgelere bağlantılar sağlar.

Bir işleç olsa da kaynaklar oluşturabilir yapmanız gerekir, bu sanal makineler, sanal ağlar ve Yönetici portalı'nda depolama hesapları gibi [kullanıcı portalında oturum açın](user/azure-stack-use-portal.md) oluşturun ve kaynakları test etmek için.

>[!NOTE]
>**Sanal makine oluşturma** hızlı başlangıç Öğreticisi kutucuğunda bağlantı Yönetici portalı'nda bir sanal makine oluşturma, sahiptir, ancak bu yalnızca ilk kez dağıtıldıktan sonra Azure Stack doğrulamak için tasarlanmıştır.

## <a name="understand-subscription-behavior"></a>Abonelik davranışı anlayın

Yönetici portalından Web'de kullanılabilir olan yalnızca bir aboneliğiniz yok. Bu abonelik *varsayılan sağlayıcı aboneliği*. Başka bir abonelik ekleyin ve bunları Yönetici portalı'nda kullanabilirsiniz olamaz.

Azure Stack operatörü, Yönetici portalı'ndan (kendiniz de dahil), kullanıcılarınız için abonelikler ekleyebilirsiniz. Kullanıcılar (kendiniz de dahil) erişebilir ve bu aboneliklerden kullanın **kullanıcı** portalı. Ancak, kullanıcı portalı herhangi bir Yönetici portalını yönetimsel veya işletimsel özelliklerine erişim sağlamaz.

Yönetici ve Kullanıcı Portalı, Azure Resource Manager'ın ayrı örnekleri tarafından desteklenir. Bu Kaynak Yöneticisi'ni ayırma nedeniyle, abonelikleri portalları aşmaz. Örneğin, Azure Stack operatörü, kullanıcı portalında oturum açın, erişemediğiniz *varsayılan sağlayıcı aboneliği*. Tüm yönetim işlevleri için erişiminiz yoksa olsa da, kullanılabilir genel teklifler abonelikleri kendiniz oluşturabilirsiniz. Kullanıcı portalında oturum açmadıysanız sürece bir kiracı kullanıcı olarak kabul edilir.

  >[!NOTE]
  >Bir kullanıcı olarak Azure Stack operatörü aynı Kiracı dizinine dahilse Geliştirme Seti ortamında kullanıcılar Yönetici portalına açmasını engellenmez. Ancak, bunlar herhangi bir yönetim işlevlerini erişemez. Ayrıca, Yönetici portalı'ndan, Kullanıcı Portalı'nda kullanılabilir abonelikleri veya erişim sunan ekleyemezler.

## <a name="administrator-portal-tips"></a>Yönetici portalı ipuçları

### <a name="customize-the-dashboard"></a>Panoyu özelleştirin

Pano, bir dizi varsayılan kutucukla içerir. Seçebileceğiniz **panoyu Düzenle** varsayılan pano değiştirmek veya **yeni Pano** özel bir pano eklemek için. Kutucukları bir panoya kolayca ekleyebilirsiniz. Örneğin, seçebileceğiniz **+ kaynak Oluştur**, sağ **sunar + planlar**seçip **panoya Sabitle**.

Bazı durumlarda, bir boş Pano portalında görebilirsiniz. Pano kurtarmak için **Pano düzenleme**ve ardından sağ tıklatın ve seçin **varsayılan durumuna sıfırlansın**.

### <a name="quick-access-to-online-documentation"></a>Hızlı erişim için çevrimiçi belgeleri

Azure Stack operatör belgeleri erişmek için Yardımı kullanın ve yönetici portalının sağ üst köşesindeki simgeyi (soru işareti) destek. İmlecinizi simgenin üzerine taşıyın ve ardından **Yardım + Destek**.

### <a name="quick-access-to-help-and-support"></a>Yardım ve Destek hızlı erişim

Yönetici portalını sağ üst köşesinde bulunan Yardım ve Destek simgesini (soru işareti) seçin ve ardından **yeni destek isteği**, aşağıdaki sonuçları biri gerçekleşir:

- Bu eylem, tümleşik bir sistem kullanıyorsanız, burada doğrudan bir destek bileti Microsoft Müşteri Destek Hizmetleri (CSS) ile açabileceğiniz bir site açar. Başvurmak [destek alınacağı](azure-stack-manage-basics.md#where-to-get-support) zaman orijinal ekipman üreticisi (OEM) donanım satıcısı Desteğinizi veya Microsoft desteği aracılığıyla gideceğine anlamak için.
- Bu eylem, Geliştirme Seti kullanıyorsanız, Azure Stack Forum sitesine doğrudan açar. Bu Forum düzenli olarak izlenir. Geliştirme Seti değerlendirme ortamı olduğundan, Microsoft CSS sunulan resmi desteği yoktur.

### <a name="quick-access-to-the-azure-roadmap"></a>Azure yol haritası hızlı erişim

Seçerseniz **Yardım ve Destek** (soru işareti) Yönetici portalı ve ardından sağ üst köşesindeki **Azure yol haritası**, yeni bir tarayıcı sekmesi açılır ve sizi Azure yol haritası götürür. Yazarak **Azure Stack** içinde **ürünleri** arama kutusu, tüm Azure Stack yol haritası güncelleştirmeleri görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure stack'teki bölge Yönetimi](azure-stack-region-management.md)
