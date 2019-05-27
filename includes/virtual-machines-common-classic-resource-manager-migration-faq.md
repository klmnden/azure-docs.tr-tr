---
title: include dosyası
description: include dosyası
services: virtual-machines
author: jpconnock
ms.service: virtual-machines
ms.topic: include
ms.date: 05/18/2018
ms.author: jeconnoc
ms.custom: include file
ms.openlocfilehash: 74496cd3d4cd01be326baae870b075eb923983af
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66158244"
---
# <a name="frequently-asked-questions-about-classic-to-azure-resource-manager-migration"></a>Klasik modelden Azure Resource Manager’a geçişle ilgili sık sorulan sorular

## <a name="does-this-migration-plan-affect-any-of-my-existing-services-or-applications-that-run-on-azure-virtual-machines"></a>Bu geçiş planı Azure sanal makinelerde çalışan mevcut hizmetlerimi ya da uygulamaların herhangi birini etkiliyor mu? 

Hayır. VM’ler (Klasik), genel kullanılabilirlikte tam olarak desteklenen hizmetlerdir. Microsoft Azure’da, ayak izinizi genişletmek için bu kaynakları kullanmaya devam edebilirsiniz.

## <a name="what-happens-to-my-vms-if-i-dont-plan-on-migrating-in-the-near-future"></a>Yakın gelecekte geçirmeyi planlamadığım VM'lerime ne olur? 

Mevcut klasik API'leri ve kaynak modelini kullanımdan kaldırmıyoruz. Resource Manager dağıtım modelinde kullanılabilen gelişmiş özellikleri dikkate alarak geçiş kolaylaştırmak istiyoruz. Resource Manager altında IaaS’ın parçası olan [geliştirmelerden bazılarını](../articles/azure-resource-manager/resource-manager-deployment-model.md) incelemenizi öneririz.

## <a name="what-does-this-migration-plan-mean-for-my-existing-tooling"></a>Bu geçiş planı, mevcut araçlarım için ne anlama geliyor? 

Resource Manager dağıtım modeli için araçlarınızı güncelleştirmek, geçiş planlarınızda hesaba katmanız gereken en önemli değişikliklerden biridir.

## <a name="how-long-will-the-management-plane-downtime-be"></a>Yönetim düzeyi kapalı kalma süresi ne kadar olacak? 

Bu, geçirilmekte olan kaynakların sayısına bağlıdır. Küçük dağıtımlar için (birkaç düzine VM) tüm geçişin bir saatten az sürmesi gerekir. Büyük ölçekli dağıtımlar (yüzlerce VM) için geçiş birkaç saat sürebilir.

## <a name="can-i-roll-back-after-my-migrating-resources-are-committed-in-resource-manager"></a>Geçiş kaynaklarım Kaynak Yöneticisi'ne işlendikten sonra işlemi geri alabilir miyim? 

Kaynaklar hazır durumda olduğu sürece geçişinizi iptal edebilirsiniz. Kaynakların işleme süreci başarıyla tamamlandıktan sonra geri alma desteklenmez.

## <a name="can-i-roll-back-my-migration-if-the-commit-operation-fails"></a>İşleme süreci başarısız olursa geçişi geri alabilir miyim? 

İşleme süreci başarısız olursa geçişi geri alamazsınız. Kaydetme işlemi de dahil olmak üzere tüm geçiş işlemleri birden fazla kez denenebilir. Bu nedenle kısa bir süre sonra işlemi yeniden denemenizi öneririz. Hatayla karşılaşmaya devam ederseniz bir destek bileti oluşturun veya [VM Forumumuzda](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesforWindows) ClassicIaaSMigration etiketiyle bir forum gönderisi oluşturun.

## <a name="do-i-have-to-buy-another-express-route-circuit-if-i-have-to-use-iaas-under-resource-manager"></a>Kaynak Yöneticisi altında IaaS kullanmam gerekiyorsa başka bir hızlı yol devresi satın almam gerekir mi? 

Hayır. Yakın zamanda [ExpressRoute devrelerini klasikten Resource Manager dağıtım modeline taşıma](../articles/expressroute/expressroute-move.md) özelliğini etkinleştirdik. Bir ExpressRoute devreniz varsa yeni bir tane satın almak zorunda değilsiniz.

## <a name="what-if-i-had-configured-role-based-access-control-policies-for-my-classic-iaas-resources"></a>Klasik Iaas Kaynaklarım için Rol Tabanlı Erişim Denetimi yapılandırdıysam ne olur? 

Geçiş sırasında kaynaklar klasikten Resource Manager’a dönüşür. Bu nedenle geçişten sonra gerçekleşmesi gereken RBAC İlkesi güncelleştirmelerini planlamanızı öneririz.

## <a name="i-backed-up-my-classic-vms-in-a-vault-can-i-migrate-my-vms-from-classic-mode-to-resource-manager-mode-and-protect-them-in-a-recovery-services-vault"></a>Klasik vm'lerimi bir kasada desteklenir. VM’lerimi klasik moddan Resource Manager moduna geçirip bunları bir Kurtarma Hizmetleri kasasında koruyabilir miyim?

<a name="vault">Zaman</a> VM'yi Klasikten Resource Manager moduna taşıma, geçişten önce alınan yedeklemeler değil yeni geçirilen Resource Manager VM geçiş. Ancak, Klasik VM yedekleri tutmak istiyorsanız, geçiş işleminden önce bu adımları izleyin. 

1. Kurtarma Hizmetleri Kasası'nda Git **korunan öğeler** sekme ve VM'yi seçin. 
2. Korumayı durdurun tıklayın. *İlişkili yedekleme verilerini sil* seçeneğini **işaretlenmemiş** olarak bırakın.

> [!NOTE]
> Verileri Tut kadar yedekleme örnek maliyeti ücretlendirilirsiniz. Yedek kopyaları, bekletme aralığına göre ayıklanır. Bununla birlikte, son yedek kopyasını açıkça yedekleme verilerini silene kadar her zaman tutulur. Bekletme aralığı bittikten sonra bekletme aralığı kasadaki korumalı öğe "Yedekleme verilerini sil" Tetikle ve sanal makine kontrol etmek için önerilir. 
>
>

Sanal makineyi Resource Manager moduna geçirmek için 

1. Yedekleme/anlık görüntü uzantısını VM'den silin.
2. Sanal makineyi, klasik moddan Resource Manager moduna geçirin. Sanal makineye karşılık gelen depolama ve ağ bilgilerinin de Resource Manager moduna geçirildiğinden emin olun.

Ayrıca, geçirilen sanal makineyi yedeklemek istiyorsanız sanal makine yönetim dikey penceresine gidin [yedeklemeyi etkinleştirme](../articles/backup/quick-backup-vm-portal.md#enable-backup-on-a-vm).

## <a name="can-i-validate-my-subscription-or-resources-to-see-if-theyre-capable-of-migration"></a>Geçişe uygun olup olmadıklarını görmek için aboneliğimi ya da kaynaklarımı doğrulayabilir miyim? 

Evet. Platform destekli geçiş seçeneğinde, geçiş için hazırlanmanın ilk adımı kaynakları geçişe uygun olup olmadığını doğrulamaktır. Doğrulama işlemi başarısız olursa, geçiş işleminin neden tamamlanamadığına dair tüm nedenlerle ilgili iletiler alırsınız.

## <a name="what-happens-if-i-run-into-a-quota-error-while-preparing-the-iaas-resources-for-migration"></a>Geçiş için IaaS kaynaklarını hazırlarken bir kota hatasıyla karşılaşırsam ne olur? 

Geçiş işlemini durdurmanızı ve VM’leri geçirdiğiniz bölgede kotaları artırmak için bir destek isteği göndermenizi öneririz. Kota isteği onaylandıktan sonra geçiş adımlarını yeniden başlatabilirsiniz.

## <a name="how-do-i-report-an-issue"></a>Bir sorunu nasıl bildirebilirim? 

Geçiş hakkında sorularınızı ve sorunlarınızı ClassicIaaSMigration anahtar sözcüğü ile [VM forumumuza](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesforWindows) gönderin. Tüm sorularınızı bu foruma göndermenizi öneririz. Destek anlaşmanız varsa, destek bileti de açabilirsiniz.

## <a name="what-if-i-dont-like-the-names-of-the-resources-that-the-platform-chose-during-migration"></a>Geçiş sırasında platformun seçtiği kaynak adlarını beğenmezsem ne yapabilirim? 

Klasik dağıtım modelinde adlarını özellikle belirttiğiniz tüm kaynaklar geçiş sırasında korunur. Bazı durumlarda yeni kaynaklar oluşturulur. Örneğin tüm VM’ler için ağ arabirimleri oluşturulur. Geçiş sırasında oluşturulan bu yeni kaynakların adlarını denetleme olanağını şu anda desteklemiyoruz. Bu özellik için oylarınızı [Azure geri bildirim forumuna](http://feedback.azure.com) girebilirsiniz.

## <a name="can-i-migrate-expressroute-circuits-used-across-subscriptions-with-authorization-links"></a>Yetkilendirme bağlantıları olan aboneliklerde kullanılan ExpressRoute devrelerini geçirebilir miyim? 

Çapraz abonelik yetkilendirme bağlantılar kullanan ExpressRoute devreleri kapalı kalma süresi olmadan otomatik olarak geçirilemez. Bunları elle nasıl geçirebileceğiniz hakkında yönergelerimiz vardır. Adımlar ve daha fazla bilgi için bkz. [ExpressRoute devrelerini ve ilgili sanal ağları klasikten Resource Manager dağıtım modeline geçirme](../articles/expressroute/expressroute-migration-classic-resource-manager.md).

## <a name="i-got-the-message-vm-is-reporting-the-overall-agent-status-as-not-ready-hence-the-vm-cannot-be-migrated-ensure-that-the-vm-agent-is-reporting-overall-agent-status-as-ready-or-vm-contains-extension-whose-status-is-not-being-reported-from-the-vm-hence-this-vm-cannot-be-migrated"></a>İletiyi aldım *"VM raporlama genel aracı durumunu hazır olarak değil. Bu nedenle VM geçirilemiyor. VM aracısının genel aracı durumunu hazır olarak bildirdiğinden emin olun"* veya *"VM uzantısı durumu bildirilmeyen VM'den içeriyor. Bu nedenle, bu VM geçirilemez."*

Bu ileti, VM’nin giden İnternet bağlantısı olmadığında alınır. VM aracısı, aracı durumunu güncelleştirmek için, giden bağlantı üzerinden her beş dakikada bir Azure depolama hesabına erişir.
