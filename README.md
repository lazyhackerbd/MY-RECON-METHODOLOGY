# MY-RECON-METHODOLOGY

What is recon?

Reconnaissance or Recon is to gather all possible information about the target or the victim you wanna hack as location , ip, open ports , services , all subdomains and all directories etc…

First of all I am gonna talk only about web applications and how to gather all possible information about the web applications to start hacking

Subdomain Enumeration :

Every domain has many subdomains can reach thousands of subs

1- https://securitytrails.com is great site for enumerating possible subdomains about any domain or organization


securitytrails.com
as you can see in the image there are more than 171K subdomains for yahoo.com and the site provides more information about the domain as records , A , AAAA , CNAME etc…

you can use securitytrails api for gathering all subdomains via terminal but it’s paid not free and recommended only for huge organizations recon

2- https://subdomainfinder.c99.nl/ is great site for gathering subdomains


subdomainfinder.c99.nl
as you can see it gives you option for copy all live subdomains , also you can use their api but it’s paid too 😊😊😊

3- amass tool from github https://github.com/owasp-amass/amass

to use amass effectively you have to provide it with many api keys

(take your time to gather all possible apis even it takes week)

after installing amass put that command in terminal

nano $HOME/.config/amass/config.yaml
and start searching for apis as shodan , censys, cloudflare ,fofa and more and more


after providing all apis you can gathered start using the amass tool as following

first put all domains in file called domains.txt or any thing other

amass enum -active -df domains.txt -config $HOME/.config/amass/config.yaml -o amass_subdoamins.txt
for large scopes for companies and ASN we use amass as :

amass intel -org "Tesla" 

then take Origin ASN (394161) for the Tesla company and

amass intel -active -asn 394161

domains related to company
if you got CIDR for company and need use amass as :

amass intel -active -cidr 159.69.129.82/32

4- assetfinder tool github https://github.com/tomnomnom/assetfinder

that tool gather all subdomains passively

assetfinder is alternative for( google dorks, crt.sh )and more passive
cat domains | assetfinder -subs-only
5- Subfinder is one of the most powerful tools but with provided Api-keys

go to ~/.config/subfinder/provider-config.yaml and add api keys from sites


api keys
then start subfinder

subfinder -d google.com 
6- virtual host

ffuf -u 'https://example.com' -H 'Host: FUZZ.example.com' -w Seclists/Discovery/DNS/top-1million-11.txt
or you can use instead of domain example.com use the ip of it


ffuf -u http://93.184.216.34 -H "Host: FUZZ.example.com" -w Seclists/Discovery/DNS/top-1million-11.txt
and after running the above command you will see many outputs with the same size ex: 4517 then filter that size with -fs


ffuf -u 'https://example.com' -H 'Host: FUZZ.example.com' -w Seclists/Discovery/DNS/top-1million-11.txt -fs 4517 
after that you can go to subdomain from vhost but not working like


but you need to add the subdomain you found to /etc/hosts this subdomain indicates to the main domain ip for ex: ffuf.me => 138.68.165.164 then any subdomain you put should indicate for the same ip as


after putting the subdomain you found in /etc/hosts and indicate it to origin domain ip save and go back to your browser and see you can access the subdomain directly as :


Observe that the page worked and downloaded secret file and you can practice more go free http://ffuf.me and https://www.acceis.fr/ffuf-advanced-tricks/

I think you have collected all possible subdomains and put them in file called subdomains.txt then run the following command to remove all duplicates

cat subdomains.txt | sort -u >> uniq_subs.txt
after that you can run httpx to get only live subdomain

cat uniq_subs.txt | httpx -o httpx
after running httpx it’s time for the Part 2

For me i use security trails , subfinder , assetfinder (for small scopes)

Part 2 :
Directory and file Enumeration

There are many ways to get directories and files

1- dirb for directory enumeration

dirb https://example.com
2- dirsearch for file enumeration

dirsearch -u https://example.com
3- ffuf for fuzzing file and directories

ffuf -u https://www.example.com/FUZZ -w wordlist/Seclists/Discovery/Web-content/raft-medium-files.txt -mc 200,302,301 -t 1000
you can use other tools as gobuster , meg , etc…

Part 3 :
Parameter fuzzing and gathering

1- arjun

arjun -u https://www.example.com/file.php 
2- paramspider

paramspider -l domains.txt -s
3- gospider

gospider -S domains.txt -o gospider
4- burpsuite paraminer

after installing the extension

go to request and right click >> extensions >> paraminer >> Guess params >> Guess everything

Part 4 :
Collecting all urls related to target

urls.txt ex: https://example.com

1- waybackurls

cat urls.txt | waybackurls
2- Gau

cat urls.txt | gau
3- Katana

katana -list urls.txt -v -jc -o katana
4- hakrawler

cat urls.txt | hakrawler
Wait me for the next steps …

follow me ,

LAZYHACKERBD
