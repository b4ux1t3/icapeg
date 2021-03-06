# ICAPeg
Open Source multi-vendor ICAP server

Scan files requested via a proxy server using ICAPeg. ICAPeg currently uses [virustotal](https://www.virustotal.com/gui/home/upload) for scanning the files following the ICAP protocol. If you don't know about the ICAP protocol, here is a
bit about it:

## What is ICAP?

**ICAP** stands for **Internet Content Adaptation Protocol**. If a **content**(for example: file) you've requested over the internet
to download or whatever, needs **adaptation**(some kind of modification or analysis), the proxy server sends the content to the icap server for adaptation and after performing the required tasks on the content, the icap server sends it back to the proxy server so that it may return the adapted content back to the destination. This can occur both during request and response.

To know more about the icap protocol, [check this out](https://tools.ietf.org/html/rfc3507).

## Things to have

Before starting to play with ICAPeg, make sure you have the following things in your machine:

1. **Golang**(latest enough to be able to use go mod)

1. A **proxy** server

1. And a **virustotal api key**. [Here is how you can get it](VIRUSTOTALAPI.md)

**NOTE**: All the settings of ICAPeg is present in the **config.toml** file in the repo, including where you should put your virustotal api key.

## How do i turn this thing on!!

To turn on the ICAPeg server, proceed with the following steps(assuming you have golang installed in you system):

1. Enable go mod by
 ```
    export GO111MODULE=on

 ```

1. Add the dependencies in the vendor file by
 ```
   go mod vendor

 ```

1. Build the ICAPeg binary  by
  ```
    go build .

  ```

1. Finally execute the file like you would for any other executable according to your OS, for unix based users though
  ```
    ./icapeg

  ```
   You should see something like, ```Staring the icap server ...```
OR, you can do none of the above and simply execute the **run.sh** shell file provided, by
 ```
  ./run.sh
```
That should do the trick.

1. Now that the server is up and running the next thing to do is setup a proxy server which can send the request body to the ICAPeg server for adaptation. [Squid](http://www.squid-cache.org/) looks like just the thing for the job, go to the site provided and set it up like you want. Here is a sample conf file for squid:
 ```
  icap_enable on
  icap_service service_resp respmod_precache icap://127.0.0.1:1344/respmod-icapeg
  adaptation_access service_resp allow all
  http_port 3128
  cache deny all
```

1. Now that you have squid running as well, you can test it out by trying to download/access a file from the internet(through the proxy) and see the magic happen! You'll be able to download/access the file if its alright, but something like a malicious file, you are gonna see something like this:
![error_page](img/error_page.png)

Oh, and do not forget to setup your Browser or Machine 's  proxy settings according to the squid.


### Contributing

This project is still a WIP. So you can contribute as well. See the contributions guide [here](CONTRIBUTING.md).

### License

ICAPeg is licensed under the [MIT License](LICENSE).
