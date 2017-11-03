---
title: "Azure RBAC sorunlarını giderme | Microsoft Docs"
description: "Sorunları veya soruları rol tabanlı erişim denetimi kaynaklar hakkında yardım alın."
services: azure-portal
documentationcenter: na
author: andredm7
manager: femila
ms.assetid: df42cca2-02d6-4f3c-9d56-260e1eb7dc44
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: 407c030ea159915d4d7ac21760a3d17ec2204372
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="role-based-access-control-troubleshooting"></a>Rol tabanlı erişim denetimi sorunlarını giderme

Azure portalı ve can rollerinde erişimi sorunlarını giderme kullanırken beklenmesi gerekenler bilmesi belge makalede rolleri ile verilen özel erişim hakları hakkında sık sorulan soruları yanıtlar. Bu üç rol tüm kaynak türleri kapsar:

* Sahip  
* Katılımcı  
* Okuyucu  

Sahipleri ve katkıda bulunanları yönetim deneyimi tam erişime sahip, ancak bir katkıda bulunan diğer kullanıcılara veya gruplara erişim veremez. Burada süre çok harcama yaptığımız böylece şeyler okuyucu rolüyle biraz daha ilginç alın. Bkz: [rol tabanlı erişim denetimi get-started makale](role-based-access-control-configure.md) erişim hakkında ayrıntılar için.

## <a name="app-service-workloads"></a>Uygulama hizmeti iş yükleri
### <a name="write-access-capabilities"></a>Yazma erişimi özellikleri
Bir tek bir web uygulaması bir kullanıcı salt okunur erişim vermek, beklediğiniz değil, bazı özellikler devre dışı bırakılır. Aşağıdaki yönetim özelliklerini gerektiren **yazma** erişim bir web uygulaması (Katkıda bulunan veya sahibi) ve herhangi bir salt okunur senaryoda kullanılamaz.

* Komutları (örneğin, Başlat, Durdur, vs.)
* Genel yapılandırma, Ölçek ayarları, Yedekleme ayarlarını ve izleme ayarları gibi ayarlarını değiştirme
* Yayımlama kimlik bilgileri ve uygulama ayarlarının ve bağlantı dizeleri gibi diğer parolaları erişme
* Akış günlükleri
* Tanılama günlüklerini yapılandırma
* Konsol (komut istemi)
* Etkin ve en son dağıtımları (için yerel git sürekli dağıtımı)
* Tahmini harcamanız
* Web testleri
* Sanal ağ (yalnızca bir sanal ağ yazma erişimine sahip bir kullanıcı tarafından önceden yapılandırılmışsa, bir okuyucu için görünür).

Bu kutucukların erişemiyorsanız, katkıda bulunan erişim web uygulaması için yöneticinizden gerekir.

### <a name="dealing-with-related-resources"></a>İlgili kaynaklar postalarla
Web uygulamaları tarafından Interplay birkaç farklı kaynaklar varlığını karmaşık. Birkaç Web sitelerini tipik kaynak grubuyla şöyledir:

![Web uygulaması kaynak grubu](./media/role-based-access-control-troubleshooting/website-resource-model.png)

Yalnızca web uygulaması, Azure portalında Web sitesi dikey işlevlerinin çoğunu erişim devre dışı vermeniz bir sonucu olarak.

Bu öğeler gerektiren **yazma** erişim **uygulama hizmeti planı** Web sitenize karşılık gelir:  

* Web uygulaması görüntüleme (ücretsiz veya standart) fiyatlandırma katmanı kullanıcının  
* Ölçek yapılandırma (örnekleri, sanal makine boyutu, otomatik ölçeklendirme ayarlarını sayısı)  
* Kotalar (depolama, bant genişliği, CPU)  

Bu öğeler gerektiren **yazma** tam erişimi **kaynak grubu** Web sitenizi içerir:  

* SSL sertifikaları ve (SSL sertifikalarını, aynı kaynak grubunda siteleri ve coğrafi konum arasında paylaşılabilir) bağlamaları  
* Uyarı kuralları  
* otomatik ölçeklendirme ayarları  
* Uygulama Öngörüler bileşenleri  
* Web testleri  

## <a name="virtual-machine-workloads"></a>Sanal makine iş yükleri
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

## <a name="see-more"></a>Diğerlerini görüntüle
* [Rol tabanlı erişim denetimi](role-based-access-control-configure.md): Azure portalında RBAC ile çalışmaya başlama.
* [Yerleşik roller](role-based-access-built-in-roles.md): Get RBAC standart gelen rolleri hakkında ayrıntılar.
* [Azure rbac'de özel roller](role-based-access-control-custom-roles.md): erişim gereksinimlerinize uyacak şekilde özel roller oluşturma hakkında bilgi edinin.
* [Erişim değişiklik geçmişi raporu oluşturma](role-based-access-control-access-change-history-report.md): RBAC içinde rol atamalarını değiştirme izler.

