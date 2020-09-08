# Snapdrop that works inside the LAN

[Snapdrop](https://snapdrop.net): local file sharing in your browser. Inspired by Apple's Airdrop.

## THIS SETUP REQUIRES A STUN SERVER IN THE LAN

A widely used STUN Server is coturn. 
After the installation you need to remove the Google STUN Server at the end of /client/scripts/network.js and insert the appropriate informations about your STUN server. 

If you used a password on your STUN Server you have to denote it like so: 

```
...
RTCPeer.config = {
    'iceServers': [{
        urls: ['stun:10.0.0.2:3478'],
	username: 'username',
	credential : 'password'
    }]
}

```

#### Snapdrop is built with the following awesome technologies:
* Vanilla HTML5 / ES6 / CSS3  
* Progressive Web App
* [WebRTC](http://webrtc.org/)
* [WebSockets](http://www.websocket.org/)
* [NodeJS](https://nodejs.org/en/)
* [Material Design](https://material.google.com/)

## Support the Snapdrop Community
Snadprop is free. Still, we have to pay for the server. If you want to contribute, please use PayPal:

[<img src="https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif">](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=74D2NE84JHCWG&source=url)

or Bitcoin:

[<img src="https://coins.github.io/thx/logo-color-large-pill-320px.png" alt="CoinThx" width="200"/>](https://coins.github.io/thx/#1K9zQ8f4iTyhKyHWmiDKt21cYX2QSDckWB?label=Snapdrop&message=Thanks!%20Your%20contribution%20helps%20to%20keep%20Snapdrop%20free%20for%20everybody!) 

Alternatively, you can become a [Github Sponsor](https://github.com/sponsors/RobinLinus).

Thanks a lot for supporting free and open software!


## Frequently Asked Questions

### Instructions
* [Video Instructions](https://www.youtube.com/watch?v=4XN02GkcHUM) (Big thanks to [TheiTeckHq](https://www.youtube.com/channel/UC_DUzWMb8gZZnAbISQjmAfQ))
* [idownloadblog](http://www.idownloadblog.com/2015/12/29/snapdrop/)
* [thenextweb](http://thenextweb.com/insider/2015/12/27/snapdrop-is-a-handy-web-based-replacement-for-apples-fiddly-airdrop-file-transfer-tool/)
* [winboard](http://www.winboard.org/artikel-ratgeber/6253-dateien-vom-desktop-pc-mit-anderen-plattformen-teilen-mit-snapdrop.html)
* [免費資源網路社群](https://free.com.tw/snapdrop/)

##### What about the connection? Is it a P2P-connection directly from device to device or is there any third-party-server?
It uses a P2P connection if WebRTC is supported by the browser. WebRTC needs a Signaling Server, but it is only used to establish a connection and is not involved in the file transfer.

##### What about privacy? Will files be saved on third-party-servers?
None of your files are ever sent to any server. Files are sent only between peers. Snapdrop doesn't even use a database. If you are curious have a look [at the Server](https://github.com/RobinLinus/snapdrop/blob/master/server/). Even if Snapdrop was able to view the files being transfered, WebRTC encrypts the files on transit, so the server would be unable to read them.

##### What about security? Are my files encrypted while being sent between the computers?
Yes. Your files are sent using WebRTC, which encrypts them on transit.

##### Why don't you implement feature xyz?
Snapdrop is a study in radical simplicity. The user interface is insanely simple. Features are chosen very carefully because complexity grows quadratically since every feature potentially interferes with each other feature. We focus very narrowly on a single use case: instant file transfer. 
We are not trying to optimize for some edge-cases. We are optimizing the user flow of the average users. Don't be sad if we decline your feature request for the sake of simplicity.


### Snapdrop is awesome! How can I support it? 
* [Donate via PayPal to help cover the server costs](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=74D2NE84JHCWG&source=url)
* [File bugs, give feedback, submit suggestions](https://github.com/RobinLinus/snapdrop/issues)
* Share Snapdrop on your social media.
* Fix bugs and make a pull request. 
* Do security analysis and suggestions

## Local Development
[Install docker with docker-compose.](https://docs.docker.com/compose/install/)

Clone the repository:
```
    git clone https://github.com/RobinLinus/snapdrop.git
    cd snapdrop
    docker-compose up -d
```

To restart the containers run `docker-compose restart`.
To stop the containers run `docker-compose stop`.


Now point your browser to `http://localhost:8080`.

### Testing PWA related features
PWAs require that the app is served under a correctly set up and trusted TLS endpoint.

The nginx container creates a CA certificate and a website certificate for you. To correctly set the common name of the certificate, you need to change the FQDN environment variable in `docker/fqdn.env` to the fully qualified domain name of your workstation.

If you want to test PWA features, you need to trust the CA of the certificate for your local deployment. For your convenience, you can download the crt file from `http://<Your FQDN>:8080/ca.crt`. Install that certificate to the trust store of your operating system.
- On Windows, make sure to install it to the `Trusted Root Certification Authorities` store.
- On MacOS, double click the installed CA certificate in `Keychain Access`, expand `Trust`, and select `Always Trust` for SSL.
- Firefox uses its own trust store. To install the CA, point Firefox at `http://<Your FQDN>:8080/ca.crt`. When prompted, select `Trust this CA to identify websites` and click OK.
- When using Chrome, you need to restart Chrome so it reloads the trust store (`chrome://restart`). Additionally, after installing a new cert, you need to clear the Storage (DevTools -> Application -> Clear storage -> Clear site data).

Please note that the certificates (CA and webserver cert) expire after a day.
Also, whenever you restart the nginx docker, container new certificates are created.

The site is served on `https://<Your FQDN>:443`.
    
## Deployment Notes
The client expects the server at http(s)://your.domain/server.

When serving the node server behind a proxy, the `X-Forwarded-For` header has to be set by the proxy. Otherwise, all clients that are served by the proxy will be mutually visible.

By default, the server listens on port 3000.

For an nginx configuration example, see `docker/nginx/default.conf`.

## Desktop App 
Note: if you are using Google Chrome, you can easily install Snapdrop PWA on your desktop by clicking the install button in the top-right corner.

If you are not using Chrome or Edge, you can install the [Snapdrop Desktop App](https://github.com/infin1tyy/snapdrop-desktop) built on top of Electron. (Thanks to Infin1tyy!).


