# Routing

_**Routing**_ is the process of mapping incoming requests to a specific page (endpoint). 

Web applications tend to map [[URL|URLs]] to a web page's actual file path on disk:
Incoming URL | Maps to
:--|:--
https://domain.com/city/london | c:/website/city/london.cshtml
https://domain.com/booking/checkout | c:/website/booking/checkout.cshtml
https://domain.com/login | c:/website/login.cshtml

[[Razor Pages]] uses _**page-based routing**_, which is the mapping between URLs and file paths as a basis. A one-to-one relationship like this can be very limiting as you would need a file for each `/city/cityName` page in the above table. **WE CAN** [[Passing Data to Pages|pass data]] in the URL as a query string value, which can be used to have a **SINGLE PAGE** respond differently based on that data.

See:
* [[Routing Basics|Basics]]
* [[Routing Templates|Templates]]
* [[Routing Parameters|Parameters]]
* 