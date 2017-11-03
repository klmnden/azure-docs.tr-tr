---
title: "Azure yığınında Yönetici portalı'nı kullanarak | Microsoft Docs"
description: "Bir Azure yığın operatör olarak, Yönetici portalı'nı kullanmayı öğrenin."
services: azure-stack
documentationcenter: 
author: twooley
manager: byronr
editor: 
ms.assetid: 02c7ff03-874e-4951-b591-28166b7a7a79
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: twooley
ms.openlocfilehash: 3a1be7a08fab8ad0253f26e6a0683617bff4b7c9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="using-the-administrator-portal-in-azure-stack"></a>Azure yığınında Yönetici portalı'nı kullanarak

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure yığınında iki portalı vardır; Yönetici portalı'nı ve Kullanıcı Portalı'nı (bazen denir *Kiracı* portalı). Bir Azure yığın operatör olarak, günlük yönetimi ve Azure yığınının işlemleri için Yönetici portalı'nı kullanabilirsiniz. 

## <a name="access-the-administrator-portal"></a>Erişim Yönetici portalı

Bir geliştirme seti ortamı için önce şunları yapabilirsiniz emin olmanız gerekir. [Geliştirme Seti ana bilgisayarına bağlanmak](azure-stack-connect-azure-stack.md) sanal özel ağ (VPN) veya Uzak Masaüstü bağlantısı aracılığıyla.

Yönetici portalına erişmek için göz atın portalı URL'si ve bir Azure yığın işleç kimlik bilgilerini kullanarak oturum açın. Tümleşik bir sistem için URL değişir portal bölge adı ve Azure yığın dağıtımınızı dış tam etki alanı adı (FQDN) bağlı.

| Ortam | Yönetici portalı URL'si |   
| -- | -- | 
| Geliştirme Seti| https://adminportal.Local.azurestack.external  |
| Tümleşik sistemler | https://adminportal. &lt; *bölge*&gt;.&lt; *FQDN*&gt; | 
| | |

 ![Yönetici portalı](media/azure-stack-manage-portals/image1.png)

Yönetici portalı'nda, gibi şeyler yapabilir:

* (sistem durumu, güncelleştirmeler, kapasite, vb. dahil.) altyapısını yönetme
* Market doldurma
* planları ve teklifleri oluştur
* kullanıcılar için abonelikleri oluşturma

İçinde **hızlı başlangıç Öğreticisi** döşeme, en sık kullanılan görevler için çevrimiçi belgelere bağlantıları vardır.
 
Sanal makineler, sanal ağlar ve depolama hesapları gibi kaynaklara Yönetici portalı'nda oluşturmak bir işleç yeteneği olsa da, aşağıdakileri yapmalısınız [kullanıcı portalı oturum açma](user/azure-stack-use-portal.md) oluşturma ve test kaynakları. ( **Bir sanal makine oluşturmak** hızlı başlangıç Öğreticisi döşemesinin bağlantısı olan bir sanal makine Yönetici portalı'nda oluşturmanıza, ancak bu yordam yalnızca ilk dağıtımdan sonra Azure yığın doğrulamak için kullanılır.)

## <a name="subscription-behavior"></a>Abonelik davranışı
 
Yönetici portalı'nda kullanılabilir olan yalnızca bir abonelik yok. Bu abonelik *varsayılan sağlayıcı abonelik*. Başka bir aboneliği kullanmak için Yönetici portalı'nda ekleyemezsiniz.

Bir Azure yığın işleci Yönetici portalı'ndan (kendiniz dahil), kullanıcılarınız için abonelikler ekleyebilir. Kullanıcılar (kendiniz dahil) erişebilir ve Kullanıcı Portalı'ndan bu abonelikleri kullanın. Kullanıcı Portalı herhangi bir Yönetici portalı'nı yönetimsel veya işletimsel özelliklerini erişim sağlamaz.

Yönetici ve kullanıcı portalı ayrı örnekleri Azure Kaynak Yöneticisi'nin tarafından desteklenir. Resource Manager ayrımı nedeniyle abonelikleri portalları geçmez. Örneğin, bir Azure yığın işleç gibi kullanıcı portalında oturum açtığında, varsayılan sağlayıcı abonelik erişemiyor. Bu nedenle, tüm yönetim işlevlerini erişimi yok. Genel önerileri kendiniz için abonelikleri oluşturabilirsiniz, ancak bir kiracı kullanıcı olarak kabul edilir.

  >[!NOTE]
  Bir kullanıcı Azure yığın işleci aynı Kiracı dizine aitse Geliştirme Seti ortamında, bunlar Yönetici portalı'na açmasını engellenmez. Ancak, bunlar yönetim işlevlerini hiçbirine erişemiyor. Ayrıca, Yönetici portalı'ndan Kullanıcı Portalı'nda kullanılabilen yapılma aboneliklerini ya da erişim sunar ekleyemezler.

## <a name="administrator-portal-tips"></a>Yönetici portalı ipuçları

### <a name="customize-the-dashboard"></a>Pano özelleştirme

Pano, bir dizi varsayılan kutucukla içerir. Tıklayabilirsiniz **düzenleme Pano** tıklayın veya varsayılan pano değiştirme **yeni Pano** özel panolar eklemek için. Pano için kutucuk kolayca ekleyebilirsiniz. Örneğin, tıklayabilirsiniz **yeni**, sağ **sunar + planları**ve ardından **panoya Sabitle**.

### <a name="quick-access-to-online-documentation"></a>Hızlı erişim için çevrimiçi belgeleri

Azure yığın işleci belgelerine erişmek için Yönetici portalı'nı sağ üst köşesinde Yardım ve Destek simgesine (soru işareti) tıklayın ve ardından **Yardım + Destek**.

### <a name="quick-access-to-help-and-support"></a>Yardım ve destek için hızlı erişim

Yönetici portalı'nı sağ üst köşesinde Yardım ve Destek (soru işareti) simgesine tıklayın ve ardından, **yeni destek isteği**, bunu aşağıdakilerden birini yapar:

- Tümleşik bir sistem kullanıyorsanız, bu eylem sizi doğrudan bir destek bileti Microsoft Müşteri Destek Hizmetleri'ne (CSS) ile açabileceğiniz bir site açar. "Destek almak Where" bölümüne bakın [Azure yığın Yönetimi Temelleri](azure-stack-manage-basics.md) Microsoft destek ya da özgün donanım üreticisi (OEM) donanım satıcısı destek zaman gitmesi gereken anlamak için.
- Geliştirme Seti kullanıyorsanız, bu eylem Azure yığın forumları sitenin doğrudan açılır. Bu forumları düzenli olarak izlenir. Geliştirme Seti bir değerlendirme ortamı olduğundan, Microsoft CSS sunulan resmi desteği yoktur.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure yığınında bölge Yönetimi](azure-stack-region-management.md)
