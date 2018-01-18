---
title: "Azure AD Connect: Önceki bir sürümden yükseltme | Microsoft Docs"
description: "Azure Active Directory yerinde yükseltme ve esnek geçiş dahil olmak üzere Connect, en son sürümüne yükseltmek için farklı yöntemler açıklanmaktadır."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 31f084d8-2b89-478c-9079-76cf92e6618f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Identity
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 4d431a9e0fab8d46b244fd40178ede594c095893
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-connect-upgrade-from-a-previous-version-to-the-latest"></a>Azure AD Connect: En son önceki bir sürümünden yükseltme
Bu konuda, Azure Active Directory (Azure AD) Bağlan yüklemenizi en son sürümüne yükseltme için kullanabileceğiniz farklı yöntemler açıklanmaktadır. Kendiniz Azure AD Connect sürümleriyle geçerli tutmanızı öneririz. Ayrıca içindeki adımları kullanın [çarpma geçiş](#swing-migration) önemli bir yapılandırma değişikliği yaptığınızda bölüm.

Dirsync'ten yükseltme istiyorsanız, bkz: [Azure AD eşitleme aracından (DirSync) yükseltme](active-directory-aadconnect-dirsync-upgrade-get-started.md) yerine.

Azure AD Connect yükseltmek için kullanabileceğiniz birkaç farklı stratejiler vardır.

| Yöntem | Açıklama |
| --- | --- |
| [Otomatik yükseltme](active-directory-aadconnect-feature-automatic-upgrade.md) |Hızlı yükleme sahip müşteriler için en kolay yöntem budur. |
| [Yerinde yükseltme](#in-place-upgrade) |Tek bir sunucu varsa, yükleme yerinde aynı sunucuda yükseltebilirsiniz. |
| [Esnek geçiş](#swing-migration) |İki sunucu ile yeni sürümü veya yapılandırma sunucularıyla birini hazırlamak ve hazır olduğunuzda etkin sunucunun değiştirin. |

İzinler için bilgi [yükseltme için gereken izinler](active-directory-aadconnect-accounts-permissions.md#upgrade).

> [!NOTE]
> Değişiklikleri Azure ad eşitleme başlatmak, yeni Azure AD Connect sunucusu etkinleştirdikten sonra DirSync veya Azure AD Sync kullanılarak geri gerekir. DirSync ve Azure AD eşitleme gibi eski istemciler için Azure AD Connect'ten eski sürüme düşürmeyi desteklenmez ve veri kaybı gibi sorunları Azure AD'de yol açabilir.

## <a name="in-place-upgrade"></a>Yerinde yükseltme
Azure AD eşitleme veya Azure AD Connect taşımak için bir yerinde yükseltme çalışır. Forefront Identity Manager (FIM) + Azure AD Bağlayıcısı ile bir çözüm veya Dirsync'ten taşıma için çalışmıyor.

Tek bir sunucu ve değerinden yaklaşık 100.000 nesneye sahip olduğunda bu tercih edilen bir yöntemdir. Out-of-box eşitleme kuralları herhangi bir değişiklik varsa, bir tam içeri aktarma ve tam eşitleme yükseltme işleminden sonra oluşur. Bu yöntem, yeni yapılandırma sistemde var olan tüm nesnelere uygulanmasını sağlar. Bu farklı çalıştır eşitleme altyapısı kapsamındaki nesneleri sayısına bağlı olarak birkaç saat sürebilir. (Bu, varsayılan olarak 30 dakikada bir eşitlenir) normal delta Eşitleme Zamanlayıcısı askıya alındı, ancak parola eşitleme devam eder. Bir hafta sırasında yerinde yükseltme yapılması düşünebilirsiniz. Varsa yeni Azure AD Connect ile out-of-box yapılandırmada değişiklik yapılmadan sürüm, normal delta alma/eşitlemesi yerine başlatır.  
![Yerinde yükseltme](./media/active-directory-aadconnect-upgrade-previous-version/inplaceupgrade.png)

Out-of-box eşitleme kuralları için değişiklik yaptıysanız, ardından bu kurallar geri yükseltmeden varsayılan yapılandırmaya ayarlanır. Yapılandırmanızı arasında yükseltme tutulur emin olmak için açıklandığı gibi değişiklikler yaptığınızdan emin olun [en iyi uygulamalar varsayılan yapılandırmasını değiştirmek için](active-directory-aadconnectsync-best-practices-changing-default-configuration.md).

Yerinde yükseltme sırasında olabilir (tam alma adımı ve tam eşitleme adımı dahil) belirli eşitleme etkinliklerini yükseltme işlemi tamamlandıktan sonra çalıştırılacak gerektiren sunulan değişiklikler. Bu tür etkinlikler erteleme bölümüne bakın. [yükselttikten sonra tam eşitleme erteleme nasıl](#how-to-defer-full-synchronization-after-upgrade).

Standart olmayan Bağlayıcısı ile (örneğin, genel LDAP Bağlayıcısı ve genel SQL bağlayıcı) Azure AD Connect kullanıyorsanız, karşılık gelen Bağlayıcı yapılandırması yenilemelisiniz [Eşitleme Hizmeti Yöneticisi'ni](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-service-manager-ui-connectors) yerinde yükseltme sonrasında. Bağlayıcı yapılandırmasını yenileme hakkında daha fazla bilgi için makale bölümüne bakın [Bağlayıcısı sürüm yayımlama geçmişi - sorun giderme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history#troubleshooting). Doğru yapılandırmasını yenileme değil, içeri ve dışarı aktarma adımlarını çalıştırmak için bağlayıcı çalışmaz. İletiyle uygulama olay günlüğünde aşağıdaki hatayı alırsınız *"derleme sürümünü AAD Bağlayıcı yapılandırması ("X.X.XXX. "X") ("X.X.XXX. gerçek sürümden daha eski "X"), "C:\Program Files\Microsoft Azure AD Sync\Extensions\Microsoft.IAM.Connector.GenericLdap.dll".*

## <a name="swing-migration"></a>Swing geçişi
Karmaşık bir dağıtım veya çok sayıda nesne varsa, Canlı sistem üzerinde bir yerinde yükseltme yapmak için pratik olabilir. Bazı müşteriler için bu işlem birden fazla gün--sürebilir ve bu süre boyunca hiçbir delta değişiklikleri işlenir. Ayrıca yapılandırmanızı önemli değişiklikler yapmayı planlayın ve buluta gönderilen önce bunları denemenin istediğinizde bu yöntemi kullanabilirsiniz.

Bu senaryolar için önerilen yöntem, esnek geçiş kullanmaktır. (En az) iki sunucu--bir etkin sunucu ve bir Hazırlama sunucusu gerekir. (Aşağıdaki resimde düz mavi çizgilerle gösterilir) etkin sunucu için etkin üretim yükü sorumludur. Hazırlama sunucunun (kesikli mor çizgilerle gösterilir) yeni sürüm veya yapılandırma hazırlanır. Tam olarak hazır olduğunda, bu sunucu etkinleştirilir. Şimdi eski sürüm veya yapılandırma yüklü olduğundan, hazırlama Server'a yapılan ve yükseltildiğinde önceki active sunucu.

İki sunucu farklı sürümlerini kullanabilirsiniz. Örneğin, Azure AD eşitleme yetkisini almayı planladığınız active server kullanabilirsiniz ve yeni hazırlama sunucunun Azure AD Connect kullanabilirsiniz. Yeni bir yapılandırma geliştirmek için esnek geçiş kullanırsanız, iki sunucularında aynı sürümlerde iyi bir fikirdir.  
![Hazırlama sunucusu](./media/active-directory-aadconnect-upgrade-previous-version/stagingserver1.png)

> [!NOTE]
> Bazı müşteriler, bu senaryo için üç veya dört sunucuların sahip olması tercih edilir. Hazırlama sunucusu yükseltildiğinde için yedek bir sunucu yok [olağanüstü durum kurtarma](active-directory-aadconnectsync-operations.md#disaster-recovery). Üç veya dört sunucularıyla her zaman almaya hazır olan bir Hazırlama sunucusu olduğunu sağlayan yeni sürümle birincil/bekleme sunucularının bir kümesi hazırlayabilirsiniz.

Bu adımlar, Azure AD eşitleme veya FIM + Azure AD Bağlayıcısı çözümüyle taşımak için de geçerlidir. Bu adımları DirSync için çalışmaz, ancak (paralel dağıtım olarak da bilinir) aynı esnek geçiş yöntem adımlara DirSync için [yükseltme Azure Active Directory eşitleme (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md).

### <a name="use-a-swing-migration-to-upgrade"></a>Yükseltme için esnek geçiş işlemini kullanın
1. Sunucularda hem de Azure AD Connect kullanıyorsanız ve yalnızca bir yapılandırma değiştirmek, olduğundan emin olun yapmayı planladığınız etkin sunucu ve hazırlama sunucu her ikisi de aynı sürümünü kullanıyor. Bu değişiklikleri daha sonra karşılaştırın kolaylaştırır. Azure AD eşitleme'den yükseltme yapıyorsanız, bu sunucular farklı sürümlerde. Azure AD Connect eski bir sürümden yükseltirken, aynı sürümü kullanan iki sunucu ile başlatmak için iyi bir fikirdir, ancak gerekli değildir.
2. Özel yapılandırma yapmış olduğunuz ve hazırlama sunucunuz varsa değil, adımları altında [özel bir yapılandırma etkin sunucudan hazırlama sunucuya taşıyın](#move-custom-configuration-from-active-to-staging-server).
3. Azure AD Connect, önceki bir sürümünden yükseltme yapıyorsanız hazırlama sunucuyu en son sürüme yükseltin. Azure AD eşitleme'den taşıyorsanız, Azure AD Connect'i hazırlama sunucunuza yükleyin.
4. Tam içeri aktarma ve tam eşitleme hazırlama sunucunuzda çalışan eşitleme altyapısı sağlar.
5. Doğrulayın "Doğrula" altındaki adımları kullanarak beklenmeyen değişiklikler yeni yapılandırmayı neden olduğunu kaydetmedi [bir sunucunun yapılandırmasını doğrulama](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server). Bir şey beklendiği gibi değilse, düzeltmek için alma çalıştırın ve eşitleme ve adımları izleyerek iyi görünüyor kadar verileri doğrulayın.
6. Etkin sunucusu olarak hazırlama sunucuya geçiş. Bu son adım "Anahtar active server" olarak [bir sunucunun yapılandırmasını doğrulama](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server).
7. Azure AD Connect yükseltiyorsanız, en son sürüm için hazırlama modunda sunulmuştur sunucusunu yükseltin. Yükseltme yapılandırma ve verileri almak için önce aynı adımları izleyin. Azure AD eşitleme'den yükseltme yaptıysanız, şimdi kapatmak ve eski sunucunuz yetkisini.

### <a name="move-a-custom-configuration-from-the-active-server-to-the-staging-server"></a>Özel yapılandırma etkin sunucudan hazırlama sunucuya taşıyın.
Yapılandırma değişiklikleri etkin sunucunun yaptıysanız, aynı değişiklikleri hazırlama sunucuya uygulanan emin olmanız gerekir. Bu taşıma ile yardımcı olmak için kullanabileceğiniz [Azure AD Connect yapılandırma Belgeleyici'yi](https://github.com/Microsoft/AADConnectConfigDocumenter).

PowerShell kullanarak oluşturduktan özel eşitleme kuralları taşıyabilirsiniz. Aynı şekilde diğer değişiklikler her iki sistemde uygulamanız gerekir ve değişiklikler geçiremezsiniz. [Yapılandırma Belgeleyici'yi](https://github.com/Microsoft/AADConnectConfigDocumenter) aynı olduklarından emin olmak için iki sistem karşılaştırma yardımcı olabilir. Bu bölümde bulunan adımları getirmede aracı de yardımcı olabilir.

Şunları sunucularda hem de aynı şekilde yapılandırmanız gerekir:

* Aynı orman bağlantısı
* Tüm etki alanı ve OU filtreleme
* Parola Eşitleme ve parola geri yazma gibi aynı isteğe bağlı özellikler

**Özel eşitleme kuralları taşıma**  
Özel eşitleme kuralları taşımak için aşağıdakileri yapın:

1. Açık **eşitleme kuralları Düzenleyicisi** etkin sunucunuzda.
2. Özel bir kural seçin. Tıklatın **verme**. Bir not defteri pencereyi getirir. Geçici dosya bir PS1 uzantısıyla kaydedin. Bu, bir PowerShell komut dosyası sağlar. PS1 dosyası hazırlama sunucusuna kopyalayın.  
   ![Eşitleme kuralı dışarı aktarma](./media/active-directory-aadconnect-upgrade-previous-version/exportrule.png)
3. Bağlayıcı GUID hazırlama sunucusunda farklıdır ve değiştirmeniz gerekir. GUID almak için başlangıç **eşitleme kuralları Düzenleyicisi**, aynı bağlı sistemini temsil eder ve out-of-box kurallardan biri seçin **verme**. GUID PS1 dosyası hazırlama sunucusundan GUID ile değiştirin.
4. Bir PowerShell komut isteminde PS1 dosyasını çalıştırın. Bu hazırlama sunucusunda özel eşitleme kuralı oluşturur.
5. Bu, özel kurallarınızı için işlemi yineleyin.

## <a name="how-to-defer-full-synchronization-after-upgrade"></a>Yükseltmeden sonra tam eşitleme erteleme nasıl
Yerinde yükseltme sırasında olabilir (tam alma adımı ve tam eşitleme adımı dahil) belirli eşitleme etkinliklerini yürütülecek gerektiren sunulan değişiklikler. Örneğin, bağlayıcı şema değişiklikleri gerektirir **tam alma** adım ve out-of-box eşitleme kuralı değişiklik gerektiren **tam eşitleme** etkilenen bağlayıcılar yürütülmek üzere adım. Yükseltme sırasında Azure AD Connect eşitleme etkinlikleri gerekli olduğunu belirler ve bunları olarak kaydeder *geçersiz kılmaları*. Aşağıdaki eşitleme döngüsü Eşitleme Zamanlayıcısı'nı bu geçersiz kılma işlemleri seçer ve bunları yürütür. Bir geçersiz kılma başarılı bir şekilde yürütüldükten sonra kaldırılır.

Burada hemen yükseltmeden sonra gerçekleşmesi için bu geçersiz kılmaları istemediğiniz durumlar olabilir. Örneğin, çok sayıda eşitlenmiş nesneleri sahip ve çalışma saatlerinden gerçekleşmesi için eşitleme adımları istersiniz. Bu geçersiz kılmaları kaldırmak için:

1. Yükseltme sırasında **işaretini** seçeneği **Yapılandırma tamamlandıktan sonra eşitleme işlemini başlatmak**. Bu Eşitleme Zamanlayıcısı'nı devre dışı bırakır ve geçersiz kılmalar kaldırılmadan önce eşitleme döngüsü alma yerden otomatik olarak engeller.

   ![DisableFullSyncAfterUpgrade](./media/active-directory-aadconnect-upgrade-previous-version/disablefullsync01.png)

2. Yükseltme tamamlandıktan sonra hangi geçersiz kılmaları eklenmiş olan bulmak için aşağıdaki cmdlet'i çalıştırın:`Get-ADSyncSchedulerConnectorOverride | fl`

   >[!NOTE]
   > Geçersiz kılmalar bağlayıcı özgüdür. Aşağıdaki örnekte, tam alma adımı ve tam eşitleme adımı her iki şirket içi AD Bağlayıcısı ve Azure AD Bağlayıcısı eklenmiştir.

   ![DisableFullSyncAfterUpgrade](./media/active-directory-aadconnect-upgrade-previous-version/disablefullsync02.png)

3. Eklenen varolan geçersiz kılmaları unutmayın.
   
4. Tam içeri aktarma ve rasgele bir bağlayıcı üzerinde tam eşitleme için geçersiz kılmaları kaldırmak için aşağıdaki cmdlet'i çalıştırın:`Set-ADSyncSchedulerConnectorOverride -ConnectorIdentifier <Guid-of-ConnectorIdentifier> -FullImportRequired $false -FullSyncRequired $false`

   Tüm bağlayıcılar üzerinde geçersiz kılmaları kaldırmak için aşağıdaki PowerShell betiğini yürütün:

   ```
   foreach ($connectorOverride in Get-ADSyncSchedulerConnectorOverride)
   {
       Set-ADSyncSchedulerConnectorOverride -ConnectorIdentifier $connectorOverride.ConnectorIdentifier.Guid -FullSyncRequired $false -FullImportRequired $false
   }
   ```

5. Zamanlayıcı sürdürmek için aşağıdaki cmdlet'i çalıştırın:`Set-ADSyncScheduler -SyncCycleEnabled $true`

   >[!IMPORTANT]
   > Erken kolaylık olması gereken eşitleme adımları yürütmek unutmayın. El ile Eşitleme Hizmeti Yöneticisi'ni kullanarak aşağıdaki adımları yürütün veya Set-ADSyncSchedulerConnectorOverride cmdlet'ini kullanarak geçersiz kılmaları geri ekleyin.

Rastgele bir bağlayıcı üzerinde tam içeri aktarma ve tam eşitleme için geçersiz kılmalar eklemek için aşağıdaki cmdlet'i çalıştırın:`Set-ADSyncSchedulerConnectorOverride -ConnectorIdentifier <Guid> -FullImportRequired $true -FullSyncRequired $true`

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md).
