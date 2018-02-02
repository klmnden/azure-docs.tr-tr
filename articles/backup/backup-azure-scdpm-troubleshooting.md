---
title: "System Center Data Protection Manager Azure Backup ile ilgili sorunları giderme | Microsoft Docs"
description: "System Center Data Protection Manager sorunları giderin."
services: backup
documentationcenter: 
author: adigan
manager: shreeshd
editor: 
ms.assetid: 2d73c349-0fc8-4ca8-afd8-8c9029cb8524
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/24/2017
ms.author: pullabhk;markgal;adigan
ms.openlocfilehash: 2244a39217f54eb5906d05990f19fc8ca2fdeb0e
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="troubleshoot-system-center-data-protection-manager"></a>System Center Data Protection Manager sorunlarını giderme

SC DPM için en son sürüm notlarında bulabilirsiniz [burada](https://docs.microsoft.com/en-us/system-center/dpm/dpm-release-notes?view=sc-dpm-2016).
Koruma desteği matrisi bulunabilir [burada](https://docs.microsoft.com/en-us/system-center/dpm/dpm-protection-matrix?view=sc-dpm-2016).

## <a name="replica-is-inconsistent"></a>Çoğaltma tutarsız

Bu hata çeşitli nedenlerden ötürü - çoğaltma oluşturma işi başarısız oldu, değişiklik günlüğü ile ilgili sorunları oluşabilir birim düzeyi filtresi bit eşlem hataları, kaynak makine kapatma beklenmedik bir şekilde, eşitleme günlüğüne ya da çoğaltma taşması gerçekten tutarsız. Bu sorunu çözmek için şu adımları izleyin:
- Tutarsız durum kaldırmak için el ile tutarlılık denetimi çalıştırmak veya günlük tutarlılık denetimi zamanlamanız.
- System Center DPM veya MAB sunucunun en son sürüm üzerinde olduğundan emin olun
- Otomatik tutarlılık denetiminin etkin olduğundan emin olun
- Komut istemi ("ardından"net start dpmra"net stop dpmra") hizmetleri yeniden başlatmayı deneyin
- Ağ bağlantısı ve bant genişliği gereksinimlerinin karşılandığından emin olun
- Kaynak makine beklenmedik şekilde kapatıldı olmadığını denetleyin
- Diskin sağlıklı olduğundan ve çoğaltma için yeterli alan olmadığından emin olun
- Yinelenen hiçbir yedekleme işi aynı anda çalıştırdığınızdan emin olun

## <a name="online-recovery-point-creation-failed"></a>Çevrimiçi kurtarma noktası oluşturma başarısız oldu

Bu sorunu çözmek için şu adımları izleyin:
- Azure Yedekleme aracısı en son sürümü olduğundan emin olun
- Koruma görev bölmesinde kurtarma noktası el ile oluşturmayı deneyin
- Veri kaynağı üzerinde tutarlılık denetimi çalıştırdığınızdan emin olun
- Ağ bağlantısı ve bant genişliği gereksinimlerinin karşılandığından emin olun
- Çoğaltma verileri tutarsız durumda. Bu veri kaynağının bir disk kurtarma noktası oluştur
- Mevcut ve eksik olmayan çoğaltma olduğundan emin olun
- Çoğaltma, USN günlüğü oluşturmak için yeterli alana sahip

## <a name="unable-to-configure-protection"></a>Koruma yapılandırılamıyor

DPM sunucusu korumalı sunucu ile bağlantı kuramadı olduğunda bu hata görünür. Bu sorunu çözmek için şu adımları izleyin:
- Azure yedekleme Aracısı'nın en son sürümde olduğundan emin olun
- DPM sunucusu ve korunan sunucu arasında (güvenlik duvarı/ağ/proxy) bağlantısı olduğundan emin olun
- SQL Server koruyorsanız, NT AUTHORITY\SYSTEM sysadmin oturum açma özelliklerinden etkin olduğundan emin olun

## <a name="this-server-is-not-registered-to-the-vault-specified-by-the-vault-credential"></a>Bu sunucu kasa kimlik bilgileri tarafından belirtilen kasaya kayıtlı değil

Seçilen kasa kimlik bilgilerini System Center DPM ile ilişkili kurtarma Hizmetleri Kasası'na ait olmadığından, bu hata görünür / Azure yedekleme sunucusu kurtarma denemesi. Bu sorunu çözmek için şu adımları izleyin:
- Yükleme için kasa kimlik bilgilerini kurtarma Hizmetleri kasası için System Center DPM / Azure yedekleme sunucusu kaydedildi.
- Sunucusu için en son indirilen kasa kimlik bilgilerini kullanarak kasası ile kaydediliyor deneyin.

## <a name="either-the-recoverable-data-is-not-available-or-the-selected-server-is-not-a-dpm-server"></a>Kurtarılabilir veriler kullanılamıyor veya seçili sunucu DPM sunucusu değil
Herhangi bir System Center DPM vardır / kurtarma Hizmetleri Kasası'na kayıtlı Azure yedekleme sunucusu veya sunucuları henüz meta veriler karşıya yüklenmedi veya seçili sunucu System Center DPM değil, bu hata görünür / Azure yedekleme sunucusu.
- Diğer System Center DPM / kurtarma Hizmetleri Kasası'na kayıtlı Azure yedekleme sunucusu varsa, en son Azure Backup aracısının yüklü olduğundan emin olun.
- Diğer System Center DPM / kurtarma Hizmetleri Kasası'na kayıtlı Azure yedekleme sunucusu varsa, kurtarma işlemini başlatmak için yüklemeden sonra bir gün bekleyin. Gecelik iş buluta korunan tüm yedeklemeler için meta veriler karşıya yükleme. Veri kurtarma için kullanılamıyor.

## <a name="the-encryption-passphrase-provided-does-not-match-with-passphrase-associated-with-the-following-server"></a>Sağlanan şifreleme parolası şu sunucuyla ilişkili parolayla eşleşmiyor.

> [!NOTE]
>Unuttunuz/şifreleme parolası kaybederseniz, verileri kurtarmak için bir seçenek yoktur. Tek seçeneğiniz gelecekteki yedekleme verilerini şifrelemek için parolayı yeniden ve kullanmaktır.
>
>

Bu hata, şifreleme parolası System Center DPM veriler şifreleme işleminde kullanılan / kurtarılmakta olan Azure Backup sunucunun verilerini sağlanan şifreleme parolası eşleşmiyor olduğunda görüntülenir. Aracısı, verileri şifrelemek alamıyor. Bu nedenle kurtarma başarısız olur. Bu sorunu çözmek için şu adımları izleyin:
- System Center DPM ile ilişkili tam aynı şifreleme parolası sağlamak / verisini kurtarıldığı Azure yedekleme sunucusu. 
