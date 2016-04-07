# Push to clojars with gpg

## Setup gpg 

Create a key

```bash
gpg --gen-key
```
(defaults are fine)

Check keys

```bash
gpg --list-keys
```

## Create clojars account

(If you don't have one yet)

## Setup gpg encrypted Leiningen credentials file

Create unencrypted file

```bash
gvim ~/.lein/credentials.clj
```

Set clojars info

```clojure
{ 
  #"https://clojars.org/repo"
  {:username "your_username" :password "your_pass"} 
}
```

Encrypt file

```bash
gpg --default-recipient-self -e \
  ~/.lein/credentials.clj > ~/.lein/credentials.clj.gpg
```

Delete unencrypted file

```bash
rm ~/.lein/credentials.clj
```

## Configure project.clj

Add keys 

```clojure
(defproject your-group/your-artifact "0.0.1"

  ...
  :signing {:gpg-key "your_email_address"}
  :deploy-repositories [["clojars" {:creds :gpg}]]
  ...

)
```

## Start gpg-agent (needed because of bug in gpg)

```bash
eval $(gpg-agent --daemon)
```

## Clean, build, and push jar

```bash
lein clean
lein jar
lein deploy clojars
```

## References
http://serverfault.com/questions/481103/gpg-agent-says-agent-exists-but-gpg-says-agent-doesnt-exist

https://github.com/technomancy/leiningen/blob/master/doc/GPG.md#creating-a-keypair

https://github.com/technomancy/leiningen/blob/master/doc/DEPLOY.md#gpg

http://thornydev.blogspot.com.au/2013/03/signing-and-promoting-your-clojure.html
