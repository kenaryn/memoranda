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
- Select `Build/Rebuild project` 
- Then click `File/Invalidate Caches/Invalidate and Restart` to ensure that the `deps.edn` file is properly regenerated
and the project's context actually reloaded.
- Finally, run `rm -rf ~/.m2/repository/org/clojure` to delete cached Clojure JAR


### Clojure CLI flags
- `-M` means "main options" and `-m` specifies the "main namespace" to run.
- `-X` means "execute (function)" and `:exec-fn` specifies which fn to execute and `:exec-args` specifies any default 
arguments.


## REPL
### Exploring the REPL
- Run `(require '[clojure.repl :refer :all])` to bring that namespace into the current context a list of helpful functions (*e.g.*
`dir`, `source`, `find-doc`, `apropos`).
When looking for a function whose description is comprised of several key words, try this out:
`(find-doc "(?=.*stack)(?=.*trace)(?=.*exception)")` and it will return `clojure.repl/pst`.


### Compiler connection to REPL
You need to introduce into current context (or requiring a dependency into runtime environment) a namespace to be able
to invok its functions. *e.g.*:
- Main entry point is located here: `<project_root>src/greet/core.clj` and contains the following:
```
(ns greet.core)
(defn -main []
  (println "Hi people!"))
```

- Now in the input field (viz. the bottom-right window of the screen), type:
```
(require 'greet.core)
(greet.core/-main)
```
to carry out the S-expressions evaluation between the reader and the compiler.

*Nota bene*: there is indeed a side-effect when requiring the namespace, that is why the compiler return `nil` value.


### Var
- When binding an anonymous function to a name (*e.g.* `(def salute (fn [name] (str "Hi, " name)))`), `Salute` is
henceforth a public Var that points to the function, which will be invokable, because a function deref a Var.


### Basics
- `def`: defines a var (*viz.* a constant value or a function), binding a name to a value. Litteral representation: `#'`
- `(ns-publics 'user)`: shows all the top-level Vars (`def` and `defn`) that you have defined into `user` ns.
- `let`: binds symbols to values in a lexical scope, which creates a new context for names that take precedence over
the names in the outer context, like so: `(let [name value] (<coding_using_that_name(s)>))`
- Names' spelling: a underscore must be used in filenames whereas a hyphen is used in namespaces. *e.g.*
                   - Filename: `user_defs.clj`
                   - Namespace: `resources.user-defs` 


## Lexikon
- form: complete expression, that is read by the Reader and stand for a compilation unit in Clojure
- Cursive: brings the same Clojure compiler to IntelliJ than the one installed on the machine.

### Macro - first aid kit
Using :require in a namespace declaration is actually just passing an argument to a macro (the macro called ns). 
The function require is mostly a convenience for working at the REPL.  It is rarely used in actual file-based code though.


### IntelliJ - Settings
- **It is necessary to add the following entry to `deps.edn` to let Cursive know about `src` folder being the source
root tree: `:paths ["src"]`**
