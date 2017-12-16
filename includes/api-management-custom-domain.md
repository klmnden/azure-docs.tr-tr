## <a name="how-apim-proxy-server-responds-with-ssl-certificates-in-the-tls-handshake"></a>APIM Proxy sunucusu TLS el sıkışma SSL sertifikalarının nasıl yanıt verir

### <a name="clients-calling-with-sni-header"></a>SNI üstbilgiyle çağırma istemcileri
Müşterinin Proxy için yapılandırılmış bir veya birden fazla özel etki alanı varsa, APIM HTTPS isteklerini özel etki alanlarındaki (örneğin, contoso.com) yanı sıra varsayılan etki alanı yanıtlayan (örneğin, hizmet name.azure api.net apim). Sunucu adı göstergesi (SNI) üstbilgisinde yer alan bilgiler bağlı olarak, uygun sunucu sertifikasıyla APIM yanıt verir.

### <a name="clients-calling-without-sni-header"></a>SNI üstbilgisi olmadan çağırma istemcileri
Müşteri göndermemek bir istemci kullanıp kullanmadığını [SNI](https://tools.ietf.org/html/rfc6066#section-3) , üst APIM aşağıdaki mantığa göre yanıtları oluşturur:

* Hizmet proxy'si için yapılandırıldığında yalnızca bir özel etki alanı varsa, varsayılan sertifika Proxy özel etki alanı verilmiş olan sertifikadır.
* Hizmet proxy'si için birden fazla özel etki alanlarını yapılandırmış varsa (yalnızca desteklenen **Premium** katmanı), müşterinin hangi sertifikanın varsayılan sertifika olmalıdır belirleyebilirsiniz. Varsayılan Sertifika ayarlamak için [defaultSslBinding](https://docs.microsoft.com/rest/api/apimanagement/apimanagementservice/createorupdate#definitions_hostnameconfiguration) özelliğinin ayarlanması true ("defaultSslBinding": "true"). Müşteri özelliği ayarlı değil, varsayılan sertifika *.azure api.net barındırılan Varsayılan Proxy etki alanı için verilen sertifikadır.
