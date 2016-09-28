<properties
   pageTitle="Kurtarma Hizmetleri kasası hakkında SSS | Microsoft Azure"
   description="SSS'nin bu sürümü, Azure Backup hizmetinin Genel Önizleme sürümünü destekler. Backup aracısı, yedekleme ve bekletme, kurtarma, güvenlik ve Azure Backup çözümü ile ilgili diğer sık sorulan soruların yanıtları."
   services="backup"
   documentationCenter=""
   authors="markgalioto"
   manager="jwhit"
   editor=""
   keywords="yedekleme çözümü; backup hizmeti"/>

<tags
   ms.service="backup"
   ms.workload="storage-backup-recovery"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.date="08/21/2016"
     ms.author="trinadhk; markgal; jimpark;"/>


# Kurtarma Hizmetleri kasası - SSS

> [AZURE.SELECTOR]
- [Klasik mod için Backup ile ilgili SSS](backup-azure-backup-faq.md)
- [Resource Manager modu için Backup ile ilgili SSS](backup-azure-backup-ibiza-faq.md)

Bu makalede, Kurtarma Hizmetleri kasasına özgü bilgiler sağlanmaktadır ve makale, [Azure Backup hakkında SSS](backup-azure-backup-faq) makalesi için tamamlayıcı niteliktedir. Azure Backup ile ilgili SSS, Azure Backup hizmeti hakkında tam kapsamlı sorular ve yanıtlar sağlamaktadır.  

Azure Backup ile ilgili sorularınızı, bu makalenin veya ilgili bir makalenin Disqus bölümünde sorabilirsiniz. Ayrıca Azure Backup hizmeti ile ilgili sorularınızı [tartışma forumunda](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup) paylaşabilirsiniz.

## Kurtarma Hizmetleri kasaları Resource Manager tabanlıdır. Backup kasaları (klasik mod) hala destekleniyor mu? <br/>
Evet, Yedekleme kasaları hâlâ destekleniyor. [Klasik portalda](https://manage.windowsazure.com) Backup kasaları oluşturun. [Azure portalında](https://portal.azure.com) Kurtarma Hizmetleri kasaları oluşturun. Ancak gelecekteki tüm geliştirmeler yalnızca Kurtarma Hizmetleri kasasında kullanılabileceği için, kurtarma hizmetleri kasası oluşturmanızı öneririz. 

## Bir Backup kasasının Kurtarma Hizmetleri kasasına geçişini sağlayabilir miyim? <br/>
Ne yazık ki hayır, şu an için bir Backup kasasının içeriğinin Kurtarma Hizmetleri kasasına geçişini sağlayamazsınız. Bu işlevi eklemeye yönelik çalışmalarımız devam ediyor ancak işlev Genel Önizleme kapsamında sunulmuyor.

## Kurtarma Hizmetleri kasaları, klasik VM’leri mi Resource Manager tabanlı VM’leri mi destekler? <br/>
Kurtarma Hizmetleri kasaları iki modeli de destekler.  Klasik portalda oluşturulan bir VM’yi (klasik mod VM’leri) veya Azure portalında oluşturulan bir VM’yi (Resource Manager tabanlı), Kurtarma Hizmetleri kasasına yedekleyebilirsiniz.

## Klasik VM’lerimi yedekleme kasasında yedekledim. Şimdi, VM’lerimi klasik moddan Resource Manager moduna geçirmek istiyorum.  Bunları nasıl kurtarma hizmetleri kasasında yedekleyebilirim?
Yedekleme kasasındaki klasik VM yedekleri, VM’leri klasik moddan Resource Manager moduna geçirdiğinizde kurtarma hizmetleri kasasında otomatik olarak yedeklenmez. VM yedeklerinin geçirilmesi için lütfen bu adımları izleyin:

1. Yedekleme kasasıda, **Korunan Öğeler** sekmesine gidin ve VM’yi seçin. [Korumayı Durdur](backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines)’a tıklayın. *İlişkili yedekleme verilerini sil* seçeneğini **işaretlenmemiş** olarak bırakın. 
2. Sanal makineyi, klasik moddan Resource Manager moduna geçirin. Sanal makine için karşılık gelen depolama ve ağın da Resource Manager moduna geçirildiğinden emin olun. 
3. Bir kurtarma hizmetleri kasası oluşturun ve kasa panosunun üstündeki **Backup** işlemini kullanarak, geçirilen sanal makinede yedeklemeyi yapılandırın. [Kurtarma hizmetleri kasasında yedeklemeyi etkinleştirme](backup-azure-vms-first-look-arm.md) hakkında daha fazla bilgi edinin



<!--HONumber=Sep16_HO3-->


