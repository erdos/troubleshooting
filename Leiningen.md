# Could not transfer artifact (HTTP)

The following error message when downloading dependencies:
```
Could not transfer artifact ... from/to ... (...): Cannot access ... with type default using the available connector factories: BasicRepositoryConnectorFactory
```

Problem: This is because the repository does not support HTTPS only HTTP.

Solution: Write the following in the first line of the `project.clj` file:

Source: [issue on GitHub](https://github.com/technomancy/leiningen/issues/2277)

```
(require 'cemerick.pomegranate.aether)
(cemerick.pomegranate.aether/register-wagon-factory!
 "http" #(org.apache.maven.wagon.providers.http.HttpWagon.))
```

# Generally speaking

Run a `lein clean` before starting to debug a suspected module.
