# REPL
- Exit the REPL: `(System/exit 0)`

## Enhance user experience in the REPL
Using rebel tool to add syntax highlighting, additional commands and on-the-fly suggestions.

1. Create the following file: `~/.clojure/dps.edn`
2. Populate it as follows:
```
{
  :aliases {:rebel {:extra-deps {com.bhauman/rebel-readline {:mvn/version "0.1.5"}}
                   :exec-fn rebel-readline.tool/repl
                   :exec-args {}
                   :main-opts ["-m" "rebel-readline.main"]}}
 }
```
3. Starting the REPL using `clojure -T:rebel`
*Nota bene*: loading a tool using `-T` flag run WITHOUT the context of the project. Hence you loads NOT the dependencies!

### Recommendations about web frameworks
Sean Corfield, veteran software architect:
"I think Luminus/Kit are overkill for beginners. There are a lot of moving parts, so it can be hard when something goes 
wrong.
When folks are getting started with Clojure and web apps, I always recommend they start with Ring and play with that, 
then either Compojure or Reitit to add routing, and gradually add more functionality and more libraries as you need it.
But, that said, Luminus and Kit give you a batteries-included, "production-grade" set of libraries in an organized 
template. So it depends what you're trying to achieve."


### Creating a new project
1. Click "New project"
2. Select Clojure/Deps
3. Name the project
4. Create a `src` subdirectory
5. Right-click on it and select "Mark directory as Source Folder"
6. Write your own program in `src/<my_file>.clj`
7. Create a `deps.edn` at the project's root consisting of the following:
```
{
 :aliases {:rebel {:extra-deps {com.bhauman/rebel-readline {:mvn/version "0.1.5"}}
                   :exec-fn rebel-readline.tool/repl
                   :exec-args {}
                   :main-opts ["-m" "rebel-readline.main"]}}
 :deps {clojure.java-time/clojure.java-time {:mvn/version "1.4.3"}
        org.clojure/clojure {:mvn/version "1.12.0"}}
}
```
8. Focus on your program's tab and push Shift + F10

*Nota Bene*: 
- select `Build/Rebuild project` 
- then click `File/Invalidate Caches/Invalidate and Restart` to ensure that the `deps.edn` file is properly regenerated
and the project's context actually reloaded.
- Finally, run `rm -rf ~/.m2/repository/org/clojure` to delete cached Clojure JAR