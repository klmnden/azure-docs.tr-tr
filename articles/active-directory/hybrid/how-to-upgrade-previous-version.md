---
title: 'Azure AD Connect: Önceki sürümden yükseltme | Microsoft Docs'
description: Azure Active Directory yerinde yükseltme ve swing geçişi dahil olmak üzere Connect, en son sürümüne yükseltmek için farklı yöntemleri açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 31f084d8-2b89-478c-9079-76cf92e6618f
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: Identity
ms.date: 04/08/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2a3e7373a8b0354a3d08debf944f2f77f1609382
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59267047"
---
# <a name="azure-ad-connect-upgrade-from-a-previous-version-to-the-latest"></a>Azure AD Connect: Önceki bir sürümden en son sürüme yükseltme
Bu konuda, Azure Active Directory (Azure AD) Connect yüklemenizi en son sürüme yükseltmek için kullanabileceğiniz farklı yöntemler açıklanır. Kendi Azure AD Connect'in sürümlerinde geçerli tutmanızı öneririz. Ayrıca adımlarda kullandığınız [Swing geçişi](#swing-migration) bölümünde önemli bir yapılandırma değişikliği yaptığınızda.

>[!NOTE]
> Şu anda Azure AD Connect sürümünden geçerli sürümüne yükseltmek için desteklenir. Ad eşitleme veya DirSync yerinde yükseltmeler desteklenmez ve swing geçişi gereklidir.  Dirsync'ten yükseltme yapmaya istiyorsanız bkz [Azure AD eşitleme aracından (DirSync) yükseltme](how-to-dirsync-upgrade-get-started.md) veya [Swing geçişi](#swing-migration) bölümü.  </br>Uygulamada, son derece eski sürümlerini kullanan müşteriler, doğrudan Azure AD Connect'e ilgili sorunlarla karşılaşabilirsiniz. Yıllardır, üretimde olan sunucuları genellikle birkaç düzeltme eki uygulanmış olan ve bunların tümü için katılması.  12-18 ay içinde yükseltilmemiş müşteriler swing yükseltme genellikle dikkate almanız gereken yerine gibi en koruyucu ve az riskli seçenek budur.

Dirsync'ten yükseltme yapmaya istiyorsanız bkz [Azure AD eşitleme aracından (DirSync) yükseltme](how-to-dirsync-upgrade-get-started.md) yerine.

Azure AD Connect'e yükseltmenin için kullanabileceğiniz birkaç farklı strateji vardır.

| Yöntem | Açıklama |
| --- | --- |
| [Otomatik yükseltme](how-to-connect-install-automatic-upgrade.md) |Bir hızlı yükleme sahip müşteriler için en kolay yöntemi budur. |
| [Yerinde yükseltme](#in-place-upgrade) |Tek bir sunucu varsa, yükleme yerinde aynı sunucuda yükseltebilirsiniz. |
| [Swing geçişi](#swing-migration) |İki sunucu, yeni yayın veya yapılandırma sunucuları birini hazırlayın ve hazır olduğunuzda, etkin sunucunun değiştirin. |

İzinler için bilgi [yükseltme için gereken izinler](reference-connect-accounts-permissions.md#upgrade).

> [!NOTE]
> Değişiklikler Azure AD'ye eşitlemeye başlamak yeni Azure AD Connect sunucunuzu etkinleştirdikten sonra DirSync veya Azure AD Sync kullanmaya geri değil. Azure AD Connect'ten DirSync ve Azure AD Sync gibi eski istemcilere eski sürüme düşürme desteklenmez ve Azure AD'de veri kaybı gibi sorunlara açabilir.

## <a name="in-place-upgrade"></a>Yerinde yükseltme
Azure AD eşitleme veya Azure AD Connect taşımak için bir yerinde yükseltme çalışır. Dirsync'ten taşımak veya Forefront Identity Manager (FIM) + Azure AD Bağlayıcısı ile bir çözüm için çalışmaz.

Tek bir sunucu ve kısa yaklaşık 100.000 nesneler varsa, bu yöntem, tercih edilir. Kullanıma hazır eşitleme kuralları için herhangi bir değişiklik varsa, bir tam içeri aktarma ve tam eşitleme yükseltme işleminden sonra oluşur. Bu yöntem, yeni yapılandırmayı sistemdeki mevcut tüm nesnelere uygulanır sağlar. Bu çalışma, eşitleme altyapısı kapsamındaki nesne sayısına bağlı olarak birkaç saat sürebilir. (Bu varsayılan olarak 30 dakikada bir eşitlenir) normal delta Eşitleme Zamanlayıcısı askıya alındı ancak parola eşitleme devam ediyor. Hafta sırasında yerinde yükseltme yapılması göz önünde bulundurabilirsiniz. Daha sonra normal değişim içeri aktarma/eşitleme varsa yeni Azure AD Connect ile out-of-box yapılandırması herhangi bir değişiklik sürüm, bunun yerine başlatır.  
![Yerinde yükseltme](./media/how-to-upgrade-previous-version/inplaceupgrade.png)

Ardından kullanıma hazır eşitleme kuralları için değişiklik yaptıysanız, bu kurallar geri yükseltmeden varsayılan yapılandırmayı ayarlanır. Yapılandırmanızı arasında yükseltme tutulur emin olmak için açıklandığı gibi değişiklikler yapmak emin [varsayılan yapılandırmanın değiştirilmesine yönelik en iyi uygulamalar](how-to-connect-sync-best-practices-changing-default-configuration.md).

Yerinde yükseltme sırasında olabilir yükseltme tamamlandıktan sonra yürütülecek (tam içeri aktarma adım ve tam eşitleme adım dahil) belirli bir eşitleme etkinliklerini gerektiren değişiklikler sunulmaktadır. Bu tür etkinlikler erteleneceği bölümüne bakın. [yükseltmeden sonra tam eşitleme erteleneceği nasıl](#how-to-defer-full-synchronization-after-upgrade).

Standart bağlayıcı ile (örneğin, genel LDAP Bağlayıcısı ve genel SQL Bağlayıcısı) Azure AD Connect kullanıyorsanız, karşılık gelen Bağlayıcı yapılandırması yenilemelisiniz [Eşitleme Hizmeti Yöneticisi](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-service-manager-ui-connectors) sonra yerinde yükseltme. Bağlayıcı yapılandırmasını yenileme hakkında daha fazla bilgi için makale bölümüne bakın. [bağlayıcı sürümü yayın geçmişi - sorun giderme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history#troubleshooting). Düzgün yapılandırmasını yenileme değil, içeri ve dışarı aktarma adımlarını çalıştırmayı bağlayıcı için çalışmaz. Uygulama olay günlüğüne ileti ile aşağıdaki hatayı alırsınız *"derleme sürümü AAD Bağlayıcı yapılandırması ("X.X.XXX. "X") ("X.X.XXX. gerçek sürümden daha eski "X"), "C:\Program Files\Microsoft Azure AD'ye Sync\Extensions\Microsoft.IAM.Connector.GenericLdap.dll".*

## <a name="swing-migration"></a>Swing geçişi
Karmaşık bir dağıtımı veya birçok nesne varsa, Canlı sistemde bir yerinde yükseltme yapmak için pratik olmayabilir. Bazı müşteriler için bu işlem, birden çok gün--sürebilir ve bu süre boyunca hiçbir değişim değişiklikleri işlenir. Ayrıca, yapılandırmanızı önemli değişiklikler yapmayı planlıyor ve buluta gönderilen önce bunları denemek istediğinizde bu yöntemi kullanabilirsiniz.

Bu senaryolar için önerilen yöntem, swing geçişi kullanmaktır. (En az) iki sunucu--etkin bir sunucu ve bir hazırlık sunucusu ihtiyacınız vardır. (Aşağıdaki resimde mavi düz çizgiler ile gösterilen) etkin sunucu için etkin üretim yük sorumludur. (Kesik mor çizgiler ile gösterilen) bir hazırlık sunucusu yapılandırması ve yeni sürüm ile hazırlanır. Tam olarak hazır olduğunda, bu sunucu etkinleştirilir. Artık eski sürüm veya yapılandırma yüklü olan, hazırlama sunucuya yapılan ve yükseltilir önceki etkin sunucu.

İki sunucu, farklı sürümlerini kullanabilirsiniz. Örneğin, Azure AD eşitleme yetkisini almayı planladığınız etkin sunucunun kullanabilirsiniz ve yeni hazırlık sunucusu, Azure AD Connect kullanabilirsiniz. Yeni bir yapılandırma geliştirmek için swing geçişi kullanırsanız, iki sunucu üzerinde aynı sürümde olması için iyi bir fikirdir.  
![Hazırlama sunucusu](./media/how-to-upgrade-previous-version/stagingserver1.png)

> [!NOTE]
> Bazı müşteriler bu senaryo için üç veya dört sunucu tercih edebilirsiniz. Hazırlama sunucusu yükseltildiğinde, bir yedekleme sunucusu yoksa [olağanüstü durum kurtarma](how-to-connect-sync-staging-server.md#disaster-recovery). Üç veya dört sunucularıyla birincil/bekleme sunucularıyla olduğunu her zaman almaya hazır bir hazırlık sunucusu sağlar. yeni sürümü, bir dizi hazırlayabilirsiniz.

Bu adımlar, Azure AD eşitleme veya FIM + Azure AD Bağlayıcısı ile bir çözüm taşıma için de geçerlidir. Bu adımlar için bir DirSync çalışmaz, ancak (paralel dağıtım olarak da bilinir) aynı swing geçişi yöntem adımlarla için bir DirSync [yükseltme Azure Active Directory eşitleme (DirSync)](how-to-dirsync-upgrade-get-started.md).

### <a name="use-a-swing-migration-to-upgrade"></a>Swing geçişi yükseltmek için kullanın
1. Etkin sunucu ve hazırlık sunucusu sunucularda hem de Azure AD Connect'i kullanın ve yalnızca değiştirme ve emin bir yapılandırma yapmayı planladığınız her ikisi de aynı sürümü kullanıyor. Bu, daha sonra farkları karşılaştırmak kolaylaştırır. Azure AD eşitleme'den yükseltme yapıyorsanız, bu sunucular farklı sürümleri vardır. Azure AD Connect'in eski bir sürümden yükseltiyorsanız, aynı sürümü kullanan iki sunucu başlatmak için iyi bir fikirdir, ancak gerekli değildir.
2. Özel yapılandırma yaptığınız ve hazırlama sunucunuzu bu gerekli değildir, altındaki adımları [özel bir yapılandırma için hazırlık sunucusu active sunucudan taşıma](#move-a-custom-configuration-from-the-active-server-to-the-staging-server).
3. Azure AD Connect'in önceki bir sürümden yükseltiyorsanız, hazırlık sunucusu en son sürüme yükseltin. Azure AD eşitleme'den taşıyorsanız, Azure AD Connect'i hazırlama sunucunuza yükleyin.
4. Tam içeri aktarma ve tam eşitleme hazırlama sunucunuzda çalışması eşitleme altyapısı sağlar.
5. "Doğrula" bölümündeki adımları kullanarak beklenmeyen değişiklikleri yeni yapılandırmayı neden olduğunu siz olun [bir sunucu yapılandırmasını doğrulamak](how-to-connect-sync-staging-server.md#verify-the-configuration-of-a-server). Bir şey beklendiği gibi değilse, bunu düzeltmek için alma ve eşitleme ve adımları izleyerek iyi gösterilene kadar verileri doğrulayın.
6. Active server hazırlama sunucuya geçiş. Bu son adım "Anahtar etkin sunucu" bileşenidir [bir sunucu yapılandırmasını doğrulamak](how-to-connect-sync-staging-server.md#verify-the-configuration-of-a-server).
7. Azure AD Connect yükseltiyorsanız, artık en son sürüm için hazırlama modunda sunucusunu yükseltin. Yükseltme yapılandırma ve verileri almak için önce aynı adımları izleyin. Azure AD eşitleme'den yükseltilmiş ise, artık devre dışı bırakmak ve eski sunucunuzun yetkisini alma.

### <a name="move-a-custom-configuration-from-the-active-server-to-the-staging-server"></a>Özel bir yapılandırma için hazırlık sunucusu active sunucudan taşıma
Etkin sunucunun yapılandırma değişiklikleri yaptıysanız, aynı değişiklikler için hazırlık sunucusu uygulandığından emin olmanız gerekir. Bu taşıma ile yardımcı olmak için kullanabileceğiniz [Azure AD Connect yapılandırma Belgeleyici'yi](https://github.com/Microsoft/AADConnectConfigDocumenter).

PowerShell kullanarak oluşturdunuz özel eşitleme kuralları taşıyabilirsiniz. Her iki sistemlerinde aynı şekilde diğer değişiklikleri uygulamalısınız ve değişiklikleri geçiremezsiniz. [Yapılandırma Belgeleyici'yi](https://github.com/Microsoft/AADConnectConfigDocumenter) aynı olduklarından emin olmak için iki sistem karşılaştırma yardımcı olabilir. Bu bölümde bulunan adımları otomatikleştirme ile araç da yardımcı olabilir.

Aşağıdakilerden her iki sunucuda aynı şekilde yapılandırmanız gerekir:

* Aynı orman bağlantısı
* Tüm etki alanı ve OU filtreleme
* Parola Eşitleme ve parola geri yazma gibi aynı isteğe bağlı özellikler

**Özel eşitleme kuralları Taşı**  
Özel eşitleme kuralları taşımak için aşağıdakileri yapın:

1. Açık **eşitleme kuralları Düzenleyicisi** etkin sunucunuzdaki.
2. Özel bir kural seçin. Tıklayın **dışarı**. Bu, bir not defteri pencereyi getirir. Geçici dosya bir PS1 uzantısıyla kaydedin. Bu PowerShell Betiği hale getirir. PS1 dosya hazırlama sunucuya kopyalayın.  
   ![Eşitleme kuralı dışarı aktarma](./media/how-to-upgrade-previous-version/exportrule.png)
3. Bağlayıcı GUID hazırlama sunucusunda farklıdır ve bunu değiştirmeniz gerekir. GUID almak için başlangıç **eşitleme kuralları Düzenleyicisi**, aynı bağlı sistem temsil eder ve kullanıma hazır kuralları birini **dışarı**. GUID PS1 dosyanızdaki hazırlık sunucusu GUID'ini değiştirin.
4. Bir PowerShell komut isteminde PS1 dosyasını çalıştırın. Bu hazırlama sunucusunda özel eşitleme kuralı oluşturur.
5. Bu, tüm özel kurallarınızı için yineleyin.

## <a name="how-to-defer-full-synchronization-after-upgrade"></a>Yükseltmeden sonra tam eşitleme erteleneceği nasıl
Yerinde yükseltme sırasında olabilir. (tam içeri aktarma adım ve tam eşitleme adım dahil) belirli bir eşitleme etkinliklerini yürütülecek gerektiren değişiklikler sunulmaktadır. Örneğin, bağlayıcı şema değişiklikleri gerektiren **tam içeri aktarma** adım ve kullanıma hazır bir eşitleme kuralı değişiklik yapılması **tam eşitleme** adım etkilenen bağlayıcılarda yürütülecek. Yükseltme işlemi sırasında Azure AD Connect eşitleme etkinlikleri gerekli olduğunu belirler ve bunları olarak kaydeder *geçersiz kılmalar*. Aşağıdaki eşitleme döngüsünde Eşitleme Zamanlayıcısı, bu geçersiz kılma işlemleri alır ve bunları yürütür. Bir geçersiz kılma başarıyla yürütüldükten sonra kaldırılır.

Burada hemen yükseltmeden sonra gerçekleşmesi için bu geçersiz kılmaları istemediğiniz durumlar olabilir. Örneğin, çok sayıda eşitlenmiş nesneleri sahip olursunuz ve iş saatlerinden sonra gerçekleşmesi için eşitleme adımları istersiniz. Bu geçersiz kılmaları kaldırmak için:

1. Yükseltme işlemi sırasında **işaretini kaldırın** seçeneği **Yapılandırma tamamlandığında eşitleme işlemini başlatmak**. Bu, Eşitleme Zamanlayıcısı'nı devre dışı bırakır ve geçersiz kılmaları kaldırılmadan önce eşitleme döngüsü alma yerden otomatik olarak önler.

   ![DisableFullSyncAfterUpgrade](./media/how-to-upgrade-previous-version/disablefullsync01.png)

2. Yükseltme tamamlandıktan sonra hangi geçersiz kılmaları eklenmiş olan bulmak için aşağıdaki cmdlet'i çalıştırın: `Get-ADSyncSchedulerConnectorOverride | fl`

   >[!NOTE]
   > Geçersiz kılmaları bağlayıcı özgüdür. Aşağıdaki örnekte, tam içeri aktarma adım ve tam eşitleme adım hem şirket içi AD Bağlayıcısı ve Azure AD Bağlayıcısı eklenmiştir.

   ![DisableFullSyncAfterUpgrade](./media/how-to-upgrade-previous-version/disablefullsync02.png)

3. Varolan aşağı not eklenmiş geçersiz kılar.
   
4. Tam içeri aktarma hem de rastgele bir bağlayıcı üzerinde tam eşitleme için geçersiz kılmaları kaldırmak için aşağıdaki cmdlet'i çalıştırın: `Set-ADSyncSchedulerConnectorOverride -ConnectorIdentifier <Guid-of-ConnectorIdentifier> -FullImportRequired $false -FullSyncRequired $false`

   Tüm bağlayıcılar üzerinde geçersiz kılmaları kaldırmak için aşağıdaki PowerShell betiğini yürütün:

   ```
   foreach ($connectorOverride in Get-ADSyncSchedulerConnectorOverride)
   {
       Set-ADSyncSchedulerConnectorOverride -ConnectorIdentifier $connectorOverride.ConnectorIdentifier.Guid -FullSyncRequired $false -FullImportRequired $false
   }
   ```

5. Zamanlayıcı sürdürmek için aşağıdaki cmdlet'i çalıştırın: `Set-ADSyncScheduler -SyncCycleEnabled $true`

   >[!IMPORTANT]
   > Gerekli eşitleme adımları en yakın zamanda yürütülecek unutmayın. El ile Eşitleme Hizmeti Yöneticisi'ni kullanarak aşağıdaki adımları yürütün veya Set-ADSyncSchedulerConnectorOverride cmdlet'ini kullanarak geçersiz kılmaları geri ekleyin.

Rastgele bir bağlayıcıda tam içeri aktarma hem de tam eşitleme için geçersiz kılmalar eklemek için aşağıdaki cmdlet'i çalıştırın:  `Set-ADSyncSchedulerConnectorOverride -ConnectorIdentifier <Guid> -FullImportRequired $true -FullSyncRequired $true`

## <a name="troubleshooting"></a>Sorun giderme
Aşağıdaki bölümde, sorun giderme ve Azure AD Connect yükseltme bir sorunla karşılaşırsanız, kullanabileceğiniz bilgiler içerir.

### <a name="azure-active-directory-connector-missing-error-during-azure-ad-connect-upgrade"></a>Azure Active Directory Bağlayıcısı eksik hata sırasında Azure AD Connect'i Yükselt

Azure AD Connect'in önceki sürümünden yükselttiğinizde, aşağıdaki hata yükseltme başındaki neden olabilir 

![Hata](./media/how-to-upgrade-previous-version/error1.png)

Azure Active Directory bağlayıcısını tanımlayıcıyla b891884f-051e-4a83-95af - 2544101 c 9083, geçerli Azure AD Connect yapılandırma için mevcut olmadığından bu hata oluşur. Bu durumda doğrulamak için bir PowerShell penceresi açın ve şu cmdlet'i çalıştırın `Get-ADSyncConnector -Identifier b891884f-051e-4a83-95af-2544101c9083`

```
PS C:\> Get-ADSyncConnector -Identifier b891884f-051e-4a83-95af-2544101c9083
Get-ADSyncConnector : Operation failed because the specified MA could not be found.
At line:1 char:1
+ Get-ADSyncConnector -Identifier b891884f-051e-4a83-95af-2544101c9083
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ReadError: (Microsoft.Ident...ConnectorCmdlet:GetADSyncConnectorCmdlet) [Get-ADSyncConne
   ctor], ConnectorNotFoundException
    + FullyQualifiedErrorId : Operation failed because the specified MA could not be found.,Microsoft.IdentityManageme
   nt.PowerShell.Cmdlet.GetADSyncConnectorCmdlet

```

PowerShell cmdlet'i hata raporlarını **belirtilen MA bulunamadı**.

Geçerli Azure AD Connect yapılandırma için yükseltme desteklenmediği için bu oluştuğunu nedeni. 

Azure AD Connect'i yeni bir sürümünü yüklemek istiyorsanız: Azure AD Connect sihirbazını kapatın, mevcut Azure AD Connect'i kaldırın ve yeni Azure AD Connect temiz bir yüklemesini gerçekleştirme.



## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md).
