# What is DNS Server

A DNS server is a system that translates domain names that people can easily remember (for example, www.google.com) into IP addresses (for example, 123.23.12.1).
These servers perform the translation necessary for devices on the internet to communicate with each other. In short, a DNS server is the address book of the internet.

DNS servers are usually managed by Internet Service Providers (ISPs), but specialized DNS services are also available.
These specialized services usually aim to provide faster, more secure and more reliable DNS resolution.
DNS servers play a critical role in keeping the internet running smoothly and quickly.


DNS sunucusu, insanların kolayca hatırlayabileceği alan adlarını (örneğin, www.google.com) IP adreslerine (örneğin, 123.23.12.1) dönüştüren bir sistemdir.
Bu sunucular, internet üzerindeki cihazların birbirleriyle iletişim kurabilmesi için gerekli çeviri işlemini gerçekleştirir. Kısacası, DNS sunucusu internetin adres defteridir.

DNS sunucuları genellikle İnternet Servis Sağlayıcıları (ISS) tarafından yönetilir, ancak özel DNS hizmetleri de mevcuttur.
Bu özel hizmetler genellikle daha hızlı, daha güvenli ve daha güvenilir DNS çözümlemesi sağlamayı amaçlar.
DNS sunucuları, internetin sorunsuz ve hızlı çalışmasında kritik bir rol oynar.

# Top 10 DNS Server

1. Google DNS
   - IP Address: 8.8.8.8, 8.8.4.4

2. Cloudflare DNS
   - IP Address: 1.1.1.1, 1.0.0.1

3. OpenDNS
   - IP Address: 208.67.222.222, 208.67.220.220

4. Comodo Secure DNS
   - IP Address: 8.26.56.26, 8.20.247.20

5. Quad9 DNS
   - IP Address: 9.9.9.9

6. Level3 DNS
   - IP Address: 209.244.0.3, 209.244.0.4

7. Verisign DNS
   - IP Address: 64.6.64.6, 64.6.65.6

8. Norton ConnectSafe
   - IP Address: 199.85.126.10, 199.85.127.10

9. CleanBrowsing DNS
   - IP Address: 185.228.168.9, 185.228.169.9

10. SafeDNS
   - IP Address: 195.46.39.39, 195.46.39.40

# What is DNS Records

DNS records are pieces of information within the Domain Name System (DNS) that define and manage various aspects of a domain name. 
These records are vital for enabling devices on the internet to communicate with each other and to ensure that resources are directed correctly.
DNS records are stored on a DNS server and can be of various types that serve different purposes. Here are some DNS record types and their descriptions:

    A Record (Address Record): Specifies an IPv4 address for a domain name. This record is used to translate a domain name directly to an IP address.
    AAAA Record (IPv6 Address Record): Specifies an IPv6 address for a domain name. This translates a domain name into an IPv6 address and is the IPv6 version of the A record.
    CNAME Record (Canonical Name Record): Used to point a domain name to another domain name. For example, the domain name www.google.com can be forwarded to google.com.
    

DNS kayıtları, Domain Name System (DNS) içindeki bilgi parçalarıdır ve bir alan adının çeşitli yönlerini tanımlar ve yönetir.
Bu kayıtlar, internet üzerindeki cihazların birbirleriyle iletişim kurmalarını sağlamak ve kaynakların doğru yönlendirilmesini sağlamak için hayati önem taşır.
DNS kayıtları bir DNS sunucusunda saklanır ve farklı amaçlara hizmet eden çeşitli tiplerde olabilir. İşte bazı DNS kayıt türleri ve açıklamaları:

    A Kaydı (Adres Kaydı): Bir alan adı için bir IPv4 adresi belirtir. Bu kayıt, bir alan adını doğrudan bir IP adresine çevirmek için kullanılır.
    AAAA Kaydı (IPv6 Adres Kaydı): Bir alan adı için bir IPv6 adresi belirtir. Bu, bir alan adını IPv6 adresine çevirir ve A kaydının IPv6 sürümüdür.
    CNAME Kaydı (Canonical Name Kaydı): Bir alan adını başka bir alan adına yönlendirmek için kullanılır. Örneğin, www.google.com alan adı google.com'a yönlendirilebilir.


# Build DNS Server

We use go-dns-package library for DNS server.

DNS sunucusu için go-dns-package kütüphanesini kullanıyoruz.

```go
go get github.com/miekg/dns
```

# Dns Resolver

A DNS Resolver is a function that queries the IP address corresponding to the name of a website.
This function takes a domain and a query type as parameters. The query type can be different types such as A, AAAA, or MX.

DNS Çözümleyici, bir web sitesinin adına karşılık gelen IP adresini sorgulayan bir işlevdir.
Bu işlev parametre olarak bir etki alanı ve bir sorgu türü alır. Sorgu türü A, AAAA veya MX gibi farklı türlerde olabilir.

```go
func dns_resolver(domain string, queryType uint) []dns.RR {
	msg := new(dns.Msg)
	msg.SetQuestion(dns.Fqdn(domain), uint16(queryType))
	msg.RecursionDesired = true

	client := &dns.Client{Timeout: 10 * time.Second}
}
```
- msg := new(dns.Msg): This command creates a new DNS (Domain Name System) message. DNS messages are used to help computers on the internet resolve IP addresses to domain names.

- msg.SetQuestion(dns.Fqdn(domain), uint16(queryType)): This line sets a question for the DNS request.
  The dns.Fqdn(domain) function formats the domain exactly, making it suitable for a DNS query.
  uint16(queryType) converts it to an unsigned integer value representing the query type.

- msg.RecursionDesired = true: This line indicates that we want a recursive response from the DNS server. That is, the server should search its own database and return results.

- client := &dns.Client{Timeout: 10 * time.Second}: This line creates the DNS client. The Timeout: 10 * time.Second parameter allows the request to timeout if we don't receive a response for 10 seconds.


