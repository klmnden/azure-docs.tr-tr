---
title: Rol tabanlı erişim denetimini Azure RBAC sorunlarını giderme | Microsoft Docs
description: Sorunları veya soruları rol tabanlı erişim denetimi kaynaklar hakkında yardım alın.
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
ms.reviewer: rqureshi
ms.custom: seohack1
ms.openlocfilehash: e1f9fa8e3abd3eee9d85c241000a07794af9d36b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="troubleshooting-azure-role-based-access-control"></a>Azure rol tabanlı erişim denetimi sorunlarını giderme 

Bu makalede kullanırken Azure portal ve can rollerinde erişimi sorunlarını giderme beklenmesi gerekenler bilmesi rolleri ile verilen özel erişim hakları hakkında sık sorulan sorular yanıtlanmaktadır. Bu üç rol tüm kaynak türleri kapsar:

* Sahibi  
* Katılımcı  
* Okuyucu  

Sahipleri ve katkıda bulunanları yönetim deneyimi tam erişime sahip, ancak bir katkıda bulunan diğer kullanıcılara veya gruplara erişim veremez. Burada süre çok harcama yaptığımız böylece şeyler okuyucu rolüyle biraz daha ilginç alın. Bkz: [rol tabanlı erişim denetimi get-started makale](role-assignments-portal.md) erişim hakkında ayrıntılar için.

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

* Uç nokta  
* IP adresleri  
* Diskler  
* Genişletmeler  

Bunlar **yazma** her ikisi de erişim **sanal makine**ve **kaynak grubu** (etki alanı adı ile birlikte), BT zamanı:  

* Kullanılabilirlik kümesi  
* Yük dengeli kümesi  
* Uyarı kuralları  

Bu kutucukların erişemiyorsanız, katkıda bulunan erişim kaynak grubu için yöneticinizden isteyin.

## <a name="see-more"></a>Daha fazla göster
* [Rol tabanlı erişim denetimi](role-assignments-portal.md): Azure portalında RBAC ile çalışmaya başlama.
* [Yerleşik roller](built-in-roles.md): Get RBAC standart gelen rolleri hakkında ayrıntılar.
* [Azure rbac'de özel roller](custom-roles.md): erişim gereksinimlerinize uyacak şekilde özel roller oluşturma hakkında bilgi edinin.
* [Erişim değişiklik geçmişi raporu oluşturma](change-history-report.md): RBAC içinde rol atamalarını değiştirme izler.

