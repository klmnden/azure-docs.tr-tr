DNS bölgesi belirli bir etki alanıyla ilgili DNS kayıtlarını barındırmak için kullanılır. Etki alanınızı barındırmaya başlatmak için bir DNS bölgesi oluşturmanız gerekir. Belirli bir etki alanı için oluşturulan DNS kayıtları etki alanıyla ilgili DNS bölgesinde olacaktır. 

Örneğin “contoso.com” etki alanı, “mail.contoso.com” (bir posta sunucusu için) ve “www.contoso.com” (bir web sitesi için) gibi bir dizi DNS kaydı içerebilir. 

## <a name="names"></a>DNS bölge adları hakkında
* Bölge adı kaynak grubunda benzersiz olmalıdır ve bölge daha önceden var olmamalıdır. Aksi takdirde işlem başarısız olur.
* Aynı bölge adı farklı bir kaynak grubunda veya farklı bir Azure aboneliğinde yeniden kullanılabilir. 
* Birden çok bölgenin aynı adı taşıdığı durumlarda, her örnek farklı ad sunucusu adreslerine atanır; yalnızca tek bir örnek üst etki alanından temsil edilebilir. Daha fazla bilgi için bkz. [Bir etki alanını Azure DNS'ye devretme](../articles/dns/dns-domain-delegation.md).

<!--HONumber=Sep16_HO3-->


