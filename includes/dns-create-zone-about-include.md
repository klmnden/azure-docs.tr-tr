DNS bölgesi belirli bir etki alanıyla ilgili DNS kayıtlarını barındırmak için kullanılır. Etki alanınızı Azure DNS'de barındırmaya başlamak için bir DNS bölgesi oluşturmanız gerekir. Ardından bu DNS bölgesinde etki alanınız için tüm DNS kayıtları oluşturulur.

Örneğin "contoso.com" etki alanı, "mail.contoso.com" (bir posta sunucusu için) ve "www.contoso.com" (bir web sitesi için) gibi birden fazla DNS kaydını içerebilir.

Azure DNS'de bir DNS bölgesi oluştururken:

* Bölge adı kaynak grubunda benzersiz olmalıdır ve bölge daha önceden var olmamalıdır. Aksi takdirde işlem başarısız olur.
* Aynı bölge adı farklı bir kaynak grubunda veya farklı bir Azure aboneliğinde yeniden kullanılabilir.
* Birden fazla bölgenin aynı adı paylaştığı durumda her örneğe farklı bir ad sunucusu adresi atanır. Etki alanı kayıt şirketinde yalnızca bir adres kümesi yapılandırılabilir.

> [!NOTE]
> Azure DNS'de aynı etki alanıyla DNS bölgesi oluşturmak için bir etki alanına sahip değilsiniz. Ancak Azure DNS ad sunucularını etki alanı kayıt şirketinde etki alanı adı için doğru ad sunucuları olarak yapılandırmak üzere etki alanının sahibi olmanız gerekmez.
> 
> Daha fazla bilgi için bkz. [Bir etki alanını Azure DNS'ye devretme](../articles/dns/dns-domain-delegation.md).

<!--HONumber=Jan17_HO1-->


