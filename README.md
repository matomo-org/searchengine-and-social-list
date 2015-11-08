These are community-contributed definitions for search engine and social network detections list maintained and used by [Piwik](http://piwik.org/), the leading open source web analytics platform.

# Social Networks

Social networks are defined in YAML format in the file `Socials.yml`

The definitions contain the name of the social network, as well as a list of one or more urls.

```YAML
"My Social Network":
  - my-social-network.com
  - mysocial.org
```

# Search Engines

Search engines are defined in YAML format in the file `SearchEngines.yml`

Definitions of search engines contain several parameters that are required to be able to detect which search engine and which search keywords are included in a given url.

Those parameters are:
- name of the engine
- URLs of the engine
- request parameters (or regexes), that can be used to get the search keyword
- backlink pattern, that can be used to create a valid link back to the search engine (with the keyword)
- charsets that might be used to convert keyword to UTF-8

For each search engine (name) it is possible to define multiple configurations.
Each configuration needs to include one or more urls, one or more parameters/regexes and may include a backlink and one or more charsets.

## Configuration parameters

### urls

Each configuration needs to contain one ore more urls. Please only define the hostname.
You can use `{}` as a placeholder for country shortcodes in subdomains or tld.
- `{}.searchengine.com` would also match `de.searchengine.com` or `nl.searchengine.com`
- `searchengine.{}` would also match `searchengine.de` or `searchengine.nl`

NOTE: For tlds only `{}` would also match combined tlds like `co.uk`. (Full list `com.*, org.*, net.*, co.*, it.*, edu.*`)

### params

Each configuration needs to contain one or more params. A param is a name of a request param that might be available in the url.
As many search engines do not use query parameters to handle the keywords, but include them in the url structure, it is also possible to define a regex.
A regex need to be encapsulated by '/' 

```YAML
SearchEngine:
  -
    urls:
      - searchengine.com
    params:
      - q
      - '/search\/[^\/]+\/(.*)/'
```

The example above would first try to get the keyword with the request param `q`. If that is not available it would use the regex `'/search\/[^\/]+\/(.*)/'` to get it.
This regex would match an url like 'http://searchengine.com/search/web/piwik'

### backlink

A backlink will be used to generate a link back to the search engine including the given keyword. backlinks may be defined per configuration and need to include `{k}` as placeholder for the keyword.

```YAML
SearchEngine:
  -
    urls:
      - searchengine.com
    params:
      - q
    backlink: '/search?q={k}'
```

For the configuration above the generated backlink would look like `searchengine.com/search?q=piwik` (assuming that `piwik` is the keyword).

NOTE: The backlink will always be generated using the __first__ defined url in this configuration block.

### charsets

Charsets can be defined if search engines are using charsets other than UTF-8. The provided charset will be used to convert any detected search keyword to UTF-8.

## Simple definition

A simple defintion of a search eninge might look like this:

```YAML
SearchEngine:
  -
    urls:
      - searchengine.com
      - search-engine.org
    params:
      - q
      - as_q
```

The example above would match for the hosts `searchengine.com` and `search-engine.org` and use the request parameters `q` and `as_q` (in this order) to detect the search keyword.

## Multiple configurations

A simple definition of a search eninge with multiple configurations might look like this:

```YAML
SearchEngine:
  -
    urls:
      - searchengine.com
    params:
      - as_q
  -
    urls:
      - search-engine.org
    params:
      - q
```

The example above would again match for the hosts `searchengine.com` and `search-engine.org`. But differently to the first example the request parameter `q` would only be used for `search-engine.org` and `as_q` only for `searchengine.com`.

## Complete definition

A complete definition (including all optionals) of a search engine might look like this:

```YAML
SearchEngine:
  -
    urls:
      - searchengine.com
    params:
      - q
    backlink: '/search?q={k}'
    charsets:
      - windows-1250
  -
    urls:
      - search-engine.org
    params:
      - as_q
```

In this case, a backlink and charset is only defined for the first configuration. Which means there is no backlink nor charset set for `search-engine.org`.

# Contribute

We welcome your contributions and Pull requests at [github.com/piwik/searchengine-and-social-list](https://github.com/piwik/searchengine-and-social-list/edit/master/README.md)! 
