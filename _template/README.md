# Vulnerability Name

### Description

Brief paragraph talking about what the vulnerability is.

### Steps to Recreate Vulnerable Environment

To recreate vulnerability X you will need the following software installed:

* Docker
* Python

Create the vulnerable environment by following these steps

* Do thing 1
 ```
 $ docker pull some-docker-repo/some-vulnerable-image
 ```
* Do thing 2
```
$ docker run -p 8080:8080 some-docker-repo/some-vulnerable-image
````

### Steps to Run the Exploit

You can exploit the vulnerable environment created above by following these steps:

* Do thing 1
 ```
 $ tool --hack vulnerable-environment
 ```
* Do thing 2
```
$ tool --pwn vulnerable-environment
````

### How the Exploit Worked

The vulnerable environment is running version X of software, which is vulnerable to remote code execution due to a security flaw found in it's Y module.

The Y module does not sanitize input coming from requests, which allows an attacker to craft a requestin a certain way which will take advantage of the unsanitized input.

In version X.1 of the software, the vulnerability was patched by adding in code which checked input for any characters that were not alphanumeric.