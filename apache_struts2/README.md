# Apache Struts2 (CVE 2017-5638)

## Description

Apache Struts versions prior to 2.3.32 and 2.5.10.1 are affected by a remote code execution vulnerability stemming from a flaw in the exception handling during file upload attempts.

## Steps to Recreate Vulnerable Environment

To recreate the Apache Struts2 vulnerability you will need to install:

* [Docker](https://docs.docker.com/engine/installation/) (community edition works fine)
* Python 2.7 or above

Create the vulnerable environment by following these steps

* Pull down a docker image containing the vulnerable version of Apache Struts2:
 ```
 $ docker pull piesecurity/apache-struts2-cve-2017-5638
 ```
* Run the docker image:
 ```
 $ docker run -p 8080:8080 piesecurity/apache-struts2-cve-2017-5638
 ````
* Visit [http://localhost:8080/showcase.action](http://localhost:8080/showcase.action) in a browser to ensure that the Apache Struts2 server is running. You should see a "Welcome!" page.

### Steps to Run the Exploit

You can exploit the vulnerable Apache Struts2 server created above by sending a specially crafted HTTP request to upload a file, where the request takes advantage of a bug with how the vulnerable Struts version handles errors. With the Jakarta Multipart Parser, Struts will parse certain header strings and execute the header value if it contains [OGNL](https://commons.apache.org/proper/commons-ognl/) code. Malicious OGNL code could allow an attacker to run shell commands on the affected server.

To run the exploit:

* Use the [pwn_struts2.py](./pwn_struts2.py) python file to send a specially crafted HTTP request to the vulnerable server:
 ```
 $ python pwn_struts2.py http://localhost:8080/ "pwd"
 ```
 
 The second  argument to the python script is the URL to attack (our vulnerable Apache Struts2 environment, and the third argument is the command that will be run via remote code execution. The above comand should output:
 ```
 [*] CVE: 2017-5638 - Apache Struts2 S2-045
 [*] cmd: pwd

 /usr/local/tomcat
 ```

 The `/usr/local/tomcat` is the result from the `pwd` command that was passed in, showing that the Apache Struts2 server is running from that directory.

## How the Exploit Worked

The specially crafted HTTP request takes advantage of a bug with how the vulnerable Struts version handles errors. With the Jakarta Multipart Parser, Struts will parse certain header strings and execute the header value if it contains [OGNL](https://commons.apache.org/proper/commons-ognl/) code. Malicious OGNL code could allow an attacker to run shell commands on the affected server.

After an attacker can run shell commands via remote code exection, the attacker has full control of the server. The attacker could do many malicious actions like move laterally from within the network, modify the contents of the server, or shut down the server.

Diving into the exploit code, the majority of the code is the [specially crafted payload](https://github.com/ErikOwen/attack_replay/blob/master/apache_struts2/pwn_struts2.py#L9) that is delivered via the [Content-Type Header](https://github.com/ErikOwen/attack_replay/blob/master/apache_struts2/pwn_struts2.py#L28). Aside from that, the code sends the HTTP request and prints out the response, which shows the results of the remote code exection.

## References

* [Vulnerable Struts2 Docker Image](https://hub.docker.com/r/piesecurity/apache-struts2-cve-2017-5638/)
* [Struts2 Exploit](https://github.com/rapid7/metasploit-framework/issues/8064)
* [Stuts2 Explained](https://www.tinfoilsecurity.com/blog/how-to-hack-with-Strutshock-Apache-Struts-2-RCE-CVE-2017-5638)
* [OGNL](https://commons.apache.org/proper/commons-ognl/)