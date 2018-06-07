---
title: 'Azure AD Connect: 10 GB sınırını sorunu LocalDB kurtarmak nasıl | Microsoft Docs'
description: Bu konu, Azure AD Connect eşitleme hizmeti LocalDB 10 GB karşılaştığında kurtarmak açıklar sınırlamak sorun.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 41d081af-ed89-4e17-be34-14f7e80ae358
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 3ba491902444c9c05f997f854353206d78f2957d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34591281"
---
# <a name="azure-ad-connect-how-to-recover-from-localdb-10-gb-limit"></a>Azure AD Connect: LocalDB 10 GB sınırının kurtarmak nasıl
Azure AD Connect’e kimlik verilerini depolamak için bir SQL Server veritabanı gerekiyor. Azure AD Connect ile yüklenen varsayılan SQL Server 2012 Express LocalDB’yi kullanabileceğiniz gibi, kendi tam SQL’nizi de kullanabilirsiniz. SQL Server Express 10 GB boyut sınırını uygular. LocalDB’yi kullanırken bu sınıra ulaşıldığında, Azure AD Connect Eşitleme Hizmeti artık düzgün başlatılamaz veya eşitleme yapamaz. Bu makalede kurtarma adımları sağlar.

## <a name="symptoms"></a>Belirtiler
İki ortak Belirtiler şunlardır:

* Azure AD Connect eşitleme hizmeti **çalıştıran** ancak ile eşitlemek başarısız *"durduruldu-veritabanı-disk dolu"* hata.

* Azure AD Connect eşitleme hizmeti **başlatamaz**. Hizmeti başlatmak denediğinizde, olay 6323 ve hata iletisi ile başarısız *"SQL sunucusu disk alanı yetersiz olduğundan sunucu bir hatayla karşılaştı."*

## <a name="short-term-recovery-steps"></a>Kısa vadeli kurtarma adımları
Bu bölümde Azure AD Connect eşitleme işlemini devam ettirmek hizmeti için gerekli DB alanı geri kazanmak için adımları sağlar. Adımları içerir:
1. [Eşitleme hizmeti durumunu belirleme](#determine-the-synchronization-service-status)
2. [Veritabanı Daralt](#shrink-the-database)
3. [Geçmiş verileri çalışmasını sil](#delete-run-history-data)
4. [Çalıştırma geçmişi veri saklama süresini kısaltmak](#shorten-retention-period-for-run-history-data)

### <a name="determine-the-synchronization-service-status"></a>Eşitleme hizmeti durumunu belirleme
İlk olarak, eşitleme hizmeti veya hala çalışıp çalışmadığını belirleyin:

1. Azure AD Connect sunucunuza yönetici olarak oturum açın.

2. Git **Hizmet Denetim Yöneticisi**.

3. Durumunu denetleme **Microsoft Azure AD eşitleme**.


4. Çalışıyorsa, etmeyin durdurmak veya hizmeti yeniden başlatın. Atla [veritabanını küçültmek](#shrink-the-database) adım ve Git [Sil'i çalıştırmak geçmiş verileri](#delete-run-history-data) adım.

5. Çalışmıyorsa, hizmeti başlatmayı deneyin. Hizmet başarıyla başlatılırsa atla [veritabanını küçültmek](#shrink-the-database) adım ve Git [Sil'i çalıştırmak geçmiş verileri](#delete-run-history-data) adım. Aksi takdirde devam [veritabanını küçültmek](#shrink-the-database) adım.

### <a name="shrink-the-database"></a>Veritabanı Daralt
Eşitleme hizmeti başlatmak için yeterli DB alan boşaltmak için küçültme işlemini kullanın. Bu, DB alanı veritabanında boşluk kaldırarak boşaltır. Bu adım en yüksek çaba aynıdır, alan her zaman kurtarabilirsiniz garanti edilmez. Küçültme işlemi hakkında daha fazla bilgi edinmek için bu makaleyi okuyun [bir veritabanını küçültmek](https://msdn.microsoft.com/library/ms189035.aspx).

> [!IMPORTANT]
> Eşitleme hizmetini çalıştırmak için alırsanız, bu adımı atlayın. Artan parçalanması nedeniyle zayıf performansa neden olabilir gibi SQL DB daraltmak için önerilmez.

Azure AD Connect için oluşturulan veritabanının adıdır **ADSync**. Küçültme işlemi gerçekleştirmek için sysadmin veya veritabanı DBO'su ile olarak oturum açmalısınız. Azure AD Connect yüklemesi sırasında aşağıdaki hesapları sysadmin hakları verilir:
* Yerel Yöneticiler
* Azure AD Connect yükleme çalıştırmak için kullanılan kullanıcı hesabı.
* Azure AD Connect eşitleme hizmeti işletim bağlamı olarak kullanılan eşitleme hizmeti hesabı.
* Yükleme sırasında oluşturulan ADSyncAdmins yerel grubu.

1. Kopyalayarak veritabanını geri **ADSync.mdf** ve **ADSync_log.ldf** altında bulunan dosyaları `%ProgramFiles%\Microsoft Azure AD Sync\Data` güvenli bir konuma.

2. Yeni bir PowerShell oturumu başlatın.

3. Klasöre gidin `%ProgramFiles%\Microsoft SQL Server\110\Tools\Binn`.

4. Başlat **sqlcmd** komutunu çalıştırarak yardımcı programı `./SQLCMD.EXE -S “(localdb)\.\ADSync” -U <Username> -P <Password>`, bir sysadmin ya da veritabanı DBO kimlik bilgilerini kullanarak.

5. Sqlcmd isteminden veritabanını küçültmek için (1 >), girin `DBCC Shrinkdatabase(ADSync,1);`, ardından `GO` sonraki satırında.

6. İşlemi başarılı olursa, eşitleme hizmetini yeniden başlatmayı deneyin. Eşitleme hizmeti başlatmak için Git [Sil'i çalıştırmak geçmiş verileri](#delete-run-history-data) adım. Değilse, desteği ile iletişime geçin.

### <a name="delete-run-history-data"></a>Geçmiş verileri çalışmasını sil
Varsayılan olarak, Azure AD Connect çalıştırma geçmişi veri yedi gün kopyalayabilmek için yukarı korur. Bu adımda, Azure AD Connect eşitleme hizmeti yeniden eşitlemeyi başlayabilmeniz DB alanı geri kazanmak için çalışma geçmişini silin.

1.  Başlat **Eşitleme Hizmeti Yöneticisi'ni** Başlangıç → eşitleme hizmeti giderek.

2.  Git **Operations** sekmesi.

3.  Altında **Eylemler**seçin **Temizle çalışır**...

4.  Ya da seçebilirsiniz **tüm metinler temizleyin** veya **Temizle çalıştıran önce... <date>**  seçeneği. İki günden daha eski olan geçmiş verileri çalıştırmak temizleyerek başlatmanız önerilir. DB boyutu sorunu yaşayıp çalıştırmaya devam ederseniz, ardından **tüm metinler temizleyin** seçeneği.

### <a name="shorten-retention-period-for-run-history-data"></a>Çalıştırma geçmişi veri saklama süresini kısaltmak
Birden çok eşitleme döngüleri sonra 10 GB sınırını sorunu yaşayıp çalıştıran olasılığını azaltmak için bu adım bağlıdır.

1. Yeni bir PowerShell oturumu açın.

2. Çalıştırma `Get-ADSyncScheduler` ve geçerli Bekletme dönemi belirtir PurgeRunHistoryInterval özelliği not edin.

3. Çalıştırma `Set-ADSyncScheduler -PurgeRunHistoryInterval 2.00:00:00` iki gün bekletme süresini ayarlama. Saklama dönemi uygun şekilde ayarlayın.

## <a name="long-term-solution--migrate-to-full-sql"></a>Uzun vadeli çözüm – tam SQL geçiş
Genel olarak, 10 GB veritabanı boyutu artık şirket içi Active Directory'nizi Azure ad ile eşitlemek Azure AD Connect için yeterli olduğunu hatırlanması işlemidir. SQL server'ın tam sürümünü kullanmaya geçiş önerilir. Mevcut Azure AD Connect dağıtımının LocalDB’sini doğrudan tüm SQL sürümünün veritabanıyla değiştiremezsiniz. Bunun yerine, tam SQL sürümü içeren yeni bir Azure AD Connect sunucusu dağıtmanız gerekir. Yeni Azure AD Connect sunucusunun (SQL DB ile), mevcut Azure AD Connect sunucusunun (LocalDB ile) yanında hazırlık sunucusu olarak dağıtıldığı durumlarda, Swing geçişi yapmanız önerilir. 
* Azure AD Connect ile uzak SQL’i yapılandırma yönergeleri için, [Azure AD Connect özel yüklemesi](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-custom) makalesine bakın.
* Azure AD Connect yükseltmesinde Swing geçişi için, [Azure AD Connect: Önceki bir sürümden en son sürümü yükseltme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version#swing-migration) makalesine bakın.

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.
