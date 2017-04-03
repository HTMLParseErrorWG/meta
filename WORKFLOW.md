# Getting started

At first create *working directory* and copy [update.sh](https://gist.github.com/inikulin/b8a9eea7fee5e3438925fb5a3981e777) to it. 
Then run `update.sh`. It will fetch all repositories you'll need for work and their dependencies.

# Adding error

1. Pick tokenizer state you are going to work on in [spreadsheet](https://docs.google.com/spreadsheets/d/1uToTV8M0aBkgdIEDRRBt4f0fJ0kUZm92olXTa9_xClU/edit?usp=sharing)
and assign yourself. 
2. Run `update.sh` from your *working directory* to align with latest changes both in upstream and origin repositories. Pay attention to possible merge conflicts during rebase, resolve them manually if necessary.
3. In `html` repository:
   1. Create new branch
   2. Add errors for chosen tokenizer state in `source` file. We add errors in format `<dfn data-x="parse-error-some-id">some-id</dfn>` (for
      examples you can take a look at already added error codes, e.g. in `Data state`). Make sure you follow [formatting guidelines](https://github.com/whatwg/html/blob/master/CONTRIBUTING.md#formatting). However, if error was defined previously and you just reuse existing code it should referenced as `<span data-x="parse-error-some-id">some-id</span>`
      Avoid introducing new error codes if you can reuse existing without ambiguity, e.g. `unexpected-null-character` error can be 
      reused nearly in all occurences of NULL character in states.
   3. Build spec and check that everything looks as intended: go to `html-build` directory in *working directory* and run `build.sh`, 
      fix linting errors if there are any. Compiled spec (part in which we are interested) can be found in `html-build/output/multipage/syntax.html`.
   4. Issue pull request with change and add reviewers.
4. In `parse5` repository:
   1. Add error code in https://github.com/HTMLParseErrorWG/parse5/blob/master/lib/common/error_codes.js if it doesn't exists yet.
   2. Add error reporting code, e.g. https://github.com/HTMLParseErrorWG/parse5/blob/master/lib/tokenizer/index.js#L648
   3. Run tests (`gulp test`) and figure out which tests fail.
5. In `html5lib-tests` repository:
   1. Add errors for failing tokenizer tests, e.g. https://github.com/HTMLParseErrorWG/html5lib-tests/blob/master/tokenizer/test3.test#L19. If there are no failing tests then add new tests for error. 
   2. Add errors for failing tree construction tests, e.g https://github.com/HTMLParseErrorWG/html5lib-tests/blob/master/tree-construction/foreign-fragment.dat#L177.
      Note that new errors should be temporary added in `#new-errors` section. Also, note that tokenizer errors don't have ranges for location,
      since they are bound to single character, so instead of `(1:44-1:44)` it should be `(1:44)`. Some test files contain exotic characters,
      therefore some editors deny to show them as text files or try to reformat them. I've tried WebStorm, VSCode and Sublime, however only
      Vim worked well for me on such files: it highlights exotic characters and don't try to be way too smart reformatting them.
   3. Run `parse5` tests to ensure that everything is fine.
   4. Issue pull request with change and add reviewers.
6. In `parse5` repository:
   1. Once tests PR gets merged issue pull request with change and add reviewers. 
   Thus, we'll have tests ready for CI test run.
7. Mark state done in [spreadsheet](https://docs.google.com/spreadsheets/d/1uToTV8M0aBkgdIEDRRBt4f0fJ0kUZm92olXTa9_xClU/edit?usp=sharing).

**NOTE:** You can include more than one state in each PR. It's up to you to choose granulation of your work. However, to avoid blocks, try to peek a reasonable number of states to work on at each iteration.
