## Free webserver SSL certificates made incredibly easy
April 18, 2016

#### Let’s Encrypt: Setting up SSL encryption on a private webserver was fairly laborious yet.

You as a private website owner maintaining your own little server with an Apache or nginx running on it might have felt quite uncomfortable when having had to set up https for your webserver till now. First you needed to find a provider for free certificates, since you probably don’t want to spend 30 bugs a month just for encrypting connections to your tiny website. Besides [startssl.com](http://startssl.com) and [cacert.org](http://www.cacert.org/) there were only few other providers that offered free certs and are commonly trusted (e.g. [namecheap.com](http://namecheap.com), but only if you buy a domain at them, or [one.com](http://one.com), but only if you buy a webspace at them). With each of them the set up was actually not that trivial. You had to generate your own private key, paste it into an incredibly poor web interface, verify your domain ownership through complicated ways (like setting up a particular e-mail address specifically for the domain you request the cert for), copy it to your server, add the certificate chain and modify the webserver’s configs. This is over now. I have discovered a super easy way to do all that automatically. There’s a new provider for free SSL certs: [**Let’s Encrypt**.](https://letsencrypt.org) All you need to do is download a small client tool to your Linux server and execute it with some parameters, like what the domain is for which you want the certificate. It then automatically verifies your ownership, generates and save a private key, the certificate and another file including the certificate as well as the entire chain in one. In case you use Apache2 webserver it even sets it up for you. In case you use nginx or any standalone webserver (like a publicly facing Node.js app) you need to include the generated cert files into the configuration, but actually that’s everything you have to do by hand. You don’t even need to generate an account or the like.

#### How does this work?

Technically it basically performs the validation (proving that you’re actually the owner of the domain) in such a way that the client tool contacts the Let’s Encrypt server to do a simple HTTP request to a specific path on your domain and at the same time starts a little webserver / listener on exactly that path (that’s why you have to stop your webserver for a short while when the client tool perfoms its verification, since it needs port 80 to listen on). If it receives the request it is proven that you’re the root of this domain. This explanation was a simplified, if you want to know how it works in detail see [https://letsencrypt.org/how-it-works/](https://letsencrypt.org/how-it-works/).

#### Why use such a certificate?

For a static webpage you definitely won’t need ssl / https, it doesn’t make sense for that purpose. But as soon as there’s any kind of private data flow FROM the client TO your server you might consider that. Especially if there’s a way for users to log in or sign up at your page you should encrypt the connection so that passwords won’t get passed through the internet in plain text.

I love that such annoying processes that traditionally required quite an amount of technical knowledge get simplified and automated more and more towards kind of a click-and-go way. Two other examples that come to my mind spontaneously are [mailcow.email](https://www.mailcow.email/) or [nvm.sh](http://nvm.sh), or of course any of those PaaS providers like DigitalOcean etc.