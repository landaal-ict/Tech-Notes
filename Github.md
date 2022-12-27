
## github project verhuizen

```
 git clone --mirror git@example.com/upstream-repository.git
 cd upstream-repository.git
 git push --mirror git@example.com/new-location.git
 ```

## Site hosten on github

Maak een private repo en een repo dat eindigd op username.github.io

koppel de map die publiek zichtbaar moet worden aan je .github.io repo
via:

```
git submodule add repo_url mapnaam
```

## ip voor github pages

```
maak A records voor:

185.199.110.153
	
185.199.109.153
	
185.199.111.153
	
185.199.108.153
```

## Cname

```
voeg een file genaamd CNAME toe aan je github.io project.
zet daar in je domeinnaam. 
bijvoorbeeld: example.com
```
