---
title: Azure RBAC sorunlarını giderme | Microsoft Docs
description: Azure rol tabanlı erişim denetimi (RBAC) ile ilgili sorunları giderin.
services: azure-portal
documentationcenter: na
author: rolyon
manager: mtillman
ms.assetid: df42cca2-02d6-4f3c-9d56-260e1eb7dc44
ms.service: role-based-access-control
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: seohack1
ms.openlocfilehash: 557d3330ef155181c050a18b14d31b65ba1f2dcf
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36295412"
---
# <a name="troubleshooting-rbac-in-azure"></a>Azure RBAC sorunlarını giderme

Bu makalede, Azure portalı ve can rollerinde erişimi sorunlarını giderme kullanırken beklenmesi gerekenler bilmesi rol tabanlı erişim denetimi (RBAC) hakkında sık sorulan sorular yanıtlanmaktadır. Bu üç rol tüm kaynak türleri kapsar:

* Sahip  
* Katılımcı  
* Okuyucu  

Sahipleri ve katkıda bulunanları yönetim deneyimi tam erişime sahip, ancak katkıda bulunan, diğer kullanıcılara veya gruplara erişim izni veremiyor. Burada süre çok harcama yaptığımız böylece şeyler okuyucu rolüyle biraz daha ilginç alın. Hakkında bilgi için erişim seee [RBAC ve Azure portalını kullanarak erişimini yönetme](role-assignments-portal.md).

## <a name="app-service"></a>App Service
### <a name="write-access-capabilities"></a>Yazma erişimi özellikleri
Bir tek bir web uygulaması bir kullanıcı salt okunur erişim vermek, beklediğiniz değil, bazı özellikler devre dışı bırakılır. Aşağıdaki yönetim özelliklerini gerektiren **yazma** erişim bir web uygulaması (Katkıda bulunan veya sahibi) ve herhangi bir salt okunur senaryoda kullanılamaz.

* Komutları (örneğin, Başlat, Durdur, vs.)
* Genel yapılandırma, Ölçek ayarları, Yedekleme ayarlarını ve izleme ayarları gibi ayarlarını değiştirme
* Yayımlama kimlik bilgileri ve uygulama ayarlarının ve bağlantı dizeleri gibi diğer parolaları erişme
* Akış günlükleri
* Tanılama günlüklerini yapılandırma
* Konsol (komut istemi)
* Etkin ve en son dağıtımları (için yerel git sürekli dağıtımı)
* Tahmini harcama
* Web testleri
* Sanal ağ (yalnızca bir sanal ağ yazma erişimine sahip bir kullanıcı tarafından önceden yapılandırılmışsa, bir okuyucu için görünür).

Bu kutucukların erişemiyorsanız, katkıda bulunan erişim web uygulaması için yöneticinizden gerekir.

### <a name="dealing-with-related-resources"></a>İlgili kaynaklar postalarla
Web uygulamaları tarafından Interplay birkaç farklı kaynaklar varlığını karmaşık. Birkaç Web sitelerini tipik kaynak grubuyla şöyledir:

![Web uygulaması kaynak grubu](./media/troubleshooting/website-resource-model.png)

Yalnızca web uygulaması, Azure portalında Web sitesi dikey işlevlerinin çoğunu erişim devre dışı vermeniz bir sonucu olarak.

Bu öğeler gerektiren **yazma** erişim **uygulama hizmeti planı** Web sitenize karşılık gelir:  

* Web uygulaması görüntüleme (ücretsiz veya standart) fiyatlandırma katmanı kullanıcının  
* Ölçek yapılandırma (örnekleri, sanal makine boyutu, otomatik ölçeklendirme ayarlarını sayısı)  
* Kotalar (depolama, bant genişliği, CPU)  

Bu öğeler gerektiren **yazma** tam erişimi **kaynak grubu** Web sitenizi içerir:  

* SSL sertifikaları ve (SSL sertifikalarını, aynı kaynak grubunda siteleri ve coğrafi konum arasında paylaşılabilir) bağlamaları  
* Uyarı kuralları  
* otomatik ölçeklendirme ayarları  
* Application Insights bileşenleri  
* Web testleri  

## <a name="azure-functions"></a>Azure İşlevleri
Bazı özellikleri [Azure işlevleri](../azure-functions/functions-overview.md) yazma erişimi gerektirir. Okuyucu rolüne atanmış bir kullanıcı, örneğin, bunlar bir işlev uygulaması işlevlerinde görmeye olmaz. Portal görüntüler **(erişimi yok)**.

![Hiçbir erişim uygulamaları işlevi](./media/troubleshooting/functionapps-noaccess.png)

Bağlanabilmesi **Platform özellikleri** sekmesini ve sonra **tüm ayarları** bazı ayarları görüntülemek için bir işlev uygulaması (bir web uygulaması'na benzer) ilgili ancak bu ayarlardan herhangi birini değiştiremez.

## <a name="virtual-machine"></a>Sanal makine
Çok gibi sanal makine veya diğer kaynakları kaynak grubunda yazma erişimi sanal makine dikey penceresinde bazı özellikler web uygulamaları ile gerektirir.

Sanal makineler, etki alanı adları, sanal ağlar, depolama hesapları ve uyarı kuralları ilişkilidir.

Bu öğeler gerektiren **yazma** erişim **sanal makine**:

* Uç Noktalar  
* IP adresleri  
* Diskler  
* Uzantılar  

Bunlar **yazma** her ikisi de erişim **sanal makine**ve **kaynak grubu** (etki alanı adı ile birlikte), BT zamanı:  

* Kullanılabilirlik kümesi  
* Yük dengeli kümesi  
* Uyarı kuralları  

Bu kutucukların erişemiyorsanız, katkıda bulunan erişim kaynak grubu için yöneticinizden isteyin.

## <a name="next-steps"></a>Sonraki adımlar
* [RBAC ve Azure portalını kullanarak erişimini yönetme](role-assignments-portal.md)
* [RBAC değişiklikleri etkinlik günlüklerini görüntüle](change-history-report.md)

