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

	response, _, err := client.Exchange(msg, "1.1.1.1:53")

	if err != nil {
		log.Fatalf("[CRITICAL ERROR] : %v ", err)
		return nil
	}

	if response == nil {
		log.Fatalf("[CRITICAL ERROR] : no reply from the server\n")
		return nil
	}

	return response.Answer
}
```

----- ENGLISH -----

1. func DNS_Resolver(domain string, queryType uint16) []dns.RR {: This line defines the DNS_Resolver function. This function will make a DNS query taking a domain name and query type and return the answers. The answers will be returned as a slice of type dns.RR.

2. msg := new(dns.Msg): Creating a new DNS message.

3. msg.SetQuestion(dns.Fqdn(domain), queryType): Sets the query part of the generated message. dns.Fqdn(domain) completes the domain name and queryType sets the query type.

4. msg.RecursionDesired = true: The "Recursion Desired" field of the message is set to true. This determines whether the server redirects the query to other servers.

5. client := &dns.Client{Timeout: 10 * time.Second}: Creates the DNS client and sets a timeout.

6. response, _, err := client.Exchange(msg, "1.1.1.1.1:53"): The generated message is sent to the DNS server at IP address "1.1.1.1.1" and a response is received. Response, error and statistics information are assigned to response, _, err variables respectively.

7. if err != nil { log.Fatalf("[CRITICAL ERROR] : %v ", err) return nil }: If there is an error, the error message is printed and the program terminates.

8. if response == nil { log.Fatalf("[CRITICAL ERROR] : no reply from the server\n") return nil }: If there is no reply from the server, an error message is printed and the program terminates.

9. return response.Answer: If there is no error and the response was received, the records in the response (Answer) are returned.

In this way, the DNS_Resolver function takes a domain name and query type, makes a DNS query using this information and returns the answers.


----- TÜRKÇE -----

1. func DNS_Resolver(domain string, queryType uint16) []dns.RR {: Bu satırda, DNS_Resolver fonksiyonu tanımlanıyor. Bu fonksiyon, bir alan adı ve sorgu türü alarak DNS sorgusu yapacak ve cevapları döndürecek. Cevaplar dns.RR türünde bir dilim olarak döndürülecek.

2. msg := new(dns.Msg): Yeni bir DNS mesajı oluşturuluyor.

3. msg.SetQuestion(dns.Fqdn(domain), queryType): Oluşturulan mesajın sorgu kısmı belirleniyor. dns.Fqdn(domain) ile alan adı tamamlanıyor ve queryType ile sorgu türü belirleniyor.

4. msg.RecursionDesired = true: Mesajın "Rekürsif İsteniyor" (Recursion Desired) alanı true olarak ayarlanıyor. Bu, sunucunun sorguyu başka sunuculara yönlendirip yönlendirmediğini belirler.

5. client := &dns.Client{Timeout: 10 * time.Second}: DNS istemcisini oluşturuluyor ve bir zaman aşımı (timeout) belirleniyor.

6. response, _, err := client.Exchange(msg, "1.1.1.1:53"): Oluşturulan mesaj "1.1.1.1" IP adresindeki DNS sunucusuna gönderiliyor ve cevap alınıyor. Cevap, hata ve istatistik bilgileri response, _, err değişkenlerine sırasıyla atanıyor.

7. if err != nil { log.Fatalf("[CRITICAL ERROR] : %v ", err) return nil }: Eğer bir hata varsa, hata mesajı yazdırılıp program sonlandırılıyor.

8. if response == nil { log.Fatalf("[CRITICAL ERROR] : no reply from the server\n") return nil }: Eğer sunucudan cevap alınamadıysa, hata mesajı yazdırılıp program sonlandırılıyor.

9. return response.Answer: Eğer herhangi bir hata yoksa ve cevap alındıysa, cevabın içindeki kayıtlar (Answer) döndürülüyor.

Bu şekilde, DNS_Resolver fonksiyonu bir alan adı ve sorgu türü alarak bu bilgileri kullanarak DNS sorgusu yapar ve cevapları döndürür. 

```go
func RRToString(rr dns.RR) string {
	switch rr := rr.(type) {
	case *dns.A:
		return fmt.Sprintf("Domain: %s\nTTL:%d\nClass: %s\nQuery Type: A\nIP Address: %s\n",
			rr.Hdr.Name, rr.Hdr.Ttl, dns.Class(rr.Hdr.Class).String(), rr.A.String())
	default:
		return "Unknown record type"
	}
}
```

----- ENGLISH -----

1. func RRToString(rr dns.RR) string {: This line defines the RRToString function. This function will take a DNS record (of type dns.RR) and return its information as text.

2. switch rr := rr.(type) {: This line uses a switch statement to perform different operations depending on the type of the variable rr.

3. case *dns.A:: If the variable rr is of type dns.A, execute the following block.

4. return fmt.Sprintf("Domain: %s\nTTL:%d\nClass: %s\nQuery Type: A\nIP Address: %s\n",
rr.Hdr.Name, rr.Hdr.Ttl, dns.Class(rr.Hdr.Class).String(), rr.A.String()): In this line, the information of the record of type dns.A is formatted and returned as text. Domain name, TTL (Time to Live), Class, Query Type and IP Address information are generated as text.

5. default:: If the variable rr is of another type, run the following block.

6. return "Unknown record type": This line returns "Unknown record type" for an unknown record type.

In this way, the RRToString function takes a DNS record and returns the relevant information as text according to the type of the record.


----- TÜRKÇE -----

1. func RRToString(rr dns.RR) string {: Bu satırda, RRToString fonksiyonu tanımlanıyor. Bu fonksiyon, bir DNS kaydı (dns.RR türünde) alarak bu kaydın bilgilerini bir metin olarak döndürecek.

2. switch rr := rr.(type) {: Bu satırda, rr değişkeninin türüne göre farklı işlemler yapmak için bir switch ifadesi kullanılıyor.

3. case *dns.A:: Eğer rr değişkeni bir dns.A türünde ise aşağıdaki bloğu çalıştır.

4. return fmt.Sprintf("Domain: %s\nTTL:%d\nClass: %s\nQuery Type: A\nIP Address: %s\n",
rr.Hdr.Name, rr.Hdr.Ttl, dns.Class(rr.Hdr.Class).String(), rr.A.String()): Bu satırda, dns.A türündeki kaydın bilgileri formatlanarak bir metin olarak döndürülüyor. Domain adı, TTL (Time to Live), Sınıf (Class), Sorgu Türü (Query Type) ve IP Adresi bilgileri metin olarak oluşturuluyor.

5. default:: Eğer rr değişkeni başka bir türde ise aşağıdaki bloğu çalıştır.

6. return "Unknown record type": Bu satırda, bilinmeyen bir kayıt türü için "Unknown record type" metni döndürülüyor.

Bu şekilde, RRToString fonksiyonu bir DNS kaydını alarak bu kaydın türüne göre ilgili bilgileri metin olarak döndürüyor. 

```go
func main() {
	var domain string
	fmt.Printf("Enter a domain : ")
	fmt.Scanln(&domain)

	answers := DNS_Resolver(domain, dns.TypeA)

	for _, answer := range answers {
		fmt.Printf(RRToString(answer))
	}

}
```

----- ENGLISH -----

1. func main() {: This line defines the main function. This function is the entry point of the program.

2. var domain string: This line defines a string variable called domain. This variable will hold the domain name entered by the user.

3. fmt.Printf("Enter a domain : "): This line prints a message asking the user to enter a domain name.

4. fmt.Scanln(&domain): This line reads the domain name entered by the user into the domain variable.

5. answers := DNS_Resolver(domain, dns.TypeA): This line calls the DNS_Resolver function to retrieve the DNS records (type A records) of the entered domain name and assigns these records to the answers variable.

6. for _, answer := range answers {: On this line, run the following block for each record in the answers array.

7. fmt.Printf(RRToString(answer)): This line calls the RRToString function to convert each DNS record to text format and prints the output to the screen.

In this way, the main function takes a domain name from the user, parses the DNS records for that domain name and prints them to the screen. 


----- TÜRKÇE ------

1. func main() {: Bu satırda, main fonksiyonu tanımlanıyor. Bu fonksiyon programın giriş noktasıdır.

2. var domain string: Bu satırda, domain adında bir string değişken tanımlanıyor. Bu değişken kullanıcı tarafından girilen domain adını tutacak.

3. fmt.Printf("Enter a domain : "): Bu satırda, kullanıcıdan bir domain adı girmesini isteyen bir mesaj yazdırılıyor.

4. fmt.Scanln(&domain): Bu satırda, kullanıcının girdiği domain adını domain değişkenine okuyarak atıyor.

5. answers := DNS_Resolver(domain, dns.TypeA): Bu satırda, DNS_Resolver fonksiyonunu çağırarak girilen domain adına ait DNS kayıtlarını (A tipi kayıtlar) alıyor ve bu kayıtları answers değişkenine atıyor.

6. for _, answer := range answers {: Bu satırda, answers dizisindeki her bir kayıt için aşağıdaki bloğu çalıştır.

7. fmt.Printf(RRToString(answer)): Bu satırda, her bir DNS kaydını metin formatına dönüştürmek için RRToString fonksiyonunu çağırıp çıktısını ekrana yazdırıyor.

Bu şekilde, main fonksiyonu kullanıcıdan bir domain adı alarak bu domain adına ait DNS kayıtlarını çözümleyip ekrana yazdırıyor. 

# Dns Server

```go
type dnsHandler struct{}

func (dh *dnsHandler) ServeDNS(rw dns.ResponseWriter, r *dns.Msg) {
	msg := new(dns.Msg)
	msg.SetReply(r)
	msg.Authoritative = true

	for _, question := range r.Question {
		answer := DNS_Resolver(question.Name, question.Qtype)
		msg.Answer = append(msg.Answer, answer...)
	}
	rw.WriteMsg(msg)
}
```

----- ENGLISH ------

1. type dnsHandler struct{}: This line defines a structure (struct) by creating a type called dnsHandler. This structure can contain a specific method that handles DNS requests.

2. func (dh *dnsHandler) ServeDNS(rw dns.ResponseWriter, r *dns.Msg) {: This line defines a function called ServeDNS, which is a method of type dnsHandler. This function is used to process DNS requests and takes two arguments: a write interface of type dns.ResponseWriter and a DNS message of type *dns.Msg.

3. msg := new(dns.Msg): This line creates a new DNS message of type dns.Msg.

4. msg.SetReply(r): This line sets the generated msg as a response. This indicates that it will respond to the incoming request.

5. msg.Authoritative = true: This line specifies that the DNS response is authorized.

6. for _, question := range r.Question { ... }: This loop processes each of the incoming DNS requests. r.Question must be an array containing the questions in the incoming DNS request.

7. answer := DNS_Resolver(question.Name, question.Qtype): This line resolves the answer to the DNS query using the DNS_Resolver function. The function takes the queried domain name and query type and returns the appropriate answers to that query.

8. msg.Answer = append(msg.Answer, answer...): This line appends the resolved answers to the DNS message's list of answers.

9. rw.WriteMsg(msg): This line sends the generated DNS response through the given dns.ResponseWriter.

----- TÜRKÇE ------

1. type dnsHandler struct{}: Bu satır, dnsHandler adında bir tür oluşturarak bir yapı (struct) tanımlar. Bu yapı, DNS isteklerini işleyen belirli bir yöntemi içerebilir.

2. func (dh *dnsHandler) ServeDNS(rw dns.ResponseWriter, r *dns.Msg) {: Bu satır, dnsHandler türünde bir yöntem olan ServeDNS adında bir fonksiyon tanımlar. Bu fonksiyon, DNS isteklerini işlemek için kullanılır ve iki argüman alır: dns.ResponseWriter türünde bir yazma arabirimi ve *dns.Msg türünde bir DNS mesajı.

3. msg := new(dns.Msg): Bu satır, yeni bir dns.Msg türünde bir DNS mesajı oluşturur.

4. msg.SetReply(r): Bu satır, oluşturulan msg'yi bir yanıt olarak ayarlar. Bu, gelen isteğe yanıt vereceğini belirtir.

5. msg.Authoritative = true: Bu satır, DNS yanıtının yetkili olduğunu belirtir.

6. for _, question := range r.Question { ... }: Bu döngü, gelen DNS isteklerinin her birini işler. r.Question, gelen DNS isteğindeki soruları içeren bir dizi olmalıdır.

7. answer := DNS_Resolver(question.Name, question.Qtype): Bu satır, DNS_Resolver fonksiyonunu kullanarak DNS sorgusunun yanıtını çözer. Fonksiyon, sorulan alan adı ve sorgu türünü alır ve bu sorguya uygun yanıtları döndürür.

8. msg.Answer = append(msg.Answer, answer...): Bu satır, çözülen yanıtları DNS mesajının yanıtlar listesine ekler.

9. rw.WriteMsg(msg): Bu satır, oluşturulan DNS yanıtını verilen dns.ResponseWriter aracılığıyla gönderir.

```go
func DNS_Server_Start() {
	handler := new(dnsHandler)

	server := &dns.Server{
		Addr:      ":53",
		Net:       "udp",
		Handler:   handler,
		UDPSize:   65535,
		ReusePort: true,
	}

	fmt.Println("Start a DNS server on port 53!")

	err := server.ListenAndServe()

	if err != nil {
		fmt.Printf("Failed to start server: %s\n", err.Error())
	}
}
```
------ ENGLISH ------

1. We create a new DNS handler called `dnsHandler`. This handler represents the structure that will process and respond to incoming DNS requests.

2. We create a DNS server called `dns.Server`. This server will listen for incoming UDP packets on the specified IP address and port number and process them through `dnsHandler`.

3. The configuration of the server is as follows:
   - `Addr`: ":53" specifies that the server listens on port number 53.
   - `Net`: "udp" indicates that the server will use the UDP protocol.
   - `Handler`: The `dnsHandler` object specifies the structure that will handle incoming requests.
   - `UDPSize`: 65535 specifies the maximum UDP packet size.
   - `ReusePort`: true allows running multiple servers on the same port.

4. `Start a DNS server on port 53!" is printed on the screen to indicate that the server is started.

5. The `server.ListenAndServe()` method starts the server and starts listening for incoming requests.

6. If the server fails to start, an error message is printed to the screen and the user is notified.

This function starts a DNS server on the specified port and listens for incoming DNS requests. 


----- TÜRKÇE ------

1. `dnsHandler` adında yeni bir DNS handler oluşturuyoruz. Bu handler, gelen DNS isteklerini işleyecek ve yanıt verecek olan yapıyı temsil eder.

2. `dns.Server` adında bir DNS sunucusu oluşturuluyor. Bu sunucu, belirtilen IP adresi ve port numarası üzerinden gelen UDP paketlerini dinleyecek ve bu paketlere `dnsHandler` üzerinden işlem yapacak.

3. Sunucunun yapılandırması şu şekildedir:
   - `Addr`: ":53" ifadesi, sunucunun 53 numaralı port üzerinden dinlemesini belirtir.
   - `Net`: "udp" ifadesi, sunucunun UDP protokolünü kullanacağını belirtir.
   - `Handler`: `dnsHandler` nesnesi, gelen istekleri işleyecek olan yapıyı belirtir.
   - `UDPSize`: 65535 ifadesi, maksimum UDP paket boyutunu belirtir.
   - `ReusePort`: true ifadesi, aynı port üzerinde birden fazla sunucu çalıştırılmasına olanak tanır.

4. "Start a DNS server on port 53!" ifadesi ekrana yazdırılarak, sunucunun başlatıldığı bilgisi verilir.

5. `server.ListenAndServe()` metodu ile sunucu başlatılır ve gelen istekler dinlenmeye başlar.

6. Eğer sunucu başlatılamazsa, hata mesajı ekrana yazdırılır ve kullanıcı bilgilendirilir.

Bu fonksiyon, belirtilen port üzerinde bir DNS sunucusunu başlatır ve gelen DNS isteklerini dinleyerek işlem yapar. 

```go
func main() {

	var domain string
	fmt.Printf("Enter a domain : ")
	fmt.Scanln(&domain)
	fmt.Println("")

	answers := DNS_Resolver(domain, dns.TypeA)

	for _, answer := range answers {
		fmt.Printf(RRToString(answer))
		fmt.Print("-------------------------\n")
	}

	var startServer string
	fmt.Println("")
	fmt.Printf("Do you want to start the DNS server? (Y/N): ")
	fmt.Scanln(&startServer)

	if startServer == "Y" || startServer == "y" {
		DNS_Server_Start()
	} else {
		fmt.Println("DNS server not initialized.")
	}

}
```

------ ENGLISH ------

1. Create a variable named `domain` and ask the user to enter a domain.
2. For the domain entered by the user, the `DNS_Resolver` function is called to perform DNS resolution and the answers are assigned to the `answers` variable.
3. For each answer in the `answers` array, the `RRToString` function is called to print the DNS records to the screen.
4. An input is received to ask the user whether to start the DNS server.
5. If the user enters "Y" or "y", the `DNS_Server_Start` function is called to start the DNS server.
6. If the user enters "N" or "n", the message "DNS server not started." is printed on the screen.

------ TÜRKÇE ------

1. `domain` adında bir değişken oluşturulur ve kullanıcıdan bir domain girmesi istenir.
2. Kullanıcının girdiği domain için `DNS_Resolver` fonksiyonu çağrılarak DNS çözümlemesi yapılır ve cevaplar `answers` değişkenine atanır.
3. `answers` dizisindeki her cevap için `RRToString` fonksiyonu çağrılarak DNS kayıtları ekrana yazdırılır.
4. Kullanıcıya DNS sunucusunu başlatıp başlatmayacağını sormak için bir input alınır.
5. Eğer kullanıcı "Y" veya "y" girerse, `DNS_Server_Start` fonksiyonu çağrılarak DNS sunucusu başlatılır.
6. Eğer kullanıcı "N" veya "n" girerse, ekrana "DNS sunucusu başlatılmadı." mesajı yazdırılır.
