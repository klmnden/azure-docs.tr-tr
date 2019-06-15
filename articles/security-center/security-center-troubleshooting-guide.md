---
title: Azure Güvenlik Merkezi Sorun Giderme Kılavuzu | Microsoft Belgeleri
description: Bu belge Azure Güvenlik Merkezi’ndeki sorunları gidermenize yardımcı olur.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 44462de6-2cc5-4672-b1d3-dbb4749a28cd
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/20/2019
ms.author: rkarlin
ms.openlocfilehash: 63275db36bdb64985625c3789d558bd09e2d47bc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60912068"
---
# <a name="azure-security-center-troubleshooting-guide"></a>Azure Güvenlik Merkezi Sorun Giderme Kılavuzu
Bu kılavuz, kuruluşları Azure Güvenlik Merkezi'ni kullanmayı planlayan ve Güvenlik Merkezi ile ilgili sorunları gidermeye ihtiyaç duyan bilgi teknolojisi (BT) uzmanları, bilgi güvenlik analizi uzmanları ve bulut yöneticileri içindir.

>[!NOTE]
>Haziran 2017'nin ilk günlerinden itibaren Güvenlik Merkezi, veri toplamak ve depolamak için Microsoft Monitoring Agent'ı kullanmaktadır. Daha fazla bilgi edinmek için [Azure Güvenlik Merkezi Platform Geçişi](security-center-platform-migration.md) makalesine bakın. Bu makaledeki bilgiler, Microsoft Monitoring Agent'a geçiş sonrasındaki Güvenlik Merkezi işlevselliğine yöneliktir.
>

## <a name="troubleshooting-guide"></a>Sorun giderme kılavuzu
Bu kılavuzda Güvenlik Merkezi ile ilgili sorunların nasıl giderildiği açıklanmaktadır. Güvenlik Merkezi’nde yapılan sorun giderme işlemlerinin birçoğu, öncelikle başarısız bileşenlere ilişkin [Denetim Günlüğü](../azure-monitor/platform/activity-logs-overview.md) kayıtlarına bakılarak gerçekleştirilir. Denetim günlükleri ile aşağıdakileri belirleyebilirsiniz:

* Hangi işlemlerin gerçekleştirildiği
* İşlemi kimin başlattığı
* İşlemin ne zaman oluştuğu
* İşlemin durumu
* İşlemi araştırmanıza yardımcı olabilecek diğer özelliklerin değerleri

Denetim günlüğü, kaynaklarınız üzerinde gerçekleştirilen tüm yazma işlemlerini (PUT, POST, DELETE) içerir, ancak okuma işlemlerini (GET) içermez.

## <a name="microsoft-monitoring-agent"></a>Microsoft Monitoring Agent
Güvenlik Merkezi, Microsoft Monitoring Agent kullanır: Azure sanal makinelerinizden güvenlik verilerini toplamak için Azure İzleyici hizmeti tarafından kullanılan aracının aynısı budur. Veri toplama etkinleştirilip aracı hedef makineye doğru şekilde yüklendikten sonra, aşağıdaki işlem yürütülmelidir:

* HealthService.exe

Hizmet yönetimi konsolunu (services.msc) açarsanız, aynı zamanda Microsoft Monitoring Agent hizmetinin aşağıda gösterildiği gibi çalıştığını görürsünüz:

![Hizmetler](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig5.png)

Sahip olduğunuz aracı sürümünü görmek için **Görev Yöneticisi**’ni açın, **İşlemler** sekmesinde **Microsoft Monitoring Agent Hizmeti**’ni bulun, sağ tıklayın ve **Özellikler**’e tıklayın. **Ayrıntılar** sekmesinde aşağıda gösterildiği gibi dosya sürümüne bakın:

![Dosya](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig6.png)


## <a name="microsoft-monitoring-agent-installation-scenarios"></a>Microsoft Monitoring Agent yükleme senaryoları
Microsoft Monitoring Agent’ı bilgisayarınıza yüklerken farklı sonuçlar üretebilen iki yükleme senaryosu vardır. Desteklenen senaryolar şunlardır:

* **Aracı Güvenlik Merkezi tarafından otomatik olarak yüklenir**: Bu senaryoda hem Güvenlik Merkezi hem de Günlük aramasında uyarıları görüntüleyebilirsiniz. Kaynağın ait olduğu aboneliğe ait güvenlik ilkesinde yapılandırılmış e-posta adresine e-posta bildirimleri alırsınız.
.
* **Aracı, Azure’da bulunan bir VM’ye el ile yüklenir**: Bu senaryoda, Şubat 2017’den önce indirilip yüklenmiş aracılar kullanıyorsanız, uyarıları yalnızca çalışma alanının ait olduğu abonelikte filtrelemeniz durumunda Güvenlik Merkezi portalında görüntüleyebilirsiniz. Kaynağın ait olduğu abonelikte filtrelemeniz halinde, herhangi bir uyarı göremezsiniz. Çalışma alanının ait olduğu aboneliğe ait güvenlik ilkesinde yapılandırılmış e-posta adresine e-posta bildirimleri alırsınız.

>[!NOTE]
> İkinci senaryoda açıklanan davranışı önlemek için aracının en son sürümünü indirdiğinizden emin olun.
>

## Monitoring Agent durumu sorunları <a name="mon-agent"></a>
**İzleme durumu**, Güvenlik Merkezi’nin otomatik sağlama için başlatılmış VM’leri ve bilgisayarları neden başarıyla izleyemediğini tanımlar. Aşağıdaki tabloda **İzleme durumu** değerleri, açıklamaları ve çözüm adımları gösterilmektedir.

| İzleme durumu | Açıklama | Çözüm adımları |
|---|---|---|
| Bekleyen aracı yüklemesi | Microsoft Monitoring Agent yüklemesi hala devam ediyor.  Yükleme birkaç saat sürebilir. | Otomatik yükleme işlemi tamamlanana kadar bekleyin. |
| Güç durumu kapalı | VM durduruldu.  Microsoft Monitoring Agent yalnızca çalışır durumdaki bir VM’ye yüklenebilir. | VM’yi yeniden başlatın. |
| Eksik veya geçersiz Azure VM aracısı | Microsoft Monitoring Agent henüz yüklü değil.  Güvenlik Merkezi’nin uzantıyı yüklemesi için geçerli bir Azure VM aracısı gereklidir. | Azure VM aracısını VM’ye yükleyin, yeniden yükleyin veya yükseltin. |
| VM durumu yükleme için hazır değil  | VM yükleme için hazır olmadığından Microsoft Monitoring Agent henüz yüklü değil. VM aracısı veya VM sağlama ile ilgili bir sorun nedeniyle VM yükleme için hazır değil. | Sanal makinenizin durumunu denetleyin. Portalda **Sanal Makineler**’e geri dönün ve durum bilgisi için VM’yi seçin. |
|Yükleme başarısız oldu - genel hata | Microsoft Monitoring Agent yüklendi, ancak bir hata nedeniyle başarısız oldu. | [Uzantıyı el ile yükleyin](../azure-monitor/learn/quick-collect-azurevm.md#enable-the-log-analytics-vm-extension) veya Güvenlik Merkezi’nin yüklemeyi yeniden denemesi için uzantıyı kaldırın. |
| Yükleme başarısız oldu - yerel aracı zaten yüklü | Microsoft Monitoring Agent yüklemesi başarısız oldu. Güvenlik Merkezi, sanal makinede zaten yüklü bir yerel Aracı (Log Analytics veya System Center Operations Manager) belirledi. VM’nin iki ayrı çalışma alanına bildirimde bulunduğu çok girişli bir yapılandırmayı önlemek için, Microsoft Monitoring Agent yüklemesi durduruldu. | Çözümlemek için iki yol vardır: [Uzantıyı el ile yükleyin](../azure-monitor/learn/quick-collect-azurevm.md#enable-the-log-analytics-vm-extension) ve istediğiniz çalışma alanına bağlayın. Veya, istediğiniz çalışma alanını varsayılan çalışma alanı olarak ayarlayın ve aracının otomatik sağlamasını etkinleştirin.  Bkz. [otomatik sağlamayı etkinleştirme](security-center-enable-data-collection.md). |
| Aracı çalışma alanına bağlanamıyor | Microsoft Monitoring Agent yüklendi, ancak ağ bağlantısı nedeniyle başarısız oldu.  İnternet erişiminin olduğundan veya aracı için geçerli bir HTTP ara sunucusunun yapılandırıldığından emin olun. | Monitoring Agent ağ gereksinimleri bakın. |
| Aracı eksik veya bilinmeyen çalışma alanına bağlandı | Güvenlik Merkezi, VM’de yüklü Microsoft Monitoring Agent’ın erişimi olmayan bir çalışma alanına bağlandığını belirledi. | Bu durum iki koşulda meydana gelebilir. Çalışma alanı silindi ve artık mevcut değil. Aracıyı doğru çalışma alanıyla yeniden yükleyin veya aracıyı kaldırıp Güvenlik Merkezi’nin otomatik sağlama yüklemesini tamamlamasını bekleyin. İkinci koşul ise çalışma alanının, Güvenlik Merkezi’nin izinli olmadığı bir aboneliğe ait olmasıdır. Güvenlik Merkezi, aboneliklerin Microsoft Güvenlik Kaynak Sağlayıcısı'na erişim izni vermesini gerektirir. Etkinleştirmek için aboneliği Microsoft Güvenlik Kaynak Sağlayıcısı’na kaydedin. Bu işlem API, PowerShell, portal veya yalnızca Güvenlik Merkezi'ndeki **Genel Bakış** Panosunda abonelik filtrelenerek yapılabilir. Daha fazla bilgi için bkz. [Kaynak sağlayıcıları ve türleri](../azure-resource-manager/resource-manager-supported-services.md#azure-portal). |
| Aracı yanıt vermiyor veya kimliği eksik | Güvenlik Merkezi, aracı yüklü olsa bile VM’den taranan güvenlik verilerini alamıyor. | Aracı, sinyal de dahil olmak üzere herhangi bir veri bildirmiyor. Aracı zarar görmüş olabilir veya trafiği engelleyen bir durum vardır. Ya da aracı verileri bildiriyordur, ancak bir Azure kaynak kimliği eksik olduğu için verilerin Azure VM ile eşleştirilmesi mümkün değildir. Linux'ta sorun gidermek için bkz: [Linux için Log Analytics aracısı için sorun giderme kılavuzu](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md#im-not-seeing-any-linux-data-in-the-oms-portal). Windows’da sorun gidermek için bkz. [Windows Sanal Makineleri’nde Sorun Giderme](https://github.com/MicrosoftDocs/azure-docs/blob/8c53ac4371d482eda3d85819a4fb8dac09996a89/articles/log-analytics/log-analytics-azure-vm-extension.md#troubleshooting-windows-virtual-machines). |
| Aracı yüklü değil | Veri toplama devre dışıdır. | Güvenlik ilkesinde veri toplamayı etkinleştirin veya Microsoft Monitoring Agent’ı el ile yükleyin. |


## Monitoring Agent ağ gereksinimleriyle ilgili sorunları giderme <a name="mon-network-req"></a>
Aracıların Güvenlik Merkezi’ne bağlanması ve kaydolması için, bağlantı noktası numaraları ve etki alanı URL’leri dahil olmak üzere ağ kaynaklarına erişebilmesi gerekir.

- Proxy sunucuları için, aracı ayarlarında uygun proxy sunucusu kaynaklarının yapılandırıldığından emin olmanız gerekir. [Proxy ayarlarını değiştirme](https://docs.microsoft.com/azure/log-analytics/log-analytics-windows-agents) hakkında daha fazla bilgi için bu makaleyi okuyun.
- İnternete erişimi kısıtlayan güvenlik duvarları için, güvenlik duvarınızı Log Analytics erişimine izin verecek şekilde yapılandırmanız gerekir. Aracı ayarlarında bir işlem yapılması gerekmez.

Aşağıdaki tabloda iletişim için gereken kaynaklar gösterilmektedir.

| Aracı Kaynağı | Bağlantı Noktaları | HTTPS denetlemesini atlama |
|---|---|---|
| *.ods.opinsights.azure.com | 443 | Evet |
| *.oms.opinsights.azure.com | 443 | Evet |
| *.blob.core.windows.net | 443 | Yes |
| *.azure-automation.net | 443 | Evet |

Aracıyla ekleme sorunları yaşarsanız, [Operations Management Suite ekleme sorunlarını giderme](https://support.microsoft.com/en-us/help/3126513/how-to-troubleshoot-operations-management-suite-onboarding-issues) makalesini okuduğunuzdan emin olun.


## <a name="troubleshooting-endpoint-protection-not-working-properly"></a>Uç nokta korumasıyla ilgili sorunları giderme işlemi düzgün çalışmıyor

Konuk aracı, [Microsoft Kötü Amaçlı Yazılımdan Koruma](../security/azure-security-antimalware.md) uzantısının gerçekleştirdiği tüm işlemlerin üst işlemidir. Konuk aracı işleminin başarısız olması durumunda konuk aracının alt işlemi olarak çalışan Microsoft Kötü Amaçlı Yazılımdan Koruma da başarısız olabilir.  Bu gibi senaryolarda aşağıdaki seçenekleri doğrulamanız önerilir:

- Hedef sanal makine özel bir görüntü mü ve sanal makineyi oluşturan kişi konuk aracısını hiç yüklememiş mi?
- Hedef bir Windows sanal makinesi değil de Linux sanal makinesiyse, kötü amaçlı yazılımdan koruma uzantısının Windows sürümünün Linux sanal makinesine yüklenmesi başarısız olur. Linux konuk aracısının işletim sistemi sürümü ve gerekli paketler açısından belirli gereksinimleri vardır ve bu gereksinimler karşılanmazsa sanal makine aracısı burada da çalışmaz.
- Sanal makine konuk aracısının eski bir sürümüyle mi oluşturulmuş? Bu durumda, bazı eski aracıların kendini otomatik olarak daha yeni bir sürüme güncelleştiremediğini ve bu soruna yol açabileceğini unutmamanız gerekir. Kendi görüntülerinizi oluşturuyorsanız konuk aracısının her zaman en son sürümünü kullanın.
- Bazı üçüncü taraf yönetim yazılımları konuk aracısını devre dışı bırakabilir ya da aracının belirli dosya konumlarına erişmesini engelleyebilir. Sanal makinenizde üçüncü taraf yazılım yüklüyse aracının dışlama listesinde olduğundan emin olun.
- Belirli güvenlik duvarı ayarları ya da Ağ Güvenlik Grubu (NSG), konuk aracısı için giden ve gelen trafiği engelleyebilir.
- Belirli bir Erişim Denetimi Listesi (ACL) disk erişimini engelleyebilir.
- Disk alanının yetersiz olması konuk aracısının düzgün çalışmasını engelleyebilir.

Microsoft Kötü Amaçlı Yazılımdan Koruma Kullanıcı Arabirimi varsayılan olarak devre dışıdır; gerektiğinde bu arabirimi nasıl etkinleştirebileceğiniz hakkında daha fazla bilgi edinmek için [Azure Resource Manager Sanal Makinelerinde Dağıtımdan Sonra Microsoft Kötü Amaçlı Yazılımdan Koruma Kullanıcı Arabirimi’ni Etkinleştirme](https://blogs.msdn.microsoft.com/azuresecurity/2016/03/09/enabling-microsoft-antimalware-user-interface-post-deployment/) konusunu okuyun.

## <a name="troubleshooting-problems-loading-the-dashboard"></a>Pano yükleme sorunlarını giderme

Güvenlik Merkezi panosunu yüklemeyle ilgili sorun yaşıyorsanız, aboneliği Güvenlik Merkezi’ne kaydeden kullanıcı (Güvenlik Merkezi’ni bu abonelikle ilk kez açan kullanıcı) ile veri toplamayı etkinleştirmek isteyen kullanıcının abonelikte *Sahip* veya *Katkıda Bulunan* rolüne sahip olduğundan emin olun. O andan itibaren, abonelikte *Okuyucu* rolüne sahip kullanıcılar da pano/uyarılar/öneri/ilke sayfasını görebilir.

## <a name="contacting-microsoft-support"></a>Microsoft Destek ile iletişim kurma
Bazı sorunlar bu makalede verilen yönergeler kullanılarak tanımlanabilirken, bazılarını Güvenlik Merkezi genel [Forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSecurityCenter)’nda da bulabilirsiniz. Ancak, daha fazla sorun giderme bilgisi gerekirse, **Azure portalında** aşağıda gösterildiği gibi yeni bir destek isteği oluşturabilirsiniz:

![Microsoft Destek](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig2.png)


## <a name="see-also"></a>Ayrıca bkz.
Bu belgede, Azure Güvenlik Merkezi'nde güvenlik ilkelerinin nasıl yapılandırılacağını öğrendiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi Planlama ve İşlemler Kılavuzu](security-center-planning-and-operations-guide.md) - Azure Güvenlik Merkezi'ni benimsemek için tasarım ile ilgili dikkat edilmesi gerekenleri planlama ve anlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve ele alma](security-center-managing-and-responding-alerts.md) - Güvenlik uyarılarını yönetme ve ele alma hakkında bilgi edinin
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmet kullanımı ile ilgili sık sorulan soruları burada bulabilirsiniz
* [Azure Güvenlik Blogu](https://blogs.msdn.com/b/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz
