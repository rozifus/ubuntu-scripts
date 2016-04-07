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

## Configure project.clj

Add keys 

```clojurescript
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
