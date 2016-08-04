<properties
   pageTitle="Azure Backup'ın genel önizleme sürümü ile ilgili SSS | Microsoft Azure"
   description="SSS'nin bu sürümü, Azure Backup hizmetinin Genel Önizleme sürümünü destekler. Backup aracısı, yedekleme ve bekletme, kurtarma, güvenlik ve Azure Backup çözümü ile ilgili diğer sık sorulan soruların yanıtları."
   services="backup"
   documentationCenter=""
   authors="markgalioto"
   manager="jwhit"
   editor=""
   keywords="backup solution; backup service"/>

<tags
   ms.service="backup"
   ms.workload="storage-backup-recovery"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.date="03/30/2016"
     ms.author="trinadhk; markgal; jimpark;"/>

# Azure Backup hizmetinin Genel Önizleme sürümü ile ilgili SSS

> [AZURE.SELECTOR]
- [Klasik mod için Backup ile ilgili SSS](backup-azure-backup-faq.md)
- [ARM modu için Backup ile ilgili SSS](backup-azure-backup-ibiza-faq.md)

Bu makalede, Azure Backup hizmetinin Genel Önizleme sürümüne özgü bilgiler sağlanmaktadır. Bu makale, yeni sık sorulan sorular alındığında güncelleştirilir ve [Azure Backup ile ilgili SSS](backup-azure-backup-faq) bölümünü tamamlayıcı niteliktedir. Azure Backup ile ilgili SSS, Azure Backup hizmeti hakkında tam kapsamlı sorular ve yanıtlar sağlamaktadır.  

Azure Backup ile ilgili sorularınızı, bu makalenin veya ilgili bir makalenin Disqus bölümünde sorabilirsiniz. Ayrıca Azure Backup hizmeti ile ilgili sorularınızı [tartışma forumunda](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup) paylaşabilirsiniz.

## Genel Önizleme sürümü neler içerir?
Genel Önizleme sürümü, Azure VM'leri koruma sırasında Kurtarma Hizmetleri kasasını ve ARM desteğini sunar. Kurtarma Hizmetleri kasası, kasaların yeni neslini teşkil eder. Bu, Azure Backup ve Azure Site Recovery (ASR) hizmetleri tarafından kullanılan kasadır. Bunu v.2 kasası olarak düşünebilirsiniz.

## Kurtarma Hizmetleri ve Yedekleme kasaları

**S1. Kurtarma Hizmetleri kasaları v.2 olduğuna göre, Yedekleme kasaları (v.1) hâlâ destekleniyor mu?** <br/>
Y1. Evet, Yedekleme kasaları hâlâ destekleniyor. Klasik portalda Yedekleme kasaları oluşturun. Kurtarma Hizmetleri kasalarını Azure portalda oluşturun.

**S2. Bir Backup kasasının Kurtarma Hizmetleri kasasına geçişini sağlayabilir miyim?** <br/>
Y2. Ne yazık ki hayır, şu an için bir Backup kasasının içeriğinin Kurtarma Hizmetleri kasasına geçişini sağlayamazsınız. Bu işlevi eklemeye yönelik çalışmalarımız devam ediyor ancak işlev Genel Önizleme kapsamında sunulmuyor.

**S3. Kurtarma Hizmetleri kasaları v.1 veya v.2 VM'leri destekliyor mu?** <br/>
 Y3. Kurtarma Hizmetleri kasaları v.1 ve v.2 VM'leri destekler. Klasik portalda oluşturulan bir VM'yi (V.1) veya Azure portalda oluşturulan bir VM'yi (V.2) bir Kurtarma Hizmetleri kasasına yedekleyebilirsiniz.


## Azure VM'ler için ARM desteği

**S1. Azure VM'ler için ARM desteği ile ilgili herhangi bir sınır var mıdır?** <br/>
Y1. ARM için PowerShell cmdlet'leri şu anda kullanılamamaktadır. Bir kaynak grubuna kaynak eklemek için Azure portalı kullanıcı arabirimini kullanmanız gerekir.



<!----HONumber=Jun16_HO2-->


