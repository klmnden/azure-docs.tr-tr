<properties
  pageTitle="Azure IoT Paketi ile ilgili SSS | Microsoft Azure"
  description="IoT Paketi için sık sorulan sorular"
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="get-started-article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="05/09/2016"
  ms.author="araguila"/>
   
# IoT Paketi için sık sorulan sorular

### Azure portalında bir kaynak grubunu silmek ile azureiotsuite.com'da önceden yapılandırılmış bir çözüm için silmeye tıklama arasındaki fark nedir?

- [Azureiotsuite.com][lnk-azureiotsuite] adresinde önceden yapılandırılmış çözümü silerseniz önceden yapılandırılmış çözümü oluşturduğunuzda sağlanan tüm kaynakları silersiniz; kaynak grubuna ek kaynaklar eklediyseniz bunlar da silinir. 

- [Azure portalında][lnk-azure-portal] önceden yapılandırılmış kaynak grubunu silerseniz yalnızca bu kaynak grubundaki kaynakları silersiniz; [klasik Azure portalında][lnk-classic-portal] önceden yapılandırılmış çözümle ilişkili Azure Active Directory uygulamasını da silmeniz gerekir.

### Bir abonelikte kaç tane DocumentDB örneği sağlayabilirim?

Beş. Bu limiti yükseltmek için bir [Azure destek bileti][link-azuresupportticket] oluşturabilirsiniz ancak varsayılan olarak abonelik başına yalnızca beş DocumentDB örneği sağlayabilirsiniz. Sonuç olarak, belirli bir abonelikte en fazla beş tane önceden yapılandırılmış uzaktan izleme çözümü sağlayabilirsiniz.

### Bir abonelikte kaç tane Ücretsiz Bing Haritaları API'si sağlayabilirim?

İki. Bir abonelikte yalnızca iki tane Ücretsiz Bing Haritaları API'si oluşturabilirsiniz. Uzaktan izleme çözümü, varsayılan olarak bir Ücretsiz Bing Haritalar API'siyle sağlanır. Sonuç olarak, herhangi bir değişiklik yapılmadıysa bir abonelikte yalnızca en fazla iki tane uzaktan izleme çözümü sağlayabilirsiniz.

### Statik haritaya sahip bir uzaktan izleme çözümü dağıtımım var; etkileşimli bir Bing haritasını nasıl eklerim? 
1. [Azure portalından][lnk-azure-portal] Kurumsal QueryKey için Bing Haritaları API'nizi edinin: 
 1. Kurum için Bing Haritaları API'nizin [Azure portalında][lnk-azure-portal] bulunduğu Kaynak Grubu'na gidin.
 2. Tüm Ayarlar'a ve ardından Anahtar Yönetimi'ne tıklayın. 
 3. İki anahtar fark edeceksiniz: MasterKey ve QueryKey. QueryKey değerini kopyalayın.

     > [AZURE.NOTE] Kurumsal için Bing Haritaları API'si hesabınız yok mu? [Azure portalında][lnk-azure-portal] + Yeni'ye tıklayıp Kurumsal için Bing Haritaları API'sini aratın ve oluşturmak için istemleri izleyerek yeni bir hesap oluşturun.

2. [Azure-IoT-Remote-Monitoring][lnk-remote-monitoring-github] konumundan en son kodu çekin.

3. Depodaki /docs/ klasöründe bulunan komut satırı dağıtım rehberini izleyerek yerel bir dağıtımı veya bulut dağıtımını çalıştırın. 

4. Yerel bir dağıtım veya bir bulut dağıtımını çalıştırdıktan sonra, kök klasörünüzde dağıtım sırasında oluşturulan *.user.config file dosyanızı arayın. Bu dosyayı bir metin düzenleyicisinde açın. 

5. QueryKey'iniz için kopyaladığınız değeri eklemek için aşağıdaki satırı değiştirin: 
   
  `<setting name="MapApiQueryKey" value="" />`

### DreamSpark için Microsoft Azure'a sahipsem önceden yapılandırılmış bir çözüm oluşturabilir miyim?
Şu anda [DreamSpark için Microsoft Azure][lnk-dreamspark] hesabıyla önceden yapılandırılmış bir çözüm oluşturamazsınız. Ancak yalnızca birkaç dakika içinde [ücretsiz bir Azure deneme sürümü hesabı][lnk-30daytrial] oluşturabilir ve böylece önceden yapılandırılmış bir çözüm oluşturabilirsiniz.

### Bir AAD kiracısını nasıl silerim?

Bkz. Eric Golpe'un blog yazısı [Bir Azure AD Kiracısını Silme Kılavuzu][lnk-delete-aad-tennant].

[link-azuresupportticket]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade 
[lnk-azure-portal]: https://portal.azure.com
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring 
[lnk-dreamspark]: https://www.dreamspark.com/Product/Product.aspx?productid=99 
[lnk-30daytrial]: https://azure.microsoft.com/free/
[lnk-delete-aad-tennant]: http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx



<!--HONumber=Jun16_HO2-->


