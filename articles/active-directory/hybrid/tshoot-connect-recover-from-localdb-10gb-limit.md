---
title: "Azure AD Connect: 10 GB sınırı sorunundan Localdb'den kurtarma | Microsoft Docs"
description: Bu konu, Azure AD Connect eşitleme hizmeti LocalDB 10 GB karşılaştığında kurtarmak açıklar sorunu sınırlayın.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 41d081af-ed89-4e17-be34-14f7e80ae358
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4d420c64c5834f7d3cb11d2f5f59e3ed85a54891
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58435609"
---
# <a name="azure-ad-connect-how-to-recover-from-localdb-10-gb-limit"></a>Azure AD Connect: LocalDB 10 GB sınırından kurtarma
Azure AD Connect’e kimlik verilerini depolamak için bir SQL Server veritabanı gerekiyor. Azure AD Connect ile yüklenen varsayılan SQL Server 2012 Express LocalDB’yi kullanabileceğiniz gibi, kendi tam SQL’nizi de kullanabilirsiniz. SQL Server Express 10 GB boyut sınırını uygular. LocalDB’yi kullanırken bu sınıra ulaşıldığında, Azure AD Connect Eşitleme Hizmeti artık düzgün başlatılamaz veya eşitleme yapamaz. Bu makalede, Kurtarma adımları sağlar.

## <a name="symptoms"></a>Belirtiler
İki ortak belirtileri vardır:

* Azure AD Connect eşitleme hizmeti **çalıştıran** eşitleme başarısız olduğunda *"durduruldu-veritabanı-disk dolu"* hata.

* Azure AD Connect eşitleme hizmeti **başlatılamadı çünkü**. Hizmeti başlatmak denediğinizde, olay 6323 ve hata iletisi ile başarısız *"SQL sunucusu disk alanı yetersiz olduğundan sunucu bir hatayla karşılaştı."*

## <a name="short-term-recovery-steps"></a>Kısa vadeli kurtarma adımları
Bu bölümde, Azure AD Connect eşitleme işlemi devam ettirmek hizmeti için gerekli veritabanı alanını geri kazanarak adımları sağlar. Adımları içerir:
1. [Eşitleme hizmeti durumunu belirleme](#determine-the-synchronization-service-status)
2. [Veritabanı Daralt](#shrink-the-database)
3. [Geçmiş verileri çalıştırmak Sil](#delete-run-history-data)
4. [Çalıştırma geçmişi verilerini saklama süresini kısaltın](#shorten-retention-period-for-run-history-data)

### <a name="determine-the-synchronization-service-status"></a>Eşitleme hizmeti durumunu belirleme
İlk olarak, eşitleme hizmeti olmadığını hala çalışıp çalışmadığını belirleyin:

1. Azure AD Connect sunucunuza yönetici olarak oturum açın.

2. Git **Hizmet Denetimi Yöneticisi**.

3. Durumunu **Microsoft Azure AD eşitleme**.


4. Çalışıyorsa, durdurmaz ya hizmeti yeniden başlatın. Atla [veritabanını küçültmek](#shrink-the-database) adım ve Git [silme çalıştırma geçmişini](#delete-run-history-data) adım.

5. Çalışmıyorsa, hizmeti başlatmayı deneyin. Hizmet başarıyla başlatılırsa, atlama [veritabanını küçültmek](#shrink-the-database) adım ve Git [silme çalıştırma geçmişini](#delete-run-history-data) adım. Aksi takdirde devam [veritabanını küçültmek](#shrink-the-database) adım.

### <a name="shrink-the-database"></a>Veritabanı Daralt
Eşitleme hizmeti başlatmak için yeterli DB yer kazanmak için küçültme işlemini kullanın. Bu, veritabanında boşluk kaldırarak DB alanını boşaltır. Bu adım, alan her zaman kurtarabilirsiniz garanti mümkün olan en iyi aynıdır. Küçültme işlemi hakkında daha fazla bilgi edinmek için bu makaleyi okuyun [bir veritabanını küçültmek](https://msdn.microsoft.com/library/ms189035.aspx).

> [!IMPORTANT]
> Eşitleme hizmetini çalıştırmak için size bu adımı atlayın. SQL DB, artan parçalanma nedeniyle kötü performansa neden olabileceğinden daraltmak için önerilmez.

Azure AD Connect için oluşturduğunuz veritabanına adıdır **ADSync**. Küçültme işlemi gerçekleştirmek için sysadmin veya veritabanı DBO'su olarak oturum açmalısınız. Azure AD Connect yüklemesi sırasında aşağıdaki hesapları sysadmin hakları bahşedilir:
* Yerel Yöneticiler
* Azure AD Connect yüklemesi çalıştırmak için kullanılan kullanıcı hesabı.
* Azure AD Connect eşitleme hizmeti işletim bağlamı olarak kullanılan eşitleme hizmeti hesabı.
* Yükleme sırasında oluşturulan ADSyncAdmins yerel grup.

1. Geri kopyalayarak veritabanını **ADSync.mdf** ve **ADSync_log.ldf** altında bulunan dosyaları `%ProgramFiles%\Microsoft Azure AD Sync\Data` güvenli bir konuma.

2. Yeni bir PowerShell oturumu başlatın.

3. Klasörüne gidin `%ProgramFiles%\Microsoft SQL Server\110\Tools\Binn`.

4. Başlangıç **sqlcmd** komutu çalıştırarak yardımcı programı `./SQLCMD.EXE -S "(localdb)\.\ADSync" -U <Username> -P <Password>`, kimlik bilgisi bir sysadmin veya veritabanı DBO kullanarak.

5. Sqlcmd komut isteminde bir veritabanı daraltmak için (1 >), girin `DBCC Shrinkdatabase(ADSync,1);`çizgidir `GO` sonraki satırdaki.

6. İşlem başarılı olursa, eşitleme hizmeti yeniden başlatmayı deneyin. Eşitleme hizmeti başlatabiliyorsanız, Git [silme çalıştırma geçmişini](#delete-run-history-data) adım. Aksi durumda, desteğe başvurun.

### <a name="delete-run-history-data"></a>Geçmiş verileri çalıştırmak Sil
Varsayılan olarak, Azure AD Connect için çalıştırma geçmişi verilerini yedi gün değerinde yukarı korur. Bu adımda, size Azure AD Connect eşitleme hizmeti yeniden eşitlemeyi başlayabilmesi veritabanı alanını geri kazanarak çalıştırma geçmişi verilerini silin.

1. Başlangıç **Eşitleme Hizmeti Yöneticisi** → Başlangıç eşitleme hizmetine giderek.

2. Git **işlemleri** sekmesi.

3. Altında **eylemleri**seçin **Temizle çalıştırmaları**...

4. Ya da tercih edebilirsiniz **tüm çalıştırmalar Temizle** veya **Temizle çalıştıran önce... \<tarih >** seçeneği. İki günden eski olan geçmiş verileri çalıştırmak temizleyerek Başlat önerilir. DB boyutu sorunu çalıştırmaya devam ederseniz, ardından **tüm çalıştırmalar Temizle** seçeneği.

### <a name="shorten-retention-period-for-run-history-data"></a>Çalıştırma geçmişi verilerini saklama süresini kısaltın
Bu adım, birden çok eşitleme döngülerinizin sonra 10 GB sınırına sorunu çalıştıran olasılığını azaltmaktır.

1. Yeni bir PowerShell oturumu açın.

2. Çalıştırma `Get-ADSyncScheduler` ve geçerli saklama süresinin belirtir PurgeRunHistoryInterval özelliğini not edin.

3. Çalıştırma `Set-ADSyncScheduler -PurgeRunHistoryInterval 2.00:00:00` iki güne ilişkin saklama süresini ayarlamak için. Saklama dönemi uygun şekilde ayarlayın.

## <a name="long-term-solution--migrate-to-full-sql"></a>Uzun süreli çözüm – tam SQL'e geçirme
Genel olarak, sorunu 10 GB veritabanı boyutu artık şirket içi Active Directory'nizi Azure ad eşitleme Azure AD Connect için yeterli olduğunu gösterir. SQL server'ın tam sürümünü kullanmaya geçmeniz özellikle önerilir. Mevcut Azure AD Connect dağıtımının LocalDB’sini doğrudan tüm SQL sürümünün veritabanıyla değiştiremezsiniz. Bunun yerine, tam SQL sürümü içeren yeni bir Azure AD Connect sunucusu dağıtmanız gerekir. Yeni Azure AD Connect sunucusunun (SQL DB ile), mevcut Azure AD Connect sunucusunun (LocalDB ile) yanında hazırlık sunucusu olarak dağıtıldığı durumlarda, Swing geçişi yapmanız önerilir. 
* Azure AD Connect ile uzak SQL’i yapılandırma yönergeleri için, [Azure AD Connect özel yüklemesi](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-custom) makalesine bakın.
* Azure AD Connect yükseltmesinde Swing geçişi için, [Azure AD Connect: Önceki bir sürümden en son sürüme yükseltme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version#swing-migration) makalesine bakın.

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
