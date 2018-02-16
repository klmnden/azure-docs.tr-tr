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
ms.openlocfilehash: bf4ea676c5309bb732f6a4ce71849606b4d2e4df
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2018
---
# <a name="troubleshoot-system-center-data-protection-manager"></a>System Center Data Protection Manager sorunlarını giderme

Bu makalede, Data Protection Manager'ı kullanırken karşılaşabileceğiniz sorunların çözümleri açıklar.

En son sürüm notları için System Center Data Protection Manager için bkz: [System Center belgelerine](https://docs.microsoft.com/en-us/system-center/dpm/dpm-release-notes?view=sc-dpm-2016). Desteği için de Data Protection Manager hakkında daha fazla bilgiyi [bu Matris](https://docs.microsoft.com/en-us/system-center/dpm/dpm-protection-matrix?view=sc-dpm-2016).


## <a name="error-replica-is-inconsistent"></a>Hata: Çoğaltma tutarsız

Bir çoğaltma şu nedenlerden dolayı tutarsız olabilir:
- Çoğaltma oluşturma işi başarısız olur.
- Değişiklik günlüğü sorunları vardır.
- Birim düzeyi filtresi bit eşlem hatalar içeriyor.
- Kaynak makine beklenmedik biçimde kapanır.
- Eşitleme günlüğüne taşar.
- Çoğaltma gerçekten tutarsız.

Bu sorunu çözmek için aşağıdaki eylemleri gerçekleştirin:
- Tutarsız durum kaldırmak için tutarlılık denetimi el ile çalıştırabilir veya günlük tutarlılık denetimi zamanlayabilirsiniz.
- Microsoft Azure yedekleme sunucusu ve Data Protection Manager'ın en son sürümünü kullandığınızdan emin olun.
- Emin **otomatik tutarlılık** ayarı etkindir.
- Komut isteminden hizmetlerini yeniden başlatmayı deneyin. Kullanım `net stop dpmra` ve ardından komut `net start dpmra`.
- Ağ bağlantısı ve bant genişliği gereksinimlerini toplantı emin olun.
- Kaynak makine beklenmedik şekilde kapatıldı olmadığını denetleyin.
- Diskin sağlıklı olduğunu ve çoğaltma için yeterli alan olduğundan emin olun.
- Eşzamanlı olarak çalışan hiçbir yinelenen yedekleme işleri olduğundan emin olun.

## <a name="error-online-recovery-point-creation-failed"></a>Hata: Çevrimiçi kurtarma noktası oluşturma başarısız oldu

Bu sorunu çözmek için aşağıdaki eylemleri gerçekleştirin:
- Azure Yedekleme aracısı en son sürümünü kullandığınızdan emin olun.
- Koruma görev bölmesinde kurtarma noktası el ile oluşturmayı deneyin.
- Veri kaynağı üzerinde tutarlılık denetimi çalıştırdığınızdan emin olun.
- Ağ bağlantısı ve bant genişliği gereksinimlerini toplantı emin olun.
- Çoğaltma verileri tutarsız bir durumda olduğunda, bu veri kaynağı bir disk kurtarma noktası oluşturun.
- Çoğaltmayı var ve değil eksik olduğundan emin olun.
- Çoğaltma güncelleştirme sıra numarası (USN) günlük oluşturmak için yeterli alana sahip olduğundan emin olun.

## <a name="error-unable-to-configure-protection"></a>Hata: Koruma yapılandırılamıyor

Bu hata, Data Protection Manager sunucusunun korumalı sunucu ile iletişim kuramıyor oluşur. 

Bu sorunu çözmek için aşağıdaki eylemleri gerçekleştirin:
- Azure Yedekleme aracısı en son sürümünü kullandığınızdan emin olun.
- (Güvenlik duvarı/ağ/proxy) Data Protection Manager sunucunuz ve korunan sunucu arasında bağlantı olduğundan emin olun.
- Bir SQL server koruyorsanız emin **oturum açma özellikleri** > **NT AUTHORITY\SYSTEM** özelliği gösterir **sysadmin** ayarı etkin.

## <a name="error-server-not-registered-as-specified-in-vault-credential-file"></a>Hata: Sunucu belirtildiği şekilde kasa kimlik bilgileri dosyasında kayıtlı değil

Bu hata, veri koruma Yöneticisi/Azure yedekleme sunucusu verileri için kurtarma işlemi sırasında oluşur. Kurtarma işleminde kullanılan kasa kimlik bilgilerini, veri koruma Yöneticisi/Azure yedekleme sunucusu için kurtarma Hizmetleri kasasına ait değil.

Bu sorunu çözmek için aşağıdaki adımları gerçekleştirin:
1. Kasa kimlik bilgilerini, veri koruma Yöneticisi/Azure yedekleme sunucusu kayıtlı olduğu kurtarma Hizmetleri Kasası'nı indirin.
2. En son indirilen kasa kimlik bilgilerini kullanarak sunucuyu kasaya kaydetmek deneyin.

## <a name="error-no-recoverable-data-or-selected-server-not-a-data-protection-manager-server"></a>Hata: Hiçbir kurtarılabilir veriler veya seçili sunucu Data Protection Manager sunucu

Bu hata aşağıdaki nedenlerle oluşur:
- Kurtarma Hizmetleri Kasası'na kayıtlı başka bir veri koruma Yöneticisi/Azure yedekleme sunucusu yok.
- Sunucular, meta veriler henüz yüklemediniz.
- Seçili sunucu veri koruma Yöneticisi/Azure yedekleme sunucusu değil.

Kurtarma Hizmetleri Kasası'na kayıtlı olan diğer veri koruma Yöneticisi/Azure yedekleme sunucusu olduğunda, sorunu çözmek için aşağıdaki adımları gerçekleştirin:
1. En son Azure Backup aracısının yüklü olduğundan emin olun.
2. En son aracısının yüklü olduğundan emin olduktan sonra bir kurtarma işlemine başlamadan önce gün bekleyin. Gecelik yedekleme işi buluta tüm korumalı yedeklemeler için meta veriler karşıya yükleme. Yedekleme verilerini sonra kurtarma için kullanılamıyor.

## <a name="error-provided-encryption-passphrase-doesnt-match-passphrase-for-server"></a>Hata: Sunucunun parolası sağlanan şifreleme parolası eşleşmiyor

Bu hata, veri koruma Yöneticisi/Azure yedekleme server verilerini kurtarırken şifreleme işlemi sırasında oluşur. Kurtarma işlemi, kullanılan şifreleme parolası sunucunun şifreleme parolası eşleşmiyor. Sonuç olarak, aracı verilerin şifresini çözemez ve kurtarma başarısız olur.

> [!IMPORTANT]
> Şifreleme parolası kaybeder veya unutursanız, veri kurtarma için diğer yöntemler vardır. Parolayı yeniden oluşturmak için tek seçenektir bakın. Gelecekteki yedekleme verilerini şifrelemek için kullanılacak yeni parolayı kullanın.
>
> Her zaman veri kurtarma gerçekleştiriyorsanız veri koruma Yöneticisi/Azure yedekleme sunucusu ile ilişkili aynı şifreleme parolası belirtin. 
>
