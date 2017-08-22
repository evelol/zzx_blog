澄清一下get和post 不再以讹传讹，GET和POST的真正区别 http://www.nowamagic.net/librarys/veda/detail/1919
GET使用URL或Cookie传参。而POST将数据放在BODY中。 -----不准确
GET的URL会有长度上的限制，则POST的数据则可以非常大。 -----不准确
POST比GET安全，因为数据在地址栏上不可见。------不准确
上面这种答案只适用于html表单提交!!!! 但是现在restful api的客户端已经不仅仅是浏览器了.

1. GET和POST与数据如何传递没有关系

GET和POST是由HTTP协议定义的。在HTTP协议中，Method和Data（URL， Body， Header）是正交的两个概念，也就是说，使用哪个Method与应用层的数据如何传输是没有相互关系的。

HTTP没有要求，如果Method是POST数据就要放在BODY中。也没有要求，如果Method是GET，数据（参数）就一定要放在URL中而不能放在BODY中。

那么，网上流传甚广的这个说法是从何而来的呢？我在HTML标准中，找到了相似的描述。这和网上流传的说法一致。但是这只是HTML标准对HTTP协议的用法的约定。怎么能当成GET和POST的区别呢？

而且，现代的Web Server都是支持GET中包含BODY这样的请求。虽然这种请求不可能从浏览器发出，但是现在的Web Server又不是只给浏览器用，已经完全地超出了HTML服务器的范畴了。

知道这个有什么用？我不想解释了，有时候就得自己痛一次才记得住。

2. HTTP协议对GET和POST都没有对长度的限制

HTTP协议明确地指出了，HTTP头和Body都没有长度的要求。而对于URL长度上的限制，有两方面的原因造成：
- 浏览器。据说早期的浏览器会对URL长度做限制。据说IE对URL长度会限制在2048个字符内（流传很广，而且无数同事都表示认同）。但我自己试了一下，我构造了90K的URL通过IE9访问live.com，是正常的。网上的东西，哪怕是Wikipedia上的，也不能信。
- 服务器。URL长了，对服务器处理也是一种负担。原本一个会话就没有多少数据，现在如果有人恶意地构造几个几M大小的URL，并不停地访问你的服务器。服务器的最大并发数显然会下降。另一种攻击方式是，把告诉服务器Content-Length是一个很大的数，然后只给服务器发一点儿数据，嘿嘿，服务器你就傻等着去吧。哪怕你有超时设置，这种故意的次次访问超时也能让服务器吃不了兜着走。有鉴于此，多数服务器出于安全啦、稳定啦方面的考虑，会给URL长度加限制。但是这个限制是针对所有HTTP请求的，与GET、POST没有关系。

3.安全不安全和GET、POST没有关系

所以, 从http上本质来讲,仅仅是语意有一定的区别.
当然, 服务器定制化的处理是另外一回事了.


POST http://task.browser.360.cn/online/setpoint HTTP/1.1 Accept: */* User-Agent: Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1) Pragma: no-cache Content-Type: application/x-www-form-urlencoded Host: task.browser.360.cn Content-Length: 449 Cookie: T=s%3Ddaf1a2e6347e01ebccc72d639441f9ef%26t%3D1456561881%26lm%3D%26lf%3D1%26sk%3D03714c868adc12684c89a65c05bc7709%26mt%3D1466821360%26rc%3D4%26v%3D2.0%26a%3D1; zsmodel=MI%20NOTE%20LTE; zsosv=4.4.2; __guid=243694361.1257306006931263000.1482113838601.9001

stamp=1482151473&qt=Q%3Du%3Dfgpubh%26n%3D%25PO%25QR%25P9%25R1%25P0%25RS%25O5%25P4%25PO%25N7%25O9%25S8%26le%3Dp3EwnT91WGDjZGLmYzAioD%3D%3D%26m%3DZGZlWGWOWGWOWGWOWGWOWGWOZwDl%26qid%3D29340825%26im%3D1%5Ft013ba372ccf308e7b6%26src%3D360se%26t%3D1%0D%0AT%3Ds%3D76f8c61854f171f63f66a8f552962e8e%26t%3D1456561881%26lm%3D%26lf%3D1%26sk%3De79c0ecb263d447d96e85f23825c3924%26mt%3D1466821360%26rc%3D9%26v%3D2%2E0%26a%3D1&verify=6fce1222e64cefa2b5b3d24d65fa9eb1

