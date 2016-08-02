<properties
   pageTitle="Azure Traffic Manager'da uç noktaları yönetme | Microsoft Azure"
   description="Bu makale, Azure Traffic Manager'da uç noktalar ekleme, kaldırma, etkinleştirme ve devre dışı bırakma konularında size yardımcı olacaktır."
   services="traffic-manager"
   documentationCenter=""
   authors="joaoma"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/17/2016"
   ms.author="joaoma" />

# Uç noktaları ekleme, devre dışı bırakma, etkinleştirme veya silme

Azure App Service'teki Web Apps özelliği, web sitesi modundan bağımsız olarak bir veri merkezi içindeki web siteleri için yük devretme ve hepsini bir kez deneme trafik yönlendirme işlevini zaten sağlamaktadır. Azure Traffic Manager, farklı veri merkezlerinde bulunan web siteleri ve bulut hizmetleri için yük devretme ve hepsini bir kez deneme trafik yönlendirmesini belirtmenize olanak tanır. Bu işlevin sağlanması için gereken ilk adım, bulut hizmeti veya web sitesi uç noktasını Traffic Manager'a eklemektir.

>[AZURE.NOTE] Klasik Azure portalını kullanarak dış konumları veya Traffic Manager profillerini uç noktalar olarak ekleyemezsiniz. [Tanım Oluşturma](http://go.microsoft.com/fwlink/p/?LinkId=400772) REST API'sini veya Windows PowerShell [Add-AzureTrafficManagerEndpoint](http://go.microsoft.com/fwlink/p/?LinkId=400774)'i kullanmanız gerekir.

Aynı zamanda bir Traffic Manager profilinin parçası olan tekil uç noktalarını da devre dışı bırakabilirsiniz. Uç noktalar hem bulut hizmetlerini hem de web sitelerini içerir. Devre dışı bırakılan bir uç nokta profilin parçası olmaya devam eder ancak profil, uç nokta profile dahil edilmemiş gibi davranır. Bu eylem, bakım modunda veya yeniden dağıtılmakta olan bir uç noktanın geçici olarak kaldırılması için son derece kullanışlıdır. Uç nokta yeniden çalışır duruma geldiği zaman etkinleştirilebilir.

>[AZURE.NOTE] Bir uç noktanın devre dışı bırakılması Azure'daki dağıtım durumu ile hiçbir şekilde bağlantılı değildir. Sağlıklı bir uç nokta Traffic Manager'da devre dışı bırakıldığında bile çalışır durumda kalır ve trafik alabilir. Ek olarak, bir uç noktanın bir profilde devre dışı bırakılması başka bir profildeki durumunu etkilemez.

## Bir bulut hizmeti veya web sitesi uç noktası ekleme


1. Klasik Azure portalındaki Traffic Manager bölmesinde, değiştirmek istediğiniz uç nokta ayarlarını içeren Traffic Manager profilini bulun ve ardından profil adının sağında yer alan oka tıklayın. Bu işlem, profilin ayarlar sayfasını açar.
2. Yapılandırmanızın zaten parçası olan uç noktaları görüntülemek için sayfanın üst kısmındaki **Uç Noktalar**'a tıklayın.
3. **Hizmet Uç Noktaları Ekleme** sayfasına erişmek için sayfanın alt kısmındaki **Ekle**'ye tıklayın. Varsayılan olarak, sayfada **Hizmet Uç Noktaları** altındaki bulut hizmetleri listelenir.
4. Bulut hizmetlerini bu profil için uç noktalar olarak etkinleştirmek üzere listeden bulut hizmetlerini seçin. Bulut hizmeti adı temizlendiği zaman uç noktalar listesinden kaldırılır.
5. Web siteleri için **Hizmet Türü** açılan listesine tıklayın ve ardından **Web uygulaması** seçeneğini belirleyin.
6. Bu profil için uç noktalar olarak eklemek üzere listeden web sitelerini seçin. Web sitesi adı temizlendiği zaman uç noktalar listesinden kaldırılır. Her Azure veri merkezi (bölge olarak da bilinir) için tek bir web sitesi seçebileceğinizi unutmayın. Birden çok web sitesini barındıran bir veri merkezindeki bir web sitesini seçerseniz ilk web sitesini seçtiğiniz zaman aynı veri merkezinde bulunan diğerleri seçilemez duruma gelir. Ayrıca, yalnızca Standart web sitelerinin listelendiğini unutmayın.
7. Bu profil için uç noktaları seçtikten sonra, sağ alt bölümdeki onay işaretine tıklayarak değişikliklerinizi kaydedin.

>[AZURE.NOTE] *Yük devretme* trafik yönlendirme yöntemini kullanıyorsanız bir uç nokta ekledikten veya kaldırdıktan sonra, yapılandırmanızda istediğiniz yük devretme sırasını yansıtması için Yapılandırma sayfasındaki Yük Devretme Öncelik Listesini ayarlamayı unutmayın. Daha fazla bilgi için bkz. [Yük devretme trafik yönlendirmesini yapılandırma](traffic-manager-configure-failover-routing-method.md).

## Bir uç noktayı devre dışı bırakma

1. Klasik Azure portalındaki Traffic Manager bölmesinde, değiştirmek istediğiniz uç nokta ayarlarını içeren Traffic Manager profilini bulun ve ardından profil adının sağında yer alan oka tıklayın. Bu işlem, profilin ayarlar sayfasını açar.
2. Yapılandırmanıza dahil edilen uç noktaları görüntülemek için sayfanın üst kısmındaki **Uç Noktalar**'a tıklayın.
3. Devre dışı bırakmak istediğiniz uç noktaya tıklayın ve ardından sayfanın alt kısmındaki **Devre Dışı Bırak**'a tıklayın.
4. Traffic Manager etki alanı adı için yapılandırılmış DNS Yaşam Süresi (TTL) temel alınarak uç noktaya akan trafik durur. Traffic Manager profilinin Yapılandırma sayfasından TTL'yi değiştirebilirsiniz.

## Bir uç noktayı etkinleştirme

1. Klasik Azure portalındaki Traffic Manager bölmesinde, değiştirmek istediğiniz uç nokta ayarlarını içeren Traffic Manager profilini bulun ve ardından profil adının sağında yer alan oka tıklayın. Bu işlem, profilin ayarlar sayfasını açar.
2. Yapılandırmanıza dahil edilen uç noktaları görüntülemek için sayfanın üst kısmındaki **Uç Noktalar**'a tıklayın.
3. Etkinleştirmek istediğiniz uç noktaya tıklayın ve ardından sayfanın alt kısmındaki **Etkinleştir**'e tıklayın.
4. Profil tarafından dikte edilen şekilde trafik hizmete akmaya başlar.

## Bir bulut hizmetini veya web sitesi uç noktasını silme


1. Klasik Azure portalındaki Traffic Manager bölmesinde, değiştirmek istediğiniz uç nokta ayarlarını içeren Traffic Manager profilini bulun ve ardından profil adının sağında yer alan oka tıklayın. Bu işlem, profilin ayarlar sayfasını açar.
2. Yapılandırmanızın zaten parçası olan uç noktaları görüntülemek için sayfanın üst kısmındaki **Uç Noktalar**'a tıklayın.
3. Uç Noktalar sayfasında, profilden silmek istediğiniz uç noktanın adına tıklayın.
4. Sayfanın alt kısmındaki **Sil**'e tıklayın.

>[AZURE.NOTE] Klasik Azure portalını kullanarak dış konumları veya Traffic Manager profillerini uç noktalar olarak silemezsiniz. Windows PowerShell'i kullanmanız gerekir. Daha fazla bilgi için bkz. [Remove-AzureTrafficManagerEndpoint](https://msdn.microsoft.com/library/dn690251.aspx).

## Sonraki adımlar


[Yük devretme yönlendirme yöntemini yapılandırma](traffic-manager-configure-failover-routing-method.md)

[Hepsini bir kez deneme yönlendirme yöntemini yapılandırma](traffic-manager-configure-round-robin-routing-method.md)

[Performans yönlendirme yöntemini yapılandırma](traffic-manager-configure-performance-routing-method.md)

[Traffic Manager düşürülmüş durumu için sorun giderme](traffic-manager-troubleshooting-degraded.md)

[Traffic Manager üzerindeki işlemler (REST API Başvurusu)](http://go.microsoft.com/fwlink/p/?LinkID=313584)



<!---HONumber=Jun16_HO2-->


