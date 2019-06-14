---
title: System Center Data Protection Manager'Azure yedekleme ile ilgili sorunları giderme
description: System Center Data Protection Manager sorunlarını giderme.
services: backup
author: kasinh
manager: vvithal
ms.service: backup
ms.topic: conceptual
ms.date: 01/30/2019
ms.author: kasinh
ms.openlocfilehash: 4108616e3ae41e2c88b74bb08d5f846c0035101f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60236204"
---
# <a name="troubleshoot-system-center-data-protection-manager"></a>System Center Data Protection Manager sorunlarını giderme

Bu makalede Data Protection Manager'ı kullanırken karşılaşabileceğiniz sorunların çözümleri açıklanmaktadır.

En son sürüm notları için System Center Data Protection Manager [System Center belgeleri](https://docs.microsoft.com/system-center/dpm/dpm-release-notes?view=sc-dpm-2016). ' De Data Protection Manager için destek hakkında daha fazla bilgi [bu Matris](https://docs.microsoft.com/system-center/dpm/dpm-protection-matrix?view=sc-dpm-2016).


## <a name="error-replica-is-inconsistent"></a>Hata: Çoğaltma tutarsız

Aşağıdaki nedenlerden dolayı bir çoğaltma tutarsız olabilir:
- Çoğaltma oluşturma işi başarısız olur.
- Değişiklik günlüğü sorunlar vardır.
- Ses düzeyi filtresi bit eşlem hatalar içeriyor.
- Kaynak makine beklenmedik bir şekilde kapatılır.
- Eşitleme günlüğüne taşıyor.
- Çoğaltmayı gerçekten tutarlı değil.

Bu sorunu çözmek için aşağıdaki eylemleri gerçekleştirin:
- Tutarsız durum kaldırmak için el ile tutarlılık denetimi çalıştırın veya günlük tutarlılık denetimi zamanlayabilir.
- Microsoft Azure Backup sunucusu ve Data Protection Manager'ın en son sürümünü kullandığınızdan emin olun.
- Emin **otomatik tutarlılık** seçeneği etkinleştirilmiştir.
- Komut isteminden hizmetleri yeniden başlatmayı deneyin. Kullanım `net stop dpmra` komutunu `net start dpmra`.
- Ağ bağlantısı ve bant genişliği gereksinimlerini karşıladığı emin olun.
- Kaynak makine beklenmedik biçimde kapatıldı olmadığını kontrol edin.
- Disk sağlıklı olduğunu ve çoğaltma için yeterli alanı olduğundan emin olun.
- Eşzamanlı olarak çalışan hiçbir yinelenen yedekleme işleri olduğundan emin olun.

## <a name="error-online-recovery-point-creation-failed"></a>Hata: Çevrimiçi kurtarma noktası oluşturma başarısız oldu

Bu sorunu çözmek için aşağıdaki eylemleri gerçekleştirin:
- Azure yedekleme aracısının en son sürümünü kullandığınızdan emin olun.
- El ile koruma görev alanında kurtarma noktası oluşturmayı deneyin.
- Veri kaynağı üzerinde tutarlılık denetimi çalıştırdığınızdan emin olun.
- Ağ bağlantısı ve bant genişliği gereksinimlerini karşıladığı emin olun.
- Çoğaltma verileri tutarsız bir durumda olduğunda, bu veri kaynağı bir disk kurtarma noktası oluşturun.
- Çoğaltmayı var ve aktarmasının eksik olduğundan emin olun.
- Çoğaltma güncelleştirme sıra numarası (USN) günlük oluşturmak için yeterli alanı olduğundan emin olun.

## <a name="error-unable-to-configure-protection"></a>Hata: Koruma yapılandırılamıyor

Data Protection Manager sunucusunun korunan sunucu iletişim kuramıyorsa bu hata oluşur. 

Bu sorunu çözmek için aşağıdaki eylemleri gerçekleştirin:
- Azure yedekleme aracısının en son sürümünü kullandığınızdan emin olun.
- Data Protection Manager sunucunuza ve korunan sunucu arasında bağlantı (ağ/güvenlik duvarı/proxy) olduğundan emin olun.
- Bir SQL server koruyorsanız emin **oturum açma özellikleri** > **NT AUTHORITY\SYSTEM** özelliği gösterir **sysadmin** ayarı etkin.

## <a name="error-server-not-registered-as-specified-in-vault-credential-file"></a>Hata: Sunucu belirtildiği gibi kasa kimlik bilgilerini kayıtlı değil

Data Protection Manager/Azure Backup sunucusu verileri için kurtarma işlemi sırasında bu hata oluşur. Kurtarma işleminde kullanılan kasa kimlik bilgilerini, Data Protection Manager/Azure Backup sunucusu için kurtarma Hizmetleri kasasına ait değil.

Bu sorunu çözmek için aşağıdaki adımları gerçekleştirin:
1. Kasa kimlik bilgilerini, Data Protection Manager/Azure Backup sunucusu kayıtlı kurtarma Hizmetleri kasasından indirin.
2. En son indirilen kasa kimlik bilgilerini kullanarak sunucuyu kasaya kaydetmek bu seçeneği deneyin.

## <a name="error-no-recoverable-data-or-selected-server-not-a-data-protection-manager-server"></a>Hata: Kurtarılabilir veriler veya seçili sunucu Data Protection Manager sunucu yok

Aşağıdaki nedenlerle bu hata oluşur:
- Hiçbir veri koruma Yöneticisi'ni / Azure Backup sunucusu kurtarma Hizmetleri kasasına kayıtlı.
- Sunucuları, meta veriler henüz yüklemediyseniz.
- Seçili sunucu bir veri koruma Yöneticisi/Azure Backup sunucusu değil.

Data Protection Manager/Azure Backup sunuculara kurtarma Hizmetleri kasasına kayıtlı olduğunda, sorunu çözmek için aşağıdaki adımları gerçekleştirin:
1. En son Azure Backup aracısının yüklü olduğundan emin olun.
2. En son aracıyı yüklendiğinden emin olduktan sonra kurtarma işlemini başlatmadan önce bir gün bekleyin. Gecelik yedekleme işi için tüm korumalı yedekleme meta verileri buluta yükler. Yedekleme verileri, ardından Kurtarma için kullanılabilir.

## <a name="error-provided-encryption-passphrase-doesnt-match-passphrase-for-server"></a>Hata: Sağlanan şifreleme parolası sunucusu için parola eşleşmiyor

Bu hata, Data Protection Manager/Azure Backup server verilerini kurtarırken şifreleme işlemi sırasında oluşur. Sunucunun şifreleme parolası kurtarma işleminde kullanılan şifreleme parolası eşleşmiyor. Sonuç olarak, aracı verilerin şifresini çözemez ve kurtarma başarısız olur.

> [!IMPORTANT]
> Şifreleme parolası kaybeder veya unutursanız, verileri kurtarmak için diğer yöntemler vardır. Tek seçenek, parolayı yeniden sağlamaktır. Gelecekteki yedekleme verilerini şifrelemek için kullanılacak yeni parolayı kullanın.
>
> Her zaman veri kurtarma gerçekleştiriyorsanız Data Protection Manager/Azure Backup sunucusu ile ilişkili şifreleme parolayı belirtin. 
>
