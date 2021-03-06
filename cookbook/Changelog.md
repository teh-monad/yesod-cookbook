# Changelog

Note: for a more technical, detailed set of information, see [[Detailed change list]]. This page mostly covers high-level features.

# Yesod 1.4

There were very few changes in the 1.4 release. Please see [the detailed change list](https://github.com/yesodweb/yesod/wiki/Detailed-change-list#yesod-14) for more information.

# Yesod 1.2.4

This represents a point release from 1.2. Cumulative changes since the initial 1.2 platform release include:

* Improved JSON responses for yesod-auth and yesod-core (Tero Laitinen and Greg Weber).
* Protocol-independent links for CDN-hosted resources (Iku Iwasa).
* A nicer yesod devel refresh page (Chris Done).
* Ability to switch logging level dynamically via `shouldLogIO`.
* Improved email authentication password security.
* Better request body support in yesod-test (Konstantine Rybinov).
* bootstrap and normalize updates in scaffolding (Iku Iwasa).
* Update many crypto libraries to latest dependencies (Alexey Kotlyarov and others).
* No double-compression in gzip middleware (John Lenz).
* Many dependency updates and documentation improvements (Lubomír Sedlář, Fredrik Carlen and others).

# Yesod 1.2

* Much more powerful multi-representation support via the selectRep/provideRep API.
* More efficient session handling.
* All Handler functions live in a typeclass, providing you with auto-lifting.
* Type-based caching of responses via the `cached` function.
* More sensible subsite handling, switch to HandlerT/WidgetT transformers.
* Simplified dispatch system, including a lighter-weight Yesod.
* Simplified streaming data mechanism, for both database and non-database responses.
* Completely overhauled yesod-test, making it easier to use and providing cleaner integration with hspec.
* Refactored persistent module structure for clarity and ease-of-use.
* yesod-auth's email plugin now supports logging in via username in addition to email address.
* combine css and javascript with combineScripts and combineStylesheets

# Yesod 1.1

## Yesod

* Upgrade to conduit 0.5
* Hierarchical routing.
* Control of sitewide Hamlet settings in the scaffolding.
* Easily add additional template languages in `Settings.hs`.
* `yesod add-handler` automates the process of adding a new route, creating the handler module, creating stub handler functions, and updating `Application.hs` and the cabal file.
* `yesod keter` builds a keter bundle. With a config setting, it will also upload it for you.
* By default, response bodies are now fully evaluated before sending to avoid empty responses when pure code throws an exception. `DontFullyEvaluate` is provided to override this default.
* Better control of uploaded file handling, defaulting to temporary file system storage.

## Shakespeare

* Hamlet defaults to newlines being added between tags.
* Tuple literal support (thanks @qnikst)
* Added back (and fixed) infix operators.

## Persistent

* Upgrade to conduit 0.5
* Sum types
* Support for `sqltype=...` attribute.

## WAI (1.3)

* Upgrade to conduit 0.5
* Warp flushes request body after processing response body. This allows you to process the request body while generating the response. While this can be useful, be warned that this can cause deadlocks with some HTTP clients.
* simple-sendfile now sends headers in the same system call as the sendfile itself, resulting in much better performance for static file serving.
* Simplified wai-extra's request body parsing, uses standard `conduit` types instead of special `BackEnd` type.
* Drastically cleaned up wai-app-static (both internals and the user-facing API).
* `warp-tls` automatically sniffs the request and determines whether to serve over HTTP or HTTPS.
* Split off the `mime-types` package from `wai-app-static`.

# Yesod 1.0

## Yesod

* Generalized yepnopeJs to jsLoader
* Generalized encryptKey to makeSessionBackend
* Upgrade to conduit 0.4
* yesod-test
* yesod-generate (external)
* `fsClass` in `FieldSettings` has been replaced by the more general purpose `fsAttrs`.
* Drastically improved compile times for Persistent models and Shakespearean templates by avoiding the Text inlining bug.

### Scaffolding

* Removed tiny option
* Including bootstrap.css
* yesod-test tests included
* Homepage is a very small tutorial
* Changed logic for approot and host config settings. Short story: host now gives the interface to bind to, approot should always be provided.

## Shakespeare

* Hamlet tags now require closing angle bracket, allow multiline tags.
* Hamlet now supports `*{attrs}` syntax within a tag.
* Removed support for infix operators.
* Support Roy.js
* Improved Coffeescript support (better job dealing with comments)
* CSS in reload and runtime mode now has whitespace. Hamlet whitespace is available by changing default settings.

## Persistent

* Upgrade to conduit 0.4

## WAI

* Upgrade to conduit 0.4

---

Older changelogs

# Yesod 0.10

* Replace lift with liftHandler and liftWidget
* No more GGHandler or GGWidget
* No more liftIOHandler
* Enumerators are gone, in come conduits, now you can catch exceptions!
* Move from pool to resource-pool
* Rework of the redirect system
* Configure database parameters via environment variables.
* Fully reworked routing, much more efficient.
* Cleaned up EntityDef, making it more resilient to renamings.


# Yesod 0.9.3

* Much slimmer scaffolding, helped out by the addition of some new configuration code in the main packages:
    * PersistConfig typeclass in persistent, with instances for each backend
    * Yesod.Config for parsing the settings.yml file
    * yesod-default to provide common functionality previously in the scaffolding
* Async Javascript loading via yepnope
* modernizr (which includes yepnope) now included in the scaffolding
* OpenID login screen provides Google and Yahoo! login buttons

# Yesod 0.9

It's worth prefacing a small explanation of how version numbers work in Yesod. There are many different packages involved, all eventually wrapped up by the "yesod" package itself. When we say version 0.9, we are referring to the version on that package.

The vast majority of the changes listed below are not part of the "yesod" package, but rather underlying packages like hamlet or yesod-forms. As such, many of these changes have been available on Hackage since before the actual 0.9 release. However, a standard usage of the "yesod" package will not notice them. For more information, please see [Using non-standard package versions](/wiki/non-standard-versions).


## Hamlet
* Removed direct polymorphic Hamlet. Non-polymorphic Hamlet is more efficient and easier to use. That being said, add ToWidget polymorphism :)
* re-organize hamlet into separate packages: hamlet (just hamlet), shakespeare (base code for other packages), shakespeare-js (julius), shakespeare-css (lucius, cassius), xml-hamlet
* add shakespeare-text for plain-text templates

## Forms
* Removed polymorphic forms. In general, the forms API has been drastically improved. If you don't use the scaffold to upgrade you will need to add this instance (used for Internationalization):

```haskell
instance RenderMessage ~sitearg~ FormMessage where
    renderMessage _ _ = defaultFormMessage
```

* add check and checkM for easy custom validations

## Persistent
* Persistent has been changed over to use phantom datatypes, inspired by groundhog. This makes the number of exported symbols significantly reduced, as well as simplifying implementations. The major user-facing results are:
    * Instead of consturctors: `selectList [PersonNameEq "Michael"]`, we now use operators: `selectList [PersonName ==. "Michael"]`
    * Annotations for updates, sorting and filtering are no longer necessary.
    * Instead of having two integers arguments at the end of select(List), we add it to the options list. So `selectList [PersonAgeEq 25] [PersonNameAsc] 10 20` becomes `selectList [PersonAge ==. 25] [Asc PersonName, LimitTo 10, OffsetBy 20]`
    * For updates, we use the =. operators: update myId [PersonAge =. 26]
* Persistent now has support for AND and OR filters, instead of just AND. For example:

```haskell
selectList ([PersonName ==. "Michael", PersonAge >=. 25] ||. [PersonAge <. 30]) []
```

* Support for arbitrary filter operators. That means you can now do LIKE queries for SQL.
* printMigration (for performing manual migrations) now prints out semi-colons at the end of its statements.

## WAI
* Debug middleware using the new logger and logging all POST parameters

## Yesod
* Built-in support for BrowserID.
* Static file serving
    * optimal caching headers and more efficient sendfile.
    * Static files can optionally be embedded directly in the executable rather than being on the  filesystem.
* Built-in thread-safe logger

### Scaffolding
* new logger setup out of the box in the new scaffolder and uses new WAI debug middleware.
* Signal handling on unix added to scaffolder (should probably be moved out of the handler).
* Documentation and code for deploying to Heroku
* upgrade to html5boilerplate version 2, which just means use normalize.css (instead of a css reset)
* More secure client side cookies, using CBC encryption instead of EBC. (Thanks to Loïc Maury.)
* file name changes
    * Controller -> Application
    * the variably name file containing the foundation type is now Foundation.hs
    * config/Settings.hs -> Settings.hs
    * config/StaticFiles.hs -> Settings/StaticFiles.hs


# Upgrading your site
* http://www.yesodweb.com/blog/2011/08/yesod-0-9-release-candidate
* import Text.Blaze.Renderer.String (renderHtml) -> import Text.Blaze.Renderer.Text (renderHtml)
* tabs are not allowed in hamlet
* hamlet -> hamlet, ihamlet, whamlet, or shamlet (see new chapter)

## scaffolding changes
* generate a new scaffold with the same foundation type.
* copy over config/settings.yml, and change the settings appropriately
* copy over Settings.hs, but you have to manually inspect against your config/Settings.hs to make sure you won't eliminate any of your current customizations
* copy over main.hs (previously it was named after your project and in the config dir), manually inspect differences with your file, and change your cabal file so the executable points to main.hs
* use the new withFOUNDATION handlers from the new scaffolding site you generated.