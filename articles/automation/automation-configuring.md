<properties
   pageTitle="Azure Automation’u yapılandırma"
   description="İlk kullanım için Azure Automation’u yapılandırmaya yönelik atmanız gereken adımları açıklar."
   services="automation"
   documentationCenter=""
   authors="MGoedtel"
   manager="stevenka"
   editor="tysonn" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/23/2016"
   ms.author="magoedte;bwren" />

# Azure Automation’u yapılandırma

Bu makalede Azure Automation’u ilk kez kullanmaya başlamak için yapmanız gereken eylemler açıklanmıştır.

## Automation hesapları

Azure Automation’u ilk kez başlattığınızda, en az bir Automation hesabı oluşturmanız gerekir. Automation hesapları Automation kaynaklarınızı (runbook'lar, varlıklar, yapılandırmalar) diğer Automation hesaplarında yer alan Automation kaynaklarından yalıtmanızı sağlar. Automation kaynaklarını ayrı mantıksal ortamlara ayırmak için Automation hesaplarını kullanabilirsiniz. Örneğin, geliştirme için bir ve üretim için başka bir hesap kullanabilirsiniz.

Her Automation hesabı için Automation kaynakları tek bir Azure bölgesiyle ilişkilendirilir, ancak Automation hesapları tüm bölgelerdeki Azure hizmetlerini yönetebilir. Farklı bölgelerde Automation hesapları oluşturmanın temel nedeni, veri ve kaynakların belirli bir bölgede yalıtılmasını gerektiren ilkelere sahip olmanız olabilir.

>[AZURE.NOTE] Azure Portal ile oluşturulan Automation hesaplarına ve bu hesaplardaki kaynaklara Klasik Azure portalından erişilemez. Bu hesapları veya kaynaklarını Windows PowerShell ile yönetmek istiyorsanız, Azure Resource Manager modüllerini kullanmanız gerekir. 
>
>Klasik Azure portalıyla oluşturulan Automation hesapları portal ya da cmdlet’ler grubu tarafından yönetilebilir. Hesap oluşturulduktan sonra, nasıl oluşturduğunuz veya hesaptaki kaynakları nasıl yönettiğiniz fark etmez. Klasik Azure portalını kullanmaya devam etmeyi planlıyorsanız, Automation hesaplarını oluşturmak için Azure Portal yerine bunu kullanmalısınız.


Azure hesabınızda, süresi geçen ödeme gibi bir sorun oluşması durumunda Automation hesabınız askıya alınabilir. Bu durumda hesaba erişemezsiniz, çalışan tüm işler askıya alınır ve tüm zamanlamalar devre dışı bırakılır. Hesabı görüntüleyebilirsiniz, ancak hesaptaki kaynakları göremezsiniz. Sorunu düzelttiğinizde ve Automation hesabı etkinleştirildiğinde, zamanlamalarınızı yeniden etkinleştirmeniz ve askıya alınmış runbook’ları yeniden başlatmanız gerekir.


## Azure kaynakları için kimlik doğrulaması yapılandırma

[Azure cmdlet'lerini](http://msdn.microsoft.com/library/azure/jj554330.aspx) kullanarak Azure kaynaklarına eriştiğinizde, Azure aboneliğiniz için kimlik doğrulaması sağlamanız gerekir. Bu işlem Azure Automation’da Azure Active Directory'de bulunan, aboneliğiniz için yönetici olarak yapılandırdığınız bir kurumsal hesap ile gerçekleştirilir. Böylece bu kullanıcı hesabı için [kimlik bilgisi](http://msdn.microsoft.com/library/dn940015.aspx) oluşturabilir ve bunu runbook'unuzda [Add-AzureAccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) ile birlikte kullanabilirsiniz.

>[AZURE.NOTE] Önceden LiveID’ler olarak bilinen Microsoft hesapları Azure Automation ile kullanılamaz.

## Bir Azure aboneliğini yönetmek için yeni bir Azure Active Directory kullanıcısı oluşturmalısınız.

1. Klasik Azure portalında, yönetmek istediğiniz Azure aboneliği için hizmet yöneticisi olarak oturum açın.
2. **Active Directory**’yi seçin
3. Azure aboneliğiniz ile ilişkili olan dizin adını seçin. Gerekirse, bu ilişkiyi **Ayarlar > Abonelikler > Dizini Düzenle**’den değiştirebilirsiniz.
4. [Yeni bir Active Directory kullanıcısı oluşturun](http://msdn.microsoft.com/library/azure/hh967632.aspx).  **Kullanıcı türü** için **kuruluşunuzdaki yeni kullanıcı**’yı seçin ve **Multi-Factor Authentication’ı etkinleştirmeyin**.
5. Kullanıcının tam adını ve geçici parolasını unutmayın.
7. **Ayarlar > Yöneticiler > Ekle**’yi seçin.
8. Oluşturduğunuz kullanıcının tam kullanıcı adını yazın.
9. Kullanıcının yönetmesini istediğiniz aboneliği seçin.
10. Azure oturumunu kapatın ve yeni oluşturduğunuz hesapla oturum açın. Kullanıcı parolasını değiştirmeniz istenir.
11. Oluşturduğunuz kullanıcı hesabı için yeni bir [Azure Automation Kimlik Bilgisi varlığı](automation-credentials.md) oluşturun. **Kimlik Bilgisi Türü**, **Windows PowerShell Kimlik Bilgisi** olmalıdır.

## Automation hesabı oluşturma

Automation hesabı, Azure Automation kaynaklarınız için bir kapsayıcıdır. Ortamlarınızı ayırmak veya iş akışlarınızı daha iyi şekilde düzenlemeniz için bir yöntem sunar. Zaten bir Automation hesabı oluşturduysanız, bu adımı atlayabilirsiniz.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.

2. **Yeni** > **Yönetim** > **Automation Hesabı**’na tıklayın.

3. **Automation Hesabı Ekle** dikey penceresinde, Automation hesabı bilgilerinizi yapılandırın. 

>[AZURE.NOTE] Azure Portal ile bir Automation hesabı oluşturulduğunda, hesap ve bununla ilişkili tüm kaynaklar Klasik Azure Portalı’na geri getirilmez. 

Yapılandırılacak parametrelerin listesi aşağıdadır:

|Parametre            |Açıklama |
|:---|:---|
| Adı | Automation hesabınızın adı; benzersiz bir değer olmalıdır. |
| Kaynak Grubu | Kaynak grupları, ilgili Azure kaynaklarını görmeyi ve yönetmeyi basitleştirir. Azure Portal’da, mevcut bir kaynak grubunu seçebilir veya Automation hesabınız için yeni bir tane oluşturabilirsiniz. Bununla birlikte, Klasik Azure Portalı’nda tüm Automation Hesapları varsayılan kaynak grubuna yerleştirilir. |
| Abonelik | Kullanılabilir abonelikler listesinden bir abonelik seçin. |
| Bölge | Bölge, hesaptaki Automation kaynaklarının nerede depolanacağını belirtir. Listeden herhangi bir bölgeyi seçebilirsiniz, bu hesabınızın işlevselliğini etkilemez, ancak hesap bölgeniz diğer Azure kaynaklarınızı depoladığınız yere yakın olursa runbook’larınız daha hızlı yürütülebilir. |
| Hesap Seçenekleri | Bu seçenek, yeni Automation Hesabınızda hangi kaynakların oluşturulacağını seçmenize olanak sağlar; **Evet** seçeneğini seçmek bir öğretici runbook oluşturur. |

![Hesap Oluşturma](media/automation-configuration/automation-01-create-automation-account.png)

>[AZURE.NOTE] Klasik Azure Portalı kullanılarak oluşturulan Automation hesabı Azure Portal kullanılarak [farklı bir kaynak grubuna taşındığında](../resource-group-move-resources.md), Automation hesabı artık Klasik Azure portalında kullanılamaz.



## Bir runbook'ta kimlik bilgisi kullanma

[Get-AutomationPSCredential](http://msdn.microsoft.com/library/dn940015.aspx) etkinliğini kullanarak bir runbook’taki kimlik bilgisini getirebilir ve ardından Azure aboneliğinize bağlanmak için [Add-AzureAccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) ile birlikte bu bilgiyi kullanabilirsiniz. Kimlik bilgisi birden çok Azure aboneliğinin yöneticisi ise, doğru olanı belirlemek için [Select-AzureSubscription](http://msdn.microsoft.com/library/dn495203.aspx)’ı kullanmalısınız. Bu, genellikle çoğu Azure Automation runbook’larının üst kısmında görünen örnek Windows PowerShell’in altında gösterilir.

    $cred = Get-AutomationPSCredential –Name "myuseraccount.onmicrosoft.com"
    Add-AzureAccount –Credential $cred
    Select-AzureSubscription –SubscriptionName "My Subscription"

Runbook uygulamanızdaki her [kontrol noktası](http://technet.microsoft.com/library/dn469257.aspx#bk_Checkpoints) sonrasında bu satırları yinelemelisiniz. Runbook askıya alınır ve ardından başka bir çalışan üzerinde işlemi sürdürürse, runbook’un yeniden kimlik doğrulaması gerçekleştirmesi gerekir.

## İlgili makaleler:
- [Azure Automation: Azure Active Directory'yi kullanarak Azure için kimlik doğrulama](https://azure.microsoft.com/blog/2014/08/27/azure-automation-authenticating-to-azure-using-azure-active-directory/)
 



<!---HONumber=Jun16_HO2-->


