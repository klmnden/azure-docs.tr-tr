---
title: Azure yığınında Yönetici portalı'nı kullanarak | Microsoft Docs
description: Bir Azure yığın operatör olarak, Yönetici portalı'nı kullanmayı öğrenin.
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
ms.date: 06/05/2018
ms.author: mabrigg
ms.openlocfilehash: 673b1144fe927e0619f5f8638d7e8ce9a181f48c
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35248529"
---
# <a name="using-the-administrator-portal-in-azure-stack"></a>Azure yığınında Yönetici portalı'nı kullanarak

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure yığınında iki portalı vardır; Yönetici portalı'nı ve Kullanıcı Portalı'nı (bazen denir *Kiracı* portal.) Bir Azure yığın operatör olarak, günlük yönetimi ve Azure yığınının işlemleri için Yönetici portalı'nı kullanabilirsiniz.

## <a name="access-the-administrator-portal"></a>Erişim Yönetici portalı

Bir geliştirme seti ortamı için önce şunları yapabilirsiniz emin olmanız gerekir. [Geliştirme Seti ana bilgisayarına bağlanmak](azure-stack-connect-azure-stack.md) sanal özel ağ (VPN) veya Uzak Masaüstü bağlantısı aracılığıyla.

Yönetici portalına erişmek için göz atın portalı URL'si ve bir Azure yığın işleç kimlik bilgilerini kullanarak oturum açın. Tümleşik bir sistem için URL değişir portal bölge adı ve Azure yığın dağıtımınızı dış tam etki alanı adı (FQDN) bağlı.

| Ortam | Yönetici portalı URL'si |   
| -- | -- | 
| Geliştirme Seti| https://adminportal.local.azurestack.external  |
| Tümleşik sistemler | https://adminportal.&lt; *bölge*&gt;.&lt; *FQDN*&gt; | 
| | |

 ![Yönetici portalı](media/azure-stack-manage-portals/image1.png)

Yönetici portalı'nda, gibi şeyler yapabilir:

* (sistem durumu, güncelleştirmeler, kapasite, vb. dahil.) altyapısını yönetme
* Market’i doldurma
* kullanıcılar için abonelikleri oluşturma
* planları ve teklifleri oluştur

**Hızlı başlangıç Öğreticisi** kutucuğu en yaygın görevler için çevrimiçi belgeleri bağlantılarını sağlar.

Bir işleç olsa da kaynaklar oluşturabilirsiniz sanal makineler, sanal ağlar ve depolama hesaplarını Yönetici portalı'nda gibi gerekir [kullanıcı portalı oturum açma](user/azure-stack-use-portal.md) oluşturmak ve test kaynakları için.

>[!NOTE]
>**Bir sanal makine oluşturmak** hızlı başlangıç Öğreticisi döşemesinin bağlantısı olan bir sanal makine Yönetici portalı'nda oluşturmanıza, ancak bu yalnızca ilk dağıtıldıktan sonra Azure yığın doğrulamak için tasarlanmıştır.

## <a name="understand-subscription-behavior"></a>Abonelik davranışlarını anlamak

Yalnızca bir abonelik Yönetici portalı'ndan kullanılabilir yoktur. Bu abonelik *varsayılan sağlayıcı abonelik*. Başka bir abonelik ekleme ve Yönetici portalı'nda kullanın.

Bir Azure yığın işleci Yönetici portalı'ndan (kendiniz dahil), kullanıcılarınız için abonelikler ekleyebilir. Kullanıcılar (kendiniz dahil) erişebilir ve bu aboneliklerden kullanmak **kullanıcı** portal. Ancak, kullanıcı portalı herhangi bir Yönetici portalı'nı yönetimsel veya işletimsel özelliklerini erişim sağlamaz.

Yönetici ve kullanıcı portalı ayrı örnekleri Azure Kaynak Yöneticisi'nin tarafından desteklenir. Bu Kaynak Yöneticisi'ni ayrımı nedeniyle abonelikleri portalları geçmez. Bir Azure yığın operatör olarak, kullanıcı portalında oturum açarsanız, örneğin, erişemiyor *varsayılan sağlayıcı abonelik*. Tüm yönetim işlevlerini erişiminiz yok olsa da, kullanılabilir genel önerileri kendiniz için abonelikleri oluşturabilirsiniz. Kullanıcı portalında oturum açtınız sürece bir kiracı kullanıcı olarak kabul edilir.

  >[!NOTE]
  >Bir kullanıcı Azure yığın işleci aynı Kiracı dizine aitse Geliştirme Seti ortamında, bunlar Yönetici portalı'na açmasını engellenmez. Ancak, bunlar yönetim işlevlerini hiçbirine erişemiyor. Ayrıca, Yönetici portalı'ndan Kullanıcı Portalı'nda kullanılabilen aboneliklerini ya da erişim sunar ekleyemezler.

## <a name="administrator-portal-tips"></a>Yönetici portalı ipuçları

### <a name="customize-the-dashboard"></a>Pano özelleştirme

Pano, bir dizi varsayılan kutucukla içerir. Seçebileceğiniz **düzenleme Pano** varsayılan pano değiştirmek veya seçmek için **yeni Pano** özel bir pano eklemek için. Bir Pano kutucukları kolayca ekleyebilirsiniz. Örneğin, seçebileceğiniz **yeni**, sağ **sunar + planları**ve ardından **panoya Sabitle**.

### <a name="quick-access-to-online-documentation"></a>Hızlı erişim için çevrimiçi belgeleri

Azure yığın işleci belgelerine erişmek için Yardım'ı kullanma ve Yönetici portalı'nı sağ üst köşesinde simgesi (soru işareti) destekler. İmleci simgesine getirin ve ardından **Yardım + Destek**.

### <a name="quick-access-to-help-and-support"></a>Yardım ve destek için hızlı erişim

Yönetici portalı'nı sağ üst köşesinde Yardım ve Destek simgesini (soru işareti) seçin ve ardından **yeni destek isteği**, aşağıdaki sonuçları biri gerçekleşir:

- Tümleşik bir sistem kullanıyorsanız, bu eylem sizi doğrudan bir destek bileti Microsoft Müşteri Destek Hizmetleri'ne (CSS) ile açabileceğiniz bir site açar. Başvurmak [destek almak nereye](azure-stack-manage-basics.md#where-to-get-support) Microsoft destek ya da özgün donanım üreticisi (OEM) donanım satıcısı destek zaman gitmesi gereken anlamak için.
- Geliştirme Seti kullanıyorsanız, bu eylem Azure yığın forumları sitenin doğrudan açılır. Bu forumları düzenli olarak izlenir. Geliştirme Seti bir değerlendirme ortamı olduğundan, Microsoft CSS sunulan resmi desteği yoktur.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure yığınında bölge Yönetimi](azure-stack-region-management.md)
